# Execution Pipeline

Platform commands typcially consist of a sequence of actions that are executed sequentially.  
An action is provided with input data and after execution it returns the modified data as output.  
This data is then provided as input to the next action step.

An action sequence is executed in a single (synchronous) sequence from initialise to complete,  
and a data set detailing it execution is kept.

However, the overall execution of a command can consists of several sequences,  
and the separate sequences can be executed asynchronously.  
This will be worked out in further detail.

The data layer is incorporated in the execution pipeline.  
