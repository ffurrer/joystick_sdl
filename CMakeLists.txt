cmake_minimum_required(VERSION 2.8.3)
project(joystick_sdl)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

find_package(catkin_simple REQUIRED)
catkin_simple(ALL_DEPS_REQUIRED)

include(FindPkgConfig)
pkg_search_module(SDL2 REQUIRED sdl2)
pkg_check_modules(SDL204 sdl2>=2.0.4)

catkin_package(
  INCLUDE_DIRS include
  LIBRARIES ${PROJECT_NAME}
  CATKIN_DEPENDS diagnostic_updater roscpp sensor_msgs
  DEPENDS ${SDL2_LIBRARIES}
)

include_directories(
  ${SDL2_INCLUDE_DIRS}
)

cs_add_library(${PROJECT_NAME} src/joystick.cpp)
target_link_libraries(${PROJECT_NAME} ${SDL2_LIBRARIES})
if(SDL204_FOUND)
  set_property(TARGET ${PROJECT_NAME} PROPERTY COMPILE_DEFINITIONS SDL2_ENABLE_JOYSTICK_POWER_LEVEL)
endif()

cs_add_executable(${PROJECT_NAME}_node src/joystick_node.cpp)
target_link_libraries(${PROJECT_NAME}_node ${PROJECT_NAME} ${SDL2_LIBRARIES})

cs_install()
cs_export()

install(DIRECTORY mappings
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
