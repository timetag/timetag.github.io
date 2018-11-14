============
Installation
============

Install as a standalone program
--------

You can install ETA GUI and ETA Backend as a standalone program. Currently Windows x68_64 builds is provided on Github.

Download
......

You can download ETA from Github Releases (https://github.com/timetag/ETA/releases). 
  
You need to download two files for the first installation,
  
*       the ``ETA_Install-win64.zip`` for the installer of ETA GUI and ETA Backend.
*       the ``ETA_LIB-win64.zip`` for the site-packages for embedded Python.

Install
......

For a new installation, you need to

*       Run the extracted ``ETA-Setup-x.x.x.exe`` to install ETA GUI and ETA Backend. 

*       Extract ``ETA_LIB.zip`` to ``C:\Users\***\AppData\Local\Programs\ETA\site-packages``
          (or any folder at least 6 levels deep in the file system to
          ensure you do not encounter a bug with Python module loader).
          
For updating the existing installation, you need to

*       Run the extracted ``ETA-Setup-x.x.x.exe`` to install ETA GUI and ETA Backend. 
  
*       Check the Github Releases (https://github.com/timetag/ETA/releases) for further information about whether the site-packages should be updated or not.
  
Configuration
......

*       ETA Backend asks for the full path to the extracted ``site-packages`` when you run it for the first time.
  
*       ETA Backend asks for the IP address and port number that you want to use for remote connection to this computer.
  
.. note::
    The computer which runs the ETA Backend will do the number crunching and it might be advantageous to run it on the computer which has the timetag files so the (potentially large) files do not have to be copied around. This is just a recommendation, though. 
 
  
*       In the ETA GUI you can then specify the IP address and port number of the computer running the backend, which can just be ``localhost:5678`` if you run backand and frontend on the same PC. 
  

Verify the Installation
......

After launching the ETA GUI, you can drag a recipe (``recipe_name.eta``) onto the main screen to load it. In order to analyze your time tag file, you have to specify path and filename in the variables and click "Run" button besides the Display Panel you want to execute.

Install as a Python pacakge
--------

Install
......

* Install Python 3
  
* Install ETA Dependencies
  
    - cffi 
    - numpy
    - llvmlite
    - numba 
    - astunparse (only works in Python <3.7.0)

* Install ETA backend
  
    ETA backend can be installed from git repository or your package manager. 
    
    (TODO: pip install) 
  
Configure Backend
......

The backend asks for the path to the site-packages of your Python installation.
   
Running the Web GUI
......

You can access the  `ETA Web GUI <https://timetag.github.io/ETA/gui/src/renderer/>`_ from any platform with a Web browser.
