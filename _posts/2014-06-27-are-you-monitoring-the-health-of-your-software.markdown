---
layout: post
title:  "Are you monitoring the health of your software?"
date:   2014-07-01
categories: code-metrics
comments: true
---

Software projects tend to become more and more complex as they evolve. This complexity, if not managed, can contribute in an important way to the project failure. One of the parts that are more exposed to a complexity increase is source code. In this article we are going to see how software complexity increases and how we can use code metrics to increase code quality avoiding that a software fails due this complexity increase.

## Software Evolution

Since 1967 people are researching about the software importance in our society and how we can establish patterns and practices that we can be applied to develop high quality software. Other studies are also trying to study how software evolves and what are the consequences of this evolution.

### Laws of Software Evolution 

Manny Lehman, in the seventies, proposed a set of 8 laws that try to explain how a software evolves. Below are the 4 laws that I think that are more relevant in nowadays.

#### 1. Continuous Change

A software will be less relevant and more useless for their users over the time unless it is constantly adapted to attend new domain requirements, e.g. new laws and regulations.

#### 2. Increasing Complexity

A software tend to become more and more complex unless actions are taken to manage this increasing complexity.

#### 3. Continuing Growth

The number of functionalities of a software will increase with the time to attend the new user requirements.

#### 4. Declining Quality

The perception of quality for a software will decrease with the time unless actions are taken to adapt the software to the user requirements.

### Software Aging

When developers fail to manage the complexity of a software occurs what we call "Software Aging" that can be summed as the increasing complexity and the declining quality over the time. These symptoms lead to dangerous consequences, first, the software is going to be harder to understand and modify, impacting the team productivity, in consequence of that, software will also be less reliable since developers cannot understand what they are really modifying, increasing the number of bugs.

Software aging, in the long run, will make the software be abandoned and replaced by a newer one since that build a new software can be cheaper than maintain a old and complex software.

### How to prevent it?

There are some practices that we can use to help us to manage software complexity and prevent the software aging.

Documentation can be used to document software architecture, user requirements and also to explain coding decisions. In my opinion, write docs to software architecture and user requirements are important however I disagree with the use of docs to explain code. [I wrote about it last year in this post.][1]

Xtreme programming proposes two practices that I think are important when developing maintainable software. [Pair programming][2] and [Collective Ownership][3] help us to spread the knowledge between the team and also to write better code since people are always writing and analyzing the same codebase by different points of views.

The other practice that I want to talk about is code review, that I will explain in the next section and is a practice that has a important role for this whole article.

## Code Review

Code review is the act of constantly analyze the code that is been produced aiming to guarantee that the code are following standards, are easy to understand and are attending the user requirements. I have seen code reviews been used in two ways, when a commit is pushed to a repository to check if the code is good enough to be accepted or periodically to control the path that the software is taking.

They can also be manual, when made by other developers, or automatic, when made by code analysis tools.

### Manual Code Review

Manual code reviews are usually made by the most senior developers to analyze the code that was produced or by the whole team in a process very similar to sprint reviews.

