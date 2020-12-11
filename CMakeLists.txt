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
set(SISL_LIBRARY "${CMAKE_BINARY_DIR}/SISL/libsisl.so" CACHE INTERNAL "")
#opennurbs
add_subdirectory("${CMAKE_SOURCE_DIR}/opennurbs")
set(openNURBS_FOUND TRUE CACHE INTERNAL "")
#cgal
message("========== CONFIGURE CGAL... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher")
message("========== END CONFIGURE CGAL ==========")

### dtk

#dtk-discrete-geometry
message("========== CONFIGURE DISCRETE GEOMETRY... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-discrete-geometry")
set(dtkDiscreteGeometry_FOUND TRUE CACHE INTERNAL "")
message("========== END CONFIGURE DG ==========")
#dtk-continuous-geometry
message("========== CONFIGURE CONTINUOUS GEOMETRY... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-continuous-geometry")
get_property(DTK_CG_INCLUDE_DIRS DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dtk-continuous-geometry PROPERTY INCLUDE_DIRECTORIES)
include_directories(${DTK_CG_INCLUDE_DIRS})
message("========== END CONFIGURE CG ==========")

#dtk-nurbs-probing
message("========== CONFIGURE NURBS PROBING... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-nurbs-probing")
message("========== END CONFIGURE NURBS PROBING ==========")

#dtk-plugins-continuous-geometry
message("========== CONFIGURE PUGINS CG... ==========")
set(CGAL_HEADER_ONLY TRUE CACHE INTERNAL "")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry")
message("========== END CONFIGURE PLUGINS CG==========")
