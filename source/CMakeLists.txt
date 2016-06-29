add_library(control care.cc
                    dare.cc
                    continuous_to_discrete.cc
                    control_internal_ctrb.cc
                    controllability.cc
                    observability.cc)

# see if library reflapacke is installed, normally for gentoo
# if not, link against lapacke
set(USE_REFLAPACKE_LIB "" CACHE INTERNAL "")
find_library(USE_REFLAPACKE_LIB reflapacke)
if (USE_REFLAPACKE_LIB)
    target_link_libraries(control reflapacke)
else()
    if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
        # if OSX, link against lapacke (which must be built) and Accelerate
        # framework (already installed)
        target_link_libraries(control lapacke "-framework Accelerate")
    else()
        target_link_libraries(control lapacke)
    endif()
endif()

if (LIBCONTROL_BUILD_TESTS)
  add_subdirectory(tests)
endif()

install(TARGETS control
        LIBRARY DESTINATION lib
        ARCHIVE DESTINATION lib)

