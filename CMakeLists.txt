
cmake_minimum_required(VERSION 2.8)
project(EIF)
IF(NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE DEBUG)
ENDIF()
message(STATUS CMAKE_MODULE_PATH)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -g -std=c++11 -Wall")

include_directories(
${PROJECT_SOURCE_DIR}    
)

add_executable(EIF main.cpp)



