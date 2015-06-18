---
layout: post
title:  "Xamarin.Forms Layouts: StackLayout"
date:   2015-06-17
categories: Mobile Xamarin
comments: true
---

Hi guys, how you doing? After a short pause, I come back to this blog to talk about Xamarin.Forms Layouts and the first Layout that I will write about is the StackLayout.

<!--more-->

### Layouts

![Layouts][5]

Layouts in Xamarin.Forms are used to organize our controls in the Pages, e.g.: organize fields in a form, organize how lists will be displayed, to create a bottom-fixed button and so on.

There are 7 types of Layouts in the Xamarin.Forms API, they are: the `StackLayout`, the `AbsoluteLayout`, the `RelativeLayout`, the `Grid`, the `ContentView`, the `ScrollView` and the `Frame`. This post is about the StackLayout.

### StackLayout

The `StackLayout` is the simplest form to organize controls in Xamarin.Forms, in this control, as its name says, we organize controls sequentially using a stack. To set the content of a StackLayout we use the `Children` property, in the code below we use a StackLayout to show, vertically, a set of letters in a [ContentPage][6].

{% highlight c# %}
public class VerticalLetters : ContentPage
{
	public VerticalLetters ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0));
		Content = new StackLayout () {					
			Children = {
				new Label() {Text="A", FontSize = 30},
				new Label() {Text="B", FontSize = 30},
				new Label() {Text="C", FontSize = 30},
				new Label() {Text="D", FontSize = 30},
				new Label() {Text="E", FontSize = 30}
			}
		};
	}
}  
{% endhighlight %}

When running the code above, we can see, in both platforms, a vertical StackLayout.

![Vertical StackLayout][1]

### Orientation

StackLayout has two orientation types, by default, StackLayout shows its children vertically, however we can use the `Orientation` property to change the way children are displayed. In the code below we set the orientation to Horizontal and now we see the letters being shown horizontally.

{% highlight c# %}
public class HorizontalLetters : ContentPage
{
	public HorizontalLetters ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0));
		Content = new StackLayout () {					
			Orientation = StackOrientation.Horizontal,
			Children = {
				new Label() {Text="A", FontSize = 30},
				new Label() {Text="B", FontSize = 30},
				new Label() {Text="C", FontSize = 30},
				new Label() {Text="D", FontSize = 30},
				new Label() {Text="E", FontSize = 30}
			}
		};
	}
}
{% endhighlight %}

When running the code above, we can see, in both platforms, a horizontal StackLayout.

![Horizontal StackLayout][2]

### Spacing

Another useful property is the `Spacing` that allows to define the spacing between each child in the Layout, by default, this value is set to `6`.

if we set this value to `50`, we can see the same letters but now with a bigger distance between them.

{% highlight c# %}
public class VerticalLetters : ContentPage
{
	public VerticalLetters ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0));
		Content = new StackLayout () {	
			Spacing = 50,
			Children = {
				new Label() {Text="A", FontSize = 30},
				new Label() {Text="B", FontSize = 30},
				new Label() {Text="C", FontSize = 30},
				new Label() {Text="D", FontSize = 30},
				new Label() {Text="E", FontSize = 30}
			}
		};
	}
}
{% endhighlight %}

![StackLayout with Spacing][3]

### Creating a login form

![StackLayout Form][4]

The most common scenario is to combine different layouts to assembly a UI, in this sample, we will use some StackLayouts to create a login form accordingly with the image above.

To create the form, we will use 3 StackLayouts, one to create the label + entry for the User field, another one to the label + entry for the password and, for last, another StackLayout to combine both and to add a Login button.

We can see it in the code below. 

{% highlight c# %}
public class LoginForm : ContentPage
{
	public LoginForm ()
	{
		Padding = new Thickness (5, Device.OnPlatform (20, 0, 0), 5, 0);
		
		var userLabel = new Label () {
			Text = "User:", 
			WidthRequest = 100
		};
		var userEntry = new Entry () {
			HorizontalOptions = LayoutOptions.FillAndExpand
		};
		var userStack = new StackLayout () {
			Orientation = StackOrientation.Horizontal,
			Children = {userLabel, userEntry}
		};

		var passwordLabel = new Label () {
			Text = "Password:", 
			WidthRequest = 100
		};
		var passwordEntry = new Entry () {
			HorizontalOptions = LayoutOptions.FillAndExpand, 
			IsPassword = true 
		};
		var passwordStack = new StackLayout () {
			Orientation = StackOrientation.Horizontal,
			Children = {passwordLabel, passwordEntry}
		};

		var loginButton = new Button () {Text = "Login"};

		var formStack = new StackLayout () {
			Children = {
				userStack,
				passwordStack,
				loginButton
			}
		};

		Content = formStack;
	}
}
{% endhighlight %}

### That's it!

That's it guys, in this post we met the `StackLayout`, it's one of the simplest layouts in Xamarin.Forms but extremely useful when we need to position UI elements sequencially.

There a lot more to come! We still have 6 layouts to cover.

[1]: /images/posts/2015-06-17/stacklayout.png 
[2]: /images/posts/2015-06-17/horizontal-layout.png 
[3]: /images/posts/2015-06-17/stacklayout-with-spacing.png
[4]: /images/posts/2015-06-17/login-form.png
[5]: /images/posts/2015-06-17/layouts.png
[6]: /2015/04/12/xamarin-forms-pages-contentpage-navigationpage/
[7]: /2015/04/04/xamarin-forms/
