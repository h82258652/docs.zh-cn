---
title: 通过 Polly 实现使用指数退避算法的 HTTP 调用重试
description: 了解如何使用 Polly 和 IHttpClientFactory 处理 HTTP 故障。
ms.date: 01/13/2021
ms.openlocfilehash: cd209aa7f2802ffea80e14f0e3e77cc4fc29b6d5
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122958"
---
# <a name="implement-http-call-retries-with-exponential-backoff-with-ihttpclientfactory-and-polly-policies"></a>通过 IHttpClientFactory 和 Polly 策略实现使用指数退避算法的 HTTP 调用重试

建议的使用指数退避算法的重试方法是利用更高级的 .NET 库，如开放源 [Polly](https://github.com/App-vNext/Polly) 库。

Polly 是一个 .NET 库，提供恢复能力和瞬态故障处理功能。 通过应用 Polly 策略（如重试、断路器、舱壁隔离、超时和回退）即可实现这些功能。 Polly 面向 .NET Framework 4.x 和 .NET Standard 1.0、1.1 和 2.0（支持 .NET Core 及更高版本）。

以下步骤说明如何通过集成到 `IHttpClientFactory` 中的 Polly（已在上一部分中说明）使用 Http 重试。

**引用 .NET 5 包**

自 .NET Core 2.1 起提供 `IHttpClientFactory`，但建议在项目中使用 NuGet 中的最新 .NET 5 包。 通常，还需要引用扩展包 `Microsoft.Extensions.Http.Polly`。

**使用 Polly 的重试策略在 Startup 中配置客户端**

如前面部分中所示，需要在标准 Startup.ConfigureServices(...) 方法中定义已命名或类型化的客户端 HttpClient 配置，但现在，可使用指数退避算法添加用于指定 Http 重试策略的增量代码，如下所示：

```csharp
//ConfigureServices()  - Startup.cs
services.AddHttpClient<IBasketService, BasketService>()
        .SetHandlerLifetime(TimeSpan.FromMinutes(5))  //Set lifetime to five minutes
        .AddPolicyHandler(GetRetryPolicy());
```

将策略添加至将要使用的 `HttpClient` 对象需要使用 AddPolicyHandler() 方法。 在此示例中，将使用指数退避算法为 Http 重试添加 Polly 策略。

若要获得更加模块化的方法，可在 `Startup.cs` 文件内的单独方法中定义 Http 重试策略，如以下代码所示：

```csharp
static IAsyncPolicy<HttpResponseMessage> GetRetryPolicy()
{
    return HttpPolicyExtensions
        .HandleTransientHttpError()
        .OrResult(msg => msg.StatusCode == System.Net.HttpStatusCode.NotFound)
        .WaitAndRetryAsync(6, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2,
                                                                    retryAttempt)));
}
```

使用 Polly 可定义一个重试策略，其中包含重试次数、指数退避算法配置以及在出现 HTTP 异常时要采取的操作，例如记录错误。 在此示例中，该策略配置为尝试 6 次（采用指数重试），以 2 秒为起始值。

## <a name="add-a-jitter-strategy-to-the-retry-policy"></a>将抖动策略添加到重试策略

在高并发率、高可伸缩性和高争用的情况下，常规重试策略可能会对系统产生影响。 在部分运行中断时，可能会有许多客户端同时发出相似的重试操作，从而形成操作高峰，为避免这种情况，一个好办法是向重试算法或策略中添加抖动策略。 该策略可以改进端到端系统的整体性能。 如 [Polly：重试与抖动](https://github.com/App-vNext/Polly/wiki/Retry-with-jitter)中所建议，一个好的抖动策略可以通过采用均匀分布的平滑重试间隔并在指数退避上应用一个控制良好的中值初始重试延迟来实现。 这种方法有助于在出现问题时分散峰值。 以下示例说明了这一原理：

```csharp

var delay = Backoff.DecorrelatedJitterBackoffV2(medianFirstRetryDelay: TimeSpan.FromSeconds(1), retryCount: 5);

var retryPolicy = Policy
    .Handle<FooException>()
    .WaitAndRetryAsync(delay);
```

## <a name="additional-resources"></a>其他资源

- **重试模式**  
  [https://docs.microsoft.com/azure/architecture/patterns/retry](/azure/architecture/patterns/retry)

- **Polly 和 IHttpClientFactory**  
  <https://github.com/App-vNext/Polly/wiki/Polly-and-HttpClientFactory>

- **Polly（.NET 的恢复和暂时性故障处理库）**  
  <https://github.com/App-vNext/Polly>

- **Polly：使用 Jitter 重试**  
  <https://github.com/App-vNext/Polly/wiki/Retry-with-jitter>

- **Marc Brooker。抖动：随机性使操作变得更好**  
  <https://brooker.co.za/blog/2015/03/21/backoff.html>

>[!div class="step-by-step"]
>[上一页](use-httpclientfactory-to-implement-resilient-http-requests.md)
>[下一页](implement-circuit-breaker-pattern.md)
