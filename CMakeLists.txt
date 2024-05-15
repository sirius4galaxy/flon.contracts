cmake_minimum_required(VERSION 3.5)

project(fullon_contracts)

set(VERSION_MAJOR 0)
set(VERSION_MINOR 4)
set(VERSION_PATCH 0)
set(VERSION_SUFFIX alpha1)

if(VERSION_SUFFIX)
  set(VERSION_FULL "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}-${VERSION_SUFFIX}")
else()
  set(VERSION_FULL "${VERSION_MAJOR}.${VERSION_MINOR}.${VERSION_PATCH}")
endif()

message(STATUS "Building fullon.contracts v${VERSION_FULL}")

include(ExternalProject)

find_package(cdt)

option(SYSTEM_CONFIGURABLE_WASM_LIMITS
       "Enables use of the host functions activated by the CONFIGURABLE_WASM_LIMITS protocol feature" ON)

option(SYSTEM_BLOCKCHAIN_PARAMETERS
       "Enables use of the host functions activated by the BLOCKCHAIN_PARAMETERS protocol feature" ON)


ExternalProject_Add(
   amax_contracts_project
   SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/contracts
   BINARY_DIR ${CMAKE_CURRENT_BINARY_DIR}/contracts
   INSTALL_DIR ${CMAKE_CURRENT_BINARY_DIR}/install
   CMAKE_ARGS
              -DCMAKE_BUILD_TYPE=Release
              -DCMAKE_TOOLCHAIN_FILE=${CDT_ROOT}/lib/cmake/cdt/CDTWasmToolchain.cmake
              -DSYSTEM_CONFIGURABLE_WASM_LIMITS=${SYSTEM_CONFIGURABLE_WASM_LIMITS}
              -DSYSTEM_BLOCKCHAIN_PARAMETERS=${SYSTEM_BLOCKCHAIN_PARAMETERS}
              -DCMAKE_INSTALL_PREFIX:PATH=<INSTALL_DIR>
  #  DEPENDS update_version
   UPDATE_COMMAND ""
   PATCH_COMMAND ""
   TEST_COMMAND ""
   BUILD_ALWAYS 1
)

# Actually install subproject.
install(
    DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/install/
    DESTINATION "${CMAKE_INSTALL_PREFIX}"
    USE_SOURCE_PERMISSIONS # Remain permissions (rwx) for installed files
)

if(APPLE)
  set(OPENSSL_ROOT "/usr/local/opt/openssl")
elseif(UNIX)
  set(OPENSSL_ROOT "/usr/include/openssl")
endif()
set(SECP256K1_ROOT "/usr/local")

if(APPLE)
  set(OPENSSL_ROOT "/usr/local/opt/openssl")
elseif(UNIX)
  set(OPENSSL_ROOT "/usr/include/openssl")
endif()
set(SECP256K1_ROOT "/usr/local")

option(BUILD_TESTS "Build unit tests" OFF)

if(BUILD_TESTS)
  message(STATUS "Building unit tests.")
  add_subdirectory(tests)
else()
  message(STATUS "Unit tests will not be built. To build unit tests, set BUILD_TESTS to ON.")
endif()
