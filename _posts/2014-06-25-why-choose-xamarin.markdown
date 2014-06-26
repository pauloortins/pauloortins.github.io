---
layout: post
title:  "Why choose Xamarin?"
date:   2014-06-25
categories: Mobile Xamarin
comments: true
---

A common doubt when we are starting a new mobile project is which strategy should we choose. Should we develop a webapp? A phonegap hybrid app? Or should we go for the native ones and develop a specific version for each platform?

Each one of these strategies has advantages and disadvantages that should be analyzed carefully accordingly to the application's characteristics and the team's knowledge. 

Examples of characteristics that can be analyzed are:

- Will it use native APIs like Geolocation, Calendar and Accelerometer?
- Available Resources (Time and money)
- Team Skills (Objective-C, Java, C#, Javascript, HTML5, CSS3)
- User needs and expectations
- Performance

### Why choose web or hybrid apps?

Web or hybrid are faster and easier to be developed, after all, only one code base will be developed and maintained, this app then will be interpreted by each native browser, for web apps, or will be packed by tools like Phonegap and then will be downloaded from stores and run in each platform, for the hybrid ones.

The skills needed to develop apps like these (HTML5, CSS3 and Javascript) are also easier to be found between the software developers since they are also used for web programming, due it, these developers are also cheaper to hire, keeping development and maintenance cost lower than when we develop native apps.

IMO, this kind of strategy is the best choice for apps that are under a tight deadlines and short budgets and will be used by users that will accept apps with medium quality and with look and feel different from the apps that they use in their native platforms. If these apps are gaining more and more importance in their business or we need to integrate these apps with native API's, I advise migrate them to native versions, that have a higher quality and access to these native API's.

### Why choose native apps? 

IMO, native apps are way better than the others. Response time, user interface, support for native API's and native behavior are characteristics that users care about and they are used to have in other apps in their smartphone. In such a way,I believe that native apps should always be kept in mind when developing a mobile strategy, mainly, if you expect that your users interact with them for a long time, e.g. a sales force app. On the other hand, native apps are also harder and more expensive to develop due the singularities of each platform. These advantages and disadvantages should be analyzed wisely.

#### Multiple Programming Languages

Develop a specific version for each mobile platform (Android, iOS and Windows Phone) generates different codebases, each one written in a different language, Java for Android, Objective-C for iOS and C# for Windows Phone, if your app has a server side, maybe you will have more one or two languages involved. Costs and work to maintain all these code bases increase with time.

#### Multiple Developers

If we are working with different programming languages, chances are high that we will need multiple developers, probably one for each platform. In this scenario, we have multiple salaries to pay and there are knowledge islands where each developer only know about his platform/language. I already see it happening in some companies and when one of these developers decides to leave the company, hire a new developer with same skills is a hard task.

#### No reuse and no knowledge sharing 

Do you know that awesome library that your iOS developer created? Or that other open source project written in Java? Both will have to be ported or replaced for other similar for each platform. Code sharing is impossible.

### And if we could merge the two worlds?

And if we could develop apps in only one language and also have the best of native apps? Xamarin tries to fill this gap.

## Xamarin, cross-platform development

Xamarin is a intermediary solution between the hybrid strategy with Phonegap and the native strategy, with Xamarin we can write C# code that will be shared by all platforms and at the same time, we are also creating apps with native behavior.

### Only One language

Xamarin allow us to write code in only one language and it solves the majority of the problems that I wrote above (multiple programming languages, multiple developers, lack of reuse and code sharing). With Xamarin we can assemble a more homogeneous team where all developers can share knowledge as well as code with each other. Now, hire another mobile developer is easier.

### Native Interface

The C# code is not totally reused between platforms and it's a advantage, you know why? Because the code that will not be shared is the one who will guarantee that apps have a unique and native behavior and will attend the user demand for apps similar to those they are also using in their smartphones. The image below shows how it works.

![Xamarin Diagram][1]

### Always up-to-date with the latest native APIs

Xamarin provides a API that mirrors the native APIs (Android and iOS). This mirrored API is so well done that we can ask for help for other iOS or Android developers or read tutorials built in native languages and convert the code easily to C#, the API names remain the same. The speed between updates is also very fast.

### Reuse your team

For us at OnceDev and for our customers it's extremely favorable. If we already have good C# developers with years of experience developing web and desktop applications why don't use them to also develop mobile apps? It allow us to reduce our cost since we dont need to hire new developers or learn a new programming language.

### Reuse your tools

Xamarin is perfectly integrated with Visual Studio. We are able to reuse the same toolset that we already use as C# developers, so you can still use TFS, Resharper, NCrunch and so on. The Xamarin 3.0 also brought new interface builders to Visual Studio, so we are able to develop interfaces without leave our favorite IDE.

### Costs

Xamarin is paid, the licenses cost $299/year (Indie), $999/year (Business) and $1899/year (Enterprise), per platform and per developer. However, salaries are high in these days, so, these licenses, at the end of the year, wont cost more than a monthly salary, but they can prevent you to hire some developers since your developers can now work, easily, in more than one platform.

### Conclusion

The bet on Xamarin have been a god choice for us at OnceDev, we were able to use all our knowledge and tools that we already have as C# developers, but this time to develop mobile apps. We also perceived a productivity increase that allow us to reduce our development cost and consequently offer better deals for our customers. Definitively, we are going to put more efforts on it in the next months.

### Want more?

Below are more resources about Xamarin:

- [Xamarin White Paper][2]
- [Xamarin Docs][3]
- [Xamarin Cross-platform Application Development][4]
- [Mobile Development with C#: Building Native iOS, Android, and Windows Phone Applications][5]

[1]: http://xamarin.com/content/images/pages/platform/code-sharing.png
[2]: http://cdn1.xamarin.com/webimages/assets/Xamarin_Whitepaper-Key_Strategies_for_Mobile_Excellence.pdf 
[3]: http://developer.xamarin.com/guides/
[4]: http://www.amazon.com/gp/product/1849698465/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1849698465&linkCode=as2&tag=paulorti-20&linkId=KRE4GQZJJSRWHTAF
[5]: http://www.amazon.com/gp/product/1449320236/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=1449320236&linkCode=as2&tag=paulorti-20&linkId=EZNVZZZ7XLW6HVJJ
