idf_component_register(
    SRCS
        "input.c"
        "main.c"
        "odroid_display.c"
        "odroid_sdcard.c"
    INCLUDE_DIRS "."
    REQUIRES
        app_update
        bootloader_support
        esp_adc
        esp_partition
        fatfs
        driver
        nvs_flash
        sdmmc
        spi_flash
        ugui
)

string(TIMESTAMP COMPILEDATE "%Y%m%d")
execute_process(
    COMMAND git rev-parse --short=10 HEAD
    WORKING_DIRECTORY "${CMAKE_CURRENT_LIST_DIR}/.."
    OUTPUT_VARIABLE GITREV
    OUTPUT_STRIP_TRAILING_WHITESPACE
    ERROR_QUIET
)

if(NOT GITREV)
    set(GITREV "unknown")
endif()

target_compile_definitions(${COMPONENT_LIB} PRIVATE
    COMPILEDATE="${COMPILEDATE}"
    GITREV="${GITREV}"
)
