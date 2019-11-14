[toc]

#How Does the Internet Work?

To understand how the internet works, we need to understand what the internet is.

The internet is a system of both *physical parts* and *logical rules* for how computers and programs should interact. 

Ultimately, the *logical rules* are the more complex, harder to grasp side of the internet, so let's start with the more straight forward *physical* side of the internet.

##Physical Parts

Physically, the internet is a network of networks all connected through tangible infrastructure such as cables, wires, radio waves, and networked devices.

#####What is a network?

The most basic mental model of a network is two computers connected to each other so they can exchange data. More commonly though, the network will be multiple computers connected via a switch. This mini network is called a Local Area Network. The scope of communication for one of these computers in the Local Area Network is limited to just the other devices that are connected to the switch.

#####Can't a Computer Communicate With *Any* Other Computer On the Internet?

Yes, but not *directly*. Every single computer in the world is not connected *directly* to every other single computer in the world. Instead, they are only *directly* connected to the other devices in their Local Area Network via the switch. However the Local Area Network will have another physical device called a router that is able to send messages to other networks. The router is like a gateway into and out of each Local Area Network. Each router's job is to know how to move packets of information from their source to their destination. How they do that is determined by the *logical rules* mentioned above, and we'll get to them in a bit, but first let's talk about the constraints of the physical network.

#####Constraints? What Constraints?

At the end of the day we are transferring bits of binary data (think "on" or "off") that are converted into signals of whatever medium we are using, like a electrical, light or radio waves. These physical signals are subject to the laws of, well, physics. For example, *the speed of light* seems really fast, but it's not instant. It takes time for light to get from one place to another, and it will take longer for light to travel half-way around the world than it will to your friend's computer down the street and back. The measure of *time* it takes for a message to get from one point on the network to another point is called *latency.*

Bandwidth is another constraint of the physical network. Bandwidth is the *amount* of data that can be sent at once. For example if you have a single lane road, only one car can travel back and forth at a time. But if you add more lanes to the road, more cars can be traveling at once. The extra lanes don't increase the speed of the cars, but they allow the amount of cars traveling at once to increase. 

##Logical Rules

The fact that we didn't spend a great deal of time on the physical elements of the internet should give you an idea of where most of the complexity is. Buckle up, we are going to go for a ride.

#####Protocols

A **protocol** is a set of of logical rules that governs the exchange of data.

There are a **lot** of different protocols with a **lot** of different purposes, but right now we just want to get a high-level mental model of what is going on.

To organize and understand what all these sets of rules are doing, we can think of protocols as operating at different *layers*. One helpful model to visualize these layers is the Internet Protocol Suite (also known as the TCP/IP model). The TCP/IP model organizes these layers into Application, Transport, Internet, and Link.

######TCP/IP

