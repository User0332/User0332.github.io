# Variable and function guide

Symbols can be created using a datatype followed by a symbol type and identifier.

Ex.

```c

int         var           foo          =            0
^           ^             ^            ^            ^
datatype    symbol type   identifier   assignment   value
```

<br/>

Below are tables for datatypes and symbol types.

<br/>

| Data Type     | Description 
| -----------   | ----------- 
| `int`         | Standard int32
| `char`	| Single character stored as an `int` (not implemented)
| `float`       | 32-bit floating-point number (not implemented)
| `float64`     | 64-bit floating-point number (not implemented)
| `uint`	| 32-bit unsigned int (not implemented)
| `ushort`	| 16-bit unsigned int (not implemented)
| `ulong`	| 64-bit unsigned int (not implemented)
| `ilong`	| 64-bit integer (not implemented)
| `ishort`	| 16-bit integer (not implemented)
| `byte`	| A singular byte of data holding a value from 0-255 (not implemented)

<br/>
<br/>

| Symbol Type   | Description 
| -----------   | ----------- 
| `var`         | Standard variable
| `ptr`         | 32-bit pointer (this can be stacked like `char ptr ptr` for multiple layers of referencing) (not implemented)
| `const`       | Constant variable (not implemented)
| `func`        | Used to define a function (not implemented)

<br/>

---------------------------------

<br/>

Functions can look a little like this:

```c
int func myfunction(int var arg)
{
	
}
```

Or this (if they are returning a pointer):

```c
int ptr func myfunction(int var arg)
{

}
```

<br/>

Functions in PogScript use the [`cdecl`](https://en.wikipedia.org/wiki/X86_calling_conventions#cdecl) calling convention.

<br/>

External symbols can be referenced using the `extern` keyword, a calling convention (if it is a function), a datatype, a symbol type, and a symbol name.

```c
extern          cdecl            int         func          printf(char ptr, ...)
^                 ^               ^           ^              ^
keyword   calling convention    datatype     symbol type   symbol name
```

<br/>

```c
extern stdcall int func Function(int var foo, char ptr bar)
```

<br/>
<br/>

Note that calling convention names like `cdecl` and `stdcall` are soft keywords, so you can still use them as symbols in the code. If the externally referenced symbol is not a function, it should not be decorated with a calling convention and should instead look like this:

<br/>

```c
extern           int           var           x
^                 ^             ^            ^
keyword        datatype    symbol type    symbol name
```

<br/>

Also be aware that symbols like C structs cannot be `extern`ed because there is no trace of them in the assembly code. C structure instances would have to be represented as a contiguous block of memory, like an array.

<br/>

[Go Back](./)
