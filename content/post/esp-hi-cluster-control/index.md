+++
author = "MOLO"
title = "让一群机器狗听指挥：ESP-Hi Web 集群控制台"
slug = "esp-hi-cluster-control"
date = "2026-10-25"
description = "基于 Flask + WebSocket 的 ESP-Hi 机器狗局域网集群控制台：多机发现、实时遥控、12 种预设动作编排、四腿舵机校准。"
tags = ["项目", "Flask", "WebSocket", "机器人"]
categories = ["项目"]
image = "https://opengraph.githubassets.com/1/tu-MOLO/Web_Cluster_Control_for_ESP_Hi"
+++

ESP-Hi 机器狗自带的上位机通常只管一台。上课或做演示时手上有好几台，就会立刻变成“切换窗口、一个个连、动作还不齐”的混乱现场。这个项目把多机控制收进一个网页。

仓库：<https://github.com/tu-MOLO/Web_Cluster_Control_for_ESP_Hi>

<!--more-->

## 它做了什么

**设备管理**：自动扫描局域网里的 ESP-Hi，集中成设备库，支持导出/导入 JSON、批量删除、重命名，实时显示在线/离线/连接状态；设备掉线会自动断开并清状态。

**运动控制**：虚拟摇杆（拖动控制方向、松手才下发，避免鼠标抖动刷指令）+ WASD 键盘长按连续下发，指令延迟可在 100–2000 ms 之间调整。

**预设动作**：内置 12 种动作——趴下、鞠躬、后仰、摇摆、前后、左右、握手、戳戳、抖腿、前跳、后跳、收腿，可勾选循环执行并设置 0.1–60 s 间隔，也能一键停止。

**舵机校准**：单设备独占校准，期间锁定其他功能防止冲突，四条腿（FL / FR / BL / BR）分别 ±1° 微调。

## 技术结构

后端是 Flask 3 + Flask-SocketIO（Eventlet 驱动），拆成四块：`app.py` 主应用、`device_manager.py` 设备管理器、`scanner_service.py` 扫描服务、`calibration_service.py` 校准服务。前端是原生 JavaScript + CSS，没有构建步骤。

选 WebSocket 而不是轮询，是因为摇杆指令和状态回传都要求低延迟；Socket.IO 同时兼顾了断线重连和房间式的多设备广播。

## 为什么用网页而不是桌面程序

机房和教室的电脑往往装不了东西、或者不让装。一个能本地启动的网页控制台，只要 Python 环境在就能用，学生用自己电脑连同一个局域网也能加入。

## 启动

```bash
cd backend
pip install flask flask-socketio flask-cors requests eventlet
python app.py
```

然后浏览器打开提示的端口即可。仓库里也带了 `build.bat` / `run.bat`，可以打包成 Windows 可执行文件分发。
