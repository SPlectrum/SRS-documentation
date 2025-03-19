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

## Data Structure Management

Data structures come with a schema.  
AVRO is going to be used to manage the schema irrespective of the actual format of the data ( JSON, AVRO, ... )

