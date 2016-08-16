#### [⇐ Previous](./9-jquery-fundamentals.md) | [Table of Contents](./../readme.md) | [Next ⇒](./11-ajax-with-jquery.md)

# How the Web Works

### Objectives:

By the end of this chapter - you should be able to

- Describe what HTTP and other common protocols are as well as their use cases
- Explain at a high level what happens when you press enter at google.com
- Diagram the request/response cycle and how information is sent from both parties

### Introduction

As we continue to talk about additional concepts in web development, it becomes increasingly important from a high level to understand what is going on.

code.org has a great video series to get you started
[here](https://www.youtube.com/watch?v=ZhEf7e4kopM&index=1&list=PLzdnOPI1iJNfMRZm5DDxco3UdsFegvuB7). Watching all of the videos in this series will give you a great idea of what IP, DNS, HTTP are and how the internet works from a very high level.

### Key Definitions/Terms (From Wikipedia + MDN)

`IP` - The Internet Protocol (IP) is the principal communications protocol in the Internet protocol suite for relaying datagrams across network boundaries. Its routing function enables internetworking, and essentially establishes the Internet. Each computer (known as a host) on the Internet has at least one IP address that uniquely identifies it from all other computers on the Internet.

`TCP` - The Transmission Control Protocol manages sending and recieving of all packets. It guarantees the reliability of our data and ensures that packets are recieved and sent properly. 

`DNS` - Domain Name System is a naming system for computers, services, or any resource connected to the Internet or a private network.

`URL` - Uniform Resource Locator (URL), commonly informally termed a web address (which term is not defined identically)[1] is a reference to a web resource that specifies its location on a computer network and a mechanism for retrieving it. 

`HTTP` - The Hypertext Transfer Protocol (HTTP) is an application protocol for distributed, collaborative, hypermedia information systems. HTTP is the foundation of data communication for the World Wide Web. Hypertext is structured text that uses logical links (hyperlinks) between nodes containing text.

`HTTPS` - HTTPS (also called HTTP over TLS HTTP over SSL and HTTP Secure) is a protocol for network security over a computer network which is widely used on the Internet. HTTPS consists of communication over Hypertext Transfer Protocol (HTTP) within a connection encrypted by Transport Layer Security or its predecessor, Secure Sockets Layer. 

`HTTP Header` - HTTP header fields are components of the header section of request and response messages in the Hypertext Transfer Protocol (HTTP). They define the operating parameters of an HTTP transaction. If we think of a request like an envelope (or packet) being sent somewhere - we can think of a header as the information written on the envelope and the data being inside the envelope. You can read more about headers [here](http://code.tutsplus.com/tutorials/http-headers-for-dummies--net-8039)

`Request / Response Cycle` - Request–response, or request–reply, is one of the basic methods computers use to communicate with each other, in which the first computer sends a request for some data and the second computer responds to the request. Usually, there is a series of such interchanges until the complete message is sent; browsing a web page is an example of request–response communication. Request–response can be seen as a telephone call, in which someone is called and they answer the call. You can read more about it [here](https://en.wikipedia.org/wiki/Request%E2%80%93response)

`Cookies` - An HTTP cookie (also called web cookie, Internet cookie, browser cookie or simply cookie) is a small piece of data sent from a website and stored in the user's web browser while the user is browsing. Cookies are sent to the browser from a server using the `Set-Cookie` header and are sent by the browser to the server using the `Cookie` header.

`HTTP Verbs` - The HTTP verbs comprise a major portion of our “uniform interface” constraint and provide us the action counterpart to the noun-based resource. The primary or most-commonly-used HTTP verbs (or methods, as they are properly called) are POST, GET, PUT, PATCH, and DELETE.

`cURL` - cURL is a computer software project providing a library and command-line tool for transferring data using various protocols. You can get started with a nice tutorial [here](https://gist.github.com/caspyin/2288960)

### Parts of a URL (from MDN)

As we start thinking about sending data to servers, one of the common ways of doing that is through parts of a URL - specifically the query string. Let's examine the parts of a URL - let's examine this url: `http://www.rithmschool.com:80/users?first=elie&second=tim#main`

- Protocol - `http`. http:// is the protocol. It indicates which protocol the browser must use. Usually it is the HTTP protocol or its secured version, HTTPS. The Web requires one of these two, but browsers also know how to handle other protocols such as mailto: (to open a mail client) or ftp: to handle file transfer, so don't be surprised if you see such protocols.

- Domain Name - `www.rithmschool.com` is the domain name. It indicates which Web server is being requested. Alternatively, it is possible to directly use an IP address, but because it is less convenient, it is not often used on the Web.

- Port - `:80` is the port. It indicates the technical "gate" used to access the resources on the web server. It is usually omitted if the web server use the standard ports of the HTTP protocol (80 for HTTP and 443 for HTTPS) to grant access to its resources. Otherwise it is mandatory.

- Path - `/users` is the path to the resource on the Web server. In the early days of the Web, a path like this represented a physical file location on the Web server. Nowadays, it is mostly an abstraction handled by Web servers without any physical reality.

- Parameters - `first=elie&second=tim` are extra parameters provided to the Web server. Those parameters are a list of key/value pairs separated with the & symbol. The Web server can use those parameters to do extra stuff before returning the resource. Each Web server has its own rules regarding parameters, and the only reliable way to know if a specific Web server is handling parameters is by asking the Web server owner. The portion of the URL that begins with a `?` is known as the `query string`

- Anchor - `#main` is an anchor to another part of the resource itself. An anchor represents a sort of "bookmark" inside the resource, giving the browser the directions to show the content located at that "bookmarked" spot. On an HTML document, for example, the browser will scroll to the point where the anchor is defined; on a video or audio document, the browser will try to go to the time the anchor represents. It is worth noting that the part after the #, also known as fragment identifier, is never sent to the server with the request.

[https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL](https://developer.mozilla.org/en-US/Learn/Common_questions/What_is_a_URL)

[https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier#Syntax)

### Exercise

Complete the [How The Web Works Exercise](https://github.com/rithmschool/prework_exercises/tree/master/how_the_web_works_exercise)

### Additional Resources

[https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser](https://www.quora.com/What-are-the-series-of-steps-that-happen-when-an-URL-is-requested-from-the-address-field-of-a-browser)

[https://github.com/alex/what-happens-when](https://github.com/alex/what-happens-when)

#### [⇐ Previous](./9-jquery-fundamentals.md) | [Table of Contents](./../readme.md) | [Next ⇒](./11-ajax-with-jquery.md)