Manual code reviews are very good to share technical skills between the whole team and to increase team knowledge about the project, however, they are also time consuming ( I have seen cases where the most senior developer almost doesn't write code cause he is always busy analyzing code ) and are difficult to be repeated in the same way (after all, we are humans, we are different from each other and we also have our good and bad days).

### Automatic Code Review

Code reviews can also be done automatically, however, I don't think that automatic code reviews should replace manual reviews, actually I think that they're complementary. Automatic code reviews can be a good filter to catch initial errors and save time that would be consumed with manual reviews by seniors developers.

However, to allow a computer to automatically review code is necessary to establish some guidelines, is necessary to establish some indicators that a computer can easily calculate and then perform logic operations on them. These indicators can be represented through code metrics.

## Code Metrics

Code Metrics are used to represent different software characteristics like size, complexity, coupling and cohesion. The first code metric used was lines of code (LOC), in sixties, to measure productivity and work effort. In 1976, McCabe proposed a metric called cyclomatic complexity to measure code complexity, since use LOC to measure complexity between softwares written in different languages is not effective (e.g. compare ruby and objective-c using lines of code is unfair). Since it, a lot of metrics have been proposed, the most famous are the [CK metrics][4], proposed in 1994 and the MOOD metrics, proposed in 1995.

Below, I chose 4 of these metrics to go deeper.

### Lines Of Code (LOC)

Lines of Code is the most obvious metric. It measures the number of lines that a class or a method has. As a best practice, is recommended to keep classes and methods as small as possible to avoid elements with too many responsibilities.

### Cyclomatic Complexity (CC)

Cyclomatic complexity was proposed to evaluate how complex methods and classes are. Basically, the CC for a method is the number of possible execution paths. For example, for a method without conditionals and loops CC is equals to 1. For a method with a conditional, the CC is equals to two since we have two possible paths, one when the conditional is evaluated as TRUE and the other one when the conditional is evaluated as FALSE.

We can calculate it using the formula:

> (number of decision ponts) - (number of exit points) + 2

Let's analyze a another example. The code below has a CC equals to 3.

{% highlight c# %}
if (c1()) 
{
    f1();
}
else
{
    f2();
}
 
if (c2()) 
{
   f3();
}
else
{
   f4();
}

{% endhighlight %}

It's recommended to keep the CC as low as possible to avoid complex code. Some authors say that the CC should not be greater than 10 or 15 but establish thresholds for code metrics is still a challenge.

CC is also directly related to unit testing. Since we want to cover all possible branches, when CC is high we need more and more tests to fully cover a method.

### Coupling

Coupling is a measure of how a class is dependent from other classes in the system and can be divided in two types, the afferent and the efferent.

Afferent coupling measures the number of other classes that use a specific class.

Efferent coupling measures the number of other classes that are used by a specific class.

The common belief says that we should keep our coupling as low as possible, after all, high coupled class are harder to reuse and to test. However, in my opinion, not every coupling is dangerous, for example, afferent coupling can be beneficial if we imagine that if a lot of classes use a specific class it can be a sign that this class is stable and reusable.

### Lack of Cohesion (LCOM)

Cohesion is a measure of how strongly related the responsibilities of a single module are. 

The LCOM uses the notion of similarity to calculate the lack of cohesion for a class. Two methods are considered similar if they share at least one class attribute or if one of the methods is called by the other one. The LCOM can be calculated using the following formula:

> (Number of pairs of methods with no similarity) - (Number of pairs of methods with some similarity)

There isn't negative LCOM, if the LCOM for a class is negative it should be evaluated as zero.

### Calculating Metrics

Let's see how we can calculate metrics for the class below. This class represents a person in a bank system, it's composed by 7 attributes (Name, LastName, Street, City, State, AccountNumber and AccountBalance) and 6 methods (GetName, GetAddress, GetAccount, Credit, Debit, CloseAccount).

{% highlight c# %}
public class Person
{
    public string Name { get; set; }
    public string LastName { get; set; }
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }
    public int AccountNumber { get; set; }
    public decimal AccountBalance { get; set; }

    public string GetName()
    {
        return string.Format("{0} {1}", Name, LastName);
    }

    public string GetAddress()
    {
        return string.Format("{0}, {1} - {2}", Street, City, State);
    }

    public string GetAccount()
    {
        return string.Format("{0}: {1}", AccountNumber, AccountBalance);
    }

    public void Credit(decimal value)
    {
        AccountBalance += value;
    }

    public void Debit(decimal value)
    {
        if (value > AccountBalance)
            throw new Exception("You don't have enough money.");

        AccountBalance -= value;
    }

    public void CloseAccount()
    {
        AccountNumber = -1;
        AccountBalance = 0;
    }
}
{% endhighlight %}

#### Lines of Code

Calculate the number of lines of code is very trivial, the whole class has 45 lines. GetName, GetAddress, GetAccount and Credit has 4 lines each, while CloseAccount and Debit has 5 and 6 respectively.

#### Cyclomatic Complexity

When calculating the cyclomatic complexity for this class we can observe that 5 methods has no conditionals or loops, accordingly to the definition, they have a complexity equals to 1. Debit has only one conditional so it's complexity is equals to 2. By summing the complexity of the methods, we can say that the Person class has a complexity equals to 7.

#### Coupling

There is no coupling in this class since it don't use any external class and we can't know if other classes are using it since we are analyzing only one class.

#### Lack of Cohesion

Calculate the LCOM here is a little tricky. To make it simpler let's separate the number of pairs of methods without similarity from those that have some similarity.

##### Similar Methods (6)
<p></p>

- GetAccount -> Credit
- GetAccount -> Debit
- GetAcchout -> CloseAccount
- Credit -> Debit
- Credit -> CloseAccount
- Debit -> CloseAccount

##### Non-Similar Methods (9)
<p></p>

- GetName -> GetAddress
- GetName -> GetAccount
- GetName -> Credit
- GetName -> Debit
- GetName -> CloseAccount
- GetAddress -> GetAccount
- GetAddress -> Credit
- GetAddress -> Debit
- GetAddress -> CloseAccount

Following the formula, we can see that the LCOM for this class is 3.

### Improving Code Using Metrics

If we analyze the metrics we extracted in the previous section, the only number that can catch our attention is the LCOM, the others are ok, after all, we are working with a very simple class. 

But what the LCOM can say? Accordingly to the definion, when the LCOM for a class is high it can be a hint that our class are accumulating responsabilities and we can split it in two or more classes.

If we analyze the class we can see that it really accumulating responsabilities, this class handles user info (Name, LastName), address (Street, City and State) and account info (AccountNumber, AccountBalance). Let's break this class in smaller pieces and see what happens.


{% highlight c# %}
public class Person
{
    public string Name { get; set; }
    public string LastName { get; set; }
    public Address Address { get; set; }
    public Account Account { get; set; }

    public string GetName()
    {
        return string.Format("{0} {1}", Name, LastName);
    }                
}

public class Address
{
    public string Street { get; set; }
    public string City { get; set; }
    public string State { get; set; }

    public string GetAddress()
    {
        return string.Format("{0}, {1} - {2}", Street, City, State);
    }
}

public class Account
{
    public int AccountNumber { get; set; }
    public decimal AccountBalance { get; set; }

    public string GetAccount()
    {
        return string.Format("{0}: {1}", AccountNumber, AccountBalance);
    }

    public void Credit(decimal value)
    {
        AccountBalance += value;
    }

    public void Debit(decimal value)
    {
        if (value > AccountBalance)
            throw new Exception("You don't have enough money.");

        AccountBalance -= value;
    }

    public void CloseAccount()
    {
        AccountNumber = -1;
        AccountBalance = 0;
    }
}
{% endhighlight %}

### Recalculating Metrics

Let's what happened with the metrics after our refactoring.

Person LCOM was equals to 6, now, after that we divided the responsabilities, we have 3 classes, Person, Address and Account, and since every method are similar, they have LCOM equals to 0 (LCOM cannot be negative). Our system became more cohesive.

Person CC was equals to 7, now, our 3 classes, Person, Address and Account, have CC equals to 1, 1 and 5, respectively. Now, our classes are also less complex.

Coupling and Lines of Code changes were insignificant to the final result.

Note that our classes are now more likely to be reused since they encapsulate behaviors that can be also used in other areas of the system.

## Tools to calculate metrics

There's a lot of tools to calculate these metrics automatically. Some of them are free, others not. Some offer support for several programming languages, others are focused in only one. Below I will discuss the ones that I think are more relevant in the field and I will also talk about Code Analytics, the tool that I'm developing as a part of my master's degree.

### Sonar

[Sonar][5] is, probably, the most used tool for metrics extraction in the industry and is also the most complete. It can analyzes more than 20 programming languages and provides good charts to help us to visualize these metrics, e.g. charts for metrics over the time and filtered by developer. Sonar has a instance for open source projects that you can see [here][8], if we want to use it to private projects we can install a local version or use a service like [CloudBees][6].

### NDepend

[NDepend][7] is commercial tool used to analyze .NET code. It analyzes more than 82 metrics and we can visualize them through different charts. We can also use the CQLinq (a query language) to write our own custom rules or to query code. Unfortunately, it's paid ($405).


### CodeAnalytics

CodeAnalytics is the tool that I'm developing in my master's degree to extract metrics from C# code. You may be asking, why am I developing another tool instead to use the existing ones? 

The first reason is that I want to provide data sharing between the users, I want allow that computer scientists and developers use metrics extracted from different projects to build thresholds and guidelines as well to develop new code metrics. This feature isn't currently supported by the other tools.

The second is that I want a project, that different from NDepend, is free and open source and, different from Sonar, has the code responsible for metric extraction entirely written in the same language that the one which is under analysis. In my opinion, is easier to convince developers to contribute to the project if they are capable to code in their primary language than force them to write extraction rules in a unique language like Java.

As soon as possible I will be writing more about CodeAnalytics here and try to release a alpha to be analyzed by the community.

## Conclusion

In this post we covered how a software project evolves and how it can be dangerous if we don't take care, we also saw how we can manage the increasing complexity through code reviews. We also saw what are code metrics and how we can calculate and use them to achieve better code. At the end, we also saw which tools we can use to automate this process and allow it to be integrated in our workflow.

Despite of code metrics have been researched for decades, I still think that most of the developers and companies even know what they are and how they can be useful to increase code quality and to guarantee that our software can grow more maintainable and reliable. However, its use is growing and I have seen some companies reporting projects that were benefited by use them.

I'm researching about code metrics for a year and I really want to bring what I'm learning to the industry, this article is my initial effort and I hope that it can generate good discussions about this subject.

See you soon!

## Want more?

Below are more resources about code metrics and code quality:

- [Refactoring: Improving the Design of Existing Code][9]
- [Object-Oriented Metrics in Practice: Using Software Metrics to Characterize, Evaluate, and Improve the Design of Object-Oriented Systems][10]
 
[1]: /2013/07/11/5-reasons-to-avoid-code-comments
[2]: http://www.extremeprogramming.org/rules/pair.html
[3]: http://www.extremeprogramming.org/rules/collective.html
[4]: http://faculty.salisbury.edu/~stlauterburg/COSC425/MetricForOOD_ChidamberKemerer94.pdf
[5]: http://www.sonarqube.org/
[6]: http://www.cloudbees.com/
[7]: http://www.ndepend.com/Default.aspx
[8]: http://nemo.sonarqube.org/
[9]: http://www.amazon.com/gp/product/0201485672/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0201485672&linkCode=as2&tag=paulorti-20&linkId=GTUH7K4JEGIHNORX
[10]: http://www.amazon.com/gp/product/3642063748/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=3642063748&linkCode=as2&tag=paulorti-20&linkId=WWCJT3SUKEO3IKEF
