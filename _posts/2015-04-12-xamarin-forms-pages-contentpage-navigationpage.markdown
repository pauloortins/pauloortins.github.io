---
layout: post
title:  "Xamarin.Forms Pages: ContentPage and NavigationPage"
date:   2015-04-12
categories: Mobile Xamarin
comments: true
---

In my last post I showed to you the structure of a Xamarin.Forms app, we also used some of its components to create a simple screen that had native look-and-feel in each one of the three platforms. From this post we will learn more about these components starting by the Pages.

<!--more-->

### Pages

A `Page` in Xamarin.Forms represents a screen in our mobile apps, it works in an analogous way to the `Activities` in the Android, to the `ViewControllers` in the iOS and the namesake `Pages` in the Windows Phone. The image below shows all types of Pages that belongs to Xamarin.Forms API, in this post we will cover the `ContentPage` and the `NavigationPage` leaving the `TabbedPage`, the `CarouselPage` and the `MasterDetailPage` to the next posts.

![Page Types][1]

#### ContentPage

The `ContentPage` is exactly a Page with a view inside her. In Xamarin.Forms each ContentPage has its content associated to a `Content` property of type View, this type is the superclass for all Layouts and Controls ([Do you remember the StackLayout, Label and Entry that we saw in the last post?)][10]. 

![ContentPage with Lorem Ipsun][2]

The image above shows a `ContentPage` where the `Content` property was filled with a Label that contains a random text. The code below was used to create this ContentPage. Take a look on it!

