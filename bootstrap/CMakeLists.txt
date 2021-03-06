cmake_minimum_required(VERSION 2.8)

if(NOT DEFINED CMAKE_TOOLCHAIN_FILE)
  if(DEFINED ENV{DOLCESDK})
    set(CMAKE_TOOLCHAIN_FILE "$ENV{DOLCESDK}/share/dolce.toolchain.cmake" CACHE PATH "toolchain file")
  else()
    message(FATAL_ERROR "Please define DOLCESDK to point to your SDK path!")
  endif()
endif()

project(bootstrap)
include("${DOLCESDK}/share/dolce.cmake" REQUIRED)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -Wall -Os -Wno-unused-variable -Wno-unused-but-set-variable -Wno-implicit-function-declaration")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -fno-rtti -fno-exceptions")

include_directories(
)

link_directories(
  ${CMAKE_CURRENT_BINARY_DIR}
)

add_executable(bootstrap
  compress.c
  crc32.c
  bootstrap.c
)

target_link_libraries(bootstrap
  gcc
  SceLibKernel_stub
  SceIofilemgr_stub
  SceKernelThreadMgr_stub
  SceSysmem_stub
  SceProcessmgr_stub
  SceDisplay_stub
  SceNet_stub
  SceNetCtl_stub
  SceHttp_stub
  SceShellSvc_stub
  SceCtrl_stub
  SceSysmodule_stub
  SceAppMgr_stub
  ScePromoterUtil_stub
)

set_target_properties(bootstrap
  PROPERTIES LINK_FLAGS "-nostdlib"
)

dolce_create_self(bootstrap.self bootstrap UNSAFE)
