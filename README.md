DRC's TurboJPEG/Java Build Scripts
==================================

These scripts are used to build the "official" TurboJPEG/Java packages, which
work on any Linux platform with GLIBC 2.17 and later, as well as Windows XP and
later and OS X/macOS 10.7 and later.

See **BUILDING.md** in the TurboJPEG/Java source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat Enterprise Linux 8 x86-64 (and its derivatives)


Build Environment: macOS
------------------------

macOS 10.15 (Catalina) or later required

CMake should be installed somewhere in the `PATH`.  The version in MacPorts
(<http://www.MacPorts.org>) works, or just install the CMake application from
the DMG (<http://www.cmake.org>) and add
**/Applications/CMake.app/Contents/bin** to the `PATH`.

Xcode 12.0 or later (available at <https://developer.apple.com/downloads> --
Apple ID required.)

Oracle JDK or OpenJDK


Build Environment: Windows
--------------------------

Windows XP 64-bit or later required

CMake (the Windows native version, not the Cygwin version) should be installed
somewhere in the `PATH`.

Ninja should be installed somewhere in the `PATH`.

The directory containing the 64-bit Visual C++ compiler should be listed in the
`PATH` environment variable.  The directory containing the 64-bit Windows SDK
libraries should be listed in the `LIB` environment variable.  The directories
containing the Visual C++ and Windows SDK header files should be listed in the
`INCLUDE` environment variable.  The easiest way to accomplish this is to use
the `vcvars64.bat` script provided by Visual C++, as described in the
TurboJPEG/Java build instructions.

The official TurboJPEG JNI library for Visual C++ is generated using
[Visual Studio 2022 Community Edition](https://visualstudio.microsoft.com), but
any reasonably modern version of Visual C++ and the Windows SDK should work.

The Windows native builds must be conducted from within an MSYS shell.  The
easiest way to do that is to install [MSYS2](http://www.msys2.org) and then use
its built-in package manager (pacman) to install the `make` package.

x64, x86, and Arm64 JDKs should be installed, but they do not need to be added
to the `PATH`.  You do, however, need to create the following symbolic links so
that the build scripts can find the JDKs.

From an Administrator Command Prompt, type:

    md %ProgramData%\Oracle\Java
    mklink /d %ProgramData%\Oracle\Java\javapath {directory of x64 JDK}
    mklink /d %ProgramData%\Oracle\Java32 {directory of x86 JDK}
    mklink /d %ProgramData%\Oracle\JavaArm64 {directory of Arm64 JDK}

Install all other software necessary to build a 32-bit and a 64-bit version of
TurboJPEG/Java (refer to **BUILDING.md**.)


Build Procedure
---------------

Executing

    buildtjj [branch/tag]

(where branch/tag is, for instance, "1.4.x" and defaults to "main") will
generate both a pristine source tarball and binaries for the platform on which
the script is executed.  These are placed under
**$HOME/src/tjj.nightly/YYYYMMDD/files**, where YYYYMMDD is a build number
based on today's date.  If the build is successful, then a sym link will be
created from **$HOME/src/tjj.nightly/latest** to
**$HOME/src/tjj.nightly/YYYYMMDD**.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running `buildtjj -e`
(assuming that **$HOME/src** is a shared directory.)

NOTE: On Windows, `buildtjj` should be run from inside an MSYS shell.  If you
are mounting your home directory as a drive letter (e.g. **H:**), then set the
`HOME` environment variable to the MinGW path for that drive (e.g. **/h**)
prior to running `buildtjj`.

Run `buildtjj -h` for usage information.


Signing the Packages
--------------------

To sign the packages using a GPG key, create a file called **gpgsign** in the
same directory as **buildtjj**, and include the following contents in the file:

    GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_PASS={password for GPG key}
