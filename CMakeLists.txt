cmake_minimum_required(VERSION 3.16)
project(Collision LANGUAGES CXX)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_BINARY_DIR}/bin)
option(BUILD_SHARED_LIBS "Build shared libraries" OFF)

include(FetchContent)
FetchContent_Declare(SFML
    GIT_REPOSITORY https://github.com/SFML/SFML.git
    GIT_TAG 2.6.x)
FetchContent_MakeAvailable(SFML)

set(COLLISION_SOURCES 
    src/main.cpp
    
 "src/circle.cpp" "src/circle.h" "src/collision.cpp")

add_executable(Collision ${COLLISION_SOURCES} "src/circle.cpp" "src/circle.h" "src/collision.cpp")
target_link_libraries(Collision PRIVATE sfml-graphics)
target_compile_features(Collision PRIVATE cxx_std_20)

if(WIN32)
    add_custom_command(
        TARGET Collision
        COMMENT "Copy OpenAL DLL"
        PRE_BUILD COMMAND ${CMAKE_COMMAND} -E copy ${SFML_SOURCE_DIR}/extlibs/bin/$<IF:$<EQUAL:${CMAKE_SIZEOF_VOID_P},8>,x64,x86>/openal32.dll $<TARGET_FILE_DIR:Collision>
        VERBATIM)
endif()

install(TARGETS Collision)
