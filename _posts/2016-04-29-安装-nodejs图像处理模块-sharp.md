---
layout: post
title: 安装 nodejs图像处理模块 sharp
---

```
sudo npm install sharp
```

报错：

```
ERROR: Please install libvips by running: brew install homebrew/science/vips --with-webp --with-graphicsmagick
```

 根据提示安装libvips

```
$ brew install homebrew/science/vips --with-webp --with-graphicsmagick
```

 安装过程如下：

```
192:~ simple$ brew install homebrew/science/vips --with-webp --with-graphicsmagick
==> Installing vips from homebrew/science
==> Installing dependencies for homebrew/science/vips: jpeg, icu4c, harfbuzz, pygobject3, sqlite, graphicsmagick
==> Installing homebrew/science/vips dependency: jpeg
==> Downloading https://homebrew.bintray.com/bottles/jpeg-8d.el_capitan.bottle.2.tar.gz
Already downloaded: /Library/Caches/Homebrew/jpeg-8d.el_capitan.bottle.2.tar.gz
==> Pouring jpeg-8d.el_capitan.bottle.2.tar.gz
Error: The `brew link` step did not complete successfully
The formula built, but is not symlinked into /usr/local
Could not symlink share/man/man1/jpegtran.1
Target /usr/local/share/man/man1/jpegtran.1
already exists. You may want to remove it:
  rm '/usr/local/share/man/man1/jpegtran.1'
 
To force the link and overwrite all conflicting files:
  brew link --overwrite jpeg
 
To list all files that would be deleted:
  brew link --overwrite --dry-run jpeg
 
Possible conflicting files are:
/usr/local/share/man/man1/jpegtran.1
/usr/local/share/man/man1/rdjpgcom.1
/usr/local/share/man/man1/wrjpgcom.1
==> Summary
🍺  /usr/local/Cellar/jpeg/8d: 19 files, 713.7K
==> Installing homebrew/science/vips dependency: icu4c
==> Downloading https://homebrew.bintray.com/bottles/icu4c-57.1.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring icu4c-57.1.el_capitan.bottle.tar.gz
==> Caveats
This formula is keg-only, which means it was not symlinked into /usr/local.
 
OS X provides libicucore.dylib (but nothing else).
 
Generally there are no consequences of this for you. If you build your
own software and it requires this formula, you'll need to add to your
build variables:
 
    LDFLAGS:  -L/usr/local/opt/icu4c/lib
    CPPFLAGS: -I/usr/local/opt/icu4c/include
 
==> Summary
🍺  /usr/local/Cellar/icu4c/57.1: 265 files, 65.0M
==> Installing homebrew/science/vips dependency: harfbuzz
==> Downloading https://homebrew.bintray.com/bottles/harfbuzz-1.2.6.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring harfbuzz-1.2.6.el_capitan.bottle.tar.gz
🍺  /usr/local/Cellar/harfbuzz/1.2.6: 123 files, 4.6M
==> Installing homebrew/science/vips dependency: pygobject3
==> Downloading https://homebrew.bintray.com/bottles/pygobject3-3.20.1.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring pygobject3-3.20.1.el_capitan.bottle.tar.gz
🍺  /usr/local/Cellar/pygobject3/3.20.1: 61 files, 2.1M
==> Installing homebrew/science/vips dependency: sqlite
==> Downloading https://homebrew.bintray.com/bottles/sqlite-3.12.2.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring sqlite-3.12.2.el_capitan.bottle.tar.gz
==> Caveats
This formula is keg-only, which means it was not symlinked into /usr/local.
 
OS X provides an older sqlite3.
 
Generally there are no consequences of this for you. If you build your
own software and it requires this formula, you'll need to add to your
build variables:
 
    LDFLAGS:  -L/usr/local/opt/sqlite/lib
    CPPFLAGS: -I/usr/local/opt/sqlite/include
 
==> Summary
🍺  /usr/local/Cellar/sqlite/3.12.2: 10 files, 2.8M
==> Installing homebrew/science/vips dependency: graphicsmagick
==> Downloading https://homebrew.bintray.com/bottles/graphicsmagick-1.3.23_1.el_capitan.bottle.tar.gz
######################################################################## 100.0%
==> Pouring graphicsmagick-1.3.23_1.el_capitan.bottle.tar.gz
🍺  /usr/local/Cellar/graphicsmagick/1.3.23_1: 474 files, 11.9M
==> Installing homebrew/science/vips
==> Downloading http://www.vips.ecs.soton.ac.uk/supported/8.3/vips-8.3.0.tar.gz
Already downloaded: /Library/Caches/Homebrew/vips-8.3.0.tar.gz
==> Downloading https://gist.githubusercontent.com/felixbuenemann/6862526323514cb7684b81cb88593d0d/raw/5d3d258f4c8c316f7c897eb5b91da771704
Already downloaded: /Library/Caches/Homebrew/vips--patch-8a7a43e9faebb38ecc8cfe4f8f1fc20ca53bc758f289b62693700818a5eb1b34.diff
==> Patching
==> Applying vips-8.3.0-graphicsmagick-fix.diff
patching file configure
patching file libvips/foreign/magick2vips.c
==> ./configure --prefix=/usr/local/Cellar/vips/8.3.0 --with-magick --with-magickpackage=GraphicsMagick
==> make install
Last 15 lines from /Users/simple/Library/Logs/Homebrew/vips/02.make:
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: .libs/libvips.a(openslide2vips.o) has no symbols
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: .libs/libvips.a(openslideload.o) has no symbols
/Applications/Xcode.app/Contents/Developer/Toolchains/XcodeDefault.xctoolchain/usr/bin/ranlib: file: .libs/libvips.a(vips2jpeg.o) has no symbols
libtool: link: rm -fr .libs/libvips.lax .libs/libvips.lax
libtool: link: ( cd ".libs" && rm -f "libvips.la" && ln -s "../libvips.la" "libvips.la" )
/bin/sh ../libtool  --tag=CC   --mode=link clang  -g -O2 -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/gobject-introspection/1.46.0_1/lib -lgirepository-1.0 -lgobject-2.0 -lglib-2.0 -lintl   -o introspect introspect.o -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/gobject-introspection/1.46.0_1/lib -lgirepository-1.0 -lgobject-2.0 -lglib-2.0 -lintl -DG_DISABLE_ASSERT -DG_DISABLE_CHECKS -I/usr/local/opt/gettext/include -I/usr/local/Cellar/webp/0.5.0/include -I/usr/local/Cellar/poppler/0.42.0/include/poppler/glib -I/usr/local/Cellar/poppler/0.42.0/include/poppler -I/usr/local/Cellar/pixman/0.34.0/include/pixman-1 -I/usr/local/Cellar/pango/1.38.1/include/pango-1.0 -I/usr/local/Cellar/orc/0.4.25/include/orc-0.4 -I/usr/local/Cellar/little-cms2/2.7/include -I/usr/local/Cellar/libtiff/4.0.6/include -I/usr/local/Cellar/librsvg/2.40.13/include/librsvg-2.0 -I/usr/local/Cellar/libpng/1.6.21/include/libpng16 -I/usr/local/Cellar/libgsf/1.14.36/include/libgsf-1 -I/usr/local/Cellar/libexif/0.6.21/include -I/usr/local/Cellar/harfbuzz/1.2.6/include/harfbuzz -I/usr/local/Cellar/graphicsmagick/1.3.23_1/include/GraphicsMagick -I/usr/local/Cellar/glib/2.46.2/lib/glib-2.0/include -I/usr/local/Cellar/glib/2.46.2/include/glib-2.0 -I/usr/local/Cellar/gdk-pixbuf/2.32.3/include/gdk-pixbuf-2.0 -I/usr/local/Cellar/freetype/2.6.3/include/freetype2 -I/usr/local/Cellar/fontconfig/2.11.1_2/include -I/usr/local/Cellar/fftw/3.3.4_1/include -I/usr/local/Cellar/cairo/1.14.6_1/include/cairo -I/usr/include/libxml2 -D_REENTRANT libvips.la -L/usr/local/Cellar/graphicsmagick/1.3.23_1/lib -lGraphicsMagick -L/usr/local/Cellar/libpng/1.6.21/lib -lpng16 -L/usr/local/Cellar/libtiff/4.0.6/lib -ltiff -lz   -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -lgmodule-2.0 -lxml2 -lgobject-2.0 -lglib-2.0 -lintl -L/usr/local/Cellar/freetype/2.6.3/lib -L/usr/local/Cellar/fontconfig/2.11.1_2/lib -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/pango/1.38.1/lib -lpangoft2-1.0 -lpango-1.0 -lgobject-2.0 -lglib-2.0 -lintl -lfontconfig -lfreetype -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/libgsf/1.14.36/lib -lgsf-1 -lgobject-2.0 -lglib-2.0 -lintl -lxml2 -L/usr/local/Cellar/fftw/3.3.4_1/lib -lfftw3 -L/usr/local/Cellar/orc/0.4.25/lib -lorc-0.4 -L/usr/local/Cellar/little-cms2/2.7/lib -llcms2 -lgif -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/cairo/1.14.6_1/lib -L/usr/local/Cellar/gdk-pixbuf/2.32.3/lib -L/usr/local/Cellar/librsvg/2.40.13/lib -lrsvg-2 -lm -lgio-2.0 -lgdk_pixbuf-2.0 -lgobject-2.0 -lglib-2.0 -lintl -lcairo -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/cairo/1.14.6_1/lib -L/usr/local/Cellar/poppler/0.42.0/lib -lpoppler-glib -lgio-2.0 -lgobject-2.0 -lglib-2.0 -lintl -lcairo    -L/usr/local/Cellar/webp/0.5.0/lib -lwebp  -L/usr/local/Cellar/libexif/0.6.21/lib -lexif -lm 
libtool: link: clang -g -O2 -o .libs/introspect introspect.o -DG_DISABLE_ASSERT -DG_DISABLE_CHECKS -I/usr/local/opt/gettext/include -I/usr/local/Cellar/webp/0.5.0/include -I/usr/local/Cellar/poppler/0.42.0/include/poppler/glib -I/usr/local/Cellar/poppler/0.42.0/include/poppler -I/usr/local/Cellar/pixman/0.34.0/include/pixman-1 -I/usr/local/Cellar/pango/1.38.1/include/pango-1.0 -I/usr/local/Cellar/orc/0.4.25/include/orc-0.4 -I/usr/local/Cellar/little-cms2/2.7/include -I/usr/local/Cellar/libtiff/4.0.6/include -I/usr/local/Cellar/librsvg/2.40.13/include/librsvg-2.0 -I/usr/local/Cellar/libpng/1.6.21/include/libpng16 -I/usr/local/Cellar/libgsf/1.14.36/include/libgsf-1 -I/usr/local/Cellar/libexif/0.6.21/include -I/usr/local/Cellar/harfbuzz/1.2.6/include/harfbuzz -I/usr/local/Cellar/graphicsmagick/1.3.23_1/include/GraphicsMagick -I/usr/local/Cellar/glib/2.46.2/lib/glib-2.0/include -I/usr/local/Cellar/glib/2.46.2/include/glib-2.0 -I/usr/local/Cellar/gdk-pixbuf/2.32.3/include/gdk-pixbuf-2.0 -I/usr/local/Cellar/freetype/2.6.3/include/freetype2 -I/usr/local/Cellar/fontconfig/2.11.1_2/include -I/usr/local/Cellar/fftw/3.3.4_1/include -I/usr/local/Cellar/cairo/1.14.6_1/include/cairo -I/usr/include/libxml2 -D_REENTRANT  -L/usr/local/Cellar/glib/2.46.2/lib -L/usr/local/opt/gettext/lib -L/usr/local/Cellar/gobject-introspection/1.46.0_1/lib -lgirepository-1.0 ./.libs/libvips.dylib -L/usr/local/Cellar/graphicsmagick/1.3.23_1/lib -L/usr/local/Cellar/freetype/2.6_1/lib -L/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX10.11.sdk/usr/lib -L/usr/local/Cellar/libpng/1.6.21/lib -L/usr/local/Cellar/libtiff/4.0.6/lib -L/usr/local/Cellar/freetype/2.6.3/lib -L/usr/local/Cellar/fontconfig/2.11.1_2/lib -L/usr/local/Cellar/pango/1.38.1/lib -L/usr/local/Cellar/libgsf/1.14.36/lib -L/usr/local/Cellar/fftw/3.3.4_1/lib -L/usr/local/Cellar/orc/0.4.25/lib -L/usr/local/Cellar/little-cms2/2.7/lib -L/usr/local/Cellar/cairo/1.14.6_1/lib -L/usr/local/Cellar/gdk-pixbuf/2.32.3/lib -L/usr/local/Cellar/librsvg/2.40.13/lib -L/usr/local/Cellar/poppler/0.42.0/lib -L/usr/local/Cellar/webp/0.5.0/lib -L/usr/local/Cellar/libexif/0.6.21/lib /usr/local/Cellar/graphicsmagick/1.3.23_1/lib/libGraphicsMagick.dylib -lbz2 -lltdl -lpthread -lpng16 -ltiff -lz -lgmodule-2.0 -lpangoft2-1.0 -lpango-1.0 -lfontconfig -lfreetype -lgsf-1 -lxml2 -lfftw3 -lorc-0.4 -llcms2 -lgif -lrsvg-2 -lgdk_pixbuf-2.0 -lpoppler-glib -lgio-2.0 -lgobject-2.0 -lglib-2.0 -lintl -lcairo -lwebp -lexif -lm
CPPFLAGS="" CFLAGS="-g -O2" LDFLAGS="" CC="clang"  /usr/local/Cellar/gobject-introspection/1.46.0_1/bin/g-ir-scanner  --add-include-path=. --namespace=Vips --nsversion=8.0 --libtool="/bin/sh ../libtool"  --include=GObject-2.0   --library=libvips.la --program=./introspect --identifier-prefix=Vips --identifier-prefix=vips --symbol-prefix=vips  --cflags-begin  -I../libvips/include --cflags-end  arithmetic/abs.c arithmetic/add.c arithmetic/arithmetic.c arithmetic/avg.c arithmetic/binary.c arithmetic/boolean.c arithmetic/complex.c arithmetic/deviate.c arithmetic/divide.c arithmetic/getpoint.c arithmetic/hist_find.c arithmetic/hist_find_indexed.c arithmetic/hist_find_ndim.c arithmetic/hough.c arithmetic/hough_circle.c arithmetic/hough_line.c arithmetic/invert.c arithmetic/linear.c arithmetic/math.c arithmetic/math2.c arithmetic/max.c arithmetic/measure.c arithmetic/min.c arithmetic/multiply.c arithmetic/nary.c arithmetic/profile.c arithmetic/project.c arithmetic/relational.c arithmetic/remainder.c arithmetic/round.c arithmetic/sign.c arithmetic/statistic.c arithmetic/stats.c arithmetic/subtract.c arithmetic/sum.c arithmetic/unary.c arithmetic/unaryconst.c colour/colour.c colour/colourspace.c colour/dE00.c colour/dE76.c colour/dECMC.c colour/float2rad.c colour/HSV2sRGB.c colour/icc_transform.c colour/Lab2LabQ.c colour/Lab2LabS.c colour/Lab2LCh.c colour/Lab2XYZ.c colour/LabQ2Lab.c colour/LabQ2LabS.c colour/LabQ2sRGB.c colour/LabS2Lab.c colour/LabS2LabQ.c colour/LCh2Lab.c colour/LCh2UCS.c colour/rad2float.c colour/scRGB2BW.c colour/scRGB2sRGB.c colour/scRGB2XYZ.c colour/sRGB2HSV.c colour/sRGB2scRGB.c colour/UCS2LCh.c colour/XYZ2Lab.c colour/XYZ2scRGB.c colour/XYZ2Yxy.c colour/Yxy2XYZ.c conversion/arrayjoin.c conversion/autorot.c conversion/bandary.c conversion/bandbool.c conversion/bandfold.c conversion/bandjoin.c conversion/bandmean.c conversion/bandrank.c conversion/bandunfold.c conversion/byteswap.c conversion/cache.c conversion/cast.c conversion/conversion.c conversion/copy.c conversion/embed.c conversion/extract.c conversion/falsecolour.c conversion/flatten.c conversion/flip.c conversion/gamma.c conversion/grid.c conversion/ifthenelse.c conversion/insert.c conversion/join.c conversion/msb.c conversion/premultiply.c conversion/recomb.c conversion/replicate.c conversion/rot.c conversion/rot45.c conversion/scale.c conversion/sequential.c conversion/subsample.c conversion/tilecache.c conversion/unpremultiply.c conversion/wrap.c conversion/zoom.c convolution/compass.c convolution/conv.c convolution/convolution.c convolution/convsep.c convolution/correlation.c convolution/fastcor.c convolution/gaussblur.c convolution/im_aconv.c convolution/im_aconvsep.c convolution/im_conv.c convolution/im_conv_f.c convolution/sharpen.c convolution/spcor.c create/black.c create/buildlut.c create/create.c create/eye.c create/fractsurf.c create/gaussmat.c create/gaussnoise.c create/grey.c create/identity.c create/invertlut.c create/logmat.c create/mask.c create/mask_butterworth.c create/mask_butterworth_band.c create/mask_butterworth_ring.c create/mask_fractal.c create/mask_gaussian.c create/mask_gaussian_band.c create/mask_gaussian_ring.c create/mask_ideal.c create/mask_ideal_band.c create/mask_ideal_ring.c create/point.c create/sines.c create/text.c create/tonelut.c create/xyz.c create/zone.c draw/draw.c draw/draw_circle.c draw/draw_flood.c draw/draw_image.c draw/draw_line.c draw/draw_mask.c draw/draw_rect.c draw/draw_smudge.c draw/drawink.c foreign/analyze2vips.c foreign/analyzeload.c foreign/csv.c foreign/csvload.c foreign/csvsave.c foreign/dzsave.c foreign/fits.c foreign/fitsload.c foreign/fitssave.c foreign/foreign.c foreign/gifload.c foreign/jpeg2vips.c foreign/jpegload.c foreign/jpegsave.c foreign/magick2vips.c foreign/magickload.c foreign/matlab.c foreign/matload.c foreign/matrixload.c foreign/matrixsave.c foreign/openexr2vips.c foreign/openexrload.c foreign/openslide2vips.c foreign/openslideload.c foreign/pdfload.c foreign/pngload.c foreign/pngsave.c foreign/ppm.c foreign/ppmload.c foreign/ppmsave.c foreign/radiance.c foreign/radload.c foreign/radsave.c foreign/rawload.c foreign/rawsave.c foreign/svgload.c foreign/tiff2vips.c foreign/tiffload.c foreign/tiffsave.c foreign/vips2jpeg.c foreign/vips2tiff.c foreign/vips2webp.c foreign/vipsload.c foreign/vipspng.c foreign/vipssave.c foreign/webp2vips.c foreign/webpload.c foreign/webpsave.c freqfilt/freqfilt.c freqfilt/freqmult.c freqfilt/fwfft.c freqfilt/invfft.c freqfilt/phasecor.c freqfilt/spectrum.c histogram/hist_cum.c histogram/hist_entropy.c histogram/hist_equal.c histogram/hist_ismonotonic.c histogram/hist_local.c histogram/hist_match.c histogram/hist_norm.c histogram/hist_plot.c histogram/hist_unary.c histogram/histogram.c histogram/maplut.c histogram/percent.c histogram/stdif.c introspect.c iofuncs/base64.c iofuncs/buf.c iofuncs/buffer.c iofuncs/cache.c iofuncs/enumtypes.c iofuncs/error.c iofuncs/gate.c iofuncs/generate.c iofuncs/header.c iofuncs/image.c iofuncs/init.c iofuncs/mapfile.c iofuncs/memory.c iofuncs/object.c iofuncs/operation.c iofuncs/rect.c iofuncs/region.c iofuncs/semaphore.c iofuncs/sink.c iofuncs/sinkdisc.c iofuncs/sinkmemory.c iofuncs/sinkscreen.c iofuncs/system.c iofuncs/threadpool.c iofuncs/type.c iofuncs/util.c iofuncs/vector.c iofuncs/vips.c iofuncs/vipsmarshal.c iofuncs/window.c morphology/countlines.c morphology/hitmiss.c morphology/labelregions.c morphology/morph.c morphology/morphology.c morphology/rank.c mosaicing/global_balance.c mosaicing/im_avgdxdy.c mosaicing/im_chkpair.c mosaicing/im_clinear.c mosaicing/im_improve.c mosaicing/im_initialize.c mosaicing/im_lrcalcon.c mosaicing/im_lrmerge.c mosaicing/im_lrmosaic.c mosaicing/im_remosaic.c mosaicing/im_tbcalcon.c mosaicing/im_tbmerge.c mosaicing/im_tbmosaic.c mosaicing/match.c mosaicing/merge.c mosaicing/mosaic.c mosaicing/mosaic1.c mosaicing/mosaicing.c resample/affine.c resample/interpolate.c resample/mapim.c resample/quadratic.c resample/reduce.c resample/resample.c resample/resize.c resample/shrink.c resample/shrinkh.c resample/shrinkv.c resample/similarity.c resample/transform.c video/im_video_test.c video/video_dispatch.c include/vips/basic.h include/vips/vips.h include/vips/object.h include/vips/image.h include/vips/error.h include/vips/foreign.h include/vips/interpolate.h include/vips/header.h include/vips/operation.h include/vips/enumtypes.h include/vips/conversion.h include/vips/arithmetic.h include/vips/colour.h include/vips/convolution.h include/vips/draw.h include/vips/morphology.h include/vips/type.h include/vips/memory.h include/vips/region.h introspect --output Vips-8.0.gir
dyld: Library not loaded: /usr/local/lib/libjpeg.8.dylib
  Referenced from: /usr/local/opt/libtiff/lib/libtiff.5.dylib
  Reason: image not found
Command '['./introspect', '--introspect-dump=/var/folders/p3/1xnx90t12696vd9q8y0y5gyh0000gp/T/tmp-introspectpn4fkb/functions.txt,/var/folders/p3/1xnx90t12696vd9q8y0y5gyh0000gp/T/tmp-introspectpn4fkb/dump.xml']' returned non-zero exit status -5
make[2]: *** [Vips-8.0.gir] Error 1
make[1]: *** [install-recursive] Error 1
make: *** [install-recursive] Error 1
 
READ THIS: https://git.io/brew-troubleshooting
If reporting this issue please do so at (not Homebrew/brew):
  https://github.com/Homebrew/homebrew-science/issues
```

