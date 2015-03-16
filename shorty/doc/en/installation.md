# Installation
-----

This package is a plugin for the ownCloud web application ("ownCloud app").


There are two ways of installation: automatic and manual


## Automatic installation:
-----
You need login to your ownCloud using an account with administrative rights.
Open the 'Apps' section of the adminstration and select 'shorty', enable it.
Then go on with the [basic configuration](configuration.md).


## Manual installation:
-----
Download the package from here:
(http://apps.owncloud.com/content/show.php/Shorty?content=150401)
Create a subfolder 'shorty' in the "apps" subfolder of your ownCloud web root.
Unpack the contents of the package into the new folder 'shorty'.
Now load ownCloud in your favorite web browser and login with an administrative
account.
Enable the plugin in the "Apps" section of the configuration ("*") inside
ownClouds web gui (requires admin rights).


TODO: Diesen Teil nach shorty_admin.md verschieben oder löschen:
* Basic configuration steps for BOTH types of installation:
The "Admin" section of the configuration allows to configure a base url to
enable usage of the static backend (see [USAGE](shorty_usage.md)).

The "Preferences" section of that configuration now offers two elements:
- a "Shortlet" to be dragged to the browsers bookmark area (see USAGE).
- a configuration of a backend to use to generate source urls (see USAGE).
The main part of the plugin can be accessed in the navigation menu as "Shorty".


***
Have fun !
