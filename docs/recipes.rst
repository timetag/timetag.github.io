============
Recipes
============
Here we want to provide some information about the pre-made recipes.

Start-Stop (w/ and w/o reset)
------------------------------
This recipe is determining the time differences between events on the zeroeth and first physical channel, starting the time on channel 0 and stopping on channel 1. These time differences are then logged into a histogram.

Delay
......
The channels are duplicated (0->2 and 1->3) with a configurable delay by the dealy line instruments. Afterwards the actual time differences are taken from the delayed copies.
To adjust the delay, change the second argument of the emit(channel_number, delay_in_ps) function. This feature lets you compensate for physical differences in fiber patchcord and coaxial cable length for the two optical and electrical signal paths in an HBT setup.

With or without reset
......................
To understand the difference, imagine the starting of the clock is triggered by an event on channel 0. If the next event happens to occur on channel 1 everything is clear: the clock is stopped and the time difference is recorded to the histogram. But if the second event occurs on channel 0 we have to decide how to handle this case. We can either ignore this event and all other events on channel 0 until an event shows up on channel 1 (start-stop without reset) or we can reset the clock on every event on channel 0, only measuring the shortest time differences (start-stop with reset). The latter case approximates a proper correlation at time delays close to 0 and is typically used as an analysis if full correlation is not available. Note, that for long time scales it is absolutely neccesarry to do a full correlation to get an accurate result, especially if interresting features are not at 0 time delay (e.g. if the antibunching dip is shifted away from 0 time delay).
You can switch between reset modes by adding/removing a transition from the "running" state to itself with the channel number of the start channel (default: channel 2).

Output
.......
You can choose to display an interactive histogram plot with plotly or bokeh or you can save a *.txt, *.png and *.eps file by running the corresponding code panel.

Correlation
------------

Two-time correlation
---------------------

Live acquisition
-----------------
