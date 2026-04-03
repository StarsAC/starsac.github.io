+++
date = 2026-04-03T22:25:35+08:00
title = "uv笔记"
description = "uv笔记"
slug = "uv-note"
tags = ["Dev","Software"]
+++

> 其它信息参阅[官方文档](https://uv.doczh.com/)
## 安装

### macOS 和 Linux
```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```
或者使用Homebrew
```bash
brew install uv
```
### Windows
```powershell
powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
```
或者使用winget
```bash
winget install --id=astral-sh.uv  -e
```
## 升级
```bash
uv self update
```
## Python 版本管理
- `uv python install`: 安装 Python 版本
- `uv python list`: 查看可用 Python 版本
- `uv python find`: 查找已安装的 Python 版本
- `uv python pin`: 将当前项目固定使用特定 Python 版本
- `uv python uninstall`: 卸载 Python 版本
## 脚本运行
执行独立的 Python 脚本，例如 `example.py`。
- `uv run`: 运行脚本
- `uv add --script`: 为脚本添加依赖
- `uv remove --script`: 从脚本移除依赖
## 项目管理
创建和开发带有 `pyproject.toml` 的 Python 项目。
- `uv init`: 创建新 Python 项目
- `uv add`: 为项目添加依赖
- `uv remove`: 从项目移除依赖
- `uv sync`: 同步项目依赖到环境
- `uv lock`: 为项目依赖创建锁文件
- `uv run`: 在项目环境中运行命令
- `uv tree`: 查看项目依赖树
- `uv build`: 构建项目为分发包
- `uv publish`: 发布项目到包索引
## pip 接口
手动管理环境和包 —— 适用于遗留工作流或高级命令无法提供足够控制的情况。
- `uv venv`：创建新的虚拟环境。
- `uv pip install`：将包安装到当前环境
- `uv pip show`：显示已安装包的详细信息
- `uv pip freeze`：列出已安装包及其版本
- `uv pip check`：检查当前环境的包兼容性
- `uv pip list`：列出已安装包
- `uv pip uninstall`：卸载包
- `uv pip tree`：查看环境的依赖树
