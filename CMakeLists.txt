cmake_minimum_required( VERSION 3.6.0 )

#if( CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR )
#    set( STANDALONE TRUE )
#endif()

project( ImageMagick )

#############################################################
# JPEG LIBRARY
#############################################################

set( source_dir jni/jpeg-9b )
set( sources
    ${source_dir}/jcapimin.c
    ${source_dir}/jcapistd.c
    ${source_dir}/jccoefct.c
    ${source_dir}/jccolor.c
    ${source_dir}/jcdctmgr.c
    ${source_dir}/jchuff.c
    ${source_dir}/jcinit.c
    ${source_dir}/jcmainct.c
    ${source_dir}/jcmarker.c
    ${source_dir}/jcmaster.c
    ${source_dir}/jcomapi.c
    ${source_dir}/jcparam.c
    ${source_dir}/jcprepct.c
    ${source_dir}/jcsample.c
    ${source_dir}/jctrans.c
    ${source_dir}/jdapimin.c
    ${source_dir}/jdapistd.c
    ${source_dir}/jdatadst.c
    ${source_dir}/jdatasrc.c
    ${source_dir}/jdcoefct.c
    ${source_dir}/jdcolor.c
    ${source_dir}/jddctmgr.c
    ${source_dir}/jdhuff.c
    ${source_dir}/jdinput.c
    ${source_dir}/jdmainct.c
    ${source_dir}/jdmarker.c
    ${source_dir}/jdmaster.c
    ${source_dir}/jdmerge.c
    ${source_dir}/jdpostct.c
    ${source_dir}/jdsample.c
    ${source_dir}/jdtrans.c
    ${source_dir}/jerror.c
    ${source_dir}/jfdctflt.c
    ${source_dir}/jfdctfst.c
    ${source_dir}/jfdctint.c
    ${source_dir}/jidctflt.c
    ${source_dir}/jidctfst.c
    ${source_dir}/jidctint.c
    ${source_dir}/jquant1.c
    ${source_dir}/jquant2.c
    ${source_dir}/jutils.c
    ${source_dir}/jmemmgr.c
    ${source_dir}/jcarith.c
    ${source_dir}/jdarith.c
    ${source_dir}/jaricom.c
    ${source_dir}/jmemnobs.c
)

add_library( jpeg STATIC ${sources} )
target_include_directories( jpeg PUBLIC ${source_dir} )

#############################################################
# TIFF LIBRARY
#############################################################

find_package(TIFF)
if(TIFF_FOUND)
    add_library( tiff ALIAS TIFF::TIFF )
else()
    set( source_dir jni/tiff-3.9.7 )
    set( sources
        ${source_dir}/libtiff/mkg3states.c
        ${source_dir}/libtiff/tif_aux.c
        ${source_dir}/libtiff/tif_close.c
        ${source_dir}/libtiff/tif_codec.c
        ${source_dir}/libtiff/tif_color.c
        ${source_dir}/libtiff/tif_compress.c
        ${source_dir}/libtiff/tif_dir.c
        ${source_dir}/libtiff/tif_dirinfo.c
        ${source_dir}/libtiff/tif_dirread.c
        ${source_dir}/libtiff/tif_dirwrite.c
        ${source_dir}/libtiff/tif_dumpmode.c
        ${source_dir}/libtiff/tif_error.c
        ${source_dir}/libtiff/tif_extension.c
        ${source_dir}/libtiff/tif_fax3.c
        ${source_dir}/libtiff/tif_fax3sm.c
        ${source_dir}/libtiff/tif_flush.c
        ${source_dir}/libtiff/tif_getimage.c
        ${source_dir}/libtiff/tif_jbig.c
        ${source_dir}/libtiff/tif_jpeg.c
        ${source_dir}/libtiff/tif_luv.c
        ${source_dir}/libtiff/tif_lzw.c
        ${source_dir}/libtiff/tif_next.c
        ${source_dir}/libtiff/tif_ojpeg.c
        ${source_dir}/libtiff/tif_open.c
        ${source_dir}/libtiff/tif_packbits.c
        ${source_dir}/libtiff/tif_pixarlog.c
        ${source_dir}/libtiff/tif_predict.c
        ${source_dir}/libtiff/tif_print.c
        ${source_dir}/libtiff/tif_read.c
        ${source_dir}/libtiff/tif_strip.c
        ${source_dir}/libtiff/tif_swab.c
        ${source_dir}/libtiff/tif_thunder.c
        ${source_dir}/libtiff/tif_tile.c
        ${source_dir}/libtiff/tif_unix.c
        ${source_dir}/libtiff/tif_version.c
        ${source_dir}/libtiff/tif_warning.c
        ${source_dir}/libtiff/tif_write.c
        ${source_dir}/libtiff/tif_zip.c
        ${source_dir}/port/lfind.c
    )

    add_library( tiff STATIC ${sources} )
    target_link_libraries( tiff PUBLIC jpeg )
    target_include_directories( tiff PUBLIC ${source_dir}/libtiff )
