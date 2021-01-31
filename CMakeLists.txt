cmake_minimum_required(VERSION 3.2)

if(DEFINED ENV{TRAVIS_BUILD_NUMBER})
	project(helloworld VERSION 0.0.$ENV{TRAVIS_BUILD_NUMBER})
else()
	project(helloworld VERSION 0.0.1)
endif()

configure_file(version.h.in version.h)
find_package(Boost COMPONENTS unit_test_framework REQUIRED)

add_executable(helloworld_cli main.cpp)
add_library(helloworld lib.cpp)
add_executable(tests tests.cpp)

set_target_properties(helloworld_cli helloworld tests PROPERTIES
	CXX_STANDARD 17
	CXX_STANDARD_REQUIRED ON
	CXX_EXTENSIONS OFF
)

target_link_libraries(helloworld_cli PRIVATE helloworld)

target_link_libraries(
	tests 
	${Boost_UNIT_TEST_FRAMEWORK_LIBRARY} 
	helloworld
)


if (MSVC)
    target_compile_options(helloworld_cli PRIVATE
        /W4
    )
    target_compile_options(helloworld PRIVATE
        /W4
    )
    target_compile_options(tests PRIVATE
        /W4
    )
else ()
    target_compile_options(helloworld_cli PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
    target_compile_options(helloworld PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
    target_compile_options(tests PRIVATE
        -Werror -Wall -Wimplicit-fallthrough -pedantic-errors
    )
endif()

install(TARGETS helloworld_cli RUNTIME DESTINATION bin)

set(CPACK_GENERATOR DEB)

set(CPACK_PACKAGE_VERSION_MAJOR "${PROJECT_VERSION_MAJOR}")
set(CPACK_PACKAGE_VERSION_MINOR "${PROJECT_VERSION_MINOR}")
set(CPACK_PACKAGE_VERSION_PATCH "${PROJECT_VERSION_PATCH}")

set(CPACK_PACKAGE_CONTACT b490@ya.ru)

include(CPack)

enable_testing()

add_test(tests tests)

