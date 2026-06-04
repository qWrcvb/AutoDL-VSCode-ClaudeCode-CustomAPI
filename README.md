# 基于 AutoDL + VSCode + Claude Code（自定义 API）搭建远程 AI 辅助开发环境

> **摘要**：本文介绍如何在 AutoDL 云服务器上部署开发环境，通过 VSCode 远程连接，并配置 Claude Code 使用自定义 API（如第三方中转或自建服务），实现"云端算力 + 本地 IDE 体验 + AI 辅助编程"三位一体的开发工作流。

## 一、方案概述与选型理由

### 1.1 为什么选这套组合？

| 组件            | 作用              | 选型理由                                        |
| --------------- | ----------------- | ----------------------------------------------- |
| **AutoDL**      | 云端 GPU/CPU 算力 | 按小时计费、镜像丰富、适合深度学习/大模型开发   |
| **VSCode**      | 本地 IDE          | 远程开发体验好、插件生态丰富、SSH 连接稳定      |
| **Claude Code** | AI 编程助手       | 代码理解能力强、支持终端交互、可自定义 API 端点 |

**核心痛点解决**：

- 本地机器配置不够 → AutoDL 提供弹性算力
- 远程服务器操作不便 → VSCode Remote-SSH 实现本地级体验
- 想用 Claude 但官方访问受限 → 自定义 API 端点绕过限制
  - 

## 二、环境准备

### 2.1 AutoDL 服务器配置

