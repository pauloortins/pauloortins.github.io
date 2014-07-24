---
layout: post
title:  "Xamarin Cross-Platform Application Development - Review"
date:   2014-07-23
categories: Mobile Xamarin
comments: true
---

Every time I can, I like to read books about the subjects that I’m looking to learn, programming is not different, besides it’s considered a practical field where most of the learning comes trough experience (the famous “learn by doing”), I think important to read books, they help us to build theoretical foundation that we can use to justify our practical decisions.

In the last week, I take a time to deep my knowledge about mobile development using Xamarin. The chosen source was the book [Xamarin Cross-platform Application Development][1] written by [@JonathanPeppers][2] and published by [Packt][8].

![Book Image][3]

Reading this book was very pleasant, in a general way, I can define it as a ideal guide to someone who is interested to learn Xamarin but is also familiar with C#. The book follows an interesting approach mixing practice and theory as the concepts are being explained.

In the first chapter, the book shows how we can configure our environment with the minimum required to develop cross-platform applications, in this moment, we install the IDEs (Xcode and [Xamarin Studio][7]), the simulators for each platform and we create the accounts on iOS Developer Program and on Google Play.

In the second chapter, is presented a quick overview about how each platform works and which basic concepts we need to know through a “Hello Word” project. This initial project is also used to check if the development environment is working properly.

In the third chapter, the book talks about code sharing, one of the things that I most like in Xamarin applications, in this chapter we can have a overview about strategies that we can use to share code between platforms, strategies like ViewModels, Inversion of Control, File Sharing and Portable Class Libraries are discussed and get their advantages and disadvantages well explained.

The next chapters show how we can create a real application using the already explained concepts. The chosen application is the XamChat, an application very similar to WhatsApp. The development proceeds step by step from the creation of the Core (code that will be shared by both platforms) as well as a chapter for the specific code for each platform.

The chapter 8 talks about an interesting point when we are developing a mobile application that is server-side communication. In this chapter, is presented the [Azure Mobile Services][6], a Microsoft Service that enable us to easily develop a backend for our apps and also to send push notifications to our users.

The chapter 9 and 10 talk again about productivity increase and code sharing, in them, are presented the Xamarin Component Store, a kind of Nuget for [Xamarin components][4], and the [Xamarin Mobile][5], a library that exposes a single set of APIs for accessing common mobile device functionality (like Location, Contacts and Camera) across iOS, Android and Windows platforms.

At the end of the book I could reach my goal that was to deepen my Xamarin knowledge as well to acquire a foundation that I can use to start new projects in the right way and aiming to reuse as much code I can.

I hope this review can help others who are looking for a good resource to learn Xamarin.

Do you know another good book about Xamarin? If yes, don't forget to share it in the comments below.

See you later.

[1]: http://goo.gl/o7DqKm
[2]: https://twitter.com/JonathanPeppers
[3]: http://ecx.images-amazon.com/images/I/51qmaWBXSDL.jpg
[4]: http://components.xamarin.com/
[5]: https://components.xamarin.com/view/xamarin.mobile
[6]: http://azure.microsoft.com/en-us/documentation/services/mobile-services/
[7]: http://xamarin.com/studio
[8]: http://www.packtpub.com/xamarin-cross-platform-application-development/book
