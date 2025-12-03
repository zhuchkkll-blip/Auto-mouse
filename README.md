AutoMouse - 零依赖 Windows 自动鼠标控制工具

![Python Version](https://img.shields.io/badge/python-3.6%2B-blue) ![Platform](https://img.shields.io/badge/platform-Windows-green)

项目简介

AutoMouse 是一个基于 Python 开发的 Windows 平台自动鼠标控制类，无需依赖任何第三方库，仅使用标准库即可实现丰富的鼠标自动化操作。其核心优势在于支持拟人化轨迹模拟，通过贝塞尔曲线与 Perlin 噪声结合，让鼠标移动更贴近人类操作习惯，适用于办公自动化、软件测试、重复性操作简化等场景。

核心特性

- 零依赖：仅依赖 Python 标准库（ctypes、time、math 等），下载即可使用

- 拟人化操作：基于贝塞尔曲线生成平滑轨迹，结合噪声添加微抖动，避免机械感

- 功能全面：支持平滑移动、单击、双击、拖拽、滚轮滚动、按键长按/松开等完整操作

- 灵活配置：可自定义移动时长、帧率、轨迹类型，适配不同场景需求

- 安全可靠：内置坐标边界保护，确保鼠标操作始终在屏幕范围内

- 实时控制：提供操作中断与重置接口，支持多线程场景下的动态控制

环境要求

- 操作系统：Windows 7 及以上（32/64 位均可）

- Python 版本：Python 3.6 及更高版本

- 依赖库：无第三方依赖，直接使用标准库

快速开始

1. 代码获取与保存

将 AutoMouse 核心代码保存为 automouse.py 文件，放入您的项目目录中。

2. 基础使用示例

以下示例演示了 AutoMouse 的核心操作，复制到您的脚本中即可运行：

from automouse import AutoMouse
import time

# 初始化自动鼠标实例
am = AutoMouse()

# 1. 移动鼠标到屏幕坐标 (500, 300)，0.8秒完成，启用拟人轨迹
print("正在移动鼠标到 (500, 300)...")
am.move_to(500, 300, duration=0.8, human=True)
time.sleep(1)

# 2. 在当前位置执行左键单击
print("执行左键单击...")
am.click()
time.sleep(1)

# 3. 移动到 (800, 400) 并执行双击
print("执行双击操作...")
am.double_click(800, 400)
time.sleep(1)

# 4. 从 (200, 200) 拖拽到 (600, 600)，1秒完成
print("执行拖拽操作...")
am.drag(200, 200, 600, 600, duration=1.0)
time.sleep(1)

# 5. 向下滚动滚轮 2 次
print("执行滚轮滚动...")
am.scroll(clicks=-2)
time.sleep(1)

# 6. 长按左键并移动
print("执行长按拖拽...")
am.move_to(300, 300)
am.left_down()  # 按住左键
am.move_to(700, 500, duration=1.0)  # 拖动过程
am.left_up()    # 松开左键
print("操作完成！")

核心 API 说明

AutoMouse 类的公开接口简洁易用，以下是常用方法的详细说明：

1. 鼠标移动

move_to(x: int, y: int, duration: float = 0.5, human: bool = True, fps: int = 60)

- 功能：平滑移动鼠标到目标坐标

- 参数：
        x/y：目标屏幕坐标（像素）

- duration：移动总时长（秒），默认 0.5 秒

- human：是否启用拟人轨迹，默认 True（贝塞尔曲线+微抖动）

- fps：移动帧率，默认 60 帧/秒

2. 点击操作

click(x: Optional[int] = None, y: Optional[int] = None, button: str = "left")

- 功能：单击指定鼠标按键

- 参数：
        x/y：可选，点击位置坐标（不指定则在当前位置点击）

- button：按键类型，可选 "left"（默认）、"right"、"middle"

double_click(x: Optional[int] = None, y: Optional[int] = None, button: str = "left")

功能与参数同 click()，用于执行双击操作。

3. 拖拽操作

drag(x1: int, y1: int, x2: int, y2: int, duration: float = 0.5, button: str = "left")

- 功能：从起点 (x1,y1) 拖拽到终点 (x2,y2)

- 参数：
        x1/y1：拖拽起点坐标

- x2/y2：拖拽终点坐标

- duration：移动过程时长，默认 0.5 秒

4. 滚轮操作

scroll(clicks: int = 1)

- 功能：控制鼠标滚轮滚动

- 参数：clicks 为滚动次数，正数向上，负数向下

5. 按键控制

- left_down()：按住鼠标左键不松开

- left_up()：松开鼠标左键

6. 操作中断

- stop()：中断当前正在执行的鼠标操作（如移动、拖拽）

- reset()：重置中断标志，恢复正常操作能力

注意事项

- 平台限制：仅支持 Windows 系统，依赖 Win32 API（user32.dll），不支持 macOS/Linux

- 权限问题：部分游戏、杀毒软件或管理员权限程序可能拦截模拟操作，建议以管理员身份运行脚本

- 坐标说明：屏幕坐标以左上角为原点 (0,0)，右下角为 (屏幕宽度-1, 屏幕高度-1)

- 线程安全：stop() 和 reset() 支持多线程调用，可用于实时中断长时间操作

- 拖拽优化：当前 drag() 方法松开按键为硬编码左键，如需支持右键/中键，可修改方法中 MOUSEEVENTF_LEFTUP 为对应常量

常见问题

Q1: 运行脚本后鼠标无反应？

A1: 请检查以下几点：1. 是否以管理员身份运行 Python 脚本；2. 目标程序是否开启了防注入/防模拟保护；3. 坐标是否在屏幕范围内。

Q2: 如何实现右键拖拽？

A2: 调用 drag() 方法时指定 button="right"，并修改方法内松开按键的代码为 MOUSEEVENTF_RIGHTUP。

Q3: 如何在多线程中使用？

A3: 可在主线程初始化 AutoMouse 实例，子线程中调用 stop() 中断操作，注意避免多线程同时操作同一实例。

扩展建议

1. 增加侧键支持：扩展 Win32 常量（如 MOUSEEVENTF_XDOWN），实现鼠标侧键的点击与长按

2. 分辨率适配：添加基于屏幕比例的坐标计算方法，支持不同分辨率下的统一操作

3. 操作录制/回放：通过记录鼠标事件与时间戳，实现操作流程的录制与重复执行

4. 速度曲线优化：增加非线性速度调整（如加速-匀速-减速），更贴近人类操作习惯

5. 异常处理：添加窗口激活、坐标无效等场景的异常捕获与提示

许可证

本项目采用 MIT 许可证开源，您可以自由使用、修改和分发，无论个人还是商业用途，均需保留原作者信息。

贡献指南

欢迎提交 Issue 反馈 Bug 或需求，也可通过 Pull Request 贡献代码。提交代码前请确保：1. 新增功能包含完整注释；2. 已通过基础功能测试；3. 代码风格与现有代码保持一致。

如有问题或建议，可通过以下方式联系：

Email: zhuchkkll@gmail.com | GitHub: github.com/zhuchkkll-blip/automouse
