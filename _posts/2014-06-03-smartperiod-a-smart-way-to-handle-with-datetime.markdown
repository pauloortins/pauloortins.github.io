---
layout: post
title: SmartPeriod - A smart way to handle with date differences
categories: C# Open-Source
date: 2014-06-03
comments: true
---
Yesterday, I released my first package on NuGet, the SmartPeriod, the experience was a lot of fun and rewarding, release a NuGet package is too simple and intuitive, I hope to be contributing with more packages in the future.

<!--more-->

The code is also available publicly on [github][1].

## SmartPeriod ##

A set of classes that makes easier to work with date differences and periods.

In my projects, I had the need to show differences between dates in different formats, e.g.:

In a health systems, ages are shown in following format:

- 10 years 2 months 5 days

When handling publishing dates, is common show how old a publication is, e.g.:

- 5 minutes ago
- 2 hours ago

Being a need in more than one project I decided to modularize and release it as an open source project, maybe other people also have the same needs and then, this project can help them in the same way a lot of other open source projects are useful for me.

## Installing ##

SmartPeriod is available as a nuget package so you can install it easily

{% highlight c# %}
PM> Install-Package SmartPeriod
{% endhighlight %}


## Using ##

SmartPeriod can be used to first, calculate the time difference between two dates and also to exibihit this difference in a custom way.

### Calculating time difference 

{% highlight c# %}
var endDate = new DateTime(2014, 4, 13, 12, 37, 28);
var startDate = new DateTime(2013, 2, 10, 10, 30, 20);            
var period = new Period(startDate, endDate);

Console.WriteLine(period.Years);   // 1
Console.WriteLine(period.Months);  // 2
Console.WriteLine(period.Days);    // 3
Console.WriteLine(period.Hours);   // 2
Console.WriteLine(period.Minutes); // 7
Console.WriteLine(period.Seconds); // 8
{% endhighlight %}

### Formatting ###

{% highlight c# %}
var endDate = new DateTime(2014, 4, 13, 12, 37, 28);
var startDate = new DateTime(2013, 2, 10, 10, 30, 20);            
var period = new Period(startDate, endDate);

Console.WriteLine(period.ToString(PeriodFormatter.DateTime()));
// 1 year, 2 months, 3 days, 2 hours, 7 minutes, 8 seconds

Console.WriteLine(period.ToString(PeriodFormatter.Date()));
// 1 year, 2 months, 3 days

Console.WriteLine(period.ToString(PeriodFormatter.Time()));
// 2 hours, 7 minutes, 8 seconds

Console.WriteLine(period.ToString(PeriodFormatter.Years()));
// 1 year

Console.WriteLine(period.ToString(PeriodFormatter.Months()));
// 14 months

Console.WriteLine(period.ToString(PeriodFormatter.Days()));
// 427 days

Console.WriteLine(period.ToString(PeriodFormatter.Hours()));
// 10250 hours

Console.WriteLine(period.ToString(PeriodFormatter.Minutes()));
// 615007 minutes

Console.WriteLine(period.ToString(PeriodFormatter.Seconds()));
// 36900428 seconds
{% endhighlight %}

#### Custom Options ####

You can also use custom options.

##### Custom Separator #####

{% highlight c# %}
var endDate = new DateTime(2014, 4, 13, 12, 37, 28);
var startDate = new DateTime(2013, 2, 10, 10, 30, 20);            
var period = new Period(startDate, endDate);

Console.WriteLine(period.ToString(PeriodFormatter.DateTime().Separator(" | ")));
// 1 year | 2 months | 3 days | 2 hours | 7 minutes | 8 seconds   
{% endhighlight %}

##### Show only the datepiece more significant #####
{% highlight c# %}
var endDate = new DateTime(2014, 4, 13, 12, 37, 28);            
var startDate = new DateTime(2014, 2, 10, 10, 30, 20);     
var period = new Period(startDate, endDate);

Console.WriteLine(period.ToString(PeriodFormatter.DateTime()
    .ShowOnlyMoreSignificant()));
// 2 months
{% endhighlight %}

##### Show empty date pieces #####

{% highlight c# %}
var endDate = new DateTime(2014, 4, 13);
var startDate = new DateTime(2014, 2, 13);            
var period = new Period(startDate, endDate);

Console.WriteLine(period.ToString(PeriodFormatter.Date().ShowEmpty()));
// 0 years, 2 months, 0 days
{% endhighlight %}

##### Custom Culture #####

{% highlight c# %}
var endDate = new DateTime(2014, 4, 13, 12, 37, 28);
var startDate = new DateTime(2013, 2, 10, 10, 30, 20);            
var period = new Period(startDate, endDate);

Console.WriteLine(period.ToString(PeriodFormatter.Years().Months().Days()
    .Culture(new SelectedCulturePtBr())));
// 1 ano, 2 meses, 3 dias
{% endhighlight %}

## Adding more cultures ##

Adding more cultures is easy, you only have to implement a interface.

{% highlight c# %}
public class SelectedCultureEnUs : ISelectedCulture
{
    public DatePiece Year { 
        get { return new YearPiece("year", "years"); } 
        set { }}
    
    public DatePiece Month { 
        get { return new MonthPiece("month", "months"); } 
        set { } }
    
    public DatePiece Day { 
        get { return new DayPiece("day", "days"); } 
        set { } }
    
    public DatePiece Hour { 
        get { return new HourPiece("hour", "hours"); } 
        set { } }
    
    public DatePiece Minute { 
        get { return new MinutePiece("minute", "minutes"); } set { } }
    
    public DatePiece Second { 
        get { return new SecondPiece("second", "seconds"); } set { } }
}
{% endhighlight %}

### Supported Cultures ###

I'm currently supporting the following cultures:

- en-US
- pt-BR

[1]: https://github.com/pauloortins/SmartPeriod 
