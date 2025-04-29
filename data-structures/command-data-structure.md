# Command Data Structure

The platform is interacted with using command (typically from a console, but can also be generated internally by e.g. a scheduler).  
(This does not stop the creation of an Web API wrapper arounds these commands.)

Commands are top level requests and their initiation and completion is tracked seperately from the session request queue.  
Commands are hierarchical, allowing recursive subcommands as needed.  
Commands can have options that are themselves commands. These take precedence and use the command info as data input. A typical example is the help command.
(It gets rewritten to a help command.)  
*spl run -h* gets rewritten to *spl help run*

*command-line-args* module uses the following data structure for bot command and command options:
option-definition
OptionDefinition ⏏
.name : string
.type : function
.alias : string
.multiple : boolean
.lazyMultiple : boolean
.defaultOption : boolean
.defaultValue : *
.group : string | Array.<string>
However, command is always a string defaultoption   
I think command should be grouped with an option array in our case, as it always starts with a command.

```
{
    "name" : "run",
    "options" : [
        { name: 'squash', type: Boolean },
        { name: 'message', alias: 'm' }
    ]
}
```

A subcommand has the same structure but is stored as a subdirectory of the main command.
 