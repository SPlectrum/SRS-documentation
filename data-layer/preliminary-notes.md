# Preliminary Notes

## API

The idea is to have two data APIs: 
 - read / write for single item access on PK 
 - consume / publish for batch (streaming)

## Data Access
 
Data is stored in topic, ordered on offset and optionally with partitions based on key segments.
However, there is also a need to efficiently select from these data structures and that requires additional lookups and indexes.  
There is no intent to have these additional lookups and indexes added to the data repository.  
Instead the data layer will create lookups and indexes as and when required and stores this within the runtime as part of the current session.  

The reasoning is the following: the actual repository contains the immutable changelog data - static.  
The views that are created from it for efficient access can be seen as dynamic representations at runtime.  
Consider the biological analogue of DNA and RNA, where RNA is the activated and DNA the static.

The data in the repositories can only be accessed through the data layer.  
It is the data layer that keeps and manages the state data associated with this accesss.  
The platform initialises on the static data at startup and readies itself for runtime use.  
Because of that dynamic nature there is a data layer instance per session.

## Data Structure Management

Data structures come with a schema.  
AVRO is going to be used to manage the schema irrespective of the actual format of the data ( JSON, AVRO, ... )

## Data Layer Underpinning

The initial implementation of the data layer will sit on top of a filesystem. 
However the implementation should be compatible with Kafka topics. 
When the data layer is robust enough then multiple underlying repositories will be made available. 
Once the platform can run on native Kafka it will be able to achieve high bandwidth in and outbound.

## Header and Value Philosophy

A data repository stores data payload, i.e. typically the contents of the *value* property.  
The associated header section of the record contains metadata about the payload.

Splectrum uses the same format to express an action request.  
A data record becomes a request if the header section contains the action request specific data properties (the spl.action hive).

Splectrum uses the same format to express an execution context request.  
A data record / request record becomes an execution context record if the header section containes the execution context specific data properties (the spl.execution hive).

Storing an action request or execution context request in the data repository does not remove any of the header properties sections.
They all remain and they only become 'active' is SPlectrum places them in the that context.
( Good housekeeping practices on the platform may mean that cleansing is done prior to storage, but it is not compulsary )

Now, when a data record is turned into an action request or an execution context request then a context overlay is done on top of the initial record.  
Turning it into an action request will mean replacing the spl.action hive (there could be some merging rules) and the same for execution context.  
This should be kept in mind.

If this overlap is to be avoided (and potential metadata loss is not acceptable) then the record (header and values combined) should be stored as a *value* data payload.
The default mode is the 'compressed' header mode. The nested record mode could be required / useful but will not be considered at the moment. 
The compressed mode can be created from the nested mode by merging the header section appropriately (potential 'data).

Note that compressed mode can also exist for composite data records - i.e. a data record that contains a number of atomic records.  
In that case header data hives are added to express the compression - needs more working out.

## Data Operations

Data operations are expressed at the logical level using the filesystem based naming.  
*spl/data/read* and */spl/data/write* with repo, directory and file as specifications. (*spl/data/consume* and *spl/data/publish* to follow)  
These translate directly to the filesystem operations *spl/data/fs/read* and *spl/data/fs/write*.
Later a Kafka physical store adaptor will be implemented, and then repo, directory and file will be translated to cluster, topic, PK etc as required.

The requests of the data pipelines do not read or write directly.  
They hand this over to the execution context, and all data access is kept as part of request execution to detach request execution from specific repository state at request execution level. This is a very important feature of the platform.

Remember that the data layer implementation resides completely within the runtime.