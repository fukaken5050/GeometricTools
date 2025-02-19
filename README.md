## About ##

The Geometric Tools Engine (GTE) is a collection of source code for computing
in the fields of mathematics, geometry, graphics, image analysis and physics.
The engine is written in C++ 14 and supports high-performance computing using
CPU multithreading and general purpose GPU programming (GPGPU). Portions of
the code are described in various books as well as in PDF documents available
at the
[Geometric Tools Website](https://www.geometrictools.com). GTE is
licensed under the
[Boost Software License 1.0](https://www.boost.org/LICENSE_1_0.txt).
The update history for the current version of GTE is
[Current Update History](https://github.com/davideberly/GeometricTools/blob/master/GTE/Gte6UpdateHistory.pdf).
The update history for all versions of GTE is
[Full Update History](https://github.com/davideberly/GeometricTools/blob/master/GTE/GteFullUpdateHistory.pdf).

I am in the process of writing The Geometric Tools Library (GTL),
which is a major revision of GTE. A large portion of GTL development
has been porting code from GTE, dealing with size_t versus signed
integer compiler complaints, and adding new features. An equally
large portion has been writing unit tests and end-to-end tests for
the mathematics code. The project has taking a significant amount
of time and effort, but the completion is drawing near. GTL is also
licensed under the
[Boost Software License 1.0](https://www.boost.org/LICENSE_1_0.txt).

## Supported Platforms ##

The mathematics code is in a header-only library, GTMathematics. A
mathematics library with GPU-based implementations is provided,
GTMathematicsGPU. The CPU-based common graphics engine code is in its
own library, GTGraphics. DirectX 11 wrappers are provided for graphics
and applications, GTGraphicsDX11 and GTApplicationsDX11, on Microsoft
Windows 10/11. OpenGL 4.5 wrappers are provided for graphics and
applications, GTGraphicsGL45 and GTApplicationsGL45, on Microsoft
Windows 10/11 and on Linux. A small number of files exist to use GLX
and X-Windows on Linux.

On Microsoft Windows 10/11, the code is maintained using Microsoft Visual
Studio 2019/2022 with Microsoft's compilers, LLVM clang-cl or with Intel C++
Compiler 2023.

On Ubuntu 22.04.1 LTS, the code is maintained using Visual Studio Code
1.49.2 and CMake 3.15.2, NVIDIA graphics drivers, OpenGL 4.5 and
gcc 11.3.0, 

On Fedora 37, the code is maintained using Visual Studio Code 1.49.2
and CMake 3.22.2, NVIDIA graphics drivers, OpenGL 4.5 and
gcc 12.2.1.

## Getting Started ##

The repository contains many sample applications to illustrate some
features of the engine. Top-level solutions/makefiles exist to build
everything in the repository. Please read the
[Installation and Release Notes](https://github.com/davideberly/GeometricTools/blob/master/GTE/Gte6p5InstallationRelease.pdf)
to understand what is expected of your development environment.

## Known 3rd Party Problems ##
I have had several known problems with compilers I use for testing.
* The "Intel C++ Compiler Classic" is version 19.2 of the compiler. This is
  now marked as deprecated, so if you select this for the platform toolset to
  use in Microsoft Visual Studio, you will see deprecation warnings. I had
  problems with the integration of Intel C++ Compiler 2022 into Microsoft
  Visual Studio 2022. You enable this compiler by selecting "Intel DPC++/C++
  Compiler 2022" as the platform toolset. Warnings occur about macro
  redefinition, and because I have "treat warnings as errors", the code does
  not compile. The problem occurred using Intel oneAPI version 2022.3. This
  has been fixed in Intel oneAPI version 2023.0 where the platform toolset
  is now "Intel C++ Compiler 2023". It is possible to enable the DPC++
  compiler, but not through the MSVS dialogs. I have a tool
  GeometricTools/Tools/ChangePlatformToolset that will do a batch replacement
  of the platform toolset in the vcxproj files. The command-line parameter
  for the DPC++ platform toolset is "Intel(R) oneAPI DPC++ Compiler 2023".
  However, this compiler does not support __stdcall, so on a Windows machine
  you can compile only non-Windows-API code (no OpenGL/WGL and no Windows SDK).
  You can compile the mathematics code you need in your application by
  generating a library using DPC++ and then linking/loading this into an
  application that has been compiled with the Intel C++ Compiler 2023.

* The Microsoft Visual Studio 2022 Debugger was broken in versions 17.4.x
  where x < 5. See the [Developer Community explanation](https://developercommunity.visualstudio.com/t/VC-174-has-a-problem-with-debugger-wa/10195269)
  for details. The work-around is mentioned there, adding /Zc:nrvo- to the option
  Configuration | C++ | Command Line. The problem has been fixed as of
  version 17.4.5.

* After installing NVIDIA driver version 531.18 on a Windows 11 machine with
  Microsoft Visual Studio 17.5.1, all DirectX 11 samples throw 2 exceptions
  in D3D11CreateDevice, both tagged as Poco::NotFoundException. If you continue
  execution after these exceptions, the applications perform correctly. If
  you then click the application Close icon, another Poco::NotFoundException
  is generated. You can continue execution after this exception. I have filed
  a report with NVIDIA, providing a minimal-code reproduction. NOTE: Versions
  531.29, 531.41 and 531.61 of the driver were released and have the same
  problem.
 
## Links to GTE-Based Projects ##
* Seb Wouter's improvement for my LCP-based test-intersection query between
  a box and a finite cylinder. When using floating-point arithmetic, the LCP
  solver had some significant rounding errors to produce incorrect results.
  His approach clips the box by the cylinder enddisk planes to form a convex
  polyhedron, projects that to a plane perpendicular to the cylinder axis to
  obtain a polygon, and then tests for polygon-circle intersection. The code
  uses a generic convex hull finder to create the convex polyhedron. My current
  GTE code avoids the generic hull finder and takes advantage of the pairs of
  parallel planes for the box to amortize the computational costs for a faster
  algorithm. The repository link is
  https://github.com/SebWouters/AabbCyl

* CodeReclaimers example of incorporating Dear ImGui into a GTE sample
  application (for Microsoft Windows). The repository link is
  https://github.com/CodeReclaimers/GTEImGuiExample
