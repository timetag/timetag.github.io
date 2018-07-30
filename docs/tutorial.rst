============
Tutorial
============

if not using ready-made recipe


Add hardware
--------

Click the “Acquisition Device” button on the main screen to open a dialogue.
Specify a relatable name, the number of physically available channels and 
the number of marker channels (specific to PicoQuant HydraHarp400). 

Add analysis instrument via “Instrument Designer”
---------

Click the “Instrument Designer” button on the main screen to create a new analysis routine and open it in the Instrument Designer. 

This is the place where you define how exactly the ETAbackend analyses your data.

You create an analysis instrument in two steps:

* Create a state diagram (https://en.wikipedia.org/wiki/State_diagram) trough which the program transitions that covers all 
relevant states that your analysis undergoes (left-hand side of the instrument)

* Create some (simple) code to define what actions should be performed upon one (or several) specific transitions (right-hand 
side of the Instrument Designer)

To get a better understanding let’s first consider a simple example of a state diagram with two state before we move on to a 
working ETA example.

The states we want to consider for our abstract example are “music_playing” and “silence”. A transition between the states is 
triggered by either “play” or “stop”, depending on the current state.


TODO: IMAGE

We start in the “silent” state, indicated by the arrow with it’s tail attached 
nowhere.

If we want this to become musical chairs we need to define some actions. We 
would like participants to start running around the chairs when entering the 
“music_playing” state and sit down on the chairs when entering the “silent” 
state, then remove the participant who did not find a chair and remove one of 
the chairs.


