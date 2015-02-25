---
layout: post
title:  "Refactoring Mindset"
date:   2015-02-22 08:00:25
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

5.7)

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
        echo "<p align=\"center\"><input type=\"submit\" 
        value=\"Save changes to all notes\" name=\"submit\">";

{% endhighlight %}

C) Both of the above lines can be combined into a predates(a, b) function that will return True if a predates b. This will help us git rid of that long string of conditions in the if block.

{% highlight php startinline=true %}

function predates($days,$year,$doy) {
	if ($days[6]->get_year()<$year 
    || ($days[6]->get_year()==$year 
    && $days[6]->get_day_of_year()<$doy)) {
    	return False;
	}
	else {
		return True;
	}
}

{% endhighlight %}

D) Then the orginal code can be rewritten as:

{% highlight php startinline=true %}

if ($edit==true && predates($days,$year,$doy)
    && $_SESSION['access_level']>=2)
        echo "<p align=\"center\"><input type=\"submit\" 
        value=\"Save changes to all notes\" name=\"submit\">";

{% endhighlight %}

E) After adding tests everything seems to be working perfectly, and we eliminated a bunch of ugly code!

5.8)

A)

{% highlight php startinline=true %}

if(!$shift->has_sub_call_list() || !(select_dbSCL($shift->get_id()) instanceof SCL)) {
		echo "<input type=\"hidden\" name=\"_submit_generate_scl\" value=\"1\"><br>
		<input type=\"submit\" value=\"Generate Sub Call List\" name=\"submit\"
			style=\"width: 250px\">";
	}
	else {
		echo "<input type=\"hidden\" name=\"_submit_view_scl\" value=\"1\"><br>
		<input type=\"submit\" value=\"View Sub Call List\" name=\"submit\" style=\"width: 250px\">";
	}

{% endhighlight %}

It looks like the sub_call_list has to be created otherwise you get a view call list.

B) The week is archived when a new week is added

from addWeek_newweek.inc 

{% highlight php startinline=true %}

else if (time()>$end) {
    $options="archived 
        (<a href=\"calendar.php?id=
            ".$week[0]."\">view</a>)";
    $week2=get_dbWeeks($week[0]);
    $week2->set_status("archived");
    update_dbWeeks($week2);
}

{% endhighlight %}

Automatically archiving the previous week could limit users ability to see what the schedule was. This should at least be extended for a few weeks.
