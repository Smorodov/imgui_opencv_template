# ----------------------------------------------------------------------------
# 
# ----------------------------------------------------------------------------
cmake_minimum_required(VERSION 2.8)
set(CMAKE_BINARY_DIR ${CMAKE_SOURCE_DIR}/build)
set(EXECUTABLE_OUTPUT_PATH ${CMAKE_BINARY_DIR})
set(LIBRARY_OUTPUT_PATH ${CMAKE_BINARY_DIR}) 

add_subdirectory(zlib)
add_subdirectory(glfw)

set(project_name imgui_opencv_template)

project(${project_name})

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -fPIC -std=c++11")

FIND_PACKAGE(OpenCV REQUIRED)
#----------------------------------------------------------------------------  
# 
# ----------------------------------------------------------------------------
include_directories(main/nativefiledialog/src/include)
#----------------------------------------------------------------------------  
#  
# ----------------------------------------------------------------------------
SET(SOURCE
main/main.cpp
main/nativefiledialog/src/nfd_common.c
main/nativefiledialog/src/nfd_win.cpp
imgui/examples/libs/gl3w/GL/gl3w.c
imgui/examples/imgui_impl_glfw.cpp
imgui/examples/imgui_impl_opengl3.cpp
imgui/imgui.cpp
imgui/imgui_draw.cpp
imgui/imgui_widgets.cpp
)
#----------------------------------------------------------------------------  
#  
# ----------------------------------------------------------------------------
SET(HEADERS
main/nativefiledialog/src/include/nfd.h
main/imageutils.h
)
#----------------------------------------------------------------------------  
#  
# ----------------------------------------------------------------------------
file(GLOB ADD_HEADERS "./imgui/*.h*")
SET(HEADERS ${HEADERS} ${ADD_HEADERS})

SOURCE_GROUP("Source Files" FILES ${SOURCE})
SOURCE_GROUP("Header Files" FILES ${HEADERS})
#----------------------------------------------------------------------------  
#  
# ----------------------------------------------------------------------------
set(Boost_USE_STATIC_LIBS   ON)
set(Boost_USE_STATIC_RUNTIME   ON)
find_package(Boost 1.54 REQUIRED COMPONENTS system thread filesystem regex)
include_directories(${Boost_INCLUDE_DIRS})
link_directories(${Boost_LIBRARY_DIRS})
get_directory_property(_my_link_dirs LINK_DIRECTORIES)
message(STATUS "_my_link_dirs = ${_my_link_dirs}") 
# ----------------------------------------------------------------------------   
# 
# ----------------------------------------------------------------------------
INCLUDE_DIRECTORIES("./")
INCLUDE_DIRECTORIES("imgui")
INCLUDE_DIRECTORIES("imgui/examples")
INCLUDE_DIRECTORIES("imgui/examples/libs/gl3w")
INCLUDE_DIRECTORIES("imgui/examples/libs/glfw/include")
INCLUDE_DIRECTORIES("main")
INCLUDE_DIRECTORIES(${CMAKE_SOURCE_DIR}/zlib)
# ----------------------------------------------------------------------------  
# 
# ----------------------------------------------------------------------------
  ADD_EXECUTABLE( ${project_name}
  				  ${SOURCE}
  				  ${HEADERS}
  				  )
# ----------------------------------------------------------------------------
# 
# ----------------------------------------------------------------------------
TARGET_LINK_LIBRARIES(${project_name}
                      ${OpenCV_LIBS}
					  optimized ${CMAKE_BINARY_DIR}/Release/glfw3.lib
					  debug 	${CMAKE_BINARY_DIR}/Debug/glfw3.lib	
					  optimized ${CMAKE_BINARY_DIR}/Release/zlib.lib
					  debug     ${CMAKE_BINARY_DIR}/Debug/zlib.lib				  
					  opengl32.lib
					  )
# ----------------------------------------------------------------------------
# 
# ----------------------------------------------------------------------------
 file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/fonts/a_FuturaOrto.TTF DESTINATION ${CMAKE_BINARY_DIR}/Release/fonts)
 file(COPY ${CMAKE_CURRENT_SOURCE_DIR}/fonts/a_FuturaOrto.TTF DESTINATION ${CMAKE_BINARY_DIR}/Debug/fonts)