# Computer Networking

## Introduction

### Network Application

- Read and write data over network
- Dominant model: bidirectional, reliable byte stream connection
  - One side reads what the other writes
  - Operates in both directions
  - Reliable (unless connection breaks

- The 2 hosts on the Internet can send data to each other, close connection on either side.

- World Wide Web (HTTP)
  - Hyper text transmission protocol
- Bit Torrent

### Layers

- Application
- Transport
  - Most common is TCP, Transmission Control Protocol.
  - If the Network layer drops some datagrams, TCP will retransmit them for multiple times if need-be.
  - If the Network layer delivers them out of order, TCP will put the data back into the right order again.
- Network
  - Deliver packets end-to-end across the Internet from the source to the destination.
  - The Network layer gives the data-to-be-sent to the Link layer for the Link layer to send it.
  - The Link layer passes the data to the Link layer of some router. The router’s Network layer examines the destination address of the datagram and is responsible for routing.
  - The datagram is sent to the Link layer of the router again and passes onto the next router, until it reaches the destination.

![1558754890511](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558754890511.png)

- Link
  - Carry the data over one link at a time, e.g., Ethernet, WiFi.
  - Provides a service to the Network layer. If there is a request of sending datagram from the Network layer, the Link layer would do it.

### IP

- IP makes a best-effort attempt to deliver our datagrams to the other end. But it makes no promises.
- IP datagrams can get lost, can be delivered out of order, and can be corrupted. There are no guarantees.

### OSI

![1558754921001](C:\Users\a\AppData\Roaming\Typora\typora-user-images\1558754921001.png)

## 

