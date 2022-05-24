# For more information about using CMake with Android Studio, read the
# documentation: https://d.android.com/studio/projects/add-native-code.html

# Sets the minimum version of CMake required to build the native library.

cmake_minimum_required(VERSION 3.18.1)

project("tolyffmpeg")

#引入头文件
include_directories(includes)
# 定义当前 so 库 - 在 java 代码中加载
add_library(tolyffmpeg SHARED native-lib.cpp)

# 添加 ffmpeg 的 avcodec、swresample、avutil 模块
set(distribution_DIR ${CMAKE_SOURCE_DIR}/../jniLibs/${ANDROID_ABI})

add_library(avcodec SHARED IMPORTED)
set_target_properties(avcodec PROPERTIES IMPORTED_LOCATION ${distribution_DIR}/libavcodec.so)

add_library(swresample SHARED IMPORTED)
set_target_properties(swresample PROPERTIES IMPORTED_LOCATION ${distribution_DIR}/libswresample.so)

add_library(avutil SHARED IMPORTED)
set_target_properties(avutil PROPERTIES IMPORTED_LOCATION ${distribution_DIR}/libavutil.so)


find_library(log-lib log)
target_link_libraries(
        tolyffmpeg
        avcodec
        swresample
        avutil
        ${log-lib})