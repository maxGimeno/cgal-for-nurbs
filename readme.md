# Nurbs Mono Repo
This repository contains most of the necessary components for using Nurbs with CGAL. You will need access to 
`dtk-continuous-geometry`, `dtk-discrete-geometry`, `dtk-nurbs-probing` and `dtk-plugins-continuous-geometry` on the INRIA gitlab to use it.
## Dependencies
The dependencies that are not provided are the ones for CGAL. You can find them [here](https://doc.cgal.org/latest/Manual/thirdparty.html)

**For Windows** usage with cygwin, there are extra steps to install BLAS and LAPACK:
 - install a recent version of mingw64 (from [this site, for example](http://mingw-w64.org/doku.php/download))
 - open mingw64.exe and type the following `pacman -S mingw64/mingw-w64-x86_64-make`
 - then type: `export PATH=C:\msys64\mingw64\x86_64-w64-mingw32\bin;C:\msys64\mingw64\bin;%PATH%`. Adapt the paths to your installation of mingw64. Note that the current PATH must not contain cygwin directories like bin, as it will probably conflict with mingw paths.
 - From here, you can follow the instructions at https://icl.cs.utk.edu/lapack-for-windows/lapack/
 
## Usage
 - In the root directory, enter `git submodule update --init` to clone all submodules.
 - Create a build directory, go in it : `mkdir build && cd build`
 - configure the project with BUILD_SHARED_LIBS=ON (because of sisl) : `cmake -DBUILD_SHARED_LIBS=ON ..`
 - Build the target `CGAL_NURBS` and run the demo : `make -j6 CGAL_NURBS && ./nurbs-viewer-mesher/Polyhedron/demo/Polyhedron/CGAL_NURBS`
