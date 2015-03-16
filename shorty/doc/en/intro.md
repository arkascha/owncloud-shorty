# Introduction
-----

## Description  

A new entry (a "Shorty"), can be created either manually by entering a URL ("New shorty") or by simply clicking the "Shortlet" whenever you visit a page you want to add to your collection. The "Shortlet" is something like a Bookmarklet, a script based bookmark meant to be stored inside the bookmark area of your web browser. When clicked, the currently open page will be added to your collection of Shortys, including a practical shortened URL.


Each Shorty contains a source and a target URL. The source URL can be used to be posted in forums, sent by email or whatever. It is typically much shorter than the full blown target URL on the web, though this may depend on which shortening service you´re using as a "backend". Basic control is implemented to define who can access your Shortys. They can be set 'private', 'shared', 'public' or 'blocked'. They can also be set to expire on a certain date and of course can be removed again. Please consult the [User´s guide](shorty_user.md) for detailed information.

-----

## Current status  

The current status of this package is as follows:


The initial release is definately buggy and contains annoying shortcomings. It has been developed on a Linux system using a Firefox browser, so this is most likely the best working combination. Basic usage test have been made with a few other browsers. The package appears to be working in general though there might be some minor differences in apeparance.


Known shortcomings are:

* only very basic input validation is done.  
* access control is not yet fine grained, no user or group support exists.  
* markup, scripting and style definitions are the work of a web app newbie.  
**Please excuse any inconvenience.**
