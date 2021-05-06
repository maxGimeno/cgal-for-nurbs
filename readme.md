# Nurbs Mono Repo
This repository contains most of the necessary components for using Nurbs with CGAL. You will need access to 
`dtk-continuous-geometry`, `dtk-discrete-geometry`, `dtk-nurbs-probing` and `dtk-plugins-continuous-geometry` on the INRIA gitlab to use it.
## Dependencies
The dependencies that are not provided are the ones for CGAL. You can find them [here](https://doc.cgal.org/latest/Manual/thirdparty.html)
There is also [TCL](https://www.tcl.tk/software/tcltk/bindist.html), [BLAS](http://www.netlib.org/blas) and [LAPACK](http://www.netlib.org/lapack/).
 
## Linux Usage

 - In the root directory, enter `git submodule update --init` to clone all submodules.
 - Create a build directory, go in it : `mkdir build && cd build`
 - configure the project with BUILD_SHARED_LIBS=ON (because of sisl) : `cmake -DBUILD_SHARED_LIBS=ON -DDTK_NURBS_PROBING_BUILD_APPS=ON ..`
 - For the GUI program, build the target `CGAL_NURBS` and run the demo : `make -j6 CGAL_NURBS && ./cgal/Polyhedron/demo/Polyhedron/CGAL_NURBS`
 - For the shell program, build the target `CGAL_NURBS_meshing` and run : `make -j6 dtkSeamGraphMeshing && ./dtk-nurbs-probing/bin/dtkSeamGraphMeshing <file-to-mesh>.3dm`

## Windows Usage (with cygwin)
 - install MKL using the online Intel oneAPI installer at https://software.intel.com/content/www/us/en/develop/tools/oneapi/base-toolkit/download.html?operatingsystem=window&distributions=webdownload
 - In the root directory of cgal-for-nurbs, enter `git submodule update --init` to update and clone all submodules.
 - create a build dir near the root to avoid too long paths errors : `mkdir C:/build-nurbs && cd C:/build-nurbs`
 - call <oneAPI_root_dir>/setvars.bat
 - call cmake with the following variables:
    CMAKE_PREFIX_PATH="<oneAPI_root_dir>/mkl/latest;<oneAPI_root_dir>/mkl/latest/lib/intel64;"(in cmake-gui, use +Add Entry -> STRING)
    BUILD_SHARED_LIBS=ON(in cmake-gui, use + Add Entry -> BOOL)
 - build the target `CGAL_NURBS` and/or `CGAL_NURBS_meshing`
 - call `BUILD_DIR=/cygdrive/c/build-nurbs/ && PATH=$BUILD_DIR/dtk-containers/bin/Release/:$BUILD_DIR/dtk-continuous-geometry/bin/Release/:$BUILD_DIR/dtk-core/bin/Release/:$BUILD_DIR/dtk-discrete-geometry/bin/Release/:$BUILD_DIR/dtk-log/bin/Release/:$BUILD_DIR/dtk-nurbs-probing/bin/Release/:$BUILD_DIR/dtk-plugins-continuous-geometry/bin/Release/:$BUILD_DIR/opennurbs/bin/Release/:$BUILD_DIR/SISL/Release/:$BUILD_DIR/cgal/Polyhedron/demo/Polyhedron/Plugins/Cad/Release/:$PATH`  to add all the created dll to the PATH
 - call `./cgal/Polyhedron/demo/Polyhedron/Release/CGAL_NURBS.exe` to launch the demo.
 - call `./dtk-nurbs-probing/bin/Release/dtkSeamGraphMeshing <file-to-mesh>.3dm` to launch the meshing program. 
