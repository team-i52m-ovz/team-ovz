# SystemRat

Functional description

The main idea of SystemRat is to collect both hardware and OS options and settings from all the machines from local network.
This application provides possibility to monitor and serve all machines, to determine possible problems with hardware and software and notify 
machine's user about possible solutions. It also notifies admin about possible checting from the user's side.
SystemRat's server also collects statistics about all the machines.

For network connection between client and srver java will be used. Main system calls will be made by C++ dll.
All system info will be collected by C++, passed in JSON to java via network to server. All collected info on server will be analyzed by the other dll.


# Database
Database diagram represent general table structure and does not contain all the fields definition

# SOLID
SystemScanerSOLID diagram shows that the main class of .dll that will lay on client machine will be IScaner.
Two classes that inherit this interface - OSScaner and HardwareScaner are responsible for scanning OS and hardware.
Hardware implementation will scan all the hardware settings and options and will create objects with all info for each hardware item.
The same for the OS part - each item will be described via instance of special class.
All info from this classes - both from the hardware and the OS - will be saved in the JSON formatt by synchronized JSONGenerator class.
The same SOLID principle we will see in network part, ui part and in the server logic.
Network part on the client side will only transfer JSON to the server and support heartbeat to let server know if apps on ecah machines are online.
So there will be at least two abstractions that will be responsible for this.
On the server part there will be responsible oject for the communication with clients, the other one will be communicating with DB.
We will need to represent info from JSON back to C++ objects to make some diagnostic about each client's machine.
All this info will be transfered to the ui part which will allow admin to easy navigate and investigate possible issues that diagnostic may find.

# GRASP
As far as our application will be producing and using a lot of objects, we definately need to structure all this info.
Of course, we should use polymorphic stuff here, make some factories that will create different types of objects 
(especially on the server side when recreating objects from the JSON file).
We need to store all this data in the DB so all this objects will be binded to each other to easily navigate through them.
SystemScanerGRASP shows two examples when we will need factories -> so the polymorphism.

# Architecture
SystemRat have several things to do :
1) Use C++ .dll to scan all needed info from the machine - hardware settings and OS.
2) Build JSON to transfer through the network
3) Pass the JSON to the Java for transferring.
4) Catch it on the server
5) Send all the info to the DB
6) Create objects from JSON
7) Pass them to C++ .dll for diagnostic

Of course, through all this time server should communicate with all its clients to be sure that all of them have sent requested info (if all are online)

This looks like microservice architecture. We have several instances that communicating with each other : .dll with client app, client with server, server with .dll and with DB.