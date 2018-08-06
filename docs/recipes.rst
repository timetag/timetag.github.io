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
To adjust the delay, change the second argument of the ``emit(channel_number, delay_in_ps)`` function. This feature lets you compensate for physical differences in fiber patchcord and coaxial cable length for the two optical and electrical signal paths in an HBT setup.

With or without reset
......................
To understand the difference, imagine the starting of the clock is triggered by an event on channel 0. If the next event happens to occur on channel 1 everything is clear: the clock is stopped and the time difference is recorded to the histogram. But if the second event occurs on channel 0 we have to decide how to handle this case. We can either ignore this event and all other events on channel 0 until an event shows up on channel 1 (start-stop without reset) or we can reset the clock on every event on channel 0, only measuring the shortest time differences (start-stop with reset). The latter case approximates a proper correlation at time delays close to 0 and is typically used as an analysis if full correlation is not available. Note, that for long time scales it is absolutely neccesarry to do a full correlation to get an accurate result, especially if interresting features are not at 0 time delay (e.g. if the antibunching dip is shifted away from 0 time delay).
You can switch between reset modes by adding/removing a transition from the ``running`` state to itself with the channel number of the start channel (default: channel 2).

Output
.......
You can choose to display an interactive histogram plot with plotly or bokeh or you can save a ``*.txt``, ``*.png`` and ``*.eps`` file by running the corresponding code panel.

Correlation
------------
This recipe is determining the time differences between all events on the zeroeth and first physical channel, starting the time on each event on channel 0 and stopping on each event on channel 1 for each start. These time differences are then logged into a histogram.

Delay
......
The channels are duplicated (0->2 and 1->3) with a configurable delay by the dealy line instruments. Afterwards the actual time differences are taken from the delayed copies.
To adjust the delay, change the second argument of the ``emit(channel_number, delay_in_ps)`` function. This feature lets you compensate for physical differences in fiber patchcord and coaxial cable length for the two optical and electrical signal paths in an HBT setup.

When to use
......................
It should be the first recipe you think about for auto- and cross correlation. It is also the right choice for investigating features far away from 0 time delay.

Output
.......
You can choose to display an interactive histogram plot with plotly or you can save a ``*.txt`` file by running the corresponding code panel. You can also have a look at the examples for fitting in the interactive plot and the examples for saving figures of a zoom-in and a full auto-correlation including analysis.

Two-time correlation
------------
This recipe records the time differences between a sync channel and two other channels into a 2D histogram. A ``COINCIDENCE(name, slots, emit_on_ch_x_when_filled)`` tool is used to record stops for events on the two channels. If ch 0 has two or more events before ch 1 has an event, the most recent timestamp is used, like in the case of start-stop with reset. Once both slots of the ``COINCIDENCE`` tool are filled, it emits a signal on a virtual channel to let the program know that it is time to record the time differences to the last sync before the earlier event into a 2D histogram.

Usage
......
Since the plot is 2D the amount of data ponts can increase dramatically with small changes in the binsize and bin number. The current plotting libraries used cannot handle these amounts of data easily and it is therefore advised to keep the bin number < 500 and increase the histogram range by binning with the binsize. The data can be moved around in the 2D histogram by adjusting the third element in each of the two dimensions of the ``HISTOGRAM(name,[(number_of_bins,bin_size,"time-your_delay"),(number_of_bins,bin_size,"time-your_delay")])``.


Output
.......
You can choose to display an interactive histogram plot with plotly or bokeh or you can save a ``*.eps`` and a ``*.png`` file by running the corresponding code panel. 

Live acquisition
-----------------
This recipe allows you to view an on-the-fly analysis of your data while the correlator is still recording it. This has been tested with HydraHarp and quTAG correlators.
You can either accumulate a histogram or only show the latest update e.g. for alignment. The analysis performed in this example is a full correlation.

Usage
......
As explained in the start-stop and correlation recipes, you can adjust a delay if the feature you are interested in is at the edge or outside the histogram area. You can do this by opening the “Instrument Designer” for the delay lines ``DL0-2`` and ``DL1-3``. The delay lines copy the events on channel 0 (or 1 in case of ``DL1-3``) to a new channel (first argument of the emit function) with a delay specified in the second argument of the ``emit`` function: ``emit(new channel number, delay in ps)``.
You can adjust the width of the histogram. This is done with a combination of the ``bins`` and ``binsize`` variables in the start screen. 
You can use the ``y_max`` variable on the home screen to adjust the desired plot height.
An important adjustment is the speed at which the file is processed. This is done by selecting how many events one chunk should have before the program bothers to analyse this chunk. Use the variable ``records_per_cut`` on the home screen to adjust this.

You might want to switch from accumulation mode to “show me only the most recent chunk” mode aka alignment mode. For this we would like to implement a button, but so far you have to go to the “Code” panel again and change line 44.
It should say ``ctxs= ctx1,sum_results=True,iterate_ctxs=True,verbose=False)`` for accumulation mode and ``ctxs= None,sum_results=True,iterate_ctxs=True,verbose=False)`` for alignment mode.
