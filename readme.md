# ARM Embedded Toolchain 2025q4 (15.2.rel1)

This is a copy of the GNU toolchain published by ARM

https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads

https://gitlab.arm.com/tooling/gnu-devtools-for-arm

Attempting to build the bare metal toolchain for other host platforms
ARM doesn't publish in their release, mainly Intel-based MacOS and 32
bit Raspberry Pi.

So far this is a work-in-progress.  Toolchain builds, but is untested.


## Toolchain Source Code

```
wget -N https://developer.arm.com/-/media/Files/downloads/gnu/15.2.rel1/srcrel/arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
mkdir -p src
tar -C src -xf arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
```

SHA256: `06f4e5792b566a771c300ac6f46c3422e23e34f4e3a5d44a8cb5594ead6fb82e`



## MacOS

Using MacOS 12.7.6, install homebrew and `Command_Line_Tools_for_Xcode_14.2.dmg`

```
brew install gnu-getopt
brew install autoconf automake libtool texinfo
brew remove zstd
```

Command to build:

```
./build-baremetal-toolchain.sh --enable-newlib-nano --release --disable-qemu --target=arm-none-eabi --config-flags-gcc=--with-multilib-list=aprofile,rmprofile --host=x86_64-apple-darwin
```

Final output file in bin-tar


## Raspberry Pi

Using Raspbian GNU/Linux 12 (bookworm)

```
sudo apt install autoconf automake libtool texinfo
sudo apt install bison flex
```

Ugly hack inside script looks for "pi5" hostname.  Edit to your Pi's hostname.


Command to build:

```
./build-baremetal-toolchain.sh --enable-newlib-nano --release --disable-qemu --target=arm-none-eabi --host=arm-linux-gnueabihf --config-flags-gcc='--with-multilib-list=aprofile,rmprofile --host=arm-linux-gnueabihf --build=arm-linux-gnueabihf' --config-flags-host-tools='--host=arm-linux-gnueabihf --build=arm-linux-gnueabihf' --config-flags-binutils='--host=arm-linux-gnueabihf --build=arm-linux-gnueabihf'
```


## Cleanup

```
rm -rf obj install nano_install install-qemu host-tools bin-tar ,stage build.status
rm -rf src
rm -f arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
rm -f arm-gnu-toolchain-15.2.rel1-*
```



