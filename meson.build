project(
  'havoc',
  'c',
  version : '0.6.0',
  meson_version : '>= 1.8.0',
  default_options: [ 'c_std=gnu99', 'warning_level=3' ],
)

compiler_flags = [
    '-Wno-unused-parameter',
    '-Wno-parentheses',
    '-Wno-format-overflow',
]

cc = meson.get_compiler('c')
add_project_arguments(
    cc.get_supported_arguments(compiler_flags),
    language: 'c'
)

add_project_arguments('-DVERSION="@0@"'.format(meson.project_version()), language : 'c')

xkbcommon = dependency('xkbcommon')
wayland_client = dependency('wayland-client')
wayland_cursor = dependency('wayland-cursor')

if host_machine.system() != 'linux'
    epoll_dep = dependency('epoll-shim')
else
    epoll_dep = []
endif

subdir('tsm')

wl = import('wayland')

executable('havoc',
    'glyph.c',
    'main.c',
    wl.scan_xml(wl.find_protocol('xdg-shell')),
    wl.scan_xml(wl.find_protocol('xdg-decoration', state: 'unstable', version: 1)),
    wl.scan_xml(wl.find_protocol('primary-selection', state: 'unstable', version: 1)),
    dependencies: [
        wayland_client,
        wayland_cursor,
        xkbcommon,
        epoll_dep,
    ],
    link_with: htsm,
)