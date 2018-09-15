============
Installation
============

Windows (as a standalone program)
--------

* Download

  You can download ETA from here: https://github.com/timetag/ETA/releases . What you should download:

  *       the ``ETA_Install-win64.zip``
  *       the ``ETA_LIB-win64.zip`` 

* Install

  *       Run the extracted ``ETA-Setup-x.x.x.exe`` to install ETA GUI and ETA Backend. 

  *       Extract ``ETA_LIB.zip`` to ``C:\Users\***\AppData\Local\Programs\ETA\site-packages``
          (or any folder at least 6 levels deep in the file system to
          ensure you do not encounter a bug with Python module loader).

* Configure Backend
    
  The backend asks for the path to the extracted ``site-packages`` and your IP address when you run it for the first time.

  The computer which runs the backend will do the number crunching and it might be advantageous 
  to run it on the computer which has the timetag files so the (potentially large) files do
  not have to be copied around. This is just a recommendation, though.
  
  In the frontend (installed by the ETA-Setup-x.x.x.exe) you can then specify the IP address of the computer 
  running the backend, which can just be localhost if you run backand and frontend on the same PC. 
    

* Verify the Installation

  After running the frontend you can drag a recipe (``recipe_name.eta``) onto the main
  screen to load it. In order to analyze your time tag file you have to specify path 
  and filename in the variables and click “Run” on the code panel you want to execute.

Other enviroments
-------

* Install

  * Install ETA Dependencies:
  
    - cffi 
    - numpy
    - llvmlite
    - numba 
    - astunparse (only works in Python <3.7.0)

  * Install the ETA backend from git repository or your package manager. (TODO: pip install) 
  
* Configure Backend

    The backend asks for the path to the site-packages of your Python installation.
   
* Running the Web GUI

    You can access the  `ETA Web GUI <https://timetag.github.io/ETA/gui/src/renderer/>`_ from any platform with a Web browser.
