---
title: 中断性变更：Kestrel：默认检测运行时的配置更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Kestrel：默认检测运行时的配置更改
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 35b733cd872b9d6d944896349921c54f672b31b0
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498095"
---
# <a name="kestrel-configuration-changes-at-run-time-detected-by-default"></a><span data-ttu-id="7c38d-103">Kestrel：默认检测运行时的配置更改</span><span class="sxs-lookup"><span data-stu-id="7c38d-103">Kestrel: Configuration changes at run time detected by default</span></span>

<span data-ttu-id="7c38d-104">Kestrel 现在会在运行时响应对项目的 `IConfiguration` 实例 `Kestrel` 部分（例如 appsettings.json）的更改。</span><span class="sxs-lookup"><span data-stu-id="7c38d-104">Kestrel now reacts to changes made to the `Kestrel` section of the project's `IConfiguration` instance (for example, *appsettings.json*) at run time.</span></span> <span data-ttu-id="7c38d-105">若要详细了解如何使用 appsettings.json 配置 Kestrel，请参阅[终结点配置](/aspnet/core/fundamentals/servers/kestrel#endpoint-configuration)中的 appsettings.json 示例。</span><span class="sxs-lookup"><span data-stu-id="7c38d-105">To learn more about how to configure Kestrel using *appsettings.json*, see the *appsettings.json* example in [Endpoint configuration](/aspnet/core/fundamentals/servers/kestrel#endpoint-configuration).</span></span>

<span data-ttu-id="7c38d-106">Kestrel 将根据需要绑定、取消绑定和重新绑定终结点，以响应这些配置更改。</span><span class="sxs-lookup"><span data-stu-id="7c38d-106">Kestrel will bind, unbind, and rebind endpoints as necessary to react to these configuration changes.</span></span>

