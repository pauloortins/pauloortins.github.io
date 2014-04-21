---
layout: post
title: "Where, when and how Apache's Commits are made"
categories: Open Source Source Mining
date: 2013-10-12
comments: true
---
<p>Six months ago, I started my master's degree where I'm researching about software engineering and mining of code repositories. In the next months I pretend to, besides write about C#, Javascript and programming in general, also write about subjects that I'm researching, tools that I'm developing, and papers that I'm reading. In this post, I will talk about the results that I had through mining and gathering information from Apache Httpd repository.</p>
<p><br/></p>
<h2>Introduction</h2>
<p>When developing a software, developers are always adding, changing and removing software artifacts. These software artifacts can be code, documentation, config files and so on. To manage these changes, developers use a VCS (version control system), good VCS examples are CVS, SVN, GIT and Mercurial. These VCS and the changes they manage, end up being a important information source about a software and everything it's related. Through mining we can answer a lot of questions about the software that are being developed:</p>
<ul>
<li>How many developers are working in a software?</li>
<li>Where they come from?</li>
<li>What time they work in the project?</li>
<li>Who are the commiters who work in each piece of the software?</li>
<li>Who introduces more bugs?</li>
<li>Who produces the better code?</li>
</ul>
<p><br/></p>
<h2>About Apache Httpd</h2>
<blockquote><p>
The Apache HTTP Server Project is an effort to develop and maintain an open-source HTTP server for modern operating systems including UNIX and Windows NT. The goal of this project is to provide a secure, efficient and extensible server that provides HTTP services in sync with the current HTTP standards.
</p></blockquote>
<p>Apache Httpd have been developed since 1996 and, today, is in version 2.4.6 released in July, 2013. During this time, more than 100 developers made more than 55k commits. Due this size, Apache Httpd is constantly studied by computer scientists being target for various studies in academy. Httpd's artifacts are managed in a SVN(Subversion) repository and can be found in this <a href="svn.apache.org/repos/asf/httpd/">link</a>, you can also find more information about the project <a href="http://httpd.apache.org/ABOUT_APACHE.html">here</a>.</p>
<p><br/></p>
<h2>The Research</h2>
<p>In this research, I was interested to gather information about the Apache Httpd Developers. The following questions were answered:</p>
<ul>
<li>Where developers come from?</li>
<li>When, time and weekday, developers make commits?</li>
<li>Which file types are edited?</li>
</ul>
<p><br/></p>
<h2>Mining Apache Httpd Repository - Getting Commits</h2>
<p>I didn't know nothing about Python, so I chose python (I'm not crazy, I was just trying to add one more tool to my belt) to extract data from the SVN Repository. Honestly, I don't know if there are others, but I found a very good tool to extract data from SVN Repositories called PySVN. I extracted all the data that I needed using the following code:</p>

{% highlight python %}
"""Documentation Link:
 http://pysvn.tigris.org/docs/pysvn_prog_ref.html#pysvn_client_log"""
import pysvn

class SvnService(object):
    """docstring for SvnService"""
    def __init__(self, repository_url):
        self.repository_url = repository_url

    def get_info(self):
        client = pysvn.Client()    
        data = client.log(self.repository_url, discover_changed_paths=True)
        return data
{% endhighlight %}
<p><br/></p>
<h2>Mining Apache Httpd Repository - Getting Geolocations</h2>
<p>When getting commits from a repository, we don't have any information about a developer, besides his login. One of my goals was draw a commit map with commits distributed by location, to get these information I grouped all the commits by developer's login and started to search manually their geolocations. The Apache Httpd project has a web page with some developers profile that includes their address, you can find this information here. After this step, I already had the geolocation information for a lot of developers, but I was still missing some of them. The Apache Foundation has another page where I can find a developer's name from his login. <a href="http://people.apache.org/committer-index.html">Here is the page</a>. At this moment, I had all developers name, so I started to google them and for my happiness most of them has a online profile(Blog, Github, personal site and so on) with their address. The last step was to get their latitude and longitude through <a href="https://developers.google.com/maps/">google maps API</a> and their address.</p>

{% highlight javascript %}
// Request
"http://maps.googleapis.com/maps/api/geocode/json?address=Brazil&sensor=false"

// Response
{
   "results" : [
      {
         // ...
            "location" : {
               "lat" : -14.235004,
               "lng" : -51.92528
            }
         // ...   
       }
   ],
   "status" : "OK"
}

