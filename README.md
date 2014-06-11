Telectroscope
========

A fun way to connect two places (offices / meeting rooms / common areas) across large distances with an automated FaceTime setup

###What does it do, and why would I want to do it?

The FaceTime portal is a fantastic way of keeping remote offices linked. Like a modern-day global water-cooler, but without the water… and just cooler!

Skype, hangouts, or facetime calls are great in regular usage too, but people tend to think ‘oh the person is too busy, I won’t bother them’. With the Portal set up, it’s a lot more relaxed and allows people to wander past the portal (if you set it up in a well-trafficked area) and catch up with people at the other site who are also near the portal. With the offices here at PaperCut we’ve noticed a definite positive change with the offices chatting between each other, and more people from different teams chatting to people in other teams. ‘Cross-collaboration’ - in presentation-speak.

This open source project provides the software, reference hardware and setup documentation required to get everything setup in your organization.

###How does it work?

The system is based around a mac mini (or other mac) connected to a TV with a webcam, in each location. The software (a script)  automates an always-on video conference session between two sites. It’s a set-and forget system - all you do is set it up and each day it boots up and the portal opens at your predefined times (i.e. overlapping business hours).

At a high level is works by automating Apple FaceTime on a set schedule. On the software side it’s a Shell Script (.command file) that runs on system startup and does the work behind the scenes.

###What are your Hardware Recommendations?

There’s no hard-and-fast rules for this - but here’s the hardware that we’re using at PaperCut.  Consider this reference hardware and a good starting point:

* Mac computer - Base model Mac Mini
* Webcam - Logitech Webcam C930e
* HDMI Cable
* External Speakers
* TV with HDMI (best to get one that has the ability to set times to automatically switch on and switch off the TV, and to be able to set the input on wakeup) - e.g. Samsung 50 or 46 inch TV

###Let's set it up!

####Step 1: Setting up the Mac

The Mac setup is pretty standard with the following configuration:

* *System Preferences -> Users and Groups:* Set up a default user account on the mac at each of the sites - e.g. site1-portal-user and site2-portal-user
* *System Preferences -> Users & Groups -> Login Options:* Ensure Automatic Login is set to the user created in the step above
* *System Preferences -> Bluetooth:* Turn off Bluetooth - The box will run without a keyboard and mouse, so turn off Bluetooth so it does not raise “unable to find” message
* *System Preferences -> Sharing:* Change the hostname of each machine to e.g. site1-portal-mac and site2-portal-mac
* *System Preferences -> Sharing:* Enable Screen Sharing to support remote management.
* *System Preferences -> Energy Saver:* Set the Screen to Never turn off
* *System Preferences -> Energy Saver -> Schedule:* Set startup to [some time before portal start time] and Set sleep time to [some time after portal end time]

####Step 2: Configure FaceTime

