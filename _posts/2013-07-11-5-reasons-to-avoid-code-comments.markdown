---
layout: post
title: 5 reasons to avoid code comments
categories: Best Practices
date: 2013-07-11
comments: true
---

![Code Comments][1]

_DISCLAIMER: When I say 'to avoid code comments', it doesn't mean that I don't write comments, it means that I try to avoid code comments as               much as I can, but sometimes I do, when I think it worth._

`We spend more time reading software than writing software.` I never seen any scientific study proving it, but in software field it's like a dogma or a common  belief. Due to it, it's important to try to write software easy to read, it's important to care about the readability of our code. There are some techniques that programmers can use to achieve it. One of them, is write code comments.

When talking about code comments, there is big debate about it. `Should we use comments to describe what our code does ? We should focus on write expressive code that doesn't require comments to be read ?` Joe Kunk wrote a blog post about this debate - <a href="http://visualstudiomagazine.com/articles/2011/01/06/to-comment-or-not-to-comment.aspx">To Comment or Not to Comment</a>. There are the ones who say that for a code be considered good it should be well-documented and there are the ones who say that we should avoid comments because it's normally used to explain/hide bad code.

`In my opinion`, influenced by the books, <a href="http://www.amazon.com/gp/product/0132350882/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0132350882&linkCode=as2&tag=paulorti-20">Clean Code</a> and <a href="http://www.amazon.com/gp/product/0201485672/ref=as_li_ss_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=0201485672&linkCode=as2&tag=paulorti-20">Refactoring</a>, `we should avoid to write comments unless we have a really good reason to write one` (for example,in a mathematical algorithm) or we are obligated to do it due to some company rules or process. Below, I listed my 5 concerns about code comments.

## Where I think that code comments fail

`1. They tend to encourage bad code.` There is an idea that commented code is a good code, so people often write comments in their code to make them look better. If we need to explain our code adding comments is already a signal that maybe we are writing bad code. Every time we start to write a comment we should think if we can be more expressive just cleaning our code.

`2. We spend more time writing and maintaining them.` Comments usually are a second version of the code. When we are writing a comment for a function we are repeating ourselves. We are transgressing the <a href="http://en.wikipedia.org/wiki/Don't_repeat_yourself">DRY (Don't Repeat Yourself)</a> principle. We are spending time and adding complexity. Software requirements changes and code has to change too. If we are writing comments we have to maintain the comments too. So we can end up spending the double of the time when we have to make a change. We could use this time to improve our code or to develop new features.

`3. Comments are not testable/verifiable.` When we are changing code we can rely on tools, like compilers, IDEs and unit tests to help us. Comments don't. Comments don't have these tools.  You can't rely on tools or unit tests to make sure they are right, in the correct place or out-of-date. Once you write a comment you have a not testable piece to care about his correctness and once it fails, it will fail silently.

`4. They are less reliable than the documented code.` Usually, comments become obsolete and they lose the connection with the code. Then, programmers can read them, and be cheated. Even if the comments are up-to-date, the only way to know if the code does what it should, will always be reading the code. A practical example, if our boss ask to us if a change was made, where we should look ? Code or Comments ?
Of course we will look at the code.

`5. Some comment styles can fill a lot of screen space.` Some comment standards (like the below) use a lot of lines being a problem when you are trying to read as much code you can.

{% highlight java %}
/*
 
 @param title The title of the CD
 @param author The author of the CD
 @param tracks The number of tracks on the CD
 @param durationInMinutes The duration of the CD in minutes
*/

public void addCD(String title, String author, int tracks, int durationInMinutes) {
    CD cd = new CD();
    cd.title = title;
    cd.author = author;
    cd.tracks = tracks;
    cd.duration = duration;
    cdList.add(cd);
}
{% endhighlight %}

[1]: /images/posts/2013-07-11/codecomments.png
