# To the Next POC - spl1

The POC has implemented some patterns well, and needs refinement on others.  
The POC is going to be rounded off once a SPlectrun app has the ability to create and execute custom commands
from command batches and JS scripts both optionally with input parameters.

## Next Step

The current spl project is going to be renamed as spl0.  
It won't be archived yet as it needs to be kept maintained - initial apps are going to be built with it.

A new spl project will be created with the current POC codebase.  
First there will be some refactor to create cleaner implementation patterns.  
This is done primarily with AI in mind. We want to come to a clear set of rules and patterns that give a clear context for AI to operate in.  

The spl package will become the internal API package - functionality that implements the internal working of SPlectrum
but that are not for direct use when building apps.  
This covers the error, execute and data APIs.  

The data layer is going to be restructured with on top level API.  
All communication with that API will be in Kafka record format, 
but where the full record is read or written it is the 'Kafka' data interface 
and where only the value part is read or written it is the data import / export interface.  
Note that import / export could itself deal with record type.  
A better metadata structure needs to be designed for the kafka data interface specifying the value section  
so the import / export interface can properly deal with it.

SPlectrun needs a context sensitive data - metadata definition / specification that is clear and sensible.

APIs need to be single purpose and no split purpose across multipl APIs.  
Examples: the initial app API functions as internal implementation of command parsing and execution and core app API - not good.  
The data layer API is spread across 2 APIs, data and blobl.  

Action input argument logic needs to be finished off in terms of order of precedence, defaults etc.  
Clear rules should be specified and presented to AI for implementation.

The execution environment implementation pattern should also include in-memory consistent design.

