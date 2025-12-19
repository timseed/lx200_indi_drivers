cmake_minimum_required(VERSION 3.0)
# Set up the project
project(indi_lx200generic)
# INDI and OpenCV flags
# Find pkg-config system tool
# We need this tooling
find_package(PkgConfig REQUIRED)
#Now using PkgConfig find libindi
pkg_check_modules(INDI REQUIRED libindi)
# INDI_CFLAGS is now available as INDI_CFLAGS or INDI_CFLAGS_OTHER
message(STATUS "INDI CFLAGS: ${INDI_CFLAGS}")

set(SRC lx200_10micron.cpp  lx200ap_gtocp2.cpp  lx200classic.cpp  lx200generic.cpp   lx200_OpenAstroTech.cpp   lx200_TeenAstro.cpp
        lx200_16.cpp        lx200ap_v2.cpp      lx200driver.cpp   lx200gotonova.cpp  lx200_pegasus_nyx101.cpp  lx200telescope.cpp
        lx200am5.cpp        lx200autostar.cpp   lx200fs2.cpp      lx200gps.cpp       lx200pulsar2.cpp          lx200zeq25.cpp
        lx200apdriver.cpp   lx200basic.cpp      lx200gemini.cpp   lx200_OnStep.cpp   ioptronHC8406.cpp         eq500x.cpp
        lx200ss2000pc.cpp
)

set(HDR lx200_10micron.h lx200_16.h lx200am5.h lx200apdriver.h lx200ap_gtocp2.h lx200ap_v2.h lx200autostar.h
        lx200basic.h lx200classic.h lx200driver.h lx200fs2.h lx200gemini.h lx200generic.h lx200gotonova.h
        lx200gps.h lx200_OnStep.h lx200_OpenAstroTech.h lx200_pegasus_nyx101.h lx200pulsar2.h ioptronHC8406.h  eq500x.h
        lx200_OnStep.h lx200ss2000pc.h)


add_library(inovasdk SHARED IMPORTED)

set_target_properties(inovasdk PROPERTIES
    IMPORTED_LOCATION /lib/x86_64-linux-gnu/libnova-0.16.so.0 
    INTERFACE_INCLUDE_DIRECTORIES /home/tim/Dev/C++/Astro/indi_3party/libinovasdk
)

add_library(indidriver SHARED IMPORTED)
set_target_properties(indidriver PROPERTIES
    IMPORTED_LOCATION /usr/local/lib/libindidriver.so
)

add_executable(lx200_generic ${SRC}
                          ${HDR}        # optional, just for IDEs
              )

# --- Optional: add compile flags from INDI pkg-config ---
target_compile_options(lx200_generic PRIVATE ${INDI_CFLAGS_OTHER})

# --- Include directories ---
target_include_directories(lx200_generic PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}  # your headers live here
    ${INDI_INCLUDE_DIRS}         # INDI includes
)

message(INFO "Link libs: ${INDI_LIBRARIES}")
target_link_directories(lx200_generic PRIVATE /usr/local/lib)
target_link_libraries(lx200_generic PRIVATE ${INDI_LIBRARIES} indidriver inovasdk )

# create symlink AFTER the executable is built
add_custom_command(TARGET lx200_generic POST_BUILD
    COMMAND ${CMAKE_COMMAND} -E create_symlink
            $<TARGET_FILE:lx200_generic>   # the actual executable
            ${CMAKE_BINARY_DIR}/indi_lx200_OnStep
    COMMENT "Creating symbolic link to lx200_generic"
)

