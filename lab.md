# 实验室虚拟机设置

## 已安装的软件

| 软件 | 链接 |
| --- | --- |
| Windows 10（内部版本 2004） | <https://www.microsoft.com/software-download/windows10> |
| Visual Studio Code | <https://code.visualstudio.com> |
| Visual Studio Code Azure 帐户扩展 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account> |
| Visual Studio Code Azure Functions 扩展 | <https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions> |
| Visual Studio Code Azure 资源管理器工具扩展 | <https://marketplace.visualstudio.com/items?itemName=msazurermtools.azurerm-vscode-tools> |
| Visual Studio Code Azure CLI 工具扩展 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.azurecli> |
| Visual Studio Code PowerShell 扩展 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.PowerShell> |
| Visual Studio Code C# 扩展 | <https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp> |
| PowerShell 7 | <https://github.com/PowerShell/PowerShell/releases/tag/v7.0.3> |
| .NET Core 3.1 SDK | <https://dotnet.microsoft.com/download/dotnet-core/3.1> |
| Azure PowerShell | <https://docs.microsoft.com/powershell/azure/install-az-ps> |
| Azure CLI | <https://docs.microsoft.com/cli/azure/install-azure-cli> |
| Azure 存储资源管理器 | <https://azure.microsoft.com/features/storage-explorer> |
| .NET 工具 - HttpRepl | <https://github.com/dotnet/HttpRepl> |
| Azure Functions Core Tools | <https://docs.microsoft.com/azure/azure-functions/functions-run-local#v3> |
| Windows 终端 | <https://aka.ms/terminal> |
| Microsoft Edge (Chromium) | <https://www.microsoft.com/edge> |

## 附加配置

- 启用 ClearType
  
- 配置 Microsoft Edge 作为默认浏览器

- 更新 VSCode 配置

  ```json
  {
    "editor.fontFamily": "'Cascadia Code', Consolas, 'Courier New', monospace",
    "update.enableWindowsBackgroundUpdates": false,
    "update.mode": "manual",
    "terminal.integrated.shell.windows": "C:\\Program Files\\PowerShell\\7\\pwsh.exe",
    "workbench.startupEditor": "none",
    "terminal.integrated.rendererType": "dom",
    "csharp.suppressDotnetInstallWarning": true,
    "csharp.suppressDotnetRestoreNotification": true,
    "csharp.supressBuildAssetsNotification": true,
    "azureFunctions.showProjectWarning": false
  }
  ```

- 更新 Windows 终端配置

  ```json
  {
    "$schema": "https://aka.ms/terminal-profiles-schema",
    "defaultProfile": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
    "profiles": [
      {
        "guid": "{574e775e-4f2a-5b96-ac1e-a2962a402336}",
        "useAcrylic": true,
        "acrylicOpacity": 0.85,
        "colorScheme": "Campbell",
        "fontFace": "Cascadia Code",
        "hidden": false,
        "name": "PowerShell",
        "source": "Windows.Terminal.PowershellCore"
      },
      {
        "guid": "{b453ae62-4e3d-5e58-b989-0a998ec441b8}",
        "hidden": false,
        "name": "Azure Cloud Shell",
        "source": "Windows.Terminal.Azure"
      }
    ],
    "schemes": [],
    "keybindings": []
  }
  ```

- 将开始菜单与任务栏配置为仅包含以下图标：
  - 文件资源管理器
  - Microsoft Edge
  - Windows 终端
  - Visual Studio Code
  - Azure 存储资源管理器

- 禁用 PowerShell 7 更新通知

  1. [创建名为 ``POWERSHELL_UPDATECHECK`` 的环境变量](https://docs.microsoft.com/powershell/module/microsoft.powershell.core/about/about_update_notifications?view=powershell-7)
  
  1. 将环境变量的值设置为 ``Off``（区分大小写）

- 至少运行一次 Azure Functions Core Tools 以配置 Windows 防火墙

  ```bash
  func init test --worker-runtime dotnet
  cd test
  func new --template 'HTTP trigger' --name web
  func start --build
  ```
