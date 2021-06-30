cmake_minimum_required(VERSION 3.15 FATAL_ERROR)

# Global configs
#set(CMAKE_DEBUG_POSTFIX "")

set(DEFINITIONS)
list(APPEND DEFINITIONS -DGLEW_NO_GLU -DIMGUI_IMPL_OPENGL_LOADER_GLEW)

# CMake options
option(BUILD_SHARED_LIBS "Build shared." OFF)
option(IMGUI_BUILD_EXAMPLES "Build examples" OFF)

# Project config
set(IMGUI_PROJECT_NAME "imgui")
# Target Config
set(IMGUI_TARGET_NAME "${IMGUI_PROJECT_NAME}")

if (WIN32)
	set(CMAKE_USE_RELATIVE_PATHS ON)
endif ()

# Modules
list(INSERT CMAKE_MODULE_PATH 0 "${CMAKE_CURRENT_LIST_DIR}/cmake")

# Project setup
project(
		${IMGUI_PROJECT_NAME}
		LANGUAGES C CXX
)

# Vendor

# Target
add_library(
		${IMGUI_TARGET_NAME}

		imgui.h
		imgui.cpp

		imconfig.h

		imgui_internal.h

		imgui_draw.cpp
		imgui_demo.cpp
		imgui_widgets.cpp

		imgui_tables.cpp

		imstb_rectpack.h
		imstb_textedit.h
		imstb_truetype.h

		backends/imgui_impl_opengl3.h
		backends/imgui_impl_opengl3.cpp
		backends/imgui_impl_glfw.cpp
		backends/imgui_impl_glfw.cpp

)

add_library(
		${IMGUI_PROJECT_NAME}::${IMGUI_TARGET_NAME}
		ALIAS
		${IMGUI_TARGET_NAME}
)

target_include_directories(
		${IMGUI_TARGET_NAME}

		PUBLIC
		$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}>
		$<INSTALL_INTERFACE:include>

		${CMAKE_SOURCE_DIR}/vendor/glfw/include
		${CMAKE_SOURCE_DIR}/vendor/glew-cmake/include

		INTERFACE

		PRIVATE
)

target_compile_definitions(${IMGUI_TARGET_NAME} PUBLIC ${DEFINITIONS})

if (IMGUI_BUILD_EXAMPLES)
	add_subdirectory(examples)
endif ()