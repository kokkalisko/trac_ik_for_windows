This repo intends to provide a Windows ported version of [**TRACLabs' IK solver**](https://bitbucket.org/traclabs/trac_ik/src/master/) for inverse kinematics. This repo has been tested with ROS melodic versions and contains all the updates up to trac_ik version 1.6.3. 

***Direct quote from the original repo:***

"The ROS packages were created to provide an alternative
Inverse Kinematics solver to the popular inverse Jacobian methods in KDL.
Specifically, KDL's convergence algorithms are based on Newton's method, which
does not work well in the presence of joint limits --- common for many robotic
platforms.  TRAC-IK concurrently runs two IK implementations.  One is a simple
extension to KDL's Newton-based convergence algorithm that detects and
mitigates local minima due to joint limits by random jumps.  The second is an
SQP (Sequential Quadratic Programming) nonlinear optimization approach which
uses quasi-Newton methods that better handle joint limits.  By default, the IK
search returns immediately when either of these algorithms converges to an
answer.  Secondary constraints of distance and manipulability are also provided 
in order to receive back the "best" IK solution.  
**Note:** TRAC-IK is built on top of the KDL library, which is not thread safe (there's some internals that I think use _static_ variables).  Thus, you should not use multiple instances of TRAC-IK in the same process."

This repo contains **4** ROS packages:

- trac\_ik is a metapackage with build and complete [Changelog](https://bitbucket.org/traclabs/trac_ik/src/HEAD/trac_ik/CHANGELOG.rst) info.

- trac\_ik\_examples contains examples on how to use the standalone TRAC-IK library.

- trac\_ik\_lib, the TRAC-IK kinematics code,
builds a .so library that can be used as a drop in replacement for KDL's IK
functions for KDL chains. Details for use are in trac\_ik\_lib/README.md.

- trac\_ik\_kinematics\_plugin builds a [MoveIt! plugin](http://moveit.ros.org/documentation/concepts/#kinematics) that can
replace the default KDL plugin for MoveIt! with TRAC-IK for use in planning.
Details for use are in trac\_ik\_kinematics\_plugin/README.md. (Note prior to v1.1.2, the plugin was not thread safe.)

In the original repo there is also: [trac\_ik\_python](https://bitbucket.org/traclabs/trac_ik/src/HEAD/trac_ik_python), a SWIG based python wrapper to use TRAC-IK. This was not possible to be ported due to missing dependencies for Windows. For any suggestions on the topic, please contact me.


## A detailed writeup on TRAC-IK:

[Humanoids-2015](https://www.researchgate.net/publication/282852814_TRAC-IK_An_Open-Source_Library_for_Improved_Solving_of_Generic_Inverse_Kinematics) (reported results are from v1.0.0 of TRAC-IK).

You can find the original code constantly updated [here](https://bitbucket.org/traclabs/trac_ik/src/master/). On the same page, you can find extensive results about the performance of this IK solution for numerous robots.

## Some guidelines for installation on Windows

One of the dependencies of these packages is [**NLopt**](https://nlopt.readthedocs.io/en/latest/). On Linux it is easy to fairly easy install this library, but on Windows **vcpkg** is probably the mean to achieve it. 

 1. For ROS on Windows vcpkg is already "installed" and can be found on the following path: 

> C:\opt\ros\melodic\x64\tools\vcpkg

This package manager can also be installed and bootstraped on another path (see [hee](https://github.com/microsoft/vcpkg))

2. Change directory to the path of vcpkg and install NLopt 

> vcpkg.exe install nlopt:x64-windows

3. Add the following path to your system PATH environmental variable:

>  %VCPKG_PATH%\installed\x64-windows\bin

As an alternative measure if NLopt is not found during building, uncomment and fix Line 18 of *trac_ik\CMakeLists.txt*

Feel free to [email me](mailto:kokkalisko@gmail.com) if you have any issues as well as suggestions for the Windows ROS package.
