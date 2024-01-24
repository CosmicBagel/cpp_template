cmake_minimum_required(VERSION 3.28)

project(cpp_pong VERSION 0.1.0 LANGUAGES C CXX)

add_executable(cpp_pong) 

#ie if this is the top most level script
#   these options should only be set if not being used by another project
if(CMAKE_SOURCE_DIR STREQUAL CMAKE_CURRENT_SOURCE_DIR)
    # all of these compiler options are effectively global

    target_compile_options(cpp_pong PRIVATE -Wall -Wextra -Wpedantic -Wconversion -Wsign-conversion)

    if (CMAKE_BUILD_TYPE STREQUAL "Debug")
        # To get nicer stack traces in error messages add -fno-omit-frame-pointer. 
        # To get perfect stack traces you may need to disable inlining (just use -O1) and 
        # tail call elimination (-fno-optimize-sibling-calls).
        target_compile_options(cpp_pong PRIVATE -fsanitize=address)
        target_link_options(cpp_pong PRIVATE -fsanitize=address)
    endif()

    # this will give us a soft enforcement of the c17 cpp20 standard, project won't error out, but 
    # because of our warning flags, we will still be told when we're using non-standard c    
    # mixing c / c++ standard can cause weird ABI issues when linking
    target_compile_features(cpp_pong PRIVATE c_std_17 cxx_std_20)
endif()

# CONFIGURE_DEPENDS flag makes it so that we don't have to run cmake when a new cpp file is added
file(GLOB cpp_pong_SRC CONFIGURE_DEPENDS "src/*.cpp")
target_sources(cpp_pong PRIVATE ${cpp_pong_SRC})

# .PHONY: lint
# lint: main.cpp
# 	cppcheck $^

# add clang-tidy-17 with all checks for cppcoreguidelines and probably some more
# docs for clang-tidy https://releases.llvm.org/17.0.1/tools/clang/tools/extra/docs/clang-tidy/index.html
