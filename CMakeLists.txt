project(imesa CXX C)
cmake_minimum_required(VERSION 3.0.0)
set(CMAKE_CXX_STANDARD 17)

# Dependencies 
find_package(GTSAM REQUIRED)
include_directories(${GTSAM_INCLUDE_DIR})


message(STATUS "================  iMESA  ======================")
# Build the iMESA library
file(GLOB_RECURSE imesa_srcs "*.cpp" "*.h")
add_library(imesa ${imesa_srcs})
target_link_libraries(imesa gtsam)
target_include_directories(imesa PUBLIC "${CMAKE_CURRENT_SOURCE_DIR}/include")

# Install the iMESA library
install(TARGETS imesa
    LIBRARY DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/lib
    ARCHIVE DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/lib
    RUNTIME DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/bin
)

# Install the header files in place
install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include/ 
    DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/include
    FILES_MATCHING PATTERN "*.h"
)

# Export targets manually to a file
export(TARGETS imesa
    FILE "${CMAKE_CURRENT_BINARY_DIR}/imesaTargets.cmake"
)

# Create and install imesaConfig.cmake manually
set(imesa_INCLUDE_DIRS "${CMAKE_CURRENT_SOURCE_DIR}/include")
set(imesa_LIBRARIES imesa)

# Configure the config file manually
configure_file(imesaConfig.cmake.in "${CMAKE_CURRENT_BINARY_DIR}/imesaConfig.cmake" @ONLY)
install(FILES "${CMAKE_CURRENT_BINARY_DIR}/imesaConfig.cmake"
              DESTINATION ${CMAKE_CURRENT_SOURCE_DIR}/cmake)