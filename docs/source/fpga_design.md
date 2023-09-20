# Building the FPGA design

The CrossLink-NX FPGA is responsible for converting data deserialized from SDI by the Semtech chip to MIPI CSI-2.
The core module of the FPGA design is a [CMOS to D-PHY converter](https://github.com/antmicro/sdi-mipi-video-converter-fpga-design/blob/main/src/cmos2dphy.py) which converts standard parallel video data into CSI-2 byte packets.

## CMOS to D-PHY Converter

The CMOS to D-PHY module provides a bridging solution for converting parallelized pixel data from the deserializer into a MIPI CSI-2 video stream.
The module supports all of the following formats:
* 720p25Hz YUV422,
* 720p30Hz YUV422,
* 720p50Hz YUV422,
* 720p60Hz YUV422,
* 1080p25Hz YUV422,
* 1080p30Hz YUV422,
* 1080p50Hz YUV422,
* 1080p60Hz YUV422.

For more details, refer to {doc}`cmos2dphy` chapter.

## Setting up the environment

The dependencies required to build the FPGA design can be installed by running:
```bash
pip3 install -r requirements.txt
```
In order to generate a bitstream from the generated sources, the [nextpnr](https://github.com/YosysHQ/nextpnr) toolchain is required.

## Building the bitstream

Once required tools are installed, project can be built by invoking:
```bash
make all
```

The output files are created in the `build/<video-format>-<lanes>` directory.

The Makefile provides configuration flags that can be set as presented below:

```bash
FLAG=flag_value make all
```

The list of available flags can be accessed with `make help` and is described below:
:::{list-table} Available `make` configuration flags.
:name: make-flags
:header-rows: 1
:widths: 15 60 25

* - Flag
  - Flag description
  - Default value
* - YOSYS
  - Path to yosys.
  - `yosys`
* - NEXTPNR
  - Path to nextpnr.
  - `nextpnr-nexus`
* - PRJOXIDE
  - Path to prjoxide.
  - `prjoxide`
* - ECPPROG
  - Path to ecpprog.
  - `ecpprog`
* - YOSYS_ARGS
  - Additional arguments for yosys.
  - `-o 1080p30-2lanes_syn.v`
* - SYNTH_ARGS
  - Additional arguments for Yosys the `synth_nexus` command.
  - `-flatten`
* - NEXTPNR_ARGS
  - Additional arguments for NEXTPNR.
  - `--placer-heap-timingweight 35`
* - PATTERN_GEN
  - Set to `1` if you want to generate design with embedded pattern generator.
  - None
* - SIM
  - Set to `1` if you want to generate Verilog sources ready for simulation with Modelsim Lattice FPGA Edition.
  - None
* - VIDEO_FORMAT
  - Video format, one of `720p60`, `1080p30` or `1080p60`.
  - `1080p30`
* - LANES
  - D-PHY lanes, must be either `2` or `4`.
  - `2`
* - TRACE
  - Set to `1` if you want to generate simulation waveforms.
  - None
:::

## Testing

If you want to test the CMOS to D-PHY Core in simulation, you can run the [cocotb](https://www.cocotb.org/) tests:

```bash
make tests
```

To run a single test:

```bash
cd tests
TOP=<module_name> make test
```
You can choose module names from `crc16`, `packet_formatter`, `mipi_dphy` and `cmos2dphy`.

**Note:** Verilator tests do not cover the D-PHY module since there is no open source simulation model available.
