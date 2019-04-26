#Notes
#Required dependencies
#OSX/LINUX PKGCONGIG
cmake_minimum_required (VERSION 3.2)

project(GAUSSProject)

#CMake Setup 
set(CMAKE_CXX_STANDARD 14)
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} ${PROJECT_SOURCE_DIR}/ThirdParty/libigl/external/eigen CACHE PATH "Root include directory for Eigen3" FORCE) 
set(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake CACHE PATH "Get libigil module" FORCE)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${PROJECT_SOURCE_DIR}/ThirdParty/libigl/external/eigen/cmake CACHE PATH "Root include directory for Eigen3" FORCE)
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PROJECT_BINARY_DIR}/lib )
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_DEBUG ${PROJECT_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_DEBUG ${PROJECT_BINARY_DIR}/lib )
set( CMAKE_RUNTIME_OUTPUT_DIRECTORY_RELEASE ${PROJECT_BINARY_DIR}/bin )
set( CMAKE_LIBRARY_OUTPUT_DIRECTORY_RELEASE ${PROJECT_BINARY_DIR}/lib )


#Use the EIGEN library included with libigl 
set(EIGEN3_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/ThirdParty/Gauss/build/ThirdParty/eigen CACHE INTERNAL "" FORCE)
include_directories(${EIGEN3_INCLUDE_DIR})

# libigl setup
option(LIBIGL_USE_STATIC_LIBRARY "Use libigl as static library" OFF)
option(LIBIGL_WITH_ANTTWEAKBAR       "Use AntTweakBar"    OFF)
option(LIBIGL_WITH_CGAL              "Use CGAL"           OFF)
option(LIBIGL_WITH_COMISO            "Use CoMiso"         OFF)
option(LIBIGL_WITH_CORK              "Use Cork"           OFF)
option(LIBIGL_WITH_EMBREE            "Use Embree"         OFF)
option(LIBIGL_WITH_LIM               "Use LIM"            OFF)
option(LIBIGL_WITH_MATLAB            "Use Matlab"         OFF)
option(LIBIGL_WITH_MOSEK             "Use MOSEK"          OFF)
option(LIBIGL_WITH_PNG               "Use PNG"            OFF)
option(LIBIGL_WITH_PYTHON            "Use Python"         OFF)
option(LIBIGL_WITH_TETGEN            "Use Tetgen"         OFF)
option(LIBIGL_WITH_TRIANGLE          "Use Triangle"       OFF)
option(LIBIGL_WITH_XML               "Use XML"            OFF)


#Use GAUSS Qt3D powered UI if you want 
option(GAUSS_USE_UI     "Use Gauss' UI Library" OFF)

#Init GUI Stuff
if(GAUSS_USE_UI)
    include(${PROJECT_SOURCE_DIR}/ThirdParty/GAUSS/src/UI/UISetup.txt)
    initUI()
endif(GAUSS_USE_UI)

#Use LibIGL viewer with application
#option(LIBIGL_WITH_OPENGL            "Use OpenGL"         ON)
#option(LIBIGL_WITH_OPENGL_GLFW       "Use GLFW"           ON)
#option(LIBIGL_WITH_OPENGL_GLFW_IMGUI "Use ImGui"          ON)
#option(LIBIGL_WITH_VIEWER            "Use OpenGL viewer"  ON)

#Some other GAUSS options you could use
option(USE_PARDISO "Use Pardiso if available" OFF)
option(USE_GUROBI "Use Gurobi optimization package if available" OFF)

# Get Gauss as a subdirectory
add_subdirectory(${PROJECT_SOURCE_DIR}/ThirdParty/Gauss ${CMAKE_CURRENT_BINARY_DIR}/Gauss)

#Add include directories you can add more if you want to use more things
set(LIBIGL_INCLUDE_DIR ${PROJECT_SOURCE_DIR}/ThirdParty/Gauss/ThirdParty/libigl/include CACHE FILEPATH "Root include directory for libigl")
find_package(LIBIGL REQUIRED)

include_directories(${Core_SOURCE_DIR}/include)
include_directories(${Base_SOURCE_DIR}/include)
include_directories(${FEM_SOURCE_DIR}/include)

include_directories(${Gauss_EXT_INCLUDE_DIRS})

#get header and source files
file(GLOB HEADER_CODE ${PROJECT_SOURCE_DIR}/*.h)
file(GLOB SRC_CODE ${PROJECT_SOURCE_DIR}/*.cpp)

include_directories(${LIBIGL_INCLUDE_DIR})
#These are needed for libigls viewer support 
include_directories(${PROJECT_SOURCE_DIR}/ThirdParty/libigl/external/glad/include)
include_directories(${PROJECT_SOURCE_DIR}/ThirdParty/libigl/external/imgui)

add_executable(GAUSSProject ${SRC_CODE} ${HEADER_CODE})
target_link_libraries(GAUSSProject Core Base FEM ParticleSystem ${GAUSS_LIBS}) #Remove UI if you don't want the GAUSS UI 

