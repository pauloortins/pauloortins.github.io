---
layout: post
title:  "Xamarin.Forms Layouts: Grid"
date:   2015-07-28
categories: Mobile Xamarin
comments: true
---

Hi Xamariners! How are your Xamarin projects? Is everything fine? I hope that yes! In this post I will show one more type of Layout that we can use in our [Xamarin.Forms][6] Apps, the Grid.

<!--more-->

### Grid

![Grid][5]

If I had to make a analogy to ease the understanding of what is a `Grid`, the first thing that I would think would be the HTML tables. In the Grid, as well as in tables, we place our UI elements in rows and columns, like the image above.

To add a control in a `Grid` we use the `Add` method that belongs to the `Children` collection, this collection contains all the elements that belongs to our `Grid`. The `Add` method allows us to insert new elements in two ways, let's see them.

The **Add(View item, int left, int top)** method adds an element in the column represented by the `left` parameter and in the row represented by the `top` parameter.

The example below shows how we can use this overload to organize 4 `Labels` in 2 rows and 2 columns.

{% highlight c# %}
public class GridPage : ContentPage
{
	public GridPage()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0), 0, 0);

		var grid = new Grid ();

		grid.Children.Add (new Label () {Text = "(0,0)", BackgroundColor=Color.Aqua}, 0, 0);
		grid.Children.Add (new Label () {Text = "(0,1)", BackgroundColor=Color.Gray}, 1, 0);
		grid.Children.Add (new Label () {Text = "(1,0)", BackgroundColor=Color.Yellow}, 0, 1);
		grid.Children.Add (new Label () {Text = "(1,1)", BackgroundColor=Color.Pink}, 1, 1);

		Content = grid;
	}
}
{% endhighlight %}

When running the code above we have the following result:

![Grid with 2 rows e 2 columns][0]

### Creating elements that occupy more than one row or column

The `Grid` allows us to go beyond to only choose the row and column for an element, we can also make this element occupy more than one row or column, in other words, we can define the `RowSpan` or `ColSpan` for each element in our grid.

The **Add(View item, int left, int right, int top, int bottom):** method adds an element in the column represented by the `left` parameter and in the row represented by the `top` parameter as in the previous example, however, in this overload we have the `right` and the `bottom` parameters that are used to define the column and row limits. As a short example, if we call this method with the following parameters `left=0, right=2, top=1 and bottom=4`, we will define an element that will occupy the columns 0 and 1 and the rows 1, 2 and 3. It's not intuitive but is how it works :D

The example below creates the same 4 labels but now we will use this overload to set different `RowSpans` and `ColumnSpans`. The Acqua and the Gray Label will occupy 2 columns and 1 row, the Yellow one will occupy 1 column and two rows and, for last, the Pink one will occupy 3 columns and 1 row.

{% highlight c# %}
public class GridPage2 : ContentPage
{
	public GridPage2()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0), 0, 0);

		var grid = new Grid ();

		// Columns 0 and 1 - Row 0		
		grid.Children.Add (new Label () {Text = "(0,0)", BackgroundColor=Color.Aqua}, 0, 2, 0, 1);

		// Columns 0 and 1 - Row 1
		grid.Children.Add (new Label () {Text = "(1,0)", BackgroundColor=Color.Gray}, 0, 2, 1, 2);

		// Column 2 - Rows 0 and 1
		grid.Children.Add (new Label () {Text = "(0,2)", BackgroundColor=Color.Yellow}, 2, 3, 0, 2);

		// Columns 0, 1 and 2 - Row 2
		grid.Children.Add (new Label () {Text = "(2,0)", BackgroundColor=Color.Pink}, 0, 3, 2, 3);

		Content = grid;
	}
}
{% endhighlight %}

When running the code above we have the following result:

![Grid with rows and columns with different spans][1]

### Another way to define elements with different ColumnSpan and RowSpan

We cannot only define the `ColumnSpan` and the `RowSpan` that an element will occupy using the `Add` method, we can also use utility methods in the `Grid` class.

The methods `Grid.SetColumSpan` and `Grid.SetRowSpan` allow us to set both spans passing the element and the number of columns or rows that it will occupy.

The code below does the same the previous code does but using Grid's methods.

{% highlight c# %}
public class GridPage3 : ContentPage
{
	public GridPage3 ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0), 0, 0);

		var grid = new Grid ();

		var aqua = new Label () {Text = "(0,0)", BackgroundColor=Color.Aqua};
		grid.Children.Add (aqua, 0, 0);
		Grid.SetColumnSpan (aqua, 2);

		var gray = new Label () { Text = "(1,0)", BackgroundColor = Color.Gray };
		grid.Children.Add (gray, 0, 1);
		Grid.SetColumnSpan (gray, 2);

		var yellow = new Label () { Text = "(0,2)", BackgroundColor = Color.Yellow };
		grid.Children.Add (yellow, 0, 2);
		Grid.SetRowSpan (yellow, 2);

		var pink = new Label () { Text = "(2,0)", BackgroundColor = Color.Pink };
		grid.Children.Add (pink, 0, 2);
		Grid.SetColumnSpan (pink, 3);

		Content = grid;
	}
}
{% endhighlight %}

