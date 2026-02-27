cmake_minimum_required(VERSION 3.16)
project(NotesApp LANGUAGES CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt6 REQUIRED COMPONENTS Widgets Test)

add_library(notes_core STATIC
    src/models/note.cpp
    src/models/label.cpp
    src/services/notemanager.cpp
    src/services/storageservice.cpp
)

target_include_directories(notes_core PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/src)
target_link_libraries(notes_core PUBLIC Qt6::Widgets)

add_executable(${PROJECT_NAME}
    src/main.cpp
    src/ui/mainwindow.cpp
    src/ui/notedialog.cpp
    src/ui/notecard.cpp
)

target_link_libraries(${PROJECT_NAME} PRIVATE notes_core)

enable_testing()

add_executable(test_note tests/test_note.cpp)
target_link_libraries(test_note PRIVATE notes_core Qt6::Test)
add_test(NAME TestNote COMMAND test_note)

add_executable(test_notemanager tests/test_notemanager.cpp)
target_link_libraries(test_notemanager PRIVATE notes_core Qt6::Test)
add_test(NAME TestNoteManager COMMAND test_notemanager)

add_executable(test_storageservice tests/test_storageservice.cpp)
target_link_libraries(test_storageservice PRIVATE notes_core Qt6::Test)
add_test(NAME TestStorageService COMMAND test_storageservice)
