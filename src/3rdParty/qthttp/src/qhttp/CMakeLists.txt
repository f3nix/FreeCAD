set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

set(QtHttp_SRCS
    qhttp.cpp
    qhttpauthenticator.cpp)

add_library(QtHttp SHARED ${QtHttp_SRCS})

target_link_libraries(QtHttp Qt5::Core Qt5::Network)

install(TARGETS QtHttp LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR})
