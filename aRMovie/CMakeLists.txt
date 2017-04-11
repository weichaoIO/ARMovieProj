# CMake版本信息
cmake_minimum_required(VERSION 3.4.1)

# 支持-std=gnu++11
set(CMAKE_VERBOSE_MAKEFILE on)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=gnu++11")


# Project 名称
set(projectName ARMovieProj)
# Module 名称
set(moduleName aRMovie)

# 工程路径
set(pathToProject D:/android_studio_workspace/${projectName})
# Native 路径
set(pathToNative ${pathToProject}/${moduleName}/src/main/cpp)

# 配置加载native依赖
include_directories(${pathToProject}/ARToolKit)
include_directories(${pathToProject}/ARToolKit/android)

# 添加待编译的文件
add_library(ARMovieNative SHARED ${pathToNative}/ARMarkerNFT.c
                                 ${pathToNative}/ARMarkerNFT.h
                                 ${pathToNative}/ARMovie.cpp
                                 ${pathToNative}/color_convert_common.c
                                 ${pathToNative}/color_convert_common.h
                                 ${pathToNative}/trackingSub.c
                                 ${pathToNative}/trackingSub.h)

# 动态方式加载
add_library(so_libc++_shared SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libc++_shared PROPERTIES IMPORTED_LOCATION ${pathToProject}/${moduleName}/src/main/jniLibs/${ANDROID_ABI}/libc++_shared.so)

# 动态方式加载
add_library(so_libARMovieNative SHARED IMPORTED)
# 引入so文件
set_target_properties(so_libARMovieNative PROPERTIES IMPORTED_LOCATION ${pathToProject}/${moduleName}/src/main/jniLibs/${ANDROID_ABI}/libARMovieNative.so)

target_link_libraries(ARMovieNative
                      so_libc++_shared so_libARMovieNative
                      log GLESv1_CM z)