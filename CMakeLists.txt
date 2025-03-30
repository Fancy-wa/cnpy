CMAKE_MINIMUM_REQUIRED(VERSION 3.5 FATAL_ERROR)
if(COMMAND cmake_policy)
	cmake_policy(SET CMP0003 NEW)
endif(COMMAND cmake_policy)

project(CNPY)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11")

option(CNPY_BUILD_SHARED "Enable building cnpy shared library" ON)
option(CNPY_BUILD_STATIC "Enable building cnpy static library" ON)

# find_package(ZLIB REQUIRED)
# include_directories(${ZLIB_INCLUDE_DIRS})
add_subdirectory(zlib)
include_directories(zlib)

if(CNPY_BUILD_SHARED)
    add_library(cnpy SHARED "cnpy.cpp")
    target_link_libraries(cnpy zlib)
    install(TARGETS cnpy LIBRARY DESTINATION lib)
endif()
if(CNPY_BUILD_STATIC)
    add_library(cnpystatic STATIC "cnpy.cpp")
    target_link_libraries(cnpystatic zlibstatic)
    install(TARGETS cnpystatic ARCHIVE DESTINATION lib)
endif()

install(FILES "cnpy.h" DESTINATION include)
install(FILES "mat2npz" "npy2mat" "npz2mat" DESTINATION bin PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)

# add_executable(example1 example1.cpp)
# target_link_libraries(example1 cnpy)
