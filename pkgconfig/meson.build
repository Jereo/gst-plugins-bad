pkgconf = configuration_data()

pkgconf.set('prefix', get_option('prefix'))
pkgconf.set('exec_prefix', '${prefix}')
pkgconf.set('libdir', '${prefix}/@0@'.format(get_option('libdir')))
pkgconf.set('includedir', '${prefix}/@0@'.format(get_option('includedir')))
pkgconf.set('GST_API_VERSION', api_version)
pkgconf.set('VERSION', gst_version)

# needed for generating -uninstalled.pc files
pkgconf.set('abs_top_builddir', join_paths(meson.current_build_dir(), '..'))
pkgconf.set('abs_top_srcdir', join_paths(meson.current_source_dir(), '..'))
pkgconf.set('audiolibdir', join_paths(meson.build_root(), gstbadaudio.outdir()))
pkgconf.set('baselibdir', join_paths(meson.build_root(), gstbadbase.outdir()))
pkgconf.set('videolibdir', join_paths(meson.build_root(), gstbadvideo.outdir()))
pkgconf.set('codecparserslibdir', join_paths(meson.build_root(), gstcodecparsers.outdir()))
pkgconf.set('insertbinlibdir', join_paths(meson.build_root(), gstinsertbin.outdir()))
pkgconf.set('mpegtslibdir', join_paths(meson.build_root(), gstmpegts.outdir()))
pkgconf.set('playerlibdir', join_paths(meson.build_root(), gstplayer.outdir()))
pkgconf.set('basecamerabinsrclibdir', join_paths(meson.build_root(), gstbasecamerabin.outdir()))
pkgconf.set('photographylibdir', join_paths(meson.build_root(), gstphotography.outdir()))

pkg_install_dir = '@0@/pkgconfig'.format(get_option('libdir'))

pkg_libs = [
  'bad-audio',
  'bad-base',
  'bad-video',
  'codecparsers',
  'insertbin',
  'mpegts',
  'player',
  'plugins-bad',
]

# XXX: requires the meson.build to be parsed/executed after gst-libs/gl/meson.build
if build_gstgl
  pkgconf.set('gllibdir', join_paths(meson.build_root(), gstgl.outdir()))
  pkg_libs += 'gl'
  pkgconf.set('GL_APIS', ' '.join(enabled_gl_apis))
  pkgconf.set('GL_WINDOWS', ' '.join(enabled_gl_winsys))
  pkgconf.set('GL_PLATFORMS', ' '.join(enabled_gl_platforms))
endif

if use_wayland
    pkgconf.set('waylandlibdir', join_paths(meson.build_root(), gstwayland.outdir()))
    pkg_libs += 'wayland'
endif

foreach p : pkg_libs
  infile = 'gstreamer-@0@.pc.in'.format(p)
  outfile = 'gstreamer-@0@-@1@.pc'.format(p, api_version)
  configure_file(input : infile,
    output : outfile,
    configuration : pkgconf,
    install_dir : pkg_install_dir)

  infile = 'gstreamer-@0@-uninstalled.pc.in'.format(p)
  outfile = 'gstreamer-@0@-@1@-uninstalled.pc'.format(p, api_version)
  configure_file(input : infile,
    output : outfile,
    configuration : pkgconf)
endforeach
