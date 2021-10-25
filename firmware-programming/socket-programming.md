# Socket Programming



## TCP/IP Programming in C/C++

we will discuss the socket API and support for TCP and UDP communications between end hosts. Socket programing is the key API for programming distributed applications on the Internet.



#### Server and Client

Clients normally communicates with one server at a time. From a server’s perspective, at any point in time, it is not unusual for a server to be communicating with multiple clients. Client need to know of the existence of and the address of the server, but the server does not need to know the address of (or even the existence of) the client prior to the connection being established

Client and servers communicate by means of multiple layers of network protocols. In this course we will focus on the TCP/IP protocol suite.

![](<../.gitbook/assets/image (115).png>)



[Read socket programming](https://www.cs.dartmouth.edu/\~campbell/cs60/socketprogramming.html)

![](<../.gitbook/assets/image (113).png>)

### Window

#### Socket programming with winsock

![](<../.gitbook/assets/image (116).png>)



Reading&#x20;

1. [열혈TCP/IP 윈도우 기반 구현하기 ](https://1d1cblog.tistory.com/322)
2. [Winsock Programming Tutorial](https://www.binarytides.com/winsock-socket-programming-tutorial/)

###

#### Requirement

* Window OS only
* C programming
* Link: `ws2_32.lib`&#x20;
* Include: `winsock2.h` &#x20;



In the above example we learned how to :

1. Create a socket
2. Connect to remote server
3. Send some data
4. Receive a reply



#### Initialising Winsock

Winsock first needs to be initialiased with

`int WSAAPI WSAStartup( [in] WORD wVersionRequested, [out] LPWSADATA lpWSAData );`

Second one is a WSADATA structure



```c
/*
	Initialise Winsock
*/

#include<stdio.h>
#include<winsock2.h>

// It automatically links this library to compiler
#pragma comment(lib,"ws2_32.lib") //Winsock Library

int main(int argc , char *argv[])
{
	WSADATA wsa;
	
	printf("\nInitialising Winsock...");
	if (WSAStartup(MAKEWORD(2,2),&wsa) != 0)
	{
		printf("Failed. Error Code : %d",WSAGetLastError());
		return 1;
	}
	
	printf("Initialised.");

	return 0;
}
```



#### Creating Socket

The [`socket()`](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-socket) function is used to create a socket.

`SOCKET WSAAPI socket( [in] int af, [in] int type, [in] int protocol );`

* Address Family : AF\_INET (this is IP version 4)&#x20;
* Type : SOCK\_STREAM (this means connection oriented TCP protocol)&#x20;
* Protocol : 0 \[ or IPPROTO\_TCP , IPPROTO\_UDP ]

```c
/*
	Create a TCP socket
*/

#include<stdio.h>
#include<winsock2.h>

#pragma comment(lib,"ws2_32.lib") //Winsock Library

int main(int argc , char *argv[])
{
	WSADATA wsa;
	SOCKET s;

	printf("\nInitialising Winsock...");
	if (WSAStartup(MAKEWORD(2,2),&wsa) != 0)
	{
		printf("Failed. Error Code : %d",WSAGetLastError());
		return 1;
	}
	
	printf("Initialised.\n");
	////////////////////////////////////////////////////////	
	
	if((s = socket(AF_INET , SOCK_STREAM , 0 )) == INVALID_SOCKET)
	{
		printf("Could not create socket : %d" , WSAGetLastError());
	}

	printf("Socket created.\n");
	////////////////////////////////////////////////////////

	return 0;
}
```



### Arduino