还是有错，重新安装node-gyp

```
sudo npm install -g node-gyp
```

安装成功后：

```
sudo node-gyp rebuild
```

 报错：

```
gyp: binding.gyp not found (cwd: /Users/simple) while trying to load binding.gyp
gyp ERR! configure error
gyp ERR! stack Error: `gyp` failed with exit code: 1
gyp ERR! stack     at ChildProcess.onCpExit (/usr/local/lib/node_modules/node-gyp/lib/configure.js:305:16)
gyp ERR! stack     at emitTwo (events.js:100:13)
gyp ERR! stack     at ChildProcess.emit (events.js:185:7)
gyp ERR! stack     at Process.ChildProcess._handle.onexit (internal/child_process.js:204:12)
gyp ERR! System Darwin 15.4.0
gyp ERR! command "/Users/simple/n/bin/node" "/usr/local/bin/node-gyp" "rebuild"
gyp ERR! cwd /Users/simple
gyp ERR! node -v v5.8.0
gyp ERR! node-gyp -v v3.3.1
gyp ERR! not ok
```

 看到这一句：

```
binding.gyp not found
```

 根据 https://github.com/nodejs/node-gyp#the-bindinggyp-file 上写的

```
The "binding.gyp" file
 
Previously when node had node-waf you had to write a wscript file. The replacement for that is the binding.gyp file, which describes the configuration to build your module in a JSON-like format. This file gets placed in the root of your package, alongside the package.json file.
 
A barebones gyp file appropriate for building a node addon looks like:
```

