# Packages, Modules and APIs

Data and functionality is organised in packages.  
These packages can then be applied to a SPlectrum install.  
There is a standalone package API (separate from the data layer) to manage install, boot and system operations.  

Functional packages feature a hierarchy of modules that are grouped into APIs.  
The idea is that all methods of an API operate on the same data structure - single concern.  
APIs that operate on related data structures can then be part of the same hierarchy.

The platform internally works with data entities featuring the same overall Kafka record structure,
and the same applies for modules.  
A modules gets developed in an 'external' format but contains all information necessary to then import it
into an native internal format.  
(The same pattern applies to APIs - all methods work on the internal data format with import and export functionality.)

## Package and API Auxiliary Library

The auxiliary libraries are used to provide common functionality to the API methods  
and to centralise the inclusion of non-ASPlectrum modules.