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
# <a name="kestrel-configuration-changes-at-run-time-detected-by-default"></a>Kestrel：默认检测运行时的配置更改

Kestrel 现在会在运行时响应对项目的 `IConfiguration` 实例 `Kestrel` 部分（例如 appsettings.json）的更改。 若要详细了解如何使用 appsettings.json 配置 Kestrel，请参阅[终结点配置](/aspnet/core/fundamentals/servers/kestrel#endpoint-configuration)中的 appsettings.json 示例。

Kestrel 将根据需要绑定、取消绑定和重新绑定终结点，以响应这些配置更改。

有关讨论，请参阅 [dotnet/aspnetcore#22807](https://github.com/dotnet/aspnetcore/issues/22807)。

## <a name="version-introduced"></a>引入的版本

5.0 预览版 7

## <a name="old-behavior"></a>旧行为

在 ASP.NET Core 5.0 预览版 6 之前，Kestrel 不支持在运行时更改配置。

在 ASP.NET Core 5.0 预览版 6 中，可以选择在运行时响应配置更改（这是现在的默认行为）。 要选择加入，需要手动绑定 Kestrel 的配置：

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

## <a name="new-behavior"></a>新行为

默认情况下，Kestrel 会在运行时响应配置更改。 为支持该更改，<xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A> 默认使用 `reloadOnChange: true` 调用 `KestrelServerOptions.Configure(IConfiguration, bool)`。

## <a name="reason-for-change"></a>更改原因

此项更改是为了在运行时支持终结点重新配置，而无需完全重启服务器。 与完全重启服务器不同，未更改的终结点甚至不会暂时解除绑定。

## <a name="recommended-action"></a>建议操作

* 在大多数情况下，Kestrel 的默认配置部分在运行时不会更改，这种更改不会有任何影响，也不需要执行任何操作。
* 对于 Kestrel 的默认配置部分在运行时发生更改并且 Kestrel 应响应这种更改的情况，这是现在的默认行为。
* 对于 Kestrel 的默认配置部分在运行时更改并且 Kestrel 不应对其做出反应的情况，你可以选择退出，如下所示：

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

**注意：**

此更改不会修改 `KestrelServerOptions.Configure(IConfiguration)` 重载的行为，它仍默认为 `reloadOnChange: false` 行为。

另外，请务必确保配置源支持重载。 对于 JSON 源，将通过调用 `AddJsonFile(path, reloadOnChange: true)` 配置重载。 默认情况下，已为 appsettings.json 和 appsettings.{Environment}.json 配置重载 。

## <a name="affected-apis"></a>受影响的 API

<xref:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults%2A?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`Overload:Microsoft.Extensions.Hosting.GenericHostBuilderExtensions.ConfigureWebHostDefaults`

-->
