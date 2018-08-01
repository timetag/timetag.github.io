.. ETA documentation master file, created by
   sphinx-quickstart on Mon Jul 30 15:47:22 2018.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

Introduction
===============================
Time resolved measurements are widely used for many research
field, including

- Time correlated photon counting
- Fluorescence lifetime imaging
- Anti-bunching
- Linear optics quantum computing
- Quantum process tomography
- Scanning fluorescence microscopy
- Photo-activated localization microscopy (PALM)
- Stochastic optical reconstruction microscopy (STORM)
- Stimulated emission depletion (STED) microscopy

Depending on the duration of the experiments and frequency of
events, time resolved measurements can easily generate huge
amount of data, which is overwhelming for common data analysis
software.


We attempt to extract the useful information form data generated
from time-resolved measurements by introducing a new kind of
time-tag analysis software.

ETA, the extensible time tag analyzer, is an event driven programming
language with graphical user interface for analyze, plot, and fit time
tagged data.

Why using ETA?
----------------------------

Fast
   In order to make ETA fast, our approach is twofold.
   First, we try to find the fastest algorithm for time-tag processing.
   On the other hand, we ETA utilizes LLVM, one of the state-of-the-
   art low-level code optimizers to perform assembly code
   transformation, to generate fast target-specific code for Intel x64
   and other processors.
   
Flexible
   ETA allows user defined analysis which is suitable for most of the
   existing analysis methods, which previously required using
   different software. ETA is also prepared for upcoming tasks.
   ETA allows fast processing of large amounts of time-tagged data,
   with a scalability from personal computers to super computers.
   
User-Friendly
   It provides an easy-to-understand graphical user interface (GUI),
   with a novel programming paradigm dedicated to simplifying time
   tag processing.


Tutorial
===============================

.. toctree::

   installation
   workflow
   recipes
   python
   advanced
