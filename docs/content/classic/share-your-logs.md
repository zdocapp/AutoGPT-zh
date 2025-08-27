# 分享日志以帮助我们改进 AutoGPT

是否注意到您的代理出现异常行为？是否有有趣的用例想要分享？或者想要报告某个错误？
请按照以下步骤启用日志并上传。您可以在提交问题报告或与我们讨论问题时附上这些日志。

## 启用调试日志

活动日志、错误日志和调试日志位于 `./logs` 目录

要打印调试日志：

```shell
./autogpt.sh --debug     # on Linux / macOS

.\autogpt.bat --debug    # on Windows

docker compose run --rm auto-gpt --debug    # in Docker
```

## 查看和分享日志

您可以通过 [e2b](https://e2b.dev) 查看和分享日志。
![E2b 日志仪表板](../imgs/e2b-dashboard.png)

1. 访问 [autogpt.e2b.dev](https://autogpt.e2b.dev) 并登录
2. 您将看到来自 AutoGPT 团队其他成员的日志可供查看
3. 或者您可以上传自己的日志。点击"Upload log folder"按钮，选择您生成的调试日志目录。等待1-2秒后页面将重新加载
4. 您可以通过分享浏览器中的URL来共享日志
![E2b 日志URL](../imgs/e2b-log-url.png)

### 为日志添加标签

您可以为团队其他成员添加自定义标签到日志中。如果您想表明代理在应对挑战时遇到问题，这将非常有用。

E2b 提供3种严重级别：

- Success（成功）
- Warning（警告）
- Error（错误）

您可以随意命名您的标签。

#### 如何添加标签

1. 点击日志文件夹名称左侧的 "plus" 按钮。

    ![E2b 标签按钮](../imgs/e2b-tag-button.png)

1. 输入新标签的名称。

1. 选择严重性级别。

    ![E2b 新建标签](../imgs/e2b-new-tag.png)