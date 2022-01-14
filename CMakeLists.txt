cmake_minimum_required(VERSION 3.10)
project(Fluid3D)

##### Setup ######
set(CMAKE_CXX_STANDARD 98)

## Solver ##
file(GLOB LIB_SOURCES "pcgsolver/*.h"
                      "pcgsolver/*.cpp"
                      "array1.h"
                      "array2.h"
                      "array2_utils.h"
                      "array3.h"
                      "array3_utils.h"
                      "fluidsim.h"
                      "fluidsim.cpp"
                      "levelset_util.h"
                      "levelset_util.cpp"
                      "util.h"
                      "vec.h")

add_library(fluid3d_lib STATIC ${LIB_SOURCES})
target_include_directories(fluid3d_lib PUBLIC ./)

# Build the executable
add_executable(fluid3d_solver ${LIB_SOURCES} main.cpp)

option(ENABLE_VIEW_APP "Build the opengl view app" OFF)
if (ENABLE_VIEW_APP)

    ##### External Libraries ######
    find_package(OpenGL)
    find_package(GLUT)

    ###### Header Files ######
    include_directories(./ ${GLU_INCLUDES})

    ##### Executables #####
    # To avoid naming conflicts (can't have a "viewpls3D" directory AND file in
    # the same directory) output all the executables to their own folder
    set(EXECUTABLE_OUTPUT_PATH "bin")

    ## viewpls3D ##
    # Find all the source (.cpp, .c) files in the current directory
    aux_source_directory(./viewpls3D VIEW_PLS_3D_SRC)

    # Build the executable
    add_executable(viewpls3D ${VIEW_PLS_3D_SRC})

    # Link the executable to external libraries
    target_link_libraries(viewpls3D ${OPENGL_LIBRARIES} ${GLUT_LIBRARIES})
endif ()