---
layout: post
title: 'Getting CSharper #1: A short introduction to C#'
categories: C# Getting CSharper
date: 2013-07-17
comments: true
---
I started to programming using C# three years ago. Coming from Java, it was love at first sight, the language offers a lot of shortcuts to accomplish tasks easier than I was used with Java. Recently, I felt the need to leave to be a only a language user and start to understand what is happening under the hood. This series of posts are a result from my experience trying to deepen my C# knowledge through books, online tutorial, videos and so on. I hope it can help other people who are trying to learn more about C# too.


## What it's C#?

First of all, is necessary to talk about what is C#, what is his history, what are his characteristics and how we can compare C# with other languages.
### A short history about C# and how it's evolving

`1997` - Microsoft started a project that was internally known as Project Lightning (and also known as Project 42). The name "Project 42" was most likely because DevDiv (the Microsoft Developer Division) is in Building 42

`1999` - [Anders Hejlsberg][1], creator of Turbo Pascal and Delphi, and actually working on [TypeScript][2], started to develop a language called Cool, which stood for "C-like Object Oriented Language", it was supposed to be a "clean-room" implementation of Java.

`2002` - C# 1 was released by Microsoft, his syntax is very inspired by Java and C/C++.

`2005` - C# 2 was released, the following features were introduced: 

- [Generics][3]
- [Partial types][4]
- [Anonymous methods][5] 
- [Iterators][6] 
- [Nullable types][7]

`2007` - C# 3 was released, the following features were introduced: 

- <a href="http://msdn.microsoft.com/en-us/library/bb384061.aspx">Implicitly typed local variables</a>
- <a href="http://msdn.microsoft.com/en-us/library/vstudio/bb384062.aspx">Object and collection initializers</a>
- <a href="http://msdn.microsoft.com/en-us/library/bb384054.aspx">Auto-Implemented properties</a>
- <a href="http://msdn.microsoft.com/en-us/library/vstudio/bb397696.aspx">Anonymous types</a>
- <a href="http://msdn.microsoft.com/en-us/library/vstudio/bb383977.aspx">Extension methods</a>
- <a href="http://msdn.microsoft.com/en-us/library/bb384065.aspx">Query expressions</a>
- <a href="http://msdn.microsoft.com/en-US/library/vstudio/bb397687.aspx">Lambda expressions</a>
- <a href="http://msdn.microsoft.com/en-us/library/bb397951.aspx">Expression trees</a>
- <a href="http://msdn.microsoft.com/en-us/library/vstudio/wa80x488.aspx">Partial Methods</a>

`2010` - C# 4 was released, the following features were introduced:

- <a href="http://msdn.microsoft.com/en-us/library/vstudio/dd264741.aspx">Dynamic binding</a>
- <a href="http://msdn.microsoft.com/en-us/library/dd264739.aspx">Named and optional arguments</a>
- <a href="http://msdn.microsoft.com/en-us/magazine/ee336029.aspx">Generic co- and contravariance</a>

`2012` - C# 5 was released, the following features were introduced:

- <a href="http://msdn.microsoft.com/en-US/library/vstudio/hh191443.aspx">Asynchronous methods</a>
- <a href="http://msdn.microsoft.com/en-us/library/hh534540.aspx">Caller info attributes</a>

## Characteristics

In few words, we can define C# as a object-oriented programming language with support for functional programming, type-safe, with automatic memory management that runs mainly on Windows Platforms on top of CLR (Runtime Environment). Let's analyze these characteristics.

### Object-Oriented

C# has support for encapsulation (we can declare private variables/functions), inheritance and polimorphism. Unlike C++, C# doesn't support multiple inheritance, instead it, we can use interfaces to make classes inherit characteristics from more than one source. The disadvantage is that interfaces only describe behaviors, they will have to be implemented in the leaf classes. In most of object-oriented programming languages, the behavior are defined by functions, in C#, there are more types like properties, that are functions to encapsulate an object's state, for example, age and name in a Person class, similar to gets and sets in Java.

### Support for Functional Programming

C# has support to functions as first-citizen, you can declare functions as variables, we can pass functions as parameter. There are LINQ, lambda expressions and so on. It makes the code shorter and more powerful.

### Type Safe 

C# is statically typed, it checks for type errors during compile time. Check errors in compile time can catch a lot of errors before a program even run. Static typing also allow tools like IntelliSense and Refactoring Tools help us to write the program, it makes easier to maintain big projects.

### Automatic Memory Management

Unlike C++, that requires that  the developers worry about manual memory management, and similar to Java, C# has a automatic garbage collector, that automatically release memory for objects that are not used anymore. It doesn't eliminate the possibility to do manual memory management, if we want, we can use pointers like C++ to achieve a even better performance.

### Runs mainly on Windows but has ports to other OS’s

C# is designed to run on windows platforms. There are some efforts to run C# in other platforms (See Mono Project), but these efforts are small and they support only a subset of C#/.NET Framework.

### CLR 

CLR (Common Language Runtine) is the .NET Framework's virtual machine. It's a runtime that is used by different programming languages in the .NET Framework (C#, VB.NET, IronRuby, IronPython and so on). It provides memory management, exception handling and thread syncronization. When we write a C# program the code will be compiled in a intermediate language called IL. This intermediate language will be loaded by CLR and will be compiled to native code by the CLR Just in Time compiler.

### .NET Framework

C# is part of the .NET Framework and it includes a lot of libraries with different objectives. The .NET Framework's Base Class Library provides user interface, data access, database connectivity, cryptography, web application development, numeric algorithms, and network communications. See the image below.

![.Net Framework][8]

## Continues
I will be writing others posts about C#, about syntax, about libraries and so on. Subscribe the newsletter or the RSS to be notified! See you soon!

[1]: http://en.wikipedia.org/wiki/Anders_Hejlsberg
[2]: http://en.wikipedia.org/wiki/TypeScript
[3]: http://msdn.microsoft.com/en-us/library/512aeb7t(v=vs.80).aspx
[4]: http://msdn.microsoft.com/en-us/library/wbx7zzdd.aspx"
[5]: http://msdn.microsoft.com/en-us/library/0yw3tz5k%28v=VS.80%29.aspx
[6]: http://msdn.microsoft.com/en-us/library/vstudio/dscyy5s0.aspx
[7]: http://msdn.microsoft.com/en-us/library/vstudio/1t3y8s4s.aspx
[8]: /images/posts/2013-07-17/netframework.png
