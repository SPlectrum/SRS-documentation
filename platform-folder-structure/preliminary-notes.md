# Preliminary Notes

In this section the folder structure of the platform will be covered.

Currently the thinking is to have the following top level structure:
 - data : the static data repositories (immutable state change data)
 - metadata : descriptive data of what resides within data
 - packages : functionality installed / available to the platform
 - runtime : process and dynamic data (associated with execution)
 - backups : housekeeping cleans up at regular intervals, and state is kept where required
 - tools : visualisation and managing tools to interact with the platform

 Although this is dealing with a folder structure, bear in mind that the setup must be compatible with Kafka.  
 It should be possible to switch the storage over to a Kafka cluster just by swithing for Kafka version of the data layer.  


