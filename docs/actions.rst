Tools and Actions
===============================

Virtual Instruments are used to perform timetag analysis in an ETA recipe. As mentioned before, each Virtual Instrument consists of a graph on the left-hand side and a code panel on the right-hand side. Actions can be put in the code panel to specify what should be done when there is a state change on the graph. 

Each Action belongs to a certain Tool. Tools can be created with a user-specified name and some other parameters. The name is used to refer to the Tool later when performing Actions. The parameters that have default values can be omitted. 

For example, if you want to record a time interval of two events, you will need to create a Tool called CLOCK first, and then do Action clock.start() to start this clock.

In the following documentation, we list the built-in Tools and their Actions in the current version of ETA. 

.. note::
    Please note that ETA is still under development. Tools and their Actions might be changed in the future.

CLOCK
------------------------------
``CLOCK(name, max start times, max stop times)``

CLOCK is a time interval recorder with a start button and a stop button. The CLOCK remembers the time when it is started or stopped, and it calculates the time interval as the output.

There is a special case of CLOCK called "multi-CLOCK", where the buttons can be pressed for multiple times. The "multi-CLOCK" has a maximum limit of recorded events, and it will drop the oldest event when it reaches this limit. 

.. note::        
        Please also note that ETA will not automatically clear the recordings in the CLOCK, they can only be overwritten.

Parameters
......

- ``Max Start Times`` (default:1)
    This parameter specifies the maximum limit of start events that can be stored in the recorder. 
    
- ``Max Stop Times`` (default:1)
    This parameter specifies the maximum limit of stop events that can be stored in the recorder.

.. note::

    Max Stop Times should be set to 1, and Max Start Times should be set to a very large number when doing correlational measurements, so that you correlate each signal on one channel to every signal on the other channel.

Actions
......

- ``clock.start()``
    Start the clock at the current time.
    
- ``clock.start(LAST_SYNC)``
    Start the clock at the last sync. The time for the last sync is calculated from SYNCRate.
    
- ``clock.stop()``
    Stop the clock at the current time.
    
- ``clock.infer_start_from_stop(SYNC)``
    Using the stopping time to find the last specified type of signal before it, and then overwrite the starting time to the time of this signal.
    If the clock is a single-start-multi-stop clock, then the earliest stopping time value it stores will be used for inferring the start.

    .. note::
        Currently it can only use the SYNC period to find the SYNC signal. This action might be removed if there is better idea for supporting Life-time measurements using HydraHarp400 T3 files.

- ``[clock1,clock2,...].sort(start)``
    Sort the starting time of a group of clocks, preserving their stopping time.
    This is useful if you want to record multi-dimensional histograms with the axis indicating the arrival order (first photon, second photon) instead of channels (detector1, detector2).
    
   .. note::
    Please note that multi-CLOCK is not yet supported.

    The first parameters can be also changed to ``stop``, to sort the stopping time of a group of clocks, preserving their starting time.

Examples
......

Performing a start-stop measurement:

.. code-block:: python    
   
    CLOCK(c1,1,1)
    state2:
        c1.start()
    state1:
        c1.stop()  
        t1.record(c1)

HISTOGRAM
------------------------------
``HISTOGRAM(name,[(number of bins, width of bins in picoseconds)),...]``

Histogram generates statistics of time intervals or an arbitrary INTEGER. Histograms can be 1-dimensional or multi-dimensional. 
The histogram can be retrieved using its name from the returned dictionary from eta.run() in the Script Panel for further processing and plotting.

Parameters
......

- ``[(Number of bins, Width of bins in picoseconds),...]`` (required)
    This parameter has to be specified in a pair, like ``(100,16)``. The first value of the pair indicates the number of bins in the histogram. The second value of the pair indicates the size of each bin in the histogram. 
    
    If the histogram is multi-dimensional, specify value for each dimension, like ``[(100,16),(200,16)]``.

 .. note::
    The product of the histogram parameters bin size and bin number gives you the maximum correlation length, if you are performing a correlational analysis. 
    
    The values that fall out of the histogram will be ignored.

- ``Extra Dimensions``
    The extra dimension before the histogram dimensions, usually used for making images.

Actions
......

- ``histogram.record(clock)``
    Record the time interval of ``clock`` into a 1-dimensional histogram. 

- ``histogram.record(clock1,clock2,...)``
    Record the time interval of ``clock1`` and ``clock2`` into a multi-dimensional histogram. The order of clock should be the same as the order of dimension. 

    This is usually used to explore the joint probability distribution of two types of events.

- ``histogram.record_all(clock)``
    ``histogram.record_all`` is the Cartesian product version of ``histogram.record``. It records all the time intervals of the multi-CLOCK ``clock`` into the histogram. 
    
    This is usually used together with a multi-start-single-stop CLOCK to correlate signals from one channel to the another channel.

    Using record_all with a multi-dimensional histogram is not yet supported.

