cmake_minimum_required(VERSION 3.13)

if(WIN32)
    set(CMAKE_TOOLCHAIN_FILE  "$ENV{VCPKG_ROOT}/scripts/buildsystems/vcpkg.cmake")
endif()

project(HelloWorld)

option(ENABLE_TESTS "Enable tests" OFF)

if(WIN32)
    find_package(wxWidgets CONFIG REQUIRED)
elseif(UNIX)
    find_package(wxWidgets REQUIRED COMPONENTS net core base)
endif()

if(wxWidgets_USE_FILE) # not defined in CONFIG mode
    include(${wxWidgets_USE_FILE})
endif()
set(TARGET HelloWorld)

# Automatically find all .cpp files in the src directory
FILE(GLOB CPP_SOURCES ./src/*.cpp)

# Automatically find all .h files in the src directory
FILE(GLOB CPP_HEADERS /include/*.h)

include_directories(${TARGET} PRIVATE ./include/${CPP_HEADERS})

add_executable(${TARGET} ${CPP_SOURCES})

if(WIN32)
    target_link_libraries(${TARGET} PRIVATE wx::core wx::base)
elseif(UNIX)
    target_link_libraries(${TARGET} ${wxWidgets_LIBRARIES})
endif()



