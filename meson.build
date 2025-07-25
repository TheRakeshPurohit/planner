project(
  'io.github.alainm23.planify',
  'vala', 'c',
  version: '4.13.0'
)

gnome = import('gnome')
i18n = import('i18n')
pkgconfig = import('pkgconfig')

add_global_arguments('-DGETTEXT_PACKAGE="@0@"'.format (meson.project_name()), language:'c')
add_project_arguments('-DLIBICAL_GLIB_UNSTABLE_API=1', language: 'c')

gio_dep = dependency('gio-2.0')
glib_dep = dependency('glib-2.0')
gee_dep = dependency('gee-0.8')
gtk_dep = dependency('gtk4')
libsoup_dep = dependency('libsoup-3.0')
sqlite3_dep = dependency('sqlite3')
libadwaita_dep = dependency('libadwaita-1', version: '>= 1.6.0')
webkitgtk_dep = dependency('webkitgtk-6.0')
gtksourceview_dep = dependency('gtksourceview-5')
granite_dep = dependency('granite-7')
json_dep = dependency('json-glib-1.0')
libecal_dep = dependency('libecal-2.0', version: '>= 3.45.1')
libedataserver_dep = dependency('libedataserver-1.2', version: '>= 3.45.1')
libical_dep = dependency('libical-glib')
libportal_dep = [ dependency('libportal'), dependency('libportal-gtk4') ]
libgxml_dep = dependency('gxml-0.20')
libsecret_dep = dependency('libsecret-1')
m_dep = meson.get_compiler('c').find_library('m', required : false)

############
# Resources #
############

asresources = gnome.compile_resources (
    'as-resources', 'data/io.github.alainm23.planify.gresource.xml',
    source_dir: 'data',
    c_name: 'as'
)

###########
# Profile #
###########

profile = get_option('profile')
if profile == 'development'
  find_program('git')
  rev_txt = run_command('git','rev-parse','--short','HEAD').stdout().strip()
  rev = '-@0@'.format(rev_txt)
  application_id = 'io.github.alainm23.planify.Devel'
  application_path = '/io/github/alainm23/planify/Devel'
else
  rev = ''
  application_id = 'io.github.alainm23.planify'
  application_path = '/io/github/alainm23/planify'
endif

############
# Config #
############
prefix = get_option('prefix')
bindir = prefix / get_option('bindir')
datadir = prefix / get_option('datadir')
localedir = prefix / get_option ('localedir')
libexecdir = prefix / get_option('libexecdir')

conf_data = configuration_data()
conf_data.set_quoted('APPLICATION_ID', application_id)
conf_data.set_quoted('GETTEXT_PACKAGE', application_id)
conf_data.set_quoted('GNOMELOCALEDIR', localedir)
conf_data.set_quoted('VERSION', meson.project_version())
conf_data.set_quoted('LOCALEDIR', localedir)
conf_data.set_quoted('PACKAGE_VERSION', meson.project_version())
conf_data.set_quoted('PREFIX', prefix)
conf_data.set_quoted('DATADIR', datadir)
conf_data.set_quoted('PROFILE', profile)

config_file = configure_file(
    configuration : conf_data,
    input : 'config.vala.in',
    output : 'config.vala'
)

subdir('core')
subdir('src')
subdir('quick-add')
subdir('data')
# subdir('search-provider')
subdir('po')

summary({
  'Tracing': get_option('tracing'),
  'Profile': get_option('profile'),
}, section: 'Development')

gnome.post_install(
  glib_compile_schemas: true,
  gtk_update_icon_cache: true,
  update_desktop_database: true,
)
