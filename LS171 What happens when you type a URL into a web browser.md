LS 171 Study Guide

- What happens when you type a URL into your browser?

1. When you type a URL into your browser and hit send, the first thing that happens is the URL is sent to the Domain Naming System (DNS) to be resolved into an IP address. The DNS is a world-wide distributed databased of hierarchically organized servers. A DNS server will either have the requsted domain name, or route the request up the server heirarchy until it is found. Once found, the IP address will be given to your browser.
2. Assuming you are using TCP at the transport layer (skip to step 4 if you are using UDP):
   1. The browser will start a connection to the server at IP address and port using the TCP three-way handshake. An empty TCP segment is sent with a `syn` flag in its header. This  TCP segment is set as the data-payload of an Internet Protocol (IP) Packet which is then set as the data-payload of an Ethernet Frame.
   2. The Ethernet Frame is sent to a router which which removes the frame layer and looks at the destination IP address inside the IP packet. It uses the IP address to determine which router to send the packet to next. The router re-encapsulates the packet into a frame and sends it to the next router. This process is repeated as the packet 'hops' from one router to another until it reaches the destination server.
   3. Once at the destination server, both the Ethernet frame and the IP packet layers are removed and the server receives the `SYN` flag inside of the TCP segment to initialize the connection. The server responds by sending a TCP segment with `SYN ACK` in the header. Just as before, the segement is encapsulated in a IP packet, which is then encapsulated into an Ethernet frame that travels through the network to the client. 
   4. The client sends an `ACK` message back to the server in the same way as described above.
3. Still assuming you are using TCP at the transport layer:
   1. The client and server now engage in the Transport Layer Security (TLS) Handshake. This begins with the the client sending a `ClientHello` message which indicates the maximum version of the TLS protocol that the client can support and also provides a list of Cipher Suites the client can use.
   2. The server responds with a `ServerHello` message which sets the protocol version, Cipher Suite, and certificate which contains its public key. Additionally it includes a `ServerHelloDone` marker.
   3. The client initiates a key exchange. This allows the client and server to obtain a copy of the symmetric encryption key. A `ClientKeyExchange` message (aka the pre-master secret) is sent to the server as well as a `ChangeCipherSpec`flag which tells the server that encrypted communications should now start using the symmetric keys.
   4. The server sends back a message with the `ChangeCipherSpec` and `Finished` flags. The client and server can now begin secure communication using the symmetric key.
4. A HTTP `GET` request is encapsulated into TCP segments /UDP data-grams which get encapsulated into IP packets which form the data payload for Ethernet Frames. These Ethernet Frames are sent on the physical network to their destination.
5. After each frame arrives at the destination, the lower layer PDUs are removed and TCP checks each segment for errors and rearragnes them in order, retransmitting corrupt or absent data. UDP does not do this.
6. The server receives the HTTP request. This will include the HTTP method, the path, and the `Host` header. It may also include parameters, other headers, and a message body.
7. The server sends an HTTP response. Which is encapsulated in to a TCP segment / UDP data-gram, which are encapsulated into IP packets which are encapsulated into Ethernet frames.
8. Ethernet frames are sent on the physcal network to the client. Again, each frame and packet is removed and TCP verifies segments. UDP would not do this.
9. A `FIN` flag is sent inside an empty TCP segment from the server to the client to close the connection. This would not occur in UDP.
10. The lower level PDUs are removed and the browser receives the HTTP response.
11. The browser displays the body of the HTTP response.