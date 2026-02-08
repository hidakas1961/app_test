#==============================================================================
# CMakeリスト
#------------------------------------------------------------------------------
# ビルド実行コマンド ※アイコンボタンだとうまくビルド出来ないので、コマンドラインで実行する
#------------------------------------------------------------------------------
# デバッグビルド：cmake --workflow --preset build-debug 
# リリースビルド：cmake --workflow --preset build-release
# すべてのビルド：cmake --workflow --preset all-configs
# すべての削除　：cmake --build _build --config Debug --target clean; cmake --build _build --config Release --target clean
#------------------------------------------------------------------------------
# CMake最低条件
cmake_minimum_required(VERSION 3.20)
#------------------------------------------------------------------------------
# C++バージョン条件
set(CMAKE_CXX_STANDARD 26)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
#------------------------------------------------------------------------------
# コンパイルオプション
add_compile_options(-save-temps)
#------------------------------------------------------------------------------
# ユニコード設定
add_compile_options(
  -DUNICODE -D_UNICODE
)
add_link_options(
  -municode
)
#------------------------------------------------------------------------------
# ビルド環境変数設定
get_filename_component(PROJECT_NAME "${CMAKE_SOURCE_DIR}" NAME)
project(${PROJECT_NAME})
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
  set(PLATFORM_NAME "x64")
elseif(CMAKE_SIZEOF_VOID_P EQUAL 4)
  set(PLATFORM_NAME "x86")
else()
    set(PLATFORM_NAME ${CMAKE_SYSTEM_PROCESSOR})
endif()
message(STATUS "ルートディレクトリ：${CMAKE_SOURCE_DIR}")
message(STATUS "プロジェクトディレクトリ：${PROJECT_SOURCE_DIR}")
message(STATUS "プロジェクト名：${PROJECT_NAME}")
message(STATUS "プラットフォーム：${PLATFORM_NAME}")
#------------------------------------------------------------------------------
# バイナリ出力パス設定
set(PRE_DIR "${CMAKE_BINARY_DIR}/../_out")
set(POST_DIR "${PLATFORM_NAME}/$<CONFIG>")
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${PRE_DIR}/bin/${POST_DIR})
set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${PRE_DIR}/obj/${POST_DIR})
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${PRE_DIR}/obj/${POST_DIR})
message(STATUS "CMAKE_RUNTIME_OUTPUT_DIRECTORY：${CMAKE_RUNTIME_OUTPUT_DIRECTORY}")
message(STATUS "CMAKE_ARCHIVE_OUTPUT_DIRECTORY：${CMAKE_ARCHIVE_OUTPUT_DIRECTORY}")
message(STATUS "CMAKE_LIBRARY_OUTPUT_DIRECTORY：${CMAKE_LIBRARY_OUTPUT_DIRECTORY}")

#==============================================================================
# ダイナミックアプリケーションターゲット追加
#------------------------------------------------------------------------------
# ダイナミック共通ライブラリ
set(TARGET_NAME "lib_common")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/lib/dynamic/${TARGET_NAME}")
#------------------------------------------------------------------------------
# ダイナミックプログラム
set(TARGET_NAME "program")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/app/dynamic/${TARGET_NAME}")
#------------------------------------------------------------------------------
# ダイナミックテストアプリケーション
set(TARGET_NAME "app_test")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/app/dynamic/${TARGET_NAME}")

#==============================================================================
# スタティックアプリケーションターゲット追加
#------------------------------------------------------------------------------
# スタティック共通ライブラリ
set(TARGET_NAME "static_lib_common")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/lib/static/${TARGET_NAME}")
#------------------------------------------------------------------------------
# スタティックプログラム
set(TARGET_NAME "static_program")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/app/static/${TARGET_NAME}")
#------------------------------------------------------------------------------
# スタティックテストアプリケーション
set(TARGET_NAME "static_app_test")
add_subdirectory("${PROJECT_SOURCE_DIR}/targets/app/static/${TARGET_NAME}")
