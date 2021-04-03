---
title: 在生产环境中运行时的迁移策略
description: 不能 tenable 将大型应用从 ASP.NET MVC 迁移到一次 ASP.NET Core。 了解将应用程序迁移到 ASP.NET Core，同时使其在现有用户的工作和生产环境中运行的策略。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 9b5d031165773c4da536bdc5478836a1b00436a1
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122724"
---
# <a name="strategies-for-migrating-while-running-in-production"></a>在生产环境中运行时的迁移策略

许多团队都有他们计划迁移到 .NET Core 的 .NET Framework 应用程序，但该应用程序非常大，需要很长时间才能完成迁移。 原始应用需要在一段时间完成迁移的情况下进行。 需要有一种方法来使应用程序的新旧版本和新版本协同工作，或者将旧版本迁移为就地迁移，而不需要进行任何分解。 团队可以使用多种不同的策略来支持这些目标。

## <a name="refactor-the-net-framework-solution"></a>重构 .NET Framework 解决方案

如果你打算将 .NET Framework 应用移植到 .NET Core，就是一个不错的开端。 这意味着更新单个类库以面向 .NET Standard，并将尽可能多的逻辑移出 ASP.NET MVC 项目并移入这些类库中。 .NET Standard 库中的任何代码都可立即从 .NET Framework 到 .NET Core 应用程序，这就是为什么此步骤在迁移过程中非常有用。

重构时，请确保遵循良好的重构基础知识。 例如，创建在开始重构之前验证系统功能的测试。 完成后，请运行这些测试，确认没有更改系统的行为。 如果还没有可以依赖的合适的自动测试，则可能需要向系统中添加特性测试。

## <a name="extract-front-end-assets-to-a-cdn"></a>将前端资产提取到 CDN

如果 .NET Framework 应用包含大量静态资产（如脚本、CSS 文件或映像），则可以将这些资源迁移到单独的 CDN。 然后，更新现有应用程序以引用这些资产的 CDN 链接。 当你将应用程序移植到 .NET Core 时，这些静态文件将不会成为迁移的一部分，你只需在 ASP.NET Core 应用的 CDN 中继续引用这些静态文件。

## <a name="extract-and-migrate-individual-microservices"></a>提取和迁移单个微服务

大型 .NET Framework 应用可能已经包含单独的可单独迁移的前端系统。 或者它们可能是迁移到微服务体系结构的候选项，其中一些现有的 ASP.NET MVC 应用程序将被拉入到新的 ASP.NET Core 微服务实现中。 可以在关联的电子书， [.Net 微服务：用于容器化 .Net 应用程序的体系结构](https://aka.ms/microservicesebook)中了解有关微服务的详细信息。

例如，现有应用程序可能有一组与用户登录和注册相关的功能。 这些可能会迁移到单独的微服务，可以使用 ASP.NET Core 生成和部署该，然后将其集成到旧 .NET Framework 应用中。 接下来，该应用程序可能有几个专用于跟踪单个用户的购物车的页面。 这些页面还可以提取到其自己的单独微服务中，并再次集成到现有应用。 通过这种方式，原始 .NET Framework 应用将继续在生产环境中运行，但其功能越来越多，来自现代化 .NET Core 微服务。

## <a name="deploy-multiple-versions-of-the-app-side-by-side-in-iis"></a>在 IIS 中并行部署应用的多个版本

使用主机标头和重定向的组合，可将现有的 ASP.NET MVC 应用配置为与同一 IIS 服务器上的 ASP.NET Core 应用并行运行。 由于单个控制器等功能段会移植到 ASP.NET Core，因此它们的路由和 Url 在 IIS 中映射以面向 ASP.NET Core 网站或子应用程序 (IIS 虚拟目录不受 ASP.NET Core 应用) 支持。 可将 ASP.NET Core 应用托管为 IIS 子应用程序（子应用）。 子应用的路径成为根应用 URL 的一部分。

## <a name="apply-the-strangler-pattern"></a>应用 Strangler 模式

通过增量迁移功能，可以将大型 ASP.NET MVC 应用逐渐替换为新的 ASP.NET Core 应用。 此方法的一种方法称为 [strangler 模式](/azure/architecture/patterns/strangler)，该模式名为 for strangler vines，它 strangle 并最终拆下树。 此方法依赖于在现有解决方案顶部实现外观层。 应使用新的问题方法或现成的解决方案（如 API 网关）构建此外观。

在外观准备就绪后，可以将部分部分路由到新的 ASP.NET Core 应用。 当你将更多的原始 .NET Framework 应用移植到 .NET Core 时，你将继续相应地更新外观层，将更多 façade's 的功能全部功能发送到新系统。 图3-5 显示了一段时间内 strangler 模式的进展情况。

![图3-5](media/Figure3-5.png)

**图3-5。** 一段时间内的 Strangler 模式。

最终，整个外观层对应于新式的新式实现。 此时，旧系统和人脸层都可以停用。

## <a name="multi-targeting-approaches"></a>多目标方法

对于面向 .NET Framework 的大型应用程序，可以通过对每个框架使用多目标和单独的代码路径，迁移到 ASP.NET Core 一段时间。 例如，在两个环境中都必须运行的代码可以使用[预 `#if` 处理器](../../csharp/language-reference/preprocessor-directives.md#conditional-compilation)指令进行修改，以实现不同的功能，或在 .NET Framework 与 .net Core 运行时使用不同的依赖项。 另一种方法是修改项目文件，使其包含不同的文件集，以供基于的框架为目标。 项目文件可使用不同的组合模式，如 `*.core.cs` ，根据目标框架包含不同的源文件集。 通常，对于将由多个 web 应用使用的库，只需要执行此方法。 对于 web 应用本身，最好具有两个单独的项目。

通过这些技术，可在添加新功能的同时，维护单个通用代码，并将应用的) 部分 (为使用 .NET Core。

## <a name="summary"></a>总结

通常，大型 ASP.NET MVC 和 Web API 应用将不会移植到 ASP.NET Core 一次，但会随着时间增量迁移。 本部分提供了执行此增量迁移的几个策略。 选择最适合你的组织和应用的)  (。

## <a name="references"></a>参考

- [.NET 微服务：容器化 .NET 应用程序的体系结构](https://aka.ms/microservicesebook)
- [eShopOnContainers Reference 微服务应用程序](https://github.com/dotnet-architecture/eShopOnContainers)
- [使用 IIS 在 Windows 上托管 ASP.NET Core](/aspnet/core/host-and-deploy/iis/)
- [绞杀者模式](/azure/architecture/patterns/strangler)

>[!div class="step-by-step"]
>[上一页](understand-update-dependencies.md)
>[下一页](example-migration-eshop.md)