endif()

#############################################################
# PNG LIBRARY
#############################################################

find_package(PNG)
if(PNG_FOUND)
    add_library(png-for-magick ALIAS PNG::PNG)
else()
    set( source_dir jni/libpng-1.5.28 )
    set( sources
        ${source_dir}/pngerror.c
        ${source_dir}/pngwio.c
        ${source_dir}/pngwrite.c
        ${source_dir}/pngwutil.c
        ${source_dir}/pngwtran.c
        ${source_dir}/pngset.c
        ${source_dir}/pngtrans.c
        ${source_dir}/example.c
        ${source_dir}/png.c
        ${source_dir}/pngrtran.c
        ${source_dir}/pngmem.c
        ${source_dir}/pngpread.c
        ${source_dir}/pngrutil.c
        ${source_dir}/pngrio.c
        ${source_dir}/pngget.c
        ${source_dir}/pngread.c
    )

    # Don't just call this 'libpng' because there might already be one
    # externally. This one is internal only for imagemagick; we do not
    # and should not expose it outside.
    add_library( png-for-magick STATIC ${sources} )
    target_include_directories( png-for-magick PUBLIC ${source_dir} )
endif()

#############################################################
# COMMON MAGICK VARIABLES
#############################################################

set( source_dir jni/ImageMagick )

set( MAGICKCORE_HDRI_ENABLE 0 CACHE STRING "HDRI enabled or not; passed to ImageMagick" )
set( MAGICKCORE_QUANTUM_DEPTH 8 CACHE STRING "quantum depth passed to ImageMagick" )

set( definitions
    MAGICKCORE_HDRI_ENABLE=${MAGICKCORE_HDRI_ENABLE}
    MAGICKCORE_QUANTUM_DEPTH=${MAGICKCORE_QUANTUM_DEPTH}
)

#############################################################
# MAGICK CORE LIBRARY
#############################################################

