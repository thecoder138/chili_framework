cmake_minimum_required (VERSION 3.0)

project(ChiliPy)

set(SOURCES 
"Game.cpp"
"Sound.cpp"
"Mouse.cpp"
"MainWindow.cpp"
"Main.cpp"
"Keyboard.cpp"
"Graphics.cpp"
"DXErr.cpp"
"COMInitializer.cpp"
"Resource.rc")

set(HEADERS
"ChiliException.h"
"ChiliWin.h"
"Colors.h"
"COMInitializer.h"
"DXErr.h"
"Game.h"
"Graphics.h"
"Keyboard.h"
"MainWindow.h"
"Mouse.h"
"Resource.h"
"Sound.h"
"SoundEffect.h")

if (${MSVC})
	set(PS_SHADER_SOURCES "FramebufferPS.hlsl")
	set(VS_SHADER_SOURCES "FramebufferVS.hlsl")
else (${MSVC})
	message("Shaders won't be compiled")
endif (${MSVC})

add_executable(Engine ${SOURCES} ${HEADERS} ${PS_SHADER_SOURCES} ${VS_SHADER_SOURCES})
add_definitions(-DUNICODE -D_UNICODE)
set_target_properties(Engine PROPERTIES LINK_FLAGS "/SUBSYSTEM:WINDOWS")
if (${MSVC})
	set_source_files_properties( ${PS_SHADER_SOURCES} PROPERTIES 
		VS_SHADER_TYPE Pixel 
		VS_SHADER_MODEL 4.0
		VS_SHADER_ENTRYPOINT FramebufferPS 
		VS_SHADER_FLAGS "/Fh\"${ChiliPy_SOURCE_DIR}\\FramebufferPS.shh\" /Vn\"FramebufferPSBytecode\""
	)
	set_source_files_properties( ${VS_SHADER_SOURCES} PROPERTIES 
		VS_SHADER_TYPE Vertex 
		VS_SHADER_MODEL 4.0
		VS_SHADER_ENTRYPOINT FramebufferVS 
		VS_SHADER_FLAGS "/Fh\"${ChiliPy_SOURCE_DIR}\\FramebufferVS.shh\" /Vn\"FramebufferVSBytecode\""
	)
endif (${MSVC})