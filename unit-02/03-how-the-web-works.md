#### [⇐ Previous](./02-intermediate-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-ajax-with-jquery.md)

# How the Web Works

### Objectives:

By the end of this chapter, you should be able to:

- Describe what HTTP and other common protocols are as well as their use cases
- Explain at a high level what happens when you type google.com into your browser's url bar and press enter
- Diagram the request/response cycle and describe how information is sent from both parties

### Introduction

As we continue to talk about additional concepts in web development, it becomes increasingly important to understand what is going on from a high level. The goal of this chapter is to give you a strong fundamental understanding of how the internet actually works.

code.org has a great video series to get you started
[here](https://www.youtube.com/watch?v=ZhEf7e4kopM&index=1&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7). Watching all of the videos in this series will give you a great idea of what IP, DNS, and HTTP are, and how the internet works from a very high level.

### Key Definitions/Terms 

`IP`- The Internet Protocol (IP) is the protocol that governs how data is sent across a network from one computer to another. Every computer has at least one IP address which identifies it uniquely.

[Further reading on IP](https://en.wikipedia.org/wiki/Internet_Protocol)

`TCP` - The Transmission Control Protocol (TCP) is the protocol that governs sending and receiving of data in units of information called packets. This protocol ensures that data is transferred reliably across a network.

[Further reading on TCP](https://en.wikipedia.org/wiki/Transmission_Control_Protocol)

`DNS` - Domain Name System (DNS) is a naming system that translates between domain names that are easy to remember (e.g. google.com) and numerical IP addresses.

[Further reading on DNS](https://en.wikipedia.org/wiki/Domain_Name_System)

`URL` - A Uniform Resource Locator (URL) is like a kind of address to a resource on the internet. URLs are also referred to as web resources. https://en.wikipedia.org/wiki/Uniform_Resource_Locator is an example of a URL.

[Further reading on URL](https://en.wikipedia.org/wiki/Uniform_Resource_Locator)

`HTTP` - From MDN:

>Hypertext Transfer Protocol (HTTP) is an application-layer protocol for transmitting hypermedia documents, such as HTML. It was designed for communication between web browsers and web servers, though it can be used for other purposes as well. It follows a classical client-server model, with a client opening a connection, making a request, and waiting until it receives a response. It is also a stateless protocol, meaning that the server does not keep any data (state) between two requests. Though often based on a TCP/IP layer, it can be used on any reliable transport layer, that is a protocol that don't lose messages silently.

[Further reading on HTTP](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol)

`HTTPS` - HTTPS is a protocol for exchanging information securely over a computer network, and is widely used on the Internet. 

[Further reading on HTTPS](https://en.wikipedia.org/wiki/HTTPS)

`HTTP Header` - HTTP header fields are components of the header section of request and response messages in the Hypertext Transfer Protocol (HTTP). They provide some extra information about the HTTP transaction. If we think of a request like an envelope (or packet) being sent somewhere, we can think of a header as the information written on the envelope and the data being inside the envelope. 

[Further reading on HTTP headers](http://code.tutsplus.com/tutorials/http-headers-for-dummies--net-8039)

`Request / Response Cycle` - The request-response (or request-reply) cycle is a pattern computers use to talk to one another; the first computer sends the second a request, and waits for the second computer to issue a response. Every time you hit enter while inside of your browser's URL bar, you are issuing a request and waiting for a response.

[Further reading on the request-response cycle](https://en.wikipedia.org/wiki/Request%E2%80%93response)

`Cookies` - A cookie is a small piece of data sent as part of the request-response cycle. Cookies are initially set by the server (as part of a `Set-Cookie` header), and afterwards are sent from the browser to the server on subsequent requests (using the `Cookie` header).

[Further reading on cookies](https://en.wikipedia.org/wiki/HTTP_cookie)

`HTTP Verbs` - We'll talk much more about HTTP verbs later on. They are used to describe an action that should be executed on a specified resource. The most-commonly-used HTTP verbs (or methods, as they are properly called) are POST, GET, PUT, PATCH, and DELETE. When you type enter in your browser's URL bar, you are issuing a GET request.

[Further reading on HTTP verbs](https://en.wikipedia.org/wiki/Hypertext_Transfer_Protocol#Request_methods)

`cURL` - cURL is a computer software project providing a library and command-line tool for transferring data using various protocols. You can get started with a nice tutorial [here](https://gist.github.com/caspyin/2288960)

### Parts of a URL

A url has different components that we must be able to identify as web developers.  Let's look at the following example to talk about each part:

__`http://www.rithmschool.com:80/users?first=elie&second=tim#main`__

- Protocol - `http`. http:// is the protocol (a well defined way of exchanging information).  Http as well as https is the most common way of exchanging information on the internet using your browser.

- Domain Name - `www.rithmschool.com` is the domain name.  A domain name is a human readable address for your website.  However, your computer must look up an _IP address_ using a service called DNS in order to communicate with the correct computer on the internet.

- Port - `:80` is the port.  A port establishes a logical pathway for information to travel on a computer.  It allows different programs on the computer to run on different ports. A valid port number can be 1 through 65535.   Typically we do not have to specify the port when using websites.  Sites with the protocol `http` default to port 80 and sites with the protocol `https` default to port 443.

- Path - `/users` is the path to the resource on the Web server.  A path may relate to a file that the web server has in its hard drive or an abstract resource that the web server is able to create, read update or delete.

- Parameters - `first=elie&second=tim` are extra parameters provided to the Web server. Parameters are a list of keys and values.  In the above example, `first` is a key and `second` is a key. `elie` is the value for `first` and `tim` is the value for `second`.  These keys can be used by the web server to change the http response returned in some way.  Each key value pair in the url is separated by a `&`.  To indicate that the parameter string is starting, a url has a `?`. 


- Anchor - `#main` is an anchor to another location on the page. An anchor represents a place on the page that the browser should scroll down to.  With modern web pages, some applications have began to use the anchor tag as a way to remember which content to display.  


### Exercise

Complete the [How The Web Works Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/how_the_web_works_exercise)

### Additional Resources

[https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser](https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser)

[https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)

[https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL](https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL)

[https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

#### [⇐ Previous](./02-intermediate-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-ajax-with-jquery.md)