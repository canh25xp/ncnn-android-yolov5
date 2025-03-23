project(yolov5ncnn)
cmake_minimum_required(VERSION 3.4.1)


set(NCNN_URL "https://github.com/Tencent/ncnn/releases/download/20201218/ncnn-20201218-android-vulkan.zip")
set(NCNN_ZIP "${CMAKE_SOURCE_DIR}/ncnn-20201218-android-vulkan.zip")
set(NCNN_DIR "${CMAKE_SOURCE_DIR}/ncnn-20201218-android-vulkan")

if(NOT EXISTS ${NCNN_DIR})
    message(STATUS "Downloading NCNN library...")
    file(DOWNLOAD ${NCNN_URL} ${NCNN_ZIP} SHOW_PROGRESS)

    # Extract the zip file
    message(STATUS "Extracting NCNN library...")
    execute_process(
        COMMAND ${CMAKE_COMMAND} -E tar xzf ${NCNN_ZIP}
        WORKING_DIRECTORY ${CMAKE_SOURCE_DIR}
    )

    # Delete the zip file
    file(REMOVE ${NCNN_ZIP})

    message(STATUS "NCNN library setup completed.")
endif()

set(ncnn_DIR ${NCNN_DIR}/${ANDROID_ABI}/lib/cmake/ncnn)
find_package(ncnn REQUIRED)

add_library(yolov5ncnn SHARED yolov5ncnn_jni.cpp)

target_link_libraries(yolov5ncnn
    ncnn

    jnigraphics
)
