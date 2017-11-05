# Crypto++ CMake

This repository contains CMake files for Wei Dai's Crypto++ (https://github.com/weidai11/cryptopp). It supplies `CMakeLists.txt` and `cryptopp-config.cmake` for Crypto++ for those who want to use CMake. CMake is officialy unsupported, so use it at your own risk.

The purpose of Crypto++ CMake is:
 
1. provide users with a centrally maintained CMake project files

The initial `cryptopp-config.cmake` and `CMakeLists.txt` were taken from the library sources when CMake support was officially dropped. Also see CMake on the Crypto++ wiki (https://www.cryptopp.com/wiki/CMake).

The CMake files are a work in progress, so use it at your own risk. Please feel free to make pull requests to fix problems.

# Workflow
The general workflow is clone Wei Dai's crypto++, add CMake as a submodule, and then copy the files of interest into the Crypto++ directory:

    git clone https://github.com/weidai11/cryptopp.git
    cd cryptopp
    git submodule add https://github.com/noloader/cryptopp-cmake.git cmake
    git submodule update --remote

    cp "$PWD/cmake/cryptopp-config.cmake" "$PWD"
    cp "$PWD/cmake/CMakeLists.txt" "$PWD"
    cp "$PWD/cmake/libcryptopp.pc.in" "$PWD"
    mkdir -p "$PWD/m4/"

To update the library and the submodule perform the following. The `make clean` is needed because reconfigure'ing does not invalidate the previously built objects or artifacts.

    cd cryptopp
    git pull
    git submodule update --remote

    cp "$PWD/cmake/cryptopp-config.cmake" "$PWD"
    cp "$PWD/cmake/CMakeLists.txt" "$PWD"
    cp "$PWD/cmake/libcryptopp.pc.in" "$PWD"

    make clean

Despite our efforts we have not been able to add the submodule to Crypto++ for seamless integration. If anyone knows how to add the submodule directly to the Crypto++ directory, then please provide the instructions.

# Integration
The CMake submodule integrates with the Crypto++ library. The library's `GNUmakefile` and `GNUmakefile-cross` were modified to clean the artifacts produced by CMake. To clean the directory after running CMake perform a `git checkout GNUmakefile` followed by a `make -f GNUmakefile distclean`.

# Collaboration
We would like all distro maintainers to be collaborators on this repo. If you are a distro maintainer then please contact us so we can send you an invite.

If you are a collaborator then make changes as you see fit. You don't need to ask for permission to make a change. Noloader is not an CMake expert so there are probably lots of opportunities for improvement.

Keep in mind other distros may be using the files, so try not to break things for the other guy. We have to be mindful of lesser-used platforms and compilers, like AIX, Solaris, IBM xlC and Oracle's SunCC.

Everything in this repo is release under Public Domain code. If the license or terms is unpalatable for you, then don't feel obligated to commit.

# Future
The CMake project files are separate at the moment for several reason, like avoiding Git log pollution with Autotool branch experiments. We also need to keep a logical separation because GNUmake is the official build system, and not the CMake project files.

Eventually we would like to do two things. First, we would like to move this project from Jeff Walton's GitHub to Wei Dai's GitHub to provide stronger assurances on provenance. Second, we would like to provide an `cmake.tar.gz` and place it in the Crypto++ `TestScripts/` directory to make it easier for folks to use CMake.