# Hello World Example

This example is implemented in its own repository: [hello-world](https://github.com/SPlectrum/hello-world).

Setting up and testing an initial request execution flow.  
Thinking of doing it using bash and inotify.  

![image](https://github.com/user-attachments/assets/6192afa9-f9dc-494a-bc9c-9e0bd22ccb82)

The aim of the Hello World example is to set up a simple folder based stream example that processes a request.  
It should process using a consumer publisher pattern and acts as validation of an initial approach which then can be reused.

It will also involve having initial thoughts about runtime etc.
and an analysis of the record structure used for the implementation.

---

Execution request requires two execution contexts.  
There is the context for the specific requests to be executed.  
But there is also a context for the execution manager.

---

[
{
"timestamp":1741686415977,
"timestampType":"CREATE_TIME",
"partition":0,
"offset":188,
"key":"session-key",
"value": {
"key":"RA0U+Yhvb8pZ02uJKLWJ3Uv/XZ5qqlZo9vv7fW5nzkA=",
"algorithm":"HmacSHA256",
"creation-timestamp":1741686415969 },
"headers":[]
}
]

