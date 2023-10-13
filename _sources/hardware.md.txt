# Hardware setup

A complete setup using the SDI-MIPI Video Converter would typically consist of at least 3 devices:

- {ref}`converter` - Antmicro's [SDI to MIPI CSI-2 Video Converter](https://github.com/antmicro/sdi-mipi-video-converter), responsible for deserializing SDI data and packeting it with the MIPI CSI-2 protocol,
- {ref}`transmitter` - a device transmitting SDI video data, connected to the SDI input port of the bridge,
- {ref}`receiver` - a device receiving MIPI CSI-2 video data, connected to the MIPI CSI-2 output port of the bridge.

(converter)=
## SDI Video Converter

The core part of this setup is the SDI to MIPI CSI-2 Video Converter.

:::{figure-md}
![SDI to MIPI CSI-2 Video Converter board](img/sdi_mipi_video_converter.png)

SDI to MIPI CSI-2 Video Converter board, rev. 1.0.0
:::

### Features

* Integrated SDI adaptive cable equalizer and output loopback connector
* Three 4-lane MIPI D-PHY transceivers at 6 Gbps per PHY exposed on two 50-pin FFC connectors
* Ability to power devices from the 5V and 3.3V rails of the side FFC connector
* Ability to power the board from either USB or Antmicro's 50-pin FFC connector
* Lattice Crosslink-NX FPGA for processing SDI and CSI streams
* 2Gbit (128Mbx16) DDR3L DRAM memory for the FPGA to support stream processing
* Ability to interface and program the FPGA via the side FFC I{sup}`2`C bus

### Architecture

The video converter board consists of the SDI deserializer, Lattice Crosslink-NX FPGA, and on-board DDR3L memory.
The {numref}`video-converter-architecture` visualizes an architecture and data exchange between the board components.

:::{figure-md} video-converter-architecture
![SDI to MIPI CSI-2 Video Converter block diagram](img/video-converter-board-flow.png)

SDI to MIPI CSI-2 Video Converter block diagram
:::

An I{sup}`2`C or JTAG commands must come from outside of the board in order to program the FPGA chip.

(transmitter)=
## Transmitter

The transmitter is a device (typically a camera) outputting data via the SDI interface.
It must be compatible with the 1080p30Hz YUV422 video format.

Several input devices have been tested with SDI to MIPI CSI-2 Video Converter, e.g:

* Atomos Shogun Flame,
* [Blackmagic HDMI to SDI Converter](https://www.blackmagicdesign.com/products/microconverters),
* [Decimator HDMI/SDI 4K Cross Converter](http://decimator.com/Products/MiniConverters/12G-CROSS/12G-CROSS.html).

(receiver)=
## Receiver

The receiver is a device that can receive data through the MIPI CSI-2 interface.
The [Software section](software.md) of this documentation covers support for the Jetson Xavier NX board.

Antmicro provides [sources](https://github.com/antmicro/sdi-mipi-bridge-linux/tree/video-converter) for a Linux distribution configured for SDI Video Converter support for Jetson Xavier NX.
