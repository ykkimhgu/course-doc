# Window Socket Programming

## Window: Socket programming with winsock



#### Requirement

* Window OS only
* C programming
* Link: `ws2_32.lib`&#x20;
* Include: `winsock2.h` &#x20;

![](<../../.gitbook/assets/image (116).png>)



Reading List

1. [열혈TCP/IP 윈도우 기반 구현하기 ](https://1d1cblog.tistory.com/322)
2. [Winsock Programming Tutorial](https://www.binarytides.com/winsock-socket-programming-tutorial/)

## Client

We wil learn how to  :

1. Initialize Winsock
2. Create a socket
3. Connect to remote server
4. Send some data
5. Receive a reply



### Initialising Winsock

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



### Creating Socket

The [`socket()`](https://docs.microsoft.com/en-us/windows/win32/api/winsock2/nf-winsock2-socket) function is used to create a socket.

`SOCKET WSAAPI socket( [in] int af, [in] int type, [in] int protocol );`

* Address Family : AF\_INET (this is IP version 4)&#x20;
* Type : SOCK\_STREAM (this means connection oriented TCP protocol)&#x20;
* Protocol : 0 \[ or IPPROTO\_TCP , IPPROTO\_UDP ]

```c
	WSADATA wsa;
	SOCKET s;
	////////////////////////////////////////////////////////	
	//Create a socket
	if((s = socket(AF_INET , SOCK_STREAM , 0 )) == INVALID_SOCKET)
	{
		printf("Could not create socket : %d" , WSAGetLastError());
	}

	printf("Socket created.\n");
```



### Connect to Server

Need (1) IP address (2) Port number to connect

Use `sockaddr_in` structure with proper values filled in.

```c
////////////////////////////////////////////////////////
// Connect to server

	struct sockaddr_in server;
	server.sin_addr.s_addr = inet_addr("74.125.235.20");
	server.sin_family = AF_INET;
	server.sin_port = htons( 80 );

	//Connect to remote server
	if (connect(s , (struct sockaddr *)&server , sizeof(server)) < 0)
	{
		puts("connect error");
		return 1;
	}
	
	puts("Connected");
	
	

/////////////////////////////////////////////////
// Defined in <winsock2.h>
/////////////////////////////////////////////////

// IPv4 AF_INET sockets:
struct sockaddr_in {
    short            sin_family;   // e.g. AF_INET, AF_INET6
    unsigned short   sin_port;     // e.g. htons(3490)
    struct in_addr   sin_addr;     // see struct in_addr, below
    char             sin_zero[8];  // zero this if you want to
};


typedef struct in_addr {
  union {
    struct {
      u_char s_b1,s_b2,s_b3,s_b4;
    } S_un_b;
    struct {
      u_short s_w1,s_w2;
    } S_un_w;
    u_long S_addr;
  } S_un;
} IN_ADDR, *PIN_ADDR, FAR *LPIN_ADDR;


struct sockaddr {
    unsigned short    sa_family;    // address family, AF_xxx
    char              sa_data[14];  // 14 bytes of protocol address
};
```

Function `inet_addr` is a very handy function to convert an IP address to a long format.

```
server.sin_addr.s_addr = inet_addr("74.125.235.20");
```

``

### Sending Data, Receive Data

Use `send(  ) `  to send some data

Use `recv()` to receive data



```c
	//////////////////////////////////////////////////////
	char *message;
	message = "GET / HTTP/1.1\r\n\r\n";
	if( send(s , message , strlen(message) , 0) < 0)
	{
		puts("Send failed");
		return 1;
	}	
	puts("Data Send\n");

		
	//////////////////////////////////////////////////////
	char server_reply[2000];
	int recv_size;
	//Receive a reply from the server
	if((recv_size = recv(s , server_reply , 2000 , 0)) == SOCKET_ERROR)
	{
		puts("recv failed");
	}	
	puts("Reply received\n");

	//Add a NULL terminating character to make it a proper string before printing
	server_reply[recv_size] = '\0';
	puts(server_reply);
```

### Close Socket

Use closesocket() and WSACleanup to unload the library

```c
closesocket(s);
WSACleanup();
```

### Example code for Client

```c
/*
	Example code for Client TCP socket
*/

#include<stdio.h>
#include<winsock2.h>

#pragma comment(lib,"ws2_32.lib") //Winsock Library

int main(int argc , char *argv[])
{
	WSADATA wsa;
	SOCKET s;
	struct sockaddr_in server;
	char *message , server_reply[2000];
	int recv_size;

	////////////////////////////////////////////////////////
	// Initialize
	printf("\nInitialising Winsock...");
	if (WSAStartup(MAKEWORD(2,2),&wsa) != 0)
	{
		printf("Failed. Error Code : %d",WSAGetLastError());
		return 1;
	}	
	printf("Initialised.\n");
	
	////////////////////////////////////////////////////////	
	//Create a socket
	if((s = socket(AF_INET , SOCK_STREAM , 0 )) == INVALID_SOCKET)
	{
		printf("Could not create socket : %d" , WSAGetLastError());
	}

	printf("Socket created.\n");
	
	////////////////////////////////////////////////////////
	// Connect to server
	server.sin_addr.s_addr = inet_addr("74.125.235.20");
	server.sin_family = AF_INET;
	server.sin_port = htons( 80 );

	//Connect to remote server
	if (connect(s , (struct sockaddr *)&server , sizeof(server)) < 0)
	{
		puts("connect error");
		return 1;
	}
	
	puts("Connected");
	
	////////////////////////////////////////////////////////	
	//Send some data
	message = "GET / HTTP/1.1\r\n\r\n";
	if( send(s , message , strlen(message) , 0) < 0)
	{
		puts("Send failed");
		return 1;
	}
	puts("Data Send\n");
	
	////////////////////////////////////////////////////////
	//Receive a reply from the server
	if((recv_size = recv(s , server_reply , 2000 , 0)) == SOCKET_ERROR)
	{
		puts("recv failed");
	}
	
	puts("Reply received\n");

	//Add a NULL terminating character to make it a proper string before printing
	server_reply[recv_size] = '\0';
	puts(server_reply);	
	
	////////////////////////////////////////////////////////
	// Close socket
	closesocket(s);
	WSACleanup();
	////////////////////////////////////////////////////////
	
	return 0;
}
```

## Server



1. Open a socket (see client)
2. Bind to address and port.
3. Listen for incoming connections.
4. Accept connections
5. Read/Send

### Bind&#x20;

We bind a socket to a particular IP address and a certain port number. We ensure that all incoming data which is directed towards this port number is received by this application

```c
//Prepare the sockaddr_in structure
	server.sin_family = AF_INET;
	server.sin_addr.s_addr = INADDR_ANY;
	server.sin_port = htons( 8888 );
	
	//Bind
	if( bind(s ,(struct sockaddr *)&server , sizeof(server)) == SOCKET_ERROR)
	{
		printf("Bind failed with error code : %d" , WSAGetLastError());
	}
	
	puts("Bind done");
```

### Listen for connection

we need to do is listen for connections. For this we need to put the socket in listening mode. Function `listen` is used to put the socket in listening mode

```
//Listen
listen(s , 3);
```

### Accept connection

```
// Some code
//Accept and incoming connection
	puts("Waiting for incoming connections...");
	
	c = sizeof(struct sockaddr_in);
	new_socket = accept(s , (struct sockaddr *)&client, &c);
	if (new_socket == INVALID_SOCKET)
	{
		printf("accept failed with error code : %d" , WSAGetLastError());
	}
	
	puts("Connection accepted");
```

### Sending Receiving data

Replying to the client

* We had to use a getchar because otherwise the output would scroll out of the client terminal without waiting

```c
// Some code
//Reply to client
	message = "Hello Client , I have received your connection. But I have to go now, bye\n";
	send(new_socket , message , strlen(message) , 0);
	
	getchar();
```

### Example code for Live Server

```c
/*
	Live Server on port 8888
*/
#include<io.h>
#include<stdio.h>
#include<winsock2.h>

#pragma comment(lib,"ws2_32.lib") //Winsock Library

int main(int argc , char *argv[])
{
	WSADATA wsa;
	SOCKET s , new_socket;
	struct sockaddr_in server , client;
	int c;
	char *message;

	printf("\nInitialising Winsock...");
	if (WSAStartup(MAKEWORD(2,2),&wsa) != 0)
	{
		printf("Failed. Error Code : %d",WSAGetLastError());
		return 1;
	}
	
	printf("Initialised.\n");
	
	//Create a socket
	if((s = socket(AF_INET , SOCK_STREAM , 0 )) == INVALID_SOCKET)
	{
		printf("Could not create socket : %d" , WSAGetLastError());
	}

	printf("Socket created.\n");
	
	//Prepare the sockaddr_in structure
	server.sin_family = AF_INET;
	server.sin_addr.s_addr = INADDR_ANY;
	server.sin_port = htons( 8888 );
	
	//Bind
	if( bind(s ,(struct sockaddr *)&server , sizeof(server)) == SOCKET_ERROR)
	{
		printf("Bind failed with error code : %d" , WSAGetLastError());
		exit(EXIT_FAILURE);
	}
	
	puts("Bind done");

	//Listen to incoming connections
	listen(s , 3);
	
	//Accept and incoming connection
	puts("Waiting for incoming connections...");
	
	c = sizeof(struct sockaddr_in);
	
	while( (new_socket = accept(s , (struct sockaddr *)&client, &c)) != INVALID_SOCKET )
	{
		puts("Connection accepted");
		
		//Reply to the client
		message = (char*)"Hello Client , I have received your connection. But I have to go now, bye\n";
		send(new_socket , message , strlen(message) , 0);
	}
	
	if (new_socket == INVALID_SOCKET)
	{
		printf("accept failed with error code : %d" , WSAGetLastError());
		return 1;
	}

	closesocket(s);
	WSACleanup();
	
	return 0;
}
```

{% hint style="info" %}
Trouble Shooting:

inet_addr''  use inet_pton() or InetPton()\~ :  [여기 클](https://m.blog.naver.com/PostView.naver?isHttpsRedirect=true\&blogId=luckywjd7\&logNo=220872794096)릭&#x20;

const char\[] error:   [여기 클릭 ](http://egloos.zum.com/kim0522ms/v/6438724)
{% endhint %}

Now run the program in 1 terminal , and open 3 other terminals. From each of the 3 terminal do a telnet to the server port.

Run telnet like this. It will launch the interactive prompt.

```
C:\>telnet
```

At the telnet shell, run the command "open localhost 8888". This command will try to connect to localhost on port number 8888.

```
Welcome to Microsoft Telnet Client
Escape Character is 'CTRL+]'
Microsoft Telnet> open localhost 8888
```

Next you should see the following message at the telnet prompt. This message is received from the socket server running on port 8888.

```
Hello Client , I have received your connection. But I have to go now, bye
```

On the other hand, the server terminal would show the following messages, indicating that a client connected to it.

```
Initialising Winsock...Initialised.
Socket created.
Bind done
Waiting for incoming connections...
Connection accepted
Connection accepted
```

So now the server is running nonstop and the telnet terminals are also connected nonstop. Now close the server program.

All telnet terminals would show "Connection to host lost."\
Good so far. But still there is not effective communication between the server and the client.

The server program accepts connections in a loop and just send them a reply, after that it does nothing with them. Also it is not able to handle more than 1 connection at a time. So now its time to handle the connections , and handle multiple connections together.

###

## Handle multiple connections

To handle every connection we need a separate handling code to run along with the main server accepting connections.\
One way to achieve this is using threads. The main server program accepts a connection and creates a new thread to handle communication for the connection, and then the server goes back to accept more connections.

On Linux threading can be done with the pthread (posix threads) library. It would be good to read some small tutorial about it if you dont know anything about it. However the usage is not very complicated.

We shall now use threads to create handlers for each connection the server accepts.&#x20;



### Linux

```c
#include<stdio.h>
#include<string.h>    //strlen
#include<stdlib.h>    //strlen
#include<sys/socket.h>
#include<arpa/inet.h> //inet_addr
#include<unistd.h>    //write
 
#include<pthread.h> //for threading , link with lpthread
 
void *connection_handler(void *);
 
int main(int argc , char *argv[])
{
    int socket_desc , new_socket , c , *new_sock;
    struct sockaddr_in server , client;
    char *message;
     
    //Create socket
    socket_desc = socket(AF_INET , SOCK_STREAM , 0);
    if (socket_desc == -1)
    {
        printf("Could not create socket");
    }
     
    //Prepare the sockaddr_in structure
    server.sin_family = AF_INET;
    server.sin_addr.s_addr = INADDR_ANY;
    server.sin_port = htons( 8888 );
     
    //Bind
    if( bind(socket_desc,(struct sockaddr *)&server , sizeof(server)) < 0)
    {
        puts("bind failed");
        return 1;
    }
    puts("bind done");
     
    //Listen
    listen(socket_desc , 3);
     
    //Accept and incoming connection
    puts("Waiting for incoming connections...");
    c = sizeof(struct sockaddr_in);
    while( (new_socket = accept(socket_desc, (struct sockaddr *)&client, (socklen_t*)&c)) )
    {
        puts("Connection accepted");
         
        //Reply to the client
        message = "Hello Client , I have received your connection. And now I will assign a handler for you\n";
        write(new_socket , message , strlen(message));
         
        pthread_t sniffer_thread;
        new_sock = malloc(1);
        *new_sock = new_socket;
         
        if( pthread_create( &sniffer_thread , NULL ,  connection_handler , (void*) new_sock) < 0)
        {
            perror("could not create thread");
            return 1;
        }
         
        //Now join the thread , so that we dont terminate before the thread
        //pthread_join( sniffer_thread , NULL);
        puts("Handler assigned");
    }
     
    if (new_socket<0)
    {
        perror("accept failed");
        return 1;
    }
     
    return 0;
}
 
/*
 * This will handle connection for each client
 * */
void *connection_handler(void *socket_desc)
{
    //Get the socket descriptor
    int sock = *(int*)socket_desc;
    int read_size;
    char *message , client_message[2000];
     
    //Send some messages to the client
    message = "Greetings! I am your connection handler\n";
    write(sock , message , strlen(message));
     
    message = "Now type something and i shall repeat what you type \n";
    write(sock , message , strlen(message));
     
    //Receive a message from client
    while( (read_size = recv(sock , client_message , 2000 , 0)) > 0 )
    {
        //Send the message back to client
        write(sock , client_message , strlen(client_message));
    }
     
    if(read_size == 0)
    {
        puts("Client disconnected");
        fflush(stdout);
    }
    else if(read_size == -1)
    {
        perror("recv failed");
    }
         
    //Free the socket pointer
    free(socket_desc);
     
    return 0;
}
```

This one looks good , but the communication handler is also quite dumb. After the greeting it terminates. It should stay alive and keep communicating with the client.

One way to do this is by making the connection handler wait for some message from a client as long as the client is connected. If the client disconnects , the connection handler ends.

So the connection handler can be rewritten like this&#x20;





The above connection handler takes some input from the client and replies back with the same. Simple! Here is how the telnet output might look

```
$ telnet localhost 8888
Trying 127.0.0.1...
Connected to localhost.
Escape character is '^]'.
Hello Client , I have received your connection. And now I will assign a handler for you
Greetings! I am your connection handler
Now type something and i shall repeat what you type 
Hello
Hello
How are you
How are you
I am fine
I am fine
```

So now we have a server thats communicative. Thats useful now.

**Linking the pthread library**

When compiling programs that use the pthread library you need to link the library. This is done like this :

```
$ gcc program.c -lpthread
```
