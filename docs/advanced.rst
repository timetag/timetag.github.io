============
Advanced
============


Talking to ETA backend via Websocket
-------------------------------------

ETA backend implements a Remote Procedure Call mechanism using JSON format, with which you can upload an existing recipe, modifying variables, like filename, execute the analysis and even get the realtime streamming of the results.

Before invoking a remote method, your program (client) need to connect to the backend (server) using the Websocket protocal. (Examples in LabVIEW and Javascript are provided. [TODO:link to .vi] )

Sending a JSON string in a format like ``{"method": "<name of method>", "args": [<arg1>,<arg2>,...] }`` to the Websocket will invoke the corresponding method immediately. Once the method is invoked, the following requests will be queued until the current method finishes.

The method might send JSON strings as responses, and the format is ``["<type>","<content>"]``. Please note that the client might get multiple responses (even in different types) after invoking the method.

List of the methods provided in ETA Backend 
-------------------------------------

1. VI Checking

    JSON: ``{ 'method': "compile_eta", 'args': [eta_file_content] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe.
    
    Responses: The updated table. No visual response unless there is error in the recipe.

2. Run Analysis or Display Panel

    JSON: ``{ 'method': "process_eta", 'args': [eta_file_content, id, group] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe. For specifying the Display Panel that you want to run, the `id` and `group` should also be provided.
    
    Responses: The updated table. Other responses are sent in code of Display Panel in the recipe, using `eta.send()`.  

3. Browse file as the parameter.

    JSON: ``{ 'method': "recipe_set_filename", 'args': [eta_file_content, id, name] }``
    
    Arg: `eta_file_content` is a string of the content of the `.eta` recipe. For specifying the parameter that you want to modify, the `id` and `name` should also be provided.
    
    Responses: Responses are sent in code of Display Panel in the recipe, using `eta.send()`.  

Type of responses from ETA Backend 
-------------------------------------

1. Errors (`err`)

2. Generic Information (`log`)

3. Main Table (`table`)

4. Switch state to running (`running`)

5. Switch state to stopped  (`stopped`)

6. URL of dashboard (`dash`)

7. Switch state to discarded (`discard`)
