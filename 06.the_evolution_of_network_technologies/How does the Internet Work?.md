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

Yes, but not *directly*. Every single computer in the world is not connected *directly* to every other single computer in the world. Instead, they are only *directly* connected to the other devices in their Local Area Network via the switch. However the Local Area Network will have another physical device called a router that is able to send messages to other networks. Each router's job is to know how to move packets of information from their source to their destination. How they do that is determined by the *logical rules* mentioned above, and we'll get to them in a bit, but first let's talk about the constraints of the physical network.

#####Constraints? What Constraints?

At the end of the day we are transfering bits of binary data (think "on" or "off") that are converted into signals of whatever medium we are using, like a electrical, light or radio waves. These physcial signals are subject to the laws of, well, physics. For example, *the speed of light* seems really fast, but it's not instant. It takes time for light to get from one place to another, and it will take longer for light to travel half-way around the world than it will to your friend's computer down the street and back. The measure of *time* it takes for a message to get from one point on the network to another point is called *latency.*

Bandwith is another constraint of the physical network. Bandwith is the *amount* of data that can be sent at once. For example if you have a single lane road, only one car can travel back and forth at a time. But if you add more lanes to the road, more cars can be traveling at once. The extra lanes don't increase the speed of the cars, but they allow the amount of cars traveling at once to increse. 

##Logical Rules

The fact that we didn't spend a ton of time on the physical elements of the internet should give you an idea of where most of the complexity is. Buckle up, we are going to go for a ride.

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

Each PDU has a data payload and a header with meta data about the PDU itself. The data payload is the data we want to transport over the network using a specific protocol at a particular network layer.

The entire PDU from a layer above gets set as the data payload for a protocol at the layer directly below. This provides seperation of the layers so that any particular layer doesn't have to worry about what is going on at other layers. This is very helpful because you may be mixing and matching protocols at a particular level. For example at the Transport level you may decide to use UDP instead of TCP, but either way the IP protocol at the Internet layer doesn't care. Higher level layers may depend on the 'service' that a lower level layer provides, but the specific implementation of the protocol will not matter.













######OSI Model:

| Layer        | Example Protocols | Purpose                                                      |
| ------------ | ----------------- | ------------------------------------------------------------ |
| Application  | HTTP              | [Used by specific applications to talk to each other.](https://web.stanford.edu/class/msande91si/www-spr04/readings/week1/InternetWhitepaper.htm) [Work with the user to prepare the data to be ready for the network.](https://www.youtube.com/watch?v=0Rb8AkTEASw) |
| Presentation |                   | [Encode, encrypt, compress](https://www.youtube.com/watch?v=0Rb8AkTEASw "starts talking about Presntation layer at 7:42 in the video") |
| Session      |                   | [Dialogue Management, keeping the connection alive](https://www.youtube.com/watch?v=0Rb8AkTEASw "Starts talking about session layer at 9:25 in the video") |
| Transport    | TCP, UDP          | To enable communication between applications, to provide reliable network communication |
| Network      | IP                | [To faciliate communication between hosts on different networks](https://launchschool.com/lessons/4af196b9/assignments/b222ecfb "Launch School "The Internet/Network Layer") |
| Data Link    | Ethernet          | [To interface between the physical network and the more logical layers above](https://launchschool.com/lessons/4af196b9/assignments/81df3782 "Launch School, The Link/Data Link Layer") |
| Physical     |                   | The tangible infrastructure that transmits the electrical signals, light, and radio waves which carry network communications |





