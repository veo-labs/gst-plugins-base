# variables used for enum generation
ORC_SOURCE=video-orc
include $(top_srcdir)/common/orc.mak

glib_enum_headers = video.h video-format.h video-color.h video-info.h video-dither.h \
			colorbalance.h navigation.h video-chroma.h video-tile.h video-converter.h \
			video-resampler.h
glib_enum_define = GST_VIDEO
glib_gen_prefix = gst_video
glib_gen_basename = video

built_sources = video-enumtypes.c
built_headers = video-enumtypes.h
BUILT_SOURCES += $(built_sources) $(built_headers)

lib_LTLIBRARIES = libgstvideo-@GST_API_VERSION@.la

CLEANFILES = $(BUILT_SOURCES)

libgstvideo_@GST_API_VERSION@_la_SOURCES = \
	colorbalance.c   	\
	colorbalancechannel.c   \
	navigation.c            \
	video.c          	\
	video-event.c          	\
	video-format.c         	\
	video-chroma.c         	\
	video-color.c         	\
	video-converter.c    	\
	video-dither.c    	\
	video-info.c         	\
	video-frame.c         	\
	video-scaler.c          \
	video-tile.c         	\
	gstvideosink.c   	\
	gstvideofilter.c 	\
	convertframe.c   	\
	gstvideometa.c   	\
	gstvideopool.c		\
	videoorientation.c	\
	videooverlay.c          \
	gstvideodecoder.c       \
	gstvideoencoder.c       \
	gstvideoutils.c		\
	gstvideoutilsprivate.c	\
	video-resampler.c	\
	video-blend.c		\
	video-overlay-composition.c

nodist_libgstvideo_@GST_API_VERSION@_la_SOURCES = $(BUILT_SOURCES)

libgstvideo_@GST_API_VERSION@includedir = $(includedir)/gstreamer-@GST_API_VERSION@/gst/video
libgstvideo_@GST_API_VERSION@include_HEADERS = \
	colorbalance.h 		\
	colorbalancechannel.h	\
	navigation.h            \
	video.h			\
	video-event.h          	\
	video-format.h         	\
	video-chroma.h         	\
	video-color.h         	\
	video-converter.h      	\
	video-dither.h      	\
	video-info.h         	\
	video-frame.h         	\
	video-scaler.h          \
	video-tile.h         	\
	gstvideosink.h 		\
	gstvideofilter.h	\
	gstvideometa.h		\
	gstvideopool.h		\
	videoorientation.h 	\
	videooverlay.h          \
	gstvideodecoder.h       \
	gstvideoencoder.h       \
	gstvideoutils.h		\
	video-resampler.h	\
	video-blend.h		\
	video-overlay-composition.h

nodist_libgstvideo_@GST_API_VERSION@include_HEADERS = $(built_headers)
noinst_HEADERS = gstvideoutilsprivate.h

libgstvideo_@GST_API_VERSION@_la_CFLAGS = $(GST_PLUGINS_BASE_CFLAGS) $(GST_BASE_CFLAGS) $(GST_CFLAGS) \
					$(ORC_CFLAGS)
libgstvideo_@GST_API_VERSION@_la_LIBADD = $(GST_BASE_LIBS) $(GST_LIBS) $(ORC_LIBS) $(LIBM)
libgstvideo_@GST_API_VERSION@_la_LDFLAGS = $(GST_LIB_LDFLAGS) $(GST_ALL_LDFLAGS) $(GST_LT_LDFLAGS)

include $(top_srcdir)/common/gst-glib-gen.mak

if HAVE_INTROSPECTION
BUILT_GIRSOURCES = GstVideo-@GST_API_VERSION@.gir

gir_headers=$(patsubst %,$(srcdir)/%, $(libgstvideo_@GST_API_VERSION@include_HEADERS))
gir_headers+=$(patsubst %,$(builddir)/%, $(built_headers))
gir_sources=$(patsubst %,$(srcdir)/%, $(libgstvideo_@GST_API_VERSION@_la_SOURCES))
gir_sources+=$(patsubst %,$(builddir)/%, $(built_sources))

