Pinuxcoin Core integration/staging tree

https://pinuxcoin.org
What is Pinuxcoin?

Pinuxcoin is an experimental new digital currency that enables instant payments to anyone, anywhere in the world. Pinuxcoin uses peer-to-peer technology to operate with no central authority: managing transactions and issuing money are carried out collectively by the network. Pinuxcoin Core is the name of open source software which enables the use of this currency.

For more information, as well as an immediately useable, binary version of the Pinuxcoin Core software, see https://pinuxcoin.org
License

Pinuxcoin Core is released under the terms of the MIT license. See COPYING for more information or see http://opensource.org/licenses/MIT.
Development process

Developers work in their own trees, then submit pull requests when they think their feature or bug fix is ready.

If it is a simple/trivial/non-controversial change, then one of the Pinuxcoin development team members simply pulls it.

If it is a more complicated or potentially controversial change, then the patch submitter will be asked to start a discussion (if they haven't already) on the mailing list.

The patch will be accepted if there is broad consensus that it is a good thing. Developers should expect to rework and resubmit patches if the code doesn't match the project's coding conventions (see doc/coding.md) or are controversial.

The master-0.10 branch is regularly built and tested, but is not guaranteed to be completely stable. Tags are created regularly to indicate new official, stable release versions of Pinuxcoin.
Testing

Testing and code review is the bottleneck for development; we get more pull requests than we can review and test on short notice. Please be patient and help out by testing other people's pull requests, and remember this is a security-critical project where any mistake might cost people lots of money.
Manual Quality Assurance (QA) Testing

Large changes should have a test plan, and should be tested by somebody other than the developer who wrote the code. Creating a thread in the Pinuxcoin discussion forum will allow the Pinuxcoin development team members to review your proposal and to provide assistance with creating a test plan.
Translations

Important: We do not accept translation changes as GitHub pull requests because the next pull from Transifex would automatically overwrite them again.

We only accept translation fixes that are submitted through Bitcoin Core's Transifex page. Translations are converted to Pinuxcoin periodically.
Development tips and tricks

compiling for debugging

Run configure with the --enable-debug option, then make. Or run configure with CXXFLAGS="-g -ggdb -O0" or whatever debug flags you need.

debug.log

If the code is behaving strangely, take a look in the debug.log file in the data directory; error and debugging messages are written there.

The -debug=... command-line option controls debugging; running with just -debug will turn on all categories (and give you a very large debug.log file).

The Qt code routes qDebug() output to debug.log under category "qt": run with -debug=qt to see it.

testnet and regtest modes

Run with the -testnet option to run with "play litecoins" on the test network, if you are testing multi-machine code that needs to operate across the internet.

If you are testing something that can run on one machine, run with the -regtest option. In regression test mode, blocks can be created on-demand; see qa/rpc-tests/ for tests that run in -regtest mode.

DEBUG_LOCKORDER

Pinuxcoin Core is a multithreaded application, and deadlocks or other multithreading bugs can be very difficult to track down. Compiling with -DDEBUG_LOCKORDER (configure CXXFLAGS="-DDEBUG_LOCKORDER -g") inserts run-time checks to keep track of which locks are held, and adds warnings to the debug.log file if inconsistencies are detected. "# Pinuxcoin" "# Pinuxcoin" 

WINDOWS BUILD NOTES

Below are some notes on how to build Bitcoin Core for Windows.

Most developers use cross-compilation from Ubuntu to build executables for Windows. This is also used to build the release binaries.

While there are potentially a number of ways to build on Windows (for example using msys / mingw-w64), using the Windows Subsystem For Linux is the most straight forward. If you are building with an alternative method, please contribute the instructions here for others who are running versions of Windows that are not compatible with the Windows Subsystem for Linux.
Compiling with the Windows Subsystem For Linux

With Windows 10, Microsoft has released a new feature named the Windows Subsystem for Linux. This feature allows you to run a bash shell directly on Windows in an Ubuntu based environment. Within this environment you can cross compile for Windows without the need for a separate Linux VM or Server.

This feature is not supported in versions of Windows prior to Windows 10 or on Windows Server SKUs.

To get the bash shell, you must first activate the feature in Windows.

    Turn on Developer Mode
        Open Settings -> Update and Security -> For developers
        Select the Developer Mode radio button
        Restart if necessary
    Enable the Windows Subsystem for Linux feature
        From Start, search for "Turn Windows features on or off" (type 'turn')
        Select Windows Subsystem for Linux (beta)
        Click OK
        Restart if necessary
    Complete Installation
        Open a cmd prompt and type "bash"
        Accept the license
        Create a new UNIX user account (this is a separate account from your Windows account)

After the bash shell is active, you can follow the instructions below for Windows 64-bit Cross-compilation. When building dependencies within the 'depends' folder, you may encounter an error building the protobuf dependency. If this occurs, re-run the command with sudo. This is likely a bug with the Windows Subsystem for Linux feature and may be fixed with a future update.
Cross-compilation

These steps can be performed on, for example, an Ubuntu VM. The depends system will also work on other Linux distributions, however the commands for installing the toolchain will be different.

Make sure you install the build requirements mentioned in build-unix.md. Then, install the toolchains and curl:

sudo apt-get install g++-mingw-w64-i686 mingw-w64-i686-dev g++-mingw-w64-x86-64 mingw-w64-x86-64-dev curl

To build executables for Windows 32-bit:

cd depends
make HOST=i686-w64-mingw32 -j4
cd ..
./autogen.sh # not required when building from tarball
./configure --prefix=`pwd`/depends/i686-w64-mingw32
make

To build executables for Windows 64-bit:

cd depends
make HOST=x86_64-w64-mingw32 -j4
cd ..
./autogen.sh # not required when building from tarball
./configure --prefix=`pwd`/depends/x86_64-w64-mingw32
make

