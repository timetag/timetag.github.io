Customizing Display Panel
===============================

In each ETA recipe, Display Panel provides the user interface for each experiment. 

The common useage of Display Panel is to start an analysis on a existing time-tag file and display results, or provide interactive controls of the acquistion device (time-tagger) or the analysis.

EAT provides embedded Python and corresponding API to allow customization of the Display Panel. Here we list all the API in the latest version of ETA. For examples, please refer to the pre-made recipes.

Cutting time-tag files
------------------------------

You need to cut the time-tag file into sections before running the analysis on these sections. When cutting the time-tag file, ETA will read the header of the time-tag and then generate cut discriptors that can be later used in the analysis. You can think of ``_cut()`` as the timetag-specific way of ``open()`` in Python.  

eta.simple_cut(filename, cuts=1, trunc=-1, format=-1):
......

``simple_cut`` takes the file name and number of cuts, and cuts the file into equal size sections. It is useful in a correlational analyis if we cut the file into some sections an run parallel analysis to get speed boosts. 

- ``filename``
    File name of the timetag file. Please note that if you run ETA Backend on a remote computer, you need to specify the path on that computer.
    
- ``cuts``
    The number of cuts that you want to generate. Default value is set to 1, thus the full time-tag will be returned in the cut descriptor.
    
- ``trunc``
    The number of cuts that is returned in the cut descriptor. By setting it to ``-1`` will give you the number of cuts specified in the ``cuts`` parameter. By setting it to any number smaller than ``cuts`` you can truncate a large timetag file. 
    
- ``format``
    Format specifies the time-tag file format that you want to use in the analysis. The default is set to the auto dection of PicoQuant devices. You can also use the integer constant ``FORMAT_SI`` for Swebian Instrument timetags, ``quTAG_FORMAT_BINARY`` for quTAG 10-byte Binary format, or  ``quTAG_FORMAT_COMPRESSED`` for quTAG COMPRESSED Binary format. 
    
        The format of time-tag you use might influece the time tag analysis results.
        
        If the original file is recorded with absolute timing (like in HHT2 mode), then every cut should keep the same absolute timing. 

        If the original file is recorded with relative timing (like in HHT3 mode), then the absolute timing for each cut will take the first event in this cut as the reference of zero.


eta.incremental_cut()
......


eta.wait_for()

Executing recipies
-----

Multi-thread and single-thread mode are only describing how to run virtual instruments on the cuts. They should not affect analysis result. However, the way how the files is cutted might affect the result.

Multithread mode is by default enabled and the thread number is set to the number of CPU cores.

If the number of cuts fed into eta.run() is less then the number of threads, the extra threads will not be created.

If the number of cuts is larger than the number of threads, they will queue up. The thread who finishes processing his cut, will go fetch the next cut in the queue.

APIs:

eta.run()

eta.display()

eta.send()


Modifying recipies
------

APIs:

eta.recipe_update()

eta.recipe_get_parameter()

eta.recipe_set_parameter()

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


