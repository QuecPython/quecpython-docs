# LVGL - Notice for Use

## Overview

During the development process when using Quecpython LVGL, you may encounter many problems. This chapter summarizes some common issues.

## Selection and Use of Images

> **Note:** Image decoding consumes RAM resources!

Images are inevitably used in the project GUI. Quecpython LVGL supports three image formats: PNG, JPG, and SJPG.

## Pros and Cons of Image Formats

### PNG

Pros

- Lossless format, the best image quality
- Support transparency

Cons

- Consume a lot of resources (RAM required for decoding: image width x image height x 4 bytes)
- Decoding takes longer (images stored in external 4-wire flash)

### JPG

Pros

- Consume fewer resources (RAM required for decoding: image width x image height x 3 bytes)
- Good image quality

Cons

- Not support transparency
- Image quality is worse than PNG and decoding consumes more resources than PNG

### SJPG

SJPG is a custom format based on the regular "JPG" format, which stands for split-JPEG. It is a small JPEG segment data with an SJPG header, which is almost the same size as a JPG file, or maybe slightly larger.

Pros

- Resource-saving (Decoding does not require a large RAM space)

Cons

- Not support transparency
- Poor image quality
- Slow loading if images are stored in external 4-wire flash
- Not suitable for image caching mechanism

## Selection of Image Formats

First, consider the transparency requirement. If transparency is required, select the PNG format directly.

Next, consider the following two scenarios.

### Images stored in built-in flash or external 6-wire flash

In this case, the flash access speed is fast, and the decoding speed difference is not significant. There is not much difference for you in the speed of loading the three image formats in the UI.

Please choose a format based on the requirements for image quality and resource allocation.

Generally, if image quality is not a high priority, SJPG can be chosen first, followed by JPG, and finally PNG.

If the module has sufficient RAM resources, PNG can be chosen first, followed by JPG, and finally SJPG.

Find a balance based on the actual situation of the project.

### Images stored in external 4-wire flash

In this case, the access speed of the external 4-wire flash is slow, and when there are many UI images, you will perceive a significant lag.

In this case, you need to enable the image caching mechanism and choose JPG or PNG. Please do not choose SJPG.

## Image Caching Mechanism

As mentioned earlier, the file system reading speed of the external 4-wire flash is relatively slow, and LVGL image decoding consumes much time, resulting in GUI display lag. The image caching mechanism of LVGL needs to be used to solve this problem.

### Usage

For JPG or PNG images, perform the following two steps before switching screens:

```python
lv.img.cache_invalidate_src(None)
lv.img.cache_set_size(cache_num)
```

`cache_num` - The number of images to be cached. Maximum value: 16.

> **Note:**
>
> - SJPG is not compatible with the image caching mechanism. This is because SJPG is essentially many small split images.
> - The imgbtn widget cannot use the image caching mechanism. Do not use the imgbtn widget!
> - Cached images will occupy RAM.

## LVGL Sleep Mode

First, when entering sleep mode, make sure that the peripherals have stopped interacting, the USB is disconnected, and the threads are blocked or suspended. Otherwise, it will not be possible to enter sleep mode.

Under the condition that the above conditions are met, the module generally follows the process below to enter sleep mode. Please correspondingly turn off the backlight according to the hardware design.

```python
import pm
import lvgl as lv

# Allow the module to enter sleep mode in idle state
pm.autosleep(1)
# Allow LVGL to enter sleep mode in idle state
lv.autoSleep(1)
# LCD enters sleep state
lcd.lcd_display_off()
# Touchpad enters sleep state if exists
tp.suspend()
# Close LCD backlight if it can be controlled
```

To exit sleep mode, perform the opposite operations as follows.

```python
import pm
import lvgl as lv

# Module exits from sleep mode
pm.autosleep(0)
# LVGL exits from sleep mode
lv.autoSleep(0)
# Wake LCD up
lcd.lcd_display_on()
# Wake touchpad up
tp.resume()
# Open LCD backlight if it can be controlled
```

## Other Notes

- Avoid widgets or styles being reclaimed by the gc module. Please define them as global variables.
- Do not perform too many time-consuming operations before switching screens, otherwise, lag may occur when you switch screens.
- Due to the underlying limitations of LVGL, do not add a style (add_style) to a widget multiple times. Adding styles more than 64 times will result in a dump.
- Image decoding consumes a large amount of heap. It is recommended to use fewer unnecessary large images. For example, a solid color background and a gradient color background can be set by using LVGL's background color, so there is no need to use images.