# Platform Clients

Within SPlectrum opening a terminal is liking running an application, a client.  
Every client is mounted as a running application and keeps its associated state within the *clients* directory of the platform data repository.

A platform instance runs local clients to connect to specfic runtime runtime sessions (boot, system, ...).  
But the aim is to facilitate remote clients once an appropriate communication mechanism is specified and implemented for two platform instances to communicate.

SPlectrum does not follow the traditional client-server paradigm.  
It is better seen as a collection of nodes that accept external clients but can also create clients onto each other.  
The aim is to create an interaction mode that is comfortably within the philosophy of peer to peer. 

