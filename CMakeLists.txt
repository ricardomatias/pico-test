# Generated Cmake Pico project file
cmake_minimum_required(VERSION 3.24)

set(CMAKE_C_STANDARD 11)
set(CMAKE_CXX_STANDARD 17)

# Initialise pico_sdk from installed location
# (note this can come from environment, CMake cache etc)
set(PICO_SDK_PATH $ENV{PICO_SDK_PATH})
set(PICO_BOARD pico CACHE STRING "Board type")

# Pull in Raspberry Pi Pico SDK (must be before project)
include(pico_sdk_import.cmake)

if (PICO_SDK_VERSION_STRING VERSION_LESS "1.4.0")
  message(FATAL_ERROR "Raspberry Pi Pico SDK version 1.4.0 (or later) required. Your version is ${PICO_SDK_VERSION_STRING}")
endif()

project(hello-pico C CXX ASM)

# Initialise the Raspberry Pi Pico SDK
pico_sdk_init()

# Add executable. Default name is the project name, version 0.1

add_executable(hello-pico hello-pico.cpp )

pico_set_program_name(hello-pico "hello-pico")
pico_set_program_version(hello-pico "0.1")

pico_enable_stdio_uart(hello-pico 1)
pico_enable_stdio_usb(hello-pico 0)

# Add the standard library to the build
target_link_libraries(hello-pico
        pico_stdlib)

# Add the standard include files to the build
target_include_directories(hello-pico PRIVATE
  ${CMAKE_CURRENT_LIST_DIR}
)

pico_add_extra_outputs(hello-pico)

include(FetchContent)

FetchContent_Declare(
  doctest
  GIT_REPOSITORY "https://github.com/doctest/doctest.git"
  GIT_TAG "v2.4.11"
)

FetchContent_MakeAvailable(doctest)

set(TESTS_EXE pico-test)

add_executable(${TESTS_EXE} EXCLUDE_FROM_ALL test/test.cpp)
target_compile_features(${TESTS_EXE} PRIVATE cxx_std_17)
target_link_options(${TESTS_EXE} PRIVATE "--specs=nosys.specs")
target_link_libraries(${TESTS_EXE} PRIVATE doctest)
set_target_properties(${TESTS_EXE} PROPERTIES OUTPUT_NAME ${TESTS_EXE} SUFFIX ".exe")

include(CTest)