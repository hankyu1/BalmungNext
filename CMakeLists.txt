cmake_minimum_required(VERSION 3.20)
project(BalmungNext)

set(CMAKE_CXX_STANDARD 23)

#set(IMGUI_PATH  "lib/imgui")
#
## GLFW
#set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
#set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
#set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
#add_subdirectory(lib/glfw)
#
## opengl
#find_package(OpenGL REQUIRED)
#
##ImGUI
## Compile as static library
#file(GLOB IMGUI_SOURCES ${IMGUI_PATH}/*.cpp)
#file(GLOB IMGUI_BACKEND
#    ${IMGUI_PATH}/backends/imgui_impl_opengl3_loader.h
#    ${IMGUI_PATH}/backends/imgui_impl_opengl3.h
#    ${IMGUI_PATH}/backends/imgui_impl_glfw.h
#    ${IMGUI_PATH}/backends/imgui_impl_opengl3.cpp
#    ${IMGUI_PATH}/backends/imgui_impl_glfw.cpp
#)
#add_library("ImGui" STATIC ${IMGUI_SOURCES} ${IMGUI_BACKEND})
#target_include_directories("ImGui" PUBLIC ${IMGUI_PATH})
#
#
#add_executable(BalmungNext main.cpp)
#target_link_libraries("BalmungNext" PUBLIC ImGui glfw OpenGL::GL)
#target_link_libraries("BalmungNext" glfw OpenGL::GL)

find_package ( OpenGL REQUIRED )
set ( PRJ_INC_LIST )
set ( PRJ_SRC_LIST )
set ( PRJ_LIBRARIES )
set ( PRJ_INCLUDE_DIRS )

set ( GLFW_HOME "lib/glfw" )
set ( GLAD_HOME "lib/glad" )
set ( IMGUI_HOME "lib/imgui" )

set ( GLFW_INC "${GLFW_HOME}/include" )
#set ( GLFW_LIB "${GLFW_HOME}/lib/glfw3dll.lib" )

## GLFW
set(GLFW_BUILD_DOCS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_TESTS OFF CACHE BOOL "" FORCE)
set(GLFW_BUILD_EXAMPLES OFF CACHE BOOL "" FORCE)
add_subdirectory(lib/glfw)

#set ( IMGUI_INC "${IMGUI_HOME}/include" )
set ( IMGUI_SRC "${IMGUI_HOME}" )
set ( IMGUI_BACKEND "${IMGUI_HOME}/backends" )

file ( GLOB root_header_files "${CMAKE_CURRENT_SOURCE_DIR}/*.h" )
file ( GLOB root_src_files "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp" )

file ( GLOB imgui_header_files "${IMGUI_SRC}/*.h" )
file ( GLOB imgui_src_files "${IMGUI_SRC}/*.cpp" )

list ( APPEND imgui_header_files "${IMGUI_BACKEND}/imgui_impl_opengl3_loader.h" )
list ( APPEND imgui_header_files "${IMGUI_BACKEND}/imgui_impl_opengl3.h" )
list ( APPEND imgui_header_files "${IMGUI_BACKEND}/imgui_impl_glfw.h" )

list ( APPEND imgui_src_files "${IMGUI_BACKEND}/imgui_impl_opengl3.cpp" )
list ( APPEND imgui_src_files "${IMGUI_BACKEND}/imgui_impl_glfw.cpp" )

list ( APPEND PRJ_INCLUDE_DIRS . )
list ( APPEND PRJ_INCLUDE_DIRS ${GLFW_INC} )
list ( APPEND PRJ_INCLUDE_DIRS ${IMGUI_SRC} )
list ( APPEND PRJ_INCLUDE_DIRS ${IMGUI_BACKEND} )

list ( APPEND PRJ_INC_LIST ${root_header_files} )
list ( APPEND PRJ_INC_LIST ${imgui_header_files} )

list ( APPEND PRJ_SRC_LIST ${root_src_files} )
list ( APPEND PRJ_SRC_LIST ${imgui_src_files} )

#list ( APPEND PRJ_LIBRARIES ${GLFW_LIB} )
list ( APPEND PRJ_LIBRARIES ${OPENGL_LIBRARIES} )

add_executable ( ${PROJECT_NAME} ${PRJ_SRC_LIST} ${PRJ_INC_LIST} )

target_include_directories ( ${PROJECT_NAME}
    PRIVATE
    ${PRJ_INCLUDE_DIRS}
)

target_link_libraries ( ${PROJECT_NAME}
    PRIVATE
    ${PRJ_LIBRARIES}
    glfw
    OpenGL::GL
)