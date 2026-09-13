# Compile Converseen on from Source Code

To manually compile **Converseen** on any **GNU/Linux** distribution, you need the **gnu c++ compiler**, **Qt5** or **Qt6 Framework Libraries** and **ImageMagick++** development libraries (preferably ImageMagick version 7 but legacy version 6 is also perfectly compatible).
Download Converseen, extract the archive content, enter directory and build the executable using these commands:

## Install Converseen using Qt5:

> tar -xvf converseen-0.*.tar.bz2
> 
> cd converseen-0.x
> 
> mkdir build
>
> cd build
> 
> cmake .. (to install on /usr/local/)
> 
> *or*
> 
> cmake -DCMAKE_INSTALL_PREFIX=/usr  .. (to install on /usr/)
> 
> make
> 
> make install

## Install Converseen using Qt6:
> tar -xvf converseen-0.*.tar.bz2
> 
> cd converseen-0.x
> 
> mkdir build
>
> cd build
> 
> cmake .. -DUSE_QT6=yes (to install on /usr/local/)
> 
> *or*
> 
> cmake -DCMAKE_INSTALL_PREFIX=/usr -DUSE_QT6=yes .. (to install on /usr/)
> 
> make
> 
> make install

## Build on macOS with JPEG-2000 (JP2) and HEIC/HEIF support

The stock Homebrew `imagemagick` formula is built **without** the OpenJPEG and libheif delegates, so a Converseen build linked against it cannot read/write `.jp2` or `.heic`/`.heif` files even though the code supports any format ImageMagick exposes. Use the `imagemagick-full` formula instead, which includes both delegates:

> brew install qt imagemagick-full
>
> mkdir build && cd build
>
> cmake -DUSE_QT6=YES -DMACOS_DEPLOY=YES \\
>   -DCMAKE_PREFIX_PATH="$(brew --prefix qt);$(brew --prefix imagemagick-full)" ..
>
> cmake --build . -j$(sysctl -n hw.ncpu)

This produces `converseen.app` linked against `imagemagick-full`, which reads and writes JP2, HEIC, HEIF, and AVIF alongside every other ImageMagick-supported format — including batch **JP2 → JPG** and **HEIC → JPG** conversion. No source changes are required; the format list in `formats.cpp` is generated at runtime from whatever ImageMagick build it's linked against.

Because `imagemagick-full` is `keg-only`, running the built app also requires `qt` and `imagemagick-full` to remain installed via Homebrew (the app is not statically bundled with these dependencies).
