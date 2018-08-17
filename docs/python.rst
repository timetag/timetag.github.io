Embedded Python
===============================


Cutting time-tag files
-----
eta.simple_cut()

eta.incremental_cut()

eta.wait_for()

Executing recipies
-----

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

Using your own  Python site-packages.
-----
Users can also use the packages from any Python 3 (x64) installation, if ETA_LIB is configured to the path to the Python site-packages.

ETA Dependencies:

- cffi 
- numpy
- llvmlite
- numba 
- astunparse (only works in Python <3.7.0)


More Examples
-----

For more examples please refer to the recipes.

