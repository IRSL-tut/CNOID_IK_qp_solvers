cmake_minimum_required(VERSION 3.8)
project(osqp-cpp)

set(CMAKE_BUILD_TYPE Release)

#find_package(catkin REQUIRED COMPONENTS)
find_package(ament_cmake REQUIRED)

find_package(absl)

find_package(Eigen3 REQUIRED)

find_package(osqp REQUIRED)
add_library(osqpstatic INTERFACE)
target_link_libraries(osqpstatic INTERFACE osqp::osqpstatic) # osqp-cppは、osqpstaticという名前を探すので、リネームする

get_target_property(absl_INCLUDE_DIRS absl::base INTERFACE_INCLUDE_DIRECTORIES)

#catkin_package(
#  INCLUDE_DIRS osqp-cpp/include ${absl_INCLUDE_DIRS}
#  LIBRARIES osqp-cpp
#  )

set(OSQP-CPP_BUILD_TESTS OFF CACHE BOOL "") # typeを与えないと、cmake3.10ではoption()コマンドに上書きされてしまう. https://cmake.org/cmake/help/latest/policy/CMP0077.html#policy:CMP0077

add_subdirectory(osqp-cpp)

install(TARGETS osqp-cpp
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  )

install(DIRECTORY osqp-cpp/include/
  DESTINATION include/${PROJECT_NAME}
  FILES_MATCHING PATTERN "*.h"
  PATTERN ".svn" EXCLUDE
  )

ament_export_libraries(osqp-cpp)
ament_export_include_directories(osqp-cpp/include)
ament_package()
