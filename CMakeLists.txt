# Minimal version of CMake:
cmake_minimum_required(VERSION 3.1)

message( "== CMake setup ==" )
cmake_minimum_required(VERSION 3.1...3.15)
project(NurbsMeshing CXX C)

enable_testing()

set(USES_SUBMODULES "ON")
include_directories(${CMAKE_BINARY_DIR})
add_subdirectory("${CMAKE_SOURCE_DIR}/blaze")
set(Blaze_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/blaze CACHE INTERNAL "")

#OpenCASCADE
if(NOT CGAL_FOR_NURBS_USE_SYSTEM_OCCT)
  set(USE_TK FALSE CACHE INTERNAL "")
  if(WIN32)
    set(USE_FREETYPE FALSE CACHE INTERNAL "")
  else()
    set(USE_FREETYPE TRUE CACHE INTERNAL "")
  endif()
  add_subdirectory("${CMAKE_SOURCE_DIR}/OCCT")
  get_property(OCCT_INCLUDE_DIRS DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/OCCT PROPERTY INCLUDE_DIRECTORIES)
  include_directories(${OCCT_INCLUDE_DIRS})
endif()
#SISL
add_subdirectory("${CMAKE_SOURCE_DIR}/SISL")
if(WIN32)
  target_compile_definitions(sisl PUBLIC SISLNEEDPROTOTYPES)
endif()
set(SISL_INCLUDE_DIR "${CMAKE_SOURCE_DIR}/SISL/include" CACHE INTERNAL "")
set(SISL_LIBRARY "sisl" CACHE INTERNAL "")


#dtk-log
message("========== CONFIGURE DTK LOG... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-log")

message(" ========= Done. ==========")

#dtk-core
message("========== CONFIGURE DTK CORE... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-core")
get_target_property(dtkCore_VERSION dtkCore VERSION)

message("========== Done. ==========")

#dtk-containers
message("========== CONFIGURE DTK CONTAINERS... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-containers")

message("========== Done. ==========")
#opennurbs
add_subdirectory("${CMAKE_SOURCE_DIR}/opennurbs")
include_directories(${CMAKE_SOURCE_DIR}/opennurbs)

set(openNURBS_FOUND TRUE CACHE INTERNAL "")

#cgal
message("========== CONFIGURE CGAL... ==========")
#SET(WITH_demos TRUE CACHE INTERNAL "")
set(CGAL_HEADER_ONLY TRUE CACHE INTERNAL "")
add_subdirectory("${CMAKE_SOURCE_DIR}/cgal")
SET(CGAL_DIR "${CMAKE_SOURCE_DIR}/cgal" CACHE INTERNAL "")
#add CGAL to the export set of dtk-nurbs-probing
if(TARGET CGAL)
  install(TARGETS CGAL
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    COMPONENT CGAL
    EXPORT dtkNurbsProbingDepends)
endif()

message("========== END CONFIGURE CGAL ==========")

### dtk

#dtk-discrete-geometry
message("========== CONFIGURE DISCRETE GEOMETRY... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-discrete-geometry")
set(dtkDiscreteGeometry_FOUND TRUE CACHE INTERNAL "")
get_property(DTK_DG_INCLUDE_DIRS DIRECTORY ${CMAKE_SOURCE_DIR}/dtk-discrete-geometry PROPERTY INCLUDE_DIRECTORIES)
include_directories(${DTK_DG_INCLUDE_DIRS})
message("========== END CONFIGURE DG ==========")


#dtk-continuous-geometry
message("========== CONFIGURE CONTINUOUS GEOMETRY... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-continuous-geometry")
get_property(DTK_CG_INCLUDE_DIRS DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dtk-continuous-geometry PROPERTY INCLUDE_DIRECTORIES)
include_directories(${DTK_CG_INCLUDE_DIRS})
set(dtkContinuousGeometry_FOUND TRUE CACHE INTERNAL "")
message("========== END CONFIGURE CG ==========")

#dtk-nurbs-probing
message("========== CONFIGURE NURBS PROBING... ==========")
set(DTK_NURBS_PROBING_BUILD_MREP TRUE CACHE INTERNAL "")
set(DTK_NURBS_PROBING_BUILD_APPS TRUE CACHE INTERNAL "")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-nurbs-probing")
get_property(DTK_NP_INCLUDE_DIRS DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dtk-nurbs-probing PROPERTY INCLUDE_DIRECTORIES)
include_directories(${DTK_NP_INCLUDE_DIRS})
set(dtkNurbsProbing_FOUND TRUE CACHE INTERNAL "")
message("========== END CONFIGURE NURBS PROBING ==========")

