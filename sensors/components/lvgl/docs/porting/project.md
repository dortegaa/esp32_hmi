```eval_rst
.. include:: /header.rst 
:github_url: |github_link_base|/porting/project.md
```

# Set up a project

## Get the library

LVGL is available on GitHub: [https://github.com/lvgl/lvgl](https://github.com/lvgl/lvgl).

You can clone it or download the latest version of the library from GitHub.

The graphics library itself is the **lvgl** directory which should be copied into your project.

## Configuration file

There is a configuration header file for LVGL called **lv_conf.h**. You modify this header to set the library's basic behavior, disable unused modules and features, adjust the size of memory buffers in compile-time, etc.

Copy **lvgl/lv_conf_template.h** next to the *lvgl* directory and rename it to *lv_conf.h*. Open the file and change the `#if 0` at the beginning to `#if 1` to enable its content.

Comments in the config file explain the meaning of the options. Be sure to set at least `LV_COLOR_DEPTH` according to your display's color depth.

Alternatively, `lv_conf.h` can be copied to another place but then you should add the `LV_CONF_INCLUDE_SIMPLE` define to your compiler options (e.g. `-DLV_CONF_INCLUDE_SIMPLE` for GCC compiler) and set the include path manually (e.g. `-I../include/gui`). 
In this case LVGL will attempt to include `lv_conf.h` simply with `#include "lv_conf.h"`.

You can even use a different name for `lv_conf.h`. The custom path can be set via the `LV_CONF_PATH` define.
For example `-DLV_CONF_PATH="/home/joe/my_project/my_custom_conf.h"`

If `LV_CONF_SKIP` is defined, LVGL will not try to include `lv_conf.h`. Instead you can pass the config defines using build options. For example `"-DLV_COLOR_DEPTH=32 -DLV_USE_BTN 1"`. The unset options will get a default value which is the same as the ones in `lv_conf_template.h`.

LVGL also can be used via `Kconfig` and `menuconfig`.  You can use `lv_conf.h` together with Kconfig, but keep in mind that the value from `lv_conf.h` or build settings (`-D...`) overwrite the values set in Kconfig. To ignore the configs from `lv_conf.h` simply remove its content, or define `LV_CONF_SKIP`. 



## Initialization

To use the graphics library you have to initialize it and setup required components. The order of the initialization is:

1. Call `lv_init()`.
2. Initialize your drivers.
3. Register the display and input devices drivers in LVGL.  Learn more about [Display](/porting/display) and [Input device](/porting/indev) registration.
4. Call `lv_tick_inc(x)` every `x` milliseconds in an interrupt to report the elapsed time to LVGL. [Learn more](/porting/tick).
5. Call `lv_timer_handler()` every few milliseconds to handle LVGL related tasks. [Learn more](/porting/task-handler).
