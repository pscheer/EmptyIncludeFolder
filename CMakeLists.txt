cmake_minimum_required(VERSION 3.14)
set(this_target Server)
set(CMAKE_CXX_STANDARD 20)
project(${this_target} LANGUAGES CXX)

find_package(Threads REQUIRED)

include(FetchContent)

# Fetch ASIO (standalone)
FetchContent_Declare(
    asio
    GIT_REPOSITORY https://github.com/chriskohlhoff/asio.git
    GIT_TAG asio-1-28-0
)
FetchContent_MakeAvailable(asio)

# Fetch nlohmann/json
FetchContent_Declare(json URL https://github.com/nlohmann/json/releases/download/v3.11.3/json.tar.xz)
FetchContent_MakeAvailable(json)

add_executable (${this_target} main.cpp)

target_include_directories(${this_target} PRIVATE
     ${asio_SOURCE_DIR}/asio/include)

target_link_libraries(${this_target} PRIVATE 
    Threads::Threads 
    nlohmann_json::nlohmann_json)

