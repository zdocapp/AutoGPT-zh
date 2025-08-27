# 测试

我们使用 [Playwright](https://playwright.dev/) 作为测试框架。

## 开始之前

几乎所有测试都需要您同时运行前端和后端服务器。如果没有运行这些服务器，您会遇到奇怪且难以调试的错误，因为测试会在应用程序处于不可交互状态时尝试与其进行交互。

## 运行测试

要运行测试，您可以使用以下命令：

无界面运行测试（headless 模式）：

```bash
pnpm test
```

如果您希望在可以识别每个使用的定位器的 UI 中运行测试，可以使用以下命令：

```bash
pnpm test-ui
```

您还可以向测试命令传递 `--debug` 参数，以在视图模式下打开浏览器而不是无头模式。这适用于 `pnpm test` 和 `pnpm test-ui` 命令。

```bash
pnpm test --debug
```

在 CI 环境中，我们以无头模式运行测试，使用多个浏览器，并对失败的测试最多重试 2 次。

您可以在 [playwright.config.ts](https://github.com/Significant-Gravitas/Autogpt/blob/master/autogpt_platform/frontend/playwright.config.ts) 中找到完整配置。

### 调试测试

有多种不同的调试测试方法。

我偏好混合使用 Playwright 的测试编辑器和 VSCode。

无论您做什么，都应该**始终**仔细检查定位器是否正确。Playwright 经常会"超时"而不会给出定位器错误的提示信息，因为它无法找到元素。您可以通过浏览器的开发者工具进行检查，在使用检查和选择元素工具时，它们应该在元素标签页中可见。

#### 使用 Playwright 测试编辑器

如果您需要调试测试，可以使用以下命令在 Playwright 测试编辑器中打开测试。这对于想要在浏览器中查看测试、了解页面状态以及测试使用的定位器非常有帮助。

```bash
pnpm test --debug --test-name-pattern="test-name"
```

#### 使用 VSCode

您可以安装 [Playwright Test for VSCode](https://marketplace.visualstudio.com/items?itemName=ms-playwright.playwright) 扩展来获得 Playwright API 的自动补全功能（ID：`ms-playwright.playwright`）。

安装此扩展将在 VSCode 中启用 `Test Explorer` 视图，允许您运行、调试和查看当前项目中的所有测试。在测试中添加断点并运行它们将自动在正确的上下文中打开测试编辑器。

## 设置用于生成测试

使用 Playwright，您可以从现有的用户会话录制中生成测试。这对于创建更能代表用户与应用程序交互方式的测试非常有用。我们通常使用此功能来检查元素将具有哪些 ID 以及需要添加哪些 ID。

持续登录非常烦人，因此我强烈建议在测试中使用保存的会话。
这将生成一个名为 `.auth/gentest-user.json` 的文件，可在所有未来的生成测试中加载，这样您就不必每次都登录。

### 为生成测试保存会话以供永久使用

```bash
pnpm gentests --save-storage .auth/gentest-user.json
```

登录后使用 `CTRL + C` 停止会话，并将 `--save-storage` 标志替换为 `--load-storage`，以便为所有未来测试加载会话。

### 为生成测试加载会话以供永久使用

```bash
pnpm gentests --load-storage .auth/gentest-user.json
```

## 如何创建新测试

测试由页面对象和测试文件组成。

页面对象是一个包含与页面交互方法的类。

测试文件是包含一个页面或一组页面测试的文件。

### 创建新的页面对象

对于测试，我们使用[页面对象模型](https://playwright.dev/docs/pom)。这是一种模式，其中每个页面都是一个类，包含该页面的所有方法和定位器。
这有助于保持测试的组织性和可读性，并确保在 UI 更改时只需在一个位置更新测试。

您应当创建一个新的页面对象（仅在需要添加新页面或**跨多个测试的UI元素**时），参考以下示例。

我们扩展了包含共享方法的 `BasePage` 类，这些方法适用于具有通用功能（如导航栏）的页面。如果您添加了类似的功能（例如侧边栏），则应将其添加到 `BasePage` 类中。否则，您应当创建一个新的页面对象。

每个页面对象应位于单独的文件中，并按 `page-name.page.ts` 格式命名。
页面对象应包含用户可在该页面上执行的操作方法，例如点击按钮、填写表单等。它还应包含该页面特有的各种有用抽象。例如，`BuildPage` 有一个连接块的方法。

以下是配置文件页面对象的简化示例：

<!-- I know there's a floating } but it closes the imported code block and makes this a valid copy-able block -->

```typescript title="frontend/src/tests/pages/profile.page.ts"
--8<-- "autogpt_platform/frontend/src/tests/pages/profile.page.ts:ProfilePageExample"
}
```

### 创建新的测试文件

对于测试，我们使用页面对象来创建测试。每个测试文件应位于 `tests` 文件夹中，并按 `test-name.spec.ts` 格式命名。一个测试文件可以包含多个测试，每个测试应与相同的概念功能相关。例如，构建页面的测试文件可以包含构建智能体、创建输入输出以及连接块的测试。如果您想专门测试构建智能体，可以创建一个名为 `building-agents.spec.ts` 的新测试。

测试可以继承一个或多个页面对象，具备前置操作和后置操作，以及许多其他特性。您可以在[此处](https://playwright.dev/docs/test-actions)了解更多关于不同特性及其使用方法的信息。

一个优秀的聚焦测试（`单元测试`或`单一概念测试`）应具备：

- 简短的名称描述测试内容
- 单一测试概念（构建智能体、添加所有模块、连接两个模块等）
- 检查前置条件、操作和后置条件，并在过程中进行多重验证

一个优秀的非聚焦测试（`集成测试`或`多概念测试`）应具备：

- 简短的名称描述测试内容
- 多个测试概念（构建多个智能体、创建->导出->导入->运行智能体、通过多种方式连接具有多个输入输出的模块等）
- 明确的用户体验验证（例如点击构建按钮确保智能体成功构建，或点击导出按钮确保智能体正确导出并显示在监控系统中）
- 不专注于单一概念，而是测试应用程序的整体流程。请记住您测试的不是像素级完美的UI，而是用户体验

一个优秀的测试套件应合理搭配聚焦测试和非聚焦测试。

### 聚焦测试示例与说明

```typescript title="frontend/src/tests/build.spec.ts"
--8<-- "autogpt_platform/frontend/src/tests/build.spec.ts:BuildPageExample"
});
```

1. `test.describe` 用于将测试分组。此处用于将构建页面的所有测试归为一组。
2. `let buildPage: BuildPage;` 用于创建构建页面的新实例。
3. `test.beforeEach` 用于在每个测试前运行代码。此处用于在每个测试前登录用户。`page` 是从 fixture 传入的页面对象，`loginPage` 是登录页面的页面对象，`testUser` 是从 fixture 传入的用户对象。fixture 用于处理身份验证和其他常见的共享状态任务。
4. `await page.goto("/login");` 用于导航到登录页面。
5. `await test.expect(page).toHaveURL("/");` 用于检查页面是否已导航到主页（即用户已登录）。
6. `test("user can add a block", async ({ page }) => {` 用于定义新测试。
7. `await test.expect(buildPage.isLoaded()).resolves.toBeTruthy();` 用于检查构建页面是否已加载。这可以合理地在 `test.beforeEach` 中完成，但为了清晰起见在此处完成，因为该测试套件中还有其他测试。
8. `await test.expect(page).toHaveURL(new RegExp("/.*build"));` 用于检查页面是否已导航到构建页面。
9. `await buildPage.closeTutorial();` 用于关闭构建页面上的教程。值得注意的是，这个包装函数实际上并不关心教程是否已打开，它确保教程**将**被关闭。这是一个有用且常见的模式，用于确保某件事会被完成，而不关心它是否已经完成。它可以用于切换设置、关闭/打开侧边栏等场景。
10. `await buildPage.openBlocksPanel();` 用于打开构建页面上的积木面板，其方式与 `closeTutorial` 函数描述相同。
11. `await buildPage.addBlock(block);` 用于将特定积木添加到构建页面。这是另一个可以在代码行内完成的实用函数，但由于页面对象模式的工作方式，我们应该将它们保留在页面对象中。（它也有助于保持测试代码更清晰，并在其他测试中使用）
12. `await buildPage.closeBlocksPanel();` 用于关闭构建页面上的积木面板。
13. `await test.expect(buildPage.hasBlock(block)).resolves.toBeTruthy();` 用于检查积木是否已添加到构建页面。

### 在测试间传递信息

你可以使用 `testInfo` 对象在测试间传递信息。这在诸如在 beforeAll 中传递代理 ID 的场景中非常有用，这样你就可以为多个测试设置共享的配置。

```typescript title="frontend/src/tests/monitor.spec.ts"
--8<-- "autogpt_platform/frontend/src/tests/monitor.spec.ts:AttachAgentId"

  test("test can read the agent id", async ({ page }, testInfo) => {
    --8<-- "autogpt_platform/frontend/src/tests/monitor.spec.ts:ReadAgentId"
    /// ... Do something with the agent id here
  });
});
```

## 另请参阅

- [编写测试](https://playwright.dev/docs/writing-tests)
- [代码生成](https://playwright.dev/docs/codegen-intro)
- [测试 UI 模式](https://playwright.dev/docs/test-ui-mode)
- [追踪查看器](https://playwright.dev/docs/trace-viewer-intro)
- [VSCode 入门指南](https://playwright.dev/docs/getting-started-vscode)
- [调试测试](https://playwright.dev/docs/debug)
- [测试夹具](https://playwright.dev/docs/test-fixtures)
- [全局设置与拆卸](https://playwright.dev/docs/test-global-setup-teardown)
- [测试参数化](https://playwright.dev/docs/test-parameterize)
- [测试事件](https://playwright.dev/docs/events)
- [测试组件](https://playwright.dev/docs/test-components)
- [测试分片](https://playwright.dev/docs/test-sharding)
- [无障碍测试](https://playwright.dev/docs/accessibility-testing)
- [身份验证](https://playwright.dev/docs/auth)
- [模拟](https://playwright.dev/docs/mock)
- [模拟浏览器 API](https://playwright.dev/docs/mock-browser-apis)
- [代码生成](https://playwright.dev/docs/codegen)
- [页面](https://playwright.dev/docs/pages)
- [测试注解](https://playwright.dev/docs/test-annotations)