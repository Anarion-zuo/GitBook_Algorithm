# Computer Networking

## Introduction

### Fundamental Rules of Communication

- Three Elements

  - Message source/Sender: the sources are people or electronic devices, that need to communicate a message to other individuals or devices.
  - Destination/Receiver: the destination receives the message and interprets it.
  - Transmission medium/Channel: it provides the pathway over which the message can travel from source to destination.
  - ![img](D:\Gitbook\GitBook_Algorithm\static\135465168496)

- Protocols

  - Timing

    Many network communication functions are dependent on timing. Timing determines the speed at which the bits are transmitted across the network. It also affects when an individual host can send data and the total amount of data that can be sent in any one transmission.

  - Message Size

    The rules that govern the size of the pieces communicated across the network are very strict. They can also be different, depending on the channel used. When a long message is sent from one host to another over a network, it may be necessary to break the message into smaller pieces in order to ensure that the message can be delivered reliably.

  - Encapsulation

    Each message transmitted on a network must include a header that contains addressing information that identifies the source and destination hosts, otherwise it cannot be delivered. Encapsulation is the process of adding this information to the pieces of data that make up the message. In addition to addressing, there may be other information in the header that ensures that the message is delivered to the correct application on the destination host.

  - Message Format

    When a message is sent from source to destination, it must use a specific format or structure. Message formats depend on the type of message and the channel that is used to deliver the message.

  - Encoding

    Messages sent across the network are first converted into bits by the sending host. Each bit is encoded into a pattern of sounds, light waves, or electrical impulses depending on the network media over which the bits are transmitted. The destination host receives and decodes the signals in order to interpret the message.

  - Message Patterns

    Some messages require an acknowledgment before the next message can be sent. This type of request/response pattern is a common aspect of many networking protocols. However, there are other types of messages that may be simply streamed across the network, without concern as to whether or not they reach their destination.

![img](D:\Gitbook\GitBook_Algorithm\static\ijnflknvflav)

### Models

- Assists in protocol design, because protocols that operate at a specific layer have defined information that they act upon and a defined interface to the layers above and below.

- Fosters competition because products from different vendors can work together.

- Enables technology changes to occur at one level without affecting the other levels.

- Provides a common language to describe networking functions and capabilities.

#### 2 Types

- Protocol model

  This model closely matches the structure of a particular protocol suite. A protocol suite includes the set of related protocols that typically provide all the functionality required for people to communicate with the data network. The TCP/IP model is a protocol model, because it describes the functions that occur at each layer of protocols within the TCP/IP suite.

- Reference model

  This type of model describes the functions that must be completed at a particular layer, but does not specify exactly how a function should be accomplished. A reference model is not intended to provide a sufficient level of detail to define precisely how each protocol should work at each layer. The primary purpose of a reference model is to aid in clearer understanding of the functions and processes necessary for network communications.

#### TCP/IP

![img](D:\Gitbook\GitBook_Algorithm\static\fsafdscsfds)

#### Open Systems Interconnection(OSI)

![img](D:\Gitbook\GitBook_Algorithm\static\fafdsfrafgdshtrserh)

- Application: The application layer provides the means for end-to-end connectivity between individuals in the human network using data networks.
- Presentation: The presentation layer provides for common representation of the data transferred between application layer services.
- Session: The session layer provides services to the presentation layer to organize its dialogue and to manage data exchange.
- Transport: The transport layer defines services to segment, transfer, and reassemble the data for individual communications between the end devices.
- Network: The network layer provides services to exchange the individual pieces of data over the network between identified end devices.
- Data Link: The data link layer protocol describes methods for exchanging data frames between devices over a common medium.
- Physical: The physical layer protocol describes the mechanical, electrical, functional, and procedural means to activate, maintain, and de-activate physical connections for bit transmission to and from a network device.

Comparing to TCP/IP, a protocol model, the OSI is more about abstractions and general rules, not specific implementation.