{% endhighlight %}
<p><br/></p>
<h2>Mining Apache Httpd Repository - Adjusting the time zone</h2>
<p>Two of my other stats depends on commit's time, the time of each commits for an obvious reason, and the weekday. The relation between time zones and weekdays are a little trickier, if a developer make a commit around midnight and we adjust the commit time accordingly to his time zone, it also can change the commit's date and of course changing the commit's weekday. To adjust the time zone, they are originally in UTC, I used a google API again, the <a href="https://developers.google.com/maps/documentation/timezone/">Google Time Zone API</a>, it's use is very simple, making request with a location (latitude and longitude), the api returns a json with information about the location's time zone.</p>

{% highlight javascript %}
// Request
"https://maps.googleapis.com/maps/api/timezone/json? +
location=39.6034810,-119.6822510&timestamp=1331161200&sensor=false"

// Response
{
   "dstOffset" : 0.0,
   "rawOffset" : -28800.0,
   "status" : "OK",
   "timeZoneId" : "America/Los_Angeles",
   "timeZoneName" : "Pacific Standard Time"
}

{% endhighlight %}
<p><br/></p>
<h2>Results</h2>
<p><br/></p>
<h2>Commits By Location</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto.png" alt="mapa-aberto" width="1048" height="456" class="alignnone size-full wp-image-1100" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado.png" alt="mapa-fechado" width="975" height="344" class="alignnone size-full wp-image-1099" /></a></p>
<p>Most of the Apache's Commits comes from USA, England and Germany. Analyzing only the Top 20 committers, 12 come from USA, 4 from Germany,<br />
2 from England, 1 from Denmark and 1 from Canada. A interesting point here is the Research Triangle Park who contributed a lot to Apache Httpd with 7 committers ( 3 of them in the Top 20).</p>
<h2>Top 20 Committers and their Locations</h2>
<p>1. William A. Rowe Jr. - Illinois, USA<br />
2. Jim Jagielski - Maryland, USA<br />
3. André L. Malo - Germany<br />
4. Jeff Trawick - North Carolina, USA<br />
5. Rich Bowen - Kentucky, USA<br />
6. Stefan Fritsch - Germany<br />
7. Rüdiger Plüm - Germany<br />
8. Dean Gaudet - California, USA<br />
9. Graham Leggett - England<br />
10. Ryan Bloom - California, USA<br />
11. Justin Erenkrantz - California, USA<br />
12. Joe Orton - England<br />
13. Joe Schaefer - Florida, USA<br />
14. Daniel Gruno - Denmark<br />
15. Joshua Slive - Canada<br />
16. Ken Coar - North Carolina, USA<br />
17. Doug MacEachern - California, USA<br />
18. Bill Stoddard - North Carolina, USA<br />
19. Ralf S. Engelschall - Germany<br />
20. Roy T. Fielding - California, USA</p>
<p><br/></p>
<h2>Commits By Time and By Weekday</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/commits-weekday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/commits-weekday.png" alt="commits-weekday" width="617" height="312" class="alignnone size-full wp-image-1098" /></a><br />
<a href="http://pauloortins.com/wp-content/uploads/2013/10/commits-timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/commits-timeoftheday.png" alt="commits-timeoftheday" width="620" height="313" class="alignnone size-full wp-image-1097" /></a></p>
<p>Most of the commits were made in work hours and in work days. It can suggest that committers made these commits while working in their jobs or in their research (like the developers from Research Triangle Park).</p>
<p><br/></p>
<h2>By Date</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/commits-time.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/commits-time.png" alt="commits-time" width="1306" height="311" class="alignnone size-full wp-image-1096" /></a></p>
<p>Analyzing commits through years, we can see that Apache Httpd is stable, it didn't have a boom (like Rails had in the last years), number of commits through the years, is, in average, almost the same.</p>
<p><br/></p>
<h2>By File Extensions</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/bubble-chart.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/bubble-chart.png" alt="bubble-chart" width="394" height="504" class="alignnone size-full wp-image-1095" /></a></p>
<p>As I expected, in a C project, most of the commits come from C files, '.c' and '.h'. The NotSpecified extension, actually is files without extensions, usually text files. There is also a lot of documentation files, written in html files and their respective translations '.html.en', '.html.fr', '.html.de' and so on.</p>
<p><br/></p>
<h2>Top 5 Committers</h2>
<p>As curiosity, below is the same graphs by committer.</p>
<p><br/></p>
<h2>William A. Rowe Jr.</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto1.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto1.png" alt="mapa-aberto" width="575" height="342" class="alignnone size-full wp-image-1140" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado1.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado1.png" alt="mapa-fechado" width="530" height="333" class="alignnone size-full wp-image-1139" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/weekday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/weekday.png" alt="weekday" width="729" height="372" class="alignnone size-full wp-image-1138" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/timeoftheday.png" alt="timeoftheday" width="728" height="376" class="alignnone size-full wp-image-1137" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/bytime.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/bytime.png" alt="bytime" width="1542" height="375" class="alignnone size-full wp-image-1136" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/files.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/files.png" alt="files" width="514" height="470" class="alignnone size-full wp-image-1135" /></a></p>
<p><br/></p>
<h2>Jim Jagielski</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto2.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-aberto2.png" alt="mapa-aberto" width="667" height="320" class="alignnone size-full wp-image-1148" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado2.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/mapa-fechado2.png" alt="mapa-fechado" width="698" height="332" class="alignnone size-full wp-image-1147" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/jim-week-day.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/jim-week-day.png" alt="jim-week-day" width="619" height="309" class="alignnone size-full wp-image-1146" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/jim-timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/jim-timeoftheday.png" alt="jim-timeoftheday" width="622" height="316" class="alignnone size-full wp-image-1145" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/jim-bydate.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/jim-bydate.png" alt="jim-bydate" width="1304" height="310" class="alignnone size-full wp-image-1144" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/jim-byfile.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/jim-byfile.png" alt="jim-byfile" width="471" height="365" class="alignnone size-full wp-image-1143" /></a></p>
<p><br/></p>
<h2>André L. Malo</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-mapa-aberto.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-mapa-aberto.png" alt="nd-mapa-aberto" width="547" height="423" class="alignnone size-full wp-image-1154" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-mapa-fechado.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-mapa-fechado.png" alt="nd-mapa-fechado" width="532" height="428" class="alignnone size-full wp-image-1153" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-weekday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-weekday.png" alt="nd-weekday" width="726" height="372" class="alignnone size-full wp-image-1152" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-timeoftheday.png" alt="nd-timeoftheday" width="732" height="384" class="alignnone size-full wp-image-1151" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-date.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-date.png" alt="nd-date" width="1534" height="371" class="alignnone size-full wp-image-1150" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/nd-fileextension.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/nd-fileextension.png" alt="nd-fileextension" width="443" height="434" class="alignnone size-full wp-image-1149" /></a></p>
<p><br/></p>
<h2>Jeff Trawick</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-mapa-aberto.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-mapa-aberto.png" alt="trawick-mapa-aberto" width="476" height="282" class="alignnone size-full wp-image-1160" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-mapa-fechado.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-mapa-fechado.png" alt="trawick-mapa-fechado" width="434" height="362" class="alignnone size-full wp-image-1159" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-weekday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-weekday.png" alt="trawick-weekday" width="726" height="376" class="alignnone size-full wp-image-1158" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-timeoftheday.png" alt="trawick-timeoftheday" width="729" height="374" class="alignnone size-full wp-image-1157" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-date.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-date.png" alt="trawick-date" width="1531" height="370" class="alignnone size-full wp-image-1156" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/trawick-filextension.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/trawick-filextension.png" alt="trawick-filextension" width="436" height="492" class="alignnone size-full wp-image-1155" /></a></p>
<p><br/></p>
<h2>Rich Bowen</h2>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-mapaaberto.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-mapaaberto.png" alt="rbowen-mapaaberto" width="440" height="329" class="alignnone size-full wp-image-1166" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-mapafechado.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-mapafechado.png" alt="rbowen-mapafechado" width="469" height="395" class="alignnone size-full wp-image-1165" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-weekday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-weekday.png" alt="rbowen-weekday" width="728" height="375" class="alignnone size-full wp-image-1164" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-timeoftheday.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-timeoftheday.png" alt="rbowen-timeoftheday" width="728" height="371" class="alignnone size-full wp-image-1163" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-date.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-date.png" alt="rbowen-date" width="1530" height="374" class="alignnone size-full wp-image-1162" /></a></p>
<p><a href="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-filextension.png"><img src="http://pauloortins.com/wp-content/uploads/2013/10/rbowen-filextension.png" alt="rbowen-filextension" width="462" height="430" class="alignnone size-full wp-image-1161" /></a></p>
