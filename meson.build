# meson.build
# supertoq 2026-01-16
# Project definition (App ID)
project('io.github.supertoq.finden', 'c', meson_version: '>= 0.59', 
  default_options: [
    'warning_level=3',
    'werror=false',
    'c_std=c11',
    'buildtype=release',
    'prefix=/usr/'
  ]
)

# Base structure
src_dir  = 'src'
data_dir = 'data'
icons_dir = join_paths(data_dir, 'icons') # Icons
po_dir   = join_paths(src_dir, 'po')      # PO files
source_root = meson.global_source_root()

# GResource XML definition
gresource_xml = join_paths(src_dir, 'io.github.supertoq.finden.gresource.xml')

# Dependencies
glib_dep    = dependency('glib-2.0', required : true)
gtk_dep     = dependency('gtk4', required : true)
adwaita_dep = dependency('libadwaita-1', version: '>= 1.8.alpha', required : true)
gnome = import('gnome')

# Remove "unused" warnings
add_project_arguments(
 '-w',
 language : 'c'
)

# Generate ressources file
gresource_c = custom_target(
  'gresource',
  output : 'io.github.supertoq.finden-gresource.c',
  command : ['glib-compile-resources', join_paths(source_root, 'src', 'resources.xml'),
                      '--sourcedir=' + join_paths(source_root, 'data', 'icons'),
                      '--generate-source',
                      '--target=' + 'io.github.supertoq.finden-gresource.c']
)

# Executable
executable('supertoq-finden',
  sources : [
    join_paths(src_dir, 'main.c'),
    join_paths(src_dir, 'config.c'),
    gresource_c
  ],
  dependencies : [glib_dep, gtk_dep, adwaita_dep],
  install : true,
  install_dir : join_paths(get_option('prefix'), 'bin')
)


# Sub - Miniterm Path
# subdir('src/miniterm')

# Install Icon (App ID)
install_data(join_paths(icons_dir, 'io.github.supertoq.finden.svg'),
 install_dir : join_paths(get_option('prefix'), 'share', 'icons', 'hicolor', 'scalable', 'apps'))
# install_data(join_paths(icons_dir, 'io.github.supertoq.miniterm.svg'),
# install_dir : join_paths(get_option('prefix'), 'share', 'icons', 'hicolor', 'scalable', 'apps'))

# Install .desktop file (App ID)
install_data(join_paths(data_dir, 'io.github.supertoq.finden.desktop.download'),
 install_dir: join_paths(get_option('prefix'), 'share', 'applications'),
 rename     : 'io.github.supertoq.finden.desktop')
# install_data(join_paths(data_dir, 'io.github.supertoq.miniterm.desktop.download'),
# install_dir: join_paths(get_option('prefix'), 'share', 'applications'),
# rename     : 'io.github.supertoq.miniterm.desktop')

# Localization files
# EN_US 
po_file_en_US = join_paths(po_dir, 'supertoq-finden_en_US.po')
mo_file = 'supertoq-finden.mo'
run_command('msgfmt', po_file_en_US, '--output-file=' + join_paths('en_US.mo'), check : true)
install_data(join_paths( 'en_US.mo'),
  install_dir: join_paths(get_option('prefix'), 'share', 'locale', 'en_US', 'LC_MESSAGES'),
  rename: mo_file
)
# EN_GB
po_file_en_GB = join_paths(po_dir, 'supertoq-finden_en_GB.po')
mo_file = 'supertoq-finden.mo'
run_command('msgfmt', po_file_en_GB, '--output-file=' + join_paths('en_GB.mo'), check : true)
install_data(join_paths( 'en_GB.mo'),
  install_dir: join_paths(get_option('prefix'), 'share', 'locale', 'en_GB', 'LC_MESSAGES'),
  rename: mo_file
)
# PL
po_file_pl = join_paths(po_dir, 'supertoq-finden_pl.po')
run_command('msgfmt', po_file_pl, '--output-file=' + join_paths('pl.mo'), check : true)
install_data(join_paths( 'pl.mo'),
  install_dir: join_paths(get_option('prefix'), 'share', 'locale', 'pl', 'LC_MESSAGES'),
  rename: mo_file
)

# Update Cache
gnome.post_install( gtk_update_icon_cache: true, update_desktop_database: true,)

