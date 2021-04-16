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
# <a name="work-with-data"></a><span data-ttu-id="238c3-103">处理数据</span><span class="sxs-lookup"><span data-stu-id="238c3-103">Work with data</span></span>

<span data-ttu-id="238c3-104">数据访问是 ASP.NET Web Forms 应用的核心功能。</span><span class="sxs-lookup"><span data-stu-id="238c3-104">Data access is the backbone of an ASP.NET Web Forms app.</span></span> <span data-ttu-id="238c3-105">如果要构建 Web 窗体，会对该数据造成什么影响？</span><span class="sxs-lookup"><span data-stu-id="238c3-105">If you're building forms for the web, what happens to that data?</span></span> <span data-ttu-id="238c3-106">通过 Web Forms，可使用以下数据访问技术与数据库进行交互：</span><span class="sxs-lookup"><span data-stu-id="238c3-106">With Web Forms, there were multiple data access techniques you could use to interact with a database:</span></span>

- <span data-ttu-id="238c3-107">“数据源”</span><span class="sxs-lookup"><span data-stu-id="238c3-107">Data Sources</span></span>
- <span data-ttu-id="238c3-108">ADO.NET</span><span class="sxs-lookup"><span data-stu-id="238c3-108">ADO.NET</span></span>
- <span data-ttu-id="238c3-109">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="238c3-109">Entity Framework</span></span>

<span data-ttu-id="238c3-110">数据源是可以放置在 Web Forms 上的控件，并且可以像配置其他控件一样对其进行配置。</span><span class="sxs-lookup"><span data-stu-id="238c3-110">Data Sources were controls that you could place on a Web Forms page and configure like other controls.</span></span> <span data-ttu-id="238c3-111">Visual Studio 提供了一组友好对话框，用于将控件配置和绑定到 Web Forms 页。</span><span class="sxs-lookup"><span data-stu-id="238c3-111">Visual Studio provided a friendly set of dialogs to configure and bind the controls to your Web Forms pages.</span></span> <span data-ttu-id="238c3-112">首次发布 Web 窗体时，喜欢“低代码”或“无代码”方法的开发人员更喜欢此技术。</span><span class="sxs-lookup"><span data-stu-id="238c3-112">Developers who enjoy a "low code" or "no code" approach preferred this technique when Web Forms was first released.</span></span>

![“数据源”](media/data/datasources.png)

<span data-ttu-id="238c3-114">ADO.NET 是与数据库进行交互的低级别方法。</span><span class="sxs-lookup"><span data-stu-id="238c3-114">ADO.NET is the low-level approach to interacting with a database.</span></span> <span data-ttu-id="238c3-115">应用可以使用命令、记录集和用于交互的数据集创建与数据库的连接。</span><span class="sxs-lookup"><span data-stu-id="238c3-115">Your apps could create a connection to the database with Commands, Recordsets, and Datasets for interacting.</span></span> <span data-ttu-id="238c3-116">然后结果可以绑定到屏幕上的字段，而无需太多代码。</span><span class="sxs-lookup"><span data-stu-id="238c3-116">The results could then be bound to fields on screen without much code.</span></span> <span data-ttu-id="238c3-117">此方法的缺点是：每一组 ADO.NET 对象（`Connection`、`Command` 和 `Recordset`）都会绑定到由数据库供应商提供的库。</span><span class="sxs-lookup"><span data-stu-id="238c3-117">The drawback of this approach was that each set of ADO.NET objects (`Connection`, `Command`, and `Recordset`) was bound to libraries provided by a database vendor.</span></span> <span data-ttu-id="238c3-118">使用这些组件会使代码变得刚性，并且难以迁移到其他数据库。</span><span class="sxs-lookup"><span data-stu-id="238c3-118">Use of these components made the code rigid and difficult to migrate to a different database.</span></span>

## <a name="entity-framework"></a><span data-ttu-id="238c3-119">Entity Framework</span><span class="sxs-lookup"><span data-stu-id="238c3-119">Entity Framework</span></span>

