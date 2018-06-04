# TwoPhaseCommit
OVERVIEW
Develop a program to implement a two phase commit application that consists of a server, 3 clients acting as transaction participants and a coordinator. Every client will connect to the server over a socket connection. A separate thread is created for each connected client. The program is written in Java language that is supported by Eclipse Integrated Development Environment (IDE).
The program consists of six  classes

1.	ServerApplet - it implements Runnable interface which executes the Server thread and relays the message from the client(s) to           Coordinator and vice-versa.
2.	ServerThread - Manages each Client and Coordinator connection.
3.	ClientApplet -  it implements Runnable interface which executes the Client Thread and sends messages to the Coordinator which is         relayed through the Server
4.	ClientThread- Receives and handles all the messages from the Coordinator which is relayed through the Server.
5.	ClientCoordinator- Sends arbitrary string to clients and GLOBALCOMMIT/GLOBALABORT actions accordingly to the clients when response       for the arbitrary string is received from the clients
6.	ClientCoordinatorThread- Receives and handles all the messages from the Coordinator which is relayed through the Server.

INSTALLATION
Eclipse Java EE IDE for Web Developers.
Version: Oxygen Release (4.7.0)
Build id: 20170620-1800
Install from: http://www.eclipse.org/downloads
Note: Also install the latest JDK and JRE

INSTRUCTIONS TO COMPILE
	Server
1.	Run the ServerApplet class as a java applet.
2.	The Server is bound to the port 8080 and it is started.
3.	The Server waits for client to connect.

ClientCoordinator:
Run the ClientCoordinator as a Java Applet and a chat interface is displayed. 
Click the connect button to connect with the server.
Text Field: Once the clients are connected, the text field is used to enter the arbitrary string that needs to be sent to the clients
Send Button: Sends the message in Text Field to all the clients in order to receive the response of Commit or Abort
Log Off: To log off from the application.

Client
1.	Run the ClientApplet as a java Applet and a chat Interface is displayed.
The chat interface has the following buttons
i.	Connect Button : On-click of this button, the client is connected to the server.
ii.	TextField :  When the connection is established, the client is asked to register a name which is entered in the TextField. If the entered name is valid (valid format displayed in chat window), the name is registered at the server. 
iii.	Log Off Button : The log off button removes the connected client from the chat room. The client details are removed from the server and the applet window is closed to exit from the chat room.
iv.	COMMIT button: To provide response for Coordinator’s message with an arbitrary string
v.	ABORT button: To provide response for Coordinator’s message with an arbitrary string


Work Flow:
Server program is executed and waits for Coordinator and Clients to connect
Coordinator program is executed and connected with the server
Client Program is executed thrice to simulate the connection of 3 clients. The clients will be in INIT state at this point.
Once the clients are connected, Coordinator sends an arbitrary string to the clients via Server and enters the WAIT state
Clients receive the arbitrary string and choose to either Commit or Abort the transaction by clicking the respective buttons. It then goes to READY state and waits for the response back from coordinator.
If a response is received from all 3 clients within a finite time and if all the clients chose to Commit, then a GLOBALCOMMIT is sent to all the clients
If a response is received from all 3 clients within a finite time and if one or more clients chose to ABORT, then a GLOBALABORT is sent to all the clients
If a response is not received within a finite time by any of the 3 clients, then a GLOBALABORT is initiated by the Coordinator.
Based on the response received from the server, the arbitrary string is either saved to the text file or discarded by the clients.

On the Contrary, if a decision from the coordinator is not received within a finite time from the coordinator, then the code is designed to do the following communication between the clients:
1.	The client which timed out, requests the status of other clients inorder to determine the course of action.
2.	If all the clients responded with Ready State, then the clients wait for the coordinator to recover
3.	If one or more clients responded with a INIT or a GLOBALABORT state, then the client aborts the transaction
4.	If one or more clients responded with a GLOBALCOMMIT state, then the client commits the transaction



Assumptions: 
1.	The Server and client can run only in Server “localhost” and port number “8080” unless it is changed in the program.
2.	The clients are assumed to be in INIT state until it chose to vote COMMIT or ABORT

Implementations:
1.	The messages that are sent across the Client-Server uses HTTP formats and commands. Messages sent to the server is sent by POST method (Server shows HTTP message format) and messages sent from the server uses the POST Method.
2.	Regular expression is used to invalidate any bad name when a client registers at the server.
