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

HTTPS is the secure version of HTTP, which uses encryption protocol to encrypt the communication between the client and the server. This encryption protocol is called [Transport Layer Security (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security), although formerly it was known as [Secure Sockets Layer (SSL)](https://www.cloudflare.com/en-gb/learning/ssl/what-is-ssl/). The default port for HTTPS is 443, though this can be changed to any other port and depends on the web-server program's configuration. 

Let's take a look at what's changed, shall we? Well then... Here is a dump/packet capture of an HTTP request.

![HTTP Dump](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/http_wireshark_dump.png)

See how everything is in plain text and everything is readable by the sniffer? Now let's take a look at the HTTPS dump.

![HTTPS Dump](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/https_wireshark_dump.png)
See the gibberish in blue? That's because the data is encrypted. The client and the server use a shared secret key to encrypt and decrypt the data.

**Note: Although the data transferred through the HTTPS protocol may be encrypted, the request may still reveal the visited URL if it contacted a clear-text DNS server. For this reason, it is recommended to utilize encrypted DNS servers (e.g. 8.8.8.8 or 1.2.3.4), or utilize a VPN service to ensure all traffic is properly encrypted.**

Let's take a look at how HTTPS operates. The schematic below gives a good overview of how HTTPS works.

![How HTTPS works - Courtesy HTB Academy](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/HTTPS_Flow.png)

Here's what happens when you visit a website using HTTPS:

- When we type in [https://]() in the URL bar, the browser first looks up the domain name in the local '/etc/hosts' file, and if the requested domain does not exist within it, then it contacts other DNS servers.
- Next the browser sends a {{< colour "blue" "client hello" >}} request at the specified port. This just contains client's supported TLS versions and cipher suites.
- After receiving the {{< colour "blue" "client hello" >}} the server sends a {{< colour "magenta" "server hello" >}} request, which contains the server's supported TLS versions and cipher suites, followed by a [{{< colour "magenta" "Server key exchange" >}}](https://en.wikipedia.org/wiki/Key_exchange) to exchange SSL (or more correctly TLS) certificates.
- The client then verifies the server's certificate/key, with the certificate authority that issued it. This confirms that the server is who it says it is, and that the client is interacting with the actual owner of the domain. 
- If the server's certificate is valid, the client then sends a {{< colour "blue" "client key exchange" >}} request, which contains one of the client's certificate.
- Following the above the client initiates an {{<colour "cyan" "encrypted handshake">}}, by sending one more random string of bytes, also called the {{<colour "yellow" "premaster secret">}} - is encrypted with the public key, received by the client from the server's SSL certificate, and can only be decrypted with the private key by the server.
- The server decrypts the premaster secret with its private key.
- Both client and server generate {{< colour "green" "session keys" >}} from the client random, the server random, and the premaster secret. Both should arrive at the same conclusion.
- The client sends a {{< colour "blue" "change cipher spec" >}} request, which tells the server that the client is ready to start sending encrypted data, along with a Finished message.
- The Finished message from server is then sent to indicate that the handshake is complete on the client side.

This is how the flow of information is, but the implementation is a bit different for each TLS versions. For eg:
Consider the TLS 1.2 handshaking process as shown below:

![TLS 1.2 Handshaking Process](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/Two-Roundtrip-Handshake.png)

Now look at the TLS 1.3 handshaking process: (KA means Key Agreement)

![TLS 1.3 Handshaking Process](https://raw.githubusercontent.com/ArkaprabhaChakraborty/ArkaprabhaChakraborty.github.io/main/data/images/Single-Round-Trip-Handshake-1.png)

**Note: A cipher suite is a set of algorithms for use in establishing a secure communications connection. There are a number of cipher suites in wide use, and an essential part of the TLS handshake is agreeing upon which cipher suite will be used for that handshake.**


For more information on HTTPS and TLS, check out the following links:
- [Patrick Nohe's Blog- Taking a Closer Look at the SSL/TLS Handshake](https://www.thesslstore.com/blog/explaining-ssl-handshake/#:~:text=The%20%E2%80%9CChange%20Cipher%20Spec%E2%80%9D%20message,complete%20on%20the%20client%20side.)
- [How HTTPS "Used to" work before RSA was broken :(](https://tiptopsecurity.com/how-does-https-work-rsa-encryption-explained/)
- [TLS RFC](https://www.rfc-editor.org/rfc/rfc8446)
- [What is SSL?](https://www.cloudflare.com/en-gb/learning/ssl/what-is-ssl/)