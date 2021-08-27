# BasicOutOfSourceCMake

## Introduction
The topic of out of source builds is often not explained simply and examples tend to get overly complex too quickly. I have tried to create the most minimal out of source CMake example here. This works with both Visual Studio and Linux. There are countless enhancements that can be made to this simplified version, but this is a great starting point.

## Out of Source Builds
In case you're not familiar with out of source builds, traditional build systems tend to compile their intermediate binary files (obj files, libs, executables, dlls, etc) right next to their source files. This means binary and source files are mixed all over in the source tree. Out of source builds, however, put all binary and intermediate files in their own directory (often a bin/ directory) and keep them all out of the source (often src/) directory.  
![image](https://user-images.githubusercontent.com/11483217/118408935-c5ef0100-b63c-11eb-9bcc-49f0666f5c02.png)

As builds become complex, out of source builds have serious advantages. Some of them are:
1. Source control is made easier - especially for git. No need for complex .gitignore rules to avoid intermediate files and accidently miss key resource/source files.
2. It is very easy to clean up intermediate files and track down build issues by simply deleting the binary output directory and starting over.
3. Text and binary searches in the source tree go faster because they are only searching the source files and don't have false hits on binary files.
4. Creating different builds (cross-compiling, 64/32 bit, debug/release, etc) is cleaner and less error prone 
5. Easier to generate output packages that don't accidently include sources.

And countless others. It's benefits really pay off as your project grows.


## Instructions for Windows/Visual Studio 2019
1. Ensure you have Visual Studio 2019 along with the "Desktop development with C++" package installed. You can install that in the Visual Studio Installer and ensure the package is selected:
![image](https://user-images.githubusercontent.com/11483217/118386182-71ab3900-b5ca-11eb-854f-85b6a7d57f69.png)

2. From the start menu, open the Developer Command Prompt for VS2019
3. Download this source to a convenient local directory and cd into that directory
4. Run `build.bat`
![image](https://user-images.githubusercontent.com/11483217/118386271-26ddf100-b5cb-11eb-95d9-0447ae697b2b.png)
5. This builds the project and make files in `...\bin\`. To build the actual project/binaries, you have 3 options:

### Build binaries with cmake
6. After the projects are created in ...\bin\, you can then run this command from the ...\bin\ directory where the project files were created from step 4
7. `cmake --build . --target ALL_BUILD --config Release -- /nologo /verbosity:minimal /maxcpucount`
![cmake2](https://user-images.githubusercontent.com/11483217/131196407-ffc079e4-0b4e-4cbd-b539-d9040923f187.png)

### Build binaries using msbuild
6. In the Developer Command Prompt for VS2019 window, in the bin\ directory, run: `msbuild helloworld.sln`
7. msbuild will build the debug version of helloworld.exe in `...\bin\src\Debug\`
8. Read up on [msbuild command line options](https://docs.microsoft.com/en-us/visualstudio/msbuild/walkthrough-using-msbuild?view=vs-2019#build-the-target) for building different configurations (debug/release, x64/x32, etc)
![msbuild](https://user-images.githubusercontent.com/11483217/131196449-9be9d9de-1925-414b-9138-b1a8f20972f4.png)

### Building in Visual Studio
6. Open Visual Studio 2019
7. Open the newly created helloworld.sln found in the bin/ directory. 
8. Build from the main Build top menu like normal

### Visual Studio project tips
- If you change any of your CMake files (ex: add/remove source files), you can build ZERO_CHECK, or re-run build.sh again. This we re-generate the solution and vsproj files.
- Windows CMake automatically makes 2 additional projects in your solution: ALL_BUILD and ZERO_CHECK.
  - ALL_BUILD will build all projects in the solution (except itself)
  - ZERO_CHECK will re-run CMAKE and generate all new vproj and sln files. This is handy if you change your CMakeFiles and need to re-run CMake.
  - You cannot generate a solution with CMake without an ALL_BUILD project, but you can prevent CMake from adding the ZERO_CHECK project by adding this line to your top-level CMakeLists.txt: `set(CMAKE_SUPPRESS_REGENERATION true)`

## Instructions for Linux
1. Ensure that you have CMake, git, gcc, and other build tools you'd like. Installing the build-essential package contains most key build tools.
2. Open a terminal and download the source to a convinient local directory. 
3. run `./build.sh` in the source directory
4. `cd bin/`
5. `make`
![image](https://user-images.githubusercontent.com/11483217/118386467-7ffa5480-b5cc-11eb-84d9-9b85937e7919.png)

## Optimizations
- Versions of CMake past 3.13 support parameters that can help you make out-of-source builds more directly (without using the build script) by using the flags:
`cmake -H. -Bbuild`

