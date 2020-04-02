Building rtl-sdr libraries on the mingw64 platform

Download and install msys2
https://www.msys2.org/
Version used
msys2-x86_64-20190524.exe

1. The tools and compiler can be downloaded and install using packman.
Run msys2.exe and execute commands in the console.
Package database synchronization and update. May require restarting the console.
#pacman -Syu
#pacman -Su

Necessary tools and package:
#pacman -S git
#pacman -S sed
#pacman -S mingw-w64-x86_64-gcc
#pacman -S mingw-w64-x86_64-libiconv mingw-w64-x86_64-iconv
#pacman -S make autoconf cmake 
#pacman -S msys/libtool msys/autoconf msys/automake-wrapper
		

2. Path settings and toolchain checking
#export PATH=$PATH:/mingw64:/mingw64/bin
#gcc --version
gcc.exe (Rev2, Built by MSYS2 project) 9.2.0
Copyright (C) 2019 Free Software Foundation, Inc.
This is free software; see the source for copying conditions.  There is NO
warranty; not even for MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.


3. Installation of the libusb library
	cd /usr/src
	git clone https://github.com/libusb/libusb.git
	cd libusb
	git reset --hard 1ce667ff7be902e376647ea81d812b75f3e43a00	
	./bootstrap.sh
	mkdir build
	cd build
	../configure --prefix=/usr --build=x86_64-w64-mingw32 --host=x86_64-w64-mingw32 --enable-examples-build --enable-tests-build
	make 
	make install

4. Installation of the rtl_sdr library.
	cd /usr/src
	git clone git://git.osmocom.org/rtl-sdr.git
	cd rtl-sdr
	wget https://github.com/anovch/rtl-sdr_mingw64/blob/master/mingw64_patch.diff
	git apply ./mingw64_patch.diff
	mkdir build
	cd build
	cmake ..
	make

5. Packing.
	mkdir /usr/src/rtl-sdr/package
	cd /usr/src/rtl-sdr/package
    	cp /usr/src/libusb/build/libusb/.libs/libusb-1.0.dll ./
	cp /usr/src/rtl-sdr/build/src/*.exe ./ 
	cp /usr/src/rtl-sdr/build/src/*.dll ./ 
	cd ..
	tar -zcvf rtl-sdr-bin.tar.gz ./package
	
	
	