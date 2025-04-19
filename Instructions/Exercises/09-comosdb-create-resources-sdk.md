---
lab:
  title: 使用 .NET 创建适用于 NoSQL 的 Azure Cosmos DB 资源
  description: 了解如何使用 Microsoft .NET SDK v3 在 Azure Cosmos DB 中创建数据库和容器资源。
---

# 使用 .NET 创建适用于 NoSQL 的 Azure Cosmos DB 资源

在本练习中，你将创建一个控制台应用，用于在 Azure Cosmos DB 中创建容器、数据库和项。

在本练习中执行的任务：

* 创建 Azure Cosmos DB 帐户
* 创建控制台应用
* 运行控制台应用并验证结果

完成此练习大约需要 30 分钟。

## 准备工作

在开始之前，请确保满足以下要求：

* Azure 订阅。 如果你还没有该订阅，可以[注册一个](https://azure.microsoft.com/)。

## 创建 Azure Cosmos DB 帐户

在本部分练习中，你将创建资源组和 Azure Cosmos DB 帐户。 还会记录帐户的终结点和访问密钥。

1. 在浏览器中，导航到 Azure 门户，网址为：[https://portal.azure.com](https://portal.azure.com)；如果出现提示，请使用 Azure 凭据登录。

1. 使用页面顶部搜索栏右侧的 **[\>_]** 按钮在 Azure 门户中创建新的 Cloud Shell，选择 ***Bash*** 环境。 Cloud Shell 在 Azure 门户底部的窗格中提供命令行接口。

    > **备注**：如果以前创建了使用 *PowerShell* 环境的 Cloud Shell，请将其切换到 ***Bash***。

1. 在 Cloud Shell 工具栏的“**设置**”菜单中，选择“**转到经典版本**”（这是使用代码编辑器所必需的）。

1. 为此练习所需的资源创建资源组。 将 **\<myResourceGroup>** 替换为希望用于资源组的名称。 如果需要，可以将 **useast** 替换为附近的区域。 

    ```
    az group create --location useast --name <myResourceGroup>
    ```

1. 运行以下命令，创建 Azure Cosmos DB 帐户。 将 **\<myCosmosDBacct>** 替换为一个唯一名称以标识自己的 Azure Cosmos DB 帐户。 将 **\<myResourceGroup>** 替换为之前所选的名称。

    >**备注：** 帐户名称只能包含小写字母、数字和连字符 (-)。 它的长度必须介于 3 到 31 个字符之间。 此命令需要几分钟时间才能完成。

    ```
    az cosmosdb create -n <myCosmosDBacct> -g <myResourceGroup>
    ```

1.  运行以下命令，检索 Azure Cosmos DB 帐户的 **documentEndpoint**。 记录命令结果中的终结点，稍后的练习中会用到它。 将 **\<myCosmosDBacct>** 和 **\<myResourceGroup>** 替换为你创建的名称。

    ```bash
    az cosmosdb show -n <myCosmosDBacct> -g <myResourceGroup> --query "documentEndpoint" --output tsv
    ```

1. 通过使用以下命令检索帐户的主键。 记录命令结果中的 **primaryMasterKey**，以供在代码中使用。 将 **\<myCosmosDBacct>** 和 **\<myResourceGroup>** 替换为你创建的名称。

     ```
    # Retrieve the primary key
    az cosmosdb keys list -n <myCosmosDBacct> -g <myResourceGroup>
    ```

## 创建控制台应用程序

在将所需的资源部署到 Azure 后，下一步是使用 Visual Studio Code 中同一终端窗口设置控制台应用程序。

1. 为该项目创建一个文件夹，并更改到该文件夹。

    ```bash
    mkdir cosmosdb
    cd cosmosdb
    ```

1. 创建 .NET 控制台应用。

    ```dotnetcli
    dotnet new console --framework net8.0
    ```

1. 运行以下命令，将 **Microsoft.Azure.Cosmos** 包以及支持的 **Newtonsoft.Json** 包添加到项目中。

    ```dotnetcli
    dotnet add package Microsoft.Azure.Cosmos --version 3.*
    dotnet add package Newtonsoft.Json --version 13.*
    ```

现在，可以使用 Cloud Shell 中的编辑器替换 **Program.cs** 文件中的模板代码。

### 添加项目的起始代码

1. 在 Cloud Shell 中运行以下命令，以开始编辑应用程序。

    ```bash
    code Program.cs
    ```

1. 在 **using** 语句之后，用以下代码片段替换任何现有代码。 请确保按照代码注释中的说明替换 **\<documentEndpoint>** 和 **\<primaryKey>** 的占位符值。

    该代码提供了应用的整体结构和一些必要元素。 查看代码中的注释，在说明中指定的区域中添加代码。 

    ```csharp
    using Microsoft.Azure.Cosmos;
    
    namespace CosmosExercise
    {
        // This class represents a product in the Cosmos DB container
        public class Product
        {
            public string? id { get; set; }
            public string? name { get; set; }
            public string? description { get; set; }
        }
    
        public class Program
        {
            // Cosmos DB account URL - replace with your actual Cosmos DB account URL
            static string cosmosDbAccountUrl = "<documentEndpoint>";
    
            // Cosmos DB account key - replace with your actual Cosmos DB account key
            static string accountKey = "<primaryKey>";
    
            // Name of the database to create or use
            static string databaseName = "myDatabase";
    
            // Name of the container to create or use
            static string containerName = "myContainer";
    
            public static async Task Main(string[] args)
            {
                // Create the Cosmos DB client using the account URL and key
    
    
                try
                {
                    // Create a database if it doesn't already exist
    
    
                    // Create a container with a specified partition key
    
    
                    // Define a typed item (Product) to add to the container
    
    
                    // Add the item to the container
                    // The partition key ensures the item is stored in the correct partition
    
    
                }
                catch (CosmosException ex)
                {
                    // Handle Cosmos DB-specific exceptions
                    // Log the status code and error message for debugging
                    Console.WriteLine($"Cosmos DB Error: {ex.StatusCode} - {ex.Message}");
                }
                catch (Exception ex)
                {
                    // Handle general exceptions
                    // Log the error message for debugging
                    Console.WriteLine($"Error: {ex.Message}");
                }
            }
        }
    }
    ```

### 添加代码，以创建客户端并执行操作 

在这部分练习中，你将在项目的指定区域中添加代码，以创建：客户端、数据库、容器，并向容器添加示例项。

1. 在 **// 使用帐户 URL 和密钥创建 Cosmos DB 客户端**注释后的空白处，添加以下代码。 此代码定义用于连接到 Azure Cosmos DB 帐户的客户端。

    ```csharp
    CosmosClient client = new(
        accountEndpoint: cosmosDbAccountUrl,
        authKeyOrResourceToken: accountKey
    );
    ```

    >备注：最佳做法是使用 *Azure 标识*库中的 **DefaultAzureCredential**。 这可能需要在 Azure 中进行一些额外的配置要求，具体取决于订阅的设置方式。 

1. 在 **// 创建数据库（如果尚不存在）** 注释后的空白处，添加以下代码。 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. 在 **// 创建具有指定分区键的容器**注释后的空白处，添加以下代码。 

    ```csharp
    Microsoft.Azure.Cosmos.Database database = await client.CreateDatabaseIfNotExistsAsync(databaseName);
    Console.WriteLine($"Created or retrieved database: {database.Id}");
    ```

1. 在 **// 定义类型化项 (Product) 以添加到容器**注释后的空白处，添加以下代码。 这将定义添加到容器中的项。

    ```csharp
    Product newItem = new Product
    {
        id = Guid.NewGuid().ToString(), // Generate a unique ID for the product
        name = "Sample Item",
        description = "This is a sample item in my Azure Cosmos DB exercise."
    };
    ```

1. 在 **// 将项添加到容器**注释后的空白处，添加以下代码。 

    ```csharp
    ItemResponse<Product> createResponse = await container.CreateItemAsync(
        item: newItem,
        partitionKey: new PartitionKey(newItem.id)
    );
    ```

1. 代码完成后，保存进度时使用 **ctrl + q** 保存文件，然后使用 **ctrl + q** 退出编辑器。

1. 在 Cloud Shell 中运行以下命令，测试项目中是否存在任何错误。 如果看到错误，请在编辑器中打开 *Program.cs* 文件，并检查是否有缺失代码或粘贴错误。

    ```bash
    dotnet build
    ```

现在项目已完成，可以开始运行应用程序并在 Azure 门户中验证结果。

## 运行应用程序并验证结果。

1. 在 Cloud Shell 中，运行 `dotnet run` 命令。 输出内容应类似于以下示例。

    ```
    Created or retrieved database: myDatabase
    Created or retrieved container: myContainer
    Created item: c549c3fa-054d-40db-a42b-c05deabbc4a6
    Request charge: 6.29 RUs
    ```

1. 在 Azure 门户中，导航到之前创建的 Azure Cosmos DB 资源。 在左侧导航栏中，选择“**数据资源管理器**”。 在“**数据资源管理器**”中，选择 **myDatabase**，然后展开 **myContainer**。 可以通过选择“**项**”来查看创建的项。

    ![显示“数据资源管理器”中“项”的位置的屏幕截图。](./media/09/cosmos-data-explorer.png)

## 清理资源

完成练习后，应删除已创建的云资源，以避免不必要的资源使用。

1. 导航到创建的资源组，然后查看本练习中使用的资源内容。
1. 在工具栏中，选择“删除资源组”****。
1. 输入资源组名称，并确认要删除该资源组。