```
{
  "targets": [
    {
      "target_name": "binding",
      "sources": [ "src/binding.cc" ]
    }
  ]
}
```

创建binding.gyp

```
vi binding.gyp
```

里面添加：

```
{
  "targets": [
    {
      "target_name": "binding",
      "sources": [ "src/binding.cc" ]
    }
  ]
}
```

然后：

```

npm init --yes
 

sudo npm install
 

sudo npm i -g sharp --unsafe-perm
```

 还是报错：

```
ERROR: Please install libvips by running: brew install homebrew/science/vips --with-webp --with-graphicsmagick

```

 再次安装：

```
brew install homebrew/science/vips
```



 安装成功后在安装sharp

```
sudo npm i -g sharp --unsafe-perm
```

```
192:~ simple$ sudo npm i -g sharp --unsafe-perm
 
> sharp@0.14.1 install /usr/local/lib/node_modules/sharp
> node-gyp rebuild
 
  TOUCH Release/obj.target/libvips-cpp.stamp
  CXX(target) Release/obj.target/sharp/src/common.o
  CXX(target) Release/obj.target/sharp/src/metadata.o
  CXX(target) Release/obj.target/sharp/src/operations.o
  CXX(target) Release/obj.target/sharp/src/pipeline.o
  CXX(target) Release/obj.target/sharp/src/sharp.o
  CXX(target) Release/obj.target/sharp/src/utilities.o
  SOLINK_MODULE(target) Release/sharp.node
  TOUCH Release/obj.target/win_copy_dlls.stamp
/usr/local/lib
└─┬ sharp@0.14.1
  ├── bluebird@3.3.5
  ├─┬ color@0.11.1
  │ ├── color-convert@0.5.3
  │ └─┬ color-string@0.3.0
  │   └── color-name@1.1.1
  ├── nan@2.3.2
  ├─┬ request@2.72.0
  │ ├── aws-sign2@0.6.0
  │ ├─┬ aws4@1.3.2
  │ │ └─┬ lru-cache@4.0.1
  │ │   ├── pseudomap@1.0.2
  │ │   └── yallist@2.0.0
  │ ├─┬ bl@1.1.2
  │ │ └─┬ readable-stream@2.0.6
  │ │   ├── core-util-is@1.0.2
  │ │   ├── isarray@1.0.0
  │ │   ├── process-nextick-args@1.0.6
  │ │   ├── string_decoder@0.10.31
  │ │   └── util-deprecate@1.0.2
  │ ├── caseless@0.11.0
  │ ├─┬ combined-stream@1.0.5
  │ │ └── delayed-stream@1.0.0
  │ ├── extend@3.0.0
  │ ├── forever-agent@0.6.1
  │ ├─┬ form-data@1.0.0-rc4
  │ │ └── async@1.5.2
  │ ├─┬ har-validator@2.0.6
  │ │ ├─┬ chalk@1.1.3
  │ │ │ ├── ansi-styles@2.2.1
  │ │ │ ├── escape-string-regexp@1.0.5
  │ │ │ ├─┬ has-ansi@2.0.0
  │ │ │ │ └── ansi-regex@2.0.0
  │ │ │ ├── strip-ansi@3.0.1
  │ │ │ └── supports-color@2.0.0
  │ │ ├─┬ commander@2.9.0
  │ │ │ └── graceful-readlink@1.0.1
  │ │ ├─┬ is-my-json-valid@2.13.1
  │ │ │ ├── generate-function@2.0.0
  │ │ │ ├─┬ generate-object-property@1.2.0
  │ │ │ │ └── is-property@1.0.2
  │ │ │ ├── jsonpointer@2.0.0
  │ │ │ └── xtend@4.0.1
  │ │ └─┬ pinkie-promise@2.0.1
  │ │   └── pinkie@2.0.4
  │ ├─┬ hawk@3.1.3
  │ │ ├── boom@2.10.1
  │ │ ├── cryptiles@2.0.5
  │ │ ├── hoek@2.16.3
  │ │ └── sntp@1.0.9
  │ ├─┬ http-signature@1.1.1
  │ │ ├── assert-plus@0.2.0
  │ │ ├─┬ jsprim@1.2.2
  │ │ │ ├── extsprintf@1.0.2
  │ │ │ ├── json-schema@0.2.2
  │ │ │ └── verror@1.3.6
  │ │ └─┬ sshpk@1.8.2
  │ │   ├── asn1@0.2.3
  │ │   ├── assert-plus@1.0.0
  │ │   ├─┬ dashdash@1.13.1
  │ │   │ └── assert-plus@1.0.0
  │ │   ├── ecc-jsbn@0.1.1
  │ │   ├─┬ getpass@0.1.6
  │ │   │ └── assert-plus@1.0.0
  │ │   ├── jodid25519@1.0.2
  │ │   ├── jsbn@0.1.0
  │ │   └── tweetnacl@0.13.3
  │ ├── is-typedarray@1.0.0
  │ ├── isstream@0.1.2
  │ ├── json-stringify-safe@5.0.1
  │ ├─┬ mime-types@2.1.10
  │ │ └── mime-db@1.22.0
  │ ├── node-uuid@1.4.7
  │ ├── oauth-sign@0.8.1
  │ ├── qs@6.1.0
  │ ├── stringstream@0.0.5
  │ ├── tough-cookie@2.2.2
  │ └── tunnel-agent@0.4.2
  ├── semver@5.1.0
  └─┬ tar@2.2.1
    ├── block-stream@0.0.8
    ├─┬ fstream@1.0.8
    │ ├── graceful-fs@4.1.3
    │ ├─┬ mkdirp@0.5.1
    │ │ └── minimist@0.0.8
    │ └─┬ rimraf@2.5.2
    │   └─┬ glob@7.0.3
    │     ├─┬ inflight@1.0.4
    │     │ └── wrappy@1.0.1
    │     ├─┬ minimatch@3.0.0
    │     │ └─┬ brace-expansion@1.1.3
    │     │   ├── balanced-match@0.3.0
    │     │   └── concat-map@0.0.1
    │     ├── once@1.3.3
    │     └── path-is-absolute@1.0.0
    └── inherits@2.0.1
 
192:~ simple$
```

