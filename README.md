# python-socket-programming
Python socket programming example

Socket programming in python is used to connect to two nodes(server,coumputer,router) in a network so they can communicate with each other.
Or we can say Socket provide bidirectional communication between two endpoints in which one is server and other is client.
Socket may communicate within same process, between processes on same machine or between processes on different machines.

We will see a program how Socket server and client communicate with each other in this tutorial.
Below step to be considered while communication.
1. Sockect Server program execute first and listen for client request.
2. Socket Client program initiate conversation first.
3. Socket Server program respond to client requests.

For socket programming in python we need need to import socket module.

We will create two files name sockect_server.py and socket_client.py for coding.

Program for socket_server.py
=================================================================
```
# first of all import the socket library
import socket

<code>
def server_program():
    #create a socket object name it server_socket
    server_socket = socket.socket()
    print ("Socket successfully created")

    # get the hostname
    host = socket.gethostname()
    port = 1234  # reserver any port in your computer, initiate port no above 1024

    # The bind() function takes hostname and port as argument
    server_socket.bind((host, port))  # bind host address and port together
    print ("socket binded to ip %s port %s" %(host,port))

    # put socket into listning mode and configure how many client the server can listen simultaneously
    server_socket.listen(2)
    print ("socket is listening")  
    
    conn, address = server_socket.accept()  # make connection with client and accept new connection
    print("Connection from: " + str(address))
    while True:
        # receive data stream. it won't accept data packet greater than 1024 bytes
        data = conn.recv(1024).decode()
        if not data:
            # if data is not received break
            break
        print("data from connected user: " + str(data))
        data = input(' -> ')
        conn.send(data.encode())  # send data to the client

    conn.close()  # close the connection


if __name__ == '__main__':
    server_program()
```
========================================================================


Program for socket_client.py

========================================================================
```
import socket
def client_program():
    host = socket.gethostname()  # as both code is running on same pc
    port = 1234  # socket server port number

    client_socket = socket.socket()  # Create a socket object 
    client_socket.connect((host, port))  # connect to the server

    message = input(" -> ")  # take input

    while message.lower().strip() != 'bye':
        client_socket.send(message.encode())  # send message
        data = client_socket.recv(1024).decode()  # receive data stream. it won't accept data packet greater than 1024 bytes
        print('Data Received from server: ' + data)  # show in terminal

        message = input(" -> ")  # again take input

    client_socket.close()  # close the connection


if __name__ == '__main__':
    client_program()
```	
=========================================================================


Step for execution:
First run python sockect_server.py
O/P
```
python .\socket_server.py
Socket successfully created
socket binded to ip ALOSING3-5D5V4 port 1234
socket is listening
```
In second terminal run python socket_client.py
Client program ask for user input. write "Hi Server"
O/P
```
PS C:\pythonic> python .\socket_client.py
 -> Hi Server
 ```
Now go to terminal where you had run sockect_server.py you can see client message there and it ask for user input to respond to client.
O/P 
```
C:\pythonic> python .\socket_server.py
Socket successfully created
socket binded to ip ALOSING3-5D5V4 port 1234
socket is listening
Connection from: ('10.65.52.31', 57331)
data from connected user: Hi Server
 ->
```
This is How Client Server communicate using socket.
Notice that socket server is running on 1234 port. But port is assigned randomly to socket client by client connect method. 

To view current state of sockets on your host use ```netstat -an```

![image](https://user-images.githubusercontent.com/44195690/112668719-d8db2580-8e84-11eb-8af8-cb7f3f907703.png)

Point to be noted.
Syntax for socket creation
```
s = socket.socket (socket_family, socket_type, protocol=0)
```
1.socket_family: AF_INET is the Internet address family for IPv4. 
2.socket_type:SOCK_STREAM is the socket type for TCP, the protocol that will be used to transport our messages in the network.
3.protocol − This is usually left out, defaulting to 0.

Server socket methods:
1. server_socket.bind((host, port)): This method binds address (host, port number) to socket.
2. server_socket.listen(2): This method sets up and start TCP listener. Max two connection is allowed.
3. conn, address = server_socket.accept(): This passively accept TCP client connection, waiting until connection arrives (blocking).

Client Socket Methods
1. connect():This method actively initiates TCP server connection.

General Socket Methods
1. recv(): This method receives TCP message.
2. send(): This method transmits TCP message
3. close(): This method closes socket
4. gethostname():Returns the hostname.

Thats all for Socket programming in Python. Hope I very much clear to explain.