<span data-ttu-id="238c3-120">实体框架 (EF) 是开放源对象关系映射框架，由.NET Foundation 维护。</span><span class="sxs-lookup"><span data-stu-id="238c3-120">Entity Framework (EF) is the open source object-relational mapping framework maintained by the .NET Foundation.</span></span> <span data-ttu-id="238c3-121">EF 最初通过 .NET Framework 发布，允许为数据库连接、存储架构和交互生成代码。</span><span class="sxs-lookup"><span data-stu-id="238c3-121">Initially released with .NET Framework, EF allows for generating code for the database connections, storage schemas, and interactions.</span></span> <span data-ttu-id="238c3-122">通过此抽象，可以专注于应用的业务规则，并使数据库能够由受信任的数据库管理员管理。</span><span class="sxs-lookup"><span data-stu-id="238c3-122">With this abstraction, you can focus on your app's business rules and allow the database to be managed by a trusted database administrator.</span></span> <span data-ttu-id="238c3-123">在 .NET 中，可使用名为 EF Core 的更新版本的 EF。</span><span class="sxs-lookup"><span data-stu-id="238c3-123">In .NET, you can use an updated version of EF called EF Core.</span></span> <span data-ttu-id="238c3-124">EF Core 通过使用 `dotnet ef` 命令行工具提供的一系列命令，帮助生成和维护代码和数据库之间的交互。</span><span class="sxs-lookup"><span data-stu-id="238c3-124">EF Core helps generate and maintain the interactions between your code and the database with a series of commands that are available for you using the `dotnet ef` command-line tool.</span></span> <span data-ttu-id="238c3-125">让我们查看一些示例，了解如何使用数据库。</span><span class="sxs-lookup"><span data-stu-id="238c3-125">Let's take a look at a few samples to get you working with a database.</span></span>

### <a name="ef-code-first"></a><span data-ttu-id="238c3-126">EF Code First</span><span class="sxs-lookup"><span data-stu-id="238c3-126">EF Code First</span></span>

<span data-ttu-id="238c3-127">开始生成数据库交互的一种快速方法是从你想要使用的类对象开始。</span><span class="sxs-lookup"><span data-stu-id="238c3-127">A quick way to get started building your database interactions is to start with the class objects you want to work with.</span></span> <span data-ttu-id="238c3-128">EF 提供一种工具，帮助为你的类生成相应的数据库代码。</span><span class="sxs-lookup"><span data-stu-id="238c3-128">EF provides a tool to help generate the appropriate database code for your classes.</span></span> <span data-ttu-id="238c3-129">此方法称为“Code First”开发。</span><span class="sxs-lookup"><span data-stu-id="238c3-129">This approach is called "Code First" development.</span></span> <span data-ttu-id="238c3-130">对于我们想要存储在 Microsoft SQL Server 等关系数据库中的示例店面应用，请考虑使用以下 `Product` 类。</span><span class="sxs-lookup"><span data-stu-id="238c3-130">Consider the following `Product` class for a sample storefront app that we want to store in a relational database like Microsoft SQL Server.</span></span>

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

<span data-ttu-id="238c3-131">Product 有一个主键和三个将在我们的数据库中创建的其他字段：</span><span class="sxs-lookup"><span data-stu-id="238c3-131">Product has a primary key and three additional fields that would be created in our database:</span></span>  

- <span data-ttu-id="238c3-132">EF 根据约定将 `Id` 属性标识为主键。</span><span class="sxs-lookup"><span data-stu-id="238c3-132">EF will identify the `Id` property as a primary key by convention.</span></span>
- <span data-ttu-id="238c3-133">`Name` 存储在为文本存储配置的列中。</span><span class="sxs-lookup"><span data-stu-id="238c3-133">`Name` will be stored in a column configured for text storage.</span></span> <span data-ttu-id="238c3-134">修饰此属性的 `[Required]` 属性会添加 `not null` 约束，帮助强制执行属性的已声明行为。</span><span class="sxs-lookup"><span data-stu-id="238c3-134">The `[Required]` attribute decorating this property will add a `not null` constraint to help enforce this declared behavior of the property.</span></span>
- <span data-ttu-id="238c3-135">`Description` 存储在为文本存储配置的列中，配置的最大长度为 4000 个字符，如 `[MaxLength]` 属性所指示。</span><span class="sxs-lookup"><span data-stu-id="238c3-135">`Description` will be stored in a column configured for text storage, and have a maximum length configured of 4000 characters as dictated by the `[MaxLength]` attribute.</span></span> <span data-ttu-id="238c3-136">数据库架构将使用数据类型 `MaxLength` 和名为 `varchar(4000)` 的列进行配置。</span><span class="sxs-lookup"><span data-stu-id="238c3-136">The database schema will be configured with a column named `MaxLength` using data type `varchar(4000)`.</span></span>
- <span data-ttu-id="238c3-137">`Price` 属性存储为货币。</span><span class="sxs-lookup"><span data-stu-id="238c3-137">The `Price` property will be stored as currency.</span></span> <span data-ttu-id="238c3-138">`[Range]` 属性生成相应的约束，以防止数据存储超出声明的最小值和最大值。</span><span class="sxs-lookup"><span data-stu-id="238c3-138">The `[Range]` attribute will generate appropriate constraints to prevent data storage outside of the minimum and maximum values declared.</span></span>

