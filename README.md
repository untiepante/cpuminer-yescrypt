This is a multi-threaded CPU miner for [GlobalBoost-Y (BSTY)](http://globalboo.st/?CohibAA) using the [Yescrypt Algorithm](litecoin-p2pool.com/yescrypt/yescrypt-v0.pdf)

(fork of Jeff Garzik's reference cpuminer)

License: GPLv2.  See COPYING for details.

Git tree:   https://github.com/noncepool/cpuminer-yescrypt

Windows Binary: https://mega.co.nz/#!uQEQVCbD!PXXZzUHRWA0rnUQWRAp9uAKvCO6222aH1IvWWikLFU8

Dependencies:

	libcurl			[http://curl.haxx.se/libcurl/](http://curl.haxx.se/libcurl/)
	jansson			[http://www.digip.org/jansson/](http://www.digip.org/jansson/)
		(jansson is included in-tree)

Install Build Dependencies on Debian, Ubuntu and other APT-based distros:

    sudo apt-get install build-essential libcurl4-openssl-dev

Install Build Dependencies on Fedora, RHEL, CentOS and other yum-based distros:

    sudo yum install gcc make curl-devel

Install Build Dependencies on OpenSUSE and other ZYpp-based distros:

    sudo zypper in gcc make libcurl-devel

Basic *nix build instructions:

    ./autogen.sh	# only needed if building from git repo
    ./nomacro.pl	# only needed if building on Mac OS X or with Clang
    ./configure CFLAGS="-O3"
    make
	
	
Notes for AIX users:

	* To build a 64-bit binary, export OBJECT_MODE=64
	* GNU-style long options are not supported, but are accessible
	  via configuration file


Windows build instructions on Ubuntu 16.04LTS:

	sudo apt-get install gcc-mingw-w64
	cd cpuminer/depend
	sh depend.sh
	cd cpuminer
	./autogen.sh
	LDFLAGS="-L depend/curl-7.38.0-devel-mingw64/lib64 -static" LIBCURL="-lcurldll" \
	CFLAGS="-O3 -msse4.1 -funroll-loops -fomit-frame-pointer" \
	./configure --host=x86_64-w64-mingw32 --with-libcurl=depend/curl-7.38.0-devel-mingw64
	make


Architecture-specific notes:

	ARM:	No runtime CPU detection. The miner can take advantage
		of some instructions specific to ARMv5E and later processors,
		but the decision whether to use them is made at compile time,
		based on compiler-defined macros.
		
		To use NEON instructions, add "-mfpu=neon" to CFLAGS.
		
	x86:	The miner checks for SSE2 instructions support at runtime,
		and uses them if they are available.
		
	x86-64:	The miner can take advantage of AVX, AVX2 and XOP instructions,
		but only if both the CPU and the operating system support them.
		
		    * Linux supports AVX starting from kernel version 2.6.30.
		    * FreeBSD supports AVX starting with 9.1-RELEASE.
		    * Mac OS X added AVX support in the 10.6.8 update.
		    * Windows supports AVX starting from Windows 7 SP1 and
		      Windows Server 2008 R2 SP1.
		      
		The configure script outputs a warning if the assembler
		doesn't support some instruction sets. In that case, the miner
		can still be built, but unavailable optimizations are left off.

Usage instructions:  Run "minerd --help" to see options.

To Connect to BSTY-P2Pool using your GlobalBoost-Y Wallet Address:
    ./minerd --algo=yescrypt -o 54.68.63.254:7225 -u BSTYaddress -p x

Note: Do not mine to online wallet addresses, including exchanges, as you may experience loss of coins due to how newly mined coins are confirmed.

Connecting through a proxy:  Use the --proxy option.

To use a SOCKS proxy, add a socks4:// or socks5:// prefix to the proxy host.

Protocols socks4a and socks5h, allowing remote name resolving, are also
available since libcurl 7.18.0.

If no protocol is specified, the proxy is assumed to be a HTTP proxy.

When the --proxy option is not used, the program honors the http_proxy
and all_proxy environment variables.

Also many issues and FAQs are covered in these forum threads dedicated to this program:

* [Original Pooler CPUMiner Bitcointalk Thread](https://bitcointalk.org/index.php?topic=55038.0)
* [GlobalBoost-Y BSTY Bitcointalk Official Thread](https://bitcointalk.org/index.php?topic=775289.0)
