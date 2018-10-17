# SystemRat

Functional description

The main idea of SystemRat is to collect both hardware and OS options and settings from all the machines from local network.
This application provides possibility to monitor and serve all machines, to determine possible problems with hardware and software and notify 
machine's user about possible solutions. It also notifies admin about possible checting from the user's side.
SystemRat's server also collects statistics about all the machines.

For network connection between client and srver java will be used. Main system calls will be made by C++ dll.
All system info will be collected by C++, passed in JSON to java via network to server. All collected info on server will be analyzed by the other dll.
