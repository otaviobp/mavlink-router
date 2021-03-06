## MAVLink Router ##

Route mavlink packets between endpoints (WIP)

In its current state it acts as a bridge between one "master" endpoint on UART
and other endpoints on UDP.

### Compilation and installation ###

In order to compile you need the following packages:

  - GCC or Clang compiler
  - C and C++ standard libraries

#### Fetch dependencies: ####

We currently depend on mavlink C library which is generated by the build
system during compilation. The corresponding submodule should be fetched.

    $ git submodule update --init --recursive

#### Build ####

Build system follows the usual configure/build/install cycle. Configuration is needed
to be done only once. A typical configuration is shown below:

    $ ./autogen.sh && ./configure CFLAGS='-g -O2' \
            --sysconfdir=/etc --localstatedir=/var --libdir=/usr/lib64 \
	    --prefix=/usr

Build:

    $ make

Install:

    $ make install
    $ # or... to another root directory:
    $ make DESTDIR=/tmp/root/dir install

### Running ###

To route mavlink packets from master `ttyS1` to 2 other UDP endpoints, do as
following:

    $ mavlink-routerd -b 1500000 -e 192.168.7.1:14550 -e 127.0.0.1:14550 /dev/ttyS1

The `-b` switch above is used to set the UART baudrate. See more options with
`mavlink-routerd --help`

It's also possible to route mavlinks packets from any interface using:

    $ mavlink-routerd -b 1500000 -e 192.168.7.1:14550 -e 127.0.0.1:14550  0.0.0.0:24550

mavlink-router also listens, by default, port 5760 for TCP connections. Any
connection there will also receive routed packets.

### Contributing ###

Pull-requests are accepted on GitHub. Make sure to check coding style with the
provided script in tools/checkpatch, check for memory leaks with valgrind and
test on real hardware.
