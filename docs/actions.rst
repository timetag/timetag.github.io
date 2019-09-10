Tools and Actions
===============================

Virtual Instruments are used to perform analysis in an ETA recipe. As mentioned before, each Virtual Instrument consists of a graph on the left-hand side and a code panel on the right-hand side.

In the code panel, users can put what should be done when there is a state change on the graph, which are called Actions.

Each Action belongs to a certian Tool. For example, if you want to record a time interval of two events, you will need to create a Tool called CLOCK.

Each Tool can be created with a user-specified name and some other parameters. The name is used to refer to the Tool later when performing Actions. The parameters that have default values can be omitted.

In the following documentation, we list the built-in Tools and their Actions in the current version of ETA. 

.. note::
    Please note that ETA is still under development. Tools and their Actions might be changed in the future.

CLOCK
------------------------------

CLOCK is a time interval recorder with a start button and a stop button. The CLOCKS remembers the time when it is started or stopped, and it calculates the time interval as the output.

.. note::
        The buttons can be pressed for multiple times. The recoder has a maximum number of recorded events, and it will drop the oldest event when it reach this limit. 
        
        Please also note that ETA will never automatically clear the recordings in the CLOCK. It can only overwritten.

Parameters
......

- ``Max Start Times`` (default:1)
    The Max Start Times parameters specifies the number of start events are preserved in the recorder. 
    
- ``Max Stop Times`` (default:1)
    The Max Start Times parameters specifies the number of stop events are preserved in the recorder.

.. note::

    Max Stop Times should be set to 1, and Max Start Times should be set to a very large number when doing correlational measurements, so that you correlate each signal on one channel to every singal on the other channel.

Actions
......


- ``clock.start()``
    Start the clock at the current time.
    
 
- ``clock.stop()``
    Stop the clock at the current time.

- ``clock.infer_start_from_stop(SYNC)``
    Using the value of stop to find the last specified signal before the stop, and then overwirte the start to the time of this signal.
    
    Currently it can only use the SYNC period to find the SYNC signal. This Action might be removed if there is better idea for supporting Life-time measurements using HydraHarp T3 files.

    If the clock is a single-start-multi-stop clock, then the earliest stop value it remembers will be used.

- ``[clock1,clock2,...].sort(start)``
    Sort the starting time time of a group of clocks, preserving their stopping time.
    This is useful if you want to do some measurements that records multi-dimensional histograms with the axis indicating the arrival order (first photon, second photon) instaed of channels (detector1, detector2).

    Please note that multi-CLOCK is not yet supported.
    The first parameters can be also changed to ``stop``.

Examples
......

Performing a start-stop measurement.

.. code-block:: python    
   
    CLOCK(c1,1,1)
    started:
        c1.start()
    stopped:
        c1.stop()  
        t1.record(c1)

HISTOGRAM
------------------------------
Histogram stores statistics with time intervals or an arbitray INTEGER. Histograms can be 1-dimensional or multi-dimensional. 
The histogram name can be read out from the Script Panel for further processing.

Parameters
......

- ``Number of bins`` (required)
    Number of bins in the histogram. If the histogram is multi-dimensional, specify value for each dimension like ``[100,200]``.

- ``Width (in ps) of bins`` (required)
    The size of each bin in the histogram. If the histogram is multi-dimensional, specify value for each dimension like ``[16,16]``.


Actions
......

- ``histogram.record(clock)``
    Record a time interval of the CLOCK ``clock`` into a 1-dimensional histogram. The values that falls out of the histogram will not be ignored.

- ``histogram.record(clock1,clock2,...)``
    Record a time interval of the CLOCK ``clock1`` and ``clock2`` into a multi-dimensional histogram. The values that falls out of the histogram will not be ignored.

- ``histogram.record_all(clock)``
    Record all the time intervals of the CLOCK into the histogram. The values that falls out of the histogram will not be ignored.

    .. note::
        The product of the histogram parameters (bin size and bin number) gives you the maximum correaltion length if you are performing a correlational analysis.
        
        Using record_all with multi-dimensional histogram is not yet fully tested.

Examples
......

Performing a correaltion:

.. code-block:: python    
   
    CLOCK(c1,100,1)
    started:
        c1.start()
    stopped:
        c1.stop()  
        t1.record_all(c1)

COINCIDENCE
------------------------------
Coincidence is a tool that emits a signal when all of its slots are fulfilled.

Parameters
......

- ``Coincidence Slots`` (required)
    The number of coincidence slots on this Coincidence counter.

- ``Emit to this channel# when fulfilled`` (required)
    Emit to this channel# when fulfilled


Actions
......


- ``coincidence.fill(slotid)``
    Mark the coincidence slot `slotid` with the current time. A signal will then be emitted if all of the slots are fulfilled.

- ``coincidence.clear()``
    Clear all coincidence slots.


SELF
------------------------------
The instrument itself is also a Tool. When using actions, it doesn't need to be referred by name.

Actions
......


- ``emit(chn, waittime=0, period=0, repeat=1)``
    Emit signal to chn after ``waittime``. It can also emit some repeated signal if  ``repeat`` is set to larger than one, with a `period` in ps.
    
    The maximum limit of channel number ``chn`` is 255.
 
    .. note::
        It is not allowed to emit to any channel that is read from a timetag file (timetagger channels or markers). The emited signal will never be written to the timetag file to prevent corrputing the original data.

        If you need to merge signals from two channels into one channel, simply emit them into a new unused channel.

        Channels can also be used as routers. For examples, you can route events to different Virtual Instruments based on some status that is controlled by the markers.
        
- ``cancel_emit(chn)``
    Flush all the previously emitted events in the channel ``chn``.

    Canceling emitting a real channel from a timetag file will terminate the analysis before the ETA reaches the ending of the current section of the file.

Examples
......

Making a dealy line:

.. code-block:: python    
   
    ch0_event:
        emit(2, 130)# here, ch0 is duplicated to ch2 with a delay of 130ps, making a delay line


Extending actions using embedded code
-----------------------

Apart from the built-in actions, you can also use a embedded code to extend the functionality of ETA.

Embedded code can be wrapped in a  ``{`` and ``}`` . If the code contains curly brackets, a pair of ``{{{`` and ``}}}`` can be used.

The embedded code uses a restricted sub-set of Python language, and a limited subset of numpy function is imported with `np`. Intenally, ETA uses Numba to compile the Python code into LLVM and link it with the built-in actions and other parts of the program. 

.. note::
    Please note that features that requires ``import`` , ``exec`` or file I/O are not available. Calling built-in actions in embedded code is not currently supported.
    
    We noticed that the built-in tools and actions already serve as a good basis for many different experiments. But we still want to add more actions for different analysis purposes. 
    

Examples
....

Here is an example for generating random numbers on transtion from a to b. You can then emit signal using the result from the embedded Python code, which might be useful for monte calor simulations.

.. code::

      a--1-->b:
          start(c1) # execute bulit-in action
          #execute the embedded Python code
          {   
              mu, sigma = 0, 0.1 # mean and standard deviation
              s = np.random.normal(mu, sigma) #generate random numbers
              print(s) # print the genreated floating number
          }
          