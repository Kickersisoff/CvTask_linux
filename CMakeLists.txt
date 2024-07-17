cmake_minimum_required(VERSION 3.10)
project(server)

set(CMAKE_CXX_STANDARD 17)

find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# Adjust the vcpkg root directory if necessary
set(VCPKG_ROOT "/opt/vcpkg")

set(Crow_DIR "${VCPKG_ROOT}/installed/x64-linux/share/crow")
set(nlohmann_json_DIR "${VCPKG_ROOT}/installed/x64-linux/share/nlohmann_json")
set(ASIO_INCLUDE_DIR "${VCPKG_ROOT}/installed/x64-linux/include")

find_package(Crow CONFIG REQUIRED)
find_package(nlohmann_json CONFIG REQUIRED)
set(CPPBASE64_DIR "${VCPKG_ROOT}/installed/x64-linux/include/cpp-base64")

# Add the directory containing base64.h to the include path
include_directories(${CPPBASE64_DIR})

INCLUDE_DIRECTORIES(${CMAKE_CURRENT_SOURCE_DIR}/../tflite-dist/include/)

ADD_LIBRARY(tensorflowlite SHARED IMPORTED)
set_property(TARGET tensorflowlite PROPERTY IMPORTED_LOCATION ${CMAKE_CURRENT_SOURCE_DIR}/../tflite-dist/libs/linux_x64/libtensorflowlite.so)

add_executable(${PROJECT_NAME} main.cpp)

target_link_libraries(server PRIVATE Crow::Crow nlohmann_json::nlohmann_json ${OpenCV_LIBS} tensorflowlite)