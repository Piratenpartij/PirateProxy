PirateProxy
===========

Welcome to PirateProxy, a Python-based generic HTTP/HTTPS web proxy. This
proxy works in a different way than standard HTTP proxies such as Squid and
Varnish. Users do not need to configure their browser proxy configurations.
Instead, this proxy can be visited like any other website. For example, if
this proxy is installed as 'http://proxy.example.org', users can go to that
URL with their browser and enter the site they want to proxy. The proxy then
requests that site and ensures that users continue to browse through the
proxy. The proxy uses URLs such as 'http://proxy.example.org/google.com'.

This proxy supports HTTP and HTTPS and can be used to visit both IPv4 and IPv6
web sites, although it is not possible to surf to IPv6 IP addresses.

The proxy is written in Python, but uses a HTML parser written in C. Possibly
it can be made to run on Windows, but so far only Linux and FreeBSD have been
tested and are known to work.

Be aware that this is still alpha-quality software which was written in a
short time period. 



Rationale
---------

This proxy was written by the Dutch Pirate Party <https://piratenpartij.nl/>
in response to increasing censorship in the world and in The Netherlands to
allow users to access otherwise blocked websites using the proxy. 

Although there is other software serving this purpose available, the
scalability and performance of these solutions was found to be insufficient
and needed features were not available.



Dependencies
------------

PirateProxy depends on a few software packages. 

For the Python-based Proxy itself:
 * Python (2.7 works)
 * The `ipy' module

For the C-based HTML parser and Python wrapper:
 * Cython



Installation
------------

Install the dependencies first. To be able to build the HTML parser used by
the proxy, you will need to have a working C compiler, autoconf, automake and 
Cython.

First, build and install the streamhtmlparser:
`$ cd streamhtmlparser`
`$ ./configure`
`$ make`

As root:
`# make install`

And the Python wrapper:
`$ cd ../streamhtmlparser/src/py-streamhtmlparser`
`$ make`

As root:
`$ make install`

Create a user with limited rights to run the proxy as and copy the PirateProxy
sub-directory to your prefered location.

NOTE: The HTML parser library and Python wrapper are installed in /usr/local.
On some systems this appears not to work out-of-the-box. A possible fix
is:
`# cp /usr/local/lib/python2.7/dist-packages/* /usr/lib/python2.7/dist-packages`
`# ldconfig -v`



Configuration
-------------

All configuration is done through the proxy.conf configuration file. A default
configuration file is supplied. 

The most important settings are:

`http_listen_port`, `https_listen_port`: The HTTP and HTTPS ports the proxy
will listen on. You can configure the listen ports to be 80 and 443 and let
users access the proxy directly. You can also place a reverse proxy in-front
of the PirateProxy. In that case, the listen ports should be different.

`http_port`, `https_port`: These are the ports the proxy is reachable on from
the outside. They may be different from the listen addresses, for example in
case  of a reverse proxy in front. 

`https_certificate`: A PEM file that contains the PirateProxy SSL certificate,
if necessary an intermediate CA certificate and the private key.

`hostname`: The hostname the Pirate Proxy is reachable on.
`proxy.example.com IN A 192.0.2.15`
`*.proxy.example.com IN A 192.0.2.15`

`block_robots`: If you want to ensure that web spiders/crawlers do not try to 
crawl the entire internet through your proxy, you can set this setting to
`yes`.

Under the section `[rewrites]` you can add custom rewrites for specific 
hostnames. The syntax is hostname=newhostname. For instances, if you want to
ensure that `http://www.example.org/` does not pass through the proxy,
you could create the following rewrite: `www.example.org=www.example.org`

You can also rewrite a specific hostname to pass through a different proxy:
`www.example.org=www.example.org.proxy2.example.com`

As a final configuration step, you will need to edit the `index.html` file
found in the `html` directory. Change the example hostname found in that
file to your selected hostname. Of course you can also apply style changes,
put a different logo on the page, etcetera.



Execution
---------

To execute the program, run `./Proxy.py -c proxy.conf`. For now, the proxy
does not daemonize and you are responsible for writing a start-up script.



Contribute
----------

You can contribute in two ways: by e-mailing a patchfile or by using Git. Git
has our preference because of the faster workflow.

*Through patches*: Make your changes and create a patchfile with `diff` and
`patch`. You can mail this to <ict@piratenpartij.nl>.

*Through Git(Hub)*: Are you not familiar with Git, and have you got 15
minutes? Take a look at <http://try.github.io> for a crash course in basic
Git. You can also ask the ICT-team for help, see below for contact
information.

(Note: the below instructions assume command line Git - if you prefer, there
are also numerous GUI-clients available for every OS.)

1. Create an account with GitHub if you do not already have one.
2. Fork this repository to your own account.
3. Clone your fork:
   `git clone https://github.com/%yourUsername%/PirateProxy.git`.
4. Optionally (but recommended), create a new breanch to work in:
   `git checkout %nameOfYourBranch%`.
5. Make your changes in your favourite editor.
6. Add the changed files to your next commit: `git add relative/path/to/file`.
7. Commit your changes: `git commit -m "%short summary%""`.
8. Push your changes to your fork: `git push`.
9. Satisfied? Send a pull request to this repository!

Need help? See below for contact information.

Branches
--------

This repository has two important branches: `master` and `current-stable`.
`master` is the development branch, pull requests and commits should always be
sent to this branch. `current-stable` equals the latest stable release.



License
-------

This software is distributed under the two-clause BSD license. 



Contact
-------

 * Mail: <ict@piratenpartij.nl>
 * IRC: <ircs://nl.pirateirc.net:994/#ppnl-irc>

You can also look at the ICT-subforum at <https://forum.piratenpartij.nl>