| Layer       | Example Protocols / PDU      | Purpose                                                      |
| ----------- | ---------------------------- | ------------------------------------------------------------ |
| Application | HTTP (requests/ responses)   | [The rules for how applications talk to each other at a syntactical level](https://launchschool.com/lessons/cc97deb5/assignments/c604eb60) |
| Transport   | TCP, UDP (segment, datagram) | [To enable communication between applications](https://launchschool.com/lessons/2a6c7439/assignments/52b3420e) and [to provide reliable network communication](https://launchschool.com/lessons/2a6c7439/assignments/52b3420e) |
| Internet    | IP (packet)                  | [To faciliate communication between hosts on different networks](https://launchschool.com/lessons/4af196b9/assignments/b222ecfb "Launch School The Internet/Network Layer") |
| Link        | Ethernet (frame)             | [To interface between the physical network and the more logical layers above](https://launchschool.com/lessons/4af196b9/assignments/81df3782 "Launch School, The Link/Data Link Layer") |

Each protocol has it's own Protocol Data Unit (PDU). For example, at the Link Layer an Ethernet PDU is called a frame, at the Internet layer an Internet Protocol PDU is called a packet, and at the Transport layer a TCP PDU is called a segment.

Each PDU has a data payload and a header with meta-data about the PDU itself. The data payload is the data we want to transport over the network using a specific protocol at a particular network layer.

The entire PDU from a layer above gets set as the data payload for a protocol at the layer directly below. This provides separation of the layers so that any particular layer doesn't have to worry about what is going on at other layers. This is very helpful because you may be mixing and matching protocols at a particular level. For example at the Transport level you may decide to use UDP instead of TCP, but either way the IP protocol at the Internet layer doesn't care. Higher level layers may depend on the 'service' that a lower level layer provides, but the specific implementation of the protocol will not matter.

We'll talk more about these layers and protocols in a bit, but first we need to know how to identify other devices we want to communicate with!

##### IP Addresses

IP addresses are like phone numbers for computer's on the internet. If you have a computer's IP address, you can send it a message. IP addresses look like this: `213.79.212.25`. Remember when we said "each router's job is to know how to move packets of information from their source to their destination"? IP addresses are how they do that. Every IP address can be broken down into two parts, a *network prefix* and a *host identifier*. For example, `213.79.212.25` can be broken down into

```
Network Prefix: 213.79
Host Identifier: 212.25
```

All computers that are connected to the Internet through a single source (such as a particular Internet Service Provider, college campus, or business) will all share the same network prefix.

Routers will send all packets of the form `213.79.*.*` to the same location. So instead of keeping track of billions of *IP addresses*, *routers* only need to keep track of less than a million *network prefix*.

##### How Do Humans Keep Track of IP Addresses?

We use human readable domain names such as `www.google.com` to point to an IP address. The assocation between domain names and IP addresses are stored in the *Domain Name System (DNS)* which is a decentralized database. Let's say you want to go to `www.google.com`. You computer will first check it's own local DNS cache to see if it has visited that site recently. If it isn't there, it will ask it's Internet Service Provider's DNS. If the ISP DNS can't find the IP address, it will ask the root name servers which can resolve every domain name for a given top-level domain such as `.com`, `.net`, and `.org`.

###How Does One Computer Talk to Another Computer?

Once we have the IP address of the server we want to talk to, we are ready to send it a message. How do we do that? Using all those layers we were talking about earlier. 

The HTTP request from the **Application Layer** we want to send gets placed in the data-payload of a TCP segment in the **Transport Layer**. In the header section of the TCP segment, a Source Port and a Destination Port are included. These are needed so that the message can get to the correct application on the server. Visualize how you have multiple applications at once- a web-browser, a messaging app, iTunes, etc. The Source Port and Destination Port specify to which application the message should go.

Then, the TCP segment gets placed as the data-payload of an IP packet in the **Internet Layer**. In the header section of the IP Packet there is a Source IP Address and Destination IP address. We got the Destination IP address from the DNS earlier, so we are good to go there.

Then the IP packet is placed as the data-payload of an Ethernet Frame in the **Link Layer**. In the header section of the Ethernet Frame there is a Source MAC Address and a Destination Mac Address, The MAC address is a physical or "burned-in" address that doesn't change on your computer. Your computer doesn't know the MAC address of the final destination server, so for now it just puts the MAC address of the local router.

Then the frame is translated into binary signals and sent through the physical medium that we talked about earlier. This is where the routers will direct the packet on to each network 'hop' in its journey. Routers will be aware of network congestion, and will try to avoid areas where there is high traffic. If a router is overflowed with packets, it will drop new incoming packets. This means that the network is inherently unreliable. To combat this, the Transmission Control Protocol (TCP) is in charge of re-sending packets if they don't reach their destination.

Once the packet arrives at the destination computer, it traverses back up the layers so eventually all that is left is the HTTP request that was orignally sent.

![Screen Shot 2019-11-13 at 2.58.12 PM](/Users/nathanworden/Documents/School/Launch_School/LS170_Networking_Foundations/06.the_evolution_of_network_technologies/Screen Shot 2019-11-13 at 2.58.12 PM.png)

### Mix and Match

Depending on your priorities, or the needs of your communication, you may choose to use 