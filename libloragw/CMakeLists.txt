cmake_minimum_required(VERSION 3.8)
project(libloragw)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)

# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

include_directories(include/libloragw)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -O2 -Wall -Wextra -std=c99")

add_library(libloragw STATIC
  src/loragw_ad5338r.c
  src/loragw_aux.c
  src/loragw_cal.c
  src/loragw_com.c
  src/loragw_debug.c
  src/loragw_gps.c
  src/loragw_hal.c
  src/loragw_i2c.c
  src/loragw_lbt.c
  src/loragw_mcu.c
  src/loragw_reg.c
  src/loragw_spi.c
  src/loragw_stts751.c
  src/loragw_sx125x.c
  src/loragw_sx1250.c
  src/loragw_sx1261.c
  src/loragw_sx1302_rx.c
  src/loragw_sx1302_timestamp.c
  src/loragw_sx1302.c
  src/loragw_usb.c
  src/sx125x_com.c
  src/sx125x_spi.c
  src/sx1250_com.c
  src/sx1250_spi.c
  src/sx1250_usb.c
  src/sx1261_com.c
  src/sx1261_spi.c
  src/sx1261_usb.c
)

target_link_libraries(libloragw
  PUBLIC
    m
    pthread
    rt
)

add_executable(test_loragw_com tst/test_loragw_com.c)

target_link_libraries(test_loragw_com
  PUBLIC
    libloragw
)

ament_export_include_directories(include)
ament_export_libraries(libloragw)
ament_export_dependencies(rclcpp)
ament_export_targets(export_libloragw HAS_LIBRARY_TARGET)

install(
  DIRECTORY include/
  DESTINATION include
)

install(
  TARGETS libloragw
  EXPORT export_libloragw
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  INCLUDES DESTINATION include
)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()

ament_package()
