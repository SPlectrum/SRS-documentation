# Preliminary Notes

In this section the directory structure of the platform will be covered.

Currently the thinking is to have the following top level structure:
 - data : the static data repositories (immutable state change data)
 - metadata : descriptive data of what resides within data
 - modules : functionality installed / available to the platform
 - runtime : process and dynamic data (associated with execution)
 - backups : platform level backups
 - tools : visualisation and managing tools to interact with the platform

The runtime directory is concerned with the processes running on the platform, the settings and the dynamic data (associated with execution).
For this purpose it has a self-contained structure.
    - backup : housekeeping cleans up at regular intervals, and state is kept where required
    - boot : startup process (boot and shutdown)
    - data : the runtime data repository - data readable by all sessions
    - metadata : the metadata associated with the runtime data repositories
    - system : management process once system as booted
    - sessions : user sessions running on the system (one per input terminal?)

 Each process (boot, system or session) within runtime contains the following structure:
  - data : operational data associated with the process
  - requests : the request submitted and executed
  - scheduler : a list of scheduled tasks triggered by a timer
  - triggers : a list of tasks triggered by a data footprint
Each process (boot, system or session) is (currently) single thread. Although the scheduler and trigger processes may be separated out in future.

 Although this is dealing with a directory structure, bear in mind that the setup must be compatible with Kafka.  
 It should be possible to switch the storage over to a Kafka cluster just by swithing for Kafka version of the data layer.  

The data directory within a process is used to contain operational data (processed from request) to facilitate the scheduler and trigger processes or when required as part of archiving. They may also be refreshed periodically for operational reasons.
  
The system process lifetime is determined by the boot process. 
The boot process stays in existence until shutdown.

Note that the boot session (and potentially the system session) may have its own modules directoryto ensure it runs on 'safe' code that keeps running when the main part of the install encounter major issues.