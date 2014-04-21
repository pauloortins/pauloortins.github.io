---
layout: post
title: 'Diving into C#/.NET ecosystem - An analysis of the C#/.NET platform'
categories: C#
date: 2013-11-02
comments: true
---

Last weekend I gave a talk in a local event about the `C#/.NET ecosystem`, the audience was dominated by students and I tried to show to them how they can analyze a programming language, points they should observe and after I analyzed the C# through these points. The post below is a compiled of this talk.

## Updated – Discussions

A huge discussion is happening on HackerNews about this theme: 

_Disclaimer: All the points below reflect my and only my opinion. If you agree with me, cool, if not, give your opinion and we can start a discussion, after all, blogs are made for it. Welcome and Good read!_

## Post Outline

  1. Introduction and Motivation
  2. Language Features
  3. Generalist/Niche
  4. Tools
  5. Costs Involved
  6. Community/Open Source
  7. Future

## Introduction and Motivation

In the last two years I started to participate a lot of the local software community (Salvador, Brazil), I gave talks, I organized meetings and even founded two user groups (.NET Salvador and Dev In Bahia).

As a result, I met a lot of people and I made a lot of friends too. But these new friends had different backgrounds than me and my colleagues, while we work with `C#` and the `.NET stack`, these new friends are people from `PHP`, `Ruby`, `Python`, `Java`, `Scala` and so on, we assembled a group where diversity is a strong characteristic. In a scenario like this, we have two options.

First, we could become enemies and start long flame wars like `Java x .NET`, `Ruby x Python` and these stuff that we see children discussing about.

The other option is `try to become a better developer and learn how other programming languages work`, how they solve problems, how is their market, their tools and how they are prepared to face the future.