<span data-ttu-id="238c3-139">我们需要将 `Product` 类添加到定义与数据库之间的连接和转换操作的数据库上下文。</span><span class="sxs-lookup"><span data-stu-id="238c3-139">We need to add this `Product` class to a database context class that defines the connection and translation operations with our database.</span></span>

```csharp
public class MyDbContext : DbContext
{
    public DbSet<Product> Products { get; set; }
}
```

<span data-ttu-id="238c3-140">`MyDbContext` 类提供一个属性，该属性定义 `Product` 类的访问和转换。</span><span class="sxs-lookup"><span data-stu-id="238c3-140">The `MyDbContext` class provides the one property that defines the access and translation for the `Product` class.</span></span>  <span data-ttu-id="238c3-141">应用程序在 `Startup` 类的 `ConfigureServices` 方法中使用以下项，对此类进行配置，使其与数据库进行交互：</span><span class="sxs-lookup"><span data-stu-id="238c3-141">Your application configures this class for interaction with the database using the following entries in the `Startup` class's `ConfigureServices` method:</span></span>

```csharp
services.AddDbContext<MyDbContext>(options =>
    options.UseSqlServer("MY DATABASE CONNECTION STRING"));
```

<span data-ttu-id="238c3-142">上述代码将连接到具有指定的连接字符串的 SQL Server 数据库。</span><span class="sxs-lookup"><span data-stu-id="238c3-142">The preceding code will connect to a SQL Server database with the specified connection string.</span></span> <span data-ttu-id="238c3-143">可以将连接字符串放置在 appsettings.json 文件、环境变量或其他配置存储位置中，并适当替换此嵌入的字符串。</span><span class="sxs-lookup"><span data-stu-id="238c3-143">You can place the connection string in your *appsettings.json* file, environment variables, or other configuration storage locations and replace this embedded string appropriately.</span></span>

<span data-ttu-id="238c3-144">然后可以使用以下命令生成适用于此类的数据库表：</span><span class="sxs-lookup"><span data-stu-id="238c3-144">You can then generate the database table appropriate for this class using the following commands:</span></span>

```dotnetcli
dotnet ef migrations add 'Create Product table'
dotnet ef database update
```

<span data-ttu-id="238c3-145">第一个命令将要对数据库架构作出的更改定义为名为 `Create Product table` 的新 EF 迁移。</span><span class="sxs-lookup"><span data-stu-id="238c3-145">The first command defines the changes you're making to the database schema as a new EF Migration called `Create Product table`.</span></span>  <span data-ttu-id="238c3-146">迁移定义如何应用和删除新的数据库更改。</span><span class="sxs-lookup"><span data-stu-id="238c3-146">A Migration defines how to apply and remove your new database changes.</span></span>

