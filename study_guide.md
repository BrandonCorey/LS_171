
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

### DNS (Domain Name Systen) ###
A distributed database that translates domain names to an IP address, which can then be used to make a request to a server
- Makes accessing webpages easier since IP addresses do not need to be memorized
- Computers that store DNS databases are called DNS servers
- Organized in a hierachical structure. If one DNS server doesn't have record of a domain name, it routes the request to a server higher up
- Devices typically have a "DNS cache", which is a temporary storage of recent DNS lookups that have been made so that it has accept to those IPs immediately

## Client-Server model of web interactions, and role of HTTP as a protocol within it ##
- client - the most common client is an application called a _web browser_
- server - the content being requested by the client is located on a remote computer called a _server_
  - These are typically just computers capable of handling inbound requests
  - Job is to send a response to an inbound request
- HTTP (Hypertext transfer protocol) - A system of rules that serve as a link between applications and the transfer of hypertext documents
  - Handles structure of the message that a host sends to another host
- Request reponse protocol - client makes request to server using HTTP and waits for the response

## TCP & UDP ##
These are transfer layer protocols, responsible for getting a message to the correct service on a device

### What are they, what are their differences, and similarities ###
TCP (transmission control protocol) - a protocol that provides reliable data transfer
- Provides reliable network communication on top of an unreliable channel (the lower layers)
- Provides data integrity, de-duplication, in-order delivery, and retransmission of lost data
- It encapuslates the application level PDU (HTTP request)
- Abstracts complexity of its data management away from application level (and as a result, from developers much of the time)
- It is a _connection-oriented protocol_, providing connection state tracking through the use of a three way handshake

**TCP Segment (PDU)**
- Data payload consists of a message, most commonly, an HTTP request
- Header:
  - Source Port: Port of the service on the device sending the request
  - Destination Port: Port of the service on the device recieving the request
  - Checksum: Similar to other layers, provides error detectio (missing data or corrupted data) using a checksum
  - Sequence Number: Helps send deliver requests in the order that they were sent
  - Acknowledgement Number: Helps verify that a message that was sent was recieved successfuly
    - If not, the message can be retransmitted
    - It can also help detect duplicate sends as well, and discard those
  - Window size: The amount of requests that the recipient **host** is able to recieve at any given moment. Corresponds to amount of space in the recipients buffer
  - flags: `SYN`, `ACK`, are used to establish a connection between the sender and reciever

### Have an understanding of threeway handshake and its purpose ###
For two services to establish a connection
- Sender sends a segment with a `SYN` flag (the value flippped to 1)
- Reciever recieves segment with `SYN`, responds with a segment with `SYN` and `ACK` (both flipped to 1)
- Reciever recieves `ACK` segment, connection is established

- The sender will immediately start sending data after it sends its `ACK`
- Reciever can only respond once it has recieved the `ACK`

## Flow control and congestion avoidance ##
**Flow Control**
- A mechanism to presvent the sener from overwhelming the reciever with too much data at once
- The reciever can only recieve so much data, any data awaiting processing is stored in a buffer
- As we said earlier, TCP segments have a `WINDOW` field that corresponds to how much room the host has left in its buffer
- This means the reciever can inform the sender how much room it has left within the `WINDOW` field with its acknowledgements or responses
  - This helps the sender and reciever prevent overwhelming each other, but not the underlying network

**Congestion avoidance**
Network congestion is when more data is being tramsmitted on the network than it has capacity to process and trasmit the data
- TCP uses lost data to determine is network congestion is happening
- Since TCP uses acknowledgements, it knows every piece of data that is lost
- If many retransmissions are lost, it will transmit less and less data until that is no longer the case

### UDP ###
A simpler transfer layer protocol that does not provide the same level of data management as TCP, but is faster as a result
**PDU*** is called a _datagram_
- Encapsulates data from the application layer
- Pronivdes **NO** guarentee of message delivery
- Pronivdes **NO** guarentee of message delivery order
- Provides no built-in congestion avoidance or flow control mechanisms
- Provides no connection state tracking, it is a _connectionless_ protocol

**Datagram headers**
- Source Port: Port number of the service on the device sending the request
- Destination Port: Port number of the service on the device recieving the request
- Length: Number of bits within the data paylaod
- Checksum: Similar to other layers, provides error detectio (missing data or corrupted data) using a checksum

### Pros and cons of TCP and UDP ###
TCP is good for applications that need to have all the data delivered in the correct order for the app to make sense (something like an email, for example)
TCP Pros:
- **Connection-oriented:** doesn't start sending data until a connection has been established between application processes
- **Data management:** provides guarentees of data integrity, de-duplication, in-order delivery, and retransmission of lost data
- **Built-in flow control and congestion avoidance:** optimizes efficiency of data transfer without need for developer implentation of similar systems
- **Generally:** provides many data managment services abstracted from the application level
TCP Cons:
- **Very complicated:** there is a lot going on with TCP under the hood
- **Latency:** TCP suffers from increased latency due to all the data-management and network traffic safeguards it has in place
- **Limited customizablity:** developers cannot implement their own versions of any of the above technologies as they are built into TCP

