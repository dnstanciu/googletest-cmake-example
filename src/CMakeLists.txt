set(Main_SOURCE_FILES main.cpp Addition.cpp)

include_directories(../include)

add_executable(googletest-cmake-example ${Main_SOURCE_FILES})

add_library(googletest-cmake-example_lib STATIC ${Main_SOURCE_FILES})

# Specify the include directories exported by this library
target_include_directories(googletest-cmake-example_lib PUBLIC ../include)


# specify the libraries this program depends on
# target_link_libraries(googletest-cmake-example)

