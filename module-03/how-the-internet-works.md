# How the Internet Works

## Opening: Quick history of the web

#### 1969: Arpanet
[Arpanet](https://en.wikipedia.org/wiki/ARPANET) was the first real network to run on packet switching technology (new at the time). On the October 29, 1969, computers at Stanford and UCLA connected for the first time. In effect, they were the first hosts on what would one day become the Internet.

#### 1969: Unix
Another major milestone during the 60′s was the inception of [Unix](https://en.wikipedia.org/wiki/Unix): the operating system whose design heavily influenced that of Linux and FreeBSD (the operating systems most popular in today’s web servers/web hosting services).

#### 1978: Spam is born
1978 is also the year that brought the first unsolicited commercial email message (later known as spam), sent out to 600 California Arpanet users by [Gary Thuerk](https://en.wikipedia.org/wiki/Spamming).

#### 1984: Domain Name System (DNS)
The domain name system was created in 1984 along with the first Domain Name Servers (DNS). The domain name system was important in that it made addresses on the Internet more human-friendly compared to its numerical IP address counterparts. DNS servers allowed Internet users to type in an easy-to-remember domain name and then converted it to the IP address automatically.

#### 1989: The proposal for the World Wide Web
1989 also brought about the proposal for the World Wide Web, written by Tim Berners-Lee. It was originally published in the March issue of MacWorld, and then redistributed in May 1990. It was written to persuade CERN that a global hypertext system was in CERN’s best interest. It was originally called "**Mesh**"; the term "[World Wide Web](https://en.wikipedia.org/wiki/World_Wide_Web)" was coined while Berners-Lee was writing the code in 1990.

#### 1991: First web page created
[First web page](http://info.cern.ch/hypertext/WWW/TheProject.html) created.

#### 1991: The first webcam
One of the more interesting developments of this era, though, was the first webcam. It was deployed at a Cambridge University computer lab, and its sole purpose was to monitor a particular coffee maker, called [The Trojan Room Coffee Pot](https://en.wikipedia.org/wiki/Trojan_Room_coffee_pot) so that lab users could avoid wasted trips to an empty coffee pot.

#### 1993: Mosaic – first graphical web browser for the general public
The first widely downloaded Internet browser, Mosaic, was released in 1993. While [Mosaic]('https://en.wikipedia.org/wiki/Mosaic_(web_browser)') wasn’t the first web browser, it is considered the first browser to make the Internet easily accessible to non-techies.

#### 1993: Governments join in on the fun
In 1993, both the White House and the United Nations came online, marking the beginning of the .gov and .org domain names.

#### 1994: Netscape Navigator
Mosaic’s first big competitor, Netscape Navigator, was released the year following (1994).

#### 1995: Commercialization of the internet
1995 is often considered the first year the web became commercialized. While there were commercial enterprises online prior to ’95, there were a few key developments that happened that year. First, SSL (Secure Sockets Layer) encryption was developed by Netscape, making it safer to conduct financial transactions (like credit card payments) online
In addition, two major online businesses got their start the same year. The first sale on "Echo Bay" was made that year. Echo Bay later became eBay. Amazon.com also started in 1995, though it didn’t turn a profit for six years, until 2001.

#### 1995: JavaScript
Java and JavaScript (originally called LiveScript by its creator, Brendan Eich, and deployed as part of the Netscape Navigator browser – see comments for explanation) was first introduced to the public in 1995. ActiveX was launched by Microsoft the following year.

#### 1998: Google!
Google went live in 1998, revolutionizing the way in which people find information online.

#### 2004: Web 2.0
Though coined in 1999 by [Darcy DiNucci](http://www.darcyd.com/fragmented_future.pdf), the term "Web 2.0", referring to websites and Rich Internet Applications (RIA) that are highly interactive and user-driven became popular around 2004. During the first Web 2.0 conference, John Batelle and Tim O’Reilly described the concept of "the Web as a Platform": software applications built to take advantage of internet connectivity, moving away from the desktop (which has downsides such as operating system dependency and lack of interoperability).

#### 2004: "The" Facebook open to college students
Facebook launched in 2004, though at the time it was only open to college students and was called "The Facebook"; later on, "The" was dropped from the name, though the URL http://www.thefacebook.com still works.

#### 2005: YouTube – streaming video for the masses
YouTube launched in 2005, bringing free online video hosting and sharing to the masses.

Reddit link voting.

Google Earth virtual globe.

#### 2006: Twitter gets twittering
Twitter launched in 2006. It was originally going to be called twittr (inspired by Flickr); the first Twitter message was "just setting up my twttr".

#### 2007: Major move to place TV shows online
Major move to place TV shows online.

Hulu was first launched in 2007, a joint venture between ABC, NBC, and Fox to make popular TV shows available to watch online.

#### 2007: The iPhone and the Mobile Web
The biggest innovation of 2007 was almost certainly the iPhone, which was almost wholly responsible for renewed interest in mobile web applications and design.


## What Is the Difference Between the Internet and the Web?

#### The Internet is a Big Collection of Computers and Cables.

The Internet is an "**interconnection of computer networks**". It is a massive hardware combination of millions of personal, business, and governmental computers, all connected like roads and highways.

The Internet started in the 1960's under the original name "ARPAnet". ARPAnet was originally an experiment in how the US military could maintain communications in case of a possible nuclear strike. With time, ARPAnet became a civilian experiment, connecting university mainframe computers for academic purposes.

**No single person owns the Internet!**

No single government has authority over its operations. Some technical rules and hardware/software standards enforce how people plug into the Internet, but for the most part, the Internet is a free and open broadcast medium of hardware networking.

#### The Web Is a Big Collection of HTML Pages on the Internet.

The World Wide Web, or "Web" for short, is a massive collection of digital pages. The Web is viewed by using free software called web browsers. Born in 1989, the Web is based on hypertext transfer protocol, the language which allows you and me to "jump" (hyperlink) to any other public web page. There are over 65 billion public web pages on the Web today.

<br>

## Server, Client, Request, HTTP?

### HTTP Basics  

HTTP allows for communication between a variety of hosts and clients, and supports a mixture of network configurations.

> To make this possible, it assumes very little about a particular system, and does not keep state between different message exchanges.

Communication between a host and a client occurs, via a request/response pair. The client initiates an HTTP request message, which is serviced through a HTTP response message in return.

### How to reach a specific server

DNS stands for:

- **Domain**
- **Name**
- **System**

All computers on the internet have a unique numeric address composed of 4 bytes of data, separated by periods. You may recognize this format...

This is an IP address:

```
123.123.123.123
```

#### IP Addresses to domains

Of course, these numbers are hard to remember, and are not really human-friendly. Can you imagine if website addresses had to be given this way? Instead of commercials on the radio saying "go to OurWebsite.com", they'd be saying "go to 12.14.142.231" and repeating it 5 times just to make sure everyone got it!

So what was needed, was a way to translate real names in to those numbers. This is why we have domain registrars. You reserve the name, and at the domain registrar (really simplifying here), your domain NAME is pointed to the server's particular unique address.

When you type a website domain in to your web browser (or other internet capable app), what happens is a "DNS Lookup" of that particular domain's IP address, so your computer can actually connect to it.

Your internet provider has a nice little cached list of all of the registered domains, and their respective IP addresses, so when you go to yahoo.com, your browser asks the DNS server "what is the IP address of yahoo.com?" The DNS server responds with "206.190.60.37", and the browser can then connect to "206.190.60.37".

In real world terms, it's like how we use street addresses for finding a house, rather than using Latitude and Longitude coordinates as they are much easier to remember and to read.

### Client and server communication

In general, a service is an abstraction of computer resources and a client does not have to be concerned with how the server performs while fulfilling the request and delivering the response. The client only has to understand the response based on the well-known application protocol, i.e. the content and the formatting of the data for the requested service.

Clients and servers exchange messages in a **request-response** messaging pattern:

1. The client sends a request
2. The server returns a response

To communicate, the computers must have a common language, and they must follow rules so that both the client and the server know what to expect. The language and rules of communication are defined in a **communications protocol**.

All client-server protocols operate in the **application layer**. The application-layer protocol defines the basic patterns of the dialogue.

A server may receive requests from many different clients in a very short period of time. Because the computer can perform a limited number of tasks at any moment, it relies on a scheduling system to prioritize incoming requests from clients in order to accommodate them all in turn. To prevent abuse and maximize uptime, the server's software limits how a client can use the server's resources. Even so, a server is not immune from abuse. A denial of service attack exploits a server's obligation to process requests by bombarding it with requests incessantly. This inhibits the server's ability to respond to legitimate requests.

#### Example

When a bank customer accesses online banking services with a web browser (the client), the client initiates a request to the bank's web server. The customer's login credentials may be stored in a database, and the web server accesses the database server as a client. An application server interprets the returned data by applying the bank's business logic, and provides the output to the web server. Finally, the web server returns the result to the client web browser for display.
In each step of this sequence of client–server message exchanges, a computer processes a request and returns data. This is the request-response messaging pattern. When all the requests are met, the sequence is complete and the web browser presents the data to the customer.

<br>

## Elements of a URL

```
    http://www.example.org/hello/world/foo.html?foo=bar&baz=bat#footer
    \___/  \_____________/ \__________________/ \_____________/ \____/
  protocol  host/domain name        path         querystring     hash/fragment
```

Element | About
------|--------
protocol | the most popular application protocol used on the world wide web is HTTP. Other familiar types of application protocols include FTP, SSH, HTTPS
host/domain name | the host or domain name is looked up in DNS to find the IP address of the host - the server that's providing the resource
path | web servers can organise resources into what is effectively files in directories; the path indicates to the server which file from which directory the client wants
querystring | the client can pass parameters to the server through the querystring (in a GET request method); the server can then use these to customise the response - such as values to filter a search result
hash/fragment | the URI fragment is generally used by the client to identify some portion of the content in the response; interestingly, a broken hash will not break the whole link - it isn't the case for the previous elements

<br>

## Verbs

URLs reveal the identity of the particular host with which we want to communicate, but the action that should be performed on the host is specified via HTTP verbs. Of course, there are several actions that a client would like the host to perform. HTTP has formalized on a few that capture the essentials that are universally applicable for all kinds of applications.

These request verbs are:

- **GET**: fetch an existing resource. The URL contains all the necessary information the server needs to locate and return the resource.
- **POST**: create a new resource. POST requests usually carry a payload that specifies the data for the new resource.
- **PUT**: update an existing resource. The payload may contain the updated data for the resource.
- **DELETE**: delete an existing resource.

The above four verbs are the most popular, and most tools and frameworks explicitly expose these request verbs. PUT and DELETE are sometimes considered specialized versions of the POST verb, and they may be packaged as POST requests with the payload containing the exact action: create, update or delete.

<br>

## Request / Response and then?

Once the request / response cycle has been executed, the web browser is in charge of interpreting the data received and show it to the user.

The main function of a browser is to present the web resource you choose, by requesting it from the server and displaying it in the browser window. The resource is usually an HTML document, but may also be a PDF, image, or some other type of content. The location of the resource is specified by the user using a URI (Uniform Resource Identifier).

The way the browser interprets and displays HTML files is specified in the HTML and CSS specifications. These specifications are maintained by the W3C (World Wide Web Consortium) organization, which is the standards organization for the web.

## Further reading

* [How the Internet Works in 5 minutes - YouTube](https://www.youtube.com/watch?v=7_LPdttKXPc)
* [How does the Internet work? - MDN](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/How_does_the_Internet_work)
