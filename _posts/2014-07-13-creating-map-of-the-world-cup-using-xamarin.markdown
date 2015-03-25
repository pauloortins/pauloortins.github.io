---
layout: post
title:  "Creating a map of the World Cup using Xamarin.iOS"
date:   2014-07-13
categories: Mobile Xamarin
comments: true
---

It's World Cup time in Brazil and is impossible to programming without at least split the attention between the PC monitor and the matches on the TV. But the matches there aren't the only thing that have excited me in the last weeks, actually, use C# to develop the web and mobile applications through Xamarin has excited me even more and the experience have been very interesting. 

In the last days I thought: "Why don't join these two things and write a blog post showing how we can use Xamarin features to visualize information about the World Cup?".

<!--more-->

In this post, I pretend to show a step by step guide about how we can create maps with Xamarin.iOS and use them to show the location of the countries and stadiums involved in the World Cup 2014. We will do it using baby steps in the following order.

- Create a Xamarin application
- Create a map
- Create points in the map
- Customize appearance of the points 

<br/>

## Creating a Xamarin application

The first step is to create a Xamarin application called `WorldCupMap`. For that, access File > New > Solution and create a project with the following config.

![Creating a Xamarin Application][1]

### Analyzing the project structure

Xamarin automatically creates a structure with some files. Let's talk about some of them.

![Project Structure][2]

The `AppDelega.cs` has a function similar to the global.asax that we use in ASP.NET applications, he is the responsible to listen and answer to application events like the DidEnterBackground event (triggered when the app goes to background) and the WillEnterForeground (triggered when the app will become active).

The `Entitlements.plist` stores the information about the use of some of Apple services like iCloud and PassBook. 

The `info.plist` is the main configuration file in a iOS application. In this file we store the app name and its identifier, the devices that the app supports (ipad/iphone/ipod), the orientations, icons and the services that can run in background like location and/or audio.

The `MainStoryboard.storyboard` is the visual representation of the views and the flow between them, the manipulation of these files is similar to the traditional drag and drop that we use in WindowsForms/WebForms. In these files we can create views using controls like labels, textfields, tables and so on.

Controllers like `WorldCupMapViewController.cs`, \ are responsible to control the interaction between views and models, in them, we put the code that do event handling, view initialization and so on.

#### If Everything is Right

If everything is right, when we run the app we will see a "beautiful" white screen.

## Creating a Map

The use of maps, nowadays, is a common feature in the apps found in AppleStore. The Mapkit (framework that handles with maps in iOS) allow us to  add maps to our apps easily. We can add a map in iOS app with these few lines.

Let's open the classe `WorldCupMapViewController.cs` and replace the content with the following code.

