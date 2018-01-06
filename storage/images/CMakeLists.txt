set(CMAKE_VERBOSE_MAKEFILE ON)
cmake_minimum_required(VERSION 3.6)
project(SF CXX)

set(CMAKE_CXX_STANDARD 11)

# Put the FindSDL2.cmake in a folder and tell CMake to find it there
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${SF_SOURCE_DIR}/cmake")

# This path is used by FindSDL2 module. 
# It will find include and lib folders under this directory.
# This is because find command doesn't include any path for Windows.
# So it can't find anything using default paths.
# SDL2_PATH is already expected in module file. I'm using the same variable.
#set(SDL2_PATH "C:\\Users\\Almas\\workspace\\c++\\shared_libs\\SDL2-devel-2.0.6-mingw\\SDL2-2.0.6\\x86_64-w64-mingw32")

#set(SDL2_PATH "${SF_SOURCE_DIR}/libs")

set(SDL2MAIN_LIBRARY "${SF_SOURCE_DIR}/libs/SDL2/lib/libSDL2main.a")

set(SDL2_INCLUDE_DIR "${SF_SOURCE_DIR}/libs/SDL2/include/SDL2")
set(SDL2_LIBRARY "${SF_SOURCE_DIR}/libs/SDL2/lib/libSDL2.dll.a")

set(SDL2_IMAGE_INCLUDE_DIR "${SF_SOURCE_DIR}/libs/SDL2_image/include")
set(SDL2_IMAGE_LIBRARIES "${SF_SOURCE_DIR}/libs/SDL2_image/lib/x64/SDL2_image.lib")

# CMake parses module files by their names. It will use FindSDL2 in this case.
# find_package(SDL2 REQUIRED)
# find_package(SDL2_image REQUIRED)
# find_package(SDL2_ttf REQUIRED)

# Explicitly specify paths for SDL_gpu. It's hardcoded and ugly but easy.
# set(SDL2_GPU_LIBRARY_PATH "C:/Development/Library/lib/libSDL2_gpu.dll.a")
# set(SDL2_GPU_INCLUDE_PATH "C:/Development/Library/include/SDL2_gpu")

# SDL2_INCLUDE_DIR variable set by the FindSDL2 module (if it finds directory).
include_directories(${SDL2_INCLUDE_DIR}
                    ${SDL2_IMAGE_INCLUDE_DIR})

#set(SOURCE_FILES src/)

file(GLOB SOURCE_FILES "src/*.h" "src/*.cpp")
add_executable(SF WIN32 ${SOURCE_FILES})

# make assets directory in build
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/assets)

# copy game assets into build/assets directory
file(COPY
        "assets/alien.png"
        "assets/coin.png"
        "assets/player.png"
        "assets/projectile.png"
        DESTINATION ${CMAKE_BINARY_DIR}/assets)

# SDL2MAIN_LIBRARY is also specified within SDL2_LIBRARY by the FindSDL2 module.
# It's needed for Windows specific main function.
# If I don't use FindSDL2 module, I have to link it as well.
target_link_libraries(SF
		${SDL2MAIN_LIBRARY}
        ${SDL2_LIBRARY}
        ${SDL2_IMAGE_LIBRARIES})