1. 登录 [AutoDL 官网](https://www.autodl.com)，根据自己的需求创建实例
2. 开机后记录以下信息位置，后续直接复制：
   1. 登录指令
   2. 登录密码

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YzE4M2I5YjI4N2E3NDAxMDEyZjFlZmFjZGYyMGY3NmZfb2lqeDhWZUp1eDZCbm5MczA5aGdqbEFTc1lSd3VwQ0tfVG9rZW46TmUxdmIyQXlMb3dscXB4Q09seWN1c3Npbm1jXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

### 2.2 本地环境

安装最新版**VSCode** + **Remote - SSH** 插件+**Claude Code for VS Code插件**

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODhlODAwNWQ5OWE3N2QxNWU4YTA2MDQzMDBhNGJhN2RfUFpsTWVROWRmQnEwYURjUndyeDBDTm4yRHFwSGRJaG9fVG9rZW46Q1czNGJ3bFI3b0Zpejh4dVh5a2M1eFM2blNjXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NGYxODFmNTg4NDUzZGU5Y2Q5NjhlNGY0Y2Y1NmE4OWZfY0tJQmQ4VDJiQVFuTElRTzFpRXZFQjJGRzg5MGJSR1dfVG9rZW46WlBRUWJVTjRqb0pCVjN4YjJnUGNUaXJGbkVmXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

## 三、开始配置

### 3.1 配置 SSH 连接

点击左下角打开远程窗口

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ODlmNDNjMjQwMDE2OWU0MTVkMzI2ODBkODZmZWJlYTZfdnAxM1NIVkVwWGdqUjVuNGxLdzVBalluUjZlTENqb21fVG9rZW46TjdoRWJkY3BTb0tGSzd4YUd4cmMwaHRibnNnXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZjkxODcyYTAzNGU4ZmVmNTZiNmE2NWM1Y2MyY2Y1ODlfWWUwSnVFanRTRkJrcnphUG9hWHJJS3YwOExvNVZkY1FfVG9rZW46Rkc1c2JHaUdIb3ZWQzl4enlOcWM0WlBqbmFmXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YmYxODQ0ZDY0ZWNiMDA4MTg5NmM1YWRiYWIzY2U5MjhfYVZEeUtPQzRqeGhPSnlsenhxeE81MVpCWDJ4RGlKdEhfVG9rZW46T0drZmJEWDZhbzVtSFN4R1J3MWNhVHdQbmtmXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

复制AutoDL上的登录指令填入：

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MjM5YTcyMjc2ZDYxODAxOTk3ZGZmOWJiNzM0MTYzOWVfRzc5SW9vc2Q2M3Y4VGQybTFzM3pZSFNqTHAyUk5MemdfVG9rZW46RWU4cWJMY1N6b1RsQTV4aUZxWGNqVWpFbmNvXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

选择配置保存路径（一般是含用户名的路径）后，点击右下角弹窗的连接按钮：

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NGMzOWY2MDRjNWNmYmU3YWI4YTc1OTBlMWZkYjY2NzdfQzc0emZGeWNHeVI2c0g0M3A5VURsbnJyZDU3bmd3azlfVG9rZW46SDMwZmIzT0lhb2doMUd4TWtYRWNxMGdsbk1jXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

复制AutoDL上的密码填入：

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=MzcyZmNmMzQxNzFiNjI0ZTNjYWQ1MWE5YTJmMjYwMDJfZnlIdmdJa1RoR2ZHMDVhWE1Hdk50em9FWE9pV2RvaVBfVG9rZW46T3VBMWJTVVRXb1dHeXp4WVNPNWNFeFJvblBjXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

### 3.2 配置Claude code插件

Claude code插件可能不会出现在远程连接界面，是因为插件被禁用，此时打开扩展商店找到Claude Code for VS Code插件页面启用即可，启用方法是点击“自动更新”左侧的蓝色按钮，以下是启用成功的插件界面。

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=NTEzNWExZTk4ZDgzZDkzZTFhZWQyNWU0YTMzMzRkNGJfd2pHVmVBWXJOdW1aSFp1U0RrQk0wek5DUGNGcGo2MnJfVG9rZW46QXhKNmI3VWQzb0JEMFZ4NWdld2NnZ0FkbkNnXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

然后就可以在远程连接界面打开Claude code。

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YmRhMWQxYjU2MTA4NjAzMjg1NjZkN2MwYjA4ZmMxNTNfQWFwZzVRc28zMjByV05IZW13NmZKVlRMbUdmN0dZRGVfVG9rZW46Rnh5R2JQY1Bkb3FpMEx4M3VIVWNjaG9vbm1lXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

接下来是修改api，在Claude code对话框输入`/config`,点击`General config`跳转到设置界面。

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZmNjZGVjOWU0YzYxMWVhNWM0OWIxOTIyOGYwMTc3YjFfM1JURVo2N3BlVUwyNnA4bHpoVnZMRTQ1WnFLMm1LVGFfVG9rZW46T2tmVGIwM2Vab3lhcHV4MTRBcmNKNXJQbnVoXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

下滑找到“在 settings.json 中编辑”并点击

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=ZGE1YTgyOTAyMTRjZGZlZDM4NzIyNjgxNjYxOTU4OTlfOVFtYTJYTDFwVmhPYUpURjQ2aUMyVlE3bFhCZEdJaDRfVG9rZW46WVpRcGJHMTNGb1hpbkx4TmJkSmNiOUQxbnRTXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

找到以下字段，并在相应位置更改密钥与url:

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=OWM3MDcxNDkwOWVlZjdkMTFiOGRhOWZhZDM5ZTQwOGVfWEZHSmdTTzJQT1FhZWtTSU9qZWxvWXpTZWdUM0ZTOUZfVG9rZW46Q0ZvcGJPWUlFb3FRQU94bHd0SmNBcVNZbmpiXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)

然后保存更改即可

## 四、使用方法

在VSCode的Remote-SSH 模式下，无法直接拖放文件到Claude code对话框作为上下文，可以通过在对话框@对应的文件来指定。

![img](https://my.feishu.cn/space/api/box/stream/download/asynccode/?code=YTU5NzM2Yjk5NDk5MDVmYzliNzNhZDI2ZTM4NWRhMzRfUGYyN0d1SjJJZVZSWXVZQVZJalZpNEd0S3FDNUJTN0pfVG9rZW46Q094MWIyWm1Ib204WWl4aEF1d2NzcmtJbmtlXzE3ODA1NDM3MTE6MTc4MDU0NzMxMV9WNA&add_watermark=true&scene_type=CCM)
