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
 - For the shell program, build the target `CGAL_NURBS_meshing` and run : `make -j6 CGAL_NURBS_meshing && ./dtk-nurbs-probing/bin/dtkSeamGraphMeshing <file-to-mesh>.3dm`

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

## Example Parameters
For testing with ABC data, one can call

    dtkSeamGraphMeshing -sharp_size=1 -smooth_size=0.1 -cell_size=5 -facet_distance=0.01`

## Regression Tests
For developers, there is a regression test suite, using CTest. The command-line to launch the regression testsuite is, for example:

    ctest -L regression_tests -C RelWithDebInfo -j6 --output-on-failure

Options:

  - `-C RelWithDebInfo` specifies the wanted build-type. This option is only required for a multi-configurations CMake generator (Visual Studio, XCode, Ninja Multi-config...). The build-type can also be:

    - `Release`, or
    - `Debug`.
  - `-j6` specifies that 6 test jobs can be run in parallel. Adapt to your CPU possibilities.
  - `--output-on-failure` instruct CTest to display the outputs of tests only in case of failure. If you want to always see outputs, use `-V` instead.

### Run Individual Tests
You can run a single test using a command line like this one, where `*step` is the name of the STEP file of the test, and `-V` is optional:

    ctest -R preprocess_serial_abc_0_335.step -V

If you have configured the project with TBB and the CMake option `CONCURRENT_MESH_3`, you can run the same test in parallel with the name `preprocess_parallel_abc_0_335.step` instead of `preprocess_serial_abc_0_335.step`.