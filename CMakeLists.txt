# Minimal version of CMake:
cmake_minimum_required(VERSION 3.1)

message( "== CMake setup ==" )
cmake_minimum_required(VERSION 3.1...3.15)
project(NurbsMeshing CXX C)

set(USES_SUBMODULES "ON")
include_directories(${CMAKE_BINARY_DIR})
add_subdirectory("${CMAKE_SOURCE_DIR}/blaze")
set(Blaze_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/blaze)
#SISL
add_subdirectory("${CMAKE_SOURCE_DIR}/SISL")
set(SISL_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/SISL" CACHE INTERNAL "")
set(SISL_LIBRARIES "${CMAKE_BINARY_DIR}/SISL/libsisl.so" CACHE INTERNAL "")
#opennurbs
add_subdirectory("${CMAKE_SOURCE_DIR}/opennurbs")


### dtk

#dtk-discrete-geometry
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-discrete-geometry")
#dtk-continuous-geometry
add_subdirectory("${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher")
set(CGAL_DIR "${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher" CACHE INTERNAL "")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-continuous-geometry")
#dtk-nurbs-probing

add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-nurbs-probing")

#dtk-plugins-continuous-geometry
set(CGAL_HEADER_ONLY TRUE CACHE INTERNAL "")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry")