![img](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/vxvOL9sXEeaiPA6SDh2LEA_46bf1811c21821e83bf567c47cc61d52_3212.JPG?expiry=1558742400000&hmac=pTPl3sKdH40Pi7a9AL8EijsYfyzrqOm8OSjTPxQvwcs)

## Ethernet

### Addressing

#### Call

Every host in the ethernet network has a physical address assigned to it when it is manufactured. The address is called Media Access Control (MAC) Address. When the address of a host is called out in the network, the host listen to the following data transmitted, and interpret it.

![img](https://d3c33hcgiwev3.cloudfront.net/imageAssetProxy.v1/TfRUvOrvEea0PA6g5Mr3FA_e47dd6762e6038044ba4aad6ddfd659f_3.2.2.3.gif?expiry=1558742400000&hmac=OAORIzxvTC1uqKAH5m7v6Ksk8BzFsMujsde9sJzeCaY)

#### MAC

In Windows operating systems, MAC address is often noted as physical address, and we can look for it by typing:

```console
ipconfig /all
```

A computer connecting to an Ethernet network is required to have at least one Ethernet network interface card (NIC), where it often has multiple. Each NIC corresponds to a MAC.

A MAC address consists of $2\times6$ digits of hex number, 48 bits. , every 2 of which is connected with a dash -. The first 3 bytes of the digits stand for an Organizationally Unique Identifier (OUI), signifying the manufacturer of the NIC. The 3 bytes remained are the unique ID for the interface. All MAC addresses that begin with the same OUI must have unique values in the last 3 bytes.

### Encapsulation

#### Frames

The process of placing one message format (the letter) inside another message format (the envelope) is called encapsulation. De-encapsulation occurs when the process is reversed by the recipient and the letter is removed from the envelope.

A frame is a computer message encapsulated in a specific format, before it is sent over the network. A frame provides the address of the intended destination and the address of the source host. The format and contents of a frame are determined by the type of message being sent and the channel over which it is communicated. Messages that are not correctly formatted are not successfully delivered to or processed by the destination host.

Frames are also referred to as Layer 2 Protocol Data Units (PDUs), for the protocols that provide the rules for the creation and format of the frame perform the functions that are specified at the data link layer (Layer 2) of the OSI model.

![img](D:\Gitbook\GitBook_Algorithm\static\fasdsfrdbmlgiuf)

#### Framing in Ethernet

- Location of the destination and source MAC address
- Preamble for sequencing and timing
- Start of frame delimiter
- Length and type of frame
- Frame check sequence to detect transmission errors

![img](D:\Gitbook\GitBook_Algorithm\static\bskfjdjhvbakfjgh)

- **Preamble** - Defined pattern of alternating 1 and 0 bits used to synchronize timing. 
- **SFD** - Marks the end of the timing information and start of the frame. The preamble and the SFD are used to indicate the beginning of the frame. They are not used in the calculation of the frame size.
- **Destination MAC Address** - The Destination MAC Address field contains the destination MAC address (receiver). The destination MAC address can be unicast (a specific host), multicast (a group of hosts), or broadcast (all hosts on the local network).
- **Source MAC Address** - The Source MAC Address field contains the source MAC address (sender). This is the unicast address of the Ethernet host that transmitted the frame.
- **Length / Type** - The Length/Type field supports two different uses. A type value indicates which protocol will receive the data, IPv4, IPv6, or others. The length indicates the number of bytes of data that follows this field.
- **Encapsulated Data** - The Data field contains the packet of information being sent. Ethernet requires each frame to be between 64 and 1518 bytes.
- **Frame Check Sequence(FCS)** - The FCS contains a 4-byte value that is created by the device that sends data and is recalculated by the destination device to check for damaged frames.

Frames that do not match these limits are not processed by the receiving hosts. Ethernet network does not care what kind of information it is careering, only transmitting data as it should be.