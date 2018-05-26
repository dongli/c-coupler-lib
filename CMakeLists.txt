cmake_minimum_required (VERSION 3.9)

project (c_coupler CXX Fortran)

set (CMAKE_Fortran_FLAGS "${CMAKE_Fortran_FLAGS} -ffree-line-length-none")

find_package (MPI)
include_directories (${MPI_CXX_INCLUDE_DIRS})

find_package (PkgConfig)
if (PKG_CONFIG_FOUND)
  pkg_search_module (NETCDF REQUIRED netcdf)
  if (NETCDF_FOUND)
    include_directories (${NETCDF_INCLUDE_DIRS})
    link_directories (${NETCDF_LIBRARY_DIRS})
  else ()
    message (FATAL_ERROR "Pkg-config could not find netcdf library!")
  endif ()
else ()
  message (FATAL_ERROR "Unable to find pkg-config library!")
endif ()

include_directories (${PROJECT_SOURCE_DIR}/src/Driver)
include_directories (${PROJECT_SOURCE_DIR}/src/Parallel_MGT)
include_directories (${PROJECT_SOURCE_DIR}/src/Data_MGT)
include_directories (${PROJECT_SOURCE_DIR}/src/Runtime_MGT)
include_directories (${PROJECT_SOURCE_DIR}/src/Utils)
include_directories (${PROJECT_SOURCE_DIR}/src/CoR)
include_directories (${PROJECT_SOURCE_DIR}/src/XML)

file (GLOB_RECURSE source_files "*.h" "*.cxx" "*.F90")

add_library (c_coupler ${source_files})
