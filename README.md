DRC's libjpeg-turbo Build Scripts
=================================

These scripts are used to build the "official" libjpeg-turbo binaries, which
work on any Linux platform with GLIB 2.5 and later, as well as Windows XP and
later and OS X 10.7 and later.

See **BUILDING.md** in the libjpeg-turbo source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 6 64-bit

Both 64-bit and 32-bit JDKs should be installed.  The 64-bit version should be
in your `PATH`, and the directory containing the 32-bit version should be
symlinked to **/usr/java/default32**.

Install all other software necessary to build a 32-bit and a 64-bit version of
libjpeg-turbo (refer to **BUILDING.md**.)


Build Environment: OS X
-----------------------

OS X 10.12 (Sierra) or later required

CMake should be installed somewhere in the `PATH`.  The version in MacPorts
(<http://www.MacPorts.org>) works, or just install the CMake application from
the DMG (<http://www.cmake.org>) and add
**/Applications/CMake.app/Contents/bin** to the `PATH`.

Xcode 8.3.x (available at <https://developer.apple.com/downloads> --
Apple ID required.)  Xcode should be installed under
**/Applications/Xcode83.app**.

Oracle JDK


Build Environment: Windows (not Cygwin)
---------------------------------------

Windows XP 64-bit or later required

CMake (the Windows native version, not the Cygwin version) should be installed
somewhere in the `PATH`.

The directory containing the 64-bit Visual C++ compiler should be listed in the
`PATH` environment variable.  The directory containing the 64-bit Windows SDK
libraries should be listed in the `LIB` environment variable.  The directories
containing the Visual C++ and Windows SDK header files should be listed in the
`INCLUDE` environment variable.  The easiest way to accomplish this is to use
the `vcvars64.bat` script provided by Visual C++, as described in the
libjpeg-turbo build instructions.

The official libjpeg-turbo binaries for Visual C++ are generated using
[Visual Studio 2015 Community Edition](https://visualstudio.microsoft.com), but
any reasonably modern version of Visual C++ and the Windows SDK should work.
Note, however, that **jpegXX.dll** will depend on the C run-time DLLs from
whichever version of Visual C++ was used to build it.

The Windows native builds (both Visual C++ and MinGW) must be conducted from
within an MSYS shell.  The easiest way to do that is to install
[MSYS2](http://www.msys2.org) and then use its built-in package manager
(pacman) to install the `make` package.

In order for the libjpeg-turbo static libraries to be compatible with a wide
variety of MinGW distributions, it is necessary to build libjpeg-turbo using
specific versions of the MinGW toolchain.  Use the
[MinGW-builds installer](http://mingw-w64.org/doku.php/download/mingw-builds)
to install the following toolchain versions into their default directories:

* Version 6.4.0 / x86_64 / POSIX threads / SEH exception handling / Rev. 0
* Version 6.4.0 / i686 / POSIX threads / DWARF exception handling / Rev. 0

Both 64-bit and 32-bit JDKs should be installed, but they do not need to be
added to the `PATH`.  You do, however, need to create a symbolic link to the
32-bit JRE so that the build scripts can find it.

From an Administrator Command Prompt, type:

    mklink /d %ProgramData%\Oracle\Java\Java32 {directory of 32-bit JRE}

Install all other software necessary to build a 32-bit and a 64-bit version of
libjpeg-turbo (refer to **BUILDING.md**.)


Build Procedure
---------------

Executing

    buildljt [branch/tag]

(where branch/tag is, for instance, "1.4.x" and defaults to "master") will
generate both a pristine source tarball and binaries for the platform on which
the script is executed.  These are placed under
**$HOME/src/ljt.nightly/YYYYMMDD/files**, where YYYYMMDD is a build number
based on today's date.  If the build is successful, then a sym link will be
created from **$HOME/src/ljt.nightly/latest** to
**$HOME/src/ljt.nightly/YYYYMMDD**.

Once a full build is completed on one platform, then you can use the existing
source tarball to build binaries on other platforms by running `buildljt -e`
(assuming that **$HOME/src** is a shared directory.)

NOTE: On Windows, `buildljt` should be run from inside an MSYS shell.  If you
are mounting your home directory as a drive letter (e.g. **H:**), then set the
`HOME` environment variable to the MinGW path for that drive (e.g. **/h**)
prior to running `buildljt`.

Run `buildljt -h` for usage information.


Signing the Linux Packages
--------------------------

To sign the RPMs and DEBs using a GPG key, create a file called **gpgsign** in
the same directory as **buildljt**, and include the following contents in the
file:

    GPG_KEY_NAME={full name of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_ID={key ID of GPG key to use (as listed in 'gpg --list-keys')}
    GPG_KEY_PASS={password for GPG key}

[debsigs](https://gitlab.com/debsigs/debsigs/tags) must be installed and in the
`PATH`.

Signing the Mac Package/DMG
---------------------------

To sign the Mac installer package and DMG, create a file called **macsign**
under **setupscripts/**, and include the following contents in the file:

    OSX_APP_CERT_NAME={full name of Mac Developer ID Application certficate (in the macOS keychain) used to sign the DMG}
    OSX_INST_CERT_NAME={full name of Mac Developer ID Installer certificate (in the macOS keychain) used to sign the installer package}

macOS 10.11 "El Capitan" or later is required in order to sign the Mac
package/DMG.

Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called **mssign** in the same directory as **buildljt**, and include the
following contents in the file:

    MS_KEY_FILE={full MinGW path to a .pfx file containing the code signing certificate}
    MS_KEY_PASS={password for certificate}

**signtool** (available in the Windows SDK) must be in the `PATH`.
