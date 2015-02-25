---
layout: post
title:  "Refactoring Mindset"
date:   2015-02-22 08:00:25
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

A) The module in RMH Homebase that displays a shift’s notes field is the calendar module.

B) 
{% highlight php %}

	if($edit && ($year < $day->get_year() || $year == $day->get_year() && $doy <= $day->get_day_of_year() ) )
			$s=$s."e";
		$s=$s."\"><table id=\"shiftbox\"><tr><td class=\"persons\">";
		if($edit && ($year < $day->get_year() || $year == $day->get_year() && $doy <= $day->get_day_of_year() ) )
			$s=$s."<a id=\"shiftlink\" href=\"editShift.php?shift=".$day->get_id()."-".$shift."\">";
		$s=$s.get_people_for_shift($day,$shift);
		if($edit && ($year < $day->get_year() || $year == $day->get_year() && $doy <= $day->get_day_of_year() ) ) {
			$s=$s."</a>";
			// if manager, adds shift notes
			if($_SESSION['access_level']>=2) {
				$s=$s."</td></tr><tr><td class=\"notes\" align=\"center\">".
				"<textarea id=\"shift_notes\" rows=\"1\" name=\"shift_notes_".$day->get_shift($shift)->get_id()."\">"
					.$day->get_shift($shift)->get_notes()."</textarea>";
			}
			else {
				$shiftnote=$day->get_shift($shift)->get_notes();
			    $s=$s."</td></tr><tr><td class=\"notes\"><p id=\"shift\">".$shiftnote."</p>";
			}
		}

{% endhighlight %}