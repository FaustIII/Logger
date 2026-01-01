cmake_minimum_required(VERSION 3.10)
project(Logger)

if(CMAKE_SYSTEM_NAME STREQUAL "Linux")
    set(CMAKE_CXX_STANDARD 17)
else()
    set(CMAKE_CXX_STANDARD 11)
endif()

# Simple choice: SHARED or STATIC (not both)
option(BUILD_AS_SHARED "Build as shared library (DLL/SO)" OFF)

if(BUILD_AS_SHARED)
    add_library(Logger SHARED src/Logger/Logger.cpp)
    message(STATUS "Building SHARED library")
    
    if(WIN32)
        target_compile_definitions(Logger PRIVATE LOGGER_EXPORTS)
    endif()
else()
    add_library(Logger STATIC src/Logger/Logger.cpp)
    message(STATUS "Building STATIC library")
endif()

target_include_directories(Logger PUBLIC include)

if(WIN32)
    target_link_libraries(Logger PRIVATE kernel32)
endif()

# Simple installation
install(TARGETS Logger
    LIBRARY DESTINATION lib
    ARCHIVE DESTINATION lib
    RUNTIME DESTINATION bin
)

install(DIRECTORY include/ DESTINATION include)
