phantomjs-linux-armv6l
======================

PhantomJS 1.9, compiled on Raspberry PI (Raspbian "wheezy").

PhantomJS is a headless WebKit with JavaScript API. It has fast and native support for various web standards: DOM handling, CSS selector, JSON, Canvas, and SVG. (http://phantomjs.org).


__Installation on Raspberry PI__

Clone this repository and extract the file <code>phantomjs-x.y.z-linux-armv6l.tar.bz2</code>.

I achieved the best screenshot results with the following font configuration.

__Caution:__ The following steps could mess up the font configuration for other applications!

<pre>
$ cd /usr/share
$ sudo mv fonts fonts.bak
$ sudo mkdir fonts

$ sudo apt-get install --reinstall ttf-mscorefonts-installer

$ sudo rm /usr/share/fonts/truetype/msttcorefonts/andalemo.ttf
$ sudo rm /usr/share/fonts/truetype/msttcorefonts/Andale_Mono.ttf

$ sudo fc-cache -rv
</pre>


__Process used to build PhantomJS__

In this chapter I describe the steps I have executed to build the PhantomJS binary.

__1.__ According to http://phantomjs.org/build.html :

<pre>
$ sudo apt-get update
$ sudo apt-get install build-essential chrpath git-core libssl-dev libfontconfig1-dev
$ git clone git://github.com/ariya/phantomjs.git
$ cd phantomjs
$ git checkout 1.9
</pre>


__2.__ Download additional 3rdparty files:

<pre>
$ mkdir src/qt/src/3rdparty/pixman && pushd src/qt/src/3rdparty/pixman && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/README && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.h && curl -O http://qt.gitorious.org/qt/qt/blobs/raw/4.8/src/3rdparty/pixman/pixman-arm-neon-asm.S; popd
</pre>


__3.__ Open <code>./build.sh</code> and delete lines 11-34:

<pre>
.. ...
11 if [[ "$MAKEFLAGS" != "" ]]; then
12 MAKEFLAGS_JOBS=$(echo $MAKEFLAGS | egrep -o '\-j[0-9]+' | egrep -o '[0-9]+')
.. ...
34 fi
.. ...
</pre>


__4.__ Open <code>./src/qt/preconfig.sh</code> and add the option <code>' -no-pch'</code> to <code>QT_CFG</code> after <code>' -no-stl'</code>:

<pre>
.. ...
29 QT_CFG+=' -no-exceptions'       # Don't use C++ exception
30 QT_CFG+=' -no-stl'              # No need for STL compatibility
31 QT_CFG+=' -no-pch'
.. ...
</pre>


__5.__ Start compilation:

<pre>
$ nohup ./deploy/build-and-package.sh > build-and-package.sh.out 2> build-and-package.sh.err &
</pre>


__6.__ Create tarball according to http://phantomjs.org/build.html :

__TODO:__ _Is this step still necessary when building with build-and-package.sh ?_

<pre>
./deploy/package.sh
</pre>


__7.__ The tarball can be found in the <code>./deploy</code> directory.

