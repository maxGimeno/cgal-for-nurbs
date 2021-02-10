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
#SISL
add_subdirectory("${CMAKE_SOURCE_DIR}/SISL")

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
add_subdirectory("${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher")
SET(CGAL_DIR "${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher" CACHE INTERNAL "")
#add CGAL to the export set of dtk-nurbs-probing
install(TARGETS CGAL
    DESTINATION ${CMAKE_INSTALL_LIBDIR}
    COMPONENT CGAL
    EXPORT dtkNurbsProbingDepends)

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
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-nurbs-probing")
get_property(DTK_NP_INCLUDE_DIRS DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/dtk-nurbs-probing PROPERTY INCLUDE_DIRECTORIES)
include_directories(${DTK_NP_INCLUDE_DIRS})
set(dtkNurbsProbing_FOUND TRUE CACHE INTERNAL "")
message("========== END CONFIGURE NURBS PROBING ==========")

#dtk-plugins-continuous-geometry
message("========== CONFIGURE PUGINS CG... ==========")
set(CMAKE_MODULE_PATH "${CMAKE_MODULE_PATH};${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry/cmake")
add_subdirectory("${CMAKE_SOURCE_DIR}/dtk-plugins-continuous-geometry")
message("========== END CONFIGURE PLUGINS CG==========")

message("========== CONFIGURE CGAL_NURBS... ==========")
add_definitions(-DDTK_PLUGINS_DIR=\"${CMAKE_BINARY_DIR}/dtk-plugins-continuous-geometry/lib\")
add_subdirectory("${CMAKE_SOURCE_DIR}/nurbs-viewer-mesher/Polyhedron/demo/Polyhedron")
message("========== END CONFIGURE CGAL_NURBS ==========")
add_dependencies(CGAL_NURBS
  dtkContinuousGeometryCore
  sisl
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
  )
