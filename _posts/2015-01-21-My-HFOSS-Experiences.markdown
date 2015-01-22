---
layout: post
title:  "H/FOSS Experiences and Reflections"
date:   2015-01-21 15:20:10
categories: homework
tags: homework
image: /assets/images/desktopx2.jpg
---

Our group finally settled on an open source project called [mopidy](https://www.mopidy.com/). This project is written in python so it will be very easy for us to get into. The following details my account of checking out and building the project from source.

First I headed over to the [docs](https://docs.mopidy.com/en/latest/) and specifically went to the [contributing](https://docs.mopidy.com/en/latest/contributing/?highlight=contribute) section. This section does a great job of getting you started into developing on the project. The first thing I had to do was make sure that I had all the major dependencies in order to start. First, I needed [Python 2.7](https://www.python.org/downloads/) which already came with Ubuntu. Next I needed [pip](https://pip.pypa.io/en/latest/) by using the ```sudo apt-get install python-pip``` command. Next, I installed [virtualenv](https://virtualenv.pypa.io/en/latest/) by using ```sudo pip install virtualenv```. This completed without issue and I went ahead and started my virtualenv on my desktop by ```virtualenv mopidy --system-site-packages```. Then I used ```cd mopidy``` and ```source bin/activate``` to start the virtualenv. Then once I had my virtualenv set up it was time to start installing the other dependencies. 

Next on the list was something called [gstreamer](http://www.gstreamer.com/) specifically version 0.10. This part got a little bit tricky because on the documentation the command to install gstreamer and the plugins was:

```
sudo apt-get install python-gst0.10 gstreamer0.10-plugins-good \
    gstreamer0.10-plugins-ugly gstreamer0.10-tools
```

However, this did not work for me and I ended up removing the "\".

```
sudo apt-get install python-gst0.10 gstreamer0.10-plugins-good gstreamer0.10-plugins-ugly gstreamer0.10-tools
```

This finally got gstreamer to install and I was able to install the remaining dependencies by navigating to the mopidy directory and using ```pip install -r dev-requirements.txt```.

Now, with all the dependencies installed the only thing left to do was to build the project. This was done by navigating to the mopidy folder and using ```sudo python setup.py develop```. Then using ```mopidy``` ran the server with no errors. 

Overall, this experience was a very good one. I got to use some knowledge I had previously of virtualenv and pip as well as learned some new things. My only major issue was with the error in the docs with the installation of the dependencies, specifically gstreamer. Hopefully, the rest of the project will end up going as smooth as this has so far. 