#dtk-plugins-continuous-geometry
message("========== CONFIGURE PLUGINS CONTINUOUS GEOMETRY... ==========")
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry/cmake")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry")
message("========== END CONFIGURE PLUGINS CONTINUOUS GEOMETRY =========")

include(GNUInstallDirs)
get_property(IS_MULTI_CONFIG GLOBAL PROPERTY GENERATOR_IS_MULTI_CONFIG)
if(IS_MULTI_CONFIG)
  set(CONFIG_DIR "/$<CONFIG>")
else()
  set(CONFIG_DIR "")
endif()
if(WIN32)
  set(OUTPUT_DIR "${CMAKE_INSTALL_BINDIR}")
else()
  set(OUTPUT_DIR "${CMAKE_INSTALL_LIBDIR}")
endif()
set(DTK_CONTINUOUS_GEOMETRY_PLUGINS_PATH "${dtk-plugins-continuous-geometry_BINARY_DIR}/${OUTPUT_DIR}${CONFIG_DIR}" CACHE INTERNAL "Path to dtk-continuous-geometry plugins" FORCE)
target_compile_definitions(dtkContinuousGeometryCore PRIVATE DTK_CONTINUOUS_GEOMETRY_PLUGINS_PATH=${DTK_CONTINUOUS_GEOMETRY_PLUGINS_PATH})

message("========== CONFIGURE CGAL_NURBS... ==========")
add_subdirectory("${CMAKE_SOURCE_DIR}/cgal/Polyhedron/demo/Polyhedron" EXCLUDE_FROM_ALL)
message("========== END CONFIGURE CGAL_NURBS ==========")
set (LIST_OF_DEPENDENCIES sisl
  blazeRationalBezierCurve2DElevator
  dtkNurbsPolyhedralSurfaceCgal
  dtkRationalBezierCurveDataSisl
  npRationalBezierCurveSphereIntersector
  sislRationalBezierSurfaceCircleIntersector
  blazeRationalBezierCurveLossLessDegreeReductor
  dtkNurbsPolyhedralSurfaceDefault
  dtkRationalBezierSurfaceDataDefault
  openNURBSBRepReader
  sislRationalBezierSurfaceLineIntersector
  defaultRationalBezierCurve2DElevator
  dtkNurbsSurfaceDataDefault
  dtkRationalBezierSurfaceDataOn
  sislNurbsSurfaceRayIntersector
  sislRationalBezierSurfacePointIntersector
  defaultRationalBezierCurveLossLessDegreeReductor
  dtkNurbsSurfaceDataOn
  dtkRationalBezierSurfaceDataSisl
  sislRationalBezierCurve2DLineIntersector
  sislRationalBezierSurfaceRayIntersector
  dtkBRepReaderCgal
  dtkNurbsSurfaceDataSisl
  dtkTrimLoopDataCgal
  sislRationalBezierCurve2DSegmentIntersector
  sislRationalBezierSurfaceSegmentIntersector
  dtkNurbsCurve2DDataOn
  dtkRationalBezierCurve2DDataOn
  dtkTrimLoopDataNp
  sislRationalBezierCurvePlaneIntersector
  dtkNurbsCurveDataOn
  dtkRationalBezierCurveDataOn
  npNurbsCurve2DCircleIntersector
  sislRationalBezierCurveSphereIntersector
  dtkNurbsCurve2DDataOCCT
  dtkNurbsCurveDataOCCT
  dtkNurbsSurfaceDataOCCT
  dtkRationalBezierCurve2DDataOCCT
  dtkRationalBezierCurveDataOCCT
  dtkRationalBezierSurfaceDataOCCT
  OpenCASCADEBRepReader)

  if(TARGET CGAL_NURBS)
  add_dependencies(CGAL_NURBS
    dtkContinuousGeometryCore
    ${LIST_OF_DEPENDENCIES})

  option(CGAL_BUILD_NURBS_DEMO "Build the 3D demo to load NURBS object" ON)
  if(CGAL_BUILD_NURBS_DEMO)
    add_custom_target(CGAL_NURBS_demo ALL)
    add_dependencies(CGAL_NURBS_demo CGAL_NURBS)
  endif()
endif() #CGAL_NURBS

add_custom_target(CGAL_NURBS_meshing ALL)
add_dependencies(CGAL_NURBS_meshing
  dtkSeamGraphMeshing
  ${LIST_OF_DEPENDENCIES})
