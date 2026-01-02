---
title: Making the New Best Python Web Framework
date: 2025-07-15
categories: ["Projects"]
tags: ["Web Stuff"]

image:
  path: /assets/img/volumetric-logo.png
  alt: The volumetric web framework.
---

{% include embed/youtube.html id="c9XAwUiMUPQ" %}



A few years ago, after falling in love with Flask[^flasknote] but also growing frustrated with having to set up all the boilerplate when creating new apps, I made a CLI tool called `new-flask-app` (sort of the same idea as `create-react-app`). I never published the tool, so it stayed on my computer, but remnants[^remnant] of it can be found in a Flask template of Python project management tool I made called `serpent`. All it did was set up a simple app directory with some boilerplate code in `app.py`, a templates folder, and a static assets folder.

```
The directory structure created by new-flask-app.

{project}/
  app.py
  templates/
    index.html
  static/
    css/
    index.css
  js/
    index.js
  images/
```

It wasn't enough for me, though, because most of the web apps I made ended up being a bunch of rendered templates or static HTML files hooked up to the backend via routes under `/api/...`. Most of the time, I had a messy `app.py` file (or multiple files for route creation) which just returned a `render_template` call on many routes. To me, it felt repetitive and even a little unintuitive because it was hard to visualize the structure of paths on the server; they were all routed by separate functions so no heirarchy was immediately visible.

Thus, WebPy was born (unfortunately, the name was similar to the popular `web.py` framework, but at the time I wasn't concerned in selecting unique names; I had to make the package name and branding use `webpy-framework` rather than `webpy` 🫠). It became my go-to web framework from there, featuring filesystem-based routes that supported static HTML, Python logic, and Markdown out-of-the box as well as JSON configuration files for server deployment and route configuration options.


```
The webpy-framework directory structure.

{project}/
  html/               (templates)
  root/               (filesystem routes)
    dash/
    config.json     (route creation options)
    index.html      (statically served HTML)
    config.json
    index.py          (Python logic route)
  static/
    css/
      index.css
    images/
    js/
      index.js

  app.py
  config.json        (server deployment options)
```


It wasn't perfect, though: plugin object support had slipped my mind in the pursuit of my original goal (reducing boilerplate). The framework was perfect until you needed to add a database or authentication or any sort of Flask plugin to it: then things started to get messy because of the way logic was split across files. It was usable, just inconvenient.

A few years later, after growing bored with `webpy-framework`, I was ready to try something new. I really loved languages like JSX, where logic and markup could be integrated seamlessly into the same file, so I was determined to oust any static HTML, Markdown, or templating when I created my new framework.

I would call it Volumetric[^volumetricname] (yup, a direct reference to its base framework, Flask). Volumetric shared the same sort of structure as WebPy, but it only supported Python files in the filesystem routes. Each Python file's route handler function could now return PyX (Python XML) as supported by the `pyjsx` library.

```py
# A sample Volumetric route handler

def handler(app: volumetric.App, *args):  
  user: User = app.plugin_objects.authservice.get_user()
  
  return body(
    <>
      <IfTruthy valid={user}>
        <h1>Hello, World!</h1>
        <h2>Secret: {app.secrets.SOME_ENV_VAR}</h2>
        <Try
		  func={lambda: <h1>{some_unknown_name}</h1>}
		  catch={Exception}
		  fallback={
		    <div>error!</div>
		  }
		/>
      </IfTruthy>
      <NotTruthy valid={user}>
        <ClientSideRedirect to="/"/>
      </NotTruthy>
    </>
  )
```

Plugin support improved because each route handler file could import objects from modules in the top-level directory of the project (or one could pass the plugin objects through the `app` object), but there was only one issue left: since the framework became centered around pure-Python development due to the use of PyX instead of Python/HTML/JS, only server-side rendering was possible (unless you embedded a script tag into your PyX, but this would defeat the purpose of pure-Python development).

To support pure-Python CSR, I attempted to use Pyodide to run Python logic on the frontend. That way, for CSR-configured routes, I could ship a block of Python code with an `update` function that re-rendered the page (React-style but no fancy hydration logic yet) every frame update via a boilerplate JS stub that invoked Pyodide and helped propagate all the changes.

```py
# A Volumetric route using the CSR API

console.log("Hello, World! (from Python!)") # run once

def btn_handler():
  window.alert("Hello!")

def update(): # run every frame
  csr = CSRHelpers()

  return (
    <>
      <h1 id="heading">Hello, World!</h1>
      <button onclick={csr.conv_func(btn_handler)}>Click Me!</button>
    </>
  )
```

I've yet to use Volumetric in a web project of mine, but I'm excited to try it out! I think that with many of my projects, though, (Volumetric included) I've lost sight of the original goal and kind of ruined the outcome by adding random features and hacking things together to make them work (like the Volumetric CSR API, which feels kind of weird, though I guess any frontend Python will always feel out of place). The lack of support for PyX in editors is also a downside, and I don't think I've got the energy to make a VSCode extension that supports Python language features *and* XML in Python files, so I'll think I'll just have to deal with the syntax errors for now.

The project can be found on [GitHub](https://github.com/User0332/volumetric).

If you want a shorter and slightly more humorous summary of the project, watch the linked video at the top!

Thanks for reading!

[^flasknote]: The Python [web framework](https://flask.palletsprojects.com/en/stable/).
[^remnant]: The Serpent [Flask Template](https://github.com/User0332/serpent/blob/77580e18b97a20284421a55f31f7a7a71d1d4d87/templatespack/flask/stempl_flask/__init__.py).
[^volumetricname]: I again failed to search for similarly-named projects before choosing a name, and so I had to name the PyPI package `volumetric-flask` (which isn't too bad, because it reads properly).