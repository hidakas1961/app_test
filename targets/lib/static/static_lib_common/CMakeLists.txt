#==============================================================================
# スタティック共通ライブラリ
message(STATUS "ターゲット名：${TARGET_NAME}")
add_library(${TARGET_NAME} STATIC
  ${PROJECT_SOURCE_DIR}/src/lib/lib_common/core/lib_common.cpp
  ${PROJECT_SOURCE_DIR}/src/lib/lib_common/core/lib_common_sub.cpp
  ${PROJECT_SOURCE_DIR}/src/lib/lib_common/core/lib_common_c.c
  ${PROJECT_SOURCE_DIR}/res/lib/lib_common/res_lib_common.rc
)
target_include_directories(${TARGET_NAME} PRIVATE
  $<$<OR:$<COMPILE_LANGUAGE:C>,$<COMPILE_LANGUAGE:CXX>>:
    "${PROJECT_SOURCE_DIR}/inc"
    "${PROJECT_SOURCE_DIR}/inc/lib"
    "${PROJECT_SOURCE_DIR}/res"
    "${PROJECT_SOURCE_DIR}/res/lib"
  >
)
target_compile_definitions(${TARGET_NAME} PRIVATE
  $<$<OR:$<COMPILE_LANGUAGE:C>,$<COMPILE_LANGUAGE:CXX>>:
    -D "PROJECT_DIR=LR\"!raw(${PROJECT_SOURCE_DIR})!raw\""
    -D "PROJECT_NAME=L\"${PROJECT_NAME}\""
    -D "TARGET_NAME=L\"${TARGET_NAME}\""
    -D "PLATFORM_NAME=L\"${PLATFORM_NAME}\""
    -D LIB_COMMON_STATIC
    -D LIB_STATIC
  >
)
set(CMAKE_STATIC_LIBRARY_PREFIX "")
set(CMAKE_SHARED_LIBRARY_PREFIX "")
set(CMAKE_IMPORT_LIBRARY_PREFIX "")
add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
    COMMAND powershell -NoProfile -Command 
        "[Console]::OutputEncoding = [System.Text.Encoding]::UTF8; echo 'ターゲットファイルパス：$<TARGET_FILE:${TARGET_NAME}>'"
)
