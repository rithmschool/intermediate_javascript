#### [⇐ Previous](./02-intermediate-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-ajax-with-jquery.md)

# How the Web Works

### Objectives:

By the end of this chapter, you should be able to:

- Describe each component of a url
- Explain at a high level what happens when you type `https://www.rithmschool.com` into your browser's url bar and press enter
- Identify the format of the HTTP request and response
- Identify the following components of an http request or response:
	- http verb
	- path
	- headers
	- body
	- status code


### Introduction

As we continue to talk about additional concepts in web development, it becomes increasingly important to understand what is going on from a high level. The goal of this chapter is to give you a strong fundamental understanding of how the internet actually works.

code.org has a great video series to get you started
[here](https://www.youtube.com/watch?v=ZhEf7e4kopM&index=1&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7). Watching all of the videos in this series will give you a great idea of what IP, DNS, and HTTP are, and how the internet works from a very high level.


### Components of a URL

A url has different components that we must be able to identify as web developers.  Let's look at the following example to talk about each component:

__`http://www.rithmschool.com:80/users?first=elie&second=tim#main`__

- Protocol - `http`. http:// is the protocol (a well defined way of exchanging information).  Http as well as https is the most common way of exchanging information on the internet using your browser.

- Domain Name - `www.rithmschool.com` is the domain name.  A domain name is a human readable address for your website.  However, your computer must look up an _IP address_ using a service called DNS in order to communicate with the correct computer on the internet.

- Port - `:80` is the port.  A port establishes a logical pathway for information to travel on a computer.  It allows different programs on the computer to run on different ports. A valid port number can be 1 through 65535.   Typically we do not have to specify the port when using websites.  Sites with the protocol `http` default to port 80 and sites with the protocol `https` default to port 443.

- Path - `/users` is the path to the resource on the Web server.  A path may relate to a file that the web server has in its hard drive or an abstract resource that the web server is able to create, read update or delete.

- Parameters - `first=elie&second=tim` are extra parameters provided to the Web server. Parameters are a list of keys and values.  In the above example, `first` is a key and `second` is a key. `elie` is the value for `first` and `tim` is the value for `second`.  These keys can be used by the web server to change the http response returned in some way.  Each key value pair in the url is separated by a `&`.  To indicate that the parameter string is starting, a url has a `?`. 


- Anchor - `#main` is an anchor to another location on the page. An anchor represents a place on the page that the browser should scroll down to.  With modern web pages, some applications have began to use the anchor tag as a way to remember which content to display.

### Important Definitions

- __Protocol__ - An explicitly defined form of communication or procedure.  The internet is built on protocols which strictly define how information is sent and received on the web.  Some example protocols are TCP, IP, UDP, HTTP, FTP and POP3.
- __HTTP__ - Hyper Text Transfer Protocol is a well defined client server protocol which allows the client to get data from the server or set data on the server.  It is the protocol in use for every web site you visit online.
- __IP Address__ - The Internet Protocol (IP) is the protocol that governs how data is sent across a network from one computer to another. Every computer has at least one IP address which is a unique identifier of that computer on the web.
- __DNS__ - Domain Name System is a server that takes a domain name like `https://www.rithmschool.com` as input and returns an ip address as output. This is necessary for the web because computers can only make connections to ip addresses.
- __HTTP Status Code__ - A numeric code that indicates the result of the HTTP request.  Generally speaking 200 statuses are success, 300's are redirects, 400's are client side errors, and 500's are server side errors.
- __DOM__ - The Document Object Model is a in memory representation of the website that can change as the user interacts with the page.  This is different than the html response that was sent by the browser since the DOM changes on the client side as the browser takes actions on the page.
- __Idempontent__ - Refers to an operation that can be repeated many times on a set of data and the state of the set of data will not change.  This concept is applicable to the HTTP spec since operations like a GET request should be idempotent but a POST request is not.

### HTTP Request and Response

At a high level, the HTTP spec involves a computer making a request for some information at a url.  The request goes to a web server (another computer) somewhere on the internet.  The web server handles that request and then sends back the appropriate data to the requesting computer.

Taking the example of `https://www.rithmschool.com`, let's take a look at what happens in a little more detail.  What happens when you type that address in your browser window?  What are the steps that bring you back a viewable web page?

1. __DNS Lookup__ - The browser uses a system called a domain name server which translates human readable urls like `https://www.rithmschool.com` into an __ip address__.  Once the browser has an ip address, it can make a connection to the server.
2. __HTTP GET Request__ - Using rithm school's ip address, the browser makes a HTTP GET request to the server.  The GET request contains the path, in this case `/`.  We will talk about the GET request in more details later.
3. __Server Receives Request__ - The Rithm School server receives the GET request and generates the appropriate HTML page.
4. __HTTP Response__ - The HTML page is sent back to the requesting computer (the browser) via a HTTP response message.  The response contains a status code, in this case `200`, and a response body.  The body of the HTTP response is the html text that will be displayed in the browser.
5. __Browser Creates DOM__ - The browser will read the body of the HTTP response.  The browser reads the response body from top to bottom.  Starting at the `<!doctype html>` tag, the browser reads the response body line by line and creates an in memory representation of the html document called the DOM.
6. __GET Requests for Images/Scripts/CSS__ - While the browser is creating the DOM, it may encounter tags that reference other files.  For example, a script tag like `<script src="main.js">` is a reference to another file on the server.  In order to use the resource, the browser must make additional GET requests.

### HTTP Format

Now let's understand the HTTP request and response in more detail.  Here is a sample http request:

__curl command__

```
curl -v 'https://www.rithmschool.com/' \
     -H 'accept-encoding: gzip, deflate, sdch, br' -H 'accept-language: en-US,en;q=0.8' \
     -H 'upgrade-insecure-requests: 1' \
     -H 'user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36' \
     -H 'accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8' \
     -H 'authority: www.rithmschool.com' --compressed
```


__HTTP Request__

```
GET / HTTP/1.1
Host: www.rithmschool.com
accept-encoding: gzip, deflate, sdch, br
accept-language: en-US,en;q=0.8
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 10_11_6) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/54.0.2840.71 Safari/537.36
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/webp,*/*;q=0.8
authority: www.rithmschool.com
```

__HTTP Response__

```
HTTP/1.1 200 OK
Date: Wed, 26 Oct 2016 14:38:55 GMT
Content-Type: text/html; charset=utf-8
Transfer-Encoding: chunked
Connection: keep-alive
Set-Cookie: __cfduid=dc463a8265a523ae34244e656361ef2a11477492735; expires=Thu, 26-Oct-17 14:38:55 GMT; path=/; domain=.rithmschool.com; HttpOnly
X-Frame-Options: SAMEORIGIN
X-Xss-Protection: 1; mode=block
X-Content-Type-Options: nosniff
Cache-Control: max-age=0, private, must-revalidate
Set-Cookie: _rithm-app_session=RlZ3T2F3Q0ZMYmFIRVBXeVJQMUZzbkY3R1lVSjNqSUxiL3JJR0t6NWZqWFdwdk9XanJzek9HZzBvb1RUUkN4U05ZWmZTTUttb045aUlLT1FmSUlySmYyalQwWXZYVnJtNFNYU1lYUTdIcW1wMnZxb0tSQ3dobXF3OFYvU3Y3RjhhMUEyMlRKZ2ZMOSs5dWdBbnJoWU1RPT0tLTRSWnRWRSttR3dDNlBFQUxuaXl4OGc9PQ%3D%3D--163a39a41af7d0f73718aee89bb9e52a603244bd; path=/; secure; HttpOnly
X-Request-Id: 8077930d-ab0c-44e2-95d0-8cf8b7bb7e34
X-Runtime: 0.008792
X-Rack-Cache: miss
Strict-Transport-Security: max-age=31536000
Via: 1.1 vegur
Server: cloudflare-nginx
CF-RAY: 2f7ea79b7e021e8f-SJC
Content-Encoding: gzip
 
<!DOCTYPE html>
<html lang="en">
<head>

  <meta property="og:image" content="https://www.rithmschool.com/assets/logos/300logo-eca4f23104b6dd23001f41f285105acac06e287a58befab70bad4d0c2fe86ebd.jpg"/>
  <meta charset="utf-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1">
  <meta name="description" content="Spend 4 months learning JavaScript, Python, and React with 4-12 other students. Learn quickly with experienced instructors. Next class Jan 16">
  <meta name="author" content="Rithm School">

  <title>Rithm School</title>

  <link rel="stylesheet" media="all" href="/assets/application-6c02ccab94eb3d1ef6ca11b543a6637fbe24039d6ec8b79eba0313ca992ac414.css" />
  <link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/jquery.slick/1.6.0/slick.css"/>
  <!-- Add the slick-theme.css if you want default styling -->
  <link rel="stylesheet" type="text/css" href="//cdn.jsdelivr.net/jquery.slick/1.6.0/slick-theme.css"/>

  <!-- Add a stylesheet tag for the name of the action -->
  <link rel="stylesheet" media="screen" href="/assets/main-ab69d4dadb064fcdbcfaa9df78b072f80633f59c5b2cced806ebde8a2f369884.css" />
  <link rel="stylesheet" media="screen" href="/assets/home-c2fad1776c2eba2f54b7e044395c569145a3771a7aaf1face93bbb7c954b7030.css" />

  <script src="/assets/application-501b04d21fa181e77f89e937da573db0747fdb888af1a319d7919a4ef67d5c10.js"></script>
  <script src="/assets/modernizr-43ece90ce3662a6bd3b00d7acdddd63c4a965f96d0b7e2b9738988f559971b3f.js"></script>

  <link rel="shortcut icon" type="image/x-icon" href="/assets/favicon-eb41ae8905bdd2b439d03b1628aaa345c177848bb8e6c36ea8c5a56cf03ef757.ico" />
.
.
.
```


Notice the format of the request and the response.  The HTTP request has the following format:

```
Request-Line
1 or more headers
CRLF
[ message-body ]
```

and the response has the following format:

```
Status-Line
1 or more headers
CRLF
[ message-body ]
```

### HTTP Verbs

* GET - An idempotent operation that is designed for getting data from the server.
* POST - A non idempotent operation that can create data on the server or otherwise modify data.
* PUT - Technically idempotent, but commonly used for updating data that already exists on the server
* PATCH - Non idempotent and also used for modifying data on the server
* DELETE - Deletes data from the server.  Not idempotent.

There are other verbs allowed in the HTTP spec but we do not use them often as web developers.  The other verbs are listed for reference:

* HEAD
* TRACE
* OPTIONS
* CONNECT


### HTTP Status Codes

The http status codes tell the client the result of making a HTTP request to the server.  The general category of status codes is as follows:

* __200's__ Success! Your request was successfully processed by the server
* __300's__ Redirect! The server will return another url for which the client should make a GET request to.
* __400's__ Client Error! The client made a request that the server decided was invalid for some reason.  This could be a get request to a resource that doesn't exit or a post request that was missing required information
* __500's__ Server Error! The client made a request to the server and the server could not fulfill the request because of some problem on the server side.  This happens frequently if a website is down and cannot respond to requests or if there is an error in your server code.



### Exercise

Complete the [How The Web Works Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/how_the_web_works_exercise)

### Additional Resources

[https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser](https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser)

[https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)

[https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL](https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL)

[https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

#### [⇐ Previous](./02-intermediate-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-ajax-with-jquery.md)