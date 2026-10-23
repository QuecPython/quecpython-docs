# qrcode- 二维码显示

根据输入的内容，生成对应的二维码。

## 二维码显示功能

### `qrcode.show`

```python
qrcode.show(qrcode_str,magnification,start_x,start_y,Background_color,Foreground_color,Rotate)
```

显示二维码到LCD。

**参数描述：**

- `qrcode_str`-string类型，二维码内容;
- `magnification`-int类型，放大倍数[1,6];
- `start_x`-int类型，二维码显示起始x坐标;
- `start_y`-int类型，二维码显示起始y坐标;
- `Background_color`-int类型,前景色（不设置即默认为0xffff）;
- `Foreground_color`-int类型,背景色（不设置即默认为0x0000）。
- `Rotate`-int类型,旋转角度,[见常量](#常量)（不设置即默认为0）。

**返回值描述：**

`0`表示成功，`-1`表示生成二维码失败，`-2`表示放大失败，`-3`表示显示失败。

## 生成二维码数据

### `qrcode.getQRData`

```python
qrcode.getQRData(qrcode_str,magnification,Background_color,Foreground_color)
```

生成并获取二维码显示数据。

**参数描述：**

- `qrcode_str`-string类型，二维码内容;
- `magnification`-int类型，放大倍数[1,6];
- `Background_color`-int类型,前景色（不设置即默认为0xffff）;
- `Foreground_color`-int类型,背景色（不设置即默认为0x0000）。

**返回值描述：**

`-1`表示生成二维码失败。

成功返回元组`(side_length, data)`

- `side_length`：二维码边长。
- `data`：二维码显示数据。

## 常量

| 常量                | 值   | 说明                     |
| ------------------- | ---- | ------------------------ |
| `qrcode.ROTATE_0`   | 0    | 二维码不旋转显示         |
| `qrcode.ROTATE_90`  | 1    | 二维码顺时针旋转90°显示  |
| `qrcode.ROTATE_180` | 2    | 二维码顺时针旋转180°显示 |
| `qrcode.ROTATE_270` | 3    | 二维码顺时针旋转270°显示 |

