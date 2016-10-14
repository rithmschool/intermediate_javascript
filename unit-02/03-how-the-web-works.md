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

### Parts of a URL (from MDN)

As we start thinking about sending data to servers, one of the common ways of doing that is through parts of a URL - specifically the query string. Let's examine the parts of a URL with the following example: `http://www.rithmschool.com:80/users?first=elie&second=tim#main`

- Protocol - `http`. http:// is the protocol. It indicates which protocol the browser must use. Usually it is the HTTP protocol or its secured version, HTTPS. The Web requires one of these two, but browsers also know how to handle other protocols such as mailto: (to open a mail client) or ftp: to handle file transfer, so don't be surprised if you see such protocols.

- Domain Name - `www.rithmschool.com` is the domain name. It indicates which Web server is being requested. Alternatively, it is possible to directly use an IP address, but because it is less convenient, it is not often used on the Web.

- Port - `:80` is the port. It indicates the technical "gate" used to access the resources on the web server. It is usually omitted if the web server use the standard ports of the HTTP protocol (80 for HTTP and 443 for HTTPS) to grant access to its resources. Otherwise it is mandatory.

- Path - `/users` is the path to the resource on the Web server. In the early days of the Web, a path like this represented a physical file location on the Web server. Nowadays, it is mostly an abstraction handled by Web servers without any physical reality.

- Parameters - `first=elie&second=tim` are extra parameters provided to the Web server. Those parameters are a list of key/value pairs separated with the & symbol. The Web server can use those parameters to do extra stuff before returning the resource. Each Web server has its own rules regarding parameters, and the only reliable way to know if a specific Web server is handling parameters is by asking the Web server owner. The portion of the URL that begins with a `?` is known as the `query string`.

- Anchor - `#main` is an anchor to another part of the resource itself. An anchor represents a sort of "bookmark" inside the resource, giving the browser the directions to show the content located at that "bookmarked" spot. On an HTML document, for example, the browser will scroll to the point where the anchor is defined; on a video or audio document, the browser will try to go to the time the anchor represents. It is worth noting that the part after the #, also known as fragment identifier, is never sent to the server with the request.

### Exercise

Complete the [How The Web Works Exercise](https://github.com/rithmschool/intermediate_js_exercises/tree/master/how_the_web_works_exercise)

### Additional Resources

[https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser](https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser)

[https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)

[https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL](https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL)

[https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

#### [⇐ Previous](./02-intermediate-jquery.md) | [Table of Contents](./../readme.md) | [Next ⇒](./04-ajax-with-jquery.md)