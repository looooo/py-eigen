cmake_minimum_required(VERSION 2.6)
project(eigpy)

set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
SET(CMAKE_CXX_FLAGS_RELEASE "-O3")
ADD_DEFINITIONS(-std=c++11)

find_package(Eigen3)
include_directories( ${EIGEN3_INCLUDE_DIR})

set(Python_ADDITIONAL_VERSIONS)
find_package(PythonInterp)
find_package(PythonLibs)

include_directories( ${PYTHON_INCLUDE_DIRS})

PYTHON_ADD_MODULE(paraEigen SHARED python_eigen.cpp)
configure_file(test.py test.py COPYONLY)
target_link_libraries(paraEigen ${PYTHON_LIBRARIES})

EXECUTE_PROCESS(COMMAND ${PYTHON_EXECUTABLE} -c 
  "from distutils.sysconfig import get_python_lib; print(get_python_lib())"
  OUTPUT_VARIABLE python_libs OUTPUT_STRIP_TRAILING_WHITESPACE )

install(TARGETS "paraEigen" DESTINATION ${python_libs})