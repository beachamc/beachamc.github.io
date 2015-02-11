---
layout: post
title:  "This Bugs Me"
date:   2015-02-10 10:17:25
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

The oldest bug I could find on our project was something to do with audio synchronization. Basically, there is an issue when switching between tracks and the head doesn't point to the new file immediately. So when a song switches you see the time for the previous song for a split second and then it catches up. The contributers mentioned that this was likely an issue with blocking and that it was only a minor issue. Since the issue is so trivial, the bug is likely to stay there for a while. The bug isn't terribly old though as it was created in January of 2013. Even though there were older entries in the bug tracker they were for future enhancements that have not been implemented and not necessarily bugs.

As far as creating a bug tracker account this was already done at this point because mopidy uses Github for their issue tracking. The system is very well maintained and has a well thought out tag system. 

For replicating a bug the easiest bug I could find to replicate was the original bug I discussed earlier. Since Mopidy is a music server, it does not have a standard client that it uses. However, people typically use a browser or a MPD client which is basically a stock client that gets its information from a server which in this case is Mopidy. For this bug I decided to use a web browser to recreate it. First I had to load some music into a local playlist through mopidy which was pretty straightforward and then I loaded up Mopidy in my browser by going to localhost and loading my local library. Then I played a song and allowed it to go on to the next song. As expected, when switching, the header on the slider for the tracking delays slightly when switching songs. This happens consistently but overall is not a major issue which is likely why it hasn't been fixed yet.

With the bug triage there was little that I could do. The lead developer is very active and moderates the bugs very quickly and makes sure that all the steps listed in the TOS article are followed. The reports themselves are actually well done--even the newer ones--in the first place so it doesn't look like he has to do a whole lot of moderation due to the quality of the community. 