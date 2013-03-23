DRC's libjpeg-turbo Build Scripts
=================================

These scripts are used to build the "official" libjpeg-turbo binaries, which
work on any Linux platform with GLIB 2.3.2 and later, as well as Windows XP and
later and OS X 10.4 and later.

See BUILDING.txt in the libjpeg-turbo source for basic build requirements.
Additional build requirements for these scripts are listed below.


Build Environment: Linux
------------------------

Recommended distro:  Red Hat or CentOS Enterprise Linux 4 64-bit

All software necessary to build a 32-bit and a 64-bit version of libjpeg-turbo

NOTE:  If building on a distro that already has GCC 4, you should edit
buildljt.linux and remove the lines that say "CC=gcc4" and "CXX=g++4".
Or, you could just create sym links from gcc4 -> gcc and g++4 -> g++.


Build Environment: OS X
-----------------------

Recommended version:  OS X 10.6 (Snow Leopard)

Xcode 3.2.6 and iOS SDK 4.3 for Snow Leopard (available at
https://developer.apple.com/downloads -- Apple ID required.)  The build scripts
need this in order to produce libjpeg-turbo binaries that are backward
compatible with OS X 10.4/10.5 and the iPhone 3G.  The Xcode tools should be
installed under /Developer, and the "Mac OS X 10.4 SDK" and "iOS SDK" options
should be installed.

Xcode 4.5.x (available at https://developer.apple.com/downloads --
Apple ID required.)  The build scripts need this in order to produce
libjpeg-turbo binaries that are compatible with the iPhone 5 and iPad 4.
Xcode should be installed under /Applications/Xcode.app.  NOTE:  Although
Xcode.app can't be run on Snow Leopard, the build scripts can still use the
SDKs contained within it.

NOTE: It is possible to use OS X 10.7 and later as a build platform, but
installing Xcode 3.2.6 on OS X 10.7 and later is a bit tricky.  You must invoke
the Xcode installer from the command line as follows:

  export COMMAND_LINE_INSTALL=1
  open "/Volumes/Xcode and iOS SDK/Xcode and iOS SDK.mpkg"

Do not install the "System Tools" option, because installing the Xcode 3.2.6
system tools on OS X 10.7 and later will render the system unbootable,
requiring you to boot into Safe Mode to fix the issue.  It is also recommended
that you uncheck "Unix Development", because this option copies the
command-line tools under /Developer/usr into /usr and may interfere with the
command-line tools from later versions of Xcode.  Also, not all of the
Xcode 3.2.6 command-line tools are compatible with OS X 10.7 and later, so
it's just cleaner to access them from /Developer/usr when needed.


Build Environment: Windows
--------------------------

Recommended O/S:  Windows XP 64-bit

CMake should be installed somewhere in the PATH.

All software necessary to build a 32-bit and a 64-bit version of libjpeg-turbo


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


Distributing a Pre-Release Build
--------------------------------

Executing

  uploadljt <SourceForge user name>

will upload the files from $HOME/src/ljt.nightly/latest/files to
http://libjpeg-turbo.sourceforge.net/ljt.nightly

Having an SSH key installed is highly recommended to avoid having to enter your
password multiple times.
