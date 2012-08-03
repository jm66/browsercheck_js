BrowserCheck 2.8

Author: Shannon Meisenheimer
        meisenheimer@ucmo.edu

Files included:
- index.html
- check.js
- plugins.js
- popup.html
- PlatformCheck.class
- images\pass.gif
- images\fail.gif
- images\warning.gif

_______________________________________________________________________________________________

The BrowserCheck package utilizes a combination of Java, Javascript, html and a tiny bit of VBscript
to run some checks on the visitor's browser.  We have been using it so our users can determine if their
browser configuration is compatible with our Blackboard platform, but it can be used to test 
for compatibility with any web-based system.

You will, at a minimum, need to modify portions of the Javascript files to meet what you deem 
"end-user compatibility" for your system.  The index.html file may need some modification in terms
of visual layout/design.  I have provided a skeletal layout to get you started.  You may need to 
add to or modify the .html structure and add some CSS to fit your institution's look and feel.

All the files included need to reside in the same directory on your webserver.

The check.js file has all the script needed for reporting/checking:
- Browser and version
- Platform (OS) and version
- Javascript
- Pop-blocker
- Cookies
- Images (through port 80)

The plugins.js file has all the script needed for reporting/checking for the presence of:
- Flash player plugin
- Shockwave player plugin
- Adobe Reader plugin
- Apple QuickTime player plugin
- RealPlayer plugin
- Windows Media Player plugin
- Silverlight plugin

The PlatformCheck.class file detects the version and vendor of the Java Plugin.  I don't believe
this works for Microsoft's VM, just Sun and Apple Java.  No modification needed here unless you want
to change how this information is displayed to the user.  This currently just shows the user the 
vendor and version of their Java plugin.  It seemed easier to just report the version
and have the user match it against a minimum requirement.  Javascript detection of the Java plugin also
didn't work very well at the time, not sure if that has changed any or not.  I may revisit it later.

Keep in mind the plugin check is looking for browser plugins, not applications installed on the
system.  Often the browser plugins are installed with the applications, but not always.  I didn't
design this part as a pass/fail check, as we don't require plugins be installed, nor require a 
minimum version.  This just reports if the plugin is installed and attempts to determine the 
version if possible.  At the time of 2.3, Adobe has not yet created a Reader plugin for Firefox on 
Mac OSX, so it reports no plugin installed, even though Adobe Reader may be installed.  I was going
to add an alert warning the user of this if they are using FF on OSX, but I don't really like alerts.

I haven't added any mobile platforms, although some could still be identified, but don't count on 
device identification.

This is checking most common browsers and versions.  I didn't bother checking for anything below 
Netscape 6.0, mostly because the agent strings are very vague and hard to identify.

I've included comments where I thought appropriate to help make modifications and for ease of 
understanding.  You will need some knowledge of html and Javascript to modify the package to
your needs, but it isn't too bad.  I used Javascript over Java for the vast majority of it so
it is easier and more convenient to make changes by others.

I will tell you, I didn't know much Javascript when I started this project.  My background is
more PHP, HMTL and CSS.  I pretty much learned Javascript as I went.  So don't be discouraged
if it doesn't make sense at first.  I've probably added a few years to my age through the 
experience :D


I'll provide some brief instructions on modification to get you started...
_______________________________________________________________________________________________

- To change what constitutes a pass/fail/warning on browser and OS, modify the 
  checkBrowser() function in check.js.
  For example the first case statement (at the time of writing this), is executed for 
  Internet Explorer.  It first evaluates the OS and OS version and then passes through cases
  for each version of IE.  You will see I have IE 8, 7 and 6 throwing a warning for 
  Windows 7.  The default condition is set to fail in the .html file 
  (this would be any IE version lower than 6.0).
  
- With Bb now supporting stable release channels for Firefox and Chrome, you will need to be more
  diligent in updating what versions of these 2 browsers are supported.  At the time of writing 
  this, FF 5.0 and Chrome 12.0 were the stable channel versions (9.1 SP6 was tested on FF 4.0 and 
  Chrome 10.0).I have set the logic to fail anything higher than the current stable release (to 
  catch beta versions).    You may want to instead throw a warning for higher versions.

- As new versions of browsers and operating systems are released they may need to be added.  Some
  of them are forward compatible and others aren't.  It just depends on how the User Agent strings
  are constructed for a given browser/operating system pair.  This would require potential
  changes to not only the checkBrowser() function, but also the 4 switch statements at the
  beginning of the file.

- The other functions in check.js shouldn't need to modified

- The plugin piece is little more complicated to modify.  The scripts used here are defined 
  specifically for each plugin.  You will need a broader understanding of Javascript to make
  changes to this file.