{% highlight c# %}
public partial class WorldCupMapViewController : UIViewController
{
	protected MKMapView map;
	public WorldCupMapViewController (IntPtr handle) : base (handle)
	{
	}

	public override void LoadView ()
	{
		map = new MKMapView (UIScreen.MainScreen.Bounds);
		View = map;
	}
}
{% endhighlight %}

### Running the App

When running the app, our app already have a map that supports all the interactions like panning and zooming.

![Mapa][3]

## Creating the Points

The next step is to create the points in the map. The MapKit offers support for markers addition through the annotations. To create a annotation we should set its title and geographic coordinates, after it, a red pin will be shown in the map, however, we can also customize it to show pins in different colors or replace them with other images.

### Creating a Data Class

To represent the information we will create a class called `WorldCupInfo` that will store the title (country/stadium name), latitude and longitude. The code is below.

{% highlight c# %}
public class WorldCupInfo
{
	public WorldCupInfo (string title, double latitude, double longitude)
	{
		Title = title;
		Latitude = latitude;
		Longitude = longitude;
	}

	public string Title {
		get;
		set;
	}

	public double Latitude {
		get;
		set;
	}

	public double Longitude {
		get;
		set;
	}
}
{% endhighlight %}

### Creating the Information List

After we create the WorldCupInfo class, we need to create the a list with all countries and stadiums that are part of World Cup. For that, we will create a attribute representing this list in the `WorldCupMapViewController.cs` controller and we also need to create a method that will initialize the information list.

{% highlight c# %}
protected MKMapView map;
private List<WorldCupInfo> _infoList;

public WorldCupMapViewController (IntPtr handle) : base (handle)
{
	_infoList = GetInfoList ();
}

public List<WorldCupInfo> GetInfoList ()
{
	var infoList = new List<WorldCupInfo> () {
		new WorldCupInfo("Algeria", 28.667122,2.677408),
		new WorldCupInfo("Argentina", -35.003106,-64.741499),
		new WorldCupInfo("Australia", -24.734834,134.248120),
		new WorldCupInfo("Belgium", 50.503887,4.469936),
		new WorldCupInfo("Bosnia and Herzegovina", 42.5564516,15.7223665),
		new WorldCupInfo("Brazil", -14.235004,-51.92528),
		new WorldCupInfo("Cameroon", 7.369721999999999,12.354722),
		new WorldCupInfo("Chile", -35.675147,-71.542969),
		new WorldCupInfo("Colombia", 4.570868,-74.29733299999999),
		new WorldCupInfo("Costa Rica", 9.748916999999999,-83.753428),
		new WorldCupInfo("Côte d'Ivoire", 7.539988999999999,-5.547079999999999),
		new WorldCupInfo("Croatia", 45.1,15.2),
		new WorldCupInfo("Ecuador", -1.831239,-78.18340599999999),
		new WorldCupInfo("England", 52.3555177,-1.1743197),
		new WorldCupInfo("France", 46.227638,2.213749),
		new WorldCupInfo("Germany", 51.165691,10.451526),
		new WorldCupInfo("Ghana", 7.946527,-1.023194),
		new WorldCupInfo("Greece", 39.074208,21.824312),
		new WorldCupInfo("Honduras", 15.199999,-86.241905),
		new WorldCupInfo("Iran", 32.427908,53.688046),
		new WorldCupInfo("Italy", 41.87194,12.56738),
		new WorldCupInfo("Japan", 36.204824,138.252924),
		new WorldCupInfo("Korea Republic", 35.907757,127.766922),
		new WorldCupInfo("Mexico", 23.634501,-102.552784),
		new WorldCupInfo("Netherlands", 52.132633,5.291265999999999),
		new WorldCupInfo("Nigeria", 9.081999,8.675276999999999),
		new WorldCupInfo("Portugal", 39.39987199999999,-8.224454),
		new WorldCupInfo("Russia", 61.52401,105.318756),
		new WorldCupInfo("Spain", 40.46366700000001,-3.74922),
		new WorldCupInfo("Switzerland", 46.818188,8.227511999999999),
		new WorldCupInfo("Uruguay", -32.522779,-55.765835),
		new WorldCupInfo("USA", 37.09024,-95.712891),
		new WorldCupInfo("Arena Fonte Nova", -12.97883,-38.5043711),
		new WorldCupInfo("Estádio das Dunas", -5.8268266,-35.2124299),
		new WorldCupInfo("Estádio Castelão", -3.8072311,-38.5224338),
		new WorldCupInfo("Arena Pernambuco", -8.0406496,-35.0082049),
		new WorldCupInfo("Arena Amazônia", -3.083142,-60.02810849999999),
		new WorldCupInfo("Estádio Nacional", -15.7835191,-47.899211),
		new WorldCupInfo("Arena Pantanal", -15.604017,-56.1216325),
		new WorldCupInfo("Arena Corinthians", -23.5453331,-46.4737017),
		new WorldCupInfo("Estádio do Maracanã", -22.9121089,-43.2301558),
		new WorldCupInfo("Estádio Mineirão", -19.865867,-43.97113150000001),
		new WorldCupInfo("Arena da Baixada", -25.4482116,-49.2769866),
		new WorldCupInfo("Estádio Beira-Rio", -30.065066,-51.2365144)
	};				

	return infoList;																																																	
}
{% endhighlight %}

### Creating the Annotations

For last, we create the annotations after the map initialization.

{% highlight c# %}
public override void LoadView ()
{
	map = new MKMapView (UIScreen.MainScreen.Bounds);
	View = map;

	_infoList.ForEach (x => map.AddAnnotation (new MKPointAnnotation () {
		Title = x.Title,
		Coordinate = new CLLocationCoordinate2D () {
			Latitude = x.Latitude,
			Longitude = x.Longitude
		}
	}));
}
{% endhighlight %}

### Running the App

If everything is right until here, when running the app, we will see a map with all countries and stadiums represented by red pins.

![Mapa with pins][4]

## Adding Custom Annotations

To customize the annotations, we need to, first, add a property to represent the image name in `WorldCupInfo`, and after, create a method that will be used to show the new images.

### Adding the Image Property

First, lets add the propery Image in WorldCupInfo.

{% highlight c# %}
public class WorldCupInfo
{
	public WorldCupInfo (string title, double latitude,
			double longitude, string image)
	{
		Title = title;
		Latitude = latitude;
		Longitude = longitude;
		Image = image;
	}

	public string Title {
		get;
		set;
	}

	public double Latitude {
		get;
		set;
	}

	public double Longitude {
		get;
		set;
	}

	public string Image {
		get;
		set;
	}
}
{% endhighlight %}

And now, add the new property in the list initialization.

{% highlight c# %}
public List<WorldCupInfo> GetInfoList ()
{
	var infoList = new List<WorldCupInfo> () {
		new WorldCupInfo("Algeria", 28.667122,2.677408, "algeria.png" ),
		new WorldCupInfo("Argentina", -35.003106,-64.741499, "argentina.png" ),
		new WorldCupInfo("Australia", -24.734834,134.248120, "australia.png" ),
		new WorldCupInfo("Belgium", 50.503887,4.469936, "belgium.png" ),
		new WorldCupInfo("Bosnia and Herzegovina", 42.5564516,15.7223665, "bosnia.png" ),
		new WorldCupInfo("Brazil", -14.235004,-51.92528, "brazil.png" ),
		new WorldCupInfo("Cameroon", 7.369721999999999,12.354722, "cameroon.png" ),
		new WorldCupInfo("Chile", -35.675147,-71.542969, "chile.png" ),
		new WorldCupInfo("Colombia", 4.570868,-74.29733299999999, "colombia.png" ),
		new WorldCupInfo("Costa Rica", 9.748916999999999,-83.753428, "costa-rica.png" ),
		new WorldCupInfo("Côte d'Ivoire", 7.539988999999999,-5.547079999999999, "cote-ivoire.png" ),
		new WorldCupInfo("Croatia", 45.1,15.2, "croatia.png" ),
		new WorldCupInfo("Ecuador", -1.831239,-78.18340599999999, "ecuador.png" ),
		new WorldCupInfo("England", 52.3555177,-1.1743197, "england.png" ),
		new WorldCupInfo("France", 46.227638,2.213749, "france.png" ),
		new WorldCupInfo("Germany", 51.165691,10.451526, "germany.png" ),
		new WorldCupInfo("Ghana", 7.946527,-1.023194, "ghana.png" ),
		new WorldCupInfo("Greece", 39.074208,21.824312, "greece.png" ),
		new WorldCupInfo("Honduras", 15.199999,-86.241905, "honduras.png" ),
		new WorldCupInfo("Iran", 32.427908,53.688046, "iran.png" ),
		new WorldCupInfo("Italy", 41.87194,12.56738, "italy.png" ),
		new WorldCupInfo("Japan", 36.204824,138.252924, "japan.png" ),
		new WorldCupInfo("Korea Republic", 35.907757,127.766922, "korea.png" ),
		new WorldCupInfo("Mexico", 23.634501,-102.552784, "mexico.png" ),
		new WorldCupInfo("Netherlands", 52.132633,5.291265999999999, "netherlands.png" ),
		new WorldCupInfo("Nigeria", 9.081999,8.675276999999999, "nigeria.png" ),
		new WorldCupInfo("Portugal", 39.39987199999999,-8.224454, "portugal.png" ),
		new WorldCupInfo("Russia", 61.52401,105.318756, "russia.png" ),
		new WorldCupInfo("Spain", 40.46366700000001,-3.74922, "spain.png" ),
		new WorldCupInfo("Switzerland", 46.818188,8.227511999999999, "switzerland.png" ),
		new WorldCupInfo("Uruguay", -32.522779,-55.765835, "uruguay.png" ),
		new WorldCupInfo("USA", 37.09024,-95.712891, "usa.png" ),
		new WorldCupInfo("Arena Fonte Nova", -12.97883,-38.5043711, "fonte-nova.jpeg" ),
		new WorldCupInfo("Estádio das Dunas", -5.8268266,-35.2124299, "arena-dunas.jpg" ),
		new WorldCupInfo("Estádio Castelão", -3.8072311,-38.5224338, "estadio-castelao.jpg" ),
		new WorldCupInfo("Arena Pernambuco", -8.0406496,-35.0082049, "arena-pernambuco.jpg" ),
		new WorldCupInfo("Arena Amazônia", -3.083142,-60.02810849999999, "arena-amazonia.jpg" ),
		new WorldCupInfo("Estádio Nacional", -15.7835191,-47.899211, "estadio-nacional.jpg" ),
		new WorldCupInfo("Arena Pantanal", -15.604017,-56.1216325, "arena-pantanal.jpg" ),
		new WorldCupInfo("Arena Corinthians", -23.5453331,-46.4737017, "arena-corinthians.jpg" ),
		new WorldCupInfo("Estádio do Maracanã", -22.9121089,-43.2301558, "estadio-maracana.jpg" ),
		new WorldCupInfo("Estádio Mineirão", -19.865867,-43.97113150000001, "estadio-mineirao.jpg" ),
		new WorldCupInfo("Arena da Baixada", -25.4482116,-49.2769866, "arena-baixada.jpg" ),
		new WorldCupInfo("Estádio Beira-Rio", -30.065066,-51.2365144, "arena-beira-rio.jpg")
	};				

	return infoList;																																																	
}
{% endhighlight %}


### Custom AnnotationViews

Maps in Xamarin.iOS handles with annotation through the delegation pattern. When we set a MKMapViewDelegate instance to the Delegate property in the map we are transferring the responsibility to handle with annotations to the new class. 

Using this delegate we can customize the image exhibition, event handling and so on. To customize the annotations exhibition, we need to create a new class that inherits from MKMapViewDelegate and overrides the method GetViewForAnnotation. 

Our delegate will also store a reference to the information list to be possible to identify which image should be show for each annotation.

The code is below.

{% highlight c# %}
class MyMapDelegate : MKMapViewDelegate
{
	private List<WorldCupInfo> _infoList;

	public MyMapDelegate(List<WorldCupInfo> infoList) 
	{
		_infoList = infoList;
	}

	string pId = "PinAnnotation";
 
	public override MKAnnotationView GetViewForAnnotation (MKMapView mapView, NSObject annotation)
	{
		MKAnnotationView anView;

		if (annotation is MKUserLocation)
			return null; 

		// create pin annotation view
		anView = (MKPinAnnotationView)mapView.DequeueReusableAnnotation (pId);

		if (anView == null)
			anView = new MKPinAnnotationView (annotation, pId);

		var pointAnnotation = (MKPointAnnotation) annotation;
		var info = _infoList.Find (x => x.Title == pointAnnotation.Title);					

		anView.Image = GetImage (info.Image);
		anView.CanShowCallout = true;

		return anView;
	}

	public UIImage GetImage(string imageName)
	{
		var documents =
			Environment.GetFolderPath (Environment.SpecialFolder.Resources);

		var filename = Path.Combine (documents, imageName);

		var image = UIImage.FromFile (filename).Scale(new SizeF() {Height=20, Width=30});

		return image;
	}
}
{% endhighlight %}

For last, we need to associate the delegate to the map. For that let's change the code that creates the map.

{% highlight c# %}
public override void LoadView ()
{
	map = new MKMapView (UIScreen.MainScreen.Bounds);
	map.Delegate = new MyMapDelegate (_infoList);
	View = map;

	_infoList.ForEach (x => map.AddAnnotation (new MKPointAnnotation () {
		Title = x.Title,
		Coordinate = new CLLocationCoordinate2D() {
			Latitude = x.Latitude,
			Longitude = x.Longitude
		}
	}));
}
{% endhighlight %}

## It's done

At the end of this article, we can see a map with all countries and stadiums with their respective images. For those that want to see the code, [it's available in OnceDev's github][6].


![Mapa com imagens][5]


[1]: /images/posts/2014-07-13/criando-aplicacao.png 
[2]: /images/posts/2014-07-13/estrutura-projeto.png
[3]: /images/posts/2014-07-13/mapa.png
[4]: /images/posts/2014-07-13/mapa-pin.png
[5]: /images/posts/2014-07-13/mapa-image.png
[6]: https://github.com/Oncedev/WorldCupMap