{% highlight c# %}
public class FirstPage : ContentPage
{
	public FirstPage ()
	{
		Content = new Label {
			XAlign = TextAlignment.Center,
			Text = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. " +
			"Nulla finibus, quam in elementum mattis, ipsum velit bibendum nisi, " +
			"ut feugiat ligula mi at lectus. Cras pretium elit a tortor accumsan " +
			"sagittis. Nunc vulputate libero sed sapien fermentum, eget condimentum " +
			"elit volutpat. Phasellus tincidunt sem mattis maximus auctor. Nunc congue " +
			"sodales urna, vel venenatis tortor tristique at. Vestibulum volutpat " +
			"sollicitudin mi vitae viverra. Nunc sollicitudin, nisl ac placerat congue, " +
			"erat libero fringilla ante, at sodales risus leo ut lorem."
		};
	}
}
{% endhighlight %}

In the `ContentPage` above we filled the `Content` property with a `Control` (Label is a Control), however, as I said in the beginning of the post, we can also use `Layouts` as Content. The image below shows a ContentPage where its Content property was filled with a `StackLayout` to show the rainbow colors.

![ContentPage with rainbow colors][3]

And below is the code that I used to create this page.

{% highlight c# %}
public class RainbowPage : ContentPage
{
	public RainbowPage ()
	{
		Content = new StackLayout () {				
			Children = {
				new BoxView() {Color = Color.Red},
				new BoxView() {Color = Color.FromRgb(255, 102, 0)},
				new BoxView() {Color = Color.Yellow},
				new BoxView() {Color = Color.Green},
				new BoxView() {Color = Color.Blue},
				new BoxView() {Color = Color.FromRgb(111, 0, 255)},
				new BoxView() {Color = Color.FromRgb(159, 0, 255)}
			}	
		};
	}
}
{% endhighlight %}

#### NavigationPage

The `NavigationPage` is used to manage a navigable stack of pages and it's also the most used way to navigate between pages in mobile apps (I'm sure that you already used several times this kind of navigation in your smartphone). A `NavigationPage` also added to the header a` NavigationBar` that will show a back button and the current page's title.

To create a `NavigationPage` we need to tell in it's constructor a initial page that will be the root of our stack. As in every stack, we will interact with it pushing and popping new pages (First-In First-Out, right?). Every Page in Xamarin.Forms implements the interface `INavigation` (box below) that provides for us the methods that we need to interact with the navigation stack. 

{% highlight c# %}
public interface INavigation
{
    Task PushAsync(Page page);
    Task<Page> PopAsync();
    Task PopToRootAsync();
    Task PushModalAsync(Page page);
    Task<Page> PopModalAsync();
}
{% endhighlight %}

In the next sample we will create 4 pages, a `NavigationPage`, that will manage the Navigation, two `ContentPage`, one red and one blue that will have buttons to interact with the navigation stack (we will be able to push a red page, to push a blue page, to pop the current page and to pop all pages except the root) and a Label that will print the navigation stack's current state. For the last, there is a `ColorPage` (that is also a ContentPage) that will encapsulate code common to the red and to the blue page, after all, we like Xamarin but we also like good practices, right? So let me show you some code. 

{% highlight c# %}
// Nav Page
public class NavPage : NavigationPage
{
	public NavPage () : base(new BluePage())
	{			
	}
}
{% endhighlight %}

NavPage code is very straightforward, it just creates a `NavigationPage` and initialize it with a `BluePage` (our root page).

{% highlight c# %}
// Blue Page
public class BluePage : ColorPage
{		
	public BluePage () : base()
	{			
		BackgroundColor = Color.Blue;
		Title = "Blue Page";
	}
}

// Red Page
public class RedPage : ColorPage
{
	public RedPage () : base()
	{			
		BackgroundColor = Color.Red;
		Title = "Red Page";
	}
}

{% endhighlight %}

Both `RedPage` and `BluePage` initialize its `BackgroundColor` property with their respective colors (it will fill the entire screen except the navigation bar) and the `Title` property that will be displayed in the navigation bar title and as a back button when we start to navigate between the pages. Pay attention that both pages inherit from `ColorPage`, since, as they have the same appearance and behavior, doesn't make sense to repeat this code in both classes.

{% highlight c# %}
// Color Page
public abstract class ColorPage : ContentPage
{
	private StackLayout _pages;

	public ColorPage ()
	{
		_pages = new StackLayout ();

		// Create Buttons
		var redButton = new Button () {Text = "Push Red Page"};
		redButton.Clicked += async (object sender, EventArgs e) => 
		{
			await this.Navigation.PushAsync(new RedPage());
		};

		var blueButton = new Button () {Text = "Push Blue Page"};
		blueButton.Clicked += async (object sender, EventArgs e) => 
		{
			await this.Navigation.PushAsync(new BluePage());
		};

		var backButton = new Button () {Text = "Back"};
		backButton.Clicked += async (object sender, EventArgs e) => 
		{
			await this.Navigation.PopAsync();
		};

		var rootButton = new Button () {Text = "Back to Root"};
		rootButton.Clicked += async (object sender, EventArgs e) => 
		{
			await this.Navigation.PopToRootAsync();
		};

		// Create Content
		Content = new StackLayout () 
		{
			Children = {
				redButton,
				blueButton,
				backButton,
				rootButton,
				_pages
			}	
		};
	}
		
	protected override void OnAppearing ()
	{
		// Override default OnAppearing and write NavigationStackContent
		_pages.Children.Clear ();
		foreach (var page in this.Navigation.NavigationStack) {
			_pages.Children.Add (
				new Label () 
				{
					Text = page.GetType().ToString()
				}
			);
		}
	}
}

{% endhighlight %}

The `ColorPage` code ended up being the most complex in this sample, however, it's not hard to understand, in the constructor, we build the page's visual elements (buttons and the label) and we initialize their respective navigation events using the methods, `PushAsync` to push a new page to the stack, `PopAsync` to pop the current page and the `PopToRootAsync` to pop all the pages except the root.

In this sample we will also use, for the first time in this blog, the `OnAppearing` method, the OnAppearing is part of the page's life cycle events, that allow us override them to do some things when a page appears, when the page disappears and so on. 

In this sample, I'm overriding the `OnAppearing` method to get all pages in the navigation stack and set the Label's text. 

But, you could ask, why I'm not doing it in the constructor? The answer is: "It's not possible".

When we are in page's constructor, the page is not built yet and, due to it, not belong yet to the navigation stack so we can't get the other pages.

When running the app we can see the following screens:

First Page

![First Page][4]

After we click in the Push Red Page button, we will be in a Red page and our navigation stack now has 2 pages.

![Second Page][5]

After we click in the *Back* button, the RedPage will be popped and we will be back to the root page. 

![Third Page][6]

If we click 3 times the *Push Red Page* button, we will be in a `RedPage` and our navigation stack now has 4 pages.

![Forth Page][7]

And for last, after we click in the *Back to Root* button, all the pages, except the root, will be popped and we will be back again to the root page.

![Fifth Page][8]

### This is it!

My goal with this post was show to you the most used Page types when developing Xamarin.Forms apps and I hope that you have liked it. [Remembering that all the code samples are already in my GitHub account.][9] In the next posts I will continue to talk about Pages more specifically about the TabbedPage and about the CarouselPage. 

I'm addicted to meet new people and to talk about programming so feel free to add me on Facebook or follow me on Twitter. See you there!

[1]: /images/posts/2015-04-12/pages.png
[2]: /images/posts/2015-04-12/loren-ipsum.png
[3]: /images/posts/2015-04-12/rainbow.png
[4]: /images/posts/2015-04-12/first-page.png
[5]: /images/posts/2015-04-12/second-page.png
[6]: /images/posts/2015-04-12/third-page.png
[7]: /images/posts/2015-04-12/forth-page.png
[8]: /images/posts/2015-04-12/fifth-page.png
[9]: https://github.com/pauloortins/MyXamarinSamples
[10]: /2015/04/05/ananomy-of-a-xamarin-forms-app/
