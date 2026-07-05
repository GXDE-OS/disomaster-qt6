cmake_minimum_required(VERSION 3.16)

# 查找 Qt5 Test 组件
find_package(Qt6 COMPONENTS Test REQUIRED)

# 测试源文件
add_executable(tests testdisomaster.cpp)

target_link_libraries(tests PRIVATE Qt6::Test disomaster)

# 注册测试到 CTest
enable_testing()
add_test(NAME tests COMMAND tests)