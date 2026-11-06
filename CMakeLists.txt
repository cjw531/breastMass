cmake_minimum_required(VERSION 3.10)
project(breastMass)

set(CMAKE_CXX_STANDARD 14)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

find_package(VTK REQUIRED COMPONENTS
  CommonCore
  CommonDataModel
  IOImage
  IOXML
  ImagingCore
)

if(VTK_USE_FILE)
  include(${VTK_USE_FILE})
endif()

find_package(Boost REQUIRED COMPONENTS program_options)
find_package(OpenMP REQUIRED)

add_executable(breastMass breastMass.cxx)

target_include_directories(breastMass PRIVATE
  ${Boost_INCLUDE_DIRS}
)

target_link_libraries(breastMass PRIVATE
  ${VTK_LIBRARIES}
  ${Boost_LIBRARIES}
  OpenMP::OpenMP_CXX
)