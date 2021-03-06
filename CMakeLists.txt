cmake_minimum_required(VERSION 3.13)
project(genetic_painter)

find_program(CONAN conan)

if (NOT CONAN)
    message(FATAL_ERROR "Needs conan package manager: please visit https://conan.io to install.")
endif()

set(CONAN_SCRIPT_FILE ${CMAKE_SOURCE_DIR}/scripts/conan.cmake)

if(NOT EXISTS ${CONAN_SCRIPT_FILE})
  message(STATUS "Downloading conan.cmake from https://github.com/memsharded/cmake-conan")

  file(DOWNLOAD "https://github.com/conan-io/cmake-conan/raw/master/conan.cmake" ${CONAN_SCRIPT_FILE})
endif()

include(${CONAN_SCRIPT_FILE})

conan_add_remote(NAME bincrafters INDEX 1
        URL https://api.bintray.com/conan/bincrafters/public-conan)

conan_cmake_run(
    CONANFILE conanfile.txt
    BASIC_SETUP CMAKE_TARGETS BUILD missing
)

set(CMAKE_CXX_STANDARD 17)
enable_testing()


add_compile_options(-Wall -Werror -Wextra -fPIC)

if(CMAKE_BUILD_TYPE MATCHES Debug)
    add_compile_options(-DDEBUG)
endif()

add_compile_options(-DQT_NO_OPENGL)

add_subdirectory(test)
add_subdirectory(src)

