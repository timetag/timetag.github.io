Embedded Python for Display Panel
===============================


Cutting time-tag files
-----

A cut is a section of an existing file.

If the original file is recorded with absolute timing (like in HHT2 mode), then every cut should keep the same absolute timing. 

If the original file is recorded with relative timing (like in HHT3 mode), then the absolute timing for each cut will take the first event in this cut as the reference of zero.


eta.simple_cut()

eta.incremental_cut()

eta.wait_for()

Executing recipies
-----

Multi-thread and single-thread mode are only describing how to run virtual instruments on the cuts. They should not affect analysis result. However, the way how the files is cutted might affect the result.

Multithread mode is by default enabled and the thread number is set to the number of CPU cores.

If the number of cuts fed into eta.run() is less then the number of threads, the extra threads will not be created.

If the number of cuts is larger than the number of threads, they will queue up. The thread who finishes processing his cut, will go fetch the next cut in the queue.

eta.run()

eta.display()

eta.send()


Modifying recipies
------

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



More Examples
-----

For more examples please refer to the recipes.

