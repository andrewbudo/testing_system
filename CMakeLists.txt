cmake_minimum_required(VERSION 3.2)

if(DEFINED ENV{TRAVIS_BUILD_NUMBER})
	project(devel VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
	project(devel VERSION 0.0.1)
endif()

configure_file(version.h.in version.h)
find_package(Boost COMPONENTS unit_test_framework REQUIRED)

add_executable(devel_cli main.cpp)
add_library(devel lib.cpp)
add_executable(tests tests.cpp)

set_target_properties(devel_cli devel tests PROPERTIES
	CXX_STANDARD 17
	CXX_STANDARD_REQUIRED ON
	CXX_EXTENSIONS OFF
)

target_include_directories(devel PRIVATE "${CMAKE_BINARY_DIR}")

set_target_properties(tests PROPERTIES
    COMPILE_DEFINITIONS BOOST_TEST_DYN_LINK
    INCLUDE_DIRECTORIES ${Boost_INCLUDE_DIR}
)

target_link_libraries(devel_cli PRIVATE devel)
target_link_libraries(tests ${Boost_UNIT_TEST_FRAMEWORK_LIBRARY} devel)

if(MSVC)
    target_compile_options(devel_cli PRIVATE
        /W4
    )
    target_compile_options(devel PRIVATE
        /W4
    )
    target_compile_options(tests PRIVATE
        /W4
    )
else()
    target_compile_options(devel_cli PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
    target_compile_options(devel PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
    target_compile_options(tests PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
endif()

install(TARGETS devel_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT b490@ya.ru)

include(CPack)

enable_testing()

add_test(tests tests)
