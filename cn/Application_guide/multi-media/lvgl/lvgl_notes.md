# LVGL-注意事项

## 概述

在实际使用Quecpython LVGL的开发过程中，客户可能会遇到很多问题，本章总结一些开发者遇到的常见问题。

## 图片的选择与使用

> **注意：**图片解码消耗RAM资源！

项目GUI不可避免要用到图片，Quecpython LVGL提供了三种图片格式的支持，分别是PNG，JPG和SJPG。

## 图片格式的优缺点

### PNG

优点

- 无损格式，图片质量较好

- 支持透明

缺点

- 解码需要RAM资源大（图像宽度 x 图像高度 x 4字节）
- 解码时间较长（图片放四线外挂flash）

### JPG

优点

- 较节省资源（解码需RAM：图像宽度 x 图像高度 x 3字节）
- 图片质量较好

缺点

- 不支持透明
- 优点同时也是缺点，画质比PNG差，解码消耗资源也较大

### SJPG

SJPG 是LVGL自定义的一种基于普通” JPG 的格式，全称split-jpeg ，它是一段带有 sjpg 标头的小 jpeg 片段数据，它的大小将几乎与 jpg 文件相当，或者可能会稍大一些。

优点

- 节省资源（解码不需申请较大RAM空间）

缺点

- 不支持透明
- 画质较差
- 若图片放四线外挂flash，则加载慢
- 不适用图片缓存机制

## 图片格式的选择

首先看透明度需求，有透明度需求的直接选择PNG格式。

其次得分以下两种情况说明。

### 图片放内置flash或外挂六线flash

这种情况下，flash访问速度快，解码速度差距不大，对于客户而言，UI加载三种格式图片的速度差距不大。

请根据对画质的需求和资源分配权衡选择。

一般而言，若画质需求不强烈，可以选择SJPG，其次选择JPG，最后PNG；

若模块RAM资源富余，可以选择PNG，其次JPG，最后SJPG。

需要根据项目实际情况找到一个平衡点。

### 图片放外挂四线flash

外挂四线flash访问速度慢，UI图片较多的时候，对于客户而言，卡顿感知明显。

这种情况下，需要打开图片缓存机制，选择JPG或PNG格式，不能选择SJPG！

## 图片缓存机制

上文说到，外挂四线flash文件系统读取速度比较慢，LVGL图片解码也比较耗时，导致GUI显示卡顿。需要使用LVGL的图片缓存机制来解决。

### 使用方法

使用JPG或者PNG格式图片，在切换界面之前执行以下两步：

```python
lv.img.cache_invalidate_src(None)
lv.img.cache_set_size(cache_num)
```

`cache_num` - 需要缓存的图片数目，最大16。

> **注意：**
>
> - SJPG与图片缓存机制不兼容！因为SJPG的实质是很多块小图片。
> - imgbtn组件不能使用图片缓存机制，不要使用imgbtn组件！
> - 缓存的图片会占据RAM。

## LVGL休眠场景

首先，在需要进入休眠模式的时候，务必确认外设是否停止交互，USB是否已断开，线程是否都已阻塞或挂起，否则无法进入休眠模式。

在确保上述条件满足的情况下，模块一般采取以下流程进休眠，其中背光关闭请根据硬件设计来对应的关闭。

```python
import pm
import lvgl as lv

# Allow module to enter low-power mode in idle state
pm.autosleep(1)
# Allow LVGL to enter low-power mode in idle state
lv.autoSleep(1)
# LCD enters sleep state
lcd.lcd_display_off()
# Touchpad enters sleep state if exists
tp.suspend()
# Close LCD backlight if it can be controlled
```

退出休眠的时候执行相反的操作，如下。

```python
import pm
import lvgl as lv

# Module exits from low-power mode
pm.autosleep(0)
# LVGL exits from low-power mode
lv.autoSleep(0)
# Wake LCD up
lcd.lcd_display_on()
# Wake touchpad up
tp.resume()
# Open lcd backlight if it can be controlled
```

## 其他注意事项

- 避免组件或者样式被gc回收，请将其定义为全局变量。
- 切换界面之前不要执行太多耗时操作，否则可能造成切换页面卡顿。
- 因LVGL底层限制，不要给一个组件多次的添加样式(add_style)，超过64次会dump。
- 从heap消耗上看，图片解码会消耗大量的heap，建议尽量少用不必要的大图片。比如纯色背景和渐变色背景都可以使用lvgl的背景色来完成，没必要使用图片。