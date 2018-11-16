============
Advanced Usage
============


Talking to ETA backend via WebSocket
-------------------------------------

ETA backend implements a Remote Procedure Call mechanism using JSON format, with which you can upload an existing recipe, modifying parameters like ``filename``, run the analysis, and even get the real-time streaming of the result.

Before invoking a remote method, connect your program (client) to ETA backend (server) using the Websocket protocal. 

(Examples in LabVIEW and Javascript are provided. [TODO:link to .vi] )

Sending a JSON string in a format of ``{"method": "<name of method>", "args": [<arg1>,<arg2>,...] }`` to the Websocket will invoke the corresponding method immediately. When invoked method is running, new requests will be queued until the current method finishes.

The method might send JSON strings as responses in a format of ``["<type>","<content>"]``. Please note that the client might get multiple responses (even in different types) after invoking one method.

Remote methods provided in ETA Backend 
-------------------------------------

It is not recommended to remotely call the functions provided by the ``eta`` object in the Embedded Python environment directly, because they are not designed for remote calling and the returned value will not be streamed back to caller's side.

There are three special functions that is provided for remote controlling ETA Backend. All these methods bundle a set of internal functions that first updates the recipe on ETA Backend to the uploaded one, perform the requested actions, and then send the updated main table as responses. Usually there is no extra response unless there is error in the recipe or there are user-defined ``eta.send()`` in the embedded Python code.


1. VI Checking

    JSON: ``{ 'method': "compile_eta", 'args': [eta_file_content] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe.
 
2. Browse file and set it as the parameter.

    JSON: ``{ 'method': "recipe_set_filename", 'args': [eta_file_content, id, name] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe. For specifying the parameter that you want to modify, the `id` and `name` should also be provided.
  
3. Run a Display Panel

    JSON: ``{ 'method': "process_eta", 'args': [eta_file_content, id, group] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe. For specifying the Display Panel that you want to run, the `id` and `group` should also be provided.
    
    Extra Responses: Other responses are sent in code of Display Panel in the recipe, using `eta.send()`.  


Type of responses from ETA Backend 
-------------------------------------

1. Errors 

    Type: ``err``
    
    JSON: ``["err","<text>"]``
    
    Args: ``<text>`` is a string of the error message.

2. Generic Information

    Type: ``log``
    
    JSON: ``["log","<text>"]``
    
    Args: ``<text>`` is a string of the message.

3. Update Main Table 

    Type: ``table``
    
    JSON: ``["table","<json>"]``
    
    Args: ``<json>`` is a JSON string of the main table.

4. Switch state to running 

    Type: ``running``
    
    JSON: ``["running"]``
    
   
5. Switch state to stopped  

    Type: ``stopped``
    
    JSON: ``["stopped"]``

6. Switch state to discarded

    Type: ``discard``
    
    JSON: ``["discard"]``


7. URL of dashboard 

    Type: ``dash``
    
    JSON: ``["dash",<url>]``
    
    Args: ``<url>`` is a string of URL to the dashboard.

8. User-defined message (eg. streamming histogram or GUI updates)

    Type: defined using eta.send(message,type)
    
    JSON: ``["<type>","<message>"]``
    
    Args: ``<message>`` is a string of a user-defined message.


