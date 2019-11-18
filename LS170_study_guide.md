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

  

####Understand the characteristics of the physical network, such as latency and bandwidth

- What is latency?

  Latency is a measure of time that it takes for data to get from one point in the network to another point in the network.

  Latency is comprised of:

  - Propagation delay: The ratio between distance and speed.
  - Transmission delay: The amount of time it takes to jump from one link to another.
  - Processing delay: The amount of time data need to be processed when jumping from one link to another.
  - Queuing delay: If there is more data than a device can handle, then the data will queue, or buffer.

- What is bandwidth?

  Bandwidth is the amount of data that can be sent at once. Bandwidth is a measure of capacity. Bandwidth is not constant across the network. The capacity at the core of the network is higher than the edges. The bandwidth that a connection receives is the lowest amount at a particular point in the overall connection. A bandwidth bottleneck is a point at which the bandwidth changes from realitevly high to relatively low.

- What is Last-mile latency?

  The delays that take place within the parts of the network that are closest to the end-points. Relates to the delays invovled in getting the network signal from your Internet Service Provider's network to your home office or network. At the network edge there are more frequent and shorter hops as the data is directed down the network hierarchy to the appropriate sub-network. The network edte is like the 'entry point' into a network like a home or corporate LAN.

- What is Round Trip Time?

  The length of time for a signal to be sent, added to the length of time for an acknowledgement or response to be received.

####Have a basic understanding of how lower level protocols operate

- How does the Link / Data Link Layer operate?

  **Operates as an interface between the physical network and the more logical layers above.** 

  The Ethernet protocol is the most commonly used protocol at this layer. The Ethernet protocol provides communication between devices on the same local network. Ethernet uses MAC addressing to identify devices connected to the local network. Uses a PDU called a Frame.

  Two of the most important aspects of Ethernet are framing and addressing.

  An Ethernet Frame adds logical structure to binary data. The data is still in the form of bits, but the structure defines which bits are the data payload, and which are the meta-datato be used in the process of transporting the frames.

  Source MAC address is the address of the device which created the frame (which can change along the data's journey)

  Destination MAC address is the address of the device for which the data is intended.

  https://launchschool.com/lessons/4af196b9/assignments/81df3782

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

- What is a Port Number?

  A port is an identifer for a specific process running on a host. This identifier is an integer in the range 0-65535. Sections of the range are reserved for specific purposes:

  - 0-1023 are well known ports
  - 1024-49151 are registered ports
  - 49152-65535 are dynamic ports (aka private ports). Cannot be registered for a specific use.

  Ports enable multiplexing and demultiplexing, or transmitting multiple signals over a single channel. The source and destination port numbers are included in the PDU for the tranport layer and can be used to direct data to a specific process on a host.

- The combination of IP address adnd port number infomration can be thought of as defining a communication end-point. This communication end point is generally referred to as a *socket*. It would look like this: `216.3.128.12:8080`. You can think of the postal service as an analogy. The IP address is like the streed address of the apartment building, the individual apartment number is like a port number. The postal service can be thought of as the Internet Protocol, and the building concierge as the Transport layer protocol (TCP or UDP).

####Have an understanding of how DNS works

####Understand the client-server model of web interactions, and the role of HTTP as a protocol within that model

## TCP & UDP

####Have a clear understanding of the TCP and UDP protocols, their similarities and differences

####Have a broad understanding of the three-way handshake and its purpose

####Have a broad understanding of flow control and congestion avoidance

## URLs

####Be able to identify the components of a URL, including query strings

####Be able to construct a valid URL

####Have an understanding of what URL encoding is and when it might be used

## HTTP and the Request/Response Cycle

####Be able to explain what HTTP requests and responses are, and identify the components of each

####Be able to describe the HTTP request/response cycle

####Be able to explain what status codes are, and provide examples of different status code types

####Understand what is meant by 'state' in the context of the web, and be able to explain some techniques that are used to simulate state

####Explain the difference between `GET` and `POST`, and know when to choose each

## Security

####Have an understanding of various security risks that can affect HTTP, and be able to outline measures that can be used to mitigate against these risks

####Be aware of the different services that TLS can provide, and have a broad understanding of each of those services