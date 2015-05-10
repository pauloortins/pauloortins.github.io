---
layout: post
title:  "Xamarin.Forms Pages: MasterDetailPage"
date:   2015-05-08
categories: Mobile Xamarin
comments: true
---

Hello buddies! This post closes the Pages section here in the blog, in this last post I will show the MasterDetailPage, samples and, as I always do, a practical application of this kind of page. I hope you like it!

<!--more-->

### MasterDetailPage

As its own name says, the `MasterDetailPage` is used to show a pair of pages where one of these pages is the `Master` page that will work as a navigation and the other page is the `Detail` page that will detail the info selected in the master page.

### MasterDetailPage on Smartphones

On smartphones, by its reduced size, we can't show the `Master` page and the `Detail` page at the same time, in this case, by default, the `Detail` page is shown and when we drag the page laterally from the left corner to the right corner, or when we click in the navigation bar button, the Master page is slided on top of the Detail page, hiding the Detail page partially and becoming completely visible.

The most recent MasterDetailPage sample for those who follow [Xamarin][12] news is the [Xamarin Evolve's][13] app, in this app the `Master` page acts as a menu where we got the following items: Sessions, Speakers, Favorites, Places, About and Sponsors; when a menu item is selected, the `Master` page is hidden and the `Detail` page is loaded with the section content.

![Aplicativo Xamarin Evolve][2]

### MasterDetailPage on Tablets

On tablets, as we have a bigger screen, is possible to show both pages side-by-side and a sample of its use is the Gmail app, where, in the `Master` page we got a list of emails and then, when one of them is selected, it's shown in details in the `Detail` page.

![Aplicativo Gmail][1]

### Places App

In this post we will use the `MasterDetailPage` to create an app called `Places`, the `Places` is an app that lists places accordingly to three possible categories: Food, Shopping and Sports.

To build Places, we will use a MasterDetailPage, where the `Master` page will have a categories menu, and when one of these categories is selected we will show name, address and photo of places that belong to the selected category.

Technically, our solution will be the following:

A `MasterDetailPage`, where the `Master` page will be a [ContentPage][14] with a [ListView][15] that will list the categories and a `Detail` page that will show one of the three [ContentPages][14]  where each one represents a category. Each one of these categories pages will also use a [ListView][15] to show the list of places.

### Creating the Master page: Menu

As we discussed above, the Menu page will be a [ContentPage][14] with a [ListView][15] that will list categories and each category is related to another [ContentPage][14], so let's first create the `Category` class.

The `Category` class is composed by a Name and a function that will return its [ContentPage][14]. 

{% highlight c# %} 
public class Category
{
	public string Name {
		get;
		set;
	}

	public Func<ContentPage> PageFn {
		get;
		set;
	}

	public Category (string name, Func<ContentPage> pageFn)
	{
		Name = name;
		PageFn = pageFn;
	}
}
{% endhighlight %}

Now, we will create a [ListView][15] to list the categories using the `TextCell` template (one of the built-in Xamarin Templates). Furthermore, we will set page's `Title` (required in a `Master` page) and the `Icon` that will be displayed in the navigation bar.

We can see what I'm saying in the code below:

{% highlight c# %}
public class MenuPage : ContentPage
{
	public MenuPage ()
	{
		Title = "Menu";
		Icon = "menu.png";

		Padding = new Thickness (10, 20);

		var categories = new List<Category> () {
			new Category("Food", () => new FoodCategoryPage()),
			new Category("Shopping", () => new ShoppingCategoryPage()),
			new Category("Sports", () => new SportsCategoryPage())
		};

		var dataTemplate = new DataTemplate (typeof(TextCell));
		dataTemplate.SetBinding (TextCell.TextProperty, "Name");

		var listView = new ListView () {
			ItemsSource = categories,
			ItemTemplate = dataTemplate
		};

		Content = listView;
	}
}
{% endhighlight %}

### Creating the MasterDetailPage

The next step is to create the `MasterDetailPage`, the MasterDetailPage has the properties `Master` and the `Detail` of type `Page` that represent the pages that we are already discussing in this post.

In the code below, I'm creating the `MasterDetail` class that inherits from `MasterDetailPage`. As for now, we only got the `MenuPage`, I will initialize the `Master` property with an instance of `MenuPage` and the `Detail` property with a blank [ContentPage][14].

{% highlight c# %}
public class MasterDetail : MasterDetailPage
{
	public MasterDetail ()
	{
		Master = new MenuPage ();

		// Por enquanto vazia
		Detail = new NavigationPage(new ContentPage ());
	}
}
{% endhighlight %}

When running the code above on smartphones we got the following result:

![MasterDetailPage with a Master Page][3]

### Creating the Categories

![Food page][4]

Our lists of places should be equal to the screens above, for that, we will use a [ListView][15] that will list objects of type 'Place' and then, we will use a template to guarantee that our objects will be rendered exactly in the way we want.

The `Place` class has the following properties: `Name`, `Icon` and `Address`.

{% highlight c# %}
public class Place
{
	public string Name {
		get;
		set;
	}

	public string Icon {
		get;
		set;
	}

	public string Address {
		get;
		set;
	}

	public Place (string name, string icon, string address)
	{
		Name = name;
		Icon = icon;
		Address = address;
	}
}
{% endhighlight %}

In this listing we will use a template called `ImageCell`, another built-in template from the Xamarin.Forms API. The `ImageCell` is a template composed by three parts: an icon, a text and a detail. Did you perceive that it's exactly what we need?

In the template config, we will associate the `Name` property from the `Place` class to the `Text` property in the template, the `Icon` to the `ImageSource` and the `Address` to the `Detail` accordingly to the code below.

{% highlight c# %}
var imageTemplate = new DataTemplate (typeof(ImageCell));
imageTemplate.SetBinding (ImageCell.TextProperty, "Name");
imageTemplate.SetBinding (ImageCell.ImageSourceProperty, "Icon");
imageTemplate.SetBinding (ImageCell.DetailProperty, "Address");
{% endhighlight %}

The rest of the code is simple and is repeated for each category.

{% highlight c# %}
public class FoodCategoryPage : ContentPage
{
	public FoodCategoryPage ()
	{
		Title = "Food";

		Padding = new Thickness (10, 20);

		var places = new List<Place> () 
		{
			new Place("McDonalds", "mcdonalds.png", "1010 S McKenzie St Foley, AL"),
			new Place("Burger King", "burgerKing.png", "910 S McKenzie St Foley AL"),
			new Place("Apple Bees", "appleBees.png", "2409 S McKenzie St, Foley, AL"),
			new Place("Taco Bell", "tacoBell.jpg", "1165 S McKenzie St, Foley, AL"),
			new Place("Subway", "subway.jpg", "610 S McKenzie St Foley, AL")
		};

		var imageTemplate = new DataTemplate (typeof(ImageCell));
		imageTemplate.SetBinding (ImageCell.TextProperty, "Name");
		imageTemplate.SetBinding (ImageCell.ImageSourceProperty, "Icon");
		imageTemplate.SetBinding (ImageCell.DetailProperty, "Address");

		var listView = new ListView () 
		{
			ItemsSource = places,
			ItemTemplate = imageTemplate
		};

		Content = listView;
	}
}

public class ShoppingCategoryPage : ContentPage
{
	public ShoppingCategoryPage ()
	{
		Title = "Shopping";

		Padding = new Thickness (10, 20);

		var places = new List<Place> () 
		{
			new Place("Walmart", "walmart.jpeg", "2200 S McKenzie St, Foley"),
			new Place("Tanger Outlet Center", "tanger.jpg", "2601 S McKenzie St, Ste 466, Foley, AL"),
			new Place("Radio Shack", "radioShack.jpg", "1190 S Mckensie, Foley, AL")
		};

		var imageTemplate = new DataTemplate (typeof(ImageCell));
		imageTemplate.SetBinding (ImageCell.TextProperty, "Name");
		imageTemplate.SetBinding (ImageCell.ImageSourceProperty, "Icon");
		imageTemplate.SetBinding (ImageCell.DetailProperty, "Address");

		var listView = new ListView () 
		{
			ItemsSource = places,
			ItemTemplate = imageTemplate
		};

		Content = listView;
	}
}

public class SportsCategoryPage : ContentPage
{
	public SportsCategoryPage ()
	{
		Title = "Sports";

		Padding = new Thickness (10, 20);

		var places = new List<Place> () 
		{
			new Place("The Gulf Bowl", "gulfBowl.jpg", "2881 S Juniper St, Foley, AL"),
			new Place("Foley Sports Complex", "foleySportsComplex.jpg", "998 W Section Ave, Foley, AL"),
			new Place("Swatters Sports Complex", "swattersSportsComplex.jpg", "21431 Co Rd 12 S Foley, AL")
		};

		var imageTemplate = new DataTemplate (typeof(ImageCell));
		imageTemplate.SetBinding (ImageCell.TextProperty, "Name");
		imageTemplate.SetBinding (ImageCell.ImageSourceProperty, "Icon");
		imageTemplate.SetBinding (ImageCell.DetailProperty, "Address");

		var listView = new ListView () 
		{
			ItemsSource = places,
			ItemTemplate = imageTemplate
		};

		Content = listView;
	}
}
{% endhighlight %}

When executing the code above on an iPhone we got the following screens:

![iPhone Categories][10]

### Linking the Categories to the Detail Page

To link the categories to the `Detail` page we need to do some code changes in our app.

The first is to pick one of the pages to be the default page (the app can't start with a blank page, right?), lets set the `FoodCategoryPage` as our default page.

{% highlight c# %}
public class MasterDetail : MasterDetailPage
{
    public MasterDetail ()
    {
	    Master = new MenuPage ();
	    Detail = new NavigationPage(new FoodCategoryPage ());
    }
}
{% endhighlight %}

The second change is to change the `MenuPage's` code to when we select a category in the [ListView][15], the Detail page be changed to show the places that belong to the selected category. For that, we will use the `ItemSelected` event that when triggered will call the `OnMenuSelect` action that will be configured by the MenuPage's caller.

The MenuPage's code is the following:

{% highlight c# %}
public class MenuPage : ContentPage
{
	public Action<ContentPage> OnMenuSelect {
		get;
		set;
	}

	public MenuPage ()
	{
		Title = "Menu";
		Icon = "menu.png";

		Padding = new Thickness (10, 20);

		var categories = new List<Category> () {
			new Category("Food", () => new FoodCategoryPage()),
			new Category("Shopping", () => new ShoppingCategoryPage()),
			new Category("Sports", () => new SportsCategoryPage())
		};

		var dataTemplate = new DataTemplate (typeof(TextCell));
		dataTemplate.SetBinding (TextCell.TextProperty, "Name");

		var listView = new ListView () {
			ItemsSource = categories,
			ItemTemplate = dataTemplate
		};

		listView.ItemSelected += (object sender, SelectedItemChangedEventArgs e) => {
			if (OnMenuSelect != null)
			{
				var category = (Category) e.SelectedItem;
				var categoryPage = category.PageFn();
				OnMenuSelect(categoryPage);
			}				
		};


		Content = listView;
	}
}
{% endhighlight %}

And for last, the `MasterDetail` page needs to configure the MenuPage's OnMenuSelect action to, when a new category is selected by the user, we set a new page to the `Detail` page and hide the `Master` page.

{% highlight c# %}
menuPage.OnMenuSelect = (categoryPage) => {
	Detail = new NavigationPage(categoryPage);
	IsPresented = false;
};
{% endhighlight %}

Below is the MasterDetail's code:

{% highlight c# %}
public class MasterDetail : MasterDetailPage
{
	public MasterDetail ()
	{
		var menuPage = new MenuPage ();
		menuPage.OnMenuSelect = (categoryPage) => {
			Detail = new NavigationPage(categoryPage);
			IsPresented = false;
		};

		Master = menuPage;

		Detail = new NavigationPage(new FoodCategoryPage ());
	}
}
{% endhighlight %}

### Tablets: Bigger screens, more possibilities

On tablets we have bigger screens and it opens more possibilities when using a `MasterDetailPage`, using the `MasterBehavior` enumeration we can change the Master page's behavior.

{% highlight c# %}
namespace Xamarin.Forms
{
	public enum MasterBehavior
	{
		Default,
		SplitOnLandscape,
		Split,
		Popover,
		SplitOnPortrait
	}
}
{% endhighlight %}

{% highlight c# %}
MasterBehavior = MasterBehavior.Default;
{% endhighlight %}

Using MasterBehavior as `MasterBehavior.Default`, in portrait mode the Master page will start hidden and will be always visible in the landscape.

![MasterBehavior.Default][5]

Using MasterBehavior as `MasterBehavior.Popover`, the Master page will start hidden in both modes (Portrait and Landscape).

![MasterBehavior.Popover][6]

Using MasterBehavior as `MasterBehavior.Split`, the Master page will be always visible in both modes (Portraid and Landscape).

![MasterBehavior.Split][7]

Using MasterBehavior as `MasterBehavior.SplitOnLandscape`, the Master page will be always visible in the landscape mode and will start hidden in the portrait. 

![MasterBehavior.SplitOnLandscape][8]

And, for last, using MasterBehavior as `MasterBehavior.SplitOnPortrait`, the Master page will be always visible in the portrait mode and will start hidden in the landscape. 

![MasterBehavior.SplitOnPortrait][9]

### This is it!

I really liked this post! It was very complete about the `MasterDetailPage`'s use and, as an extra, we also have a functional menu that we can use in our apps. Together with the post about [ContentPage and NavigationPage][15] and the post about [TabbedPage and CarouselPage][16] this post closes the Pages section here in the blog. Starting next post we will talk about the kinds of `Layouts!` 

See you around!

[The code used in this post is available in my github.][11]

[1]: /images/posts/2015-05-08/gmail-ipad.jpg 
[2]: /images/posts/2015-05-08/xamarin-evolve.png 
[3]: /images/posts/2015-05-08/master.png 
[4]: /images/posts/2015-05-08/food.png 
[5]: /images/posts/2015-05-08/master-behavior-default.png 
[6]: /images/posts/2015-05-08/master-behavior-popover.png 
[7]: /images/posts/2015-05-08/master-behavior-split.png 
[8]: /images/posts/2015-05-08/master-behavior-split-landscape.png 
[9]: /images/posts/2015-05-08/master-behavior-split-portrait.png 
[10]: /images/posts/2015-05-08/categories.png 
[11]: https://github.com/pauloortins/MyXamarinSamples/tree/master/Places
[12]: http://xamarin.com/
[13]: https://evolve.xamarin.com/
[14]: /2015/04/12/xamarin-forms-pages-contentpage-navigationpage/
[15]: /2015/05/05/xamarin-forms-controls-listview/ 
[16]: /2015/04/18/xamarin-forms-pages-tabbedpage-carouselpage/