GstVideo-@GST_API_VERSION@.gir: $(INTROSPECTION_SCANNER) libgstvideo-@GST_API_VERSION@.la
	$(AM_V_GEN)PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" \
		GST_PLUGIN_SYSTEM_PATH_1_0="" GST_PLUGIN_PATH_1_0="" GST_REGISTRY_UPDATE=no \
		$(INTROSPECTION_SCANNER) -v --namespace GstVideo \
		--nsversion=@GST_API_VERSION@ \
		--strip-prefix=Gst \
		--warn-all \
		--c-include "gst/video/video.h" \
		-I$(top_srcdir)/gst-libs \
		-I$(top_builddir)/gst-libs \
		--add-include-path=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-@GST_API_VERSION@` \
		--add-include-path=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-base-@GST_API_VERSION@` \
		--library=libgstvideo-@GST_API_VERSION@.la \
		--include=Gst-@GST_API_VERSION@ \
		--include=GstBase-@GST_API_VERSION@ \
		--libtool="$(top_builddir)/libtool" \
		--pkg gstreamer-@GST_API_VERSION@ \
		--pkg gstreamer-base-@GST_API_VERSION@ \
		--pkg-export gstreamer-video-@GST_API_VERSION@ \
		--add-init-section="gst_init(NULL,NULL);" \
		--output $@ \
		$(gir_headers) \
		$(gir_sources)

# INTROSPECTION_GIRDIR/INTROSPECTION_TYPELIBDIR aren't the right place to
# install anything - we need to install inside our prefix.
girdir = $(datadir)/gir-1.0
gir_DATA = $(BUILT_GIRSOURCES)

typelibsdir = $(libdir)/girepository-1.0/

typelibs_DATA = $(BUILT_GIRSOURCES:.gir=.typelib)

%.typelib: %.gir $(INTROSPECTION_COMPILER)
	$(AM_V_GEN)PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" \
		$(INTROSPECTION_COMPILER) \
		--includedir=$(srcdir) \
		--includedir=$(builddir) \
		--includedir=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-@GST_API_VERSION@` \
		--includedir=`PKG_CONFIG_PATH="$(GST_PKG_CONFIG_PATH)" $(PKG_CONFIG) --variable=girdir gstreamer-base-@GST_API_VERSION@` \
		$(INTROSPECTION_COMPILER_OPTS) $< -o $(@F)

CLEANFILES += $(BUILT_GIRSOURCES) $(typelibs_DATA)
endif

Android.mk: Makefile.am $(BUILT_SOURCES)
	androgenizer -:PROJECT libgstvideo -:SHARED libgstvideo-@GST_API_VERSION@ \
	 -:TAGS eng debug \
         -:REL_TOP $(top_srcdir) -:ABS_TOP $(abs_top_srcdir) \
	 -:SOURCES $(libgstvideo_@GST_API_VERSION@_la_SOURCES) \
	           $(nodist_libgstvideo_@GST_API_VERSION@_la_SOURCES) \
	 -:CFLAGS $(DEFS) $(DEFAULT_INCLUDES) $(libgstvideo_@GST_API_VERSION@_la_CFLAGS) \
	 -:LDFLAGS $(libgstvideo_@GST_API_VERSION@_la_LDFLAGS) \
	           $(libgstvideo_@GST_API_VERSION@_la_LIBADD) \
	           -ldl \
	 -:HEADER_TARGET gstreamer-@GST_API_VERSION@/gst/tag \
	 -:HEADERS $(libgstvideo_@GST_API_VERSION@include_HEADERS) \
	           $(nodist_libgstvideo_@GST_API_VERSION@include_HEADERS) \
	 -:PASSTHROUGH LOCAL_ARM_MODE:=arm \
	> $@
