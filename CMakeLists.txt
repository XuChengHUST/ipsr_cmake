cmake_minimum_required(VERSION 3.14)

project(
    ipsr
    VERSION 0.1.0
    DESCRIPTION "Iterative Poisson Surface Reconstruction (iPSR) for Unoriented Points"
    HOMEPAGE_URL "https://lcs.ios.ac.cn/~houf/pages/ipsr/index.html"
    LANGUAGES CXX
)

# ---- Declare library ----

add_library(
    ipsr_lib OBJECT
    Src/PlyFile.cpp
)

target_include_directories(
    ipsr_lib ${warning_guard}
    PUBLIC
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/source>"
)

target_compile_features(ipsr_lib PUBLIC cxx_std_11)

# ---- Declare executable ----

add_executable(ipsr_exe Src/main.cpp)
add_executable(ipsr::exe ALIAS ipsr_exe)

set_target_properties(
    ipsr_exe PROPERTIES
    OUTPUT_NAME ipsr
    EXPORT_NAME exe
)

if(MSVC)
    target_compile_definitions(ipsr_exe PRIVATE _CRT_SECURE_NO_WARNINGS)
    target_compile_options(ipsr_exe PRIVATE /bigobj)
endif(MSVC)

target_compile_features(ipsr_exe PRIVATE cxx_std_11)
target_include_directories(
    ipsr_exe
    PUBLIC
    "$<BUILD_INTERFACE:${PROJECT_SOURCE_DIR}/Src>"
)
target_link_libraries(ipsr_exe PRIVATE ipsr_lib)