



# UIautomator2 



[![Build Status](https://travis-ci.org/openatx/uiautomator2.svg?branch=master)](https://travis-ci.org/openatx/uiautomator2) [![PyPI](https://img.shields.io/pypi/v/uiautomator2.svg)](https://pypi.python.org/pypi/uiautomator2) ![PyPI](https://img.shields.io/pypi/pyversions/uiautomator2.svg) [![Windows Build](https://ci.appveyor.com/api/projects/status/github/openatx/uiautomator2)](https://ci.appveyor.com/project/openatx/uiautomator2)



**该项目正在火热的开发中** QQ群号: *499563266*

<p align="left"><img src="docs/img/qqgroup.png" /></div>

uiautomator2 是一个Android UI自动化框架，支持Python编写测试脚本对设备进行自动化。底层基于Google uiautomator，Google提供的[uiautomator](https://developer.android.com/training/testing/ui-automator.html)库可以获取屏幕上任意一个APP的任意一个控件属性，并对其进行任意操作，但有两个缺点：1. 测试脚本只能使用Java语言 2. 测试脚本必须每次被上传到设备上运行。
我们希望测试能够用一个更脚本化的语言，例如Python编写，同时可以每次所见即所得地修改测试、运行测试。这里要非常感谢 Xiaocong He ([@xiaocong][])，他将这个想法实现了出来（见[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)），原理是在手机上运行了一个http服务器，将uiautomator中的功能开放出来，然后再将这些http接口，封装成Python库。
我们的uiautomator2项目是对[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)的增强，主要有以下部分：

- 设备和开发机可以脱离数据线，通过WiFi互联（基于[atx-agent](https://github.com/openatx/atx-agent)）
- 集成了[openstf/minicap](https://github.com/openstf/minicap)达到实时屏幕投频，以及实时截图
- 集成了[openstf/minitouch](https://github.com/openstf/minitouch)达到精确实时控制设备
- 修复了[xiaocong/uiautomator](https://github.com/xiaocong/uiautomator)经常性退出的问题
- 代码进行了重构和精简，方便维护
- Requirements: `Android >= 4.4` `Python >=2.7 || <= 3.7`

虽然我说的很简单，但是实现起来用到了很多的技术和技巧，功能非常强，唯独文档有点少。哈哈

- 开源需要大家的贡献！
  - `大神这个能不能加上` -> `大神我加了个这个，PR review一下` https://github.com/openatx/uiautomator2/pull/157
  - `大神没找到文档啊` -> `大神我把xxx这里写了一下，PR了合并一下吧` https://github.com/openatx/uiautomator2/pull/260
  - `大神我这个手机上跑不了` -> `大神，我适配了xxx，PR合并一下吧` https://github.com/openatx/uiautomator2/pull/163
  - `大神我是小白，这个怎么用啊` -> `大神我是小白看了文档试用了一下，这个是我在TesterHome分享的踩坑贴，有些可以并到README`
  - `大神这个是不是一直免费啊` -> `大神我能做什么` https://github.com/openatx/uiautomator2/projects/1

**[安装](#installation)**

**[连接设备](#connect-to-a-device)**

**[命令行工具](#command-line)**

**[全局设定](#global-settings)**

- **[调试HTTP请求](#debug-http-requests)**
- **[隐式等待](#implicit-wait)**

**[App management](#app-management)**

- **[Install an app](#install-an-app)**
- **[Launch an app](#launch-an-app)**
- **[Stop an app](#stop-an-app)**
- **[Stop all running apps](#stop-all-running-apps)**
- **[Push and pull files](#push-and-pull-files)**
- **[Auto click permission dialogs](#auto-click-permission-dialogs)**

**[UI automation](#basic-api-usages)**

- **[Shell commands](#shell-commands)**
- **[Session](#session)**
- **[Retrieve the device info](#retrieve-the-device-info)**
- **[Key Events](#key-events)**
- **[Gesture interaction with the device](#gesture-interaction-with-the-device)**
- **[Screen-related](#screen-related)**
- **[Selector](#selector)**
- **[Watcher](#watcher)**
- **[Global settings](#global-settings)**
- **[Input method](#input-method)**
- **[Toast](#toast)**
- **[XPath](#xpath)**

**[常见问题](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**

- **[502错误](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**
- **[Connection Error](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**
- **[深度睡眠](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**
- **[Testerhome问题收集贴](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**
- **[点击偏差](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**
- **[释放AccessibilityService](https://github.com/openatx/uiautomator2/wiki#%E5%B8%B8%E8%A7%81%E9%97%AE%E9%A2%98-testerhome%E8%AE%A8%E8%AE%BA%E5%B8%96)**

**[实验性功能](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**

- **[远程投屏](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[插上自动init](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[htmlreport](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[诊断uiautomator2方法](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[Plugin](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[Hooks](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**
- **[失败时弹出提示框](https://github.com/openatx/uiautomator2/wiki#%E5%AE%9E%E9%AA%8C%E6%80%A7%E5%8A%9F%E8%83%BD)**

**[项目历史](#项目历史)**

**[Contributors](#contributors)**

**[LICENSE](#license)**



## 安装

1. **安装 uiautomator2**

   ```bash
   # 由于uiautomator2依然处于开发状态，您需要添加 --pre 才可以安装开发版本
   pip install --upgrade --pre uiautomator2
   
   # 或者直接用github源代码下载
   git clone https://github.com/openatx/uiautomator2
   pip install -e uiautomator2
   ```

   项目需要使用 `pillow` 来处理推向信息（可选）。

   ```bash
   pip install pillow
   ```

2. **在设备上安装demean程序**
   电脑连接上一个手机或多个手机, 确保adb已经添加到环境变量中，执行下面的命令会自动安装本库所需要的设备端程序：[uiautomator-server](https://github.com/openatx/android-uiautomator-server/releases) 、[atx-agent](https://github.com/openatx/atx-agent)、[openstf/minicap](https://github.com/openstf/minicap)、[openstf/minitouch](https://github.com/openstf/minitouch)

   ```bash
   # init 所有的已经连接到电脑的设备
   python -m uiautomator2 init
   ```

   > 有时候init也会出错，请参考[手动Init指南](https://github.com/openatx/uiautomator2/wiki/Manual-Init)

   安装提示`success`代表已安装完成。

3. **安装 weditor (UI 查看器)**

   因为uiautomator是独占资源，所以当atx运行的时候uiautomatorviewer是不能用的，为了减少atx频繁的启停，我们开发了基于浏览器技术的weditor UI查看器。<https://github.com/openatx/weditor>

   安装方法(备注: 目前最新的稳定版为 0.1.0)

   ```bash
   pip install -U weditor
   ```

   Windows系统可以使用命令在桌面创建一个快捷方式 `python -m weditor --shortcut` 。

   命令行启动 `python -m weditor` 会自动打开浏览器，输入设备的ip或者序列号，点击**Connect**即可。

   > 具体参考文章：[浅谈自动化测试工具python-uiautomator2](https://testerhome.com/topics/11357)

4. **【推荐】AppetizerIO 所见即所得脚本编辑器**

   [AppetizerIO](https://www.appetizer.io) 提供了对uiautomator2的深度集成，可以图形化管理ATX设备，还有所见即所得脚本编辑器

   - 到网站下载直接打开，首次使用需要注册账号
   - `设备管理` 界面里可以检查设备是否正常init，起停atx-agent，抓取atx-agent.log文件
   - `APP测试->脚本助手`调出脚本助手，实时界面同步，点击界面直接插入各种代码，同时支持uiautomator和Appium
   - **[视频教程 请戳这里](https://github.com/openatx/uiautomator2/wiki/Appetizer%E6%89%80%E8%A7%81%E5%8D%B3%E6%89%80%E5%BE%97u2%E8%84%9A%E6%9C%AC%E7%BC%96%E8%BE%91%E5%99%A8)**  [其他文档在此](http://doc.appetizer.io)

## 连接设备

连接设备有两种方法：

1. **通过WIFI连接（推荐）**

   假设设备的IP地址是 `10.0.0.1`，与您的PC机在同一个网段内。

```python
import uiautomator2 as u2

d = u2.connect('10.0.0.1') # 也可以用 u2.connect_wifi('10.0.0.1')
print(d.info)
```

1. **通过USB线连接**
   假设设备的序列号是 `123456f` (可以通过 `adb devices` 查看序列号)

```python
import uiautomator2 as u2

d = u2.connect('123456f') # 也可以用 u2.connect_usb('123456f')
print(d.info)
```

如果不带参数调用 `u2.connect()` ， `uiautomator2` 会首先从环境变量 `ANDROID_DEVICE_IP`中获取设备IP地址。

如果上述环境变量为空，`uiautomator2`会接着使用 `connect_usb` 方法，此时您要确保只有一台设备与您的PC机连接。

## 命令行工具

> 以下命令中的的`$device_ip`代表设备的ip地址

- `init`: 为设备安装所需要的程序

- `install`: 安装apk，apk通过URL给出

  ```bash
  $ python -m uiautomator2.cli install $device_ip https://example.org/some.apk
  MainThread: 15:37:55,731 downloading 80.4 kB / 770.6 kB
  MainThread: 15:37:56,763 installing 770.6 kB / 770.6 kB
  MainThread: 15:37:58,780 success installed 770.6 kB / 770.6 kB
  ```

- `clear-cache`: 清空缓存

  ```bash
  $ python -m uiautomator2 clear-cache
  ```

- `app-stop-all`: 停止所有应用

  ```bash
  $ python -m uiautomator2 app-stop-all $device_ip
  ```

- `screenshot`: 截图

  ```bash
  $ python -m uiautomator2 screenshot $device_ip screenshot.jpg
  ```

- `healthcheck`: 健康检查

  ```bash
  $ python -m uiautomator2 healthcheck $device_ip
  ```

## API 参考

### 全局设定

本节介绍了部分全局设定。

#### 调试HTTP请求

跟踪HTTP请求和相应以理清运行过程。

```python
>>> d.debug = True
>>> d.info
12:32:47.182 $ curl -X POST -d '{"jsonrpc": "2.0", "id": "b80d3a488580be1f3e9cb3e926175310", "method": "deviceInfo", "params": {}}' 'http://127.0.0.1:54179/jsonrpc/0'
12:32:47.225 Response >>>
{"jsonrpc":"2.0","id":"b80d3a488580be1f3e9cb3e926175310","result":{"currentPackageName":"com.android.mms","displayHeight":1920,"displayRotation":0,"displaySizeDpX":360,"displaySizeDpY":640,"displayWidth":1080,"productName"
:"odin","screenOn":true,"sdkInt":25,"naturalOrientation":true}}
<<< END
```

#### 默认等待

设置（查找和选择控件时的）默认的等待时间，以秒为单位。

```python
d.implicitly_wait(10.0)
d(text="Settings").click() # 如果10秒后 Settings 按钮依然没有显示出来，则报UiObjectNotFoundError错误

print("wait timeout", d.implicitly_wait()) # 获取默认等待时间
```

该方法设置的默认时间对下列操作有用： `click`, `long_click`, `drag_to`, `get_text`, `set_text`, `clear_text`, etc.

### App 管理

本节介绍如何管理APP。

#### 安装一个App

目前仅支持通过网络获取APK包安装。

```python
d.app_install('http://some-domain.com/some.apk')
```

#### 启动一个App

```python
d.app_start("com.example.hello_world") # 通过包名启动
```

#### 停掉一个App

```python
# 相当于 `am force-stop`, 因此您会丢失数据
d.app_stop("com.example.hello_world") 
# 相当于 `pm clear`
d.app_clear('com.example.hello_world')
```

#### 停掉所有App

```python
# 停掉所有App
d.app_stop_all()
# 停掉所有App，除了 com.examples.demo
d.app_stop_all(excludes=['com.examples.demo'])
```

#### 获取app信息

```python
d.app_info("com.examples.demo")
# 预期输出
#{
#    "mainActivity": "com.github.uiautomator.MainActivity",
#    "label": "ATX",
#    "versionName": "1.1.7",
#    "versionCode": 1001007,
#    "size":1760809
#}

# 保存app图标
img = d.app_icon("com.examples.demo")
img.save("icon.png")
```

#### 文件传输

- 发送文件到设备去

  ```python
  # 发送文件到一个目录
  d.push("foo.txt", "/sdcard/")
  # 发送并重命名
  d.push("foo.txt", "/sdcard/bar.txt")
  # 发送一个文件对象
  with open("foo.txt", 'rb') as f:
      d.push(f, "/sdcard/")
  # 发送并且更改文件的读写权限
  d.push("foo.sh", "/data/local/tmp/", mode=0o755)
  ```

- 从设备获取文件

  ```python
  d.pull("/sdcard/tmp.txt", "tmp.txt")
  
  # 如果在设备里找不到文件，则会报FileNotFoundError错误
  d.pull("/sdcard/some-file-not-exists.txt", "tmp.txt")
  ```

#### 检查并维持设备端守护进程处于运行状态

```python
d.healthcheck()
```

#### ~~Auto click permission dialogs~~

> **注意** `disable_popups`函数，检测发现很不稳定，暂时不要使用，等候通知。

在 0.1.1 版引入

```python
d.disable_popups() # 自动跳过弹窗
d.disable_popups(False) # 不自动跳过弹窗
```

![popup](D:/Typora/docs/img/popup.png)

如果这个方法在你的设备中不起作用，你可以发起一个pull request或者创建一个issue来改进这个方法。请按照下面步骤提供信息给我们：

1. 打开`uiautomatorviewer.bat`
2. 获取popup的层次结构

![hierarchy](D:/Typora/docs/img/uiautomatorviewer-popup.png)

现在您已经知道按钮的text和包的名字。请发起一个pull request或者创建一个issue来帮忙改进这个方法。

### 基本的 API 用法

本节会介绍如何对设备做一些基本的操作。

#### Shell 命令

- 运行一个短期命令 (默认 60s 超时)

  > **注意：**超时功能需要 `atx-agent >=0.3.3` ；另外`adb_shell`方法已被抛弃，请使用 `shell`方法 。

  简单用法：

  ```python
  output, exit_code = d.shell("pwd", timeout=60) # timeout 60s (Default)
  # output: "/\n", exit_code: 0
  # 相当于使用命令行输入: adb shell pwd
  
  # 因为 `shell` 的返回值是一个`namedtuple("ShellResponse", ("output", "exit_code"))`
  # 所以我们可以：
  output = d.shell("pwd").output
  exit_code = d.shell("pwd").exit_code
  ```

  第一个参数可以是一个列表，比如：

  ```python
  output, exit_code = d.shell(["ls", "-l"])
  # output: "/....", exit_code: 0
  ```

  This returns a string for stdout merged with stderr.

  如果这个命令是一个阻塞式命令，那么`shell`方法也会阻塞，直到命令执行结束或者超时。命令执行期间不会有任何哪怕是部分的输出。这个额API不适合长期的命令。该方法的命令在一个类似`abd shell`的环境中执行，因此命令的执行权限与 `adb`相同（比App的权限要高）。

- 运行一个长期命令

  添加`stream=True` 会返回一个 `requests.models.Response` 对象. 详情请参考 [requests stream](http://docs.python-requests.org/zh_CN/latest/user/quickstart.html#id5)

  ```python
  r = d.shell("logcat", stream=True)
  # r: requests.models.Response
  deadline = time.time() + 10 # run maxium 10s
  try:
      for line in r.iter_lines(): # r.iter_lines(chunk_size=512, decode_unicode=None, delimiter=None)
          if time.time() > deadline:
              break
          print("Read:", line.decode('utf-8'))
  finally:
      r.close() # 必须调用该方法
  ```

  在 `r.close()` 被调用时命令停止执行。

#### 会话

一个**会话**代表一个App的使用周期，可以用来启动App，以及检测App的崩溃。

- 启动与关闭App。

  ```python
  sess = d.session("com.netease.cloudmusic") # 启动网易云音乐
  sess.close() # 停止网易云音乐
  ```

- 使用python `with` 启动与关闭App。

  ```python
  with d.session("com.netease.cloudmusic") as sess:
      sess(text="Play").click()
  ```

- 与正在运行的App绑定。

  ```python
  sess = d.session("com.netease.cloudmusic", attach=True)
  ```

- 检测App崩溃。

  ```python
  # 当App正常运行时
  sess(text="Music").click() # 操作执行正常
  
  # 当App崩溃时
  sess(text="Music").click() # 报错 SessionBrokenError
  # 其他在会话中执行的操作也会报错 raise SessionBrokenError too
  
  ```

  ```python
  # 检查会话是否在进行中
  # 注意: 该方法名以后或许会变
  sess.running() # True or False
  ```

#### 获取设备信息

获取基本信息

```python
d.info
```

可能的输出：

```
{ 
    u'displayRotation': 0,
    u'displaySizeDpY': 640,
    u'displaySizeDpX': 360,
    u'currentPackageName': u'com.android.launcher',
    u'productName': u'takju',
    u'displayWidth': 720,
    u'sdkInt': 18,
    u'displayHeight': 1184,
    u'naturalOrientation': True
}

```

获取屏幕大小

```python
print(d.window_size())
# 竖放设备时的可能输出: (1080, 1920)
# 横放设备时的可能输出: (1920, 1080)
```

获取当前App的信息。对于一些安卓设备来说，输出有可能为空（见例3）

```python
print(d.current_app())
# Output example 1: {'activity': '.Client', 'package': 'com.netease.example', 'pid': 23710}
# Output example 2: {'activity': '.Client', 'package': 'com.netease.example'}
# Output example 3: {'activity': None, 'package': None}
```

等待Activity

```python
d.wait_activity(".ApiDemos", timeout=10) # default timeout 10.0 seconds
# Output: true of false
```

获取设备序列号

```python
print(d.serial)
# output example: 74aAEDR428Z9
```

获取网无线网络IP地址

```python
print(d.wlan_ip)
# output example: 10.0.0.1
```

获取详细的设备信息

```python
print(d.device_info)
```

下面是可能的输出结果：

```
{'udid': '3578298f-b4:0b:44:e6:1f:90-OD103',
 'version': '7.1.1',
 'serial': '3578298f',
 'brand': 'SMARTISAN',
 'model': 'OD103',
 'hwaddr': 'b4:0b:44:e6:1f:90',
 'port': 7912,
 'sdk': 25,
 'agentVersion': 'dev',
 'display': {'width': 1080, 'height': 1920},
 'battery': {'acPowered': False,
  'usbPowered': False,
  'wirelessPowered': False,
  'status': 3,
  'health': 0,
  'present': True,
  'level': 99,
  'scale': 100,
  'voltage': 4316,
  'temperature': 272,
  'technology': 'Li-ion'},
 'memory': {'total': 3690280, 'around': '4 GB'},
 'cpu': {'cores': 8, 'hardware': 'Qualcomm Technologies, Inc MSM8953Pro'},
 'presenceChangedAt': '0001-01-01T00:00:00Z',
 'usingBeganAt': '0001-01-01T00:00:00Z'}

```

### 关键事件（Key Events）

- 开/关屏幕

  ```python
  d.screen_on() # 开屏幕
  d.screen_off() # 关屏幕
  ```

- 获取当前屏幕状态

  ```python
  d.info.get('screenOn') # 要求 Android 版本大于 4.4
  ```

- 按键

  ```python
  d.press("home") # 按下 home 键
  d.press("back") # 按下 返回键
  d.press(0x07, 0x02) # 按下代号为 0x07 0x02 的键
  ```

- 目前支持下列按键名字：

  - home
  - back
  - left
  - right
  - up
  - down
  - center
  - menu
  - search
  - enter
  - delete ( or del)
  - recent (recent apps)
  - volume_up
  - volume_down
  - volume_mute
  - camera
  - power

您可以在这里找到所有按键的代号： [Android KeyEvnet](https://developer.android.com/reference/android/view/KeyEvent.html)

- 开屏幕锁

  ```python
  d.unlock()
  # 这相当于
  # 1. 启动 activity: com.github.uiautomator.ACTION_IDENTIFY
  # 2. 按下 home 键
  ```

#### 与设备的手势交互

- 按下屏幕某个位置

  ```python
  d.click(x, y)
  ```

- 双击

  ```python
  d.double_click(x, y)
  d.double_click(x, y, 0.1) # default duration between two click is 0.1s
  ```

- 长按

  ```python
  d.long_click(x, y)
  d.long_click(x, y, 0.5) # long click 0.5s (default)
  ```

- 滑动

  ```python
  d.swipe(sx, sy, ex, ey)
  d.swipe(sx, sy, ex, ey, 0.5) # swipe for 0.5s(default)
  ```

- 拖拽

  ```python
  d.drag(sx, sy, ex, ey)
  d.drag(sx, sy, ex, ey, 0.5) # swipe for 0.5s(default)
  ```

- 从某个点滑动到某个点

  ```python
  # 从点(x0, y0) 滑到点 (x1, y1) 再滑到 (x2, y2)
  # 两点之间的运动时间为0.2秒
  d.swipe((x0, y0), (x1, y1), (x2, y2), 0.2)
  ```

  多用于九宫格解锁，提前获取到每个点的相对坐标（这里支持百分比），更详细的使用参考这个帖子 [使用u2实现九宫图案解锁](https://testerhome.com/topics/11034)

- 触碰并拖动 (Beta)

  这个接口属于比较底层的原始接口，感觉并不完善，不过凑合能用。注：这个地方并不支持百分比

  ```python
  d.touch.down(10, 10) # 模拟按下
  time.sleep(.01) # down 和 move 之间的延迟，自己控制
  d.touch.move(15, 15) # 模拟移动
  d.touch.up() # 模拟抬起
  ```

注意：点击、滑动和拖动操作支持百分比位置值，比如： 

```python
d.long_click(0.5, 0.5) # 长按屏幕中心点
```

#### 有关屏幕

- 获取 / 设置 屏幕方向

  可能的方向有：

  - `natural` or `n`
  - `left` or `l`
  - `right` or `r`
  - `upsidedown` or `u` (不能设置)

  ```python
  # 获取方向. 可能的输出有："natural" or "left" or "right" or "upsidedown"
  orientation = d.orientation
  
  # 注意: not pass testing in my TT-M1
  # 设置方向并冻结旋转.
  # 注意: 设置 "upsidedown" 要求 Android>=4.3.
  d.set_orientation('l') # or "left"
  d.set_orientation("l") # or "left"
  d.set_orientation("r") # or "right"
  d.set_orientation("n") # or "natural"
  ```

- 冻结/解冻 旋转

  ```python
  # 冻结旋转
  d.freeze_rotation()
  # 解冻旋转
  d.freeze_rotation(False)
  ```

- 截屏

  ```python
  # 截屏并保存到电脑上, 要求 Android>=4.2.
  d.screenshot("home.jpg")
  
  # 获取 PIL.Image 格式的图片. 当然，您需要先安装 Pillow
  image = d.screenshot() # 默认格式为 "pillow"
  image.save("home.jpg") # or home.png. 目前只支持png和jpg格式
  
  # 获取 opencv 格式的图片. 当然，您需要先安装 numpy 和 opencv 
  import cv2
  image = d.screenshot(format='opencv')
  cv2.imwrite('home.jpg', image)
  
  # 获取原生的 jpeg 数据
  imagebin = d.screenshot(format='raw')
  open("some.jpg", "wb").write(imagebin)
  ```

- 导出UI布局

  ```python
  # get the UI hierarchy dump content (unicoded).
  xml = d.dump_hierarchy()
  ```

- Open notification or quick settings

  ```python
  d.open_notification()
  d.open_quick_settings()
  ```

#### 选择器

选择器是一种方便的**获取当前屏幕视野内某个UI对象**的机制。

```python
# 选择 text 为'Clock'以及 className 为 'android.widget.TextView' 的UI对象
d(text='Clock', className='android.widget.TextView')
```

选择器支持一下参数。详情请参考[UiSelector Java doc](http://developer.android.com/tools/help/uiautomator/UiSelector.html) 

- `text`, `textContains`, `textMatches`, `textStartsWith`
- `className`, `classNameMatches`
- `description`, `descriptionContains`, `descriptionMatches`, `descriptionStartsWith`
- `checkable`, `checked`, `clickable`, `longClickable`
- `scrollable`, `enabled`,`focusable`, `focused`, `selected`
- `packageName`, `packageNameMatches`
- `resourceId`, `resourceIdMatches`
- `index`, `instance`

##### 子节点和兄弟节点

- 子节点

  ```python
  # get the children or grandchildren
  d(className="android.widget.ListView").child(text="Bluetooth")
  ```

- 兄弟节点

  ```python
  # get siblings
  d(text="Google").sibling(className="android.widget.ImageView")
  ```

- 根据 text 或 description  或 instance 获取子节点

  ```python
  # 获取某个子节点，其 className 为"android.widget.LinearLayout"，且该节点的子节点的 text为 "Bluetooth"
  d(className="android.widget.ListView", resourceId="android:id/list") \
   .child_by_text("Bluetooth", className="android.widget.LinearLayout")
  
  # 选择允许 scroll search 的子节点
  d(className="android.widget.ListView", resourceId="android:id/list") \
   .child_by_text(
      "Bluetooth",
      allow_scroll_search=True,
      className="android.widget.LinearLayout"
    )
  ```

  - `child_by_description` is to find children whose grandchildren have
    the specified description, other parameters being similar to `child_by_text`.
  - `child_by_instance` is to find children with has a child UI element anywhere
    within its sub hierarchy that is at the instance specified. It is performed
    on visible views **without scrolling**.

  详细信息请参考下面链接：

  - [UiScrollable](http://developer.android.com/tools/help/uiautomator/UiScrollable.html), `getChildByDescription`, `getChildByText`, `getChildByInstance`
  - [UiCollection](http://developer.android.com/tools/help/uiautomator/UiCollection.html), `getChildByDescription`, `getChildByText`, `getChildByInstance`

  上述方法支持链式调用，例如下面的层次布局：

  ```xml
  <node index="0" text="" resource-id="android:id/list" class="android.widget.ListView" ...>
    <node index="0" text="WIRELESS & NETWORKS" resource-id="" class="android.widget.TextView" .../>
    <node index="1" text="" resource-id="" class="android.widget.LinearLayout" ...>
      <node index="1" text="" resource-id="" class="android.widget.RelativeLayout" ...>
        <node index="0" text="Wi‑Fi" resource-id="android:id/title" class="android.widget.TextView" .../>
      </node>
      <node index="2" text="ON" resource-id="com.android.settings:id/switchWidget" class="android.widget.Switch" .../>
    </node>
    ...
  </node>
  ```

  ![settings](https://raw.github.com/xiaocong/uiautomator/master/docs/img/settings.png)

  若要切换“Wi-Fi”，我们需要先选中“Wi-Fi”的切换开关。然而，该UI的层次结构中存在着多于一个的切换开关，且拥有几乎一样的属性值。我们无法根据className去选择，所以我们需要下面的选择策略：

  ```python
  d(className="android.widget.ListView", resourceId="android:id/list") \
    .child_by_text("Wi‑Fi", className="android.widget.LinearLayout") \
    .child(className="android.widget.Switch") \
    .click()
  ```

- 相对位置

  我们也可以使用相对位置方法来获取组件：`left`, `right`, `top`, `bottom`

  - `d(A).left(B)`, 选择在A左边的B
  - `d(A).right(B)`, 选择在A右边的B
  - `d(A).up(B)`, 选择在A上面的B
  - `d(A).down(B)`, 选择在A下面的B

  因此对于上面的例子，我们可以使用一下方式来选择：

  ```python
  ## 选择 "Wi‑Fi" 右边的 "switch" 
  d(text="Wi‑Fi").right(className="android.widget.Switch").click()
  ```

- 多个实例

  有时屏幕可能包含具有相同属性的多个视图，例如 文字，然后你会必须使用选择器中的“instance”属性来选择一个符合需求的实例，如下所示：

  ```python
  d(text="Add new", instance=0)  # 选中第一个text属性为“Add new”组件
  ```

  此外，uiautomator2提供一个类似列表的API（类似JQuery）：

  ```python
  # get the count of views with text "Add new" on current screen
  d(text="Add new").count
  
  # same as count property
  len(d(text="Add new"))
  
  # get the instance via index
  d(text="Add new")[0]
  d(text="Add new")[1]
  ...
  
  # iterator
  for view in d(text="Add new"):
      view.info  # ...
  ```

  **Notes**: when using selectors in a code block that walk through the result list, you must ensure that the UI elements on the screen
  keep unchanged. Otherwise, when Element-Not-Found error could occur when iterating through the list.

#### Get the selected ui object status and its information

- Check if the specific UI object exists

  ```python
  d(text="Settings").exists # True if exists, else False
  d.exists(text="Settings") # alias of above property.
  
  # advanced usage
  d(text="Settings").exists(timeout=3) # wait Settings appear in 3s, same as .wait(3)
  ```

- Retrieve the info of the specific UI object

  ```python
  d(text="Settings").info
  ```

  Below is a possible output:

  ```
  { u'contentDescription': u'',
  u'checked': False,
  u'scrollable': False,
  u'text': u'Settings',
  u'packageName': u'com.android.launcher',
  u'selected': False,
  u'enabled': True,
  u'bounds': {u'top': 385,
              u'right': 360,
              u'bottom': 585,
              u'left': 200},
  u'className': u'android.widget.TextView',
  u'focused': False,
  u'focusable': True,
  u'clickable': True,
  u'chileCount': 0,
  u'longClickable': True,
  u'visibleBounds': {u'top': 385,
                      u'right': 360,
                      u'bottom': 585,
                      u'left': 200},
  u'checkable': False
  }
  
  ```

- Get/Set/Clear text of an editable field (e.g., EditText widgets)

  ```python
  d(text="Settings").get_text()  # get widget text
  d(text="Settings").set_text("My text...")  # set the text
  d(text="Settings").clear_text()  # clear the text
  ```

- Get Widget center point

  ```python
  x, y = d(text="Settings").center()
  # x, y = d(text="Settings").center(offset=(0, 0)) # left-top x, y
  ```

#### Perform the click action on the selected UI object

- Perform click on the specific   object

  ```python
  # click on the center of the specific ui object
  d(text="Settings").click()
  
  # wait element to appear for at most 10 seconds and then click
  d(text="Settings").click(timeout=10)
  
  # click with offset(x_offset, y_offset)
  # click_x = x_offset * width + x_left_top
  # click_y = y_offset * height + y_left_top
  d(text="Settings").click(offset=(0.5, 0.5)) # Default center
  d(text="Settings").click(offset=(0, 0)) # click left-top
  d(text="Settings").click(offset=(1, 1)) # click right-bottom
  
  # click when exists in 10s, default timeout 0s
  clicked = d(text='Skip').click_exists(timeout=10.0)
  
  # click until element gone, return bool
  is_gone = d(text="Skip").click_gone(maxretry=10, interval=1.0) # maxretry default 10, interval default 1.0
  ```

- Perform long click on the specific UI object

  ```python
  # long click on the center of the specific UI object
  d(text="Settings").long_click()
  ```

#### Gesture actions for the specific UI object

- Drag the UI object towards another point or another UI object 

  ```python
  # notes : drag can not be used for Android<4.3.
  # drag the UI object to a screen point (x, y), in 0.5 second
  d(text="Settings").drag_to(x, y, duration=0.5)
  # drag the UI object to (the center position of) another UI object, in 0.25 second
  d(text="Settings").drag_to(text="Clock", duration=0.25)
  ```

- Swipe from the center of the UI object to its edge

  Swipe supports 4 directions:

  - left
  - right
  - top
  - bottom

  ```python
  d(text="Settings").swipe("right")
  d(text="Settings").swipe("left", steps=10)
  d(text="Settings").swipe("up", steps=20) # 1 steps is about 5ms, so 20 steps is about 0.1s
  d(text="Settings").swipe("down", steps=20)
  ```

- Two-point gesture from one point to another

  ```python
  d(text="Settings").gesture((sx1, sy1), (sx2, sy2), (ex1, ey1), (ex2, ey2))
  ```

- Two-point gesture on the specific UI object

  Supports two gestures:

  - `In`, from edge to center
  - `Out`, from center to edge

  ```python
  # notes : pinch can not be set until Android 4.3.
  # from edge to center. here is "In" not "in"
  d(text="Settings").pinch_in(percent=100, steps=10)
  # from center to edge
  d(text="Settings").pinch_out()
  ```

- Wait until the specific UI appears or disappears

  ```python
  # wait until the ui object appears
  d(text="Settings").wait(timeout=3.0) # return bool
  # wait until the ui object gone
  d(text="Settings").wait_gone(timeout=1.0)
  ```

  The default timeout is 20s. see **global settings** for more details

- Perform fling on the specific ui object(scrollable)

  Possible properties:

  - `horiz` or `vert`
  - `forward` or `backward` or `toBeginning` or `toEnd`

  ```python
  # fling forward(default) vertically(default) 
  d(scrollable=True).fling()
  # fling forward horizontally
  d(scrollable=True).fling.horiz.forward()
  # fling backward vertically
  d(scrollable=True).fling.vert.backward()
  # fling to beginning horizontally
  d(scrollable=True).fling.horiz.toBeginning(max_swipes=1000)
  # fling to end vertically
  d(scrollable=True).fling.toEnd()
  ```

- Perform scroll on the specific ui object(scrollable)

  Possible properties:

  - `horiz` or `vert`
  - `forward` or `backward` or `toBeginning` or `toEnd`, or `to`

  ```python
  # scroll forward(default) vertically(default)
  d(scrollable=True).scroll(steps=10)
  # scroll forward horizontally
  d(scrollable=True).scroll.horiz.forward(steps=100)
  # scroll backward vertically
  d(scrollable=True).scroll.vert.backward()
  # scroll to beginning horizontally
  d(scrollable=True).scroll.horiz.toBeginning(steps=100, max_swipes=1000)
  # scroll to end vertically
  d(scrollable=True).scroll.toEnd()
  # scroll forward vertically until specific ui object appears
  d(scrollable=True).scroll.to(text="Security")
  ```

#### Watcher

你可以注册一个 [watchers](http://developer.android.com/tools/help/uiautomator/UiWatcher.html) 

- 注册Watcher

  当选择器没有找到匹配的元素时，uiautomator2会执行所有注册的watcher

  - 当条件匹配时，点击目标对象

  ```python
  d.watcher("AUTO_FC_WHEN_ANR").when(text="ANR").when(text="Wait") \
                               .click(text="Force Close")
  # d.watcher(name) ## 创建一个watcher，名为name.
  #  .when(condition)  ## 选择器的匹配条件，也是watcher监听的条件.
  #  .click(target)  ## 点击目标对象.
  ```

  你可以使用不带参数调用`click`

  ```python
  d.watcher("ALERT").when(text="OK").click()
  # 相当于 
  d.watcher("ALERT").when(text="OK").click(text="OK")
  ```

  - 当条件符合时按下某个键

  ```python
  d.watcher("AUTO_FC_WHEN_ANR").when(text="ANR").when(text="Wait") \
                               .press("back", "home")
  # d.watcher(name) ## 创建一个watcher，名为name
  #  .when(condition)  ## 选择器的匹配条件，也是watcher监听的条件.
  #  .press(<keyname>, ..., <keyname>.()  ## 按顺序一个个点击
  ```

- 检查某个watcher是否被触发

  一个watcher被触发，意味着所有条件都符合，且watcher已经运行了

  ```python
  d.watcher("watcher_name").triggered
  # true in case of the specified watcher triggered, else false
  ```

- 根据名字清除watcher

  ```python
  # remove the watcher
  d.watcher("watcher_name").remove()
  ```

- l写出所有 watchers

  ```python
  d.watchers
  # a list of all registered watchers
  ```

- 检查有没有watcher被触发了

  ```python
  d.watchers.triggered
  # 有任何watcher被触发都会返回 true 
  ```

- 重置所有watchers

  ```python
  # 重置所有watchers，这以后 d.watchers.triggered 的值变为 false.
  d.watchers.reset()
  ```

- 清除watchers

  ```python
  # 清除所有watchers
  d.watchers.remove()
  # 根据名字清除watcher, 相当于 d.watcher("watcher_name").remove()
  d.watchers.remove("watcher_name")
  ```

- 强制运行鄋watcher

  ```python
  # force to run all registered watchers
  d.watchers.run()
  ```

- 当页面更新后运行所有watcher。通常可以用来自动点击权限确认框，或者自动安装

  ```python
  d.watcher("OK").when(text="OK").click(text="OK")
  # enable auto trigger watchers
  d.watchers.watched = True
  
  # disable auto trigger watchers
  d.watchers.watched = False
  
  # get current trigger watchers status
  assert d.watchers.watched == False
  ```

另外文档还是有很多没有写，推荐直接去看源码[__init__.py](uiautomator2/__init__.py)

#### 全局设置

```python
# set delay 1.5s after each UI click and click
d.click_post_delay = 1.5 # default no delay

# set default element wait timeout (seconds)
d.wait_timeout = 30.0 # default 20.0
```

UiAutomator中的超时设置(隐藏方法)

```python
>> d.jsonrpc.getConfigurator() 
{'actionAcknowledgmentTimeout': 500,
 'keyInjectionDelay': 0,
 'scrollAcknowledgmentTimeout': 200,
 'waitForIdleTimeout': 0,
 'waitForSelectorTimeout': 0}

>> d.jsonrpc.setConfigurator({"waitForIdleTimeout": 100})
{'actionAcknowledgmentTimeout': 500,
 'keyInjectionDelay': 0,
 'scrollAcknowledgmentTimeout': 200,
 'waitForIdleTimeout': 100,
 'waitForSelectorTimeout': 0}
```

为了防止客户端程序响应超时，`waitForIdleTimeout`和`waitForSelectorTimeout`目前已改为`0`

参考：[Google uiautomator Configurator](https://developer.android.com/reference/android/support/test/uiautomator/Configurator)

#### Input 方法

这种方法通常用于不知道控件的情况下的输入。第一步需要切换输入法，然后发送adb广播命令，具体使用方法如下

```python
d.set_fastinput_ime(True) # 切换成FastInputIME输入法
d.send_keys("你好123abcEFG") # adb广播输入
d.clear_text() # 清除输入框所有内容(Require android-uiautomator.apk version >= 1.0.7)
d.set_fastinput_ime(False) # 切换成正常的输入法
d.send_action("search") # 模拟输入法的搜索
```

**send_action** 说明

该函数可以使用的参数有 `go search send next done previous`

_什么时候该使用这个函数呢？_

有些时候在EditText中输入完内容之后，调用`press("search")` or `press("enter")`发现并没有什么反应。
这个时候就需要`send_action`函数了，这里用到了只有输入法才能用的[IME_ACTION_CODE](https://developer.android.com/reference/android/view/inputmethod/EditorInfo)。
`send_action`先broadcast命令发送给输入法操作`IME_ACTION_CODE`，由输入法完成后续跟EditText的通信。（原理我不太清楚，有了解的，提issue告诉我)

#### Toast

显示信息框

```python
d.toast.show("Hello world")
d.toast.show("Hello world", 1.0) # show for 1.0s, default 1.0s
```

获取信息框

```python
# [Args]
# 5.0: max wait timeout. Default 10.0
# 10.0: cache time. return cache toast if already toast already show up in recent 10 seconds. Default 10.0 (Maybe change in the furture)
# "default message": return if no toast finally get. Default None
d.toast.get_message(5.0, 10.0, "default message")

# common usage
assert "Short message" in d.toast.get_message(5.0, default="")

# clear cached toast
d.toast.reset()
# Now d.toast.get_message(0) is None
```

### XPath

比如其中一个节点的内容

```xml
<android.widget.TextView
  index="2"
  text="05:19"
  resource-id="com.netease.cloudmusic:id/qf"
  package="com.netease.cloudmusic"
  content-desc=""
  checkable="false" checked="false" clickable="false" enabled="true" focusable="false" focused="false"
  scrollable="false" long-clickable="false" password="false" selected="false" visible-to-user="true"
  bounds="[957,1602][1020,1636]" />
```

xpath定位和使用方法

有些属性的名字有修改需要注意

```
description -> content-desc
resourceId -> resource-id

```

常见用法

```python
# wait exists 10s
d.xpath("//android.widget.TextView").wait(10.0)
# find and click
d.xpath("//*[@content-desc='分享']").click()
# check exists
if d.xpath("//android.widget.TextView[contains(@text, 'Se')]").exists:
    print("exists")
# get all text-view text, attrib and center point
for elem in d.xpath("//android.widget.TextView").all():
    print("Text:", elem.text)
    # Dictionary eg: 
    # {'index': '1', 'text': '999+', 'resource-id': 'com.netease.cloudmusic:id/qb', 'package': 'com.netease.cloudmusic', 'content-desc': '', 'checkable': 'false', 'checked': 'false', 'clickable': 'false', 'enabled': 'true', 'focusable': 'false', 'focused': 'false','scrollable': 'false', 'long-clickable': 'false', 'password': 'false', 'selected': 'false', 'visible-to-user': 'true', 'bounds': '[661,1444][718,1478]'}
    print("Attrib:", elem.attrib)
    # Coordinate eg: (100, 200)
    print("Position:", elem.center())
```

其他XPath常见用法

```
# 所有元素
//*

# resource-id包含login字符
//*[contains(@resource-id, 'login')]

# 按钮包含账号或帐号
//android.widget.Button[contains(@text, '账号') or contains(@text, '帐号')]

# 所有ImageView中的第二个
(//android.widget.ImageView)[2]

# 所有ImageView中的最后一个
(//android.widget.ImageView)[last()]

# className包含ImageView
//*[contains(name(), "ImageView")]

```

# 项目历史

- 项目重构自 <https://github.com/openatx/atx-uiautomator>

## Google uiautomator与uiautomator2的区别

1. API相似但是不完全兼容
2. uiautomator2是安卓项目，而uiautomator是Java项目
3. uiautomator2可以输入中文，而uiautomator的Java工程需借助utf7输入法才能输入中文
4. uiautomator2必须明确EditText框才能向里面输入文字，uiautomator直接指定父类也可以在子类中输入文字
5. uiautomator2获取控件速度比uiautomator快

## [CHANGELOG (generated by pbr)](CHANGELOG)

从git历史中查看变更记录

```
git log --graph --date-order -C -M --pretty=format:"<%h> %ad [%an] %Cgreen%d%Creset %s" --all --date=short

```

## 依赖项目

- uiautomator守护程序 <https://github.com/openatx/atx-agent>
- uiautomator jsonrpc server<https://github.com/openatx/android-uiautomator-server/>

# Contributors

- codeskyblue ([@codeskyblue][])
- Xiaocong He ([@xiaocong][])
- Yuanyuan Zou ([@yuanyuan][])
- Qian Jin ([@QianJin2013][])
- Xu Jingjie ([@xiscoxu][])
- Xia Mingyuan ([@mingyuan-xia][])
- Artem Iglikov, Google Inc. ([@artikz][])

[@codeskyblue]: https://github.com/codeskyblue
[@xiaocong]: https://github.com/xiaocong
[@yuanyuan]: https://github.com/yuanyuanzou
[@QianJin2013]: https://github.com/QianJin2013
[@xiscoxu]: https://github.com/xiscoxu
[@mingyuan-xia]: https://github.com/mingyuan-xia
[@artikz]: https://github.com/artikz

Other [contributors](../../graphs/contributors)

# LICENSE

[MIT](
