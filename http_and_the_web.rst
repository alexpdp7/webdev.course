HTTP and the Web
================

In the `basic HTML <basic_html>`_ section we have discussed the HTML format,
but we have just viewed our HTML files by opening files in our computer in a
web browser. This obviously does not allow others to access them.

The objective on this course is to publish our web developments in the
World-Wide Web. To do that, we upload them to a web server which exposes them
via the HTTP protocol.

The Hypertext Transfer Protocol, or HTTP for short has a simple
request-response operation (there are extensions and further complexity to
this, but we better ignore them for now). When we type an URL in our browser
such as http://webdevcourse.readthedocs.org/, our browser identifies the
webserver for the webdevcourse.readthedocs.org domain and sends a request for
the specified URL; the web server replies with the content of that URL.

You can use the `curl <https://curl.haxx.se/>`_ tool and equivalent tools to
reproduce and inspect what the web browser does. On a command-line with curl
installed::

 $ curl http://webdevcourse.readthedocs.org
 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
 <title>Redirecting...</title>
 <h1>Redirecting...</h1>
 <p>You should be redirected automatically to target URL: <a href="/en/latest/">/en/latest/</a>.  If not click the link.

This performs the basically the same operation that your browser does when you
type in an URL, and shows you the resulting returned document (in this case, a
redirection to the proper URL for the document).

If you use the `-v` option, `curl` will show you the internal details of the
request::

 $ curl -v http://webdevcourse.readthedocs.org
 * Rebuilt URL to: http://webdevcourse.readthedocs.org/
 * Hostname was NOT found in DNS cache
 *   Trying 162.209.114.75...
 * Connected to webdevcourse.readthedocs.org (162.209.114.75) port 80 (#0)
 > GET / HTTP/1.1
 > User-Agent: curl/7.38.0
 > Host: webdevcourse.readthedocs.org
 > Accept: */*
 > 
 < HTTP/1.1 302 FOUND
 * Server nginx/1.4.6 (Ubuntu) is not blacklisted
 < Server: nginx/1.4.6 (Ubuntu)
 < X-Deity: web02
 < X-Served: Flask
 < Content-Type: text/html; charset=utf-8
 < Date: Sat, 20 Feb 2016 11:45:59 GMT
 < Location: http://webdevcourse.readthedocs.org/en/latest/
 < X-Redirct-From: Flask
 < Connection: keep-alive
 < Content-Length: 229
 < 
 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
 <title>Redirecting...</title>
 <h1>Redirecting...</h1>
 * Connection #0 to host webdevcourse.readthedocs.org left intact
 <p>You should be redirected automatically to target URL: <a href="/en/latest/">/en/latest/</a>.  If not click the link.

Let's break down that in parts (we will omit some things which are not
important now)::

 * Rebuilt URL to: http://webdevcourse.readthedocs.org/
 * Hostname was NOT found in DNS cache
 *   Trying 162.209.114.75...
 * Connected to webdevcourse.readthedocs.org (162.209.114.75) port 80 (#0)

; this is just curl location the appropriate web server; while we humans prefer
to use names such as webdevcourse.readthedocs.org, computers internally refer
to internet systems by their IP address; the first step is converting the
domain name to the IP address using DNS. The next bit::

 > GET / HTTP/1.1

is basically our HTTP request; `curl` is requesting (GETting) the
document in the `/` URL  using the HTTP 1.1 protocol. That is follwed by::

 < HTTP/1.1 302 FOUND
 < Content-Type: text/html; charset=utf-8
 < Location: http://webdevcourse.readthedocs.org/en/latest/
 < Content-Length: 229
 < 
 <!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 3.2 Final//EN">
 ...

This is the response from the web server; it says that it has FOUND a document
(which is specifies with the numerical code 302) and confirms the use of the
HTTP 1.1 protocol. It says that the content at that URL is HTML in the UTF-8
charset and is 229 bytes long.

Then it follows it with the actual content.

Forms
-----

HTTP and HTML combine to create the most simple mechanism for user interaction
in web pages. With what we have seen until now, we can only publish a series of
URLs with links between them and have the user jump from one to other, which
is a very limited form of interaction.

The next step to links is web forms. A form is constructed in an HTML page with
the use of `form`, `input`, etc. tags. We can create text input boxes,
dropdowns, checkboxes, etc. and those forms can be "submitted" (by clicking on
a submit button, for instance). When a form is submitted, the browser makes an
HTTP request like when you click on a link to request a new page, however the
information contained in the form is sent in the HTTP request.

The web server can then process the information in the form and do something
with it. For instance, with the following HTML fragment::

 <form action="http://foo.bar/process.form/">
   <label> Type your name: <input name="name" type="text"/></label>
   <input type="submit">Tell me your name</input>
 </form>

You will get a form with a text box so you can enter your name and a submit
button to send the information to the http://foo.bar/process.form/ URL.

There, the web server will receive the name you have typed in the form and it
can do whatever it wants with it- it can store a list of all the people who
have sent their names, reply to the request with an HTML document saying
"greetings, {your name here}", etc.

AJAX and XMLHttpRequest
-----------------------

Forms are a very basic, but quite powerful means of interaction with web
pages; many useful things can be built with using them. However, around the
year 2000, web developers experimented with Javascript to create more
sophisticated ways to interact with the user. This culminated in the creation
and standardization of the `XMLHttpRequest` Javascript object, which allows
Javascript code to make HTTP requests not tied to a link or a form.

Using `XMLHttpRequest` we can do stuff such as:

* When the user reaches the bottom of a page (containing, for instance, a list
  of articles), request more articles and append them to the end, creating the
  illusion of an infinite list of articles.
* Link to content or submit forms without triggering a full-page refresh in the
  browser for faster and seamless interaction.
* Update information in the page live without reloading and losing context.

and many other things. These techniques became known as `AJAX` (asynchronous
Javascript and XML), although really neither `XMLHttpRequest` or `AJAX` usually
involve XML nowadays.

Single-page apps
----------------

The next iteration after AJAX was completely abandoning the traditional links
and forms loading new pages in the browser and just load a single page in the
browser and perform all further interactions with the user via Javascript and
`XMLHttpRequest`, with no further reloads, thus providing in theory a smoother
user experience and more sophisticated forms of interactions.

The poster child of this movement was Google's GMail web interface which
brought a new era of sophisticated web applications, free from the limitations
of the previous generation and allowing a richer experience, equalling that
of native, traditional desktop applications, while having the advantages of
the web.
