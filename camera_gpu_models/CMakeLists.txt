cmake_minimum_required(VERSION 2.8.3)
project(camera_gpu_models)

set(CMAKE_BUILD_TYPE "Release")
set(CMAKE_CXX_FLAGS "-std=c++11")
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -fPIC")

find_package(catkin REQUIRED COMPONENTS
    roscpp
    std_msgs
    )

find_package(Boost REQUIRED COMPONENTS filesystem program_options system)
include_directories(${Boost_INCLUDE_DIRS})

find_package(OpenCV REQUIRED)

# set(EIGEN_INCLUDE_DIR "/usr/local/include/eigen3")
find_package(Ceres REQUIRED)
include_directories(${CERES_INCLUDE_DIRS})


catkin_package(
    INCLUDE_DIRS include
    LIBRARIES camera_gpu_models
    CATKIN_DEPENDS roscpp std_msgs
#    DEPENDS system_lib
    )

include_directories(
    ${catkin_INCLUDE_DIRS}
    )

include_directories("include")

add_executable(Calibrations 
    src/intrinsic_calib.cc
    src/chessboard/Chessboard.cc
    src/calib/CameraCalibration.cc
    src/camera_gpu_models/Camera.cc
    src/camera_gpu_models/CameraFactory.cc
    src/camera_gpu_models/CostFunctionFactory.cc
    src/camera_gpu_models/PinholeCamera.cc
    src/camera_gpu_models/PinholeFullCamera.cc
    src/camera_gpu_models/CataCamera.cc
    src/camera_gpu_models/EquidistantCamera.cc
    src/camera_gpu_models/ScaramuzzaCamera.cc
    src/sparse_graph/Transform.cc
    src/gpl/gpl.cc
    src/gpl/EigenQuaternionParameterization.cc)

add_library(camera_gpu_models
    src/chessboard/Chessboard.cc
    src/calib/CameraCalibration.cc
    src/camera_gpu_models/Camera.cc
    src/camera_gpu_models/CameraFactory.cc
    src/camera_gpu_models/CostFunctionFactory.cc
    src/camera_gpu_models/PinholeCamera.cc
    src/camera_gpu_models/PinholeFullCamera.cc
    src/camera_gpu_models/CataCamera.cc
    src/camera_gpu_models/EquidistantCamera.cc
    src/camera_gpu_models/ScaramuzzaCamera.cc
    src/sparse_graph/Transform.cc
    src/gpl/gpl.cc
    src/gpl/EigenQuaternionParameterization.cc)

target_link_libraries(Calibrations ${Boost_LIBRARIES} ${OpenCV_LIBS} ${CERES_LIBRARIES})
target_link_libraries(camera_gpu_models ${Boost_LIBRARIES} ${OpenCV_LIBS} ${CERES_LIBRARIES})
