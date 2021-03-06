csstest_env = environment()
csstest_env.set('GTK_A11Y', 'test')
csstest_env.set('GSK_RENDERER', 'cairo')
csstest_env.set('G_TEST_SRCDIR', meson.current_source_dir())
csstest_env.set('G_TEST_BUILDDIR', meson.current_build_dir())
csstest_env.set('GIO_USE_VFS', 'local')
csstest_env.set('GSETTINGS_BACKEND', 'memory')
csstest_env.set('G_ENABLE_DIAGNOSTIC', '0')

subdir('parser')
subdir('nodes')
subdir('style')
subdir('change')

testexecdir = join_paths(installed_test_bindir, 'css')
testdatadir = join_paths(installed_test_datadir, 'css')

test_api = executable('api', 'api.c',
  c_args: common_cflags,
  dependencies: libgtk_dep,
  install: get_option('install-tests'),
  install_dir: testexecdir,
)

test('api', test_api,
  args: ['--tap', '-k' ],
  protocol: 'tap',
  env: csstest_env,
  suite: 'css',
)

test_data = executable('data', ['data.c', '../../gtk/css/gtkcssdataurl.c'],
  c_args: common_cflags,
  include_directories: [confinc, ],
  dependencies: gtk_deps,
  install: get_option('install-tests'),
  install_dir: testexecdir,
)

test('data', test_data,
  args: ['--tap', '-k' ],
  protocol: 'tap',
  env: csstest_env,
  suite: 'css',
)

if get_option('install-tests')
  conf = configuration_data()
  conf.set('libexecdir', gtk_libexecdir)
  configure_file(input: 'api.test.in',
    output: 'api.test',
    configuration: conf,
    install_dir: testdatadir,
  )
endif

if false and get_option ('profiler')

  adwaita_env = csstest_env
  adwaita_env.set('GTK_THEME', 'Adwaita')
  test('performance-adwaita', test_performance,
    args: [
      '--mark', 'css validation',
      '--name', 'performance-adwaita',
      '--output', join_paths(meson.current_build_dir(), 'output'),
      join_paths(meson.current_build_dir(), '../../demos/widget-factory/gtk4-widget-factory'),
    ],
    env: adwaita_env,
    suite: [ 'css' ],
  )

  empty_env = csstest_env
  empty_env.set('GTK_THEME', 'Empty')
  test('performance-empty', test_performance,
    args: [
      '--mark', 'css validation',
      '--name', 'performance-empty',
      '--output', join_paths(meson.current_build_dir(), 'output'),
      join_paths(meson.current_build_dir(), '../../demos/widget-factory/gtk4-widget-factory'),
    ],
    env: empty_env,
    suite: [ 'css' ],
  )
endif
