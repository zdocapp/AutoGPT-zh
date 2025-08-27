# 贡献文档

我们欢迎您为我们的文档做出贡献！如果您想要贡献，请按照以下步骤操作。

## 设置文档环境

1. 克隆代码库：

    ```shell
    git clone github.com/Significant-Gravitas/AutoGPT.git
    ```

1. 安装依赖项：

    ```shell
    python -m pip install -r docs/requirements.txt
    ```

    或

    ```shell
    python3 -m pip install -r docs/requirements.txt
    ```

1. 使用 mkdocs 的实时服务器开始迭代：

    ```shell
    mkdocs serve
    ```

1. 打开浏览器并访问 `http://127.0.0.1:8000`。

1. 当您保存更改时，服务器将自动重新加载文档。

## 添加新页面

1. 在 `docs/content` 目录中创建一个新的 markdown 文件。
1. 在 `mkdocs.yml` 文件的 `nav` 部分添加新页面。
1. 向新的 markdown 文件中添加内容。
1. 运行 `mkdocs serve` 查看您的更改。

## 检查链接

要检查文档中的损坏链接，请运行 `mkdocs build` 并在控制台输出中查找警告信息。

## 提交拉取请求

当您准备好提交更改时，请创建一个拉取请求。我们将审核您的更改，如果合适的话会将其合并。