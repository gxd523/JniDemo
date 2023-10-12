# For more information about using CMake with Android Studio, read the documentation:
# https://d.android.com/studio/projects/add-native-code.html.
# For more examples on how to use CMake, see https://github.com/android/ndk-samples.

# Sets the minimum CMake version required for this project.
# 设置生成native库所需的CMake的最低版本
cmake_minimum_required(VERSION 3.22.1)

# Declares the project name.
# The project name can be accessed via ${ PROJECT_NAME},
# Since this is the top level CMakeLists.txt,
# the project name is also accessible with ${CMAKE_PROJECT_NAME}
# (both CMake variables are in-sync within the top level build script scope).
project("jni")

# Creates and names a library, sets it as either STATIC
# or SHARED, and provides the relative paths to its source code.
# You can define multiple libraries, and CMake builds them for you.
# Gradle automatically packages shared libraries with your APK.
#
# In this top level CMakeLists.txt, ${CMAKE_PROJECT_NAME} is used to define
# the target library name; in the sub-module's CMakeLists.txt, ${PROJECT_NAME}
# is preferred for the same purpose.
#
# In order to load a library into your app from Java/Kotlin, you must call
# System.loadLibrary() and pass the name of the library defined here;
# for GameActivity/NativeActivity derived applications, the same library name must be
# used in the AndroidManifest.xml file.
# 编译动态库
add_library(
        # so文件名称libxxx.so，要和下面的target_link_libraries的第一个参数保持一致
        ${CMAKE_PROJECT_NAME}
        # SHARED打出的包为so文件, STATIC打出的包为a文件
        SHARED
        # List C/C++ source files with relative paths to this CMakeLists.txt.
        # 指定C++文件的相对路径
        native-lib.cpp
)

# 指定头文件路径
include_directories(src/main/cpp/inc/)

# Searches for a specified prebuilt library and stores the path as a variable.
# Because CMake includes system libraries in the search path by default,
# you only need to specify the name of the public NDK library you want to add.
# CMake verifies that the library exists before completing its build.
# 寻找动态库
find_library(
        # Sets the name of the path variable.
        log-lib
        # Specifies the name of the NDK library that you want CMake to locate.
        log
)

# Specifies libraries CMake should link to your target library.
# You can link libraries from various origins, such as libraries defined in this build script,
# prebuilt third-party libraries, or Android system libraries.
# 链接动态库
target_link_libraries(
        ${CMAKE_PROJECT_NAME}
        # List libraries link to the target library
        android
        log
)

# 设置so的输出路径
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../jniLibs/${CMAKE_ANDROID_ARCH_ABI})
#set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_SOURCE_DIR}/../jniLibs/${ANDROID_ABI})