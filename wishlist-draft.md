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
This means that all the functional blocks it uses to create processes etc. are compatible with this design.  
The execution flow is functional - not imperative

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

---

## Execution Plan and Happy Path

Actions on the platform - be it internal or external - always end up with an execution plan following which the request is put on the internal execution queue.  
Execution should be happy path first, with recalculated alternative execution flow upon error.  
Functional blocks should not be concerned with conditional execution paths - these should be made available as alternative paths for the 'compiler' to decide when to include in the execution plan.  
This keeps execution paths clean and allows functionality to be extended based on data conditions and without changing valid happy path and alternative execution paths.

---

## Internal Data Repositories

The aim is to have the make the (physical) data repository or -ies of the platform switchable, while the platform adheres to the same logical data repository pattern.  
Not all repositories need to be fully equivalent, only sufficiently equivalent.  
The platform runs on streaming data and this is reflected by its logical repository, but note that the underlying physical repository could be external and non-streaming (e.g. git or rdbm)
provided there is a compliant interface between logical and physical (e.g. rdbm is fully cdc enabled).

### Model Data Repository

The model data repository is Kafka, however there should be a choice of repositories for platform internal use that are sufficiently equivalent to Kafka (depending on the use case).

### Folder Based Repositories

---

## External Repositories

External Repositories are those that are natively not equivalent with the internal logical repository model - Kafka.  
However, it is not as clear cut as one might think: It is possible to design data streaming compatible data structures within (natively) non-streaming repositories.

---
