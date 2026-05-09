# ARM Embedded Toolchain 2025q4 (15.2.rel1)

This is a copy of the GNU toolchain published by ARM

https://developer.arm.com/downloads/-/arm-gnu-toolchain-downloads

https://gitlab.arm.com/tooling/gnu-devtools-for-arm

Attempting to build the bare metal toolchain for other host platforms
ARM doesn't publish in their release, mainly Intel-based MacOS and 32
bit Raspberry Pi.

So far this is a work-in-progress.  Toolchain builds, and the x86_64
build for Intel-based Macintosh has successfully compiled a program
which ran correctly on Teensy 4.0.  Much more testing is needed.


## Toolchain Download

Look for Releases on this github repository.


## Toolchain Source Code

To begin the long process of building the toolchain from source, download the
source code snapshot.

```
wget -N https://developer.arm.com/-/media/Files/downloads/gnu/15.2.rel1/srcrel/arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
mkdir -p src
tar -C src -xf arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
```

Optional, check the SHA256 signature to confirm arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz was downloaded correctly.

SHA256: `06f4e5792b566a771c300ac6f46c3422e23e34f4e3a5d44a8cb5594ead6fb82e`

Building the toolchain takes several hours and consumes about 60GB disk space.


## Macintosh - Intel Based

Using MacOS 12.7.6, install homebrew and `Command_Line_Tools_for_Xcode_14.2.dmg`

```
brew install gnu-getopt bison
brew install autoconf automake libtool texinfo
brew remove zstd
```

Command to build:

```
./build-baremetal-toolchain.sh --enable-newlib-nano --release --disable-qemu --target=arm-none-eabi --config-flags-gcc=--with-multilib-list=aprofile,rmprofile --host=x86_64-apple-darwin
```

Rename final output:

```
name='arm-gnu-toolchain-15.2.rel1-darwin-x86_64-arm-none-eabi'
mkdir -p bin-tar/$name
cd bin-tar/$name
tar -xf ../arm-none-eabi-tools.tar.xz
cd ..
tar -cJf ../$name.tar.xz $name
cd ..
shasum -a 256 $name.tar.xz > $name.tar.xz.sha256asc
```


## Raspberry Pi - 32 bit

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

Rename final output:

```
name='arm-gnu-toolchain-15.2.rel1-gnueabihf-arm-none-eabi'
mkdir -p bin-tar/$name
cd bin-tar/$name
tar -xf ../arm-none-eabi-tools.tar.xz
cd ..
tar -cJf ../$name.tar.xz $name
cd ..
sha256sum $name.tar.xz > $name.tar.xz.sha256asc
```

## Cleanup

```
rm -rf obj install nano_install install-qemu host-tools bin-tar ,stage build.status
rm -rf src
rm -f arm-gnu-toolchain-src-snapshot-15.2.rel1.tar.xz
rm -f arm-gnu-toolchain-15.2.rel1-*
```



