
# Prevent overriding the parent project's compiler/linker
# settings on Windows
set(gtest_force_shared_crt ON CACHE INTERNAL "")

# google test does not properly use pthreads on mingw
if (MINGW)
  set(gtest_disable_pthreads  ON CACHE INTERNAL "")
endif()

set(BUILD_GTEST                 ON  CACHE INTERNAL "")
set(BUILD_GMOCK                 OFF CACHE INTERNAL "")

# Must build GTest as static since EXCLUDE_FROM_ALL, would exclude
# needed GTest shared libraries from being installed.
set(BUILD_SHARED_LIBS OFF)

set(GTEST_SRC "${CMAKE_CURRENT_SOURCE_DIR}/itkgoogletest/")

# check if we are using "system" source code
if (ITK_USE_SYSTEM_GOOGLETEST)
  set(GTEST_SRC "${GTEST_ROOT}")
endif()

# Add googletest directly to our build but exclude from using it's
# target's and installation unless referenced by other dependencies.
add_subdirectory( "${GTEST_SRC}"
  "${CMAKE_CURRENT_BINARY_DIR}/GTest-build" EXCLUDE_FROM_ALL)

add_library(ITK::GTest ALIAS gtest)
add_library(ITK::GTestMain ALIAS gtest_main)
