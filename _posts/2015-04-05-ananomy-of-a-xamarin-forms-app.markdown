---
layout: post
title:  "Anatomy of a Xamarin.Forms app"
date:   2015-04-05
categories: Mobile Xamarin
comments: true
---

Hi folks, continuing our post series about Xamarin.Forms, this time I will show how to create our first Xamarin.Forms app, how the project is structured and which are the most important files.

<!--more-->

### Creating the first app

In this post I will be using the [Xamarin Studio, Xamarin Official IDE][1], the Xamarin Studio, as well as in Visual Studio, comes with some template options that help us to save time when creating a new project, fortunately, there is a template for Xamarin.Forms apps! To create a new app we will use the template selected in the image below, our first app, with a touch of originality, will be named `MyFirstApp`.

![Creating the first app][2]

After the creation of the solution, the template already brings to us some projects, they are the `MyFirstApp`, the `MyFirstApp.Droid` and the `MyFirstApp.iOS`. We can see them in the image below:

![First Project][3]

### MyFirstApp, shared project

![Shared Project in Detail][4]

Much is said about productivity and code sharing in Xamarin apps, and is through this project that this magic happens, in this project we are going to put the code that will be used for business rules, creation of views, view models, database connection, access to web services and so on, in a few words, we want to put here the maximum amount of code possible. This project will be consumed by each platform project, in this post, the `MyFirstApp.Droid` and the `MyFirstApp.iOS`. The code below deserves some comments:

{% highlight c# %}
public class App : Application
{
	public App ()
	{
		// The root page of your application
		MainPage = new ContentPage {
			Content = new StackLayout {
				VerticalOptions = LayoutOptions.Center,
    				Children = {
						new Label {
							XAlign = TextAlignment.Center,
							Text = "Welcome to Xamarin Forms!"
						}
					}
				}
			};
	}

	protected override void OnStart ()
	{
		// Handle when your app starts
	}

	protected override void OnSleep ()
	{
		// Handle when your app sleeps
	}

	protected override void OnResume ()
	{
		// Handle when your app resumes
	}
}
{% endhighlight %}

The code above shows the creation of the class `App`, this project's entry point, in this class is created the app's initial page, lifecycle events are defined, as well as we can put info that can be shared by different pages in our app, e.g. user info or last filters used in the actual session.

### MyFirstApp.Droid

![Android Project in Detail][5]

An Android developer could easily think that this project was generated with the [Android Studio][7], all the files and folders are equal. In this project we will put everything that is specific to the android version, images, custom controls or layouts and classes that access any native API like text-to-speech, file access and NFC.

{% highlight c# %}
[Activity (Label = "MyFirstApp.Droid", Icon = "@drawable/icon", 
		MainLauncher = true, ConfigurationChanges = ConfigChanges.ScreenSize | ConfigChanges.Orientation)]
public class MainActivity : global::Xamarin.Forms.Platform.Android.FormsApplicationActivity
{
	protected override void OnCreate (Bundle bundle)
	{
		base.OnCreate (bundle);

		global::Xamarin.Forms.Forms.Init (this, bundle);

		LoadApplication (new App ());
	}
}
{% endhighlight %}

As well as in the Android apps written in Java, in a Xamarin.Forms Android App, the `MainActivity` is also the app's entry point, in this case, we want to use it to invoke the shared project's entry point (remember that we are sharing the creation of the pages between the projects). In the `MainActivity` we can also configure dependency injections, analytics services (like Google Analytics), crash reports services (like Xamarin Insights) and any other functionality that needs to be initialized from an Android project.

### MyFirstApp.iOS

![iOS Project in Detail][6]

If an Android developer could easily think that the Android project was generated with the Android Studio, the same could happen with a iOS developer, the folders, files, the way the images are nominated are also the same. As well as it happens in the `MyFirstApp.Droid`, in this project we will put code that is specific to the iOS version (custom UI, use of native APIs and so on). The image below show a print of the file `Info.plist`, we can see that the file is equal to the file that we use in the [XCode][8].

![Info.plist Info][9]
![Info.plist Icons][10]

In the iOS, the entry point is the class `Main`, as well as it happens in iOS with objective-C, it will call the class `MainDelegate` that will invoke our shared entry point. And as well as in the `MainActivity`, in this class we can put code that will be use to initialize services like analytics, crash reports, dependency injection and so on.

### This is it!

In this post, I wanted to show how we can create an app using Xamarin.Forms, what is the initial structure and the responsibility of each class, in the next posts I will talk more about Pages, Layouts and Controls that are the components that we use to create our apps. See you soon!

[1]: http://xamarin.com/studio
[2]: /images/posts/2015-04-05/creating-first-app.png
[3]: /images/posts/2015-04-05/project-created.png
[4]: /images/posts/2015-04-05/core-detailed.png
[5]: /images/posts/2015-04-05/droid-detailed.png
[6]: /images/posts/2015-04-05/ios-detailed.png
[7]: http://developer.android.com/tools/studio/index.html
[8]: https://developer.apple.com/xcode/
[9]: /images/posts/2015-04-05/infoplist-info.png 
[10]: /images/posts/2015-04-05/infoplist-icons.png 