* Create an Apple ID for each of the sites, in order to use with FaceTime at the [Apple ID page](https://appleid.apple.com)
* Login to FaceTime with the Apple ID assigned for that site

####Step 3: Setup the script

* Copy the script ```telectroscope.command``` onto the Desktop: Download from the [Telectroscope github repository](http://github.com/squashedbeetle/telectroscope)
* Ensure the script has execute permissions (e.g. chmod +x ~/Desktop/telectroscope.command )
* Add the telectroscope.command to the login startup items for the account: System Preferences -> Users & Groups -> Login Items, then add ~/Desktop/telectroscope.command
* Edit the ‘Config Section’ of the script:
```
CALLER_FACETIME_ID - enter the facetime login of the caller site
CALLER_HOSTNAME - enter the hostname of the caller machine
RECEIVER_FACETIME_ID - enter the facetime login of the receiver site
RECEIVER_HOSTNAME - enter the hostname of the receiver machine
OPEN_UTC_HOURS - list the hours of the day that the portal should be open
OPEN_UTC_DAYS_OF_WEEK - list the days of the week you want the portal to be open
```

####Step 4: Setup Additional items to make your life happier!

#####Webcam Zoom
* Try using a utility from the manufacturer of your Webcam - e.g. the Logitech Utility from the Mac App Store, and use it to adjust the zoom/pan to make sure the picture looks good!
* 1.3x times zoom
* Ideally position the camera so that when people on the other side are looking at the screen, it looks like they are looking directly into the portal

#####TV Setup
* It’s recommended to make the following changes on the TV (assuming the TV supports it)
* Turn on/off on a schedule (matching the portal open/close schedule)
* Turn off on no HDMI input
* Due to the delay in signal through the HDMI cable, the echo/feedback cancellation built into Skype seems to have problems so you will need to use external speakers plugged directly into the mac, rather than using the speakers built into the TV.

#####Manual Management/Startup
* If you need to temporarily turn off the portal, simply close the Terminal window.  When finished, open it again by double clicking on telectroscope.command on the Desktop.

###Looks good so far - are there any cool features?

* Automatic wake/sleep of the TV and the Mac, and automatic start/stop of the script means that the portal automatically opens/closes with no intervention required on either end!
* Custom sounds - we prefer Wallace and Gromit sound effects, but Dr Who and other sounds are also more than appropriate!
* Automatic reconnect on failure - if the call drops due to a bad connection, the call with be re-attempted regularly.

###Why external speakers?

It’s probably worth re-iterating this part - due to the delay in signal through the HDMI cable, the echo/feedback cancellation built into Skype seems to have problems - so you will need to use external speakers plugged directly into the mac, rather than using the speakers built into the TV.

###What are the details of the script workings?

* On startup telectroscope.command is automatically started and permanently runs in a Terminal Window.
* One system is the “called” and the other the “receiver”

*On the Receiver:*
* FaceTime is configured to automatically accept calls from “caller”.
* If the portal time window is open (based on config), FaceTime is started and waits for a call.
* When a call is received/started, full screen mode is activated.
* FaceTime is quit when the portal time is closed.

*On the Caller:*
* If the portal time window is open (based on config), FaceTime is started and instructed to call “receiver”.
* When a call is received/started, full screen mode is activated.
* If the call drops, or is unable to start, it retries
* FaceTime is quit when the portal time is closed.

###Talking of Mac - why did you choose that platform?

There were a few reasons that a mac setup worked for us in our situation, but there are always other ways of doing things…

* Easy to replicate the exact same setup between different sites
* Mac hardware is available world-wide
* Facetime has excellent quality - in testing from the West Coast of the US to Australia on a daily basis, the quality remains exceptional

###How much bandwidth does FaceTime need, and how much data will I use up when the Portal is open?

* HD Video bandwidth suggestions for FaceTime from Apple are listed here: http://support.apple.com/kb/ht4534
* In terms of data usage, we’ve found that 300 MB data uploaded / 800 MB downloaded for a 1 hour call is average, however it depends on how much activity there is, so these numbers could vary greatly!

###How do I configure the Portal Open/Close sounds?

* For the Portal Open sound - create or copy a .wav file to the same directory as the script, and name it: portal_open.wav
* For the Portal Close sound - create or copy a .wav file to the same directory as the script, and name it: portal_close.wav

###Where does the ‘Telectroscope’ name come from?

It’s an old-school (19th century, according to [Wikipedia](http://en.wikipedia.org/wiki/Telectroscope) term applying to ‘science-based systems of distant seeing’. It’s also a nod to a more [modern day art project](http://www.artichoke.uk.com/events/the_telectroscope/), allowing people in New York and London to see each other, through a steam-punk-esque ‘tube’... 

###The fine print...

This open source project is sponsored by [PaperCut Software](http://www.papercut.com/), the coffee-fueled producers of print management software.  The Authors are Chris ([codedance](https://github.com/codedance/)) and Tim ([squashedbeetle](https://github.com/squashedbeetle/))
