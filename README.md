DRC's libjpeg-turbo Build Scripts
=================================

These scripts are used to build the "official" libjpeg-turbo binaries, which
work on any Linux platform with GLIBC 2.17 and later, as well as Windows XP and
later and OS X/macOS 10.7 and later.

See **BUILDING.md** in the libjpeg-turbo source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 7 x86-64

Complete Linux build environment requirements are best understood by examining
the official Docker recipe at <https://github.com/libjpeg-turbo/docker>.


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


Build Environment: Windows (not Cygwin)
---------------------------------------

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
libjpeg-turbo build instructions.

The official libjpeg-turbo binaries for Visual C++ are generated using
[Visual Studio 2022 Community Edition](https://visualstudio.microsoft.com), but
any reasonably modern version of Visual C++ and the Windows SDK should work.
Note, however, that **jpegXX.dll** will depend on the C run-time DLLs from
whichever version of Visual C++ was used to build it.

The Windows native builds (both Visual C++ and MinGW) must be conducted from
within an MSYS shell.  The easiest way to do that is to install
[MSYS2](http://www.msys2.org) and then use its built-in package manager
(pacman) to install the `make` package.

In order for the libjpeg-turbo static libraries to be compatible with a wide
variety of MinGW distributions, it is necessary to build libjpeg-turbo using
specific versions of the MinGW toolchain.

Download
[**x86\_64-6.4.0-release-posix-seh-rt\_v5-rev0.7z**](https://sourceforge.net/projects/mingw-w64/files/Toolchains targetting Win64/Personal Builds/mingw-builds/6.4.0/threads-posix/seh/x86_64-6.4.0-release-posix-seh-rt_v5-rev0.7z)
and extract it to
**C:\Program Files\mingw-w64\x86\_64-6.4.0-posix-seh-rt\_v5-rev0**.

Download
[**i686-6.4.0-release-posix-dwarf-rt\_v5-rev0.7z**](https://sourceforge.net/projects/mingw-w64/files/Toolchains targetting Win32/Personal Builds/mingw-builds/6.4.0/threads-posix/dwarf/i686-6.4.0-release-posix-dwarf-rt_v5-rev0.7z)
and extract it to
**C:\Program Files (x86)\mingw-w64\i686-6.4.0-posix-dwarf-rt\_v5-rev0**.

x64 and x86 JDKs should be installed, but they do not need to be added to the
`PATH`.  You do, however, need to create the following symbolic links so that
the build scripts can find the JDKs.

From an Administrator Command Prompt, type:

    md %ProgramData%\Oracle\Java
    mklink /d %ProgramData%\Oracle\Java\javapath {directory of x64 JDK}
    mklink /d %ProgramData%\Oracle\Java32 {directory of x86 JDK}

Install all other software necessary to build a 32-bit and a 64-bit version of
libjpeg-turbo (refer to **BUILDING.md**.)


Build Procedure
---------------

Executing

    buildljt [branch/tag]

(where branch/tag is, for instance, "1.4.x" and defaults to "main") will
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

To sign and notarize the Mac installer package and DMG, create a file called
**macsign** under **setupscripts/**, and include the following contents in the
file:

    MACOS_APP_CERT_NAME={full name of Mac Developer ID Application certficate (in the macOS keychain) used to sign the DMG}
    MACOS_INST_CERT_NAME={full name of Mac Developer ID Installer certificate (in the macOS keychain) used to sign the installer package}
    MACOS_NOTARIZE_ID={Apple ID (e-mail address) for notarization}
    MACOS_NOTARIZE_TEAM_ID={Team ID for notarization}
    MACOS_NOTARIZE_PASS={app-specific password for notarization}

macOS 10.11 "El Capitan" or later is required in order to sign the Mac
package/DMG.  macOS 12 "Monterey" or later and Xcode 14 or later are required
in order to notarize the Mac package/DMG.

Signing the Windows Packages
----------------------------

To sign the Windows installers using a code signing certificate, create a file
called **mssign** in the same directory as **buildljt**, and include the
following contents in the file:

    MS_KEY_FILE={full MinGW path to a .pfx file containing the code signing certificate}
    MS_KEY_PASS={password for certificate}

**signtool** (available in the Windows SDK) must be in the `PATH`.
