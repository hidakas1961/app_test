#==============================================================================
# ダイナミックプログラム
message(STATUS "ターゲット名：${TARGET_NAME}")
add_executable(${TARGET_NAME}
  ${PROJECT_SOURCE_DIR}/src/app/program/program.cpp
  ${PROJECT_SOURCE_DIR}/src/app/program/program_main.cpp
  ${PROJECT_SOURCE_DIR}/src/app/program/program_sub.cpp
  ${PROJECT_SOURCE_DIR}/src/app/program/program_c.c
  ${PROJECT_SOURCE_DIR}/res/app/program/res_program.rc
)
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
target_compile_definitions(${TARGET_NAME} PRIVATE
  $<$<OR:$<COMPILE_LANGUAGE:C>,$<COMPILE_LANGUAGE:CXX>>:
    -D "PROJECT_DIR=LR\"!raw(${PROJECT_SOURCE_DIR})!raw\""
    -D "PROJECT_NAME=L\"${PROJECT_NAME}\""
    -D "TARGET_NAME=L\"${TARGET_NAME}\""
    -D "PLATFORM_NAME=L\"${PLATFORM_NAME}\""
    -D APP_DYNAMIC
  >
)
set(CMAKE_STATIC_LIBRARY_PREFIX "")
set(CMAKE_SHARED_LIBRARY_PREFIX "")
set(CMAKE_IMPORT_LIBRARY_PREFIX "")
add_custom_command(TARGET ${TARGET_NAME} POST_BUILD
    COMMAND powershell -NoProfile -Command 
        "[Console]::OutputEncoding = [System.Text.Encoding]::UTF8; echo 'ターゲットファイルパス：$<TARGET_FILE:${TARGET_NAME}>'"
)
