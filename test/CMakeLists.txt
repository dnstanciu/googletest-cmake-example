find_package(Threads REQUIRED)

# Enable ExternalProject CMake module
include(ExternalProject)

# Download and install GoogleTest
ExternalProject_Add(
    gtest
    URL http://googletest.googlecode.com/files/gtest-1.7.0.zip
    PREFIX ${CMAKE_CURRENT_BINARY_DIR}/gtest
    # Disable install step
    INSTALL_COMMAND ""
)

# Create a libgtest target to be used as a dependency by test programs
add_library(libgtest IMPORTED STATIC GLOBAL)
add_dependencies(libgtest gtest)

# Set gtest properties
ExternalProject_Get_Property(gtest source_dir binary_dir)
set_target_properties(libgtest PROPERTIES
    "IMPORTED_LOCATION" "${binary_dir}/libgtest.a"
    "IMPORTED_LINK_INTERFACE_LIBRARIES" "${CMAKE_THREAD_LIBS_INIT}"
#    "INTERFACE_INCLUDE_DIRECTORIES" "${source_dir}/include"
)
# I couldn't make it work with INTERFACE_INCLUDE_DIRECTORIES
include_directories("${source_dir}/include")


####################
### Actual Tests ###
####################

# an executable for the sample test
add_executable(Sample_test main.cpp sampleTest.cpp)

target_link_libraries(Sample_test libgtest)

add_test(NAME Sample_test
         COMMAND Sample_test)

# an executable for testing the Addition class
add_executable(runAddition_test main.cpp AdditionTest.cpp)

target_link_libraries(runAddition_test
    libgtest
    googletest-cmake-example_lib
)

add_test(NAME runAddition_test
         COMMAND runAddition_test)