_______________________________________________________________________________________________



Version History:
_______________________________________________________________________________________________

- 1.0 - Checked Browser and version, Platform (OS) and version, Javascript, Pop-blocker, 
        Cookies, Images (through port 80), Java vendor and version.  Evaluated browser
        version and OS, but not OS version.  1.0 relied on several if-else-if statements and 
        used way too many indexOfs.  This slowed it down a bit, but worked great for a first
        attempt.

- 2.0 - Converted all if-else-if's to switch statements which should run faster and I think 
        is easier to read.  I used string assignments when possible (and when seemed efficient) 
        to reduce the use of indexOf.  I also broadened the "checkBrowser" function, so it now 
        can evaluate browser and OS version for compatibility.  This made the function a bit more 
        complex, but allows for finer granularity in what is checked.

- 2.1 - Added Plug-in checks for Flash, Shockwave, Adobe Reader, QuickTime, RealPlayer and
        Windows Media Player.  These do slow it down a bit compared to version 2.0, but I'm currently
        looking at streamlining this a bit to remove redundant processes.

- 2.2 - Optimize plugin checks to speed up script.  I reduced the number of functions for checking 
        plugins from 8 or more to  3.  This sped it up a little more.

- 2.3 - I noticed the plugin checks were slowing down the page loading, especially in IE (because
        of all the AcitveX objects), so I broke the checks into 2 different Javascript files.  
        One file for the browser check, check.js, and one for the plugins, pugins.js.  I now have
        it set up so the user clicks a "Check Plugins" link which kicks off the plugins check, so 
        the initial page load doesn't have to wait for it to process.  As far as I can tell this is
        running faster.  I also added Microsoft Silverlight to the list of what is being checked.

- 2.4 - Updated to include 64-bit version detection for Windows (this doesn't work on Firefox versions
        3.5, or 3.6, but I believe does work in Firefox 4.0).  This is due to the lack of a bit 
        identification in the navigator object for those versions of the browser.  Updated for 
        detection of specific versions of OSX (10.5 and 10.6).  Updated compatibility requirements for 
        Bb v9.1.

- 2.5 - Updated to include compatibility requirements for Bb v9.1SP2 (which includes Safari 5.0).

- 2.6 - Updated to include detection of IE9.

- 2.7 - Updated to include compatibility requirements for Bb v9.1SP6 (which includes Firefox 4.0 & 5.0
        and Chrome 10.0, 11.0, and 12.0).  
		
- 2.8 - Updated to include compatibility requirements for Bb v9.1SP7 (which includes Firefox 8.0
        and Chrome 15.0).  
		

For future versions:
- Optimize browser and OS checks using pattern matching against values stored in a static
  array to further reduce the use of IndexOf.
- Continue to update for new versions of browsers, operating systems and plugins as they are released.

If you make any improvements/additions to this script, please share them with me and I will modify 
this and redistribute.  As I stated before, I'm by no means trained as a Javascript programmer,
nor a technical writer (as you can probably tell from this ReadMe) so I welcome any comments you may 
have for improvement.
_______________________________________________________________________________________________

   

Resource List:
_______________________________________________________________________________________________

I am by no means claiming all the work done here is my own.  All the javascript used is available
on the web, granted, I had to piece it all together...

- http://www.builtfromsource.com/2007/06/26/detecting-plugins-in-internet-explorer-and-a-few-hints-for-all-the-others/#quicktime-player

- http://www.aspfree.com/c/a/Windows-Scripting/Detecting-Plugins-in-Internet-Explorer/

- http://developer.apple.com/internet/webcontent/detectplugins.html

- http://www.streaming-media.info/cnt520.html#ex3

- http://www.xs4all.nl/~ppk/js/flash.html

- http://dithered.chadlindstrom.ca/javascript/quicktime_detect/example.html

- http://www.zytrax.com/tech/web/browser_ids.htm

- http://www.w3schools.com/JS/js_cookies.asp

- http://techpatterns.com/downloads/javascript_cookies.php

- http://www.knallgrau.at/code/plugin_js/demo/plugin-detection

- http://www.oreillynet.com/pub/a/javascript/2001/07/20/plugin_detection.html

- http://www.useragentstring.com/pages/Chrome/

_______________________________________________________________________________________________

I'd like to give special thanks to my UCM colleague, Jim Vanhorn, and my buddy Matt Henley for 
helping me out with some logic and syntax.  They know a bit more Javascript than I.  Hopefully, 
I didn't drive them nuts listening to me complain...

Shannon
meisenheimer@ucmo.edu
