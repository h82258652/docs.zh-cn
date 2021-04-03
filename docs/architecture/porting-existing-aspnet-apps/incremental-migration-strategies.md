---
title: 增量迁移策略
description: 团队可以采用哪些战略，使他们能够以增量方式将大型应用从 ASP.NET MVC 迁移到 .NET Core？
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 615d2a853b698bfc752c3baf36f31ab2a4f2bab8
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122880"
---
# <a name="strategies-for-migrating-incrementally"></a>增量迁移策略

迁移任何大型应用程序的最大挑战是确定如何将该过程分解为更小的任务。 可以应用多个策略来实现此目的，包括将应用分解为水平层，如 UI、数据访问、业务逻辑，或将应用分解为单独的小型应用。 另一种策略是将应用程序的部分或全部升级到最新的 .NET Core 版本。 您可以使用的一种方法是迁移应用程序的 [垂直切片](https://deviq.com/practices/vertical-slices) ，而不是尝试一次迁移一个水平层。 让我们考虑一下其中每个不同的方法。

## <a name="migrating-slice-by-slice"></a>按切片迁移切片

一种成功的迁移方法是识别功能的垂直切片，并逐个地将其迁移到目标平台。 第一步是创建新的 ASP.NET Core 3.1 或5应用。 接下来，确定将首先迁移的单个页面或 API 终结点。 仅构建必要的功能，以便在 ASP.NET Core 应用程序中支持这一路由。 然后，使用 HTTP 重写和/或反向代理开始向新应用程序（而不是 ASP.NET 应用程序）发送这些页或终结点的请求。 此方法非常适合 API 项目，但也适用于许多 MVC 应用。

在按切片迁移切片时，会在新项目或解决方案中重新创建单个 API 终结点或请求的路由的整个堆栈。 首先，此类切片通常需要完成的工作量最大，因为通常需要创建多个项目，并做出有关数据访问和解决方案组织的决策。 第一扇区的功能反映了现有应用的功能之后，就可以对其进行部署，现有应用可以重定向到该应用，也可以直接将其删除。 此方法随后会重复出现，直到整个应用都移植到新的结构。

有关如何使用 IIS 遵循此策略的一些具体指导 [，请参阅部署方案第5章](deployment-scenarios.md)。

## <a name="migrating-layer-by-layer"></a>按层迁移层

考虑迁移大型 ASP.NET 4.5 应用的挑战。 一种方法是将整个应用直接从 .NET Framework 4.5 迁移到 .NET Core 3.1。 但是，这种方法需要考虑两个框架和版本之间的每个重大更改，这一点很重要。 一次在一个项目上执行此操作提供一组步进石子，使整个解决方案不需要同时移动。

对 .NET 生态系统的最新补充，有助于在不同的 .NET [.NET Standard](https://dotnet.microsoft.com/platform/dotnet-standard)框架之间实现互操作性。 .NET Standard 允许根据一组标准 Api 构建库，确保它们可在任何 .NET 应用中使用。 .NET Standard 2.0 很明显，因为它涵盖大多数 .NET Framework 和 .NET Core 应用使用的大多数基类库功能。 遗憾的是，支持 .NET Standard 2.0 的最早版本的 .NET .NET Framework 4.6.1，在 .NET Framework 4.8 中有大量更新，这使其成为初次升级的极具吸引力的选择。

逐步升级 .NET Framework 4.5 系统层的一种方法是先将其类库依赖项更新为 .NET Framework 4.8。 然后，将这些库修改为 .NET Standard 类库。 如有必要，请使用多目标编译和条件编译。 在应用程序依赖项需要 .NET Framework 且不能轻松直接移植到使用 .NET Standard 和 .NET Core 的方案中，此步骤非常有用。 由于 ASP.NET Core 2.1 应用可以使用 .NET Framework 库，因此下一步是将应用程序的部分或全部 web 功能迁移到 ASP.NET Core 2.1 (，如 [前一章](choose-net-core-version.md)) 所述。 这是一个 "自下而上的" 方法，从低级别类库依赖项开始，到 web 应用入口点工作。

在 ASP.NET Core 2.1 上运行应用程序后，将其迁移到 ASP.NET Core 3.1 隔离相对比较简单。 此步骤中最可能的难题是更新不兼容的依赖项，以支持 .NET Core 和可能更高版本的 .NET Standard。 对于不依赖于 .NET Framework 库的依赖项的应用程序，无需升级到 ASP.NET Core 2.1。 直接迁移到 ASP.NET Core 3.1 更有意义，需要更少的工作量。

当应用程序在 .NET Core 3.1 上运行时，迁移到当前的 .NET 5 版本相对较为轻松。 此过程主要涉及更新项目文件的目标框架及其关联的 NuGet 包依赖项。 尽管有几项 [重大更改需要考虑](../../core/compatibility/5.0.md)，但大多数应用程序不需要对从 .net Core 3.1 到 .net 5 的重大修改。 在 [.Net Core 3.1 和 .net 5 之间进行选择](choose-net-core-version.md)的主要决定因素可能是支持。

作为另一种替代方法，另一种替代方法是从 web 应用 (甚至整个解决方案开始) 并使用自动化工具来帮助进行升级。 [.Net 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)可用于帮助将 .NET Framework 应用升级到 .net Core/.net 5。 它自动执行许多与升级应用相关的常见任务，例如修改项目文件格式、设置适当的目标框架、更新 NuGet 依赖关系等。

作为另一种替代方法，另一种替代方法是从 web 应用 (甚至整个解决方案开始) 并使用自动化工具来帮助进行升级。 [.Net 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)可用于帮助将 .NET Framework 应用升级到 .net Core/.net 5。 它自动执行许多与升级应用相关的常见任务，例如修改项目文件格式、设置适当的目标框架、更新 NuGet 依赖关系等。

## <a name="references"></a>参考

- [什么是 .NET Standard？](https://dotnet.microsoft.com/platform/dotnet-standard)
- [.NET 5 简介](https://devblogs.microsoft.com/dotnet/introducing-net-5/)
- [从 ASP.NET Core 3.1 迁移到5。0](/aspnet/core/migration/31-to-50)
- [.NET 升级助手工具](https://aka.ms/dotnet-upgrade-assistant)

>[!div class="step-by-step"]
>[上一页](choose-net-core-version.md)
>[下一页](migrate-web-forms.md)
