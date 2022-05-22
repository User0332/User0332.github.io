# Variable and function guide

Symbols can be created using a datatype followed by a symbol type and identifier.

Ex.

```c

int         var           foo          =            0
^           ^             ^            ^            ^
datatype    symbol type   identifier   assignment   value
```

<br/>

Below are tables for datatypes and symbol types

<br/>

| Data Type     | Description 
| -----------   | ----------- 
| `int`         | standard int32
| `char`        | single character stored as an `int` (not implemented)
| `float`       | floating-point number (not implemented)

<br/>
<br/>

| Symbol Type   | Description 
| -----------   | ----------- 
| `var`         | standard variable
| `ptr`         | variable pointer (this can be stacked like `char ptr ptr`) (not implemented)
| `const`       | constant variable (not implemented)
| `func`        | used to define a function (not implemented)

<br/>

---------------------------------

<br/>

Functions can look a little like this:

```c
int func myfunction(int var arg)
{
	
}
```

<br/>

External symbols can be referenced using the `extern` keyword, a calling convention, a datatype, a symbol type, and a symbol name.

```c
extern          cdecl            int         func          printf(char ptr, ...)
^                 ^               ^           ^              ^
keyword   calling convention    datatype     symbol type   symbol name
```

```c
extern stdcall int func Function(int var foo, char ptr bar)
```

<br/>

Note that calling convention names like `cdecl` and `stdcall` are soft keywords, so you can still use them as symbols in the code.