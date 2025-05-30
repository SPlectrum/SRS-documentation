[Home](./README.md)
# SPlectrum Key Points

One of the main aims is to design SPlectrum to enable AI driven development.  
For this reason we want to work with a number of simple, strict design patterns that can be cast into simple rules. 

TDD - fully encapsulated execution state - streaming application platform - DSL apps - peer-to-peer - AI development

---

## Native Data Format - Kafka Message

For SPlectrum, the main properties are _headers_ and _value_.  
The _value_ property contains the data payload, _headers_ contain metadata.  
In this context metadata is something that is context specific.

```
{
  "key": "optional-key",
  "value": {
    "field1": "value1",
    "field2": "value2"
  },
  "headers": 
  {
    {"key": "headerKey1", "value": "headerValue1"},
    {"key": "headerKey2", "value": "headerValue2"}
  }
}
```
Note that the _headers_ format is compatible, but not exactly the same as Kafka message headers:  
The are a list of keyvalues, and multiple entries with the same key are allowed.  
Splectrum uses unique keys, typically with JSON object values.  
Key namespacing makes it always possible to merge existing header keyvalues with SPlectrum header data in an orthogonal way.

Where SPlectrum header data structures would become large with respect to data payload size, foreign key referencing can be used.

---

## Single Concern

Every facet of SPlectrum should deal with one and one concern only.

---

## Packages, APIs and Actions

This applies to core functionality, and gets extended to apps - an App in this sense is seen as a package of SPlectrum implemented functionality.  
Rules on naming, scope and single concern of actions.
A strict set of rules for input arguments associated with actions, APIs etc.

---

## Execution Workspace

Where API associated data is stored.  
APIs consists of a set of actions operating on the same (in the broad sense) data structure - single concern.  
The API key contains two parts, the action key three parts. These URIs are used to reference input arguments associated with them in the header section.  
This needs extending to workspace entries, both header section and data payload section.

A workspace entries can be several instances, although one is only live at any one time.  
A new instance results in the old one being archived, and a new active entry being created.


---

## Executable Code

At the base level the functional code of SPlectrum consists of single concern functions.  
These run on a single request execution record which has record of all the single concern execution steps.  
The specific request should be step by step reproducible with the post-execution request execution record 
and the functional code that was invoked - without having access to the data repositories.  
I.e. the request execution record contains all the interactions with the data repositories.  
All data that may be required - metrics, logging, etc should be included in this.

Single concern actions are composed into pipelines which can branch.  
Initial implementation of such pipelines will be through commandline commands - creating DSLs.  
These can then be transformed into single commands through the creation of appropriate pipeline actions.  
These implementations could then be further optimized through pseudo compilation, 
backed up with test suites at each step for comparative validation of the functionality.  
Included in these may be other language implementations.

---

## Test Driven Development

The aims of any sort of development should be expressed and input / output data pairs.  
Requirements should be such that these pairs can be easily created, preferrable a narrative that is strict enough for AI to autogenerate these.

---

## Data Layer - Equivalent Data Repository Structures

There is no direct access to data repositories, all access is through the data layer and triggered by the absence of the data in the workspace.  
The initial data repository format is a file system.  
However, the semantics of storing data in folders and files should be compatible with kafka topics, partitions and primary keys.  
The data structure implementation should be direct compatible with an in memory store.  

---

## DSL Engine

Building complexity through DSL apps ( create a specific narrative )

---

## Peer to Peer

Aim to port it to pear platform running on bare.

---

## Streaming Applications

SPlecctrum apps should be ready to be deployed as streaming application on Kafka and similar.

---

## Tool Integration

No embedded tool integration is allowed (except for functional modules that are part of the inner core - commandline-args, commandline-usage, ... ).  
Tools should be API wrapped, come with online help and test suites for validation.

Rules should be created for AI to work with these APIs and implement functionality from them.  
Missing functionality would then become candidates for additional APis, or is covered by adhoc implementation within an app.

---

## Versioning

Versions are associated to fully specified and validated units.  
This means requirement test cases, validation test suites (all levels) and functional code.  
There should be an authoritative set - any further equivalence is assessed by checking that a set validates the authoritative test suites.  
In fact, a version is associated with a test suite set - ignores how it is implemented and what the specifics of bugs are.  
This means that 2 so-called identical versions may have different bugs, but positively validate the same requirements.  
From this it is evident how important the requirements are !

---

## Mirror on Life

Mirror on life, more specifically the living cell:  
Self-contained as far as functional information / history is concerned.  
Agile in its expression of actual functionality.  
Additive with growing functionality, high degree of decentralisation.  
Cooperative between instances (cells). SPlectrum should create a cooperative processing node structure.

---

## Full Lifecycle Approach

In computer parlance: bootloader - boot - system and sessions.  
A flexible structure of apps build around a core engine - that makes up a node.

The system becomes dynamic by launching request executions within an app context.  
This can be done commandline, or through a request queue, HTTP or TCP clients

---

## AVRO

Data schemas - input / output / repository - should strictly use to AVRO.

---
