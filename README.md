# 2-phase-distributed-commit-2PC-protocol
**2-phase distributed commit (2PC) protocol**

**Introduction**

This project is implemented on Python language. We have used python’s socket library for communicating between coordinator and participants. In this project we explored how a 2PC Protocol works in a distributed system.

**Implementation:**

We are using a transaction mechanism to demonstrate the working of 2PC protocol. Here we have “User1” and “User2”, the Client can request to transfer some amount of money to transfer from one user to another user. The Coordinator handles the request and syncs with the Participants to process the transaction using 2PC protocol.

**1.	Coordinator**
   
-	Coordinator opens 2 socket connections one for client and one for participants.
  
-	As soon as a Client is connected it start a thread to process any request received from the Client. The same process is done for both the Participants.
  
-	When the Coordinator receives a request from Client, it begins the transaction process by sending “PREPARE” message to the Participants. Based on the Vote it received from the Participants, the Coordinator either sends a “COMMIT” or “ABORT” message to the Participants.
  
-	After the Commit or Abort process, the Coordinator responds back to the Client with a successful transaction message or failed transaction message.
  
-	Every step of the Coordinator is saved on disk file “coordinator.json”.
  
**2.	Client**
   
-	After the Client is connected to the Coordinator, it create 2 threads one to receive message and one to send message to the Coordinator.
  
-	Based on the user input the Client send a “TRANSFER” request to the Coordinator.
  
-	When it receives any response from the Coordinator it prints them to the terminal.
  
**3.	Participant**

-	Once the participant is connected to the Coordinator, it receives a name for itself (“Participant 1” or “Participant 2”).
  
-	It create 2 threads one to receive message and one to send message to the Coordinator.
  
-	When it receives a “PREPARE” message, it either sends “VOTE:Y” or “VOTE:N”. The Coordinator then uses this votes to determine if it should commit or abort.
	
-	When it receives “COMMIT” message it saves transaction to the file. If it receives “ABORT” message it aborts all the process.
  
-	Every step of the Participants is saved on disk files “participant1.json” and “participant2.json”. This helps the Participants to resume it process when it crashes and comes back online.

**What we learned:**

-	How to 2PC protocol works in a distributed system.
  
-	How to implement multiple sockets.
  
-	How to rejoin a socket after crash.
  
**What issues we faced:**

-	One major issue we faced was to use 2 socket connection and sync the communication between them. We had to read multiple documentation to come up with a working solution.
  
-	The other issue we faced was to reconnect the participant to the socket and pick up the task it was working on before it crashed. We resolved this by assigning a name to each participant as soon as they connect to the coordinator. The coordinator keeps a record of the name of the participant with its address. And when a participant is disconnected it will remove the address from its record. When the participant will rejoin it will get the new address of the participant and assign the name to it.
 
**Screenshots:**

**1)	Coordinator fails to send prepare message**
 ![image](https://github.com/NehaMore2202/2-phase-distributed-commit-2PC-protocol/assets/154467395/fe0191b7-4f88-4069-b647-327d5900b60c)


**2)	Participant 2 fails to vote**
 ![image](https://github.com/NehaMore2202/2-phase-distributed-commit-2PC-protocol/assets/154467395/83f86e37-ccd4-41b4-8750-6b7656432300)

 
**3)	Participant 2 Votes No**
 ![image](https://github.com/NehaMore2202/2-phase-distributed-commit-2PC-protocol/assets/154467395/1aef701d-5fb0-41b2-8ab5-0d3badd3d12f)


**4)	All participants votes yes**
 ![image](https://github.com/NehaMore2202/2-phase-distributed-commit-2PC-protocol/assets/154467395/766a2a40-f162-4653-be85-503e75553a03)

 
 
