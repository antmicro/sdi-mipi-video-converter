# Introduction

This document describes Antmicro's [SDI to MIPI CSI-2 Video Converter](https://github.com/antmicro/sdi-mipi-video-converter), which uses a Lattice Crosslink-NX FPGA and an SDI deserializer to perform the conversion.
SDI is a popular standard used in video broadcasting, while MIPI CSI-2 is a mobile/embedded camera standard that is directly supported by a wide range of SoCs.
The SDI to MIPI CSI-2 Video Converter also provides on-board DDR3 memory for processing the captured stream.

## Documentation structure

This documentation provides implementation details and usage instructions.
It is divided into the following chapters:

* {doc}`hardware` - description of the hardware, physical connections and configurations,
* {doc}`fpga_design` - how to generate FPGA bitstreams for all available SDI signal configurations,
* {doc}`software` - how to build and use a Linux BSP that supports the MIPI SDI Video Converter for Jetson Xavier NX,
* {doc}`cmos2dphy` - detailed description of the CMOS to D-PHY IP Core.

