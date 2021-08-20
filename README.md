Extensible Timetag Analyzer 
===============
[![CI](https://github.com/timetag/ETA/workflows/CI/badge.svg)](https://github.com/timetag/ETA/actions)
[![License](https://img.shields.io/github/license/timetag/ETAServer.svg)](https://github.com/timetag/ETAServer/blob/master/LICENSE)
[![Github All Releases](https://img.shields.io/github/downloads/timetag/ETABackend/total.svg)](https://github.com/timetag/ETABackend/releases)
[![DOI](https://zenodo.org/badge/125106142.svg)](https://zenodo.org/badge/latestdoi/125106142)
[![Documentation Status](https://readthedocs.org/projects/eta/badge/?version=latest)](https://eta.readthedocs.io/en/latest/?badge=latest)
[![PyPI](https://img.shields.io/pypi/v/etabackend.svg)](https://pypi.org/project/etabackend/)


[![ETA](https://eta.readthedocs.io/en/latest/_static/logo.png)
](https://eta.readthedocs.io/en/latest/?badge=latest)

Our newly published [article (Z. Lin et al 2021 JINST 16 T08016)](https://doi.org/10.1088/1748-0221/16/08/T08016) explains the ideas underpinning ETA.

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

Currently, ETA supports timetag formats of devices from PicoQuant (HydraHarp T2&T3 format, PicoHarp T2&T3 format, TimeHarp T2&T3 format), Swabian Instrument (TIME TAGGER 20 file format), quTools (quTAG file format), and Becker & Hickl (SPC-134/144/154/830 file format).

### Realtime streaming analysis 

ETA provides support to file formats of quTAG, HydraHarp, and Swabian Instruments time-to-digital devices, with tested real-time g(2) analysis recipes that could help you with diagnosing your experiment setup (e.g. the alignment of the optical path).

Cick the video below to see how it works!

[![ETA for realtime analysis](https://img.youtube.com/vi/jZfEFNOJwpM/0.jpg)](https://www.youtube.com/embed/jZfEFNOJwpM)

### Correlating data from multiple timetagers (NEW!)

The latest version of ETA makes it easier to import and correlate timetag files from many different time-tagger devices. 

## Get Started!
### Install ETA

For a full installation, [download and install](https://eta.readthedocs.io/en/latest/installation.html) GUI and backend. Make sure to grab the latest [release](https://github.com/timetag/ETA/releases).

If you are using Linux or Mac OS, our backend is also available on [pypi](https://pypi.org/project/etabackend/) and you can use your favorite web browser to launch the [ETA GUI (instructions)](https://eta.readthedocs.io/en/latest/installation.html) as a installation-free frontend. Of course, Windows users are free to do the same.

### Recipies

Check out the pre-made [recipies](https://eta.readthedocs.io/en/latest/recipes.html) 🎉 that could be used immediately in your experiments.

### Citing ETA

ETA is a result of our scientific research. A Zenodo entry ([![DOI](https://zenodo.org/badge/125106142.svg)](https://zenodo.org/badge/latestdoi/125106142)) tracks every released version and can be cited in the 
Methods section to help readers to identify the specific software version. We discuss the concepts behind ETA in our [recent article (Z. Lin et al 2021 JINST 16 T08016)](https://doi.org/10.1088/1748-0221/16/08/T08016)
and would appreciate it if you cited it in your work.