<span data-ttu-id="238c3-147">应用后，你的数据库中就有了一个简单的 `Product` 表，项目中也添加了一些新的类来帮助管理数据库模式。</span><span class="sxs-lookup"><span data-stu-id="238c3-147">Once applied, you have a simple `Product` table in your database and some new classes added to the project that help manage the database schema.</span></span>  <span data-ttu-id="238c3-148">默认情况下，你可以在名为“迁移”的新文件夹中找到这些生成的类。</span><span class="sxs-lookup"><span data-stu-id="238c3-148">You can find these generated classes, by default, in a new folder called *Migrations*.</span></span>  <span data-ttu-id="238c3-149">对 `Product` 类作出更改或添加更多与数据库进行交互的相关类时，需要使用迁移的新名称再次运行命令行命令。</span><span class="sxs-lookup"><span data-stu-id="238c3-149">When you make changes to the `Product` class or add more related classes you would like interacting with your database, you need to run the command-line commands again with a new name of the migration.</span></span>  <span data-ttu-id="238c3-150">此命令将生成另一组迁移类来更新数据库架构。</span><span class="sxs-lookup"><span data-stu-id="238c3-150">This command will generate another set of migration classes to update your database schema.</span></span>

### <a name="ef-database-first"></a><span data-ttu-id="238c3-151">EF Database-First</span><span class="sxs-lookup"><span data-stu-id="238c3-151">EF Database First</span></span>

<span data-ttu-id="238c3-152">对于现有数据库，可以使用 .NET 命令行工具为 EF Core 生成类。</span><span class="sxs-lookup"><span data-stu-id="238c3-152">For existing databases, you can generate the classes for EF Core by using the .NET command-line tools.</span></span> <span data-ttu-id="238c3-153">要为类搭建基架，请使用以下命令的变体：</span><span class="sxs-lookup"><span data-stu-id="238c3-153">To scaffold the classes, use a variation of the following command:</span></span>

```dotnetcli
dotnet ef dbcontext scaffold "CONNECTION STRING" Microsoft.EntityFrameworkCore.SqlServer -c MyDbContext -t Product -t Customer
```

