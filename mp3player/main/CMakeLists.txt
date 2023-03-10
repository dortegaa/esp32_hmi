idf_component_register(SRCS "ui_audio.c" "audio.c" "main.c"
                    INCLUDE_DIRS "." "include")
spiffs_create_partition_image(storage ../spiffs FLASH_IN_PROJECT)