============
Installation
============

Windows
--------

* Download

  You can download ETA from here: https://github.com/timetag/ETA/releases . What you should download:

  *       the ``ETA_Install-win64.zip``
  *       the ``ETA_LIB-win64.zip`` (recommended, alternatively an existing Python 3 (x64) site-packages location can be used,
          which is usually located at ``C:\Users\***\AppData\Local\Programs\Python\Python36\Lib\site-packages``)

* Installation

  *       Run the extracted ``ETA-Setup-x.x.x.exe`` to install ETA GUI and ETA Backend. 

  *       The ``ETA_LIB.zip`` should be extracted to a folder at least 6 levels deep in the file system to
          ensure you do not encounter a bug with windows 
          (e.g. ``C:\folder1\folder2\folder3\folder4\folder5\site-packages``).

* Configure (Backend)
    
  The backend asks for the path to the extracted site-packages when you run it for the first time.

  The computer which runs the backend will do the number crunching and it might be advantageous 
  to run it on the computer which has the timetag files so the (potentially large) files do
  not have to be copied around. This is just a recommendation, though. In the frontend
  (installed by the ETA-Setup-x.x.x.exe) you can then specify the ip of the computer 
  running the backend, which can just be localhost if you run back and front end on the same PC. 
    

* Verify the Installation

  After running the frontend you can drag a recipe (``recipe_name.eta``) onto the main
  screen to load it. In order to analyze your time tag file you have to specify path 
  and filename in the variables and click “Run” on the code panel you want to execute.

Linux & Mac OS
-------

TODO: Compile and install on Linux

Web GUI
-------
You can access the  `ETA Web GUI <https://timetag.github.io/ETA/gui/src/renderer/>`_ from any platform with a Web browser.