The next sections are a result from a reflection that I did about my toolset(strongly based in C#/.NET), how it can be compared to other toolsets and how it’s prepared for the future.

## Language Features

In this section, I want to analyze the features that the language have and how the language is evolving in the last years. IMHO, C# has a lot of cool features and Microsoft is making an excellent job through the last years keeping the language in the edge. Below is some of the features/characteristics that I like in C#.

### Static Typing

Programming languages can have `dynamic types or static types`, they also can have `weak types or strong types` ([a good explanation about it here][1]). C# has `strong and static types`, it means that it can rely on compiler to catch some syntactic errors, it has a better performance since it’s compiled and it also has good tools for `code completion`, `code analysis` and `refactoring support`.

It’s a very controversy topic, and I know we can be less bureaucratic and more productive with dynamic languages, but most of my work is develop software for enterprises and in this environment, software normally last for 5, 10, 15 years, a lot of developers work in these projects, they have different skill levels and they normally don’t write tests (unit or integration), so at least with static typing I’m guaranteeing that they won’t add any syntatic error.


{% highlight javascript %}
    // A Javascript Code with dynamic typing
    var student = {
        Age: 17,
        Name: 'Paulo Ortins'
    };

    function printAdultStudent(student) {
        if (student.Age &gt; 18) {
            console.message(student.Name);
        }
    }

{% endhighlight %}


{% highlight c# %}

    // The same code now in C#, more bureaucratic but with type checking
    public class Student
    {
        public int Age { get; set; }
        public string Name { get; set; }
    }

    Student student = new Student()
    {
        Age = 17,
        Name = "Paulo Ortins"
    };

    public void PrintAdultStudent(Student student)
    {
        if (student.Age &gt; 18)
        {
            Console.WriteLine(student.Name);
        }
    }
{% endhighlight %}


### Type Inference/Annonymous Type

Despite to be statically typed, the C# team is always working on syntactic improvements to provide us an easy way to write static typed code. One of these improvements is `type inference` that avoid us to type twice a variable type. The other is `annonymous type` that make possible to write variable without a predefined type and still be able to rely on compiler to catch type errors.


{% highlight c# %}
    // Type Inference
    Student student = new Student();
    var student = new Student();

    Dictionary students = new Dictionary();
    var students = new Dictionary();

    // Annonymous Type
    var student = new {Name = "Paulo Ortins", Age = 23};

    student.Age = 25; // ok, with intellisense
    student.Aeg = 25; // compiler error
{% endhighlight %}



### Extension Methods

`Extension Methods` is a feature added in C# 3.0 (2008) that enable us to add new methods to existing types, without using inheritance or modifying the original type, adding `power and expressiveness` to our code.

Let me give some examples of how extension methods can be used:

1\. Replace the `!` operator. The `!` operator doesn’t add expressiveness to our code, actually, it’s sometimes misread by developers, I prefer the way that `Python` and `VB.NET` handle with contrary operations that is through the `not` operator. Below is how we can handle it using `C#` and `Not`:



{% highlight c# %}
    class BoolExtensions
    {
        static void Main(string[] args)
        {
            Console.WriteLine(false.Not()); 
	    // use .Not(), for me, is more elegant than use the '!' operator
            Console.WriteLine(!false);
        }
    }

    static class MyExtension
    {
        public static bool Not(this bool flag)
        {
            return !flag;
        }
    }
{% endhighlight %}


2\. Add more power and expressiveness when `working with lists`.

{% highlight c# %}
    class Person
    {
        public int Age { get; set; }
        public decimal Grade { get; set; }
    }

    class PersonExtensions
    {
        static void Main(string[] args)
        {
            var people = new List();
            var approved = people.Approved();
            var adults = people.Adults();
            var approvedAdults = people.Adults().Approved();
        }
    }

    static class MyExtension
    {
        public static IEnumerable Adults(this IEnumerable people)
        {
            return people.Where(x =&gt; x.Age &gt;= 18);
        }

        public static IEnumerable Approved(this IEnumerable people)
        {
            return people.Where(x =&gt; x.Grade &gt;= 7.0M);
        }
    }

{% endhighlight %}



### LINQ

`Language-Integrated Query` is a set of features added in C# 3.0, based on `extension methods` and `functional programming`, that add powerful query capabilities to C# as part of the language. Traditionally, queries are expressed as strings, for example, a SQL Command, without any type checking or code completion support and even worst, we have to learn a different query language for each data source (SQL, XML and so on). LINQ provide a `single and simple way to develop queries`, we can write queries against our typed collections using C# code and relying on code completions and compile errors to guide us and avoid misspelled fields. You can use LINQ to query against `SQL`, `XML`, `DataSets` and `Collections`. Below are some LINQ samples, you can find more examples [here][2].



{% highlight c# %}

    public void LinqWhere()
    {
        int[] numbers = { 5, 4, 1, 3, 9, 8, 6, 7, 2, 0 };

        var lowNums =
            from n in numbers
            where n &lt; 5
            select n;

        Console.WriteLine("Numbers &lt; 5:");
        foreach (var x in lowNums)
        {
            Console.WriteLine(x);
        }
    }

    /*
    Numbers &lt; 5:
    4
    1
    3
    2
    0
    */

    public void LinqGroup()
    {
        string[] words = { "blueberry", "chimpanzee", 
		"abacus", "banana", "apple", "cheese" };

        var wordGroups =
            from w in words
            group w by w[0] into g
            select new { FirstLetter = g.Key, Words = g };

        foreach (var g in wordGroups)
        {
            Console.WriteLine("Words that start with the letter" + 
			      " '{0}':", g.FirstLetter);
            foreach (var w in g.Words)
            {
                Console.WriteLine(w);
            }
        }
    }

    /*
    Words that start with the letter 'b':
    blueberry
    banana
    Words that start with the letter 'c':
    chimpanzee
    cheese
    Words that start with the letter 'a':
    abacus
    apple
    */

{% endhighlight %}


### Functional Programming Support

C#, like Ruby, Python, Javascript and Scala, also provides to us functional programming support. We can pass functions as parameters, we can use functions as variables of and also return functions from another functions. It enables us to write more declarative code, with fewer lines of code and consequently less error prone. Below is some examples.


{% highlight c# %}
    class FunctionalExamples
    {
        class Person
        {
            public int Age { get; set; }
            public decimal Grade { get; set; }
        }

        private static void Main(string[] args)
        {
            // Action
            var person = new Person() {Age = 19, Grade = 7.0M};
            Action printAge = () =&gt; Console.WriteLine(person.Age);
            Action printGrade = () =&gt; Console.WriteLine(person.Grade);
            PrintSomething(printAge);   // 19
            PrintSomething(printGrade); // 7.0

            // Functions
            DoAndPrintMathOperation((num1,num2) =&gt; num1 * num2); // 50
            DoAndPrintMathOperation((num1,num2) =&gt; num1 - num2); // 5
            DoAndPrintMathOperation((num1,num2) =&gt; num1 %2B num2); // 15
            DoAndPrintMathOperation((num1,num2) =&gt; num1 / num2); // 2

        }

        public static void PrintSomething(Action printFunction)
        {
            printFunction();
        }

        public static void DoAndPrintMathOperation(Func mathOperation)
        {
            const int num1 = 10;
            const int num2 = 5;
            var result = mathOperation(num1, num2);
            Console.WriteLine(result);
        }
    }

{% endhighlight %}

### Async and Await

Asynchronous Programming is essential when we are handling activities that are potentially blocking, for example, when we are accessing a Database, or accessing a Web page or working with files. These activities are normally slow and, when executed in synchronous processes, they can stop the entire execution. Asynchronous Programming allow us to execute these tasks without forcing the application to wait for them, improving the responsiveness and scalability.

C# 5.0, brought two new keywords, async and await, that make easier to write async methods. Below, I wrote a example comparing sync methods with async methods and you can see how easy is to write async methods with these new keywords and how it can be used to improve application performance.

{% highlight c# %} 

private static void Main(string[] args)
{
    Action runAsync = () =&gt;
    {
        var siteLength = GetSiteContentLengthAsync("http://msdn.microsoft.com");
            siteLength.ContinueWith(x =&gt; Console.WriteLine("COMPLETED"));
            Thread.Sleep(1000);
        };

    Action runSync = () =&gt;
    {
        var siteLength = GetSiteContentLengthSync("http://msdn.microsoft.com");
        Console.WriteLine("COMPLETED");
        Thread.Sleep(1000);
    };

    Profile(runAsync);
    Profile(runSync);
}

public static void Profile(Action action)
{
    var initialTime = DateTime.Now;

    for (var i = 0; i &lt; 10; i%2B%2B)
    { 
       action();
    }

    var endTime = DateTime.Now;
       Console.WriteLine("{0} ms", (endTime - initialTime).TotalMilliseconds);
}

async static Task GetSiteContentLengthAsync(string webSite)
{
    var urlContents = await new HttpClient().GetStringAsync(webSite);
    return urlContents.Length;
}

static int GetSiteContentLengthSync(string webSite)
{
    var urlContents = new HttpClient().GetStringAsync(webSite).Result;
    return urlContents.Length;
}

/*
COMPLETED
...
COMPLETED
10114,1591 ms

COMPLETED
...
COMPLETED
23363,9914 ms
*/

{% endhighlight %}

## Generalist/Niche

One of the points that I consider when I choose a new language to learn is how this new language increase my tool set and C# increase it a lot, with C# we can develop `web`, `mobile` and `desktop applications`, we can also develop `kinect applications` and `big data solutions`. In other words, it delivers solutions to almost every problem that we face nowadays.

As Java, C# also runs on top of a `VM(Virtual Machine)` called `CLR (Common Language Runtime)`. The compilation process is also the same. The compiler converts a .NET language to an intermediate language called `CIL (Common Intermediate Language)` that is the same as `Java bytecode` and in runtime the CLR converts this intermediate language to native code. Mono project offers an open source implementation of .NET Framework, C# and CLR, it enables C# to be used for `different purposes` and in `different platforms.`

![400px-CLR_diag.svg][3]

#### Figure 1: .NET compilation pipeline.

### Web

C# has some frameworks to help us to develop web applications, the most used are `ASP.NET MVC` and `ASP.NET WebForms`, both developed by Microsoft. The community has also developed their own frameworks, for example, [NancyFx][4], [FubuMVC][5], [OpenRasta][6] and others.

### Desktop

With C# you can, obviously, develop `applications for windows` using WPF (Windows Presentation Foundation), you can also use Metro to develop applications for Windows 8.

Mono also added more versatility to .NET when we talk about desktop applications, there are ports for `Linux` and for `MacOS` that enable us to develop C# applications for these platforms.

![monoForMacOS][7]

#### Figure 2: TouchDraw, a MacOS application built with Mono for Mac.

### Mobile

Develop for mobile devices is becoming a key factor for developers, mobile market is increasing their share by large steps. C# offers solutions for the three main operating systems, `Windows Phone`, `Android` and `iOS`.

Windows Phone, since Microsoft bought Nokia, is increasing their sales and consequently their market share, some people are already betting that,in the next years, Windows Phone will beat iOS as the second biggest player in the market. C# developers, today, have the opportunity to learn this technology while it isn’t mainstream and, if Windows Phone really become a major platform, get a good position in the future.

![mobile_market_share][8]

#### Figure 3: Mobile Market Share Prediction – 2015.

When we talk about the two biggest players nowadays, we can have C# code running on Android and on iOS through `Mono for Android` and `Mono for iOS`, and even better, unlike happens with hybrid applications written in Javascript, where applications don’t have a native behaviour, when we use Mono we can use native controls, using native interface builders and give our application a `native behavior`, and even better, we can do it while `sharing almost 75% of the code base` between the different versions.

![Screen Shot 2013-10-29 at 8.46.30 PM][9]

#### Figure 4: The same application with native controls and sharing almost 75% of the code base.

### Cloud/Big Data

Another tendency when developing applications is, first, the capacity to `handle thousands and thousands users`, and, second, the capacity `to gather and analyze all the data` produced by this users and discover patterns to `help the companies to take decisions`.

According to a survey conducted by Gartner Inc., [by 2015, big data demand will reach 4.4 million jobs globally][10] but only one-third of these jobs will be filled. Almost 3 million jobs will be waiting for those developers who start to develop big data solutions in the next years.

As C# developers, we can develop cloud solutions using `AWS` and `Azure`, from Amazon and Microsoft respectively and many others. They also enable us to perform `map reduce` operations through `Elastic Map Reduce` and `Azure HDInsight`.

![hadoop_azure][11]

#### Figure 5: HDInsight, hadoop on Windows Azure.

### Natural User Interfaces/Kinect

Natural User Interface or NUI, is the name given to user interfaces that are almost invisible and based in `natural movements`. It gained traction when started to replace games joysticks like Kinect, but also started to gain traction for other areas, like `remote physioterapy`, `surgery support`, `house monitoring `and so on.

The most used tool to develop NUIs have been the `Microsoft Kinect` where one of the languages used to develop these applications is C#.

![kinect-phi][12]

#### Figure 6: People are using Kinect to support physiotherapy sessions.

## Tools

C# has `mature tools` to assist developers to be productive and to write maintainable code. The most used are:

`Visual Studio` – By far the `most used tool by C# developers` and and also considered by a lot of people the `better existing IDE`. It has good support for code completion, refactoring, web designer, debugging, unit tests, code analysis, deployment, version control and much more.

`MonoDevelop` – Visual Studio only runs on Windows, but remember that C# runs also on Linux and on Mac, MonoDevelop is the choice to write C# code in these platforms. Unlike Visual Studio, MonoDevelop is free and open source.

`Xamarin Studio` – Xamarin Studio is the IDE used to programming using C# for `mobile applications` for iOS and Android, it was developed by Xamarin, the same company that developed Mono.

## Costs Involved

This is the point where `C# really sucks`. Almost every tool, OS, IDEs and plugins, are `paid`. Below are the costs at this moment.

Windows 8

  * Express: `Free, but you lose some features like plugins support`
  * Standard: `$120`
  * Pro: `$200`

Visual Studio 2013

  * Upgrade from 2012: `$99` until 12/31, after `$299`
  * Full Price: `$499`

Xamarin Studio

  * Indie: `$299/year, by platform`
  * Business: `$999/year, by platform`

At least for students, both Microsoft and Xamarin offers `huge discounts` or even `free versions` of their software. To get Microsoft products by free you need to register yourself with a valid educational email in their program, `DreamSpark`. For Xamarin, you need to get in contact with them by email.

For those with startups or companies with less than 5 five years, Microsoft has a program called `BizSpark`, that offers free licenses and free support during 3 years.

![dreamspark][13]

#### Figure 7: DreamSpark, support for students.

## Open Source/Community

I will start this topic with one recommendation. Read my disclaimer. Then read again. After these two years involved with people from other communities, I have to say that C#/.NET `community is not good` when compared with other communities like `Java`, `Ruby`, `Python` and now `Node.js`, where people develop their own language, tools, IDE’s and so on.

There are `few open source projects` but they `never become too much popular`. People are always waiting for Microsoft to develop new software/tools/IDEs and it results in a delay and in a dependency when talking about adopting new technologies like SASS, LESS and CoffeeScript, even when a non-Microsoft tool is developed, people tend to wait until a Microsoft solution is released to start to use these new technologies.

When good non-Microsoft tools are released (Resharper, CodeRush, RedGate Tools, NCrunch and others), `they are usually paid`, looks like the community culture is, `if people pay for new software, people will sell new software`. IMHO, it breaks a little bit the concept of community, that is everybody helping each other to build better and better tools.

Despite of it, `Microsoft is trying to change this scenario` a little bit. Recently, Microsoft is releasing some of their frameworks as open source projects and `inviting developers to contribute`. For example, the following frameworks are created by Microsoft and now are also open source:

Microsoft is also `recognizing people from their efforts` in `open source projects` and in `community support`. Those that more contribute in these topics in a given year can become a `Microsoft MVP` and receive incentives from MS to continue contributing. You can read more about Microsoft MVP [here][14].

Recently, other people also discussed about .NET community and how it’s evolving, if you want to read more, check these links below:

## Future

Worth betting in .NET for the next years?

IMHO, yes, and I’m betting on it. The platform is `evolving fast` and is `being generalist` that enable us to solve problems in a variety of fields, as I wrote in this section. It’s not perfect, but people are putting efforts to solve the problems and continue to push it forward.

Are there a future where we will have a `top language` like C#, running on a `free Visual Studio version` with `free plugins` built on top of a `vibrant community`? I hope so!

Wish to learn more about C#? [Check out these resources][15].

November 2, 2013 @ 12:56 pm

   [1]: http://coding.smashingmagazine.com/2013/04/18/introduction-to-programming-type-systems/
   [2]: http://code.msdn.microsoft.com/101-LINQ-Samples-3fb9811b
   [3]: http://pauloortins.com/wp-content/uploads/2013/10/400px-CLR_diag.svg_.png
   [4]: http://nancyfx.org/
   [5]: http://mvc.fubu-project.org/
   [6]: http://openrasta.org/
   [7]: http://pauloortins.com/wp-content/uploads/2013/10/monoForMacOS-1024x762.png
   [8]: http://pauloortins.com/wp-content/uploads/2013/10/mobile_market_share-300x247.png
   [9]: http://pauloortins.com/wp-content/uploads/2013/10/Screen-Shot-2013-10-29-at-8.46.30-PM.png
   [10]: http://visualstudiomagazine.com/articles/2013/09/24/big-data-deployments-going-slow.aspx
   [11]: http://pauloortins.com/wp-content/uploads/2013/10/hadoop_azure.jpeg
   [12]: http://pauloortins.com/wp-content/uploads/2013/10/kinect-phi.jpeg
   [13]: http://pauloortins.com/wp-content/uploads/2013/10/dreamspark.jpeg
   [14]: http://mvp.microsoft.com/en-US/default.aspx
   [15]: http://pauloortins.com/good-resources-to-learn-casp-net/ (Resources to become a Ninja: C#)
  
