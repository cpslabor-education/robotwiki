# Building Kernel for Linux L4T

Sources:
- https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/xavier_toolchain.html
- https://docs.nvidia.com/jetson/l4t/index.html#page/Tegra%20Linux%20Driver%20Package%20Development%20Guide/kernel_custom.html#wwpID0E06C0HA

## Getting toolchain
Acquire GCC Linaro toolchain:
```bash
wget http://releases.linaro.org/components/toolchain/binaries/7.3-2018.05/aarch64-linux-gnu/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
```
Extract the downloaded sources:
```bash
mkdir $HOME/l4t-gcc
cd $HOME/l4t-gcc
tar xf gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu.tar.xz
```
Set the cross-compile environment variable:
```bash
export CROSS_COMPILE=$HOME/l4t-gcc/gcc-linaro-7.3.1-2018.05-x86_64_aarch64-linux-gnu/bin/aarch64-linux-gnu-
```

## Compiling kernel
Create workspace:
```bash
TEGRA_KERNEL_OUT=$HOME/l4t-robotlabor
cd <kernel_source>
mkdir -p $TEGRA_KERNEL_OUT
make ARCH=arm64 O=$TEGRA_KERNEL_OUT tegra_defconfig
```
Compile kernel:
```bash
make ARCH=arm64 O=$TEGRA_KERNEL_OUT -j4
```

## Kernel configuration
You can customize kernel with menuconfig:
```bash
make ARCH=arm64 O=$TEGRA_KERNEL_OUT nconfig
```

You might need __ncruses__:
```bash
sudo apt install libncurses-dev
```