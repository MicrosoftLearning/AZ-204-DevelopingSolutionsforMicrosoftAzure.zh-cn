---
lab:
  title: 在 Azure 应用服务中切换部署槽位
  description: 了解如何在 Azure 应用服务中切换部署槽位。 在本练习中，你将：将一个简单的应用部署到应用服务；对应用进行一个小更改并将其部署到过渡槽；最后切换槽位，以便将更新后的应用投入生产环境中。
---

# 在 Azure 应用服务中切换部署槽位

在本练习中，你将使用 Azure CLI **az webapp up** 命令将基本 HTML+CSS 站点部署到 Azure 应用服务。 接下来，更新代码并将更改部署到过渡槽。 最后，切换槽位。

在本练习中执行的任务：

* 下载示例应用并将其部署到 Azure 应用服务。
* 创建过渡部署槽位。
* 更改示例应用并将其部署到过渡槽。
* 切换过渡槽和默认生产槽，以将更改移动到生产槽。

完成此练习大约需要 **20** 分钟。

## 开始之前

为了完成本练习，你需要：

* Azure 订阅。 如果你尚未拥有，可以注册一个 [https://azure.microsoft.com/](https://azure.microsoft.com/)。

## 下载并部署示例应用

在本部分中，你将下载示例应用并设置变量，以更方便地输入命令，然后使用 Azure CLI 命令创建 Azure 应用服务资源并部署静态 HTML 网站。

1. 在浏览器中，导航到 Azure 门户，网址为：[https://portal.azure.com](https://portal.azure.com)；如果出现提示，请使用 Azure 凭据登录。

1. 使用页面顶部搜索栏右侧的 **[\>_]** 按钮在 Azure 门户中创建新的 Cloud Shell，选择 ***Bash*** 环境。 Cloud Shell 在 Azure 门户底部的窗格中提供命令行接口。

    > **备注**：如果以前创建了使用 *PowerShell* 环境的 Cloud Shell，请将其切换到 ***Bash***。

1. 在 Cloud Shell 工具栏的“**设置**”菜单中，选择“**转到经典版本**”（这是使用代码编辑器所必需的）。

1. 运行以下 **git** 命令，以克隆示例应用存储库。

    ```bash
    git clone https://github.com/Azure-Samples/html-docs-hello-world.git
    ```

1. 通过运行以下命令设置变量以保存资源组和应用名称。 记下运行命令后显示的 **appName** 的值，稍后将在本练习中用到它。

    ```bash
    resourceGroup=rg-mywebapp

    appName=mywebapp$RANDOM
    echo $appName
    ```

1. 导航到包含示例代码的目录并运行 **az webapp up** 命令。 **备注：** 此命令可能需要几分钟时间才能运行。

    ```bash
    cd html-docs-hello-world

    az webapp up -g $resourceGroup -n $appName --sku P0V3 --html
    ```

    现在部署已完成，可以查看 Web 应用。

1. 在 Azure 门户中，导航到部署的 Web 应用。 可以在“**搜索资源、服务和文档 (G + /)**”搜索栏中输入之前记录的名称，然后从列表中选择资源。

1. 选择位于“**Essentials**”部分的“**默认域**”字段中的 Web 应用链接。 该链接将在新选项卡中打开网站。

## 将更新后的代码部署到部署槽位

在本部分中，你将创建一个部署槽位，修改应用中的 HTML，并将更新后的代码部署到新的部署槽位。

### 创建部署槽位 

1. 返回包含 Azure 门户 和 Cloud Shell 的选项卡。

1. 在 Cloud Shell 中输入以下命令，以创建名为 *staging* 的部署槽位。

    ```bash
    az webapp deployment slot create -n $appName -g $resourceGroup --slot staging
    ```

1. 等待命令完成，然后选择左侧菜单中的“**部署 > 部署槽位**”以查看 Web 应用的部署槽位。 请注意，新槽位的名称包含附加到 Web 应用名称中的 *-staging*

### 更新代码并将其部署到过渡槽

1. 在 Cloud Shell 中，键入 `code index.html` 以打开编辑器。 在 `<h1>` 标题标签中，将“*Azure 应用服务 - 示例静态 HTML 网站*”更改为“*Azure 应用服务过渡槽*” - 或更改为你想要的任何其他内容。

1. 使用命令 **ctrl-s** 保存，使用 **ctrl-q** 退出。

1. 在 Cloud Shell 中运行以下命令，以创建更新项目的 zip 文件。 下一步将需要 zip 或 Web 应用程序资源 (WAR) 文件。

    ```bash
    zip -r stagingcode.zip .
    ```

1. 在 Cloud Shell 中运行以下命令，以将更新部署到过渡槽。

    ```bash
    az webapp deploy -g $resourceGroup -n $appName --src-path ./stagingcode.zip --slot staging
    ```

1. 在 Web 应用的左侧菜单中选择“**部署 > 部署槽位**”，然后选择之前创建的过渡槽。

1. 选择“**Essentials**”部分“**默认域**”字段中的链接。 该链接将在新选项卡中打开过渡槽的网站。

## 切换过渡槽和生产槽。

可以使用工具栏中的“**切换**”选项在 Azure 门户中执行切换。 如果选择左侧菜单中的“**概述**”或“**部署 > 部署槽位**”，“**切换**”选项将显示在工具栏中。

1. 在 Azure 门户中，选择工具栏中的“**切换**”，以打开“**切换**”面板。

1. 查看切换面板中的设置。 “**源**”应显示 **-staging** 槽，而“**目标**”应显示默认生产槽。

    ![“切换”面板的屏幕截图。](./media/02/app-service-swap-panel.png)

1. 选择“**开始切换**”并等待操作完成。 可以在“**通知**”面板中跟踪完成情况，选择门户顶部的铃铛图标即可打开该面板。

1. 若要验证切换情况，请导航到已部署的 Web 应用。 在“**搜索资源、服务和文档 (G + /)**”搜索栏中输入之前创建的 Web 应用名称（例如 *mywebapp12360*），然后从列表中选择资源。

1. 选择位于“**Essentials**”部分的“**默认域**”字段中的 Web 应用链接。 该链接将在新选项卡中打开网站（生产槽）。

1. 若要验证更改，可能需要刷新页面才能显示这些更改。

## 清理资源

完成练习后，应删除已创建的云资源，以避免不必要的资源使用。

1. 导航到创建的资源组，然后查看本练习中使用的资源内容。
1. 在工具栏中，选择“删除资源组”****。
1. 输入资源组名称，并确认要删除该资源组。
