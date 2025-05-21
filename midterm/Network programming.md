#### Explain the various protocols in TCP / IP model with drawing
![[Pasted image 20250410033850.png]]
ARP : Address Resolution protocol
RARP : Reverse Address Resolution protocol
IP : internet protocol
ICMP : internet control message protocol
IGMP : internet group management protocol
TCP : transmission control protocol 
UDP : user datagram protocol

-------
#### Describe Little endian , big endian byte order 
![[Pasted image 20250410034149.png]]
The terms "little endian" and "big endian" indicates which end of the multibyte value , the little end or the big end , is stored at the starting address of the value.
Little-endian byte order is an incremental addressing (increasing from right to left)
big-endian byte order is a decremental addressing (increasing from left to right)

---
#### State the socket address data structure for IPv4
an IPV4 socket address structure , commonly called an "internet socket address structure" is named "sockaddr_in" and is defined by including the "<netinet/in.h>" header it is shown as following 
```c
struct in_addr{
	in_addr_t s_addr;
}
struct sockaddr_in{
	uint8_t sin_len;
	sa_family_t sin_family;
	in_port_t sin_port;
	struct in_addr sin_addr;
	char sin_zero[8];
}
```

----
#### How to identify the connection between two machines
we can identify the connection between two machines by IP address and port number for each machine Connection ID = {IPs , PORTs , IPd , PORTd}

---
#### how to solve TCP issue that receiver has to know the number of bytes
- we can use UDP sends and receives datagrams for applications , A datagram is a unit of information (certain number of byes and information that is specified by the sender) that travels from the sender to the receiver unlike TCP 
- send with message header with fixed size at which we add size of message and data type ... etc
---
#### Explain the socket address structure passed from process to kernel
The three functions bind , connect and sendto pass a socket address structure from the process to the kernel , one argument to those functions is the pointer to the socket address structure and another argument is the integer size of the structure.

![[Pasted image 20250410042530.png]]