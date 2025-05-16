project('com.github.donadigo.eddy', 'vala', 'c')

gnome = import('gnome')
i18n = import('i18n')

conf = configuration_data()
conf.set_quoted('LOCALEDIR', get_option('prefix') / get_option('localedir'))
conf.set_quoted('GETTEXT_PACKAGE', meson.project_name())
config_file = configure_file(
  input: 'src' / 'Config.vala.in',
  output: '@BASENAME@',
  configuration: conf
)

add_global_arguments('-DGETTEXT_PACKAGE="@0@"'.format (meson.project_name()), language:'c')
add_project_arguments('-DI_KNOW_THE_PACKAGEKIT_GLIB2_API_IS_SUBJECT_TO_CHANGE', language : 'c')

posix_dep = meson.get_compiler('vala').find_library('posix')

deps = [
  dependency('glib-2.0'),
  dependency('granite-7'),
  dependency('gtk4'),
  dependency('packagekit-glib2'),
  posix_dep,
  meson.get_compiler('c').find_library('m', required : false)
]

if get_option('use_zeitgeist')
  deps += dependency('zeitgeist-2.0')
  add_project_arguments('--define=USE_ZEITGEIST', language: 'vala')
endif

executable(
    meson.project_name(),
    config_file,
    'src/Application.vala',
    'src/AppSettings.vala',
    'src/Constants.vala',
    'src/DetailedView.vala',
    'src/FolderLoader.vala',
    'src/MainWindow.vala',
    'src/MimeTypeHelper.vala',
    'src/Package.vala',
    'src/PackageListView.vala',
    'src/PackageRow.vala',
    'src/TransactionResult.vala',
    'src/LogManager.vala',
    'src/PackageUri.vala',
    dependencies: deps,
    install: true
)

subdir('data')
subdir('po')

gnome.post_install(
  glib_compile_schemas: true,
  gtk_update_icon_cache: true,
  update_desktop_database: true,
  )