- ``histogram[x][y][...].record(clock,...)``
    Record the time interval of ``clock`` into an image of 1-dimensional or multi-dimensional histograms. This is usually used in biology imaging, where x and y can be obtained from state transitions of markers, which indicates the position of the scanning instrument.

- ``histogram[x][y][...].record_all(clock,...)``
    Combination of ``histogram[x][y].record(clock,...)`` and ``histogram.record_all(clock)``

Examples
......

Performing a correlation:

.. code-block:: python    
    HISTOGRAM(t1,(100,16))
    CLOCK(c1,100,1)
    started:
        c1.start()
    stopped:
        c1.stop()  
        t1.record_all(c1)

COINCIDENCE
------------------------------
``COINCIDENCE(name, length_of_array, emission_channel)``

Coincidence is a Tool that emits a signal when all of its slots are fulfilled.

Parameters
......

- ``Coincidence Slots`` (required)
    The number of coincidence slots on this Coincidence counter.

- ``Emit to this channel# when fulfilled`` (required)
    Emit to this channel# when all of the coincidence slots are fulfilled.


Actions
......


- ``coincidence.fill(slotid)``
    Mark the coincidence slot `slotid` with the current time. Then, a signal will then be emitted at the current time, if all of the slots are fulfilled. 

- ``coincidence.clear()``
    Clear all coincidence slots.


SELF
------------------------------
``emit(channel, delay_in_ps=0, period=0, repeat_#_of_times=1)``

The instrument itself is also a Tool. When using its actions, the instrument doesn't need to be referred by its name.

Actions
......


- ``emit(chn, waittime=0, period=0, repeat=1)``
    Emit a signal to ``chn`` after ``waittime``, both are either integer values or the name of an INTEGER Tool. It can also emit some repeated signals with a `period` in ps if  ``repeat`` is set to larger than one. If reapeat is set to 0, no event will be emitted, which might be used as a conditional emittion.
    
    The maximum limit of channel number ``chn`` is 255.
 
    .. note::
        It is not allowed to emit to any channel that is read from a timetag file (timetagger channels or markers). The emitted signal will never be written to the timetag file to prevent corrputing the original data.

        If you need to merge signals from two channels into one channel, simply emit them into a new unused channel.

        Channels can also be used as routers. For examples, you can route events to different Virtual Instruments based on some status that is controlled by the markers.
        
- ``cancel_emit(chn)``
    Flush all the previously emitted events in the channel ``chn``.

    Cancelling emitting a real channel from a timetag file will terminate the analysis before the ETA reaches the ending of the current section of the file.

Examples
......

Making a delay line:

.. code-block:: python    
   
    ch0_event:
        emit(2, 130)# here, ch0 is duplicated to ch2 with a delay of 130ps, making a delay line


Extending Actions using Embedded Code
-----------------------

Apart from the built-in actions, you can also use a embedded code to extend the functionality of ETA.

Embedded code can be wrapped in a  ``{`` and ``}`` . If the code contains curly brackets, a pair of ``{{{`` and ``}}}`` can be used.

The embedded code uses a restricted sub-set of Python language, and a limited subset of Numpy function is imported with `np`. Internally, ETA uses Numba to compile the Python code into LLVM and link it with the built-in actions and other parts of the program. 

.. note::
    Please note that features that requires ``import``, ``exec`` or file I/O are not available. Calling built-in actions in embedded code is not currently supported.
    
    We noticed that the built-in tools and actions already serve as a good basis for many different experiments. But we still want to add more actions for different analysis purposes. If you created some custom Action for extending the functionality of ETA, please share it :)


Examples
....

Here is an example for sampling randomly delays from a exponential decay and then emitting a signal with this delay whenever a transition from state a to state b happens via channel 1. This type of embedded code might, for example, be useful for a Monte-Carlo simulation.

.. code::
        INTEGER(random_delay) # define an INTEGER for use by both actions and embedded Python
        a--1-->b:
            start(c1) # execute built-in action
            #execute the embedded Python code
            {
                delay_from_sync = 200
                binsize = 16
                random_delay_arr = ((np.random.exponential(125, 1)+delay_from_sync)/binsize) 
                random_delay = round(random_delay_arr[0])*binsize
            }
            emit(3,random_delay) # emit on the channel 3 with a dealy of random_delay


Here is an advanced example for simulating a 50%-50% beam splitter for randomly redistributing a singal on channel 3 to channels 4 and 5.

.. code::
        VFILE(4)
        VFILE(5)
        INTEGER(retchn)
        a--3-->b:
            {
                options = np.asarray([4,5])
                retchn = np.random.choice(options)
            }
            emit(retchn,0)
