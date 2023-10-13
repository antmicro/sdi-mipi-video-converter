# Preparing the software

To receive video from a SDI-MIPI Video Converter, the receiver must include drivers for the Semtech GS2971A deserializer.

## Building the BSP

To enable the SDI-MIPI Video Converter support, you need to build the kernel with the required drivers and the device tree for the board.
Antmicro provides a preconfigured [Linux kernel](https://github.com/antmicro/sdi-mipi-bridge-linux) for the Jetson Xavier NX.

To fetch, build, and add the modified kernel to a stock [L4T 32.4.4 package](https://developer.nvidia.com/embedded/linux-tegra-r3244), you must do the following:

1. Get and extract the cross-compilation toolchain:

```bash
wget http://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/aarch64-linux-gnu/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
tar xf gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
export PATH=$(pwd)/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu/bin:$PATH
```

2. Get and set up the L4T-based host software:

```bash
wget https://developer.nvidia.com/embedded/L4T/r32_Release_v4.4/r32_Release_v4.4-GMC3/T186/Tegra186_Linux_R32.4.4_aarch64.tbz2
tar xf Tegra186_Linux_R32.4.4_aarch64.tbz2
wget https://developer.nvidia.com/embedded/L4T/r32_Release_v4.4/r32_Release_v4.4-GMC3/T186/Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2
sudo tar xf Tegra_Linux_Sample-Root-Filesystem_R32.4.4_aarch64.tbz2 -C Linux_for_Tegra/rootfs/
pushd Linux_for_Tegra
sudo ./apply_binaries.sh
sudo chown -R $USER rootfs/lib/modules
sudo chown -R $USER rootfs/lib/firmware
popd
```

3. Clone the kernel sources and `checkout` the branch that matches your hardware:

```bash
git clone https://github.com/antmicro/sdi-mipi-bridge-linux
pushd sdi-mipi-bridge-linux
git checkout video-converter
```

4. Build the kernel using `make`:

```bash
export ARCH=arm64
export CROSS_COMPILE=aarch64-linux-gnu-
make tegra_defconfig
make -j$(nproc)
```

5. Install the kernel image, modules and device tree blob:

```bash
cp ./arch/arm64/boot/Image ../Linux_for_Tegra/kernel/
cp ./arch/arm64/boot/dts/tegra194-p3668-all-p3509-0000.dtb ../Linux_for_Tegra/kernel/dtb/
INSTALL_MOD_PATH=../Linux_for_Tegra/rootfs/ make modules_install
sudo chown -R root ../Linux_for_Tegra/rootfs/lib/modules
sudo chown -R root ../Linux_for_Tegra/rootfs/lib/firmware
popd
```

## Uploading the BSP

To flash the software on the device, put the device in recovery mode, connect it to the host PC with a USB cable, and use the following command:

```bash
pushd Linux_for_Tegra
sudo ./flash.sh jetson-xavier-nx-devkit-emmc mmcblk0p1
popd
```
