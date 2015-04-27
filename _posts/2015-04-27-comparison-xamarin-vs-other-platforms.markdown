---
layout: post
title:  "Comparison: Xamarin vs Other Platforms"
date:   2015-04-27
categories: Mobile Xamarin
comments: true
---

Are you curious to know how good is Xamarin performance when compared with other platforms? What's the final app size? How much code sharing can we expect? In this post we will see a list full of comparisons that answer these questions and even more.

<!--more-->

### Cause everyone wants to know
 
Every time I give a talk or a course or I talk with someone about Xamarin questions like these arise every moment. Productivity, performance, user experience, UI and tooling are obvious concerns when starting a new mobile project and everyone wants to know it.

Normally, these questions are answered in the following way: "Buddy, I have some links in my bookmarks that I can share with you". The problem with this approach is that, it's not scalable, automatic and neither publicly available.

My idea with this post is to create a list where I can share links that do good comparisons between Xamarin and other platforms in areas like performance, user experience, productivity, costs, market and any other info that can be useful to us.

### A collaborative list and made by multiple hands

I will appreciate any collaboration from you, feel free to send me any comparison that you judge relevant.

Below is our list until now.

#### General

[Choosing the Right Mobile Techonology:][1]
The Magento wrote a excellent white paper where they do a comparison between 4 development platforms: Native, Cordova, Classic Xamarin and Xamarin.Forms. In these paper they discuss the following points: user experience, performance, productivity, maturity, tools, code sharing, industry trends and future support.

#### Performance

[08/24/2014 - Performance comparison between Xamarin.Android+C# vs Dalvik/ART+Java:][6]
Comparison between Xamarin.Droid + C# and Android + Java in the following points: arithmetic, collections, generics and strings.

[12/23/2014 - Mobile Development Platform Performance (Native, Cordova, Classic Xamarin, Xamarin.Forms) part. 1:][2]
In this comparison Kevin Ford created test applications to evaluate the apps size and also measured performance indexes like app load times, the time to load 1,000 records from Azure Mobile services and the time to do prime number calculations.

Interesting points:

The Xamarin app is way bigger that the app size when using other development platforms, at least for small/test applications, I confess that I'm curious too see this comparison in a real production app (Do you know someone who migrated a Android/iOS app to Xamarin? Please tell me).

Xamarin performance was very close to the native platforms and even faster in some scenarios. Cordova performance was the worst (It was already expected).

[02/04/2015 - Mobile Development Platform Performance (Native, Cordova, Classic Xamarin, Xamarin.Forms) part. 2:][3]
Some months later, Kevin Ford did one more post where he measured again the size of the apps and more performance indexes, in this time, those related to IO. In this study, the performance indexes were: load times, time to insert 1,000 records in a SQLite, time to read 1,000 records from a SQLite, time to insert 1,000 lines in a file and, for last, the time to to read 1,000 lines from a file.

The results in this  were similar to the first post, the more interesting point in this post to me was the difference between Xamarin Versions in Android when using different SQLite libraries, when using a cross-platform implementation the measures were the worst (worse than the Cordova version!), however, when using a specific version for Xamarin.Droid the measures were the same than the ones for java.

[02/10/2015 - Mobile App Performance part. 1:][4]
In thi post Harry Cheung uses a GPS Tracking app to compare both computation performance as memory use when using different hybrid and native platforms in iOS and in Android. In iOS were tested the J2ObjC, Xamarin, RoboVM, RubyMotion and Swift while in Android were only tested the Java and Xamarin. Impresses how Xamarin is competitive when compared with the native platforms and it's still cross-platform.  

[03/15/2015 - Mobile App Performance part. 2:][5]
After some feedbacks, Harry Cheung added more mobile platforms and more devices to his benchmark transforming it in the most complete performance benchmark available. In iOS were tested the following platforms: C++, Swift, Xamarin, J2ObjC, RoboVM, Objective-C and Javascript (Mobile Browsers and Titanium) while in Android were the following: C++, Xamarin, Java and Javascript (Mobile Browsers and Titanium).

The top 3 winners were (in order), in iOS: C++, Swift and Xamarin, and in Android: C++, Xamarin and Java. With a second and a third place here (always with close numbers) Xamarin can be considered a really fast alternative to mobile development.

#### Code Sharing and User Experience

I'm still looking for articles that talk about code sharing in real apps and articles that evaluate the user reviews when using apps made in different platforms, here, a good study would be to run a experiment to collect user feedback when using different versions of the same app varying only the development platform.

[1]: http://magenic.com/Portals/0/Magenic-White-Paper-Choosing-the-Right-Mobile-Technology.pdf
[2]: http://windingroadway.blogspot.com.br/2014/12/mobile-development-platform-performance.html
[3]: http://windingroadway.blogspot.com.br/2015/02/mobile-development-platform-performance.html
[4]: https://medium.com/@harrycheung/cross-platform-mobile-performance-testing-d0454f5cd4e9
[5]: https://medium.com/@harrycheung/mobile-app-performance-redux-e512be94f976
[6]: http://xamarinandroid.blogspot.com.br/2014/08/performance-comparison-between.html
