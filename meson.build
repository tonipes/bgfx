project('bgfx', 'c', 'cpp', version : '1.0.0', license: 'BSD 2-Clause', default_options : ['cpp_std=c++14'])

includes = [
	'include',
	'3rdparty',
	'3rdparty/khronos',
	'3rdparty/fcpp',
	'3rdparty/glslang/glslang/Public',
	'3rdparty/glslang/glslang/Include',
	'3rdparty/glsl-optimizer/include',
	'3rdparty/glsl-optimizer/src/glsl',
]

src = ['src/amalgamated.cpp']

bx_dep = dependency('bx', fallback: ['bx', 'bx_dep'])
bimg_dep = dependency('bimg', fallback: ['bimg', 'bimg_dep'])
dependencies = [bx_dep, bimg_dep]

objcpp_args = []
if host_machine.system() == 'darwin'
	add_languages('objcpp')
	src = ['src/amalgamated.mm']
	appleframeworks_dep = dependency('appleframeworks', modules : ['Metal', 'QuartzCore', 'Cocoa'])
	dependencies += appleframeworks_dep
	objcpp_args = ['-std=c++14']
endif

if host_machine.system() == 'linux'
  dependencies += dependency('GL')
  dependencies += dependency('X11')
endif

bgfx_lib = static_library(
  'libbgfx',
  src,
  include_directories : includes,
  dependencies        : dependencies,
  objcpp_args         : objcpp_args
)

bgfx_dep = declare_dependency(include_directories : includes, link_with: bgfx_lib, dependencies: dependencies)

subdir('3rdparty')

#if building for native, also build tools
if not meson.is_cross_build()
	subdir('tools')
endif
