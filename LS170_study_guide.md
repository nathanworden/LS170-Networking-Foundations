[toc]

##The Internet

####Have a broad understanding of what the internet is and how it works

- What is the internet?

The internet is a system of both *physical parts* and *logical rules* for how computers and programs should interact. It is a network of networks comprised of these componenets. 

Physically, the internet is a network of networks all connected through tangible infrastructure such as cables, wires, radio waves, and networked devices.

Logically, the internet is a system of layered sets of rules, (aka protocols). These rules control the exchange of data over the network. Different protocols are concerned with different aspects of network communication. These protocols work together though encapsulation, where a PDU at one layer becomes the data payload of the PDU of a protocol at a lower layer.



Although not a perfect model if we are thinking about the specifics of implementation, a useful mental model for the layers of the internet would be the Internet Protocol Suite.

######TCP/IP

| Layer       | Example Protocols / PDU      | Purpose                                                      |
| ----------- | ---------------------------- | ------------------------------------------------------------ |
| Application | HTTP (requests/ responses)   | [The rules for how applications talk to each other at a syntactical level](https://launchschool.com/lessons/cc97deb5/assignments/c604eb60) |
| Transport   | TCP, UDP (segment, datagram) | [To enable communication between applications](https://launchschool.com/lessons/2a6c7439/assignments/52b3420e) and [to provide reliable network communication](https://launchschool.com/lessons/2a6c7439/assignments/52b3420e) |
| Internet    | IP (packet)                  | [To faciliate communication between hosts on different networks](https://launchschool.com/lessons/4af196b9/assignments/b222ecfb "Launch School The Internet/Network Layer") |
| Link        | Ethernet (frame)             | [To interface between the physical network and the more logical layers above](https://launchschool.com/lessons/4af196b9/assignments/81df3782 "Launch School, The Link/Data Link Layer") |

- How Does the Internet Work?

  A networks is at least two computers connected in such a way that they can exchange data. More commonly a Local Area Network is a group of computers all connected through a device such as a switch or a hub. These LANs are connected via routers to larger networks. And these larger networks are connected to still larger networks. The internet is a vast network of networks.

- What happens when you input an address into your browser?

  1. User enters a URL in browser.
  2. Browser connects to DNS.
  3. DNS returns IP address to browser.
  4. Browser starts a connection to the server at the IP address and port. To start the connection, an empty TCP segment is sent with a `SYN` flag in its’ header. This TCP segment is encapsulated inside of an IP packet inside of an Ethernet frame.
  5. The Ethernet frame moves along to each hop at the network. At each hop, the frame is processed by removing the frame layer, and the router looks at the destination IP address inside of the packet and determine the packet’s next stop. The packet is re-encapsulated into a frame and goes to it’s next destination.
  6. The frame gets to the server. The Ethernet frame and the IP packet layers are removed, and the server retrieves the `SYN` inside of the TCP segment to initialize a connection.
  7. Client and server go through the Three Way Handshake. The server sends a TCP segment containing a `SYN ACK` in its’ header. The segment is encapsulated inside an IP packet, which is itself encapsulated into an Ethernet frame that travels through the network to the client. The client sends a `ACK` message in the same manner.
  8. Client and server go through the TLS Handshake. Client sends a `ClientHello` message that contains information about the TLS protocol, including version number and a list of cipher suites (cryptographic algorithms that establish and maintain the connection) the client is able to use. Server sends a `ServerHello` message, setting the TLS protocol version and cipher suite. Server sends a certificate that contains its public key, and a `ServerHelloDonemarker` that tells the client it is done with this step of the handshake. Client initiates a key exchange process that enables client and server to obtain a copy of the symmetric encryption key. Client sends a `ChangeCipherSpec` flag to tell the server that encrypted communications should now start using the symmetric keys.
  9. The HTTP `GET` request is encapsulated into TCP segments which get encapsulated into IP packets which form the data payload for Ethernet frames.
  10. Ethernet frames are sent on the physical network to their destination.
  11. After each frame arrives at the destination, lower layer PDUs are dropped and TCP checks each segment for errors and rearranges them in order, retransmitting corrupt or absent data.
  12. Server receives the HTTP request.
  13. Server sends HTTP response.
  14. The HTTP response is encapsulated into TCP segments which are encapsulated into IP packets which are encapsulated into Ethernet frames.
  15. Ethernet frames are sent on the physical network to the client. Each frame is dropped and TCP verifies segments.
  16. The FIN flag is sent inside an empty TCP segment from the server to the client to close the connection.
  17. The lower level PDUs are dropped and the browser receives the HTTP response.
  18. The browser displays the body of the HTTP response.

####Understand the characteristics of the physical network, such as latency and bandwidth

- What is latency?

  Latency is a measure of time that it takes for data to get from one point in the network to another point in the network.

  Latency is comprised of:

  - Propagation delay: The ratio between distance and speed.
  - Transmission delay: The amount of time it takes to jump from one link to another.
  - Processing delay: The amount of time data need to be processed when jumping from one link to another.
  - Queuing delay: If there is more data than a device can handle, then the data will queue, or buffer.

  Latency is a physical limitation of the network and thus affects the speed at which communication can happen back and forth on the network. 

- What is bandwidth?

  Bandwidth is the amount of data that can be sent at once. Bandwidth is a measure of capacity. Bandwidth is not constant across the network. The capacity at the core of the network is higher than the edges. The bandwidth that a connection receives is the lowest amount at a particular point in the overall connection. A bandwidth bottleneck is a point at which the bandwidth changes from realitevly high to relatively low.

  Bandwidth is a physical limitation of the network and thus affects the amount of data that can be sent at once in network communcation.

- What is Last-mile latency?

  The delays that take place within the parts of the network that are closest to the end-points. Relates to the delays invovled in getting the network signal from your Internet Service Provider's network to your home office or network. At the network edge there are more frequent and shorter hops as the data is directed down the network hierarchy to the appropriate sub-network. The network edte is like the 'entry point' into a network like a home or corporate LAN.

- What is Round Trip Time?

  The length of time for a signal to be sent, added to the length of time for an acknowledgement or response to be received.

####Have a basic understanding of how lower level protocols operate

- How does the Link / Data Link Layer operate?

  **Operates as an interface between the physical network and the more logical layers above.** 

  The Ethernet protocol is commonly used protocol at this layer. The Ethernet protocol provides communication between devices on the same local network. Ethernet uses MAC addressing to identify devices connected to the local network. Uses a PDU called a Frame.

  Two of the most important aspects of Ethernet are framing and addressing.

  An Ethernet Frame adds logical structure to binary data. The data is still in the form of bits, but the structure defines which bits are the data payload, and which are the meta-datato be used in the process of transporting the frames.

  Source MAC address is the address of the device which created the frame (which can change along the data's journey)

  Destination MAC address is the address of the device for which the data is intended.

  https://launchschool.com/lessons/4af196b9/assignments/81df3782

- How do MAC Addresses work? What are the characteristics of a MAC address?

Every network-enabled device is assigned a unique MAC Address when it is manufacutred. It is linked to the specific physical device, so it is often referred to as the *physical address* or *burned-in address*. MAC Addresses are a swquence of six two digit hexadecimal numbers, e.g. `00:20:76:7d:87:0a`. Ranges of addresses are assigned to different network hardware manufacturers.

Hubs send messages to all connected devices. Each receving device would check its MAC Address agains the Destination MAC address in the frame to see if it was the intended recipient. If it wasn't it would just ignore the frame. A switch will look at the destination address in the frame and direct the frame only to the device for which it is intended. A switch does this by keeping a record of the MAC addresses of the devices connected to it in a MAC address table. The table associated each address with the Ethernet port to which the device is connected on the switch.

MAC addresses are physcial, not logical. They are tied to a specific physical device.

MAC addresses are flat, not hierarchical. The entire address is a sequence of values and can't be broken down into sub-divisions.

MAC Addressing works for local networks, but doesn't scale well past that. You wouuld need to store impossibly large lookup tables to keep track of which MAC Addresses were part of which local networks.

- How does the Network / Internet Layer operate?

  **Protocols at this layer operate to facilitate communication between hosts on different networks**

  The Intenet Protocol is the predominant protocol used at this layer for inter-network communication. The Internet Protocol enables communcation between two networked devices anywhere in the world by using IP Addressing. There are two versions: IPv4 and IPv6.

  Two of the most important aspects of the Internet protocol are Routing capability via IP addressing and Encapsulation of data into packets.

  A packet is a PDU of the IP protocol. It is made up of a Data Paylaod and  Header. The payload is the PDU from the layer above, and the header is split into logical fields with provide meta data used in transporting the packet.

  IP addresses are logical in nature. They can be assigned as required to devices as they join the network. The IP address that a device is assigned must fall within a range of addresses available to the local network that the device is connected to. The range is defined by a network hierarchy, where an overall network is split into logical sub-networks, each defined by the range of IP addresses available to it.

  Routers that want to forward an IP packet to any address in the entire range only need to keep a record of which router on the network controls access to the segement with that network address, not every single device in that range.

  The splitting of a network into parts is referred to as sub-netting. By dividing IP address ranges fruther, subnets can be split into smaller subnets to create even more tiers within the hierarchy.

  https://launchschool.com/lessons/4af196b9/assignments/b222ecfb

####Know what an IP address is and what a port number is

- What is an IP Address? 

  An IP address is used to identify host machines. It is used to create a communication channel between hosts, sending a packet from one host to another. IP addresses enable communication between two networked devices anywere in the world. However, if we want to create networked applications, we need more than just communication between devices. We need to be able to transport data between specific applications on the client and server.

  **Talk about what the numbers mean on IPv4 and IPv6**

  Logical in nature, not like mac addresses. Can be assigned as required. 
  
- What is a Port Number?

  A port is an identifer for a specific process running on a host. This identifier is an integer in the range 0-65535. Sections of the range are reserved for specific purposes:

  - 0-1023 are well known ports
  - 1024-49151 are registered ports
  - 49152-65535 are dynamic ports (aka private ports). Cannot be registered for a specific use.

  Ports enable multiplexing and demultiplexing, or transmitting multiple signals over a single channel. The source and destination port numbers are included in the PDU for the tranport layer and can be used to direct data to a specific process on a host.

- The combination of IP address adnd port number infomration can be thought of as defining a communication end-point. This communication end point is generally referred to as a *socket*. It would look like this: `216.3.128.12:8080`. You can think of the postal service as an analogy. The IP address is like the streed address of the apartment building, the individual apartment number is like a port number. The postal service can be thought of as the Internet Protocol, and the building concierge as the Transport layer protocol (TCP or UDP).

  https://launchschool.com/lessons/2a6c7439/assignments/41113e98

- Why are multiplexing and demultiplexing important?

  They allow a client or server to run multiple different processes on a server at once.

####Have an understanding of how DNS works

- How does the DNS work?

The Domain Naming System translates computer names like www.google.com to an IP address and maps the request to a remote server. It keeps trak of URLs and their corresponding IP addresses on the Internet. An address like www.google.com might be resolved to an IP address `197.251.230.45`. The Domain Naming system is a very large world-wide network of hierarchically organized DNS servers, and no single DNS server contains the complete database. If a DNS server does not contain a requested domain name, the DNS server routes the request to another DNS server up the hierarchy. Eventually, the address will be found in the DNS database on a particular DNS server, and the corresponding IP address wll be used to receive the request.

Here's the flow:

1. You enter an address like http://www.google.com in your web browser.
2. Your request is sent to your device's network interface.
3. The request goes over the internet where a search for http://www.google.com begins. Behind the scenes, http://www.google.com is simply a human-friendly name that represents an IP Address associated with a remote coputer or a server.
4. The remote server accepts the request and sends a respnse over the Internet to your network interface which hands it to your browser.
5. Finally, the browser displays the response in the form of a web page.

When your browser issues a request, it's simply sending some text to an IP address. Because the client (web browser) and the server (recipient of the request) have an agreement, or protocol, in the form of HTTP, the server can take apart the request, understand its componenets and send a response back to the web browser.

####Understand the client-server model of web interactions, and the role of HTTP as a protocol within that model

- Describe the client-server model of web interactions

A client such as a web browser is responsible for issuing HTTP requests and processing the HTTP response in a user-friendly manner onto your screen.

Servers are just machines or devices capable of handling inbound requests, processing them, and their job is to issue a response back. 

The client makes a request, and the server issues a response to that request. 

A client server model is a type of network articheture. A client, the computer, connects to another computer, a server, and they exchange data back and forth.

https://launchschool.com/books/http/read/background#resources

- What is HTTP's role as a protocol within the client-server model?

HTTP operates at the application layer and is concered with structuring the messages that are exchanged between applications; it's actually TCP/IP that's ensuring that the request/response cycle gets completed between your browser and the server.

https://launchschool.com/lessons/cc97deb5/assignments/586769d9

## TCP & UDP

####Have a clear understanding of the TCP and UDP protocols, their similarities and differences

- What is TCP?

TCP provides reliable network communication on top of an unreliable channel. 

TCP accomplishes the following:

​		 - Data integrity

​		 - De-duplication

​		 - In-Order Delivery 

 		- Retransmission of lost data

The downsides are the performance impacts.

- What are the similarities of TCP and UDP?

  - They both provide data encapsulation (segments for TCP and datagrams for UDP) and multiplexing ( transmitting multiple signals over a single channel.)
  - They both have a checksum in their header

- What are the differences of TCP and UDP?

  - UDP doesn't do anything to resolve the inherent unreliablity of the layers below it, unlike TCP.
  - UDP doesn't provide a guarantee of message delivery, TCP does
  - UDP does not provide a guarantee of message delivery order, TCP does
  - UDP doesn't provide a built-in congetstion avoidance or flow-control mechanisms, TCP does.
  - UDP does not provide connection state tracking, (since it is a connectionless protocol), TCP does.

- Why use UDP instead of TCP?

  Speed and flexibility. Applications using UDP at the Transport layer can just start sending data without having to wait for a connection to be established with the application process of the receiver. In addition to this, the lack of acknowledgements and retransmissions means that the actual data delivery itself is faster; once a datagram is sent it doesn't have to be sent again. Latency is less of an issue wince without acknowledgements data essentially just flows one way: from sender to receier. The lack of in-order deliver also removes the issue of Head-of-line blocking (at least at the Transport layer). While you are more flexible and free, there is also a certain expectation that you would implement some form of congestion avoidance in your UDP-based application.

- What are the Disadvantages of TCP?

Although TCP provides reliable data transfer, and also uses flow control and congestion avoidance techniques to try and do so efficiently, there are also drawbacks to using it. We've already seen that there is a latency overhead in establishing a TCP connection due to the handshake process. Another potential issue with using TCP is Head-of-Line (HOL) blocking.

Head-of-line blocking is a general networking concept, and isn't specific to TCP. In general terms it relates to how issues in delivering or processing one message in a sequence of messages can delay or 'block' the delivery or processing of the subsequent messages in the sequence.

With TCP, HOL blocking can occur as a result of the fact that TCP provides for in-order delivery of segments. Although this in order delivery is one aspect of TCP's reliability, if one of the segments goes missing and needs to be retransmitted, the segments that come after it in the sequence can't be processed, and need to be buffered until the retransmission has occurred. This can lead to increased queuing delay which, as we saw in an [earlier assignment](https://launchschool.com/lessons/4af196b9/assignments/097d7577), is one of the elements of latency.

https://launchschool.com/lessons/2a6c7439/assignments/d09ddd52

The main *downsides of TCP* are the *latency overhead of establishing a connection*, and the potential *Head-of-line blocking* as a result of in-order delivery.

https://launchschool.com/lessons/2a6c7439/assignments/4ab0993c

####Have a broad understanding of the three-way handshake and its purpose

Background:

Problem: Individual messages can become corrupt or lost.

Solution: Use an acknowledgement message.

Problem: What if the acknowledgement is not receivd?

Solution: re-send the message if the acknowledgement is not received within a certain time-frame.

Problem: The message is received but acknowledgement is not received (or not in time), resulting in a duplicate message.

Solution: add sequence numbers to the messages

One of the main reasons for the three-way handshake process is to syncronise (`SYN`) the sequence numbers that will be used during the connection.

The Three-Way Handshake is used to establish a connection. There is a fair amount of complexity going on, especially in the initial extablishment of a connection. An important part of the process is that the sender cannot send any application data until after it has sent the `ACK` Segment. This means that there is an entire round-trip of latency before any application data can be exchanged. Since the hand-shake process occurs every time a TCP connection is made, this clearrly has an impact on any application which uses TCP at the transport layer.

https://launchschool.com/lessons/2a6c7439/assignments/d09ddd52

Looks like this:

| Client Start State | Client Action                                                | Client End State | Server Start State | Server Action                                                | Server End State |
| ------------------ | ------------------------------------------------------------ | ---------------- | ------------------ | ------------------------------------------------------------ | ---------------- |
| `CLOSED`           | Sends a `SYN` Segment                                        | `SYN-SENT`       | `LISTEN`           | Waits for a connection request                               | -                |
| `SYN-SENT`         | Waits to receive an `ACK` to the `SYN` it sent, as well as the server's `SYN` | `SYN-SEN`        | `LISTEN`           | Sends a `SYN ACK` Segment which serves as both it's `SYN` and an `ACK` for the client's `SYN` | `SYN-RECEIVED`   |
| `SYN-Sent`         | Receives the `SYN ACK` Segment sent by the server, and sends an `ACK` in response. The client is now finished with the connection establishment process | `ESTABLISHED`    | `SYN-RECEIVED`     | Waits for an `ACK` for the `SYN` it just sent                | `                |
| `ESTABLISHED`      | Ready for data transfer. Can start sending application data. | `ESTABLISHED`    | `SYN-RECEIVED`     | Receives the ACK sent in response to its SYN. The server is now finished with the connection establishment process. | `ESTABLISHED`    |
|                    |                                                              |                  |                    |                                                              |                  |



####Have a broad understanding of flow control and congestion avoidance

TCP invovles a lot of overhead in terms of establishing connections, and providing realibility through the retransmission of lost data. In order to mitigate against this additional overhead, it is important that the actual functioning of data transfer when using the protocol occurs as efficiently as possible. In order to help facilitate efficient data transfer once a connection is established, TCP provides mechisms for flow control and congestion avoidance.

- How does Flow Control work?

Flow control is a mechanism to prevent the sender from overwhelming the receiver with too much data at once. The receiver will only be able to proceess a certain amount of data in a particular time-frame. Data awaiting processing is stored in a 'buffer'. The buffer size will depend on the amount of memory allocated according to the configuration of the OS and the physical resources available.

Each side of a connection can let the other side know the amount of data that it is willing to accept via the WINDOW field of the TCP header. This number is dynamic, and can change during the course of a connection. If the receiver's buffer is getting full it can set a low amount in the WINDOW field of a Segment it sends to the sender, the sender can then reduce the amount of data it sends accordingly.

Although flow control prevents the sender from overwhelming the receiver, it doesn't prevent either the sender or receiver from overwhelming the underlying network. For that task we need a different mechanism: Congestion Avoidance.

- How does Congestion Avoidance Work? 

Network Congestion is a situation that occurs when there is more data being transmitted on the network than there is network capacity to process and transmit the data. You can perhaps think of it as similar to a gridlock of vehicles on a road network. Instead of things coming to a standstill however, the 'excess vehicles' are simply lost.

In the [last lesson](https://launchschool.com/lessons/4af196b9/assignments/b222ecfb) we looked at IP packets moving across the networks in a series of 'hops'. At each hop, the packet needs to be processed: the router at that hop runs a checksum on the packet data; it also needs to check the destination address and work out how to route the packet to the next hop on its journey to that destination. All of this processing takes time, and a router can only process so much data at once. Routers use a 'buffer' to store data that is awaiting processing, but if there is more data to be processed than can fit in the buffer, the buffer over-flows and those data packets are dropped.

As we've already seen, TCP retransmits lost data. If lots of data is lost that means lots of retransmitted data, which is inefficient. Ideally we want to keep retransmission to a minimum. TCP actually uses data loss as a feedback mechanism to detect, and avoid, network congestion; if lots of retransmissions are occurring, TCP takes this as a sign that the network is congested and reduces the size of the transmission window.

There are various different approaches and algorithms for determining the size of the initial transmission window, and how much it should be reduced or increased by depending on network conditions. The exact algorithm or approach used will depend on which variant of TCP is in operation.

https://launchschool.com/lessons/2a6c7439/assignments/d09ddd52

## URLs

####Be able to identify the components of a URL, including query strings

- What are the components of a URL?
  - `http`: The **scheme**. It always comes before the colon and two forward slashes and tells the web client how to access the resource. In this case it tells the web client to use the Hypertext Transfer Protocol or HTTP to make a request. Other popular URL schemes are `ftp`, `mailto` or `git`.
  - `www.example.com`: The **host**. It tells the client where the resource is hosted or located.
  - `:88` : The **port** or port number. It is only required if you want to use a port other than the default.
  - `/home/`: The **path**. It shows what local resource is being requested. This part of the URL is optional.
  - `?item=book` : The **query string**, which is made up of **query parameters**. It is used to send data to the server. This part of the URL is also optional.

  Sometimes, the path can point to a specific resource on the host. For instance, [www.example.com/home/index.html](http://www.example.com/home/index.html) points to an HTML file located on the example.com server.

  Sometimes, we may want to include a port number which the host uses to listen to HTTP requests. A URL in the form of: http://localhost:3000/profile is using the port number `3000` to listen to HTTP requests. The default port number for HTTP is port `80`. Even though this port number is not always specified, it's assumed to be part of every URL. **Unless a different port number is specified, port `80` will be used by default in normal HTTP requests.** To use anything other than the default, one has to specify it in the URL.

- Explain Query Strings/Parameters

A simple URL with a query string might look like:

```irb
http://www.example.com?search=ruby&results=10
```

Let's take that apart:

| Query String Component | Description                                                  |
| :--------------------- | :----------------------------------------------------------- |
| ?                      | This is a reserved character that marks the start of the query string |
| search=ruby            | This is a parameter name/value pair.                         |
| &                      | This is a reserved character, used when adding more parameters to the query string. |
| results=10             | This is also a parameter name/value pair.                    |

Now let's take a look at an example. Suppose we had the following URL:

**http://www.phoneshop.com?product=iphone&size=32gb&color=white**

In the above example, name/value pairs in the form of `product=iphone`, `size=32gb` and `color=white` are passed to the server from the URL. This is asking the `www.phoneshop.com` server to narrow down on a product `iphone`, size `32gb` and color `white`. How the server uses these parameters is up to the server side application.

Another common place where you may have seen query parameters in action is when you perform a search in any modern search engine. **Because query strings are passed in through the URL, they are only used in HTTP GET requests.** We'll talk about the different HTTP requests later in the book, but for now just know that whenever you type in a URL into the address bar of your browser, you're issuing HTTP GET requests. Most links are also HTTP GET requests, though there are some minor exceptions.

Query strings are great to pass in additional information to the server, however, there are some limits to the use of query strings:

- Query strings have a maximum length. Therefore, if you have a lot of data to pass on, you will not be able to do so with query strings.
- The name/value pairs used in query strings are visible in the URL. For this reason, passing sensitive information like username or password to the server in this manner is not recommended.
- Space and special characters like `&` cannot be used with query strings. They must be URL encoded, which we'll talk about next.

####Be able to construct a valid URL

- Given the following URL:

https://amazon.com/Double-Stainless-Commercial-Refrigerator/B60HON32?ie=UTF8&qid=142952676&sr=93&keywords=commercial+fridge

a. Identify the host: `amazon.com`

b. Identify the names of the query parameters: `ie`, `qid`, `sr`, `keywords`

c. Identify the values of the query parameters: `UTF8`, `142952676`, `93`, `commercial + fridge`

d. Identify the scheme: `https`

e. Identify the path: `/Double-Stainless-Commercial-Refrigerator/B60HON32`

f. Identify the port: There isn't one in this URL.

- Add the port `3000` to the following URL:

http://amazon.com/products/B60HON32?qid=142952676&sr=93

http://amazon.com:3000/products/B60HON32?qid=142952676&sr=93

- Given the following URL:

  http://localhost:4567/todos/15

  a. Identify the query parameters: none

  b. Identify the scheme: `http`

  c. Identify the path: `/todos/15`

  d. Identify the host: `localhost`

  e. Identify the port:` 4567`

- What are two different ways to encode a space in a query parmeter? 

`+` and `%20`

- What character indicates the beginning of a URL's query parameters?

`?`

- What character is used between the name and the value of a query parameter?

`=`

- What character is used between multiple query parameters?
- `&`

####Have an understanding of what URL encoding is and when it might be used

By default URLs are designed to accept only certain characters in the [ASCII character set](http://en.wikipedia.org/wiki/ASCII). As such, unsafe or reserved characters not included in this set have to be converted or encoded to conform to this format. URL encoding serves the purpose of replacing these non-conforming characters with a `%` symbol followed by two hexadecimal digits that represent the [ASCII code](http://www.asciitable.com/) of the character.

Below are some popular encoded characters and example URLs:

| Character | ASCII code | URL                                                          |
| :-------- | :--------- | :----------------------------------------------------------- |
| Space     | 20         | [http://www.thedesignshop.com/shops/tommy%20hilfiger.html](http://www.thedesignshop.com/shops/tommy hilfiger.html) |
| !         | 21         | [http://www.thedesignshop.com/moredesigns%21.html](http://www.thedesignshop.com/moredesigns!.html) |
| +         | 2B         | http://www.thedesignshop.com/shops/spencer%2B.html           |
| #         | 23         | http://www.thedesignshop.com/%23somequotes%23.html           |

Characters must be encoded if:

1. They have no corresponding character within the [ASCII character set](http://www.asciitable.com/).
2. The use of the character is unsafe. For example `%` is unsafe because it is used for encoding other characters.
3. The character is reserved for special use within the URL scheme. Some characters are reserved for a special meaning; their presence in a URL serve a specific purpose. Characters such as `/`, `?`, `:`, `@`, and `&` are all reserved and must be encoded.

For example `&` is reserved for use as a query string delimiter. `:` is also reserved to delimit host/port components and user/password.

So what characters can be used safely within a URL? Only alphanumeric and special characters `$-_.+!'()",` and reserved characters when used for their reserved purposes can be used unencoded within a URL. As long as a character is not being used for its reserved purpose, it has to be encoded.

## HTTP and the Request/Response Cycle

####Be able to explain what HTTP requests and responses are, and identify the components of each

- What are HTTP requests?

A HTTP request and response are strings that are organized in a standared format  that another machine understands.

https://launchschool.com/books/http/read/background#resources

An *HTTP Request* consists of a *request line*, *headers*, and an optional *body*.

- What are the components of an HTTP request?
  - The method (required)
  - The path (required)
  - the Host (required)
  - parameters (optional)
  - headers (optional)
  - message body (optional)

####Be able to describe the HTTP request/response cycle

- Please describe the HTTP request/response cycle

Let's say you have a webserver and a browser. If you type in a URL to the browser, the browser will try and retreive that page for you. The browser creates an HTTP request. There are **two required pieces of information, the method** (such as GET), **and the path** (such as /tasks). You can also send optional fields such as 'Parameters', 'headers', or 'body'. The domain name is used to determine what server to send the request to. It is not used in the actual request itself. 

After the server sees the request, it will perform some sort of work. What it does will vary based on what the server does, such as if there is data in the database, if there are files in the file server. A common workflow would look like this:

	 - A request would come in
	 - verify the user session
	 - load tasks from the data base
	 - render some HTML
	 - Send response back to client.

In the response, there will be a status (200 OK), Headers (a collection of medta data about the response, such as content type, meaning, how the browser it can interprest the response), and Body (the bulk of the data that is being sent. If it is a web page, the body will contain all the HTML code that the browser will display. If it was something else, say an audio file, then the body would be that data.)

When the browser receives the response, it looks at the content-type header and then it will act accordingly, for example display the webpage to the viewer.  

https://launchschool.com/lessons/cc97deb5/assignments/83ae67aa

####Be able to explain what status codes are, and provide examples of different status code types

- What is a status code?

A status code is a three-digit number that the server sends back after receiving a request signifying the status of the request. The status text displayed next to the status code provides the description of the code. 

- Please provide examples of different status code types

| Status Code | Status Text           | Meaning                                                      |
| ----------- | --------------------- | ------------------------------------------------------------ |
| 200         | OK                    | The request was handled successfully                         |
| 302         | Found                 | The requested resource has changed temporarily. Usually results in a redirect to another URL. |
| 404         | Not Found             | The requested resource cannot be found.                      |
| 500         | Internal Server Error | The server encontered a generic error.                       |

More detail:

302 Found - Commonly, when a resource is moved, a requestr is re-routed from the original URL to the new URL where the resource now resides. This is generally called a `redirect`. A status code of 302 tells your browser that a resource has been moved and it automatically follows the new re-routed URL in the `Location` response header.

404 Not Found - The requested resrouce cannot be found. A resource can be anything; video viles, CSS stylesheets, JavaScript files, images, etc. 

500 Internal Server Error - Means "There's something wrong on the server side." This is a generaic error status code and can mean a lot of different things. It could be a mis-configured server setting, a misplaced comma in the application code, or anything in between. It's a server issue. Someone with access to the server will have to debug and fix the problem.

####Understand what is meant by 'state' in the context of the web, and be able to explain some techniques that are used to simulate state

- What does 'state' mean in the context of the web?

Each Request/Response cycle is independent of Request and Responses that came before or those that come after.

https://launchschool.com/lessons/cc97deb5/assignments/9f4e349a

It means that the server does not need to hang on to information (e.g. state) between requests. As a result, when a request breaks en-route to the server, no part o the system has to do any cleanup.

And it means that, since the web is built on HTTP and HTTP is stateless, it makes the web resilient, distributed, and hard to control. It's also what makes it so difficult to secure and build on top of.

- Please explain some techniques that are used to simulate state

Statefulness can be simulated through techniquest which use session IDs, cookies, and AJAX (Asynchronous JavaScript calls).

  - Session IDs: The server sends a form of unique token, or session identifier to the client. Whenver a client makes a request to that server, the client appends this token as part of the request, allowing the server to identify clients.

    Passing a session id back and forth between the client and server creates a sense of persistent connection between requests. 

    Session IDs have consequences however. You have to inspect every request to see if it contains a session identifer. If it does, the server must check to ensure that this particular session id is still valid. The server will have to maintain some rules about how to handle session expiration and also decide how to store its session data. Also, the server needs to retrieve the session data based on the session id. Finally, the server needs to recreate the application state (e.g. the HTML for a web request) from the session data and send it back to the client as the response.

    The server has to work hard to simulate statefulness. Every request still gets its own response, even if most of that response is identical to the previous response. 

- Cookies: A cookie is a piece of data that's sent from the server and stored in the client during a request/response cycle. Cookies are small files stored in the browser and contain the session information. By default, most browsers have cookies enabled. When you access any website for the first time, the server sends session information and sets it in your browser cookie on your local computer. Note that the actual session data is stored on the server. The client side cookie is compared with the server-side session data on each request to identify the current session. This way, when you visit the same website agan, your session will be recognized because of the stored cookie with its assocaited information.

- AJAX: Asynchronous JavaScript and XML allows browsers to issue requests and process responses without a full page refresh. The response is processed by a callback function, which is usually some client-side JavaScript code. You can thnk of a `callback` as a piece of logic you pass on to some function to be executed after a certain event has happened.

https://launchschool.com/lessons/cc97deb5/assignments/9f4e349a

####Explain the difference between `GET` and `POST`, and know when to choose each

- What is the difference between `GET` and `POST`?

`GET` requests should only retrieve content from the server. They can generally be thought of as "read only" operations. One expection to this is a webpage that tracks how many times it is viewed. `GET` still works because the main content of the page didn't change. 

https://launchschool.com/lessons/cc97deb5/assignments/83ae67aa

`GET`requests are used to regtrieve a resource- most links are `GET`s.

The response from a `GET` request can be anything, but if it's HTML and that HTML references other resources, your browser will automatically request those referenced resources. A pure HTTP tool will not.

`POST` requests invovle changing values taht are stored on the server. Most HTML forms that submit their values to the server will use `POST`. Search forms are a noticeable exception to this rule: they often use `GET` since they are not changing any data on the server, only viewing it.

`POST` is used when you want to initiate some action on the server, or send data to a server. Typically from within a browser, you use `POST` when submitting a form. `POST` requests allow us to send much alrger and sensitie data to the server, such as images or videos. Using a `POST` request to send a username and password to the server for authentication would be a good use case because `POST` requests help sidestep the query string size limitation that you have with `GET` requests. With `POST` requests, we can send significantly larger forms of information to the server.

https://launchschool.com/books/http/read/making_requests#get

## Security

####Have an understanding of various security risks that can affect HTTP, and be able to outline measures that can be used to mitigate against these risks

- What are the security risks that can affect HTTP?
  - Somone stealing your browser's session id- they can log in as you.

When the client and server send requests and responses to each other, these are being sent in the form of strings. A malicious hacker could attach to the same network and use packet sniffing to read the messages being sent back and forth. Requests can contain the session id, which uniquely identifies the client to the server, so if someone else copied the session id, they could craft a request to the server and pose as your client, and therefore automatically be logged in without even having access to your username or password.

HTTPS is an answer to this. In HTTPS every request/repsonse is encrypted before being transported on the network. HTTPS sends messages through a cryptographic protocol called TLS for encryption. TLS uses certificates to communicate with remote servers and exchange security keys before data encryption happens.

- What is Session Hijacking?

When an attacker gets a hold of the session id they can now access the web application as you. 

- How can you guard against Session Hijacking?
  - Same-orgin policy: permits unrestricted interaction between resources originating from the same origin, but restricts certain interactions between resources originating from different origins. Orgin refers to the combination of the URL's scheme, hostname, and port. Cross-origin requests are typically restricted where resources are being accessed programmically using APIs or fetch. Same-orgin policy doesn't restrict all cross-origin requests. Linking, redirects, or form submissions to different origins are typically allowed. Same-origin policy can be an issue for web developers who have a legitimate need for making cross-origin requests. Cross-origin resource sharing or CORS was developed to deal with this issue. This works by adding new HTTP headers, which allows servers to serve resources cross-origin to certain specified origins. https://launchschool.com/books/http/read/security#securehttp
  - Resetting Sessions: A successful login renders an old session id invalid and creates a new one. If an attacker hijacks a session id, the victim will will be required to authenticate on the next request. With the new session id the attacker will not have access anymore.
  - Expiration time on sessions: A session that does not expire gives an attacker an unlimited amount of time to pretend to be the real user. Having a session expire after, say 30 minutes, gives the attacker a far narrower window to access the application.
  - HTTPS: see above.
- What is Cross-Site Scripting (XSS) ?

When you allow users to input HTML or JavaScript that ends up being displayed on the site directly. (For example a form that allows you to add comments which will be displayed on the site.). If the server side code doesn't do any sanitization of input, the user input will be injected into the page contents, and the browser will *interprest the HTML and JavaScript and execute it.* Attackers can craft malicious HTML and JavaScript and be very destructive to both the server as well as future visitors of the page. For example, an attacker can use JavaScript to grab the session id of every future visitor of the site and then come back and assume their identity. The malicious code would bypass the same-origin policy because the code lives on the site. 

- How can you guard against Cross-Site Scripting?
  - Always sanitize user input. Eliminate problematic input, such as \<script> tags, or disallowing HTML and JavaScript input altogether in favor of a safer format, like Markdown.
  - Escape all user input data when displaying it. If you do need to allow users to input HTML and JavaScript, then when you print it out, make sure to escape it so that the browser does not interpret it as code.

  https://launchschool.com/books/http/read/security#securehttp

####Be aware of the different services that TLS can provide, and have a broad understanding of each of those services

- What are the different services that Transport Layer Security (TLS) provides?

  - **Encryption**: A process of encoding a message so that it can only be read by those with an authorized means of decoding the message.

  To securily send messages via HTTP we want both the request and the response to be encrypted in such a way that they can only be decrypted by the intended recipient. The most efficient way to do this is via symmetric key cryptography. If we want to use symmetric keys however, we also need a way to securly change the symmetric key.

  TLS uses a combination of symmetric and asymmetric cryptography. The inital symmetric key exchange is conducted using asymmetric key encryption, and then the bulk of the message exchange is conducted via symmetric key encryption. The TLS Handshake process is used to:

  		 - Agree which version of TLS to be used in establishing a secure connection.
  		 - Agree on the various algorithms that will be included in the cipher suite.
  		 - Enable the exchange of symmetric keys that will be used for message encryption.

  https://launchschool.com/lessons/74f1325b/assignments/54f6defc

  - **Authentication**: A process to verify the identity of a particular party in the message exchange.
    - In the TLS handshake the server provides its certificate which contains the Public Key. Another function of the certificate is to provide a means of identification for the party providing it. The server can send its certficate along with its public key. Then it can create a 'signature' with some data encryped with the server's private key. The server then sends the signiture to the client along with the original data from which the signature was created. When the client decrypts the signature, if it matches the original data then it shows that only the party in possession of the private key could have encrypted the signiture.
    - How do we know if a certificate is genuine or not?
      - Certificate Authorities. They verfy that the party requesting the certificat is who they say they are. And they also digitally sign the certificate being issued. There is a chain of trust of Certificat Authorities up to Root Certificate Authorities. Ultimatley this system still relies on trust. 
  - **Integrity**: A process to detect whether a message has been interfered with or faked. There is a Message Authentication Code (MAC) field in a TLS PDU. It is similar in concept to checksum fields. Other checksums are about error detection (i.e. to test if some data was corrupted during transport). The intention of the MAC field in a TLS record is to add a layer of security by providing a means of checking that the message hasn't been altered in transit.
    - Message Authentication Codes are implemented through a hashing algorithm. The sender will create a small amount of data derived from the actual data. This is called a digest. The digest is created using a specific hashing algorithm combined with a pre-agreed hash value. The sender encrypts the data payload using the symmetric key, encapsulates it into a TLS record, and passes the record to the Transport layer to be sent. Upon receipt of the message, the receiverdecrypts the data payload using the symmetric key. The receiver then also creates a digest of the payload using the same algorithm and hash value. If the two digests match, this confirms the integrity of the message.