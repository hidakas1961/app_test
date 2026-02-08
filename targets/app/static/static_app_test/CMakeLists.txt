#==============================================================================
# スタティックテストアプリケーション
#------------------------------------------------------------------------------
message(STATUS "ターゲット名：${TARGET_NAME}")
#------------------------------------------------------------------------------
# ソースファイル
add_executable(${TARGET_NAME}
  ${PROJECT_SOURCE_DIR}/src/app/program/program.cpp
  ${PROJECT_SOURCE_DIR}/src/app/app_test/core/app_test.cpp
  ${PROJECT_SOURCE_DIR}/src/app/app_test/core/app_test_sub.cpp
  ${PROJECT_SOURCE_DIR}/src/app/app_test/core/app_test_c.c
  ${PROJECT_SOURCE_DIR}/src/app/app_test/helper/app_test_helper.cpp
  ${PROJECT_SOURCE_DIR}/res/app/app_test/res_app_test.rc
)
#------------------------------------------------------------------------------
# インクルードディレクトリ
target_include_directories(${TARGET_NAME} PRIVATE
  $<$<OR:$<COMPILE_LANGUAGE:C>,$<COMPILE_LANGUAGE:CXX>>:
    "${PROJECT_SOURCE_DIR}/inc"
    "${PROJECT_SOURCE_DIR}/inc/app"
    "${PROJECT_SOURCE_DIR}/inc/lib"
    "${PROJECT_SOURCE_DIR}/res"
    "${PROJECT_SOURCE_DIR}/res/app"
    "${PROJECT_SOURCE_DIR}/res/lib"
  >
)
#------------------------------------------------------------------------------
# コンパイルオプション
target_compile_definitions(${TARGET_NAME} PRIVATE
  $<$<OR:$<COMPILE_LANGUAGE:C>,$<COMPILE_LANGUAGE:CXX>>:
    -D "PROJECT_DIR=LR\"!raw(${PROJECT_SOURCE_DIR})!raw\""
    -D "PROJECT_NAME=L\"${PROJECT_NAME}\""
    -D "TARGET_NAME=L\"${TARGET_NAME}\""
    -D "PLATFORM_NAME=L\"${PLATFORM_NAME}\""
    -D LIB_COMMON_STATIC
    -D APP_STATIC
  >
)
#------------------------------------------------------------------------------
# リンクファイル
target_link_libraries("${TARGET_NAME}" PRIVATE static_lib_common)
#------------------------------------------------------------------------------
# その他
set(CMAKE_STATIC_LIBRARY_PREFIX "")
set(CMAKE_SHARED_LIBRARY_PREFIX "")
set(CMAKE_IMPORT_LIBRARY_PREFIX "")
add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
    COMMAND powershell -NoProfile -Command 
        "[Console]::OutputEncoding = [System.Text.Encoding]::UTF8; echo 'ターゲットファイルパス：$<TARGET_FILE:${TARGET_NAME}>'"
)
