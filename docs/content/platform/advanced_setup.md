# 高级设置

以下高级步骤适用于具有系统管理员经验的用户。如果您对这些步骤不熟悉，请参考[基础设置指南](../platform/getting-started.md)。

## 项目说明

进行高级设置时，请首先按照[基础设置指南](../platform/getting-started.md)启动并运行服务器。当服务器正常运行后，您可以按照以下步骤根据特定需求配置服务器。

## 配置

### 通过环境变量设置配置

服务器使用环境变量存储配置。您可以在项目根目录的 `.env` 文件中设置这些环境变量。`.env` 文件应如下所示：

```bash
# .env
KEY1=value1
KEY2=value2
```

服务器启动时将自动加载 `.env` 文件。您也可以在 shell 中直接设置环境变量。请参考您操作系统的文档了解如何在当前会话中设置环境变量。

有效选项列在构建器和服务器目录根目录下的 `.env.default` 文件中。您可以将 `.env.default` 文件复制为 `.env` 并根据需要修改值。

```bash
# Copy the .env.default file to .env
cp .env.default .env
```

### 密钥目录

密钥目录位于 `./secrets`。您可以将所需的任何密钥存储在此目录中。服务器启动时将自动加载这些密钥。

名为 `my_secret` 的密钥示例如下所示：

```bash
# ./secrets/my_secret
my_secret_value
```

这在 Docker 环境中运行时非常有用，您可以将密钥复制到容器中，而无需在 Dockerfile 中暴露它们。

## 数据库选择

### PostgreSQL

我们使用 Supabase PostgreSQL 作为数据库。您需要将用于生成和运行 Prisma 的命令替换为以下内容

```bash
poetry run prisma generate --schema postgres/schema.prisma
```

这将生成适用于 PostgreSQL 的 Prisma 客户端。您还需要在单独的容器中运行 PostgreSQL 数据库。您可以使用 `rnd` 目录中的 `docker-compose.yml` 文件来运行 PostgreSQL 数据库。

```bash
cd autogpt_platform/
docker compose up -d --build
```

然后您可以从 `backend` 目录运行迁移。

```bash
cd ../backend
prisma migrate dev --schema postgres/schema.prisma
```

## AutoGPT 代理服务器高级设置

本指南将引导您完成使用外部数据库（postgres）的 Docker 化设置

### 环境配置

我们使用 Poetry 来管理依赖项。要设置项目，请在此目录内按照以下步骤操作：

0. 安装 Poetry
    ```sh
    pip install poetry
    ```
    
1. 配置 Poetry 在项目目录中使用 .venv
    ```sh
    poetry config virtualenvs.in-project true
    ```

2. 进入 poetry shell

   ```sh
   poetry shell
   ```

3. 安装依赖项

   ```sh
   poetry install
   ```

4. 复制 .env.default 为 .env

   ```sh
   cp .env.default .env
   ```

5. 生成 Prisma 客户端

   ```sh
   poetry run prisma generate
   ```

   > 如果 Prisma 为全局 Python 安装而不是虚拟环境生成客户端，目前的缓解方法是卸载全局 Prisma 包：
   >
   > ```sh
   > pip uninstall prisma
   > ```
   >
   > 然后再次运行生成命令。路径_应该_看起来像这样：  
   > `<some path>/pypoetry/virtualenvs/backend-TQIRSwR6-py3.12/bin/prisma`

6. 从 /rnd 文件夹运行 postgres 数据库

   ```sh
   cd autogpt_platform/
   docker compose up -d
   ```

7. 运行迁移（从 backend 文件夹）

   ```sh
   cd ../backend
   prisma migrate deploy
   ```

### 运行服务器

#### 直接启动服务器

运行以下命令：

```sh
poetry run app
```