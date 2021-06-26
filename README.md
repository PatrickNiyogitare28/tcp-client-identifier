# tcp-client-identifier
Client-server app
The following client-server application enables a client to connect to a server and send only one message. 
We are using socketio to achieve tcp connection between the server and the client.
The client will connect to the network using program in tcpClient.c
The network administrator will automatically know who has connected to the network using the program in tcpServer.c
We have to run the server first tcpServer.c to allow clients to connect to our server using tcpClient.c
To illustrate that packets can flow from client to server we will have to send a message from the client and our server will respond with the same message.
The packets are able to flow in either directions, from client to server and vise versa. 

<img src="https://github.com/PatrickNiyogitare28/tcp-client-identifier/blob/master/assets/screenshots/connection.png">

## Instructions
You need a linux environment to be able to run the application due to the fact that some networking programming libraries of c we are using are found in Linux GCC.
So to be able to run the program its good to have a linux environment(linux OS)
  - [x] Run file tcpServer.c
  - [x] Run then tcpClient.c
  - [x] Type a message in console of tcpClient.c

## Expectations
We expect the server to console the client who has connected to our network
The server should respond with the same message after the client has sent a message to a server


TCP sockets are used for communication between a server and a client process. The server’s code runs first, which opens a port and listens for incoming connection requests from clients. Once a client connects to the same (server) port, the client or server may send a message. Once the message is sent, whoever receives it (server or client) will process it accordingly.




## Explanation
Include the header files sys/socket.h and arpa/inet.h:
``
#include <sys/socket.h>
#include <arpa/inet.h>
``


Create a socket that returns a socket descriptor; this will be used to refer to the socket later on in the code:

``
int socket_desc = socket(AF_INET, SOCK_STREAM, 0);
``

The server-side code keeps the address information of both the server and the client in a variable of type sockaddr_in, which is a struct.
Initialize the server address by the port and IP:

``
struct sockaddr_in server_addr;
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(2000);
server_addr.sin_addr.s_addr = inet_addr("127.0.0.1");
``

``
bind(socket_desc, (struct sockaddr*)&server_addr, sizeof(server_addr);
``

Bind the socket descriptor to the server address:

``
bind(socket_desc, (struct sockaddr*)&server_addr, sizeof(server_addr);
``


Turn on the socket to listen for incoming connections:

``
listen(socket_desc, 1);
``

Store the client’s address and socket descriptor by accepting an incoming connection:

``
struct sockaddr client_addr;
int client_size = sizeof(client_addr);
int client_sock = accept(socket_desc, (struct sockaddr*)&client_addr, &client_size);
``

The server-side code stops and waits at accept() until a client calls connect().
Communicate with the client using send() and recv():

``
recv(client_sock, client_message, sizeof(client_message), 0);
send(client_sock, server_message, strlen(server_message), 0);
``

When recv() is called, the code stops and waits for a message from the client.
Close the server and client socket to end communication:

``
close(client_sock);
close(socket_desc);
``
# Client-side

## Client-side
## Explanation

Include header files, create a socket, and initialize the server’s address information in a variable of type sockaddr_in, similar to how it was done at the server-side:

``
#include <sys/socket.h>
#include <arpa/inet.h>
int socket_desc = socket(AF_INET, SOCK_STREAM, 0);
struct sockaddr_in server_addr;
server_addr.sin_family = AF_INET;
server_addr.sin_port = htons(2000);
server_addr.sin_addr.s_addr = inet_addr("127.0.0.1");
``

Send a connection request to the server, which is waiting at accept():
``
connect(socket_desc, (struct sockaddr*)&server_addr, sizeof(server_addr));
``
Communicate with the server using send() and recv():
``
send(socket_desc, client_message, strlen(client_message),0);

recv(socket_desc, server_message, sizeof(server_message),0);
``

The client waits for the server to send a message when recv() is called.
Close the socket:
``
close(socket_desc);
``
A deadlock will occur if both the client and the server are waiting for each other’s message at recv().

## Output images
The client has connect to the server and the server is consoling that their a new tcp connection on the network

<img src="https://github.com/PatrickNiyogitare28/tcp-client-identifier/blob/master/assets/screenshots/server-client.PNG">

The client has sent a message (Hello) to a server and the server is responding with the same message to show that packets can flow in either direction from client - server and vice versa.

# Author
patrickniyogitare28@gail.com

[![Twitter Badge](https://img.shields.io/badge/-@niyogitare-1ca0f1?style=flat&labelColor=1ca0f1&logo=twitter&logoColor=white&link=https://twitter.com/niyogitare)](https://twitter.com/niyogitare)

