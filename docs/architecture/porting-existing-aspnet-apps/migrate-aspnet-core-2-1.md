---
title: 迁移到 ASP.NET Core 2.1
description: 作为支持 .NET Framework 运行时目标的最新 .NET Core 版本，迁移到 .NET Core 2.1 对于某些应用迁移计划而言是一个中间步骤吗？
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: e4db85405edc7e7f51ab6f8b422451cf7acc065e
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122841"
---
# <a name="migrate-to-aspnet-core-21"></a>迁移到 ASP.NET Core 2.1

ASP.NET Core 2.1 是一个有趣的版本，因为它是当前支持的 ASP.NET Core 版本，它支持 .NET Core 和 .NET Framework 运行时。 同样，与将应用程序的所有部分升级到 .NET Core 相比，它可以为某些应用提供更简单的升级途径。 作为 LTS 版本，对 .NET Core 2.1 的支持将持续到8月2021。 对于在 .NET Framework 上运行的 ASP.NET Core 2.1 的支持，只要支持其基础 .NET Framework，就会继续。

## <a name="should-apps-run-on-net-framework-with-aspnet-core-21"></a>应用是否应在 ASP.NET Core 2.1 的 .NET Framework 上运行

ASP.NET Core 2.2 及更早版本支持 .NET Core 和 .NET Framework 运行时。 在完全移植到 .NET Core 之前，将部分或全部应用程序迁移到 ASP.NET Core 2.1 作为步进石是有意义的吗？ 应用程序或应用程序的子集可以看到其前端 ASP.NET 逻辑 .NET Framework 使用 ASP.NET Core，同时仍然使用库来实现业务逻辑和基础结构使用。 此方法在以下情况下可能非常有用：有一个相对较薄的 UI 层没有多个业务逻辑，而类库中有一组更大的功能。

仅将前端 web 层移植到 ASP.NET Core 2.1 的主要优点是，在初始迁移期间，现有的 .NET 类库可以保持原样。 它们可能是其他 .NET 应用程序继续使用的，或者不需要在计划的完整迁移到 .NET Core 的第一次迭代范围内。 减小大型应用的初始迁移范围有助于提供以步进方式进入所需结束状态的增量目标，这通常是 .NET Core 的完整端口。

如果你有一个可能使用此策略的现有应用程序，你现在可以执行的一些操作是将业务逻辑、数据访问和其他非 UI 逻辑从 ASP.NET 项目移出，并将其移到单独的类库中。 如果你对系统进行了自动测试，使你能够在迁移之前和之后验证行为是否保持一致，此方法也会有所帮助。

如果你的应用程序太大，无法同时迁移整个 web 应用，并且你需要能够将新的 ASP.NET Core 应用程序并行部署到现有的 ASP.NET 应用程序中，则可以使用部署策略来实现此目的。 [第5章：部署方案](deployment-scenarios.md)中对这些内容进行了介绍。

请记住，ASP.NET Core 2.1 是 .NET Core 的最后一个 LTS 版本，它支持在 .NET Framework 和使用 .NET Framework 库上运行。 此版本不久将会受支持，但只要 .NET Framework (，即使在 .NET Core 2.1 支持) 结束后 ASP.NET Core 2.1 .NET Framework 也将受支持。 有关详细信息，请参阅 [.NET Framework 上的 ASP.NET Core 2.1](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。

## <a name="references"></a>参考

[从 ASP.NET 迁移到 ASP.NET Core 2。1](/aspnet/core/migration/proper-to-2x/?preserve-view=true&view=aspnetcore-2.1)

>[!div class="step-by-step"]
>[上一页](migration-considerations.md)
>[下一页](choose-net-core-version.md)
