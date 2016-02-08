DRC's libjpeg-turbo Build Scripts
=================================

These scripts are used to build the "official" libjpeg-turbo binaries, which
work on any Linux platform with GLIB 2.5 and later, as well as Windows XP and
later and OS X 10.5 and later.

See BUILDING.txt in the libjpeg-turbo source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 5 64-bit

Both 64-bit and 32-bit JDKs should be installed.  The 64-bit version should be
in your PATH, and the directory containing the 32-bit version should be
symlinked to /usr/java/default32.

Install all other software necessary to build a 32-bit and a 64-bit version of
libjpeg-turbo (refer to BUILDING.txt.)

To reproduce the official libjpeg-turbo distribution, you will need to install
newer versions of m4, libtool, autoconf, and automake from source.  The
autotools.install script, located in the same directory as this README file,
will download and build the necessary packages and install them under
/opt/autotools.  You will need to add /opt/autotools/bin to the PATH prior to
invoking buildljt.


Build Environment: OS X
-----------------------

OS X 10.8 (Mountain Lion) or later required

GCC 5 (installed through MacPorts)

Xcode 4.3.x (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce
libjpeg-turbo binaries that are backward compatible with the iPhone 3G.
Xcode should be installed under /Applications/Xcode43.app.

Xcode 4.6.x (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce
libjpeg-turbo binaries that are compatible with the iPhone 5 and iPad 4.
Xcode should be installed under /Applications/Xcode46.app.  NOTE: we could use
Xcode 5.1.x, as below, but Xcode 4.6.x was the last version to include
LLVM-GCC, which performs better with libjpeg-turbo than clang.

Xcode 5.1.x (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce
libjpeg-turbo binaries that are compatible with the iPhone 5S and later and
OS X 10.5.  Xcode should be installed under /Applications/Xcode51.app.

Apple Java for OS X (needed in order to test the 32-bit x86 build)

Oracle JDK


Build Environment: Windows (not Cygwin)
---------------------------------------

Windows XP 64-bit or later required

CMake (the Windows native version, not the Cygwin version) should be installed
somewhere in the PATH.

The directory containing the 64-bit Visual C++ compiler
(e.g. c:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\bin\amd64)
should be listed in the PATH environment variable.

The directory containing the 64-bit Windows SDK libraries
(e.g. C:\Program Files\Microsoft SDKs\Windows\v7.0\Lib\x64)
should be listed in the LIB environment variable.

The directories containing the Visual C++ and Windows SDK header files
(e.g. c:\Program Files (x86)\Microsoft Visual Studio 10.0\VC\include and
C:\Program Files\Microsoft SDKs\Windows\v7.0\include)
should be listed in the INCLUDE environment variable.

The official libjpeg-turbo binaries for Visual C++ are generated using the
Windows SDK for Windows 7 and .NET Framework 3.5 SP1
(http://www.microsoft.com/en-us/download/details.aspx?id=3138),
which contains Visual C++ 10.0, but any reasonably modern version of Visual
C++ and the Windows SDK should work.

The Windows native builds (both Visual C++ and MinGW) must be conducted from
within an MSYS shell.  MSYS can be installed from:
http://downloads.sourceforge.net/mingw/MSYS-1.0.11.exe
Edit /etc/fstab in your MSYS installation and assign the /mingw mount point
to the directory containing a 32-bit MinGW toolchain and the /mingw64 mount
point to the directory containing a 64-bit MinGW64 toolchain (the
x32-4.8.1-win32-dwarf and x64-4.8.1-win32-seh toolchains from MinGW-builds are
used to generate the official libjpeg-turbo binaries for GCC, but TDM-GCC and
other distributions work as well.)

Both 64-bit and 32-bit JDKs should be installed, but they do not need to be
added to the PATH (CMake should find them automatically.)

Install all other software necessary to build a 32-bit and a 64-bit version of
libjpeg-turbo (refer to BUILDING.txt.)


Build Procedure
---------------

Executing

  buildljt [repository path]

(where repository path is, for instance, "branches/1.2.x", and defaults to
"trunk") will generate both a pristine source tarball and binaries for the
platform on which the script is executed.  These are placed under
$HOME/src/ljt.nightly/YYYYMMDD/files, where YYYYMMDD is a build number based
on today's date.  If the build is successful, then a sym link will be created
from $HOME/src/ljt.nightly/latest to $HOME/src/ljt.nightly/YYYYMMDD.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running 'buildljt -e'
(assuming that $HOME/src is a shared directory.)

NOTE: On Windows, buildljt should be run from inside a MinGW shell.  If you
are mounting your home directory as a drive letter (e.g. H:), then set the HOME
environment variable to the MinGW path for that drive (e.g. /h) prior to
running buildljt.

Run 'buildljt -h' for usage information.


Signing the Linux Packages
--------------------------

To sign the RPMs and DEBs using a GPG key, create a file called "gpgsign" in
the same directory as buildljt, and include the following contents in the file:

GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
GPG_KEY_PASS={password for GPG key}

debsigs (available at https://gitlab.com/debsigs/debsigs/tags) must be
installed and in the PATH.


Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called "mssign" in the same directory as buildljt, and include the following
contents in the file:

MS_KEY_FILE={full path to a .pfx file containing the code signing certificate}
MS_KEY_PASS={password for certificate}

signtool (available in the Windows SDK) must be in the PATH.


Distributing a Pre-Release Build
--------------------------------

Executing

  uploadljt <SourceForge user name>

will upload the files from $HOME/src/ljt.nightly/latest/files to
http://libjpeg-turbo.sourceforge.net/ljt.nightly

Having an SSH key installed is highly recommended to avoid having to enter your
password multiple times.
