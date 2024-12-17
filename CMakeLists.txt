cmake_minimum_required(VERSION 3.25)
set(CMAKE_CXX_STANDARD 14)

SET(LIB_FILE ${CMAKE_CURRENT_LIST_DIR}/libsodium.a)
SET(LIB_HEADER_FOLDER ${CMAKE_CURRENT_LIST_DIR})

# how to build the result of the library
add_custom_command(OUTPUT ${LIB_FILE}
    COMMAND make static
    WORKING_DIRECTORY ${CMAKE_CURRENT_LIST_DIR})

# create a target out of the library compilation result
add_custom_target(sodium_target DEPENDS ${LIB_FILE})

# create an library target out of the library compilation result
add_library(sodium STATIC IMPORTED GLOBAL)
add_dependencies(sodium sodium_target)

# specify where the library is and where to find the headers
set_target_properties(sodium
    PROPERTIES
    IMPORTED_LOCATION ${LIB_FILE}
    INTERFACE_INCLUDE_DIRECTORIES ${LIB_HEADER_FOLDER})
