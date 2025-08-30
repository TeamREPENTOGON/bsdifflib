project (bsdifflib C)

cmake_minimum_required(VERSION 3.0.0)

option(BSDIFFLIB_BUILD_EXEC "Build bsdifflib executables" OFF)

file(GLOB_RECURSE BSDIFF_BZIP2_SRC bzip2/src/*.c bzip2/include/bzip2/*.h)

add_library(bsdiff_bzip2 STATIC ${BSDIFF_BZIP2_SRC})
target_include_directories(bsdiff_bzip2 PRIVATE bzip2/include/bzip2)

add_library(bsdifflib STATIC src/bsdifflib.c)
add_library(bspatchlib STATIC src/bspatchlib.c)

target_link_libraries(bsdifflib bsdiff_bzip2)
target_link_libraries(bspatchlib bsdiff_bzip2)

target_include_directories(bsdifflib PRIVATE bzip2/include/bzip2 include/bsdifflib)
target_include_directories(bspatchlib PRIVATE bzip2/include/bzip2 include/bsdifflib)

if (${BSDIFFLIB_BUILD_EXEC})
  add_executable(bsdiff src/bsdiff.c)
  add_executable(bspatch src/bspatch.c)

  target_link_libraries(bsdiff bsdifflib)
  target_link_libraries(bspatch bspatchlib)

  target_include_directories(bsdiff PRIVATE include/bsdifflib)
  target_include_directories(bspatch PRIVATE include/bsdifflib)
endif()
