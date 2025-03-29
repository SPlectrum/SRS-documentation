# Preliminary Notes Avro Schema

All schemas are expressed in Avro. 
The Avro schema may cover the value property of a record, or a property within the header of the record.

## Schema and Registry

AVRO schemas will be used for
 - data payloads
 - action headers
 - execution headers
 - data record headers

 The platform contains 2 repositories: 
  - the platform repository : for data of the installed platfrom applications
  - the runtime repository : for data associated with the runtime processes (boot, system, ...)  
There is a registry for each, because both repositories are independent of each other.  
The registry resides in the metadata folder. Note that this folder will contains more data than just schemas.  

Schemas will be organised in line with the packages and modules that the data is associated with.  
The naming of schemas consists of two parts: *subject* and *name*.  
Both will require namespacing to provide fine-grained extensible naming mechanism.  
The schema registry has versioning (with evolution and compatibility types).  

The naming structure of the schemas is still under review - versioning (and associated evolution) should be meaningful.  
At the same time there should only be one owner of a data structure (within an logical application context).  
It would make sense to use reasonbly 'standardised' schemas where possible to reduce the overall management cost,
but with schema that allow for extensions to suit specific applications.

## Schemas and Actions

Actions have input and output data payloads.  
The data payload is double wrapped: *execution* --> *action* --> *data*  

The headers of these wrappers need to conform to schemas as well.  
More importantly, the schema of these different warppers should be orthogonal - there should be no overlap.  
And the header data structures should be extensible so that new structure can easily be introduced.  
Also, these header data structure may exists as data payloads.

Input and Output data schema are important when creating pipelines.  
It should be possible to 'automatically' zoom in and out of data structure to create action pipelines (main and sub) that work on this data.
Again, this are needs further research.
