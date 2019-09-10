Tools and Actions
===============================

Virtual Instruments are used to perform analysis in an ETA recipe. As mentioned before, each Virtual Instrument consists of a graph on the left-hand side and a code panel on the right-hand side.

In the code panel, users can put what should be done when there is a state change on the graph, which are called Actions.

Each Action belongs to a certian Tool. For example, if you want to record a time interval of two events, you will need to create a Tool called CLOCK.

Each Tool can be created with a user-specified name and some other parameters. The name is used to refer to the Tool later when performing Actions. The parameters that have default values can be omitted.

In the following documentation, we list the built-in Tools and their Actions in the current version of ETA. 

.. note::
    Please note that ETA is still under development, and Tools and their Actions might be changed in the future.


CLOCK
------------------------------

CLOCK is a time interval recorder with a start button and a stop button. The CLOCKS remembers the time when it is started or stopped, and it calculates the time interval as the output.

Parameters
......

- ``Max Start Times`` (default:1)
    The Max Start Times parameters specifies the number of start events are preserved in the recorder. 
    
- ``Max Stop Times`` (default:1)
    The Max Start Times parameters specifies the number of stop events are preserved in the recorder.

    .. note::
        Currently, Max Stop Times can only be set to 1.

Actions
......


- ``clock.start()``
    Start the clock at the current time.
    
    .. note::
        The buttons can be pressed for multiple times. The recoder has a maximum number of recorded events, and it will drop the oldest event when it reach this limit.

- ``clock.stop()``
    Start the clock at the current time.


Examples:

.. code-block:: python    
   
    CLOCK(c1,1,1)
    started:
        c1.start()
    stopped:
        c1.stop()  
        t1.record(c1)

HISTOGRAM
------------------------------
Histogram stores statistics with time intervals or an arbitray INTEGER. The histogram name can be read out from the Script Panel for further processing.

Parameters
......

- ``Number of bins`` (required)
    Number of bins in the histogram. 

- ``Width (in ps) of bins`` (required)
    The size of each bin in the histogram.


Actions
......


- ``histogram.record(clock)``
    Record a time interval of the CLOCK into the histogram. The values that falls out of the histogram will not be ignored.

- ``histogram.record_all(clock)``
    Record all the time intervals of the CLOCK into the histogram. The values that falls out of the histogram will not be ignored.

    .. note::
        The product of the histogram parameters (bin size and bin number) gives you the maximum correaltion length if you are performing a correlational analysis.
        

Examples:

.. code-block:: python    
   
    CLOCK(c1,100,1)
    started:
        c1.start()
    stopped:
        c1.stop()  
        t1.record_all(c1)

SELF
------------------------------
The instrument itself is also a Tool. When using actions, it doesn't need to be referred by name.

Actions
......


- ``emit(????)``
    TODO: emit
        

Examples:

.. code-block:: python    
   
    ch0_event:
        emit(2, 130)# here, ch0 is duplicated to ch2 with a delay of 130ps, making a delay line


Extending actions using embedded code
-----------------------

Apart from the built-in actions, you can also use a embedded code to extend the functionality of ETA.

Embedded code can be wrapped in a  ``{`` and ``}`` . If the code contains curly brackets, a pair of ``{{{`` and ``}}}`` can be used.

The embedded code uses a restricted sub-set of Python language. Intenally, ETA uses Numba to compile the Python code into LLVM and link it with the built-in actions and other parts of the program. 

Please note that features that requires ``import`` , ``exec`` or file I/O are not available. Calling built-in actions in embedded code is not currently supported.

However, a limited subset of numpy function is imported with `np`. 
Here is an example for generating random numbers on transtion from a to b.

.. code::

      a--1-->b:
          start(c1) # execute bulit-in action
          #execute the embedded Python code
          {   
              mu, sigma = 0, 0.1 # mean and standard deviation
              s = np.random.normal(mu, sigma) #generate random numbers
              print(s) # print the genreated floating number
          }
          # you can also emit signal using the result from the embedded Python code, which might be useful for monte calor simulations

