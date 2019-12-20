============
Installation
============

Installing ETA
--------
ETA is comprised of two parts communicating via websocket: the GUI and the backend. We chose to separate the program in this way to allow remote analysis of time tags. Since correlators are typically close to the setup and require a fast interface for data transfer, it is advisable to run the backend on the computer controlling the correlator. The GUI, however, can be used from anywhere with network access to the data aquisition computer. If you transfered the time tag files to another computer for evaluation, make shure you are running both the GUI and the backend locally.

Currently, ETA has been tested on 64-bit versions of Microsoft Windows 7/10 and Ubuntu 18.04/19.04 with Python 3.6/3.7, but it may also work nicely on other platforms. We recommend users to install ETA as a standalone program on Windows, and as a Python package on other platforms. 

Install as a standalone program
......

You can install ETA GUI and ETA Backend as a standalone program. Currently, Windows x68_64 binary builds are provided on Github.

For a new installation, you need to

You can download ETA from Github Releases (https://github.com/timetag/ETA/releases). 
  
*      You need to download the ``ETA_Install-win64.zip`` for the installer of ETA GUI and ETA Backend. 

.. note::
    Please note that ``ETA_LIB-win64.zip`` is no longer required for ETA (>=0.6.0).
 
*       Run the extracted ``ETA-Setup-x.x.x.exe`` to install ETA GUI and ETA Backend. (It is recommended to temporarlily disable realtime thread scanning on your anti-virus software to accelerate the file unzipping.)

*       After installation, two icons will be created on the desktop. Click the `ETA Backend` (icon with black background) to start the backend first, and then click `ETA` (icon with white background) to launch the GUI.

Updating the existing installation:

*       ETA will attempt to download a new release if one exists at program start. It will then be automatically installed when the program is closed. If you prefrer to do it manually you can run the extracted ``ETA-Setup-x.x.x.exe`` like in a fresh installation. 
  
*       Check the Github Releases (https://github.com/timetag/ETA/releases) for further information about whether the recipes should be updated.


Install as a Python package
......
For offical Python distribution:
,,,,

* Install ETA Backend 
    ``pip install etabackend``
    
* Run ETA Backend
    ``python -m etabackend``
For Anaconda/Miniconda in separate environment:
,,,,

* Install ETA Backend 
    open Anaconda promt and enter
    
    ``conda create -n ETA python=3``
     
    ``conda install -n ETA pip``
    
    ``activate ETA``
    
    ``pip install etabackend``
    
* Run ETA Backend
    Create a text file and add the following, adjusting the path to your Anaconda installation, then save save as a .bat file. 
    
    .. code::
    
            @echo off
            set PATH=%PATH%;C:\Users\<username>\Anaconda3\Scripts
            @call C:\Users\<username>\Anaconda3\Scripts\activate.bat ETA
            @call python -m etabackend
    
    When running sucessfully, this .bat file should look like
    
    .. figure:: _static/ETA_backend.jpg
        :align: center
        :width: 50 %
        
* Run the Web GUI

     Please not that the GUI will not be installed when ETA is installed as a Python package. Instead, you can access the  `ETA Web GUI <https://timetag.github.io/ETA/gui/src/renderer/>`_ on any platform with a Web browser.

Verify the Installation
......

After launching the ETA GUI, you can drag a recipe (``recipe_name.eta``) onto the main screen to load it. In order to analyze your time tag file, you have to specify path and filename in the variables and click "Run" button besides the Display Panel you want to execute.


Configurating ETA for remote access
--------

The computer which runs the ETA Backend will do the number crunching and it might be advantageous to run it on the computer which has the timetag files so the (potentially large) files do not have to be copied around. This is just a recommendation, though. 

*   In the ETA GUI you can then specify the IP address and port number of the computer running the backend, which can just be ``localhost:5678`` if you run backend and frontend on the same PC. 

*    ETA Backend uses enviroment variable ETA_IP and ETA_PORT for IP address and port that you want to use for remote connection to this computer. 
  
.. note::
     Remote access might be dangerous if your IP and port are directly accessible via Internet. Considering setting up a firewall.
