# AutoGPT Platform 安装器

AutoGPT Platform 提供易于使用的安装程序，帮助您在系统上快速设置平台。本文档介绍如何在 Linux/macOS 和 Windows 系统上使用安装脚本。

## 安装器功能

安装脚本将执行以下操作：

1. 检查必备条件（Git、Docker、npm）
2. 克隆 AutoGPT 代码库
3. 使用 Docker 设置后端服务
4. 设置前端应用程序
5. 启动后端和前端服务

## 前置条件

运行安装程序前，请确保已安装以下组件：

- **Git**：用于克隆代码库
- **Docker**：用于运行后端服务
- **Node.js 和 npm**：用于前端应用程序

## 快速单行命令安装

为方便起见，您可以使用以下单行命令安装 AutoGPT Platform：

### Linux/macOS

```bash
curl -fsSL https://setup.agpt.co/install.sh -o install.sh && bash install.sh
```

### Windows 平台

```powershell
powershell -c "iwr https://setup.agpt.co/install.bat -o install.bat; ./install.bat"
```

## 手动安装

如果您愿意，也可以手动下载并运行安装脚本：

- **Linux/macOS:** `setup-autogpt.sh`
- **Windows:** `setup-autogpt.bat`

这些脚本位于 `autogpt_platform/installer/` 目录中。

## 安装完成后

安装完成后：

- 后端服务将在 Docker 容器中运行
- 前端应用程序可通过 http://localhost:3000 访问

## 停止服务

要停止服务，请在运行前端的终端中按 Ctrl+C，然后运行：

```bash
cd AutoGPT/autogpt_platform
docker compose down
```

## 故障排除

如果在安装过程中遇到任何问题：

1. 确保所有先决条件已正确安装
2. 检查 Docker 是否正在运行
3. 确保您有稳定的互联网连接
4. 验证您是否有足够的权限来创建目录和运行 Docker