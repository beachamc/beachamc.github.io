---
layout: post
title:  "Refactoring Mindset"
date:   2015-02-22 08:00:25
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

A) The module in RMH Homebase that displays a shift’s notes field is the calendar module.

B) The following code is ugly:

From calendar.php

{% highlight php startinline=true %}

if ($edit==true && !($days[6]->get_year()<$year 
    || ($days[6]->get_year()==$year 
    && $days[6]->get_day_of_year()<$doy) ) 
    && $_SESSION['access_level']>=2)
        echo "<p align=\"center\">
        <input type=\"submit\" 
        value=\"Save changes to all notes\" 
        name=\"submit\">";

{% endhighlight %}

from calendarFam.php

{% highlight php startinline=true %}

if ($edit==true && !($days[6]->get_year()<$year 
    || ($days[6]->get_year()==$year 
    && $days[6]->get_day_of_year()<$doy) ) 
    && $_SESSION['access_level']>=2)
        echo "<p align=\"center\">
        <input type=\"submit\" 
        value=\"Save changes to all notes\" 
        name=\"submit\">";

{% endhighlight %}