<span data-ttu-id="7c38d-107">有关讨论，请参阅 [dotnet/aspnetcore#22807](https://github.com/dotnet/aspnetcore/issues/22807)。</span><span class="sxs-lookup"><span data-stu-id="7c38d-107">For discussion, see issue [dotnet/aspnetcore#22807](https://github.com/dotnet/aspnetcore/issues/22807).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="7c38d-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="7c38d-108">Version introduced</span></span>

<span data-ttu-id="7c38d-109">5.0 预览版 7</span><span class="sxs-lookup"><span data-stu-id="7c38d-109">5.0 Preview 7</span></span>

## <a name="old-behavior"></a><span data-ttu-id="7c38d-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="7c38d-110">Old behavior</span></span>

<span data-ttu-id="7c38d-111">在 ASP.NET Core 5.0 预览版 6 之前，Kestrel 不支持在运行时更改配置。</span><span class="sxs-lookup"><span data-stu-id="7c38d-111">Before ASP.NET Core 5.0 Preview 6, Kestrel didn't support changing configuration at run time.</span></span>

<span data-ttu-id="7c38d-112">在 ASP.NET Core 5.0 预览版 6 中，可以选择在运行时响应配置更改（这是现在的默认行为）。</span><span class="sxs-lookup"><span data-stu-id="7c38d-112">In ASP.NET Core 5.0 Preview 6, you could opt into the now-default behavior of reacting to configuration changes at run time.</span></span> <span data-ttu-id="7c38d-113">要选择加入，需要手动绑定 Kestrel 的配置：</span><span class="sxs-lookup"><span data-stu-id="7c38d-113">Opting in required binding Kestrel's configuration manually:</span></span>

```csharp
using Microsoft.AspNetCore.Hosting;
using Microsoft.Extensions.Hosting;

public class Program
{
    public static void Main(string[] args) =>
        CreateHostBuilder(args).Build().Run();

    public static IHostBuilder CreateHostBuilder(string[] args) =>
        Host.CreateDefaultBuilder(args)
            .ConfigureWebHostDefaults(webBuilder =>
            {
                webBuilder.UseKestrel((builderContext, kestrelOptions) =>
                {
                    kestrelOptions.Configure(
                        builderContext.Configuration.GetSection("Kestrel"), reloadOnChange: true);
                });

                webBuilder.UseStartup<Startup>();
            });
}
```

## <a name="new-behavior"></a><span data-ttu-id="7c38d-114">新行为</span><span class="sxs-lookup"><span data-stu-id="7c38d-114">New behavior</span></span>

<span data-ttu-id="7c38d-115">默认情况下，Kestrel 会在运行时响应配置更改。</span><span class="sxs-lookup"><span data-stu-id="7c38d-115">Kestrel reacts to configuration changes at run time by default.</span></span> <span data-ttu-id="7c38d-116">为支持该更改，<xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> 默认使用 `reloadOnChange: true` 调用 `KestrelServerOptions.Configure(IConfiguration, bool)`。</span><span class="sxs-lookup"><span data-stu-id="7c38d-116">To support that change, <xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> calls `KestrelServerOptions.Configure(IConfiguration, bool)` with `reloadOnChange: true` by default.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="7c38d-117">更改原因</span><span class="sxs-lookup"><span data-stu-id="7c38d-117">Reason for change</span></span>

<span data-ttu-id="7c38d-118">此项更改是为了在运行时支持终结点重新配置，而无需完全重启服务器。</span><span class="sxs-lookup"><span data-stu-id="7c38d-118">The change was made to support endpoint reconfiguration at run time without completely restarting the server.</span></span> <span data-ttu-id="7c38d-119">与完全重启服务器不同，未更改的终结点甚至不会暂时解除绑定。</span><span class="sxs-lookup"><span data-stu-id="7c38d-119">Unlike with a full server restart, unchanged endpoints aren't unbound even temporarily.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="7c38d-120">建议操作</span><span class="sxs-lookup"><span data-stu-id="7c38d-120">Recommended action</span></span>

* <span data-ttu-id="7c38d-121">在大多数情况下，Kestrel 的默认配置部分在运行时不会更改，这种更改不会有任何影响，也不需要执行任何操作。</span><span class="sxs-lookup"><span data-stu-id="7c38d-121">For most scenarios in which Kestrel's default configuration section doesn't change at run time, this change has no impact and no action is needed.</span></span>
* <span data-ttu-id="7c38d-122">对于 Kestrel 的默认配置部分在运行时发生更改并且 Kestrel 应响应这种更改的情况，这是现在的默认行为。</span><span class="sxs-lookup"><span data-stu-id="7c38d-122">For scenarios in which Kestrel's default configuration section does change at run time and Kestrel should react to it, this is now the default behavior.</span></span>
* <span data-ttu-id="7c38d-123">对于 Kestrel 的默认配置部分在运行时更改并且 Kestrel 不应对其做出反应的情况，你可以选择退出，如下所示：</span><span class="sxs-lookup"><span data-stu-id="7c38d-123">For scenarios in which Kestrel's default configuration section changes at run time and Kestrel shouldn't react to it, you can opt out as follows:</span></span>

    ```csharp
    using Microsoft.AspNetCore.Hosting;
    using Microsoft.Extensions.Hosting;

    public class Program
    {
        public static void Main(string[] args) =>
            CreateHostBuilder(args).Build().Run();

        public static IHostBuilder CreateHostBuilder(string[] args) =>
            Host.CreateDefaultBuilder(args)
                .ConfigureWebHostDefaults(webBuilder =>
                {
                    webBuilder.UseKestrel((builderContext, kestrelOptions) =>
                    {
                        kestrelOptions.Configure(
                            builderContext.Configuration.GetSection("Kestrel"), reloadOnChange: false);
                    });

                    webBuilder.UseStartup<Startup>();
                });
    }
    ```

<span data-ttu-id="7c38d-124">**注意：**</span><span class="sxs-lookup"><span data-stu-id="7c38d-124">**Notes:**</span></span>

<span data-ttu-id="7c38d-125">此更改不会修改 `KestrelServerOptions.Configure(IConfiguration)` 重载的行为，它仍默认为 `reloadOnChange: false` 行为。</span><span class="sxs-lookup"><span data-stu-id="7c38d-125">This change doesn't modify the behavior of the `KestrelServerOptions.Configure(IConfiguration)` overload, which still defaults to the `reloadOnChange: false` behavior.</span></span>

<span data-ttu-id="7c38d-126">另外，请务必确保配置源支持重载。</span><span class="sxs-lookup"><span data-stu-id="7c38d-126">It's also important to make sure the configuration source supports reloading.</span></span> <span data-ttu-id="7c38d-127">对于 JSON 源，将通过调用 `AddJsonFile(path, reloadOnChange: true)` 配置重载。</span><span class="sxs-lookup"><span data-stu-id="7c38d-127">For JSON sources, reloading is configured by calling `AddJsonFile(path, reloadOnChange: true)`.</span></span> <span data-ttu-id="7c38d-128">默认情况下，已为 appsettings.json 和 appsettings.{Environment}.json 配置重载 。</span><span class="sxs-lookup"><span data-stu-id="7c38d-128">Reloading is already configured by default for *appsettings.json* and *appsettings.{Environment}.json*.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="7c38d-129">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="7c38d-129">Affected APIs</span></span>

<xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults`

-->