set( sources
    ${source_dir}/coders/aai.c
    ${source_dir}/coders/art.c
    ${source_dir}/coders/avs.c
    ${source_dir}/coders/bgr.c
    ${source_dir}/coders/bmp.c
    ${source_dir}/coders/braille.c
    ${source_dir}/coders/cals.c
    ${source_dir}/coders/caption.c
    ${source_dir}/coders/cin.c
    ${source_dir}/coders/cip.c
    ${source_dir}/coders/clip.c
    ${source_dir}/coders/clipboard.c
    ${source_dir}/coders/cmyk.c
    ${source_dir}/coders/cut.c
    ${source_dir}/coders/dcm.c
    ${source_dir}/coders/dds.c
    ${source_dir}/coders/debug.c
    ${source_dir}/coders/dib.c
    ${source_dir}/coders/dng.c
    ${source_dir}/coders/dps.c
    ${source_dir}/coders/dpx.c
    ${source_dir}/coders/emf.c
    ${source_dir}/coders/ept.c
    ${source_dir}/coders/exr.c
    ${source_dir}/coders/fax.c
    ${source_dir}/coders/fits.c
    ${source_dir}/coders/fpx.c
    ${source_dir}/coders/gif.c
    ${source_dir}/coders/gradient.c
    ${source_dir}/coders/gray.c
    ${source_dir}/coders/hald.c
    ${source_dir}/coders/hdr.c
    ${source_dir}/coders/histogram.c
    ${source_dir}/coders/hrz.c
    ${source_dir}/coders/html.c
    ${source_dir}/coders/icon.c
    ${source_dir}/coders/info.c
    ${source_dir}/coders/inline.c
    ${source_dir}/coders/ipl.c
    ${source_dir}/coders/jbig.c
    ${source_dir}/coders/jp2.c
    ${source_dir}/coders/jpeg.c
    ${source_dir}/coders/label.c
    ${source_dir}/coders/mac.c
    ${source_dir}/coders/magick.c
    ${source_dir}/coders/map.c
    ${source_dir}/coders/mat.c
    ${source_dir}/coders/matte.c
    ${source_dir}/coders/meta.c
    ${source_dir}/coders/mono.c
    ${source_dir}/coders/mpc.c
    ${source_dir}/coders/mpeg.c
    ${source_dir}/coders/mpr.c
    ${source_dir}/coders/msl.c
    ${source_dir}/coders/mtv.c
    ${source_dir}/coders/mvg.c
    ${source_dir}/coders/null.c
    ${source_dir}/coders/otb.c
    ${source_dir}/coders/palm.c
    ${source_dir}/coders/pattern.c
    ${source_dir}/coders/pcd.c
    ${source_dir}/coders/pcl.c
    ${source_dir}/coders/pcx.c
    ${source_dir}/coders/pdb.c
    ${source_dir}/coders/pdf.c
    ${source_dir}/coders/pes.c
    ${source_dir}/coders/pict.c
    ${source_dir}/coders/pix.c
    ${source_dir}/coders/plasma.c
    ${source_dir}/coders/png.c
    ${source_dir}/coders/pnm.c
    ${source_dir}/coders/ps.c
    ${source_dir}/coders/ps2.c
    ${source_dir}/coders/ps3.c
    ${source_dir}/coders/psd.c
    ${source_dir}/coders/pwp.c
    ${source_dir}/coders/raw.c
    ${source_dir}/coders/rgb.c
    ${source_dir}/coders/rla.c
    ${source_dir}/coders/rle.c
    ${source_dir}/coders/scr.c
    ${source_dir}/coders/sct.c
    ${source_dir}/coders/sfw.c
    ${source_dir}/coders/sgi.c
    ${source_dir}/coders/stegano.c
    ${source_dir}/coders/sun.c
    ${source_dir}/coders/svg.c
    ${source_dir}/coders/tga.c
    ${source_dir}/coders/thumbnail.c
    ${source_dir}/coders/tiff.c
    ${source_dir}/coders/tile.c
    ${source_dir}/coders/tim.c
    ${source_dir}/coders/ttf.c
    ${source_dir}/coders/txt.c
    ${source_dir}/coders/uil.c
    ${source_dir}/coders/url.c
    ${source_dir}/coders/uyvy.c
    ${source_dir}/coders/vicar.c
    ${source_dir}/coders/vid.c
    ${source_dir}/coders/viff.c
    ${source_dir}/coders/wbmp.c
    ${source_dir}/coders/webp.c
    ${source_dir}/coders/wmf.c
    ${source_dir}/coders/wpg.c
    ${source_dir}/coders/x.c
    ${source_dir}/coders/xbm.c
    ${source_dir}/coders/xc.c
    ${source_dir}/coders/xcf.c
    ${source_dir}/coders/xpm.c
    ${source_dir}/coders/xps.c
    ${source_dir}/coders/xwd.c
    ${source_dir}/coders/ycbcr.c
    ${source_dir}/coders/yuv.c
    ${source_dir}/coders/djvu.c
    ${source_dir}/coders/fd.c
    ${source_dir}/coders/jnx.c
    ${source_dir}/coders/json.c
    ${source_dir}/coders/mask.c
    ${source_dir}/coders/miff.c
    ${source_dir}/coders/pango.c
    ${source_dir}/coders/rgf.c
    ${source_dir}/coders/screenshot.c
    ${source_dir}/coders/sixel.c
    ${source_dir}/coders/vips.c
    ${source_dir}/coders/pgx.c
    ${source_dir}/filters/analyze.c
    ${source_dir}/MagickCore/accelerate.c
    ${source_dir}/MagickCore/animate.c
    ${source_dir}/MagickCore/annotate.c
    ${source_dir}/MagickCore/artifact.c
    ${source_dir}/MagickCore/attribute.c
    ${source_dir}/MagickCore/blob.c
    ${source_dir}/MagickCore/cache.c
    ${source_dir}/MagickCore/cache-view.c
    ${source_dir}/MagickCore/channel.c
    ${source_dir}/MagickCore/cipher.c
    ${source_dir}/MagickCore/client.c
    ${source_dir}/MagickCore/coder.c
    ${source_dir}/MagickCore/color.c
    ${source_dir}/MagickCore/colormap.c
    ${source_dir}/MagickCore/colorspace.c
    ${source_dir}/MagickCore/compare.c
    ${source_dir}/MagickCore/composite.c
    ${source_dir}/MagickCore/compress.c
    ${source_dir}/MagickCore/configure.c
    ${source_dir}/MagickCore/constitute.c
    ${source_dir}/MagickCore/decorate.c
    ${source_dir}/MagickCore/delegate.c
    ${source_dir}/MagickCore/deprecate.c
    ${source_dir}/MagickCore/display.c
    ${source_dir}/MagickCore/distort.c
    ${source_dir}/MagickCore/distribute-cache.c
    ${source_dir}/MagickCore/draw.c
    ${source_dir}/MagickCore/effect.c
    ${source_dir}/MagickCore/enhance.c
    ${source_dir}/MagickCore/exception.c
    ${source_dir}/MagickCore/feature.c
    ${source_dir}/MagickCore/fourier.c
    ${source_dir}/MagickCore/fx.c
    ${source_dir}/MagickCore/gem.c
    ${source_dir}/MagickCore/geometry.c
    ${source_dir}/MagickCore/histogram.c
    ${source_dir}/MagickCore/identify.c
    ${source_dir}/MagickCore/image.c
    ${source_dir}/MagickCore/image-view.c
    ${source_dir}/MagickCore/layer.c
    ${source_dir}/MagickCore/linked-list.c
    ${source_dir}/MagickCore/list.c
    ${source_dir}/MagickCore/locale.c
    ${source_dir}/MagickCore/log.c
    ${source_dir}/MagickCore/magic.c
    ${source_dir}/MagickCore/magick.c
    ${source_dir}/MagickCore/matrix.c
    ${source_dir}/MagickCore/memory.c
    ${source_dir}/MagickCore/mime.c
    ${source_dir}/MagickCore/module.c
    ${source_dir}/MagickCore/monitor.c
    ${source_dir}/MagickCore/montage.c
    ${source_dir}/MagickCore/morphology.c
    ${source_dir}/MagickCore/nt-base.c
    ${source_dir}/MagickCore/nt-feature.c
    ${source_dir}/MagickCore/opencl.c
    ${source_dir}/MagickCore/option.c
    ${source_dir}/MagickCore/paint.c
    ${source_dir}/MagickCore/pixel.c
    ${source_dir}/MagickCore/policy.c
    ${source_dir}/MagickCore/prepress.c
    ${source_dir}/MagickCore/profile.c
    ${source_dir}/MagickCore/property.c
    ${source_dir}/MagickCore/quantize.c
    ${source_dir}/MagickCore/quantum.c
    ${source_dir}/MagickCore/quantum-export.c
    ${source_dir}/MagickCore/quantum-import.c
    ${source_dir}/MagickCore/random.c
    ${source_dir}/MagickCore/registry.c
    ${source_dir}/MagickCore/resample.c
    ${source_dir}/MagickCore/resize.c
    ${source_dir}/MagickCore/resource.c
    ${source_dir}/MagickCore/segment.c
    ${source_dir}/MagickCore/semaphore.c
    ${source_dir}/MagickCore/shear.c
    ${source_dir}/MagickCore/signature.c
    ${source_dir}/MagickCore/splay-tree.c
    ${source_dir}/MagickCore/static.c
    ${source_dir}/MagickCore/statistic.c
    ${source_dir}/MagickCore/stream.c
    ${source_dir}/MagickCore/string.c
    ${source_dir}/MagickCore/thread.c
    ${source_dir}/MagickCore/threshold.c
    ${source_dir}/MagickCore/timer.c
    ${source_dir}/MagickCore/token.c
    ${source_dir}/MagickCore/transform.c
    ${source_dir}/MagickCore/type.c
    ${source_dir}/MagickCore/utility.c
    ${source_dir}/MagickCore/version.c
    ${source_dir}/MagickCore/vision.c
    ${source_dir}/MagickCore/widget.c
    ${source_dir}/MagickCore/xml-tree.c
    ${source_dir}/MagickCore/xwindow.c
)

