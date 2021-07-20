cmake_minimum_required(VERSION 3.12)
project(decimal_for_cpp)

if(CMAKE_PROJECT_NAME STREQUAL PROJECT_NAME)
    set(IS_TOPLEVEL_PROJECT TRUE)
else()
    set(IS_TOPLEVEL_PROJECT FALSE)
endif()

option(DECIMAL_FOR_CPP_BUILD_TESTING "Build decimal_for_cpp testing" ${IS_TOPLEVEL_PROJECT})
option(DECIMAL_FOR_CPP_INSTALL "Install decimal_for_cpp targets" ${IS_TOPLEVEL_PROJECT})

add_library(${PROJECT_NAME} INTERFACE)
add_library(${PROJECT_NAME}::${PROJECT_NAME} ALIAS ${PROJECT_NAME})
target_include_directories(${PROJECT_NAME} INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/include)

if(DECIMAL_FOR_CPP_BUILD_TESTING)
    include(CTest)

    set(CMAKE_CXX_STANDARD 11 CACHE STRING "C++ standard version to use (default is 11)")
    set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_STANDARD 11 CACHE STRING "C++ standard version to use (default is 11)")
set(CMAKE_CXX_STANDARD_REQUIRED ON)

    find_package(Boost COMPONENTS unit_test_framework REQUIRED)

    if(Boost_FOUND)
        include_directories(${Boost_INCLUDE_DIRS} include)
        add_executable(test_runner include/decimal.h tests/runner.cpp tests/decimalTest.ipp)
        target_include_directories(test_runner PRIVATE ${BOOST_INCLUDE_DIRS})
    endif()

    enable_testing()
    add_test(test_runner test_runner)
endif()

if(DECIMAL_FOR_CPP_INSTALL)
    install(DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/include DESTINATION ${CMAKE_INSTALL_INCLUDEDIR})
endif()