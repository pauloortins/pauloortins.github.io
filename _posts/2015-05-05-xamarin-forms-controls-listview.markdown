---
layout: post
title:  "Xamarin.Forms Controls: ListView"
date:   2015-05-05
categories: Mobile Xamarin
comments: true
---

In this post we will talk about the ListView, a control used to show data in a list format.

<!--more-->

### ListView

When developing mobile apps we always have the need to show data in a list format, these data can be people, phone numbers, messages, restaurants, questions or anything else. The Xamarin.Forms has a specific control for this need, is the ListView and we'll deepen our knowledge about he in this post.

### Creating the first ListView

To create a ListView we don't need nothing more than a `ItemsSource`, that are the data that will be shown in the list. The code below creates a `ListView` to show a list of cities.

{% highlight c# %}
public class TextListViewPage : ContentPage
{
	public TextListViewPage ()
	{
		var cities = new List<string> () {
			"Miami",
			"San Francisco",
			"Los Angeles",
			"Orlando",
			"Houston",
			"New York City"
		};

		var listView = new ListView () {
			ItemsSource = cities
		};

		Content = listView;
	}
}

{% endhighlight %}

When executing the code above in Android as well as in iOS we got the following screens:

![ListView][1]

### Listing complex objects

ListViews are not only limited to strings, as the property `ItemsSource` is an `IEnumerable`, we can also pass complex objects to it.

The code below shows an example using a `City` class.

{% highlight c# %}
public class ComplexObjectListViewPage : ContentPage
{
	class City
	{
		public string Name {
			get;
			set;
		}

		public string State {
			get;
			set;
		}
	}

	public ComplexObjectListViewPage ()
	{			
		var cities = new List<City> () {
			new City() {State = "FL", Name = "Miami"},
			new City() {State = "CA", Name = "San Francisco"},
			new City() {State = "CA", Name = "Los Angeles"},
			new City() {State = "FL", Name = "Orlando"},
			new City() {State = "TX", Name = "Houston"},
			new City() {State = "NY", Name = "New York City"},
		};

		var listView = new ListView () {
			ItemsSource = cities
		};

		Content = listView;
	}
}
{% endhighlight %}

When running the code above, we will perceive that the result wasn't the expected, instead to show the name of each city, the list is showing the class fullname. Why is it happening? By default, when there isn't a template, the ListView executes the `ToString()` method in each object and due to it the class fullname is being shown.

To achieve the same result that we had before (a list of cities), we need to use a `DataTemplate` that will stablish a template to render each object in the list.

![Rendering complex objects using a ListView][2]

### Using a DataTemplate to render a ListView

Xamarin.Forms comes with some built-in templates:

- [TextCell][7]
- [ImageCell][8]
- [SwitchCell][9]
- [EntryCell][10]

In this example we will use the `TextCell`, a simple text template, to show the name of each city. First, we need to instantiate a `DataTemplate` class of type `TextCell`, and after, bind the template text to the property `Name` of the `City` class.

{% highlight c# %}
var dataTemplate = new DataTemplate (typeof(TextCell));
dataTemplate.SetBinding (TextCell.TextProperty, "Name");
{% endhighlight %}

The final code, in this example, is the following:

{% highlight c# %}
public class ComplexObjectListViewPage : ContentPage
{
	class City
	{
		public string Name {
			get;
			set;
		}

		public string State {
			get;
			set;
		}
	}

	public ComplexObjectListViewPage ()
	{			
		var cities = new List<City> () {
			new City() {State = "FL", Name = "Miami"},
			new City() {State = "CA", Name = "San Francisco"},
			new City() {State = "CA", Name = "Los Angeles"},
			new City() {State = "FL", Name = "Orlando"},
			new City() {State = "TX", Name = "Houston"},
			new City() {State = "NY", Name = "New York City"},
		};

		var dataTemplate = new DataTemplate (typeof(TextCell));
		dataTemplate.SetBinding (TextCell.TextProperty, "Name");

		var listView = new ListView () {
			ItemsSource = cities,
			ItemTemplate = dataTemplate
		};

		Content = listView;
	}
}
{% endhighlight %}

Now, when running the code above, we got the expected result.

![Listing complex objects using TextCell][3]

### Using Custom Templates

Several times only the templates already provided by [Xamarin][12] won't be enough to our mobile apps, in these cases we will have to build our own custom templates. In this example I will show how to create a custom template to list the state and the name of each city.

To create a custom template we should, first, create a class that inherits from `ViewCell`, second, define how our new template will be rendered and, for last, how it will use the data from each item in the list. 

In the code below we create a custom template called `CustomCell` that will have 4 Labels, where two of them will be static ("State:" and "Name:") while the others will be dynamic and bound to the `State` and `Name` properties of the `City` class.

{% highlight c# %}
class CustomCell : ViewCell
{
	public CustomCell()
	{
		var stateLabel = new Label () 
		{
			Text = "State:"
		};

		var stateText = new Label ();
		stateText.SetBinding (Label.TextProperty, "State");

		var cityLabel = new Label () 
		{
			Text = "City:"	
		};

		var cityText = new Label ();
		cityText.SetBinding (Label.TextProperty, "Name");

		var view = new StackLayout () {
			Orientation = StackOrientation.Horizontal,
			Children = 
			{
				stateLabel,
				stateText,
				cityLabel,
				cityText
			}
		};

		View = view;
	}
}
{% endhighlight %}

Now, we need to associate the new template to a `DataTemplate` and then associate it to the `ListView`.

{% highlight c# %}
var dataTemplate = new DataTemplate (typeof(CustomCell));

var listView = new ListView () {
	ItemsSource = cities,
	ItemTemplate = dataTemplate
};
{% endhighlight %}

The complete code, in this example, is the following:

{% highlight c# %}
public class CustomCellListViewPage : ContentPage
{
	class CustomCell : ViewCell
	{
		public CustomCell()
		{
			var stateLabel = new Label () 
			{
				Text = "State:"
			};

			var stateText = new Label ();
			stateText.SetBinding (Label.TextProperty, "State");

			var cityLabel = new Label () 
			{
				Text = "City:"	
			};

			var cityText = new Label ();
			cityText.SetBinding (Label.TextProperty, "Name");

			var view = new StackLayout () {
				Orientation = StackOrientation.Horizontal,
				Children = 
				{
					stateLabel,
					stateText,
					cityLabel,
					cityText
				}
			};

			View = view;
		}
	}

	class City
	{
		public string Name {
			get;
			set;
		}

		public string State {
			get;
			set;
		}
	}

	public CustomCellListViewPage ()
	{
		Padding = new Thickness (20);

		var cities = new List<City> () {
			new City() {State = "FL", Name = "Miami"},
			new City() {State = "CA", Name = "San Francisco"},
			new City() {State = "CA", Name = "Los Angeles"},
			new City() {State = "FL", Name = "Orlando"},
			new City() {State = "TX", Name = "Houston"},
			new City() {State = "NY", Name = "New York City"},
		};

		var dataTemplate = new DataTemplate (typeof(CustomCell));

		var listView = new ListView () {
			ItemsSource = cities,
			ItemTemplate = dataTemplate
		};

		Content = listView;
	}
}
{% endhighlight %}

When running the code above, we got a list where the state and name of each city are shown.

![ListView Custom Template][4]

### Grouping Lists

A ListView also supports grouping using sections and lateral jumplists to rapid navigation. In the example I will show the same cities but this time grouped by states.

To group the cities we will have to change a little bit the way we are creating our list of cities, instead to create an unique list with all the cities, we will create a list of states where each of these states will have a name and a collection of cities, in practice, we will have a list of lists. Note that the `State` class inherits from a collection, after all, it's a list of cities.

{% highlight c# %}
class State : ObservableCollection<City>
{
	public State(string name)
	{
		Name = name;
	}

	public string Name {
		get;
		set;
	}
}

class City
{
	public string Name {
		get;
		set;
	}
}
{% endhighlight %}

We also need to change the ListView's initialization to set its grouping, for that, we need to set the `IsGroupingEnabled`, `GroupDisplayBinding` and `GroupShortNameBinding` properties.

{% highlight c# %}
var listView = new ListView () {
	IsGroupingEnabled = true,
	GroupDisplayBinding = new Binding("Name"),
	GroupShortNameBinding = new Binding("Name"),
	ItemsSource = states,
	ItemTemplate = dataTemplate
};

{% endhighlight %}

The `IsGroupingEnabled` property sets if a ListView is grouped or not, the `GroupDisplayBinding` property sets which text will be shown in the section header and, for last, the `GroupShortNameBinding` property sets which text will be shown in the lateral jumplist.

Below is the complete code:

{% highlight c# %}
public class GroupingListViewPage : ContentPage
{		
	class State : ObservableCollection<City>
	{
		public State(string name)
		{
			Name = name;
		}

		public string Name {
			get;
			set;
		}
	}

	class City
	{
		public string Name {
			get;
			set;
		}
	}

	public GroupingListViewPage ()
	{
		Padding = new Thickness (20);

		var states = new List<State> () {
			new State("FL") 
			{
				new City() {Name = "Miami"},
				new City() {Name = "Orlando"},
			},
			new State("CA") 
			{					
				new City() {Name = "Los Angeles"},
				new City() {Name = "San Francisco"},
			},
			new State("TX") 
			{					
				new City() {Name = "Houston"},
				new City() {Name = "Austin"},
			},
			new State("NY") 
			{					
				new City() {Name = "New York City"},
			}
		};

		var dataTemplate = new DataTemplate (typeof(TextCell));
		dataTemplate.SetBinding (TextCell.TextProperty, "Name");

		var listView = new ListView () {
			IsGroupingEnabled = true,
			GroupDisplayBinding = new Binding("Name"),
			GroupShortNameBinding = new Binding("Name"),
			ItemsSource = states,
			ItemTemplate = dataTemplate
		};

		Content = listView;
	}
}
{% endhighlight %}

When running the code above we got the same list of cities but now grouped by states.

![Grouped ListView][5]

### Customizing the Section Headers

Just as we do with list items, we can also customize the section headers using custom templates. In this example we will customize the section headers to display, in addition to the state name, also an icon that identifies each city, for that, we will create a custom template called `StateTemplate`.

In this template, we will have a Label bound to the `Name` property and an Image where its `Source` property is bound to the `Icon` property of the `State` class.

I know, at this time, you are already an expert in custom templates =D but check out how the code looks.

{% highlight c# %}
class StateTemplate : ViewCell
{
	public StateTemplate()
	{
		var text = new Label();
		text.SetBinding(Label.TextProperty, "Name");

		var icon = new Image() {
			WidthRequest = 25,
			HeightRequest = 25
		};
		icon.SetBinding(Image.SourceProperty, "Icon");

		var view = new StackLayout() {
			Orientation = StackOrientation.Horizontal,
			Children = {
				icon,
				text
			}
		};

		Height = 30;
		View = view;
	}
}
{% endhighlight %}

We also need to change the ListView's initialization to set the `GroupHeaderTemplate` to the template that we created.

{% highlight c# %}
var listView = new ListView () {
	IsGroupingEnabled = true,
	GroupDisplayBinding = new Binding("Name"),
	GroupShortNameBinding = new Binding("Name"),
	ItemsSource = states,
	ItemTemplate = dataTemplate,
	GroupHeaderTemplate = new DataTemplate(typeof(StateTemplate)),
	HasUnevenRows = true
};
{% endhighlight %}

Below is the complete code:

{% highlight c# %}
public class GroupingTemplateListViewPage : ContentPage
{		
	class State : ObservableCollection<City>
	{
		public State(string name, string icon)
		{
			Name = name;
			Icon = icon;
		}

		public string Name {
			get;
			set;
		}

		public string Icon {
			get;
			set;
		}
	}

	class StateTemplate : ViewCell
	{
		public StateTemplate()
		{
			var text = new Label();
			text.SetBinding(Label.TextProperty, "Name");

			var icon = new Image() {
				WidthRequest = 25,
				HeightRequest = 25
			};
			icon.SetBinding(Image.SourceProperty, "Icon");

			var view = new StackLayout() {
				Orientation = StackOrientation.Horizontal,
				Children = {
					icon,
					text
				}
			};

			Height = 30;
			View = view;
		}
	}

	class City
	{
		public string Name {
			get;
			set;
		}
	}			

	public GroupingTemplateListViewPage ()
	{
		Padding = new Thickness (20);

		var states = new List<State> () {
			new State("FL", "fl.jpg") 
			{
				new City() {Name = "Miami"},
				new City() {Name = "Orlando"},
			},
			new State("CA", "ca.jpeg") 
			{					
				new City() {Name = "Los Angeles"},
				new City() {Name = "San Francisco"},
			},
			new State("TX", "tx.jpg") 
			{					
				new City() {Name = "Houston"},
				new City() {Name = "Austin"},
			},
			new State("NY", "ny.jpg") 
			{					
				new City() {Name = "New York City"},
			}
		};

		var dataTemplate = new DataTemplate (typeof(TextCell));
		dataTemplate.SetBinding (TextCell.TextProperty, "Name");

		var listView = new ListView () {
			IsGroupingEnabled = true,
			GroupDisplayBinding = new Binding("Name"),
			GroupShortNameBinding = new Binding("Name"),
			ItemsSource = states,
			ItemTemplate = dataTemplate,
			GroupHeaderTemplate = new DataTemplate(typeof(StateTemplate)),
			HasUnevenRows = true
		};

		Content = listView;
	}
}
{% endhighlight %}

When running the code above, our section headers now have a image followed by a text.
Ao executar o código acima, os cabeçalhos dos nossos grupos agora possuem imagem e texto.

![ListView agrupado por estados com cabeçalho customizado][6]

### Using ListView's events

In the end, only listing isn't enough for mobile apps, we need interaction, we need to be notified when an item is tapped or selected and then take some action e.g.: display a message or go to another page. ListView has two events that can help us in these scenarios, the `ItemTapped` and the `ItemSelected`.

The `ItemTapped` event is triggered everytime we tap on one of the items.
{% highlight c# %}
listView.ItemTapped += (object sender, ItemTappedEventArgs e) => {
	DisplayAlert("ItemTapped", e.Item.ToString(), "Ok");
};
{% endhighlight %}

While the `ItemSelected` is triggered everytime the `SelectedItem` property changes, so, this event is triggered everytime we tap on an unselected item as well as when we change the SelectedItem programmatically. 

{% highlight c# %}
listView.ItemSelected += (object sender, SelectedItemChangedEventArgs e) => {
	DisplayAlert("ItemSelected", e.SelectedItem.ToString(), "Ok");
};

// The ItemSelected is also triggered here.
listView.SelectedItem = object;
{% endhighlight %}

### That's it!

This post was long but it's justified since the ListView has a lot of possible configurations. Now you are able to use ListViews with any configuration in your mobile apps! 

If you have any doubt, suggestion or only want to say a Hello World, leave a comment below :D

[These samples are already in my github account.][11]

See you around!

[1]: /images/posts/2015-05-05/list-view-text.png 
[2]: /images/posts/2015-05-05/complex-object-text.png 
[3]: /images/posts/2015-05-05/complex-object-template.png 
[4]: /images/posts/2015-05-05/custom-template.png 
[5]: /images/posts/2015-05-05/grouping-list.png 
[6]: /images/posts/2015-05-05/grouping-template.png 
[7]: http://iosapi.xamarin.com/index.aspx?link=T%3AXamarin.Forms.TextCell
[8]: http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.ImageCell
[9]: http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.SwitchCell
[10]: http://iosapi.xamarin.com/?link=T%3aXamarin.Forms.EntryCell
[11]: https://github.com/pauloortins/MyXamarinSamples/tree/master/ListViewSample
[12]: www.xamarin.com