<span data-ttu-id="238c3-154">上述命令使用指定的连接字符串和 `Microsoft.EntityFrameworkCore.SqlServer` 提供程序连接到数据库。</span><span class="sxs-lookup"><span data-stu-id="238c3-154">The preceding command connects to the database using the specified connection string and the `Microsoft.EntityFrameworkCore.SqlServer` provider.</span></span> <span data-ttu-id="238c3-155">连接以后，会创建一个名为 `MyDbContext` 的数据库上下文类。</span><span class="sxs-lookup"><span data-stu-id="238c3-155">Once connected, a database context class named `MyDbContext` is created.</span></span> <span data-ttu-id="238c3-156">此外，还会为使用 `-t` 选项指定的 `Product` 和 `Customer` 表创建支持类。</span><span class="sxs-lookup"><span data-stu-id="238c3-156">Additionally, supporting classes are created for the `Product` and `Customer` tables that were specified with the `-t` options.</span></span> <span data-ttu-id="238c3-157">此命令有许多配置选项，可生成适用于数据库的类层次结构。</span><span class="sxs-lookup"><span data-stu-id="238c3-157">There are many configuration options for this command to generate the class hierarchy appropriate for your database.</span></span> <span data-ttu-id="238c3-158">有关完整参考信息，请参阅[命令的文档](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold)。</span><span class="sxs-lookup"><span data-stu-id="238c3-158">For a complete reference, see [the command's documentation](/ef/core/miscellaneous/cli/dotnet#dotnet-ef-dbcontext-scaffold).</span></span>

<span data-ttu-id="238c3-159">有关 [EF Core](/ef/core/) 的详细信息可在 Microsoft Docs 站点上找到。</span><span class="sxs-lookup"><span data-stu-id="238c3-159">More information about [EF Core](/ef/core/) can be found on the Microsoft Docs site.</span></span>

## <a name="interact-with-web-services"></a><span data-ttu-id="238c3-160">与 Web 服务进行交互</span><span class="sxs-lookup"><span data-stu-id="238c3-160">Interact with web services</span></span>

<span data-ttu-id="238c3-161">首次发布 ASP.NET 后，SOAP 服务是 Web 服务器和客户端交换数据的首选方式。</span><span class="sxs-lookup"><span data-stu-id="238c3-161">When ASP.NET was first released, SOAP services were the preferred way for web servers and clients to exchange data.</span></span> <span data-ttu-id="238c3-162">自那时起发生了很多变化，与服务的首选交互方式已转变为直接的 HTTP 客户端交互。</span><span class="sxs-lookup"><span data-stu-id="238c3-162">Much has changed since that time, and the preferred interactions with services have shifted to direct HTTP client interactions.</span></span> <span data-ttu-id="238c3-163">通过 ASP.NET Core 和 Blazor，可以在 `Startup` 类的 `ConfigureServices` 方法中注册 `HttpClient` 的配置。</span><span class="sxs-lookup"><span data-stu-id="238c3-163">With ASP.NET Core and Blazor, you can register the configuration of your `HttpClient` in the `Startup` class's `ConfigureServices` method.</span></span> <span data-ttu-id="238c3-164">需要与 HTTP 终结点交互时，请使用该配置。</span><span class="sxs-lookup"><span data-stu-id="238c3-164">Use that configuration when you need to interact with the HTTP endpoint.</span></span> <span data-ttu-id="238c3-165">请考虑使用以下配置代码：</span><span class="sxs-lookup"><span data-stu-id="238c3-165">Consider the following configuration code:</span></span>

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

<span data-ttu-id="238c3-166">每当需要访问 GitHub 中的数据时，请创建名为 `github` 的客户端。</span><span class="sxs-lookup"><span data-stu-id="238c3-166">Whenever you need to access data from GitHub, create a client with a name of `github`.</span></span> <span data-ttu-id="238c3-167">为客户端配置了基址，并正确设置了请求标头。</span><span class="sxs-lookup"><span data-stu-id="238c3-167">The client is configured with the base address, and the request headers are set appropriately.</span></span> <span data-ttu-id="238c3-168">通过 `@inject` 指令或属性上的 `[Inject]` 属性将 `IHttpClientFactory` 注入你的 Blazor 组件。</span><span class="sxs-lookup"><span data-stu-id="238c3-168">Inject the `IHttpClientFactory` into your Blazor components with the `@inject` directive or an `[Inject]` attribute on a property.</span></span> <span data-ttu-id="238c3-169">使用以下语法创建命名客户端，并与服务进行交互：</span><span class="sxs-lookup"><span data-stu-id="238c3-169">Create your named client and interact with services using the following syntax:</span></span>

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

<span data-ttu-id="238c3-170">此方法会返回描述 dotnet/docs GitHub 存储库中的问题集合的字符串。</span><span class="sxs-lookup"><span data-stu-id="238c3-170">This method returns the string describing the collection of issues in the *dotnet/docs* GitHub repository.</span></span> <span data-ttu-id="238c3-171">它会以 JSON 格式返回内容，并反序列化为合适的 GitHub 问题对象。</span><span class="sxs-lookup"><span data-stu-id="238c3-171">It returns content in JSON format and is deserialized into appropriate GitHub issue objects.</span></span> <span data-ttu-id="238c3-172">有许多方法可以将 `HttpClientFactory` 配置为提供预先配置的 `HttpClient` 对象。</span><span class="sxs-lookup"><span data-stu-id="238c3-172">There are many ways that you can configure the `HttpClientFactory` to deliver preconfigured `HttpClient` objects.</span></span> <span data-ttu-id="238c3-173">尝试使用不同的名称和使用的各种 Web 服务的终结点对多个 `HttpClient` 实例进行配置。</span><span class="sxs-lookup"><span data-stu-id="238c3-173">Try configuring multiple `HttpClient` instances with different names and endpoints for the various web services you work with.</span></span> <span data-ttu-id="238c3-174">借助此方法，可以更容易地在每个页面上处理与这些服务的交互。</span><span class="sxs-lookup"><span data-stu-id="238c3-174">This approach will make your interactions with those services easier to work with on each page.</span></span> <span data-ttu-id="238c3-175">有关详细信息，请参阅 [IHttpClientFactory 的文档](/aspnet/core/fundamentals/http-requests)。</span><span class="sxs-lookup"><span data-stu-id="238c3-175">For more details, read [the documentation for the IHttpClientFactory](/aspnet/core/fundamentals/http-requests).</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="238c3-176">[上一页](forms-validation.md)
>[下一页](middleware.md)</span><span class="sxs-lookup"><span data-stu-id="238c3-176">[Previous](forms-validation.md)
[Next](middleware.md)</span></span>
