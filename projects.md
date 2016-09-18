---
layout: page
title: projects
permalink: /projects/
---

### [Using acoustic emission to control radiofrequency ablation](http://onlinelibrary.wiley.com/doi/10.1111/jce.12598/abstract)


My recent project at the Department of Cardiology of Westmead Public Hospital. Using in-vitro experiments we demonstrated that acoustic emission produced during Radio Frequency ablation in cardiac tissue can be used as an indicator of overheating. An automated system was developed that can adequately predict tissue overheating and potentially reduce clinical complications of RF ablation.

### [MUSHRA Test](http://mushra.kosobrodov.net)

Multichannel implementation of MUSHRA listening tests as specified by [ITU Recommendation BS.1534](https://www.itu.int/rec/R-REC-BS.1534/en). I developed this software for ambisonic experiments while working at the University of Sydney. 

Main features:

* Support for multichannel audio, as many channels as your hardware can handle
* Standalone application that does not rely on MAX, Matlab etc.
* Supports ASIO, Windows Audio and DirectX audio interfaces (on Windows)
* Written using [JUCE](http://juce.com) cross-platform C++ library and can be easily compiled for other platforms
* Open-source project released under GPL.

Visit [MUSHRA Test](http://mushra.kosobrodov.net) site for more details.

### [Multichannel Audio for Matlab](http://mcha.kosobrodov.net)

mCha is a cross-platform Matlab toolbox for simultaneous recording, processing and playback of multichannel audio. It is written in C++ using [JUCE](http://juce.com) and [FFTW](http://www.fftw.org) library and is optimised for simultaneous recording and playback of a large number of audio channels. It was intended to replace Matlab binding for PortAudio since we had problems using it. 

Main features:

* Support of 64 and 32 bit Matlab on Windows and Linux
* MEX functions return control to Matlab immediately after starting recording/playback thread and could be terminated by the user at any time
* Supported audio interfaces: ASIO, WindowsAudio and DirectSound
* Audio device settings could be stored in *.xml files allowing flexible change of configurations
* Playback file formats: WAV, OGG and AIFF with 16, 24 and 32 bit resolution
* Recording file format: 32-bit floating point WAV (one file for each channel)
* Recording and playback to/from Matlab matrices
* Real-time fast convolution of input and output signals using partitioned convolution technique
* User defined IIR filters can be applied to input and output signals in real time
* Multithreaded disk read/write operations.

Visit [Multichannel Audio for Matlab](http://mcha.kosobrodov.net) site for more details.