add_library( MagickCore SHARED ${sources} )
set( dependencies PRIVATE tiff jpeg png )
if( ANDROID )
    list( APPEND dependencies PRIVATE m )
endif()
target_link_libraries( MagickCore ${dependencies} )
target_include_directories( MagickCore PUBLIC ${source_dir} )
target_compile_definitions( MagickCore PUBLIC ${definitions} )

#############################################################
# MAGICK WAND LIBRARY
#############################################################

set( sources
    ${source_dir}/MagickWand/animate.c
    ${source_dir}/MagickWand/compare.c
    ${source_dir}/MagickWand/composite.c
    ${source_dir}/MagickWand/conjure.c
    ${source_dir}/MagickWand/convert.c
    ${source_dir}/MagickWand/deprecate.c
    ${source_dir}/MagickWand/display.c
    ${source_dir}/MagickWand/drawing-wand.c
    ${source_dir}/MagickWand/identify.c
    ${source_dir}/MagickWand/import.c
    ${source_dir}/MagickWand/magick-cli.c
    ${source_dir}/MagickWand/magick-image.c
    ${source_dir}/MagickWand/magick-property.c
    ${source_dir}/MagickWand/magick-wand.c
    ${source_dir}/MagickWand/mogrify.c
    ${source_dir}/MagickWand/montage.c
    ${source_dir}/MagickWand/operation.c
    ${source_dir}/MagickWand/pixel-iterator.c
    ${source_dir}/MagickWand/pixel-wand.c
    ${source_dir}/MagickWand/script-token.c
    ${source_dir}/MagickWand/stream.c
    ${source_dir}/MagickWand/wand.c
    ${source_dir}/MagickWand/wandcli.c
    ${source_dir}/MagickWand/wand-view.c
)

add_library( MagickWand SHARED ${sources} )
set( dependencies PRIVATE tiff jpeg png PUBLIC MagickCore )
if( ANDROID )
    list( APPEND dependencies PRIVATE m )
endif()
target_link_libraries( MagickWand ${dependencies} )
target_include_directories( MagickWand PUBLIC ${source_dir} )
target_compile_definitions( MagickWand PUBLIC ${definitions} )
