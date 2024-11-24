# EmptyIncludeFolder

This is a minimal example showing what I believe is a bug in VS2022 Dev Containers.

In VSCode in Windows 11, this project builds without error in "local machine" and "dev container."

But in Visual Studio 2022 with linux embedded tools, (which is what VSCode is invoking after all) the CMake configuration works, but the build fails in dev container only.

A little investigation relveals that these "local machine" folders are populated:

`EmptyIncludeFolder\out\build\x64-debug\_deps\asio-src\asio\include`
`EmptyIncludeFolder\out\build\x64-debug\_deps\json-src\include`

But the analogous "dev container" folders are mysteriously empty...

`EmptyIncludeFolder\out\build\linux-debug\_deps\asio-src\asio\include`
`EmptyIncludeFolder\out\build\linux-debug\_deps\json-src\include`

...but they are not empty when I use VSCode, nor when I run on external hardware over build-on ssh connection via VS2022.

In VS2022 dev container, CMake 'delete cache and reconfigure' runs without error:

```
1> Copying files to the remote machine.
1> Finished copying files (elapsed time 00h:00m:01s:312ms).
1> CMake generation started for configuration: 'linux-debug'.
1> Found cmake executable at /usr/bin/cmake.
1> /usr/bin/cmake -G "Ninja"   -DCMAKE_BUILD_TYPE:STRING="Debug" -DCMAKE_INSTALL_PREFIX:PATH="/workspaces/EmptyIncludeFolder/out/install/linux-debug"   /workspaces/EmptyIncludeFolder/CMakeLists.txt;
1> [CMake] -- The CXX compiler identification is GNU 10.2.1
1> [CMake] -- Detecting CXX compiler ABI info
1> [CMake] -- Detecting CXX compiler ABI info - done
1> [CMake] -- Check for working CXX compiler: /usr/bin/c++ - skipped
1> [CMake] -- Detecting CXX compile features
1> [CMake] -- Detecting CXX compile features - done
1> [CMake] -- Looking for C++ include pthread.h
1> [CMake] -- Looking for C++ include pthread.h - found
1> [CMake] -- Performing Test CMAKE_HAVE_LIBC_PTHREAD
1> [CMake] -- Performing Test CMAKE_HAVE_LIBC_PTHREAD - Failed
1> [CMake] -- Looking for pthread_create in pthreads
1> [CMake] -- Looking for pthread_create in pthreads - not found
1> [CMake] -- Looking for pthread_create in pthread
1> [CMake] -- Looking for pthread_create in pthread - found
1> [CMake] -- Found Threads: TRUE  
1> [CMake] -- Using the multi-header code from /workspaces/EmptyIncludeFolder/out/build/linux-debug/_deps/json-src/include/
1> [CMake] -- Configuring done
1> [CMake] -- Generating done
1> [CMake] -- Build files have been written to: /workspaces/EmptyIncludeFolder/out/build/linux-debug
1> [CMake] 
1> Extracted CMake variables.
1> Extracted source files and headers.
1> Extracted code model.
1> Extracted includes paths.
1> CMake generation finished.
```

But the build fails becuase the files are not found.

```
 cd /workspaces/EmptyIncludeFolder/out/build/linux-debug;/usr/bin/cmake --build /workspaces/EmptyIncludeFolder/out/build/linux-debug --config Debug;

[1/2] Building CXX object CMakeFiles/Server.dir/main.cpp.o
FAILED: CMakeFiles/Server.dir/main.cpp.o 
/usr/bin/c++  -I_deps/asio-src/asio/include -I_deps/json-src/include -g -std=gnu++2a -MD -MT CMakeFiles/Server.dir/main.cpp.o -MF CMakeFiles/Server.dir/main.cpp.o.d -o CMakeFiles/Server.dir/main.cpp.o -c ../../../main.cpp
../../../main.cpp:1:10: fatal error: asio.hpp: No such file or directory
    1 | ﻿#include <asio.hpp>
      |       ^~~~~~~~~~
compilation terminated.
ninja: build stopped: subcommand failed.


Build All failed.
```


About Visual Studio:
```
Microsoft Visual Studio Community 2022
Version 17.11.5
VisualStudio.17.Release/17.11.5+35327.3
Microsoft .NET Framework
Version 4.8.09032

Installed Version: Community

Visual C++ 2022   00482-90000-00000-AA945
Microsoft Visual C++ 2022

ASP.NET and Web Tools   17.11.231.19466
ASP.NET and Web Tools

Azure App Service Tools v3.0.0   17.11.231.19466
Azure App Service Tools v3.0.0

C# Tools   4.11.0-3.24460.3+5649376e0e5f5db3743a94a62b073f2cce4be5d9
C# components used in the IDE. Depending on your project type and settings, a different version of the compiler may be used.

GitHub Copilot   0.2.1657.32929
GitHub Copilot is an AI pair programmer that helps you write code faster and with less work.

Linux Core Dump Debugging   1.0.9.35103
Enables debugging of Linux core dumps.

Microsoft JVM Debugger   1.0
Provides support for connecting the Visual Studio debugger to JDWP compatible Java Virtual Machines

NuGet Package Manager   6.11.1
NuGet Package Manager in Visual Studio. For more information about NuGet, visit https://docs.nuget.org/

Test Adapter for Boost.Test   1.0
Enables Visual Studio's testing tools with unit tests written for Boost.Test.  The use terms and Third Party Notices are available in the extension installation directory.

Test Adapter for Google Test   1.0
Enables Visual Studio's testing tools with unit tests written for Google Test.  The use terms and Third Party Notices are available in the extension installation directory.

TypeScript Tools   17.0.30715.2002
TypeScript Tools for Microsoft Visual Studio

Visual Basic Tools   4.11.0-3.24460.3+5649376e0e5f5db3743a94a62b073f2cce4be5d9
Visual Basic components used in the IDE. Depending on your project type and settings, a different version of the compiler may be used.

Visual C++ for Linux Development   1.0.9.35103
Visual C++ for Linux Development

Visual Studio IntelliCode   2.2
AI-assisted development for Visual Studio.
```