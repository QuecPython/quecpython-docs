# 开发第一个脚本

在开始编写代码前，请确保您已完成硬件、固件和软件环境的准备。

## 编写第一个脚本文件

打开VScode，输入下方字符串

```python
print("hello world")
```

<img src="media/VScode1.jpg" alt="image-2021081301" style="zoom:150%;" />

接下来将编写好的脚本保存在本地，将文件名命名为 `helloworld.py`（仅允许英文、数字和下划线，**禁止使用中文和特殊字符**）

<img src="media/VScode2.png" alt="image-2021081301" style="zoom:150%;"/>

下面通过QPYcom将上面编辑好的文件传输到模组中并运行

## 传输脚本文件到模组

在传输脚本文件前要确保QPYcom串口已打开

<img src="media/QPYcom1.png" alt="image-2021081301" style="zoom:150%;" />

**方法一:** 通过拖拽形式传输

打开QPYcom点击`文件`，点击图示上的按钮选择`helloworld.py`脚本文件路径

<img src="media/QPYcom2.png" alt="image-2021081301" style="zoom:150%;" />

选择存放`helloworld.py`脚本文件路径后，在左侧可以看到编写好的脚本文件

<img src="media/QPYcom3.png" alt="image-2021081301" style="zoom:150%;" />

将左侧`helloworld.py`脚本文件拖拽到右侧`usr`中进行传输，下载进度和结果会在下方状态栏显示<img src="media/QPYcom4.png" alt="image-2021081301" style="zoom:150%;" />

右侧usr显示`helloworld.py`说明成功传输到模组中

<img src="media/QPYcom5.png" alt="image-2021081301" style="zoom:150%;" />

**方法二**:通过工具按钮传输

点击右侧 "**+**"按钮进行传输文件

<img src="media/QPYcom6.png" alt="image-2021081301" style="zoom:150%;" />

找到编写好的`helloworld.py`文件，点击右下角**打开**

<img src="media/QPYcom7.png" alt="image-2021081301" style="zoom:150%;" />

右侧usr显示`helloworld.py`说明成功传输到模组中

<img src="media/QPYcom8.png" alt="image-2021081301" style="zoom:150%;" />

**方法三:**创建用户项目进行传输

点击**下载**页面

创建用户项目（点击"创建"按钮）

创建好项目名称，点击"OK"

<img src="media/QPYcom9.png" alt="image-2021081301" style="zoom:150%;" />

左侧显示创建好的项目

<img src="media/QPYcom10.png" alt="image-2021081301" style="zoom:150%;" />

在"用户脚本/文件"区域选择`usr`根目录鼠标右键**添加文件**

<img src="media/QPYcom11.png" alt="image-2021081301" style="zoom:150%;" />

添加成功后会显示准备好进行传输的`helloword.py`文件，点击右下角<img src="media\readme\QPYcom17.png"/>会出现“下载脚本“与”下载固件”，选择**下载脚本**进行传输

<img src="media/QPYcom12.jpg" alt="image-2021081301" style="zoom:150%;" />

下载好后会提示”文件下载成功“

<img src="media/QPYcom13.png" alt="image-2021081301" style="zoom:150%;" />

## 执行脚本文件

**方法一**：通过repl执行脚本

打开串口之后，在QPYcom交互页面输入以下代码执行脚本文件

```python
import example
example.exec("/usr/hello world.py") # filePath为要执行的脚本文件路径
```

**方法二**：通过GUI工具执行脚本

在工具的文件页面选择要执行的`helloworld.py`脚本文件然后点击<img src="media\readme\QPYcom18.png"/>按钮运行

<img src="media/QPYcom14.png" alt="image-2021081301" style="zoom:150%;" />

**方法三**：通过右键菜单运行脚本文件

选择脚本文件，右键点击运行

<img src="media/QPYcom15.png" alt="image-2021081301" style="zoom:150%;" />

**执行结果如图所示**

<img src="media/QPYcom16.png" alt="image-2021081301" style="zoom:150%;" />

## 停止程序运行

如何停止正在运行的程序,根据运行的脚本文件类型有以下方法:

<table valign="center">
	<thead>
    <tr>
		<th>程序名是否</br>为main.py</th>
		<th>程序中是否</br>包含了死循环</th>
		<th>程序中是否</br>使用了多线程</th>
		<th valign="middle">停止步骤</th>
	</tr>
	 </thead>
    <tr>
		<td rowspan="4">✓</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + A 键进入 RAW 模式</br>（2）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（3）按 Ctrl + B 键回到普通交互模式</br>（4）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + A 键进入 RAW 模式</br>（2）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（3）按 Ctrl + B 键回到普通交互模式</br>（4）若以上方法无效，请耐心等待程序运行结束</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件</td>
	</tr>
	<tr>
		<td rowspan="4">✗</td>
		<td rowspan="2">✓</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（2）若以上方法无效，请直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + D 键重启 QuecPython 虚拟机</br>（2）若以上方法无效，请直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="2">✗</td>
		<td rowspan="1">✓</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件或直接重启模块</td>
	</tr>
	<tr>
		<td rowspan="1">✗</td>
		<td>（1）按 Ctrl + C 键打断程序运行</br>（2）若以上方法无效，请重新烧录固件或直接重启模块</td>
	</tr>
</table>

## 常见问题与注意事项

### 1. 执行脚本无输出 / 提示文件不存在

- 路径拼写错误：REPL 执行`exec`函数时路径文件名大小写 / 拼写与模组内文件不一致；
- GUI 执行选错文件：未选中右侧`usr`目录下的目标脚本，点击执行按钮后无实际程序运行；
- 脚本语法错误：脚本内存在语法问题（如缺少括号、缩进错误），执行时静默失败无输出。

### 2. 脚本文件下载失败

- 串口未就绪：下载前未打开串口，或开发板未烧录对应固件 / 固件烧录失败，导致模组无响应；
- 命名 / 路径非法：文件名含中文、特殊字符，或本地文件路径选择错误；
- 拖拽操作不当：未将本地文件拖至右侧模组`usr`目录区域。