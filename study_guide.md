
## The internet ##
### What is the internet, and how does it work? ###
The internet is essentially a large number of inter-connected networks. It is a network of networks
- These networks are connected via routers many routers that redirect network traffic

**A network**
Two or more devices connected in a way in which they can communicate data
- LAN (Local Area Network) - Devices connected via physical network cables, and perhaps other network devices such as a switch or HUB

## The physical Network ##
### What are the characteristics of the physical network (such as latency and bandwich) ###
- The physical network is used to transfer bits (binary data) across physical space
  - To do this, the bits are converted into signals (electrical, light, or radio)

### Latency ###
 A measure of time that it takes for data to get from one point to another (typically measured in milliseconds)
 - Can be thought of as a measure of delay for data transfer, as well as sending preamble and SFD

**Propogratin delay** - amount of time a message takes to travel from sender to reciever (strictly the time it takes, based on distance / speed of signal medium)
**Transmission delay** - Time is takes for message to move from network device to transmission medium (wire, fiber optic, radio wave etc...)
- Example of this is the interframe gap at the data-link layer
**Processig delay** - Time it takes for a device to process a message
- A router may need to process an IP packet (look at its header, IP destination) before routing it along
- A switch may need to process an ethernet frame (look at header, MAC address) before moving it along
- TCP introduces many processing delays.
  - flow control
  - congestion avoidance
  - sending acknowledgements
  - storing out of order data in buffer (HOL Blocking)

**Queing delay** - The amount of time data sits inside of a buffer or queue of a network device
- Host processing out of order data in the context of TCP protocol (HOL blocking) and storing it in a buffer
- Routers or switches putting data into a queue before they process it

**Last mile latency** refers to the fact that there are a higher proportion of delays from an ISP to a host than in core network of the internet

**Round-trip time** is the time for a message to be sent, delivered, and for a response or acknowledgement to be recieved
 
 ### Bandwith ###
 A measure of the amount of data that is sent within a particular amount of time (typically measured in bits/seconds)
 - The amount of bandwith (data per second) a device recieves will be equal to the bandwith transferred by the slowest network point in the connection
 - The slowest device in the connection is referred to as the "bottleneck"
   - This doesn't have to be a network device, as the bottleneck could be something like a wire or cable as well (CAT5 ethernet for example)
   - A bottleneck could also be the a point in the connection with congestion, with is certainly not a device

## How do lower level protocols operate ##
### Data Link layer ###
This layer is responsible for allowing the more logical layers to interface with physical with the physical. Its protocols are concerned with identifying physical devices to send data to
### Ethernet Protocol ###
**PDU** - Ethernet frame
- Encapsulates data from the Network layer above (IP packet) (lowest layer where encapsulation takes place)
- Has a header, payload, and a trailer
- The payload of the PDU at this layer is essentially a stream of bits
  - The ethernet protocol adds structure to this binary data

**Important fields in ethernet frame**
- Source and Destination MAC address - 6 bytes long each. Says which device sent the data, and which it is to be delivered (within a local network)
 - Note that **MAC addresses** are "burined-in" to any network enabled device by the manufacturere. Essentally an ID number
- Length - Size of the data payload in bytes
- Frame Check Sequence - A checksum created by sender of data with an algorithm. Reciever uses same algoirthm on the data to generate checksum, and compares to sender
 - If the checksums are different, data has been losses or corrupted, and the frame is dropped

**Notes about MAC addresses**
There are static ID addresses for network enabled devices (devices that have a NIC card)
- Local network devices like switches have MAC tables that they use to route data to the correct device using MAC source address and destination address, specified by the Ethernet protocol
  - The switch will figure out the MAC addresses through communication with the devices on the network, and map them to a port on itself

### Network Layer: Internet Protocol (IP) ###
This layer and its protocols are concerned with the transfer of data between networks

### Internet Protocol (IP) ###
**PDU** - is called a data packet/packet
**PDU - Data packets/packets**
- Encapsulates data from the transport layer (typically a TCP segment or UDP datagram)
- Has a header and a data payload

**Important Fields**
Version - indicates the version of IP e.g IPv4 vs IPv6
ID, Flags, Fragment Offset: - If a piece of data has to be fragmented because it is too large, these flags will help it be reassembled
TTL - Time to live. If a packet lives too long without reaching a destination, it will be dropped
Protocol - Indicates protocol used for the payload (TCP vs UDP)
Checksum - Sender generates value using data to be sent and alogirthm. Recipient uses same algorithm or recieved data. If values don't match, packet is dropped
  - This is used so that corrupted or incomplete data is identified
Source address - 32 bit IP address of sender
Destination address: 32 bit IP address of recipient

**IP Addresses (IPv4)**
IP addresses are addresses used to identify networks
- They are logical in nature, and hierarchical
- They are not tied to a specific devise, but are assigned as needed to an available range designated to the network
  - This range is defined within a hierarchy, and subnetworks of the main network fall within this range, as well as each device
  - Splitting of a network into small subnetworks is called **subnetting**
- The addresses are 32 bits long and are divided into four sections of 8 bits
  - In decimal format, each of these four sections range from 0-255

e.g if a networks address is 109.168.172.0, the range of IPs it can assign is 109.168.172.1 --> 109.168.172.254
  **- This means we can use the first 3 sections of an IP address to determine which network it belongs to**

### Port Number ###
A port is an identifier for a specific process running on a host. It is represented as an interget 0-65535
- Can have an interger range of 0-65335
- 0-1023 --> well known ports (http: 80, FTP: 20-21, SMTP: 25...)
- 1024-49151 --> registered ports (by private entities like Mircosoft, IBM etc...), sometimes used for _ephemeral_ ports
- 49152-65535 --> dynamic ports/private ports, cannot be registered for a specific use. Can be customized for services or allocated as _ephemeral_ ports
