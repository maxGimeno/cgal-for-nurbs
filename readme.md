# Nurbs Mono Repo
This repository contains most of the necessary components for using Nurbs with CGAL. You will need access to 
`dtk-continuous-geometry`, `dtk-discrete-geometry`, `dtk-nurbs-probing` and `dtk-plugins-continuous-geometry` on the INRIA gitlab to use it.
## Dependencies
The dependencies that are not provided are the ones for CGAL. You can find them [here](https://doc.cgal.org/latest/Manual/thirdparty.html)
## Usage
 - In the root directory, enter `git submodule update --init` to clone all submodules.
 - Create a build directory, go in it : `mkdir build && cd build`
 - configure the project with BUILD_SHARED_LIBS=ON (because of sisl) : `cmake -DBUILD_SHARED_LIBS=ON ..`
 - Build the target `CGAL_NURBS` and run the demo : `make -j6 CGAL_NURBS && ./nurbs-viewer-mesher/Polyhedron/demo/Polyhedron/CGAL_NURBS`
