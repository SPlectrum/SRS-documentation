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
The property names used in this example are experimental.

---
```
// execution flow : commandline --> create request --> execute request

// request gets wrapped in a streaming data record
{
  "headers": [
    "action": "request/commandline"
    "status": "pending"
  ],
  "value": "spl hello-world my friends"
}

// request gets parsed into an internal action
{
  "headers": [
    "action": "internal/hello-world"
    "status": "pending"
  ],
  "value": { "person": "my friends" }
}

// 
{
  "headers": [
    "action": "internal/hello-world"
    "status": "complete"
  ],
  "value": "Hello my friends !"
}

// Once the request is entered into the execution pipeline it gets wrapped in an execution context wrapper
{
  "headers": [
    "action": "execute/prepare"
  ]
  "value": {
    "headers": [
      "action": "request/commandline"
      "status": "pending"
    ],
    "value": "spl hello-world my friends"
  }
}

// the execution context prepares the request by setting an execution plan
{
  "headers": [
    "action": "request/parse"
  ]
  "value": {
    "headers": [
      "action": "request/parse"
      "status": "pending"
    ],
    "value": "spl hello-world my friends"
  }
}

// this returns a subsequent action upon return - real world processing will be more elaborate
{
  "headers": [
    "action": "execute/complete"
  ]
  "value": {
    "headers": [
      "action": "internal/hello-world"
      "status": "pending"
    ],
    "value": { "person": "my friends" }
  }
}

// this returns a subsequent action upon return - real world processing will be more elaborate
{
  "headers": [
    "action": "internal/hello-world"
  ]
  "value": {
    "headers": [
      "action": "internal/hello-world"
      "status": "pending"
    ],
    "value": { "person": "my friends" }
  }
}

// action completed - request completed
{
  "headers": [
    "action": "execute/complete"
  ]
  "value": {
    "headers": [
      "action": "internal/hello-world"
      "status": "complete"
    ],
    "value": "Hello my friends !"
  }
}
```
