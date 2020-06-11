# Python PJSIP
Building SIP applications using PJSIP with Python3 Bindings

## Setup PJSIP
PJSIP is a free and open source multimedia communication library written in C language implementing standard based protocols such as SIP, SDP, RTP, STUN, TURN, and ICE. It combines signaling protocol (SIP) with rich multimedia framework and NAT traversal functionality into high level API that is portable and suitable for almost any type of systems ranging from desktops, embedded systems, to mobile handsets.

Download the latest version of PJSIP from [here](https://www.pjsip.org/download.htm)

## Build and Install PJSIP
Download PJSIP from their official site and `untar` it. You will get a folder `pjproject-x-y`. Let's say `pjproject-2.10` (at the time of this writing). Run the following commands to install PJSIP on `Ubuntu`:

```bash
$ sudo apt install build-essential python3 python3-dev libasound2-dev
$ cd pjproject-2.10/
$ export CFLAGS="$CFLAGS -fPIC"
$ ./configure --enable-shared
$ make dep
$ make
$ sudo make install
```
This will install PJSIP into your machine. The libraries are installed in `/usr/local/lib`. Follow the instructions below to add library to the PATH.

1. Create a new file in /etc/ld.so.conf.d/ called pjsip.conf
2. Edit the file and add `/usr/local/lib` (this is where our pjsip library files are available)
3. Reload with `sudo ldconfig`

## Build and Install Python3 bindings for PJSIP
Run the following commands to get the Python3 binding from [here](https://github.com/mgwilliams/python3-pjsip)

```bash
$ cd pjproject-x.x/pjsip-apps/src/
$ git clone https://github.com/mgwilliams/python3-pjsip.git
$ cd python3-pjsip
```
Before starting the build, you have to comment few lines of code to make DTMF working.

1. Open `_pjsua.c` in `python3-pjsip` folder
2. Search for `py_pjsua_call_dial_dtmf`
3. Comment the lines as shown below:

```cpp
static PyObject *py_pjsua_call_dial_dtmf(PyObject *pSelf, PyObject *pArgs)
{    	
    ...
    
    if (!PyArg_ParseTuple(pArgs, "iO", &call_id, &pDigits)) {
        return NULL;
    }

    /*if (!PyBytes_Check(pDigits))
	return Py_BuildValue("i", PJ_EINVAL);*/

    digits = PyUnicode_ToPJ(pDigits);
    ...
}
```

Build and Install now:

```bash
$ python3 setup.py build
$ sudo python3 setup.py install
```

Test for successful installation:

```bash
$ python3
Python 3.6.9 (default, Apr 18 2020, 01:56:04) 
[GCC 8.4.0] on linux
Type "help", "copyright", "credits" or "license" for more information.
>>> import pjsua
>>> 
```
PJSIP is successfully installed with Python3 bindings

