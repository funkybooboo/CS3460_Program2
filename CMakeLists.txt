cmake_minimum_required(VERSION 3.12)

set(PROJECT RandDistributions)

project(${PROJECT})

set(SOURCE_FILES Assignment2.cpp)

add_executable(${PROJECT} ${SOURCE_FILES})

set_property(TARGET ${PROJECT} PROPERTY CXX_STANDARD 20)

if (CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(${PROJECT} PRIVATE /W4 /permissive-)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    target_compile_options(${PROJECT} PRIVATE -Wall -Wextra -pedantic)
elseif (CMAKE_CXX_COMPILER_ID STREQUAL "Clang")
    target_compile_options(${PROJECT} PRIVATE -Wall -Wextra -pedantic)
endif ()

find_program(CLANG_FORMAT "clang-format")

if (CLANG_FORMAT)
    unset(SOURCE_FILES_PATHS)
    foreach (SOURCE_FILE ${SOURCE_FILES})
        get_source_file_property(WHERE ${SOURCE_FILES} LOCATION)
        set(SOURCE_FILES_PATHS ${SOURCE_FILES_PATHS} ${WHERE})
    endforeach ()

    add_custom_target(
            ClangFormat
            COMMAND ${CLANG_FORMAT}
            -i
            -style=file
            ${SOURCE_FILES_PATHS}
    )

    add_dependencies(${PROJECT} ClangFormat)

endif ()
