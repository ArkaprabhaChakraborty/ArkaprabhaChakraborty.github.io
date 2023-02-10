+++
showdate = true
title = "HTTP and HTTPS Basics - How the internet works"
date = 2022-01-10
url = "/blog/http_basics"
+++
# [HTTP (HyperText Transfer Protocol)](https://www.rfc-editor.org/rfc/rfc2616)
Most internet communications are made with web requests through the HTTP protocol. HTTP is an application-level protocol used to access the World Wide Web resources. The term hypertext stands for text containing links to other resources and text that the readers can easily interpret.

HTTP communication involves a client sending a request for a resource to a server, which then processes the request and returns the resource. We use the same technique to access webpages/visit different websites, we provide a [Fully Qualified Domain Name (FQDN)](https://en.wikipedia.org/wiki/Fully_qualified_domain_name) as a [Uniform Resource Locator (URL)](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_is_a_URL), which is just a very specific [Uniform Resource Identifier (URI)](https://en.wikipedia.org/wiki/Uniform_Resource_Identifier). Read this [blog](https://www.hostinger.com/tutorials/uri-vs-url#:~:text=URI%20identifies%20a%20resource%20and,a%20domain%20name%20and%20port.) from Hostinger to know more

The default port for HTTP is 80, though this can be changed to any other port and depends on the web-server program's configuration.

# URL
This a sample URL. The different components of the URL are explained below in the table.
![URL structure Image - Courtesy HTB Academy modules](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/url_structure.png)

|Component |	Example	| Description
----------|----------|----------
|Scheme	   |http:// https:// | This is used to identify the protocol being accessed by the client, and ends with a colon and a double slash (://)
|User Info |	admin:password@ |	This is an optional component that contains the credentials (separated by a colon :) used to authenticate to the host, and is separated from the host with an at sign (@)
|Host |	inlanefreight.com |	The host signifies the resource location. This can be a hostname or an IP address
|Port |	:80 |	The Port is separated from the Host by a colon (:). If no port is specified, http schemes default to port 80 and https default to port 443
|Path |	/dashboard.php	| This points to the resource being accessed, which can be a file or a folder. If there is no path specified, the server returns the default index (e.g. index.html).
|Query String |	?login=true |	The query string starts with a question mark (?), and consists of a parameter (e.g. login) and a value (e.g. true). Multiple parameters can be separated by an ampersand (&).
|Fragments |	#status |	Fragments are processed by the browsers on the client-side to locate sections within the primary resource (e.g. a header or section on the page).

Not all components are required to access a resource. The main mandatory fields are the scheme and the host, without which the request would have no resource to request.

# HTTP Information Flow

![HTTP Information Flow - Courtesy HTB Academy modules](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/HTTP_Flow.png)

**Note: Our browsers usually first look up records in the local '/etc/hosts' file, and if the requested domain does not exist within it, then they would contact other DNS servers. We can use the '/etc/hosts' to manually add records to for DNS resolution, by adding the IP followed by the domain name.**

# [HTTPS (HTTP Secure)](https://www.rfc-editor.org/rfc/rfc2660)

HTTPS is the secure version of HTTP, which uses SSL/TLS to encrypt the communication between the client and the server. The default port for HTTPS is 443, though this can be changed to any other port and depends on the web-server program's configuration. 

Let's take a look at what's changed, shall we? Well then... Here is a dump/ packet capture of an HTTP request.
![HTTP Dump](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/http_wireshark_dump.png)
See how everything is in plain text and everything is readable by the sniffer? Now let's take a look at the HTTPS dump.
![HTTPS Dump](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/https_wireshark_dump.png)
See the gibberish in blue? That's because the data is encrypted. The client and the server use a shared secret key to encrypt and decrypt the data.
**Note: Although the data transferred through the HTTPS protocol may be encrypted, the request may still reveal the visited URL if it contacted a clear-text DNS server. For this reason, it is recommended to utilize encrypted DNS servers (e.g. 8.8.8.8 or 1.2.3.4), or utilize a VPN service to ensure all traffic is properly encrypted.**