UDP Pros:
UDP is good for latency sensitive applications that do care as much about losing pieces of data, data being delivered out of order etc.. (like a video call app)
- **Simple:** it will begin sending data to the destination without any of the data reliablity steps needing to take place
- **Faster:** it does not need to process the data as intensively as TCP, which means less latency, and faster delivery
- **customizable:** developers can reintroduce some feature sets of TCP without relying on all of them, something data retransmission or congestion avoidance perhaps

UDP Cons:
- **No reliabiliy built in:** there is no data transfer realibility built in aside from a checksum
- **No transfer optimizations:** things like congestion avoidance or control flow are not built in

## URL ##
Unique Resource Locator (URL) is an identifier used to connect with servers
- Allows you to specify which type of protocol be used to connect to the server with a **scheme**
- Offers an alternative to typing in the IP address of the server with a **host** (also refered to as the domain)
- Allows you to specify which port should be used to send the message
- Allows you to specify which resources on the server you wish to access with a **path**
- Allows you to submit data as **parameters** to the server through the uses of special syntax called **query strings**
  - Initiated in the url with a question mark (`?`)
  - Can use `&` to seperate multiple paramters in a query string

### Be able to identify different componentfs of a URL ###
ex) `http://www.example.com:88/home?item=book`
scheme: `http`
host: `www.example.com`
port: `88`
path: `home`
parameters: `?item=book`

### Understand URL encoding ###
URLs are designed to only accept certain characters in the standard 128 character ACII set. Anything else must be encoded (replaced with other characters)
- space --> `%20`
- $ --> `%24`
- £ --> `%C2%A3`

Query strings have some additional syntax, for example, a space between words can either be `%20` or `+`

## HTTP ##
### What are HTTP requests and reponses ###
HTTP - A system of rules that serve as the link between applications and the transfer of hypertext documents
  - **Requests** --> This is an HTTP message sent a client to a server
  - **Responses** --> This is an HTTP message sent from a server to a client

 ### HTTP Reqeust ###
 Two most common HTTP requests are `GET` and `POST`
 - `GET` - Used to retrieve information from the server (most common)
 - `POST` - used to send information to the server

**GET Requests**
Used to retrieve infromation from the server
- Typically in the initiated in the browser by either clicking a link or entering a URL or IP address in an address bar
- Used to retrieve information from the server (most common)
- Resposnse can be anything, but if its HTML, your browser will automatically issue GET requests to any other resources references in the HTML
- `GET` Requests can also be made programatically

**POST Requests**
Used when we want to iniate some action on the server or send data to the server
- Typically used when submitting a form within a browser
- Allows us to send much larger information that with query strings
- Data sent with a `POST` is contained within the `HTTP` body

**Components of HTTP Requests**
Method + URL + HTTP version + Headers + Body
- METHOD + URL + HTTP version --> is referred to as the request line and is the first line of the request
  - Method - A set of meta-data we include in our request to give the server info on how to handle it
  - Headers - Supplemental information about the request/response that provides useful details to server/client (colon seperated name-value pairs as plain text)
  - HTTP version - the verison of HTTP that is to be used for the request
- Note that if a request is made _AFTER_ a connection has already been made to a host, the full URL is not needed, and the **path** can be provided instead
- In HTTP/1.1 and later, the `Host` (e.g www.excample.com) is a required header field
- Only required Headders are `Host` and `Connection` for HTTP/1.1 and later

**Components of HTTP Response**
Status line + Headers + Body
- Status line includes HTTP version, a status code, and status text
- Body includes any HTML, JSON etc..
- Only required header is `Date`, and `Content-Type` is recommended

**Status codes for HTTP responses**
Three digit numbers that the server sends back after recieving a request signifying the status of the request
- 200 --> OK - The request was handled successfully
- 302 --> Found - The requested resource has changed temporarily. Usually results in a redirect
- 404 --> Not Found - The requested resource cannot be found
- 500 --> Internal Server Error - The server has encountered a generic error

## Statefulness ##
"State" in the context of web apps refers to the current configuaration of elements within the appliaction at a given moment

A **stateless** protocol is one that is designed in a way where each request/response pair is indepenedent
- HTTP is a stateless protocol
- Each request/response has no knowledge of any previous ones
- Is easier to use and consumes less resources as the server does not need to hang on to state between requests
  - If a requests breaks, the server does not have to do any cleanup

### Stateful web apps ###
A stateful web app is one that appears to retain an amount of persiting data between HTTP requests
- Since HTTP is a stateless protocol, developers often use certain tricks to give the client and psuedo-stateful experience

**Techniques**

Server can send a unique token to the client
- Client then appends this token to all of its requests to get a modified response (i.e a logged in web page)
- Token is called a **Session Identifier (session ID)**
  - A session is a period of interaction between a user and a web program
- We can use our modified web pages based on the session ID to display a faux persistant view between client page refereshes

A common way to store the Session ID is within a cookie, which is a small file stored on within the client browser
- Cookies contain session information, including, but not limited to Session ID
- Cookie does not contain actual session data, that will be hosted on the the server

AJAX
- Asyncrhonous JavaScript and XML
- Allows browsers to issue requests and process responses without a full page requests
- Less expensive becaues it doesn't require the server to re-render the whole webpage, only the part that needs to be updated
- Requesta and responses are sent as normal, but each response is processed by server-side callback function that is responsible for updating the HTML accordingly
