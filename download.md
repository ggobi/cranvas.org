---
layout: page
title: Download
header: Download and install
group: navigation
---
{% include JB/setup %}

* Install Qt
  * [Ubuntu](#install-qt-under-ubuntu)
  * [Mac OS X](#install-qt-under-mac-os-x)
  * [Windows](#install-qtqtbaseqtpaint-under-windows)
* Install the `cranvas` package (using the `devtools` package)
  {% highlight r %}
  ## use devtools to install other dependencies from GitHub
  if (!require('devtools')) install.packages('devtools')  # needs Curl for RCurl
  library(devtools)
  ## On Ubuntu or Mac, it takes a few minutes to install qtbase, but you do not need to reinstall it every time
  ## WINDOWS users, please do NOT run the next line!
  install_github('qtbase', 'ggobi', ref='qt4'); install_github('qtpaint', 'ggobi')

  install.packages(c('scales', 'tourr', 'objectSignals', 'objectProperties', 'plumbr','SearchTrees')) # install scales from CRAN
  pkgs <- list(hadley = c('productplots', 'densityvis'))
  for (repo in names(pkgs)) {
    for (pkg in pkgs[[repo]]) install_github(pkg, repo)
  }
  ## and finally cranvas:
  install_github('cranvas', 'ggobi', args="--no-multiarch")
  {% endhighlight %}





## Install Qt under Ubuntu

(Optional but recommended) Add the CRAN Ubuntu repository to your `/etc/apt/sources.list` file as per the [instructions on CRAN](http://cran.r-project.org/bin/linux/ubuntu/).

Then run

```
sudo apt-get update
sudo apt-get install libqt4-dev qt4-qmake cmake r-base-dev libcurl4-gnutls-dev
```

which will install all required packages, including current R versions.





## Install Qt under Mac OS X

### Using Homebrew + Qt5

First install Homebrew if you have not done so: <http://brew.sh/#install>. We did not test the OS X binary of R provided on CRAN, and you are recommended to remove the existing version of R and install R via homebrew instead (it will be much easier to maintain all your software packages in the future).

Open a terminal and install R, cmake, and Qt5:

```bash
brew tap homebrew/science
brew install r cmake qt5
# make sure qmake is in PATH for the next step
export PATH=$PATH:`brew --prefix qt5`/bin
R
```

Now we have opened an R session, and we can install **qtbase** and **qtpaint** from Github (do not do this in RStudio; just stay in the terminal):

```r
library(devtools)
install_github('ggobi/qtbase')
install_github('ggobi/qtpaint')
```

The hardest part is done, and we can install **cranvas** with its dependencies now.

### Manual installation (Qt 4.8)

1. Make sure that you have a recent version of xcode installed.
2. Install Qt (version 4.8) from [http://qt-project.org/downloads](http://qt-project.org/downloads).
3. Install cmake (version 2.8.1 or later); obtain it from [cmake.org](cmake.org).
4. Check your PATH, ensuring that both are visible.
5. Install the dependent packages by the R code above.
6. If you get errors like "smokedata.cpp:6646: error: ‘QDBusServer’ was not declared in this scope" switch to RStudio and build from there. It works! Using R (Good Sport) and the latest RStudio version 0.97.449.
7. If you get errors like "Library not loaded: libQtCLucene.4.dylib", try to find the directory for libQtCLucene.4.dylib (should be under some Qt folder), add it to your library path.





## Install Qt/qtbase/qtpaint under Windows

### Install Qt

1.  Install R and Rtools. Add the following to your PATH, in which ... should be substituted by the directory that installed R and Rtools.
  ```
  ...\Rtools\bin;
  ...\Rtools\gcc-4.6.3\bin;
  ```
  For 32-bit Windows, add `...\R\bin\i386` to PATH.
  For 64-bit Windows, add `...\R\bin\x64`.

2.	Install [cmake](http://www.cmake.org) 2.8.11 from [http://www.cmake.org/files/v2.8/cmake-2.8.11-win32-x86.exe](http://www.cmake.org/files/v2.8/cmake-2.8.11-win32-x86.exe). Add `...\CMake 2.8\bin` to the PATH.

3.	Download and install Qt.

  * For 32-bit Windows, download the installer from [here](http://download.qt-project.org/official_releases/qt/4.8/4.8.4/qt-win-opensource-4.8.4-mingw.exe), and run the installation. Then add `...\Qt\4.8.4\bin` to your PATH.

  * For 64-bit Windows, the approach is a little complicated.

    1. Download MinGW from [http://sourceforge.net/projects/mingwbuilds](http://sourceforge.net/projects/mingwbuilds/). Install it with settings:
      Version - 4.8.1;
      Architecture - x64;
      Threads - posix;
      Exception - sjlj;
      Build revision - rev5.
      Then add it to PATH. (For example, `...\MinGW\bin`) NOTE: No spaces are allowed in the directory.

    2.	Install Perl and add its bin to the PATH.

    3.	Download Qt from [http://download.qt-project.org/archive/qt/4.8/4.8.4/qt-everywhere-opensource-src-4.8.4.zip](http://download.qt-project.org/archive/qt/4.8/4.8.4/qt-everywhere-opensource-src-4.8.4.zip)	and extract it somewhere, for example, `C:\Qt\4.8.4\`. Add its bin folder to PATH. (`C:\Qt\4.8.4\bin`)

    4.	Open `C:/Qt/4.8.4/mkspecs/win32-g++/qmake.conf` and set both `QMAKE_CFLAGS` and `QMAKE_LFLAGS` to "-m64". Also, add "-F pe-x86-64" to `QMAKE_RC`. (i.e., `QMAKE_RC = $${CROSS_COMPILE}windres -F pe-x86-64`)

    5.	Open `C:/Qt/4.8.4/qmake/Makefile.win32-g++-sh`, add "-m64" to `CFLAGS` (i.e., `CFLAGS =	-c -o$@ -O -m64 \`)	and change `LFLAGS` to "-s -m64". (i.e., `LFLAGS = -s -m64`)

    6.	Run the following two lines (one by one) in the command prompt under the Qt directory (`C:/Qt/4.8.4/`). This Qt compiling step may take 3-4 hours.
    ```
    configure -release -opensource -nomake examples -nomake demos -no-qt3support
    mingw32-make
    ```

    7.	If you get an error like: `__cpuid(info, 1) was not declared in this scope`,	then open `C:/Qt/4.8.4/src/corelib/tools/qsimd.cpp`, add `#include <intrin.h>` on top.



### Build qtbase

1.	Install Perl and add its bin to the PATH.

2.	Create the `CMAKE` environment variable to point to your `cmake.exe`.
	(for example, `...\CMake 2.8\bin\cmake.exe`)

3.	Create `QMAKE` environment variable to point to `qmake.exe`.
	(for example, `...\Qt\4.8.4\bin\qmake.exe`)

4.	Create `RC_COMPILER` environment variable to point to `windres.exe` from Rtools. Make sure to change the backslash(`\`) to slash(`/`) here.
	(for example, `.../Rtools/gcc-4.6.3/bin/windres.exe`)

5.	Create `QTBASE_QT_PATH` to the directory containing `qmake.exe`.
	(for example, `...\Qt\4.8.4\bin`)

6.	Make sure perl, cmake, Qt, R(32 or 64 bit), Rtools, Rtools-gcc(only for Win 64) are on your PATH.
	Example PATH to build qtbase (better to put these before others):
  ```
  C:\MinGW\bin; (only for Windows 64 bit)
  C:\Program Files\Rtools\bin;
  C:\Program Files\Rtools\gcc-4.6.3\bin;
  C:\Program Files\R\bin\i386; (only for Windows 32 bit)
  C:\Program Files\R\bin\x64; (only for Windows 64 bit)
  C:\Qt\4.8.4\bin;
  C:\Perl\bin\;
  C:\Program Files\CMake 2.8\bin;
  [... other directories ...]
  ```

7.	Download [qtbase](https://github.com/ggobi/qtbase/archive/master.zip) and unzip it. It does not matter where you unzip it. Change the folder name from "qtbase-master" to "qtbase".

8.	(This should be fixed in code) Check `.../qtbase/src/mkdef.sh`.
	Change `sed -n $1 tmp >> qtbase.def` to

	  - Windows 32: `sed -n 's/^.* [BCDRT] _/ /p' tmp >> qtbase.def`
    - Windows 64: `sed -n 's/^.* [BCDRT] / /p' tmp >> qtbase.def`

9.	Make a copy of the whole qtbase folder. Put it in another directory, or just rename it.
	Don't delete this clean copy until you successfully install qtbase after step 10.

10.	Start the command shell, go to the directory containing qtbase (i.e., the parent directory of qtbase). Run:
  ```
  R CMD INSTALL --build qtbase
  ```
	Please make sure the folder qtbase is completely clean (i.e., not pre-installed).
  If not, get the clean copy from step 9.



### Build qtpaint

1.	Add the `...\qtbase\local\lib` to PATH.
	where ... is replaced by your R library path. Run `?.libPaths` in R to find out it.

2.	Download glext.h from [http://www.opengl.org/registry/api/glext.h](http://www.opengl.org/registry/api/glext.h).
	And copy it to `...\Qt\4.8.4\src\opengl\`.
	Add a line to `...\Qt\4.8.4\include\QtOpenGL\qgl.h` :
  ```
  #include "../../src/opengl/glext.h"
  ```
	??? An alternative way is editing `OpenGLPainter.cpp`
  ```
  	#ifdef Q_OS_WIN
  	#include "glext.h"
  	#endif
  ```

3.	Download [qtpaint](https://github.com/ggobi/qtpaint)
	Start the command shell, go to the directory containing qtpaint. Run:
  ```
  R CMD INSTALL --build qtpaint
  ```
