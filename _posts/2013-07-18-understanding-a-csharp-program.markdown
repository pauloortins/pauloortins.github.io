---
layout: post
title: 'Getting CSharper #2: Understanding a C# Program'
categories: C# Getting CSharper
date: 2013-07-18
comments: true
---
In the [previous post][1] we talked about C# history, how it’s evolving and some of his characteristics. Now, we will start to see code. We are going to see the C# keywords, blocks, how a C# program is structured and so on.

<!--more-->

Understanding our First C# Program

{% highlight c# %}
using System; // Using declaration

namespace ConsoleApplication1 // Namespace declaration
{
   /*
      This is my 
      multiline comment
    */
    class Program // Class declaration
    {
        static void Main() // Main declaration
        {
            string name1 = "Philip";  // Statement/Variable Declaration
            int age1 = 23;            // Statement/Variable Declaration

            string name2 = "Roger";   // Statement/Variable Declaration
            int age2;                 // Statement/Variable Attribution
            age2 = age1 + 10;
            
	    Console.WriteLine(CreatePhrase(name1, age1)); 
	    // Statement/Method Call
            
	    Console.WriteLine(CreatePhrase(name2, age2)); 
	    // Statement/Method Call
        }

	// Method Declaration           
        static string CreatePhrase(string name, int age) 
        {
            return string.Format("Hi {0}, you are {1} years old.", name, age); 
	    // Statement/Return
        }
    }
}

{% endhighlight %}
What this program does? It prints a welcome message to two people. Now, let's using a bottom-up approach to figure out what each part means.

### Statements

Statements are the smallest element in a language. A program is formed by a sequence of one or more statements. If you look to our program we will see that we have 7 statements, statements can be classified by type. Declaration Statements are statements used to declare a variable. Attribution Statements are used to assign a value to a variable. Call Statements are statements used to call a method (we see more about it soon). The last statement that we are using in our program is the Return Statement that we use to finish a method and return a value.

### Methods

Sometimes, to break our program in subroutines that we can reuse latter, or to simplify our code, we can group statements in methods. Methods can receive one or more input data aka parameters and can return, or not, data to the caller. Our method <em>CreatePhrase </em>receive two parameters name and age and return a welcome message. <em>Main </em>method, when we are executing a console application (our program is a console application), the C# recognizes a method called <em>Main </em>as an entry point of execution and this method will be called to run the program.

### Classes

In our example, we have a class called <em>Program</em>. A class is a unit of code who has state (data field members) and behaviors (methods). Our <em>Program </em>class has two methods, <em>Main</em> and <em>CreatePhrase</em>. A class is a kind of type and we can combine some of them to design our programs.

### Namespaces

When our project is getting large, we feel the need to organize these types in other structures. They are called namespaces, and they are sets of types (class is a type, interface is a type and so on). In our program, we are declaring a namespace called <em>ConsoleApplication1 </em>which has only one class <em>Program</em>. Now look at the <em>Console.WriteLine</em> statement. Sometimes in our program we need to use types that were already created by other people, it's a good practice, we don't have to reinvent the wheel every time. To reuse a type already created we can import a namespace and we do it through the using statement. In our example, we are using a class called <em>Console </em>who belongs to a namespace called <em>System</em>. The using is there for convenience, without it, we should type the fully qualified name '<em>System.Console.WriteLine</em>' to call the method <em>WriteLine</em>.

### Syntax

C# syntax is inspired by C, C++ and Java.

### Identifiers

Identifiers are names that we, programmers, choose for our types (variables, methods, classes, namespaces and so on). In our program we are using the following identifiers: <em>ConsoleApplication1</em>, <em>Program</em>, <em>Main</em>, <em>name1</em>, <em>age1</em>, <em>name2</em>, <em>age2</em>, <em>CreatePhrase</em>. And the .NET Framework is using these: <em>System</em>, <em>Console</em>, <em>WriteLine</em>, <em>string</em>, <em>Format</em>. Identifiers in C# must be a whole word, formed by numbers, letters and underscore. By convention, parameters, local variables, and private fields should be in camel case (e.g. <em>personName</em>) and all other identifiers (namespaces, classes, methods) should be in pascal case (e.g. <em>WriteLine</em>, <em>Console</em>, <em>System</em>).

### Keywords

Keywords are names reserved by the language, you can't use them as identifiers. In our program, using, class, namespace, static are example of keywords.<br />
Below there is a list of C# keywords:

![Keywords][2]

### Braces and Semicolons

Like C, C++ and Java, C# uses braces to delimit statements blocks and semicolons to mark the end of the line.

### Comments

C# has support for two different types of code comments. Single-line comments and multi-line comments. A single-line comment starts with '//' and it continues until the end of the line. If we look at our program we will perceive that most of our program comments are single line comments. And there are the multiline comments, that start with '/*' and ends with '*/' and can contain one or more comment lines.

## Conclusion

I think that we cover a little bit about C# strutures and syntax. In the next posts we will start to go deep in the language. There are a lot of ground to cover!

[1]: /2013/07/17/getting-csharper-1-a-short-introduction-to-csharp/
[2]: /images/posts/2013-07-18/keywords.png
