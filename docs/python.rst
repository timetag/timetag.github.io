Customizing Display Panel
===============================

In each ETA recipe, Display Panel provides the user interface for each experiment. 

The common useage of Display Panel is to start an analysis on a existing time-tag file and display results, or provide interactive controls of the acquistion device (time-tagger) or the analysis.

EAT provides embedded Python and corresponding API to allow customization of the Display Panel. Here we list all the API in the latest version of ETA. For examples, please refer to the pre-made recipes.

.. note::
    When running the Display Panel, the entire recipe will be sent to ETA Backend. ETA will first check the Virtual Instruments and then start exectuing the code for the current Display Panel. 
    
    The code is executed in an isolated enviroment with an ``eta`` object and other user-defiened parameters (only those in the same group of Display Panel will be visible) as global variables.
    
    You can also import third-party Python libraries like ``numpy`` or ``matplotlib``, etc. 
    
    Please note that ETA Backend can only run one Display Panel at the same time. If you have multiple ETA GUI conneted to ETA Backend, the running requests will be put in a queue. 


Cutting time-tag files
------------------------------

You need to cut the time-tag file into sections before running the analysis on these sections. When cutting the time-tag file, ETA will read the header of the time-tag and then generate cut discriptors that can be later used in the analysis. You can think of ``_cut()`` as the timetag-specific way of ``open()`` in Python.  

eta.simple_cut(filename, cuts=1, trunc=-1, format=-1)
......

``simple_cut`` takes the file name and number of cuts, and cuts the file into equal size sections. It is useful in a correlational analyis if we cut the file into some sections an run parallel analysis to get speed boosts. 

- ``filename``
    File name of the timetag file. Please note that if you run ETA Backend on a remote computer, you need to specify the path on that computer.
    
- ``cuts``
    The number of cuts that you want to generate. A list of cut descriptors will be retured if you specify a value larger than one. Default value is set to 1, thus the full time-tag will be returned in one cut descriptor.
    
- ``trunc``
    The number of cuts that is returned in the cut descriptor. By setting it to ``-1`` will give you the number of cuts specified in the ``cuts`` parameter. By setting it to any number smaller than ``cuts`` you can truncate a large timetag file. 
    
- ``format``
    Format specifies the time-tag file format that you want to use in the analysis. The default is set to the auto dection of PicoQuant devices. You can also use the integer constant ``FORMAT_SI`` for Swebian Instrument timetags, ``quTAG_FORMAT_BINARY`` for quTAG 10-byte Binary format, or  ``quTAG_FORMAT_COMPRESSED`` for quTAG COMPRESSED Binary format. 
    
    .. note::
        The format of time-tag you use might influece the time tag analysis results.
        
        If the original file is recorded with absolute timing (like in HHT2 mode), then every cut should keep the same absolute timing. 

        If the original file is recorded with relative timing (like in HHT3 mode), then the absolute timing for each cut will take the first event in this cut as the reference of zero.


eta.incremental_cut(filename, cut=None, rec_per_cut=-10, format=-1, verbose=True)
......
``simple_cut`` takes the previous cut discriptor and and number of records, and generate a new cut discriptor by sliding the cut window in the timetag-file. It is useful for implementing real-time analysis by interatively fed the retured cut descriptor into itself.

- ``filename``
    The same as ``simple_cut``. 
    
- ``cut``
    The cut descriptor of the previous cut. If there is no cut provided, a new cut will be generated.

- ``rec_per_cut``
    The number of records in the returned cut. 
    
    .. note::
        Please note that if the number exceeds the size of the timetag file, a cut will still be returned, and it will point to a non-existing area of a timetag file. 

        You can also set a negative value, then the number of records in this cut will be calculated as the number of records beteewn the ending of the last cut to the current end of file minus the absolute value of this nagatvie number. 

        The timetag file that serves as the FIFO when you perform a realtime analysis might have pre-initialized-but-not-yet-written areas, and the negative value here can help you get rid of that.
    
- ``format``
    The same as ``simple_cut``.
    
- ``format``
    The same as ``simple_cut``.
    
- ``verbose``
    Specify if the cut information will be displayed on the analysis log.
    
eta.wait_till_presnese(cut, timeout=1, raiseerr=False):
......
``wait_till_presnese`` will validate if the cut is presense using the current size of the file in this cut descriptor. A bollean will be returend as result. If you run a real-time analysis you can then loop on ``eta.wait_till_presnese``  to wait until the cut is filled with records. 

- ``cut``
    The cut descriptor to be validated.

- ``timeout``
    Value in seconds, specify the maximum waiting time for ``wait_till_presnese`` .
    
- ``raiseerr``
    Specify if an ValueError will be raised when timeout happens.

Executing Analysis
-----

eta.run(cuts, ctxs=None, sum_results=True, return_ctxs=False, group="main", verbose=True)
......

Once you have cuts, you can run Virtual Instruments and fed the cuts into the instruments and obtain results. The analysis resutls will be returend in a Python dictionaray, with the histogram names as the keys.

- ``cuts``
    The cut descriptors that is fed into the instruments.
    
    .. note::
        There was a parameter for multi-thread and single-thread mode in previus verisons of ETA, and it is recenlty removed.
        
        Multi-thread and single-thread mode are only describing how to run virtual instruments on the cuts. They should not affect analysis result. However, the way how the files is cutted might affect the result.

        Multithread mode is by default enabled and the thread number is set to the number of CPU cores.

        If the number of cuts fed into eta.run() is less then the number of threads, the extra threads will not be created.

        If the number of cuts is larger than the number of threads, they will queue up. The thread who finishes processing his cut, will go fetch the next cut in the queue.
        
- ``ctxs``
    The context information of the analysis. 

- ``sum_results``
    Specifies if the results will be sumed up. This is useful for correlational analysis if you cut the file into pieces and then merges the histograms together. Users can also set this value to False and implement their own data aggreation methods, like concating the histograms to generate large images.

- ``return_ctxs``
    Specifies if the extra context information should be returned, so that you can iteratively call this function using the returned context. If this value is set to False then only the results will be returned. 
    
- ``group``
    The group of instruments that you want to run analysis on.

- ``verbose``
    Specifies if the inforamtion of analysis will be displayed on the GUI.

eta.display(app)
......

You can send results to ETA GUI using this function. The value of app can be either a Dash or Bokeh graph currently.

.. note::
    Use ``app = dash.Dash()`` to create a Dash graph.

eta.send(text,endpoint="log"):
......
This is the ETA alternatve for ``print()`` in Python.

- ``text``
    String of information to be sent to ETA GUI. 
    
- ``endpoint``
    Can either be ``log`` or ``err``, for indicating the type of message.
    
Modifying recipies
------

eta.recipe_update()
......
Send the updated recipe on ETA Backend back to ETA GUI.

eta.recipe_get_parameter(name)
......
Get a parameter in a recipe using the name of the parameter. If there are multiple parameters with the same name, only the first one will be returned.

- ``name``
    Name of the parameter, as shown in the ETA GUI.
    
eta.recipe_set_parameter(name, value)
......
Set value of a parameter in a recipe using the name of the parameter. If there are multiple parameters with the same name, only the first one will be changed.

- ``name``
    Name of the parameter, as shown in the ETA GUI.
    
- ``value``
    Value of the parameter, as shown in the ETA GUI.
    
List of packages in ETA_LIB
-----

ETA_LIB is the standard distribution for ETA packages, which includes some commonly used libraries.

- numpy
- scipy
- lmfit
- matplotlib
- dash
- dash-renderer 
- dash-html-components 
- dash-core-components
- plotly
- bokeh


