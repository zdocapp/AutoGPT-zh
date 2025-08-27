# AutoGPT 入门指南：自托管教程

## 项目说明

本指南将帮助您设置项目的服务器和构建器。

<!-- The video is listed in the root Readme.md of the repo -->

<!--We also offer this in video format. You can check it out [here](https://github.com/Significant-Gravitas/AutoGPT?tab=readme-ov-file#how-to-setup-for-self-hosting). -->

!!! warning
    **请勿遵循任何外部教程，因为它们很可能已过时**

## 前置条件

要设置服务器，您需要安装以下组件：

- [Node.js](https://nodejs.org/en/)
- [Docker](https://docs.docker.com/get-docker/)
- [Git](https://git-scm.com/downloads)

### 检查是否已安装 Node.js 和 NPM

我们使用 Node.js 来运行前端应用程序。

如需安装 Node.js 的帮助：
https://nodejs.org/en/download/

NPM 已包含在 Node.js 中，但如果您需要安装 NPM 的帮助：
https://docs.npmjs.com/downloading-and-installing-node-js-and-npm

您可以通过运行以下命令来检查是否已安装 Node.js 和 NPM：

```bash
node -v
npm -v
```

安装 Node.js 后，您可以继续下一步。

### 检查是否已安装 Docker 和 Docker Compose

Docker 用于容器化应用程序，而 Docker Compose 用于编排多容器 Docker 应用程序。

如需安装 Docker 的帮助：
https://docs.docker.com/desktop/

Docker-compose 已包含在 Docker Desktop 中，但如果您需要安装 Docker Compose 的帮助：
https://docs.docker.com/compose/install/

您可以通过运行以下命令来检查是否已安装 Docker：

```bash
docker -v
docker compose -v
```

安装好 Docker 和 Docker Compose 后，您可以继续下一步。

<details>
 <summary>
 Raspberry Pi 5 特定说明
 </summary>
    在运行 Raspberry Pi OS 的 Raspberry Pi 5 上，默认的 16K 页面大小会导致 <code>supabase-vector</code> 容器出现问题（预期应为 4K）。
    </br>
    要解决此问题，请编辑 <code>/boot/firmware/config.txt</code> 并添加：
    </br>
    ```ini
    kernel=kernel8.img
    ```
    然后重新启动。您可以通过以下命令检查页面大小：
    </br>
    ```bash
    getconf PAGESIZE
    ```
    <code>16384</code> 表示 16K（不正确），<code>4096</code> 表示 4K（正确）。
    调整后，<code>docker compose up -d --build</code> 应能正常工作。
    </br>
    更多背景信息请参阅 <a href="https://github.com/supabase/supabase/issues/33816">supabase/supabase #33816</a>。
</details>

## 使用自动设置脚本快速设置（推荐）

如果您在本地自托管 AutoGPT，我们推荐使用官方设置脚本简化流程。这将安装依赖项（如 Docker），拉取最新代码，并以最小工作量启动应用程序。

适用于 macOS/Linux：

```
curl -fsSL https://setup.agpt.co/install.sh -o install.sh && bash install.sh
```

适用于 Windows（PowerShell）：

```
powershell -c "iwr https://setup.agpt.co/install.bat -o install.bat; ./install.bat"
```

如果您正在进行开发或测试设置，并希望跳过手动配置，此方法是理想选择。

## 手动设置

### 克隆代码库

第一步是将 AutoGPT 代码库克隆到您的计算机。
为此，请在计算机上的文件夹中打开终端窗口并运行：

```
git clone https://github.com/Significant-Gravitas/AutoGPT.git
```

如果遇到问题，请按照[此指南](https://docs.github.com/en/repositories/creating-and-managing-repositories/cloning-a-repository)操作。

完成后，您可以继续设置过程。

### 运行 AutoGPT 平台

要运行该平台，请按照以下步骤操作：

* 进入 AutoGPT 文件夹内的 `autogpt_platform` 目录：
  ```bash
   cd AutoGPT/autogpt_platform
  ```

- 将 `.env.default` 文件复制到 `autogpt_platform` 中的 `.env`：

  ```
   cp .env.default .env
  ```

  此命令会将 `.env.default` 文件复制到 `autogpt_platform` 目录中的 `.env`。您可以修改 `.env` 文件以添加自己的环境变量。

- 运行平台服务：
  ```
   docker compose up -d --build
  ```
  此命令将以分离模式启动 `docker-compose.yml` 文件中定义的所有必要后端服务。

### 检查应用程序是否正在运行

您可以在浏览器中访问 [http://localhost:3000](http://localhost:3000) 来检查服务器是否正在运行。

**注意：**

默认情况下，不同服务的应用程序运行在以下端口：

前端 UI 服务器：3000
后端 WebSocket 服务器：8001
执行 API REST 服务器：8006

### 补充说明

您可能需要在 `autogpt_platform/backend` 目录中的 `.env` 文件中更改加密密钥。

要生成新的加密密钥，请在 python 中运行以下命令：

```python
from cryptography.fernet import Fernet;Fernet.generate_key().decode()
```

或者在 `autogpt_platform/backend` 目录中运行以下命令：

```bash
poetry run cli gen-encrypt-key
```

然后，将 `autogpt_platform/backend/.env` 文件中的现有密钥替换为新密钥。

### 📌 Windows 安装说明

在 Windows 上安装 Docker 时，**强烈建议**选择 **WSL 2** 而不是 Hyper-V。使用 Hyper-V 可能导致与 Supabase 的兼容性问题，导致 `supabase-db` 容器被标记为 **不健康**。

#### **为 Docker 启用 WSL 2 的步骤：**

1. 安装 [WSL 2](https://learn.microsoft.com/en-us/windows/wsl/install)。
2. 确保您的 Docker 设置使用 WSL 2 作为默认后端：

- 打开 **Docker Desktop**。
  - 导航至 **设置 > 常规**。
  - 勾选 **使用基于 WSL 2 的引擎**。

3. 重启 **Docker Desktop**。

#### **已经使用 Hyper-V 安装了 Docker？**

如果您最初使用 Hyper-V 安装了 Docker，您**无需重新安装**。您可以通过以下步骤切换到 WSL 2：

1. 打开 **Docker Desktop**。
2. 转到 **设置 > 常规**。
3. 启用 **使用基于 WSL 2 的引擎**。
4. 重启 Docker。

🚨 **警告：** 启用 WSL 2 可能会**清除您现有的容器和构建历史记录**。如果您有重要的容器，请在切换前考虑备份它们。

更多详细信息，请参阅 [Docker 官方文档](https://docs.docker.com/desktop/windows/wsl/)。

## 开发指南

### 前端开发

#### 本地运行前端

要在本地运行前端，您需要在机器上安装 Node.js 和 PNPM。

安装 [Node.js](https://nodejs.org/en/download/) 以管理依赖项并运行前端应用程序。

安装 [PNPM](https://pnpm.io/installation) 以管理前端依赖项。

运行服务依赖项（后端、数据库、消息队列等）：

```sh
docker compose --profile local up deps_backend --build --detach
```

进入 `autogpt_platform/frontend` 目录：

```sh
cd frontend
```

安装依赖：

```sh
pnpm install
```

生成 API 客户端：

```sh
pnpm generate:api-client
```

运行前端应用程序：

```sh
pnpm dev
```

#### 代码格式化与检查

项目中已设置自动格式化程序和检查工具。运行方式如下：
格式化代码：

```sh
pnpm format
```

检查代码：

```sh
pnpm lint
```

#### 测试

要运行测试，您可以使用以下命令：

```sh
pnpm test
```

### 后端开发

#### 本地运行后端

要在本地运行后端，您需要在机器上安装 Python 3.10 或更高版本。

安装 [Poetry](https://python-poetry.org/docs/#installation) 以管理依赖项和虚拟环境。

运行后端依赖项（数据库、消息队列等）：

```sh
docker compose --profile local up deps --build --detach
```

进入 `autogpt_platform/backend` 目录：

```sh
cd backend
```

安装依赖：

```sh
poetry install --with dev
```

运行后端服务器：

```sh
poetry run app
```

#### 代码格式化与检查

项目中已设置自动格式化程序和检查工具。运行方式如下：

格式化代码：

```sh
poetry run format
```

检查代码：

```sh
poetry run lint
```

#### 测试

要运行测试：

```sh
poetry run pytest -s 
```

## 添加新的智能体模块

要添加新的智能体模块，您需要创建一个继承自 `Block` 的新类并提供以下信息：

* 所有模块代码都应位于 `blocks` (`backend.blocks`) 模块中
* `input_schema`：输入数据的模式，由 Pydantic 对象表示
* `output_schema`：输出数据的模式，由 Pydantic 对象表示
* `run` 方法：模块的主要逻辑
* `test_input` 和 `test_output`：模块的示例输入和输出数据，将用于自动测试模块
* 您可以使用 `test_mock` 字段来模拟模块中声明的函数，以便进行单元测试
* 完成模块创建后，可以通过运行 `poetry run pytest backend/blocks/test/test_block.py -s` 来测试它
* 将您的更改创建 Pull Request 到存储库的 `dev` 分支，以便与社区分享 :)