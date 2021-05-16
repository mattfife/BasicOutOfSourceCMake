# BasicOutOfSourceCMake

## Introduction
Out of source builds are often not explained very simply and examples tend to get overly complex very quickly. I have tried to create just about the absolute minimal out of source CMake example here. This works with both Visual Studio and Linux. There are countless enhancements that can be made to this simplified version, but this is a great starting point.

## Out of Source Builds
In case you're not familiar with out of source builds, traditional build systems tend to compile their intermediate binary files (obj files, libs, executables, dlls, etc) right next to their source files. This means binary and source files are mixed all over in the source tree. Out of source builds, however, put all binary and intermediate files in their own directory (often a bin/ directory) and keep them all out of the source (often src/) directory. As builds become complex, out of source builds have serious advantages. Some of them are:
1. Source control is made easier - especially for git. No need for complex .gitignore rules to avoid intermediate files and accidently miss key resource/source files.
2. It is very easy to clean up intermediate files and track down build issues by simply deleting the binary output directory and starting over.
3. Text and binary searches in the source tree go faster because they are only searching the source files and don't have false hits on binary files.
4. Creating different builds (cross-compiling, 64/32 bit, debug/release, etc) is cleaner and less error prone 
5. Easier to generate output packages that don't accidently include sources.

And countless others. It's benefits really pay off as your project grows.

## Instructions for Visual Studio
1. Ensure you have Visual Studio 2019 along with the 'Desktop development with C++' package installed. You can install that in the Visual Studio Installer and ensure the package is selected:
![image](https://user-images.githubusercontent.com/11483217/118386182-71ab3900-b5ca-11eb-854f-85b6a7d57f69.png)

2. From the start menu, open the Developer Command Prompt for VS2019
3. Download this source to a convinient local directory and cd into that directory
4. Run build.bat
![image](https://user-images.githubusercontent.com/11483217/118386271-26ddf100-b5cb-11eb-95d9-0447ae697b2b.png)
5. Open Visual Studio 2019
6. Open the newly created helloworld.sln found in the bin/ directory. 
7. Build like normal
8. If you need to add files to the CMake, you'll want to add them to the CMakeFile.txt, close the project in Visual Studio, run the build script again, then re-open the solution in Visual Studio.

## Instructions for Linux
1. Ensure that you have CMake, git, gcc, and other build tools you'd like. Installing the build-essential package contains most key build tools.
2. Open a terminal and download the source to a convinient local directory. 
3. run ./build.sh in the source directory
4. cd bin/
5. make
![image](https://user-images.githubusercontent.com/11483217/118386467-7ffa5480-b5cc-11eb-84d9-9b85937e7919.png)

## Optimizations
Versions of CMake past 3.13 support parameters that can help you make out-of-source builds more directly (without using the build script) by using the flags:
cmake -H. -Bbuild 
