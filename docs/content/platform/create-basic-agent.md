# **使用 AutoGPT 创建基础 AI 智能体**

## **概述**

本指南将引导您使用 AutoGPT 的可视化构建器创建一个简单的问答 AI 智能体。这是一个基础示例，可扩展为更复杂的智能体。

## **示例智能体：问答（使用 AI）**

使用输入和输出块创建简单问答智能体的分步指南。

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/ih57vRbH0H0?si=PGHx_qquYpXofiu_" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></center> 

## **所需组件**

1. 输入块
2. AI文本生成器块
3. 输出块

## **分步说明**

### **1. 设置基础结构**

1. 添加输入块
2. 添加 AI 文本生成器块
3. 添加输出块

### **2. 连接组件**

1. 将输入块连接到 AI 文本生成器的提示
2. 将 AI 文本生成器的响应连接到输出块的值

### **3. 配置块名称**

* 将输入块命名为："question"
* 将输出块命名为："answer"

### **4. 保存智能体**

1. 点击保存按钮
2. 为智能体命名（例如："question and answer"）

### **5. 测试智能体**

1. 点击运行按钮
2. 在输入字段中输入问题（例如："地球离冥王星有多远？"）
3. 通过以下任一方式查看结果：
    * "查看更多"选项
    * "智能体输出"部分

## **查看结果**

您可以通过两种方式访问 AI 的响应：

* 点击"查看更多"获取详细结果
* 检查"智能体输出"部分获取响应

## **示例智能体：计算器（不使用 AI）**

使用输入和输出块创建简单计算器代理的分步指南。

<center><iframe width="560" height="315" src="https://www.youtube.com/embed/ESLKHcXxRvA?si=i2L2sloLskSMO8_I" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture; web-share" referrerpolicy="strict-origin-when-cross-origin" allowfullscreen></iframe></center>

##**所需组件**

* 输入块（2个实例）
* 计算器块
* 输出块

## **设置说明**

### **1. 添加输入块**

1. 添加两个输入块，一个位于另一个下方
2. 将第一个输入块命名为"a"
3. 将第二个输入块命名为"b"

### **2. 添加计算器块**

1. 向工作区添加一个计算器块
2. 将其放置在输入块之后

### **3. 添加输出块**

1. 添加一个输出块
2. 将其命名为"results"

### **4. 连接块**

1. 将"a"输入块连接到计算器块的"a"输入
2. 将"b"输入块连接到计算器块的"b"输入
3. 将计算器块的结果连接到输出块的value输入

### **5. 保存代理**

1. 为代理命名（例如"简单计算代理"）
2. 保存配置

## **测试计算器**

### **运行计算**

1. 在两个输入块中输入数值
2. 选择所需操作（例如乘法）
3. 点击"运行"按钮执行计算

### **查看结果**

有两种方式可以查看计算结果：

1. 点击"查看更多"以查看详细输出
2. 检查"代理输出"部分，该部分显示来自输出块的结果

## **示例计算**

* 输入 A: 227
* 输入 B: 17
* 操作: 乘法
* 预期输出将在结果部分显示

## **提示**

* 修改后务必保存您的智能体
* 运行前验证所有连接是否正确建立
* 使用"查看更多"选项获取详细的输出信息

## **Note**

虽然这些都是基础示例，但您可以通过添加更多模块和功能来增强智能体，创建更复杂的交互。