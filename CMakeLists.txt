set(SRC_FILES
    "port/esp/esp_hal_misc.cpp"
    "port/esp/esp_hal_serial.cpp"
    "port/esp/esp_hal_gpio.cpp"
    "port/esp/esp_hal_bldc_3pwm.cpp"
    "port/esp/esp_app_fmath.cpp"
    "port/esp/esp_app_print.cpp"
    "port/angle_sensor/as5600.cpp"
    "port/angle_sensor/mt6701.cpp"
    "port/angle_sensor/as5048a.cpp")

if(CONFIG_SOC_MCPWM_SUPPORTED)
    list(APPEND SRC_FILES "port/esp/esp_hal_bldc_6pwm.cpp")
endif()

set(INC_FILES "port/esp/include/" "port/angle_sensor")
set(IGNORE_FILES "port/esp/esp_hal_adc.cpp" "port/esp/esp_hal_bldc_6pwm.cpp"
                 "port/esp/esp_hal_bldc_3pwm.cpp" "port/angle_sensor/mt6701.cpp" "port/angle_sensor/as5048a.cpp")

idf_component_register(
  SRCS
  ${SRC_FILES}
  INCLUDE_DIRS
  ${INC_FILES}
  REQUIRES
  driver
  esp_timer
  esp_adc)

set_source_files_properties(
  ${IGNORE_FILES}
  PROPERTIES
    COMPILE_OPTIONS
    "-Wno-volatile;-Wno-deprecated-enum-float-conversion;-Wno-missing-field-initializers"
)

idf_component_get_property(arduino-foc_lib zzy__arduino-foc COMPONENT_LIB)
cmake_policy(SET CMP0079 NEW)
target_link_libraries(${arduino-foc_lib} PUBLIC ${COMPONENT_LIB})

target_compile_definitions(${COMPONENT_LIB} PUBLIC ESP_H ARDUINO_ARCH_ESP32)
