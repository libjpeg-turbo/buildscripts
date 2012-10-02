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

Xcode 3.2.x (available at https://developer.apple.com/downloads -- Apple ID
required)


Build Environment: Windows
--------------------------

Recommended O/S:  Windows XP 64-bit

CMake should be installed somewhere in the PATH.

All software necessary to build a 32-bit and a 64-bit version of libjpeg-turbo


Build Procedure
---------------

Executing

  buildljt <repository path>

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
