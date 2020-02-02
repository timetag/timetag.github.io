Extensible Timetag Analyzer 
===============
[![Build status](https://ci.appveyor.com/api/projects/status/ajwi3hnfxnsakj0u?svg=true)](https://ci.appveyor.com/project/linzuzeng/etabackend)
[![License](https://img.shields.io/github/license/timetag/ETAServer.svg)](https://github.com/timetag/ETAServer/blob/master/LICENSE)
[![Github All Releases](https://img.shields.io/github/downloads/timetag/ETABackend/total.svg)](https://github.com/timetag/ETABackend/releases)
[![DOI](https://zenodo.org/badge/125106142.svg)](https://zenodo.org/badge/latestdoi/125106142)
[![Documentation Status](https://readthedocs.org/projects/eta/badge/?version=latest)](https://eta.readthedocs.io/en/latest/?badge=latest)
[![PyPI](https://img.shields.io/pypi/v/etabackend.svg)](https://pypi.org/project/etabackend/)


[![ETA](https://eta.readthedocs.io/en/latest/_static/logo.png)
](https://eta.readthedocs.io/en/latest/?badge=latest)

## Introduction

### What is time resolved measurements

Time resolved measurements are widely used for many research field, including

* Time correlated photon counting
* Fluorescence lifetime imaging
* Anti-bunching
* Linear optics quantum computing
* Quantum process tomography
* Scanning fluorescence microscopy
* Photo-activated localization microscopy (PALM)
* Stochastic optical reconstruction microscopy (STORM)
* Stimulated emission depletion (STED) microscopy

Depending on the duration of the experiments and frequency of events, time resolved measurements can easily generate huge amounts of data, which is overwhelming for common data analysis software.

###  How ETA is going to help you

We attempt to extract the useful information form data generated from time-resolved measurements by introducing a new kind of time-tag analysis software.

ETA, the extensible time tag analyzer, is an event driven programming language with graphical user interface for analyzing, plotting, and fitting of time tagged data.

Currently, ETA supports the timetag format of devices from PicoQuant (HydraHarp T2&T3 format, PicoHarp T2&T3 format, TimeHarp T2&T3 format), Swabian Instrument (TIME TAGGER 20 file format), quTools (quTAG file format), and Becker & Hickl (SPC-134/144/154/830 file format).

### ETA for realtime analysis (NEW!)

ETA provides support to the format of quTAG, HydraHarp, and Swebian Instrument time-tagger devices, with tested real-time g(2) analysis recipes that could help you with the diagnosis of experiment setups (e.g. the alignment of the optical path).

[![ETA for realtime analysis](https://img.youtube.com/vi/EtHMVqtiPIA/0.jpg)](https://www.youtube.com/watch?v=EtHMVqtiPIA")


## Get Started!
### Install ETA

For a full installation, [download](https://github.com/timetag/ETA/releases) and [install](https://eta.readthedocs.io/en/latest/installation.html) GUI and backend.

You can also try [ETA Web GUI](https://timetag.github.io/ETA/gui/src/renderer/) without installation.

### Recipies

Check out the pre-made [recipies](https://eta.readthedocs.io/en/latest/recipes.html) 🎉 that could be used immediately in your experiments.
