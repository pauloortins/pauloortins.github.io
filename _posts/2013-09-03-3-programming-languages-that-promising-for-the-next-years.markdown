---
layout: post
title: 3 programming languages that are promising for the next years
categories: Clojure Elixir Go Programming Languages
date: 2013-09-03
comments: true
---
<p>As every addicted for programming languages, I feel myself excited when I see new programming languages. In the last years I did at least a toy project in C#, Java, Javascript, Ruby, Python, Scala and Objective-C. Recently, I made a search for new and exciting programming languages. Below is the the languages that I think more interesting and are promising more in the next years. They are open source and focused on writing concurrency software. I will definitively give a try to one of them. The problem is choose which one!</p>
<p><br/></p>

<!--more-->

<h2>Go</h2>
<br/>

{% highlight go %}
package main

import "fmt"

func plus(a int, b int) int {
    return a + b
}

func main() {
    res := plus(1, 2)
    fmt.Println("1+2 =", res)
}

{% endhighlight %}

<p>Go, also known as Golang, is an open source language developed by Rob Pike, Robert Griesemer and Ken Thompson at Google and since 2009 is used in some of the Google's production systems. Your main goal is to make easy to write concurrent systems. Go is compiled like C and is garbage collected like Java and aims to be have an efficiency of a statically typed compiled language with the ease of programming of a dynamic language.</p>
<p>Go syntax is similar to C and Java, blocks are surrounded by curly braces and there are control flows like if, switch and for. Unlike C, line-ending semicolons are optional, Go doesn't include type inheritance, generics and method overloading. Go also, makes heavy use of interfaces.</p>
<p>Go concurrency, that is the best point in the language, is implemented through the goroutines, who looks like small threads. Goroutines are created through the <em>go</em> statement from anonymous or named functions. These goroutines are executed concurrently with other goroutines, including their caller. Execution control is moved between them by blocking them when sending or receiving messages. Goroutines can also share data with other goroutines.</p>
<h2>Current State</h2>
<p>Go is now in version 1.1, and have been used by the following companies:</p>
<ul>
<li>Google</li>
<li>Heroku</li>
<li>SoundCloud</li>
<li>Canonical</li>
<li>CloudFlare</li>
</ul>
<h2>Resources to learn Go</h2>
<p><a href="http://golang.org/">Official Site</a><br />
<a href="https://gobyexample.com/">Go By Example</a><br />
<a href="http://golang.org/doc/effective_go.html">Effective Go</a><br />
<a href="http://www.amazon.com/gp/product/0321774639/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0321774639&linkCode=as2&tag=paulorti-20">Programming in Go: Creating Applications for the 21st Century (Developer's Library)</a></p>
<p><br/></p>
<h2>Elixir</h2>
<br/>

{% highlight elixir %}

defmodule Hello do
  IO.puts "Defining the function world"

  def world do
    IO.puts "Hello World"
  end

  IO.puts "Function world defined"
end

Hello.world

{% endhighlight %}
<p>Created by José Valim (former Rails Committer), Elixir is inspired by the the best parts of scripting languages like Ruby and Python, but built on top of Erlang VM. Exixir's goal, like Go, is to make easy to write concurrent software, it was built on top of Erlang VM, that is known to be very good on concurrency, and use the Ruby's syntax instead the Erlang's syntax, that is known to be very expressive.</p>
<p>Elixir and Erlang shared the same bytecode and datatypes. This means you can invoke Erlang code from Elixir (and vice-versa) without any conversion or performance hit. It's good, because Elixir, despite is a new language, is benefited by code that is already maintained by the Erlang community.</p>
<h2>Current State</h2>
<p>Elixir is still in Beta and the actual version is the 0.10.0 released on 07/13/2013.</p>
<h2>Resources to learn Elixir</h2>
<p><a href="http://elixir-lang.org/">Official Site</a><br />
<a href="https://peepcode.com/products/elixir">Meet Elixir ScreenCast with José Valim</a><br />
<a href="http://pragprog.com/book/elixir/programming-elixir">Programming Elixir</a></p>
<p><br/></p>
<h2>Clojure</h2>
<br/>


{% highlight c# %}
(loop [i 0]
  (when (< i 5)
    (println "i:" i)
    (recur (inc i)))i)

{% endhighlight %}
    

<p>Clojure is dialect of LISP programming language created by Rich Hickey. It’s also a predominantly functional language, dynamic, built on top of JVM (but has ports to CLR – ClojureCLR, and to Javascript Engine – ClojureScript). It is designed to be a general-purpose language, combining the approachability and interactive development of a scripting language with an efficient and robust infrastructure for multithreaded programming.</p>
<p>Clojure syntax, as other Lisps, is built on S-expressions that are first parsed into data structures by a reader before being compiled.</p>
<p>Clojure's best part is that it allows the use of the well established JVM but in a simpler way and, like the other functional programming languages, Clojure is expressive allowing us to reduce the number of lines of code that we need to type.</p>
<h2>Current State</h2>
<p>Clojure's actual stable version is 1.5.1, but also there is a development version that is currently in the version 1.6. Clojure is been used by the following companies:</p>
<ul>
<li>Amazon</li>
<li>Citigroup</li>
<li>BackType</li>
<li>Berico</li>
</ul>
<h2>Resouces to learn Clojure</h2>
<p><a href="http://clojure.org/">Official Site</a><br />
<a href="http://www.amazon.com/gp/product/1449394701/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1449394701&linkCode=as2&tag=paulorti-20">Clojure Programming</a><br />
<a href="http://www.amazon.com/gp/product/1935182641/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1935182641&linkCode=as2&tag=paulorti-20">The Joy of Clojure: Thinking the Clojure Way</a><br />
<a href="http://www.amazon.com/gp/product/1934356867/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1934356867&linkCode=as2&tag=paulorti-20">Programming Clojure</a></p>
