# AutoGPT + Docker 指南

!!! important
    需要使用 Docker Compose 版本 1.29.0 或更高版本来支持 Compose 文件格式的 3.9 版本。
    您可以通过运行以下命令来检查系统上安装的 Docker Compose 版本：

    ```shell
    docker compose version
    ```

    This will display the version of Docker Compose that is currently installed on your system.

    If you need to upgrade Docker Compose to a newer version, you can follow the installation instructions in the Docker documentation: https://docs.docker.com/compose/install/

## 基础设置

1. 确保已安装 Docker，请参阅[系统要求](./index.md#requirements)
2. 为 AutoGPT 创建项目目录

    ```shell
    mkdir AutoGPT
    cd AutoGPT
    ```

3. 在项目目录中，创建名为 `docker-compose.yml` 的文件：

    <details>
    <summary>
      <code>docker-compose.yml></code> 适用于 <= v0.4.7 版本
    </summary>

    ```yaml
    version: "3.9"
    services:
      auto-gpt:
        image: significantgravitas/auto-gpt
        env_file:
          - .env
        profiles: ["exclude-from-up"]
        volumes:
          - ./auto_gpt_workspace:/app/auto_gpt_workspace
          - ./data:/app/data
          ## allow auto-gpt to write logs to disk
          - ./logs:/app/logs
          ## uncomment following lines if you want to make use of these files
          ## you must have them existing in the same folder as this docker-compose.yml
          #- type: bind
          #  source: ./azure.yaml
          #  target: /app/azure.yaml
    ```
    </details>

    <details>
    <summary>
      <code>docker-compose.yml></code> 适用于 > v0.4.7 版本（包括 <code>master</code>）
    </summary>

    ```yaml
    version: "3.9"
    services:
      auto-gpt:
        image: significantgravitas/auto-gpt
        env_file:
          - .env
        ports:
          - "8000:8000"  # remove this if you just want to run a single agent in TTY mode
        profiles: ["exclude-from-up"]
        volumes:
          - ./data:/app/data
          ## allow auto-gpt to write logs to disk
          - ./logs:/app/logs
          ## uncomment following lines if you want to make use of these files
          ## you must have them existing in the same folder as this docker-compose.yml
          ## component configuration file
          #- type: bind
          #  source: ./config.json
          #  target: /app/config.json
    ```
    </details>

1. 下载 [`.env.template`][.env.template] 并将其保存为 AutoGPT 文件夹中的 `.env` 文件。
2. 按照标准[配置说明](./index.md#completing-the-setup)进行操作，
   从第 3 步开始，不包括 `poetry install` 步骤。
3. 从 [Docker Hub] 拉取最新镜像

    ```shell
    docker pull significantgravitas/auto-gpt
    ```
4. _可选：挂载配置文件。_
      如果您有组件配置文件，例如 `config.json`，请将其放置在 `classic/original_autogpt/data/` 目录中。或者将其放置在 `classic/original_autogpt/` 目录中，并取消注释 `docker-compose.yml` 中挂载该文件的行。
      有关配置的更多信息，请参阅[组件配置](../../forge/components/components.md#json-configuration)

!!! note "Docker only supports headless browsing"
    AutoGPT 默认使用无头模式浏览器：`HEADLESS_BROWSER=True`。
    请勿在 Docker 环境中更改此设置，否则 AutoGPT 将崩溃。

[.env.template]: https://github.com/Significant-Gravitas/AutoGPT/tree/master/classic/original_autogpt/.env.template

[Docker Hub]: https://hub.docker.com/r/significantgravitas/auto-gpt

## 开发者设置

!!! tip
    如果您已克隆代码库并已（或希望）对代码库进行修改，请使用此设置。

1. 将 `.env.template` 复制为 `.env`。
2. 遵循标准[配置说明](./index.md#completing-the-setup)，
   从第 3 步开始，并排除 `poetry install` 步骤。

## 使用 Docker 运行 AutoGPT

完成上述设置说明后，您可以使用以下命令运行 AutoGPT：

```shell
docker compose run --rm auto-gpt
```

此操作将创建并启动一个 AutoGPT 容器，并在应用程序停止后移除该容器。
这并不意味着您的数据会丢失：应用程序生成的数据存储在 `data` 文件夹中。

子命令和参数的使用方式与[用户指南]中描述的一致：

* 运行 AutoGPT：
    ```shell
    docker compose run --rm auto-gpt serve
    ```
* 在 TTY 模式下运行 AutoGPT，并启用连续模式：
    ```shell
    docker compose run --rm auto-gpt run --continuous
    ```
* 在 TTY 模式下运行 AutoGPT，并为所有活跃插件安装依赖项：
    ```shell
    docker compose run --rm auto-gpt run --install-plugin-deps
    ```

如果您有胆量，也可以使用"原生" docker 命令构建并运行：

```shell
docker build -t autogpt .
docker run -it --env-file=.env -v $PWD:/app autogpt
docker run -it --env-file=.env -v $PWD:/app --rm autogpt --gpt3only --continuous
```

[user guide]: ../usage.md/#command-line-interface