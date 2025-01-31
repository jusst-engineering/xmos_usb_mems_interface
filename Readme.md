# XMOS USB - MEMS microphone array audio interface
Hardware and firmware files for a MEMS microphones USB audio interface based on the XMOS XUF216. Allows to connect up to  16 microphones and provides the option to cascade multiple base boards.  

This work was done as part of my master's thesis at the TU Berlin, Department of Engineering Acoustics [https://www.akustik.tu-berlin.de/menue/home/parameter/en/](https://www.akustik.tu-berlin.de/menue/home/parameter/en/). The system was evaluated with the Acoular Framework for beamforming [http://www.acoular.org/](http://www.acoular.org/).

<img src="https://github.com/simongapp/xmos_usb_mems_interface/blob/master/images/IMG_5449.JPG">  

## Features
- Operates at 48kHz/24bit, with USB 2.0 (UAC2)
- One base board can process up to 16 MEMS microphones
- Cascade multiple baseboards by sharing a word clock or sync to external clock master
- Physically detached microphones, allows to add microphones in pairs of two
- Max. cable length between microphones and base board: 2.5m @ 48kHz
- Impedance matched clock and data transmission lines

## Folders
- 00PCB: EAGLE PCB files for the microphone PCBs and base board
- 01Firmware: XMOS firmware, port documentation and visualizations of the reference code

## Usage
- Clone to your local hard drive

- How to add the firmware files to xTimeComposer (based on https://gist.github.com/ahogen/93bf33ce47bb96c7fda60d844238fa43):  
  1. Link (but don't copy) projects and libraries from SVN dir into workspace
     1. File > Import > General > Existing Projects into Workspace
     2. Choose "Select root directory"
     3. Click "Browse" and navigate to the SVN dir where your projects and libraries were exported
     4. Click "Select All" to choose all projects and libraries
     5. Make sure "Copy projects into workspace" is not checked
     6. Finish

## Known issues
- Data transfer from the microphone boards to the base board leads to inverted data signals on the base board. Fixed by multiplying -1 on the PCM signals.
- No deterministic phase relationship between input and output of the CS2100. This results in a non-deterministic clock shift of up to 162.5ns@48kHz. Considered negligible with respect to the period of audio frequencies from 50ms(20Hz) to 42us(20kHz).
- The current firmware starts audio sampling as soon as the uC is ready, which leads to different buffer sizes and thus a delay of several samples at the host when using multiple base boards. Solution: Synchronize start of audio sampling to first SOF after device configuration. To be implemented in the future.

## License
MIT License

Copyright (c) 2021 Simon Gapp

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
