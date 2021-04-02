---
title: 迁移注意事项
description: 团队需要了解哪些内容才能正确决定是否以及如何从 ASP.NET MVC 迁移到 .NET Core？
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: c669dc477469c92dfa3acda8209ba7a1fdea5ed5
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122854"
---
# <a name="migration-considerations"></a>迁移注意事项

在将应用程序移植到 .NET Core 时，最基本的问题团队必须解决这一问题？ 在某些情况下，最好的方法是使用 ASP.NET MVC 和/或 Web API 继续 .NET Framework。 本章介绍了迁移到 .NET Core 的原因。 本章还考虑了方案和 counterpoints，以便保持 .NET Framework。

## <a name="is-migration-to-net-core-appropriate"></a>是否适合迁移到 .NET Core？

让我们从一些可能需要迁移到 .NET Core (或 .NET 5) 的原因着手。 还有很多内容，因此请不要将此列表详尽。

### <a name="cross-platform-support"></a>跨平台支持

基于 .NET Core 构建的应用程序确实跨平台，可以在 Windows、Linux 和 macOS 上运行。 你的开发人员不仅可以使用所需的任何硬件，还可以在任何位置托管你的应用程序。 示例从本地 IIS 到云中的 Azure 或从 Linux Docker 容器到 IoT 设备的范围。

### <a name="performance-and-scalability"></a>性能和可伸缩性

使用 .NET Core 构建的应用程序在 [任意位置可用的最快技术堆栈](https://www.techempower.com/benchmarks/#hw=ph&test=plaintext)上运行。 ASP.NET MVC 应用程序通常 ASP.NET Core 上的性能改进，特别是在更新后，才能利用 .NET Core 中提供的一些新功能。

### <a name="cloud-native"></a>云原生

出于上述原因和其他原因，.NET Core 应用非常适合在云托管环境中运行。 轻型和 fast，可以将 .NET Core 应用部署到 Azure 应用服务或容器，并根据需要水平缩放以满足即时系统需求。

### <a name="maintainable"></a>容易

对于许多应用程序，尽管他们一直在满足客户和业务需求，但技术债务已积累并维护应用程序的成本高昂。 ASP.NET Core 应用比 ASP.NET MVC 应用更易于测试，使其更易于重构和扩展。

### <a name="modular"></a>模块化

ASP.NET Core 是模块化的，使用 NuGet 包作为框架的第一类部分。 为 .NET Core 构建的应用程序都支持依赖项注入，从而可以轻松地从给定环境中所需的任何实现编写解决方案。 利用 .NET Core 生成微服务比在 IIS 上依赖 ASP.NET MVC 更容易，后者会打开其他选项，将大型应用分解为较小的模块。

### <a name="modern"></a>新式

一直在一种新式、积极发展的技术堆栈上，都有一种优势。 新功能和 c # 语言功能将仅添加到 .NET Core。 .NET Framework 的最新发布版本4.8，并且 c # 版本以外的 c # 不是以 .NET Framework 为目标。 尽管 ASP.NET MVC 将在很多年内仍受支持，但最优秀的 .NET 软件开发人员可能希望使用更先进的 .NET Core framework，它提供的所有优点 (仅) 上面的部分。 查找具有维护 ASP.NET MVC 应用的技能的开发人员在某个时间点将开始成为一项挑战，因为这将查找在线培训和故障排除帮助。 例如，可能不会对 ASP.NET MVC 5 撰写许多新的博客文章，而是为 .NET 5.0 编写大量的博客文章。

考虑迁移到 .NET Core 有许多有说服力的原因，这可能是您阅读本书的原因！ 但让我们考虑一些缺点和原因，原因可能是更有可能保持 .NET Framework。

## <a name="when-is-net-framework-appropriate"></a>何时 .NET Framework 适用的情况？

继续 .NET Framework 的最大原因是，应用程序不处于活动状态，并且不会从上面列出的优势中获益。 在这种情况下，可能不是一个很好的业务案例来移植应用程序的成本。 如果你的应用程序可能会受益于 .NET Core 产品/服务的优点，但如果你需要某些在 .NET Core 中不可用的技术，你可能仍需要继续 .NET Framework。 .NET Core 提供了一些 [.net 技术](../../core/porting/net-framework-tech-unavailable.md)，其中包括 Appdomain、远程处理、代码访问安全性 (CAS) 、安全透明度和 `System.EnterpriseServices` 。 此处提供了这些技术及其替代项的简要概述。 有关更详细的指南，请参阅文档。

### <a name="application-domains"></a>应用程序域

应用程序域 (AppDomain) 可将应用相互隔离。 Appdomain 需要运行时支持，而且可能会占用大量资源。 不支持创建更多的应用程序域，而且将来不会将此功能添加到 .NET Core。 对于代码隔离，将流程或容器用作备用。 一些客户使用 Appdomain 作为卸载程序集的一种方法。 在 .NET Core 中， [AssemblyLoadContext](https://docs.microsoft.com/dotnet/standard/assembly/unloadability) 提供了一种替代方法来卸载程序集。

### <a name="wcf"></a>WCF

.NET Core 不支持服务器端 WCF。 .NET Core 支持 WCF 客户端，但不支持 WCF 主机。 需要此功能的应用将需要升级到不同的通信技术 (例如，作为迁移的一部分的 gRPC 或 REST) 。

[.Net Foundation 中提供了一个 WCF 客户端端口](../../core/dotnet-five.md#windows-communication-foundation)。 它完全是开放源代码、跨平台并受 Microsoft 支持。 还有一个 *不* 受 Microsoft 正式支持的社区支持的 [CoreWCF 项目](https://github.com/CoreWCF/CoreWCF)。

若要了解有关从 WCF 迁移到 gRPC 的详细信息，请参阅 [WCF 开发人员的 gRPC](https://docs.microsoft.com/dotnet/architecture/grpc-for-wcf-developers/) 电子书。

### <a name="remoting"></a>远程处理

.NET 远程处理被认为是存在问题的体系结构。 它用于进行跨 AppDomain 的通信，该通信现已不再受支持。 同样，远程处理也需要运行时支持，进行维护的成本较高。 由于这些原因，.net Core 不支持 .NET 远程处理，并且产品团队不打算在将来添加对它的支持。 可以使用几种备选消息传递策略和实现来替换 .NET Core 应用中的远程处理。

### <a name="code-access-security-cas-and-security-transparency"></a> (CA) 和安全透明度的代码访问安全性

.NET Core 不支持这两种技术。 相反，建议使用操作系统提供的安全边界。 例如，虚拟化、容器或用户帐户。 用所需的最小特权集运行进程。

## <a name="references"></a>参考

[.NET Core 中不可用的 .NET Framework 技术](../../core/porting/net-framework-tech-unavailable.md)

>[!div class="step-by-step"]
>[上一页](introduction.md)
>[下一页](migrate-aspnet-core-2-1.md)
