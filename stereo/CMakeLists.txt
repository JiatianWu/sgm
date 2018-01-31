cmake_minimum_required(VERSION 2.8)

project(deeprob_cv_stereo)
set(DEEPROB_CV_STEREO_VERSION_MAJOR 0)
set(DEEPROB_CV_STEREO_VERSION_MINOR 1)

file(GLOB DEEPROB_CV_STEREO_INC_FILES "include/*.h" "include/impl/*.hpp")
file(GLOB DEEPROB_CV_STEREO_SRC_FILES "src/*.cpp" "src/*.c")

add_library(${PROJECT_NAME} ${DEEPROB_LIBRARY_TYPE} ${DEEPROB_CV_STEREO_SRC_FILES} ${DEEPROB_CV_STEREO_INC_FILES})
# target_link_libraries(${PROJECT_NAME} ${Boost_LIBRARIES})

install(TARGETS ${PROJECT_NAME} DESTINATION lib)
install(DIRECTORY include DESTINATION include/${CMAKE_PROJECT_NAME}/cv/stereo)

add_subdirectory(test)
