if get_option('gtk_doc')

  gidocgen_dep = dependency('gi-docgen', version: '>= 2021.6',
                          fallback: ['gi-docgen', 'dummy_dep'],
                          required: true)

  toml_conf = configuration_data()
  toml_conf.set('VERSION', meson.project_version())

  gidocgen = find_program('gi-docgen', required: true)

  gidocgen_common_args = [
    '--quiet',
    '--no-namespace-dir',
    '--fatal-warnings',
  ]

  subdir('glib')
  subdir('gmodule')
  subdir('gobject')
  subdir('gio')

endif
