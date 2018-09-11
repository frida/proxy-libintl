project('proxy-libintl', 'c',
  version : '1',
  meson_version : '>= 0.46.0',
  default_options : [ 'warning_level=1',
                      'c_std=gnu99',
                      'buildtype=debugoptimized' ])

install_headers('libintl.h')

intl_lib = library('intl',
  'libintl.c',
  c_args: ['-DSTUB_ONLY'],
  soversion : 8,
  darwin_versions : ['10', '10.5'],
  install : true)

intl_dep = declare_dependency(link_with : intl_lib,
  include_directories : include_directories('.'))
