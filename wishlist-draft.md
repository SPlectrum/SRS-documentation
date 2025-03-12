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
