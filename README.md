# Media Player Classic - Qute Theater

## A clone of Media Player Classic reimplemented in Qt.

![Screenshot][1]

[Media Player Classic - Home Cinema][2] (mpc-hc) is considered by many to
be the quintessential media player for the Windows desktop. Media Player
Classic - Qute Theater (mpc-qt) aims to reproduce most of the interface and
functionality of mpc-hc while using [libmpv][3] to play video instead of
DirectShow.

## Releases

There is currently no official build/release. Despite this, you may [build it
from source][4] and determine if the experience from the current codebase is
satisfactory to you. If not, please open an issue that may direct the
developers in a helpful direction.

Compiling from source offers several advantages over pre-built releases, such
as the ability to use the most current codebase instead of a snapshot of some
earlier state. There could also be unofficial packages in some Linux
distribution archives (e.g. [Arch User Repository][5], [FreshPorts][6])
available for installation.

## Features

Nearly everything that mpc-hc does.  For the most part, unwritten
portions relate to setting options and streaming from devices.

### Improvements over MPC-HC

**Multiple playlists:**  When you're watching shows on your backlog, load
every show into separate playlists and still keep track of the last played
file _for each playlist_.  Finally you can eliminate the need to keep track of
your progress in a spreadsheet, all while never leaving the comfort of your
favorite media player.

**Quick queuing:**  Out-of-order playback in the same style of xmms/qmmp.
Got some compilation albums in a playlist, but want to hear only some rock
tracks for a while?  Now you can, without obliterating your playlist.

**Playlist searching:**  Multi-threaded playlist searching, in the same style
as other media players.  Find the tracks you want, when you want them.

**Screenshot templates:**  Take screenshots with a custom, sleek and stylized
filename.  Only include the information that you want.

**Looped playback:** Selectively show part of video/music tracks.  Amazing,
isn't it?

**Custom metadata:**  Display custom metadata in the playlist window.  Want to
show the artist as well as the title, down to even the encoder used?  Nothing
is stopping you.

### Upcoming features

**Native filter-chain support:**  Comprehensive integration of mpv/ffmpeg's
filter interface/library, right inside your media player.

**Encoding support like VirtualDub:**  Churn out memes faster.  No need to
open a video editor when your media player can do your job for you.

**Race Inspired Cosmetic Enhancements:**  In-app custom styling support.

[Suggestions welcome][7].

## Compiling

### Prerequisites

You need git, make, qmake, the current Qt 5 Software Development Kit, as well as
a recent version of libarchive and version 0.29.0 or newer of libmpv with the
accompanying development headers for both libraries installed on your system before
you can attempt a build.  On Debian-based Linux distributions (xUbuntu, Linux Mint,
et al.) you can usually install all of these with this one shell command:

```bash
sudo apt -y install git libarchive-dev libmpv-dev libqt5x11extras5-dev make qmake qt5-default qtcreator qttools5-dev-tools
```

The major options of note available in the Makefile are `--enable-libmpv-shared` for
shared library support (you want to use this unless you also built libmpv from source),
and `--enable-libarchive` if you want to use mpc-qt as a comic book viewer.

### I don't know sh..., I mean git

First ensure you have the [prerequisites mentioned above][8], then open a terminal
emulator and start by downloading a copy of this repository to your computer. This
command will do that and make the new folder containing the code your working
directory:

```bash
git clone https://github.com/RogueScholar/mpc-qt.git && cd mpc-qt
```

Then build with qmake and make.

```bash
qmake -makefile && make -j$(nproc)
```

If your terminal emulator returns to a command prompt without reporting any errors,
you can install your spiffy new build with this command:

```bash
sudo make install
```

You're done! Take note of the folder displayed in your shell prompt though, because
in the future, should you wish to compile another build with any improvements to the
codebase since this one, you can return to it and update your copy of the source code
with one simple command:

```bash
make clean && git pull origin master
```

The re-build process is done with the same qmake and make install commands as before.

### I have compiler/linker errors

First, check the version of libmpv on your system; some package repositories
(_**cough**_ Debian _**cough**_) have an ancient version of mpv. On Debian-based
Linux distributions this command will show you what you have and any other versions
that are available:

```bash
apt-cache -a show libmpv-dev
```
If you see a version older than 0.29.0 listed as installed, you can either find
another package compatible with your distribution of at least that version, or you
can uninstall your current package and [build libmpv][9] yourself with these commands:

```bash
sudo apt purge libmpv libmpv-dev
git clone https://github.com/mpv-player/mpv-build.git && cd mpv-build
./use-ffmpeg-master
./use-libass-master
./use-mpv-master
sudo apt -y install devscripts equivs
rm -f mpv-build-deps\_\*\_\*.deb
mk-build-deps -s sudo -i
echo --enable-libmpv-shared > mpv_options
./update
./build -j$(nproc)
sudo ./install
sudo ldconfig
```

If you encountered no errors, the latest version of libmpv should now be installed
to `/usr/local`.

### Microsoft Windows build systems

While this program is meant for Linux systems, it is possible to compile it on
Microsoft Windows computers with the [MSYS2 edition of Qt Creator][10] due to the
cross-platform nature of the Qt 5 toolkit. MSVC is not supported, though. Also,
the build process needs librsvg, ImageMagick and Inkscape to create the Windows
.ico file for the desktop/Start menu shortcuts to display. Use `pacman -Ss <package name>`
to find them.

mpc-qt can be compiled with a libmpv linked to MSYS2's ffmpeg libraries, or by
using the [prebuilt library from srsfckn.biz][11]. To use the prebuilt library,
after cloning this repository download libmpv from the [mpv windows release page][11],
and extract it somewhere. Place the libraries for your architechture from mpv-dev.7z
(e.g. `mpv-dev.7z/64`) into `mpv-dev/lib`. Then place the include files from mpv-dev.7z
(usually at `mpv-dev.zip/include`) into `mpv-dev/include/mpv`. Compile with the 64-bit
Qt 5 framework as usual.

Bleeding-edge git master builds that use new features not yet in a release can usually
be made with [Shinchiro builds of libmpv][12]. Unpack in the same manner as above.

[1]: https://user-images.githubusercontent.com/15098724/60409942-9040ec00-9b7a-11e9-9d0b-35f2b2579333.png
[2]: https://mpc-hc.org/
[3]: https://github.com/mpv-player/mpv
[4]: README.md#Compiling
[5]: https://aur.archlinux.org/packages/mpc-qt-git
[6]: https://www.freshports.org/multimedia/mpc-qt
[7]: https://github.com/RogueScholar/mpc-qt/issues/new
[8]: README.md#Prerequisites
[9]: https://github.com/mpv-player/mpv-build
[10]: https://wiki.qt.io/MSYS2
[11]: https://mpv.srsfckn.biz/
[12]: https://sourceforge.net/projects/mpv-player-windows/files/libmpv/
