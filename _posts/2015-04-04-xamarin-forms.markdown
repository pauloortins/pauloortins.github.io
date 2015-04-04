---
layout: post
title:  "Xamarin.Forms, what is it?"
date:   2015-04-04
categories: Mobile Xamarin
comments: true
---

Hi guys, how you doing? I'm here one more time to talk about Xamarin, in this post I'll talk about Xamarin.Forms, a API launched in 2014 that enable us to share even more code between our mobile apps.

<!--more-->

### Xamarin what?

If you don't know very well what Xamarin is or you never heard anything about it, I wrote a blog post explaining everything [you can check it here][3]. The truth is that the platform is evolving fast and each year, we, mobile developers, are presented by Miguel de Icaza, Nat Friedman and their team with new solutions for our biggest problems.

The [Xamarin.Forms][4] was one of these big gifts, as we saw in the other post, Xamarin allows us to create hybrid apps using C# and sharing a good amount of our code between the platforms, the code that we can share is the one responsible for common features between the platforms, e.g. business rules, validation, web services, database connections and so on.

However, in this approach, an amount of code still needs to be written specifically for each platform, that is the UI code, accordingly to the Xamarin approach when writing an UI for each platform we can guarantee that our users will have the best user experience available and exactly the same that they have when they are using other apps in their preferred operating system. So, until this point, to create a view using Xamarin I had to create an `UILabel` in the iOS, a `TextView` in the Android and a `TextBlock` in the Windows Phone.

But wait, if we already know which classes we need to instantiate in each platform, what are their attributes and all these classes are already written in C#, by default (Windows Phone), Xamarin.Droid (Android) and Xamarin.iOS (iOS), why we can’t create a class that know how each platform instantiates a text control and reuse even more code? To insert a text control we could instantiate a class called `Label` and let Xamarin do his work and instantiate the right class for each platform, to instantiate a text input we could instantiate an `Entry` and maybe, in a dream, instantiate a `TabbedPage` and the Xamarin, by itself, create a page with tabs exactly equal to how they are created in each platform (in the iOS, tabs stay in the bottom with a text and an icon, in the Android, tabs are text only and stay in the top). I have to tell you something, it’s not dream and it’s not far to happen, it’s exactly the **Xamarin.Forms**, an API that encapsulates the native API’s and allows us to create controls using an unified set of controls and share even more code between the apps, in some cases, a value close to 100% can be expected.

### Xamarin.Forms, only one code, three different experiences

Take a look in the code below, it uses Xamarin.Forms API to create a page with two tabs, one tab is a login page, in this sample, we can already start to know some of the Xamarin.Forms components, as the `TabbedPage` that is used to create pages with tabs, the `ContentPage` used to create pages with content, the `Entry` used to create text inputs and the `Button` to create the buttons.

{% highlight c# %}
using Xamarin.Forms;

var profilePage = new ContentPage {
    Title = "Profile",
    Icon = "Profile.png",
    Content = new StackLayout {
        Spacing = 20, Padding = 50,
        VerticalOptions = LayoutOptions.Center,
        Children = {
            new Entry { Placeholder = "Username" },
            new Entry { Placeholder = "Password", IsPassword = true },
            new Button {
                Text = "Login",
                TextColor = Color.White,
                BackgroundColor = Color.FromHex("77D065") }}}
};

var settingsPage = new ContentPage {
    Title = "Settings",
    Icon = "Settings.png",
    (...)
};

var mainPage = new TabbedPage { Children = { profilePage, settingsPage } };
{% endhighlight %}

When executing the code above the magic happens and we can see the three apps below, **the most important thing here is to see how native these apps look, we can see it in the tabs placement and in the buttons and input texts.**

![Apps Xamarin.Forms][1]

### There are a lot more components to be used

The components cited above are some of the several components available, the image below shows it. Xamarin.Forms components are divided in three 3 categories, **Pages**, are the actual pages and `TabbedPage` and `ContentPage` belong to this category, the **Layouts**, are components that are used to organize other components, the `StackLayout` belongs to this category, and for last, the **Controls** that are the controls that the user will interact, the `Entry` and the `Button` belong to this last category.

![Pages, Layouts e Controles][2]

### Access to natives APIs and Elements

And that's not all, despite of Xamarin.Forms encapsulates each platform APIs, it's still possible, if needed, to use specific implementations as text-to-speech APIs, file access or custom UI. These specific implementations are done in two ways: using `CustomRenderers` or using `DependecyService`.

### See you soon!

I can't be more excited with Xamarin.Forms, the API is evolving fast and, for the happiness of my customers, it already has proven to be an excellent competitive advantage enabling us to deliver **great apps in the 3 major platforms and with a very reduced cost.**
In the next posts I will talk more about Xamarin.Forms, but if you can't wait, at the end of this post you can find some very interesting links. Cya!

### Interesting Links

More about Xamarin.Forms:

- [Xamarin Tutorial][6]
- [Creating Mobile Apps with Xamarin.Forms, in preview][7]
- [Xamarin.Forms Kickstarter, web book about Xamarin.Forms][5]
- [Talk - Your First Xamarin.Forms App by Craig Dunn][8]

[1]: /images/posts/2015-04-04/example-forms.png
[2]: /images/posts/2015-04-04/forms-controls.png
[3]: /2014/06/25/why-choose-xamarin/
[4]: http://xamarin.com/forms
[5]: http://www.xforms-kickstarter.com/
[6]: http://developer.xamarin.com/guides/cross-platform/xamarin-forms/introduction-to-xamarin-forms/
[7]: http://www.amazon.com/gp/product/B00NXYJ8DK/ref=as_li_tl?ie=UTF8&camp=1789&creative=390957&creativeASIN=B00NXYJ8DK&linkCode=as2&tag=paulorti-20&linkId=7445BJ7K7EJKNLXU
[8]: https://www.youtube.com/watch?v=xWyOcEpNyXQ
