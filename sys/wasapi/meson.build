wasapi_sources = [
  'gstwasapi.c',
  'gstwasapisrc.c',
  'gstwasapisink.c',
  'gstwasapiutil.c',
  'gstwasapidevice.c',
]

if host_system != 'windows'
  if get_option('wasapi').disabled()
    subdir_done()
  elif get_option('wasapi').enabled()
    error('Cannot build wasapi plugin when not building for Windows')
  endif
endif

ole32_dep = cc.find_library('ole32', required : get_option('wasapi'))
ksuser_dep = cc.find_library('ksuser', required : get_option('wasapi'))
have_audioclient_h = cc.has_header('audioclient.h')
if not have_audioclient_h and get_option('wasapi').enabled()
  error('wasapi plugin enabled but audioclient.h not found')
endif

if ole32_dep.found() and ksuser_dep.found() and have_audioclient_h
  gstwasapi = library('gstwasapi',
    wasapi_sources,
    c_args : gst_plugins_bad_args + ['-DCOBJMACROS'],
    include_directories : [configinc],
    dependencies : [gstaudio_dep, ole32_dep, ksuser_dep],
    install : true,
    install_dir : plugins_install_dir)
  pkgconfig.generate(gstwasapi, install_dir : plugins_pkgconfig_install_dir)
endif
