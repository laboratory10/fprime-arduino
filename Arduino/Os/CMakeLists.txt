####
# ArduinoOs:
#
# The Arudino Os support layer uses the F prime baremetal virtualization technology and a custom
# rebuild of the Arduino framework in order to ensure that all the code can compile into a sensible
# binary. This file sets up the Os layer to support all of that.
####
restrict_platforms(ArduinoFw)
add_custom_target("${FPRIME_CURRENT_MODULE}")

arduino_lib_installed("SD" LIB_SD_INSTALLED)
option(FPRIME_ARDUINO_ENABLE_SD "Enable SD-backed OS implementations" OFF)

# Set up Arduino implementations
register_os_implementation("Console" Arduino)
register_os_implementation("RawTime" Arduino)
if (FPRIME_ARDUINO_ENABLE_SD)
    if (NOT LIB_SD_INSTALLED)
        message(FATAL_ERROR "FPRIME_ARDUINO_ENABLE_SD is ON but the Arduino SD library is not installed")
    endif()
    target_use_arduino_libraries("SD")
    register_os_implementation("File;FileSystem;Directory" Arduino_SD)
endif()