When running the code above we have the same result:

![Grid with rows and columns with different spans][2]

### Using different sizes for each column/row

I don't know if you perceived that but in all grids that we created until here, the rows and columns had exactly the same size, however, in the real world it won't happen, in the real world we have to be able to customize these sizes and we can do that using the `RowDefinitions` and `ColumnDefinitions` properties.

The `GridUnitType` enum provides for us some size definitions:

**Absolute:** Sets the cell's size equal to the number of device-specific-units (dpu).   
**Auto:** Sets the cell's size equal to its content.    
**Star:** Sets the cell's size equal to a proportion of the remaining space after all the cells with `Absolute` and `Auto` definitions are placed. Example: If you had two column definitions with GridLength equal to (1, GridUnitType.Star) both columns will divide the remaining space, however, if one of them has GridLength equals to (2, GridUnitType.Start) and the other one equals to (1, GridUnitType.Start) the former will occupy 66% of the remaining space and the latter only 33%.  

The code below shows a row with multiple columns, where the first column is set with an `Auto` definition, in other words, it will occupy the needed space to `show all its content`, the second one is set with an `Absolute` definition with `size 48` and it will occupy `exactly 48 dpus`, and, for last, the third one is set using a `Star` definition and, how it's the only one with this definition, it will occupy `all the the remaining space`.

{% highlight c# %}
public class GridPage4 : ContentPage
{
	public GridPage4 ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0), 0, 0);
		var grid = new Grid ();

		grid.Children.Add (
			new Image() {
				Source= Device.OnPlatform("Icon-60@2x.png", "icon.png", "")
			}, 0, 0);

		grid.Children.Add (new Label () { Text = "40", BackgroundColor = Color.Yellow}, 1, 0);
		grid.Children.Add (new Label () { Text = "Remainder", BackgroundColor = Color.Pink}, 2, 0);

		grid.ColumnDefinitions.Add (new ColumnDefinition () 
			{
				Width = new GridLength(1, GridUnitType.Auto)
			});

		grid.ColumnDefinitions.Add (new ColumnDefinition () 
			{
				Width = new GridLength(40, GridUnitType.Absolute)
			});

		grid.ColumnDefinitions.Add (new ColumnDefinition () 
			{
				Width = new GridLength(1, GridUnitType.Star)
			});

		Content = grid;
	}
}
{% endhighlight %}

When running the code above we have the following result:

![Grid with ColumnDefinitions][3]

The same can be used for rows. The code below shows a unique column with multiple `RowDefitions`.
 
{% highlight c# %}
public class GridPage5 : ContentPage
{
	public GridPage5 ()
	{
		Padding = new Thickness (0, Device.OnPlatform (20, 0, 0), 0, 0);
		var grid = new Grid ();

		grid.Children.Add (
			new Image() {
				Source= Device.OnPlatform("Icon-60@2x.png", "icon.png", "")
			}, 0, 0);

		grid.Children.Add (new Label () { Text = "40", BackgroundColor = Color.Yellow}, 0, 1);
		grid.Children.Add (new Label () { Text = "Remainder", BackgroundColor = Color.Pink}, 0, 2);

		grid.RowDefinitions.Add (new RowDefinition () 
			{
				Height = new GridLength(1, GridUnitType.Auto)
			});

		grid.RowDefinitions.Add (new RowDefinition () 
			{
				Height = new GridLength(40, GridUnitType.Absolute)
			});

		grid.RowDefinitions.Add (new RowDefinition () 
			{
				Height = new GridLength(1, GridUnitType.Star)
			});

		Co  ntent = grid;
	}
}
{% endhighlight %}
 
When running the code above we have the following result:    
   
![Grid with RowDefinitions][4]

### This is it!

This is it, Xamariners, in this post we covered all the use possibilities of a Grid, that is very useful component when we need to design screens with multiple elements.

I'm 100% available to answer any question or to hear suggestions, so if you have one, leave a comment.

[0]: /images/posts/2015-07-28/grid1.png
[1]: /images/posts/2015-07-28/grid2.png
[2]: /images/posts/2015-07-28/grid2.png
[3]: /images/posts/2015-07-28/grid3.png
[4]: /images/posts/2015-07-28/grid4.png
[5]: /images/posts/2015-07-28/grid.png
[6]: /2015/04/04/xamarin-forms/
