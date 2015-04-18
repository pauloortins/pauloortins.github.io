---
layout: post
title:  "Xamarin.Forms Pages: TabbedPage and CarouselPage"
date:   2015-04-18
categories: Mobile Xamarin
comments: true
---

Hi guys! In this post I come back to show more two options of Pages in Xamarin.Forms, they are the TabbedPage and the CarouselPage. Let's see how we can use them to create our mobile apps.

<!--more-->

### TabbedPage

Besides the stack-based navigation, that we can achieve using the [NavigationPage][7], another very used kind of navigation is the tab-based navigation. We use this kind of navigation when accessing our social accounts in our devices, we can see this kind of navigation in the [Facebook][1], [Twitter][2], [Instagram][3] and in [Quora][11] (picture below).

![Quora Screenshots][4]

Something to keep in mind when we talk about tabs is the way each operating system handle them, in Android, tabs are positioned in the top and only text are shown, in iOS, tabs are positioned at the bottom and contain a text and an icon, in Windows Phone, tabs don't have a tab format they are only texts. I always talk about it in this blog, respect these different characteristics is important to guarantee that our users always have a fluid and intuitive user experience. You can check this differences in the Quora app above and in the demo below.

Fortunatelly, there is the `TabbedPage`, a specific `Page` to work with tabs and that take care of all these differences for us. [In my intro post about Xamarin.Forms][5], we had the first contact with this class however in this post we will deepen this knowledge.

![Tabs Demo][6]

A TabbedPage doesn't have content by itself, actually, we will set children [ContentPages][7] that will be managed by the `TabbedPage`. It's important to don't forget the `Title` and `Icon` properties since they will be used in the UI. The code below shows how we create a `TabbedPage` and two `ContentPages` (that will be the our tabs). As icons, I'm using the [Ionic Icons][12] that is a very good library to find icons to use in our apps, by default, in iOS, the only one that have tabs with icons, images are converted to gray independent of its original color.

{% highlight c# %}
// Blue Page
public class BluePage : ContentPage
{
	public BluePage ()
	{
		BackgroundColor = Color.Blue;
		Title = "Blue Page";
		Icon = "heart.png";
	}
}

// Red Page
public class RedPage : ContentPage
{
	public RedPage ()
	{
		BackgroundColor = Color.Red;
		Title = "Red Page";
		Icon = "icecream.png";
	}
}

// TabPage
public class TabPage : TabbedPage
{
	public TabPage ()
	{
		this.Children.Add (new BluePage ());
		this.Children.Add (new RedPage ());
	}
}
{% endhighlight %}

When executing the code above in Android and in iOS, we see a view with two tabs and, the most important, with a native lock-and-feel in both platforms.

![TabbedPage][8]

When working with tabs, we frequently need to interact with the current tab or to trigger an action when the current tab is changed. In the `TabbedPage` we have the `CurrentPage` property that can be used  to set and get the current tab and the event `CurrentPageChanged` that is triggered everytime a new tab is selected.

{% highlight c# %}
// Gets and sets the current page
public T CurrentPage {
	get;
	set;
}

// Event triggered when a new tab is selected
public event EventHandler CurrentPageChanged

{% endhighlight %}

### CarouselPage

Galeries are also very common in mobile apps, these galleries can be galleries of people, photos, albums, restaurants and so on. In this navigation type, we have the current page that is the page we are seeing in detail and we can navigate between pages using the *swipe left* and *swipe right*, events that consist in drag the screen horizontally to the left or to the right. In the iPhone photo app below, we can navigate between the photos through a gallery.

![Gallery Sample][9]

To create page galleries in Xamarin.Forms, the `CarouselPage` is the right choice. The `CarouselPage` is from the same category of `TabbedPage`, both inherit from the same class (the `MultiPage`, that is a page with children pages), however, while `TabbedPage` organizes its children pages in tabs, the `CarouselPage` organizes these pages side-by-side. The code below shows how we can create a `CarouselPage` with 4 colored `ContentPages`, you can see that the setup is equals to the one we did in the `TabbedPage`, the difference is only how these children pages are handled under the hood.

{% highlight c# %}
// Blue Page
public class BluePage : ContentPage
{
	public BluePage ()
	{
		BackgroundColor = Color.Blue;
	}
}

// Red Page
public class RedPage : ContentPage
{
	public RedPage ()
	{
		BackgroundColor = Color.Red;
	}
}

// Yellow Page
public class YellowPage : ContentPage
{
	public YellowPage ()
	{
		BackgroundColor = Color.Yellow;
	}
}

// Green Page
public class GreenPage : ContentPage
{
	public GreenPage ()
	{
		BackgroundColor = Color.Green;
	}
}

// CarouselPage
public class MyCarouselPage : CarouselPage
{
	public MyCarouselPage ()
	{
		this.Children.Add (new BluePage ());
		this.Children.Add (new RedPage ());
		this.Children.Add (new YellowPage ());
		this.Children.Add (new GreenPage ());
	}
}
{% endhighlight %}

When executing the project below, we can see the blue page as starting page but we can navigate between the pages dragging the screen horizontally.

![Carousel][10]

As `CarouselPage` inherits from the same base class that the `TabbedPage`, we can also access the current page and trigger select events in the same way we can do in a `TabbedPage`.

{% highlight c# %}
// Gets and sets the current page
public T CurrentPage {
	get;
	set;
}

// Event triggered when a new tab is selected
public event EventHandler CurrentPageChanged

{% endhighlight %}

Both samples are available in my github: [TabbedPage][13] and [CarouselPage][14].

### This is it!

In this post we saw more two kinds of `Pages`, this time, pages that act like a managers of other pages that are the `TabbedPage` and and the `CarouselPage` and we also saw how easy is to use them to create navigations equal to the ones that we use in other apps in our mobile devices. In the next post I will finish this section with the last kind of page: the `MasterDetailPage`.

[1]: www.facebook.com
[2]: www.twitter.com
[3]: www.instagram.com
[4]: /images/posts/2015-04-17/quora.png
[5]: /2015/04/04/xamarin-forms/
[6]: /images/posts/2015-04-17/example-forms.png 
[7]: /2015/04/12/xamarin-forms-pages-contentpage-navigationpage//
[8]: /images/posts/2015-04-17/tabpage.png
[9]: /images/posts/2015-04-17/gallery.jpg
[10]: /images/posts/2015-04-17/carousel.png
[11]: www.quora.com
[12]: http://ionicons.com/
[13]: https://github.com/pauloortins/MyXamarinSamples/tree/master/TabsPage
[14]: https://github.com/pauloortins/MyXamarinSamples/tree/master/Carousel
