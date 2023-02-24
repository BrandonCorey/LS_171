
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
 - Can be thought of as a measure of delay for data transfer

**Propogratin delay** - amount of time a message takes to travel from sender to reciever (strictly the time it takes, based on distance / speed of signal medium)
**Transmission delay** - Time is takes for message to move from network device to transmission medium (wire, fiber optic, radio wave etc...)
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
