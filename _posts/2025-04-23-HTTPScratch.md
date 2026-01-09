---
title: Making a Web Server from Scratch
date: 2025-04-23
categories: ["Projects"]
tags: ["Web Stuff"]
published: false
image:
  path: /assets/img/httpscratch-thumbnail.png
---

{% include embed/youtube.html id="fHKBNyu-RpM" %}

A while ago, I built a web app,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;on top of a web framework that I also wrote[^webpy-note],<br/>
&nbsp;&nbsp;&nbsp;&nbsp;written on top of another framework,<br/>
&nbsp;&nbsp;&nbsp;&nbsp;which used yet another library to execute via WSGI[^wsgi-note].

*That's a lot of abstraction.*

It's for good reason, because writing every web application by parsing HTTP requests or even using straight WSGI would be a pain, but it does hide what the frameworks are doing behind the scenes. Today, I'm going to throw all of the abstraction away (well, most of it ![@add-em-dash](-) the significant parts, anyway) and attempt to write a simple HTTP server using only sockets.

I'll still be using Python because there is no way I'm ever gonna do network programming in C (I guess I could use .NET but I'm already familiar with the Python socket API so... Python it is).
I'll start by importing `socket`, binding to a port, and listening for incoming connections.

```py
import socket

MAX_CONN = 5

def setup():
	sock = socket.socket(socket.AF_INET, socket.SOCK_STREAM)

	sock.bind(("127.0.0.1", 5000))

	sock.listen(MAX_CONN)

	print("Connected!")

	return sock
```

Raw HTTP requests look like this:

![@todo.add](-)

[^webpy-note]: The [WebPy](https://github.com/User0332/webpy) framework! I've talked a little bit about it in the [Volumetric article](/posts/volumetric). The other two libraries involved are [Flask](https://flask.palletsprojects.com/en/stable/) (WebPy's base) and [Werkzeug](https://werkzeug.palletsprojects.com/en/stable/) (Flask's base).
[^wsgi-note]: Python’s *Web Server Gateway Interface*.