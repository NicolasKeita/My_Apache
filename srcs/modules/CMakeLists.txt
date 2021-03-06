cmake_minimum_required(VERSION 3.14.5)
project(My_Apache)

set(BINARY              module)
set(CMAKE_CXX_STANDARD	20)

add_library(${BINARY}_SSL MODULE
        SSL.cpp
        )

target_include_directories(${BINARY}_SSL SYSTEM PUBLIC
        ${CONAN_INCLUDE_DIRS_BOOST}
        ${CONAN_INCLUDE_DIRS_OPENSSL}
        ${CMAKE_HOME_DIRECTORY}/lib/openZia
        )

target_include_directories(${BINARY}_SSL PRIVATE
        ${CMAKE_HOME_DIRECTORY}/lib/uti/
        )

target_link_libraries(${BINARY}_SSL PRIVATE
        openZia
        ${CONAN_LIBS_BOOST}
        ${CONAN_LIBS_OPENSSL}
        )

if(CMAKE_COMPILER_IS_GNUCXX)
    target_compile_options(${BINARY}_SSL PRIVATE
            -Wall
            -Wextra
            -Wshadow
            -Wnon-virtual-dtor
            -Wold-style-cast
            -Wcast-align
            -Wunused
            -Woverloaded-virtual
            -Wpedantic
            -Wconversion
            -Wsign-conversion
            -Wnull-dereference
            -Wdouble-promotion
            -Wformat=2
            -Wduplicated-cond
            -Wduplicated-branches
            -Wlogical-op
            -Wuseless-cast)
endif()

# # # # # Module HTTP # # # #


add_library(${BINARY}_HTTP MODULE
        HTTP.cpp
        )

target_include_directories(${BINARY}_HTTP SYSTEM PUBLIC
        ${CONAN_INCLUDE_DIRS_BOOST}
        ${CONAN_INCLUDE_DIRS_OPENSSL}
        ${CMAKE_HOME_DIRECTORY}/lib/openZia
        )

target_include_directories(${BINARY}_HTTP PRIVATE
        ${CMAKE_HOME_DIRECTORY}/lib/uti/
        )

target_link_libraries(${BINARY}_HTTP PRIVATE
        openZia
        ${CONAN_LIBS_BOOST}
        ${CONAN_LIBS_OPENSSL}
        )

if(CMAKE_COMPILER_IS_GNUCXX)
    target_compile_options(${BINARY}_HTTP PRIVATE
            -Wall
            -Wextra
            -Wshadow
            -Wnon-virtual-dtor
            -Wold-style-cast
            -Wcast-align
            -Wunused
            -Woverloaded-virtual
            -Wpedantic
            -Wconversion
            -Wsign-conversion
            -Wnull-dereference
            -Wdouble-promotion
            -Wformat=2
            -Wduplicated-cond
            -Wduplicated-branches
            -Wlogical-op
            -Wuseless-cast)
endif()

if (MSVC)
    # Force to always compile with W4
    if(CMAKE_CXX_FLAGS MATCHES "/W[0-4]")
        string(REGEX REPLACE "/W[0-4]" "/W4" CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")
    else()
        set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4")
    endif()
    # Tells the compiler you are on Windows 10. It suppresses some warnings.
    add_compile_definitions(_WIN32_WINNT=0x0A00)
endif()
