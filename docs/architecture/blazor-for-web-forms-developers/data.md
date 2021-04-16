---
title: 数据访问和管理
description: 了解如何访问和处理 ASP.NET Web Forms 和 Blazor 中的数据。
author: csharpfritz
ms.author: jefritz
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 66e6001cbcac612cb556e90fb86fd694ca7d1459
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509749"
---
# <a name="work-with-data"></a>处理数据

数据访问是 ASP.NET Web Forms 应用的核心功能。 如果要构建 Web 窗体，会对该数据造成什么影响？ 通过 Web Forms，可使用以下数据访问技术与数据库进行交互：

- “数据源”
- ADO.NET
- Entity Framework

数据源是可以放置在 Web Forms 上的控件，并且可以像配置其他控件一样对其进行配置。 Visual Studio 提供了一组友好对话框，用于将控件配置和绑定到 Web Forms 页。 首次发布 Web 窗体时，喜欢“低代码”或“无代码”方法的开发人员更喜欢此技术。

![“数据源”](media/data/datasources.png)

ADO.NET 是与数据库进行交互的低级别方法。 应用可以使用命令、记录集和用于交互的数据集创建与数据库的连接。 然后结果可以绑定到屏幕上的字段，而无需太多代码。 此方法的缺点是：每一组 ADO.NET 对象（`Connection`、`Command` 和 `Recordset`）都会绑定到由数据库供应商提供的库。 使用这些组件会使代码变得刚性，并且难以迁移到其他数据库。

## <a name="entity-framework"></a>Entity Framework

实体框架 (EF) 是开放源对象关系映射框架，由.NET Foundation 维护。 EF 最初通过 .NET Framework 发布，允许为数据库连接、存储架构和交互生成代码。 通过此抽象，可以专注于应用的业务规则，并使数据库能够由受信任的数据库管理员管理。 在 .NET 中，可使用名为 EF Core 的更新版本的 EF。 EF Core 通过使用 `dotnet ef` 命令行工具提供的一系列命令，帮助生成和维护代码和数据库之间的交互。 让我们查看一些示例，了解如何使用数据库。

### <a name="ef-code-first"></a>EF Code First

开始生成数据库交互的一种快速方法是从你想要使用的类对象开始。 EF 提供一种工具，帮助为你的类生成相应的数据库代码。 此方法称为“Code First”开发。 对于我们想要存储在 Microsoft SQL Server 等关系数据库中的示例店面应用，请考虑使用以下 `Product` 类。

```csharp
public class Product
{
    public int Id { get; set; }

    [Required]
    public string Name { get; set; }

    [MaxLength(4000)]
    public string Description { get; set; }

    [Range(0, 99999,99)]
    [DataType(DataType.Currency)]
    public decimal Price { get; set; }
}
```

Product 有一个主键和三个将在我们的数据库中创建的其他字段：  

- EF 根据约定将 `Id` 属性标识为主键。
- `Name` 存储在为文本存储配置的列中。 修饰此属性的 `[Required]` 属性会添加 `not null` 约束，帮助强制执行属性的已声明行为。
- `Description` 存储在为文本存储配置的列中，配置的最大长度为 4000 个字符，如 `[MaxLength]` 属性所指示。 数据库架构将使用数据类型 `MaxLength` 和名为 `varchar(4000)` 的列进行配置。
- `Price` 属性存储为货币。 `[Range]` 属性生成相应的约束，以防止数据存储超出声明的最小值和最大值。

我们需要将 `Product` 类添加到定义与数据库之间的连接和转换操作的数据库上下文。

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

`MyDbContext` 类提供一个属性，该属性定义 `Product` 类的访问和转换。  应用程序在 `Startup` 类的 `ConfigureServices` 方法中使用以下项，对此类进行配置，使其与数据库进行交互：

```csharp
services.AddDbContext<MyDbContext>(options =>
    options.UseSqlServer("MY DATABASE CONNECTION STRING"));
```

上述代码将连接到具有指定的连接字符串的 SQL Server 数据库。 可以将连接字符串放置在 appsettings.json 文件、环境变量或其他配置存储位置中，并适当替换此嵌入的字符串。

