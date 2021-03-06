project(
	'grim',
	'c',
	version: '0.0.0',
	license: 'MIT',
	meson_version: '>=0.43.0',
	default_options: [
		'c_std=c11',
		'warning_level=2',
		'werror=true',
	],
)

add_project_arguments('-Wno-unused-parameter', language: 'c')

grim_inc = include_directories('include')

cc = meson.get_compiler('c')
cairo = dependency('cairo')
wayland_client = dependency('wayland-client')
wayland_protos = dependency('wayland-protocols', version: '>=1.14')
jpeg = dependency('libjpeg')
math = cc.find_library('m')

subdir('protocol')

executable(
	'grim',
	files([
		'box.c',
		'buffer.c',
		'cairo_jpg.c',
		'main.c',
		'output-layout.c',
		'render.c',
	]),
	dependencies: [
		cairo,
		client_protos,
		jpeg,
		math,
		wayland_client,
	],
	include_directories: [grim_inc],
	install: true,
)

scdoc = find_program('scdoc', required: false)

if scdoc.found()
	sh = find_program('sh')

	man_pages = ['grim.1.scd']

	mandir = get_option('mandir')

	foreach src : man_pages
		topic = src.split('.')[0]
		section = src.split('.')[1]
		output = '@0@.@1@'.format(topic, section)

		custom_target(
			output,
			input: src,
			output: output,
			command: [
				sh, '-c', '@0@ < @INPUT@ > @1@'.format(scdoc.path(), output)
			],
			install: true,
			install_dir: '@0@/man@1@'.format(mandir, section)
		)
	endforeach
endif
