# Configuration
---
There are six aspects that can be configured, all optional:  
1. System wide "static backend" (optional)  
2. Selection of a backend per user (optional)  
3. Import of the "shortlet" into a web browser (optional)  
4. Access control inside each single "Shorty" (implicit)  
5. Enabling of sending of Shortys as SMS (optional)  
6. Installation of the additional tracking plugin (optional)  

---

## 1. System wide "static backend" (optional, for advanced users)
(Requires administrative rights and some technical knowledge)  
This optional setting enables a 'static backend'. That is a backend generating
source urls based on a static base url, thus the name. That base url should be
chosen as short as possible, since this defines the total length of all links
to be posted and used. Most likely the definition of that url requires the
configuration of rewrite rules on the server side. So it is only an option for
experienced administrators with access to the server configuration, be it
centralized or based on decentralized per-directory rules (".htaccess files").
The rules must be configured such that all requests to urls of the scheme
`<base_url><shorty_key>` are mapped directly onto the relay service web url of
the Shorty app: `http://<domain>/<owncloud>/public.php?service=shorty_relay&id=<shorty_key>`.

If in doubt refer to the url shown as "relay url" inside the sharing dialog for
each Shorty. That scheme is the one you want to rewrite your requests to, you
just have to make sure the individual <shorty key> survives the redirect.
The `<shorty_key>` is a string, 6-12 characters long, hard to predict. It is
guaranteed to be unique throughout the system (though in a technically crude
manner...).

There are two variants of such a setup: same domain or cross domain. The first
is easier to setup, the second allows greater flexibility to have a really
short base url, since you can use a shorter domain name compared to the one the
owncloud installation lives on. This is desirable, but more complex.

The perfect situation for the definition of a meaningful static
backend would be a domain with a very short name and configuration access to
something close to the web root. At least you should try to find a setup where
the web path of the ownCloud application is not part of the base url. So that
you get something like `http://<domain_name>/<shorty_key>`.
(Note that the `<shorty_key>` is NOT part of the base url configuration).

Your setup will automatically be verified by Shorty during the setup. Only in
case of a successful verification the setup will be accepted (valid and usable).
Only then the setup is saved and the static backend is offered as an option in
the settings and the user preferences.

---

### 1.a Configuration of a static backend, same domain setup
You need some kind of rewriting rule to map the requests to the short base url
onto the final url of Shortys relay service. Typically a rewriting module for
the http server is used for such a purpose, e.g. mod_rewrite in case of the 
Apache http server. The rule must look something like this:  
```
RewriteEngine  on  
SSLProxyEngine on  
RewriteCond    %{HTTP_HOST}              ^proxy.domain.xyz$ [NC]  
RewriteRule    ^/([A-Za-z0-9]{4,12})$    https://owncloud.domain.xyz/public.php?service=shorty_relay&id=$1 [QSA,L]
```  
Where "proxy.domain.xyz" is the short domain used in the base url setup and
"owncloud.domain.xyz" is the domain where owncloud is delivered from. Different
variants exist obviously, but the basic idea should be clear...

---

### 1.b Configuration of a static backend, cross domain setup
Obviously you need a similar rewriting setup to the first alternative:  
```
RewriteEngine   on  
SSLProxyEngine  on  
RewriteCond     %{HTTP_HOST}            ^proxy.domain.xyz$ [NC]  
RewriteRule     ^/([A-Za-z0-9]{4,12})$  https://owncloud.domain.xyz/public.php?service=shorty_relay&id=$1 [QSA,L,P]  
RewriteRule     ^/(.*)$                 https://owncloud.domain.xyz/$1 [QSA,L,P]  
```
Note the additional 'P' flag in the RewriteRules. This is required (and relies
on the internal proxy module) because otherwise the cross domain setup will be
blocked by most modern browsers due to cross domain security considerations.
In additional you need to whitelist "proxy.domain.xyz" inside your owncloud
configuration. Otherwise owncloud will reject these requests. Whitelisting is
possible by simply adding the domain name inside the global configuration file
config/config.php:
```
'trusted_domains' =>  array (  
    0 => 'owncloud.domain.xyz',  
    1 => 'proxy.domain.xyz',  
  )  
```
The first rule relays the request to the Shorty itself, whilst the second one
is a general relay that allows you to interact using the whitelisted domain.
Without that second rule the relaying service would be crippled and unusable.

---

## 2. Selection of a backend per user (optional)  
To generate the a source url that is part of every Shorty the app uses a
backend. The configuration is done by using a preference option in the personal
preference section of the configuration. You can simply chose one of the
offered backends (combo box). Changing the backend does not affect any
previously generated Shortys. Meaning they stay valid and usable and keep
their previously defined source url which has probably been published.

Different backends are implemented:

### i. "-none-"  
As you have guessed this is something like a "dummy" backend without any
implemented logic. That means the source urls generated are exactly based on
the web url of the Shorty app in your ownCloud system. This is not a very
clever setup, but it certainly works and forward requests.

### ii. "static backend"  
If configured in the system administration, a "static backend" is offered. For
a description see C-1. This backend typically offers shorter source urls, but
its setup requires administrative rights on some http server system.

### iii. online services (url shorteners)  
A few online services are offered as backends to generate short source urls.
Usage of some of those services requires you to open a free account at their
site. Detailed configuration requirements are displayed for the chosen backend.
If you don´t care for details and just want short urls then have a try with the
ti.ny backend. No registration required, reliable service. But keep in mind
you depend on an external service as opposed to using a local static backend.
If that service gets shut down, you published Shortys are broken.

---

## 3. Import of the "Shortlet" into a web browser  (optional)  
Shorty comes with a neat little "Shortlet" offered in the personal preferences
section of ownCloud. It is a "button" you can import into your web browsers
bookmark toolbar or area by a simple drag&drop. The Shortlet should work with
most modern browsers, though probably not all. Just have a try with it:
you click it for any page you want to create a Shorty for. You are forwarded to
your ownClouds Shorty app, the dialog for a new Shorty opens and is prefilled
with the url of the page you came from. Very convenient...

---

## 4. Access control inside each single "Shorty" (implicit)  
There are a few attributres you can configure freely inside each Shorty:
- an arbitrary title shown inside the Shorty app (serves recognition).
- a notes area, maybe you want to write down whom you send that Shorty?
- a status option that controls access permissions to the Shorty.

---

## 5. Sending of Shortys as SMS
Besides sending of Shortys as email message and copying a Shortys source url
to the clipboard a third action can be enabled inside the sharing dialog:
To send a Shortys source url as SMS.

However, the approach is extremely minimalistic, it relies on the client system
to correctly handle a 'sms url'. This is typically only given on a mobile
device, a smart phone. In addition, such url does not allow to specify a message
body, so the Shortys source url has to be copied and pasted manually.
The option is disabled by default.

---

## 6. Installation of the optional tracking plugin
There is a separate app available: "Shorty Tracking". The app acts as a plugin
to the main 'Shorty' app, meaning it requires the main app to be installed and
enhances that with additional features. When installed additional details about
each single request to an existing Shorty are stored and can be visualized.
This helps maintaining a collection of Shortys and offers a more detailed
insight into how those Shortys are used. Keep in mind however that tracking
users raises legal issues. You probably have to inform users about the fact
that their clicks are tracked if your site does not serve a private purpose
only.

---