然后可以使用以下命令生成适用于此类的数据库表：

```dotnetcli
dotnet ef migrations add 'Create Product table'
dotnet ef database update
```

第一个命令将要对数据库架构作出的更改定义为名为 `Create Product table` 的新 EF 迁移。  迁移定义如何应用和删除新的数据库更改。

应用后，你的数据库中就有了一个简单的 `Product` 表，项目中也添加了一些新的类来帮助管理数据库模式。  默认情况下，你可以在名为“迁移”的新文件夹中找到这些生成的类。  对 `Product` 类作出更改或添加更多与数据库进行交互的相关类时，需要使用迁移的新名称再次运行命令行命令。  此命令将生成另一组迁移类来更新数据库架构。

### <a name="ef-database-first"></a>EF Database-First

对于现有数据库，可以使用 .NET 命令行工具为 EF Core 生成类。 要为类搭建基架，请使用以下命令的变体：

```dotnetcli
dotnet ef dbcontext scaffold "CONNECTION STRING" Microsoft.EntityFrameworkCore.SqlServer -c MyDbContext -t Product -t Customer
```

上述命令使用指定的连接字符串和 `Microsoft.EntityFrameworkCore.SqlServer` 提供程序连接到数据库。 连接以后，会创建一个名为 `MyDbContext` 的数据库上下文类。 此外，还会为使用 `-t` 选项指定的 `Product` 和 `Customer` 表创建支持类。 此命令有许多配置选项，可生成适用于数据库的类层次结构。 有关完整参考信息，请参阅[命令的文档](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)。

有关 [EF Core](/ef/core/) 的详细信息可在 Microsoft Docs 站点上找到。

## <a name="interact-with-web-services"></a>与 Web 服务进行交互

首次发布 ASP.NET 后，SOAP 服务是 Web 服务器和客户端交换数据的首选方式。 自那时起发生了很多变化，与服务的首选交互方式已转变为直接的 HTTP 客户端交互。 通过 ASP.NET Core 和 Blazor，可以在 `Startup` 类的 `ConfigureServices` 方法中注册 `HttpClient` 的配置。 需要与 HTTP 终结点交互时，请使用该配置。 请考虑使用以下配置代码：

```csharp
services.AddHttpClient("github", client =>
{
    client.BaseAddress = new Uri("http://api.github.com/");
    // Github API versioning
    client.DefaultRequestHeaders.Add("Accept", "application/vnd.github.v3+json");
    // Github requires a user-agent
    client.DefaultRequestHeaders.Add("User-Agent", "BlazorWebForms-Sample");
});
```

每当需要访问 GitHub 中的数据时，请创建名为 `github` 的客户端。 为客户端配置了基址，并正确设置了请求标头。 通过 `@inject` 指令或属性上的 `[Inject]` 属性将 `IHttpClientFactory` 注入你的 Blazor 组件。 使用以下语法创建命名客户端，并与服务进行交互：

```razor
@inject IHttpClientFactory factory

...

@code {
    protected override async Task OnInitializedAsync()
    {
        var client = factory.CreateClient("github");
        var response = await client.GetAsync("repos/dotnet/docs/issues");
        response.EnsureStatusCode();
        var content = await response.Content.ReadAsStringAsync();
    }
}
```

此方法会返回描述 dotnet/docs GitHub 存储库中的问题集合的字符串。 它会以 JSON 格式返回内容，并反序列化为合适的 GitHub 问题对象。 有许多方法可以将 `HttpClientFactory` 配置为提供预先配置的 `HttpClient` 对象。 尝试使用不同的名称和使用的各种 Web 服务的终结点对多个 `HttpClient` 实例进行配置。 借助此方法，可以更容易地在每个页面上处理与这些服务的交互。 有关详细信息，请参阅 [IHttpClientFactory 的文档](/aspnet/core/fundamentals/http-requests)。

>[!div class="step-by-step"]
>[上一页](forms-validation.md)
>[下一页](middleware.md)
