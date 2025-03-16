# Wishlist Draft

This is where initial ideas are dumped before being lifted out for detailing or removed ...

---

## Native Data Format - Kafka Message

The native data format of the platform is a Kafka message.   
The data payload shema is defined with an AVRO schema (irrespective of data format).  
The message headers should be used intensively for metadate and should backed with an AVRO schema.

---

## Platform Application Design

The platform runs streaming (compatible) applications. 
This means that all the functional blocks - modules - it uses to create processes etc. are compatible with this design.  
The execution flow is functional - not imperative.  
Modules should be single concern.  
There is a single internal input output pattern, an array of messages in and out.  
The execution flow created with such modules should be determined by specific patterns in the visible data and not by module internal state or logic. 
By making use of the visible data state in this way, there is no need for any additional logging. 
The aim is here to create a platform that has all its data requirements fully encapsulated - all can be found in the data stream generated.  

### Task Engine & InfoMetis - Similarities & Differences

The platform will incorporate a lot of features that were present in the Task Engine, but it will also differ from it substantially.  
The aim is to have a pure streaming application (consumers & producers) and the Task Engine was not.
I want it to be simpler and better prepared for auto-codegeneration.

From InfoMetis it inherits the git and container integration, the full lifecycle approach.  
InfoMetis used container embedded git repositories, with the container registry being the authoritative source.  
The main reason was that the container wrapper provided a functional wrapper that allowed more diverse artifacts to be stored within (including the git repository).  
Because a containerised InfoMetis runtimer that managed all these container (and this git repository) there was no need to set up complex git with submodules.

---

## Header Metadata Candidates

It will be important to have a workable namespacing algorithm that results in a flexible and extensible structure of namespaces for header data structures.  
There are a lot of candidates for inclusion into the header. It is the responsibility of the platform to manage that.  
There should be switches included so that the metadata stack can be tailored to the execution environmnent.  
The aim is that the header metadata makes the data stream complete - i.e. without the need to created additional data stream for logging, monitoring etc.  

### A Set of Unique Identifiers

The set consists of a global unique identifier and a set of context unique identifiers.  
There could be a variety of contexts, e.g. data structure related, source related, ...  

### A Set of Foreign Key Identifiers

This set of identifiers allows the inclusion of the data context of the payload, this refers as much to structure as to instance.  
This way data from the wider structure can be requested / included as and when needed rather than requiring the inclusion of a broad set of data just in case.  

### Tracing Metadata

Needs a bit of thinking - is there always only one set of tracing data or will this end up being context specific ?

### Metrics

Would it be a good idea to include metrics in there as well

### Data Lineage

The tracking of movement and transformation of the data payload, including more importantly which processors and what versions.

---

## Exceptions Information

There must definitely be a section for error data.  
A functional block throws the error and it is the responsibility of the platform to include it in the header.

---

## Execution Queue

The Platform has its own runtime and execution queue and client submit request to the platform.  
This ensures that the platform has ownership of the execution and is able to manage the flow.  

---

## Full Lifecycle Platform

The core functionality of the platform is to configure, deploy run and monitory streaming functionality.  
The main aim is to apply this to distributed systems, however it will use the same paradigm for transient processes and its own internal implementation.  
One way to look at it is that it's behaviour is similar to SQL server: SQL that is written is compiled, get executed and comes with a bunch of metrics and other metadata.  

It should be compatible with decentralised  
This means being self-sufficient for the functionality that is locally required and at the same time being able to interact flexibly with other platform instances.  
Every instance has its own local lifecycle in terms of install, boot, runtime etc.  
Thinking about a client-server pattern, this would mean that both client and server are instances each with their own functionality - one client, the other server.

---

## Execution Plan and Happy Path

Actions on the platform - be it internal or external - always end up with an execution plan following which the request is put on the internal execution queue.  
Execution should be happy path first, with recalculated alternative execution flow upon error.  
Functional blocks should not be concerned with conditional execution paths - these should be made available as alternative paths for the 'compiler' to decide when to include in the execution plan.  
This keeps execution paths clean and allows functionality to be extended based on data conditions and without changing valid happy path and alternative execution paths.

---

## Fine-grained Modular Design

Code implementation should be done through small and wherever possible single-concern code units.  
This should allow for easy implementation and maintenance.  
The execution flow should be governed by specific data patterns in the visible data state and not by code module internal logic or state.  
This approach is well-suited for runtime execution plan creation.  
However, the platform should have - where appropriate - the capability to compile fine-grained modules into a single executable.  
Even more, it may as part of this change the module code to implement optimisations while maintaining the same execution outcome.

---

## Testing Strategy

The aim is to use integration testing from the ground up and only use integration testing.  
Within this paradigm testing a module is integration testing of an atomic system consisting of one module.  
Test are not written in code, but constructed in data. There is only one type of test rig as the platform implement a single internal input output pattern.

Generation of complete data streams during execution enables the generation of bug reports and test data sets and will allow for blunt bulk compare tools to check for test success.  
Bearing in mind that the finegrained execution setup should allow for 'compilation' into single executable actions there is further room for a unique test approach.

---

## Internal Data Repositories

The aim is to have the make the (physical) data repository or -ies of the platform switchable, while the platform adheres to the same logical data repository pattern.  
Not all repositories need to be fully equivalent, only sufficiently equivalent.  
The platform runs on streaming data and this is reflected by its logical repository, but note that the underlying physical repository could be external and non-streaming (e.g. git or rdbm)
provided there is a compliant interface between logical and physical (e.g. rdbm is fully cdc enabled).

### Model Data Repository - Logical Repository

The model data repository - i.e. the logical data repository - is Kafka.  
However there should be a choice of physical repositories for platform internal use that are sufficiently compliant with the logical repository.  
What this means is that physical repositories do not need all configurations available at the logical repository level.

### Folder Based Repositories

The filesystem offers a versatile tool to create a variety of file folder structure patterns that are compliant with the logical repository.  
Various patterns will be worked out in detail and used heavily during the initial implementation stages.

### Third Party Repositories

I am thinking here of satisfactory repository patterns that could be implemented on top of Redis, git, SQL rdb, ...  
This will be looked into as the need arises.

---

## External Repositories

External Repositories are those that are natively not equivalent with the internal logical repository model - Kafka.  
However, it is not as clear cut as one might think: It is possible to design data streaming compatible data structures within (natively) non-streaming repositories.  
External repositories are repositories the platform synchronises from or to but that do not function as an internal repository.

---

## Code Repositories Strategy

Git will be used for the code and code instance repositories.  
Code instance repositories are used to gather runtime data of executing code modules.  
The final strategy needs to be one suitable for a highly automated, AI powered environment. That will probably be a repository per versioned code module.  
However, the initial core code will be in an all-in-one repository as I see this as bootstrapping code.

Code is only one part of the assets that need to be stored.  
I would like to see a complete storage solution - code base, user stories, issues, test catalog - anything that is part of it.  
I think one should move to full encapsulation, which would make it ready for a true decentralised approach.  
Of course it should be possible to dock repositories into larger structures - i.e. a team working on many modules plus an overall solution but each component would store the information relevant to its context.  
At the moment I can only think of a container image to make this happen.

---

## Minimum Runtime State Design

Being a data streaming application with full coverage of meaningful state changes for the current execution context of the platform the local state approach needs to be done differently.  
Only forward visibility is required with appropriate back state references. This is quite different.
