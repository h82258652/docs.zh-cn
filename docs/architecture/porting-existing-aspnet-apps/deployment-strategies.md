---
title: 部署策略
description: 从 ASP.NET 迁移到 ASP.NET Core 时，团队可以使用哪些部署策略？ 增量迁移能否实现 .NET Framework 和 .NET Core 应用的并行部署，提供无缝的最终用户体验？
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 4c69f049611e4995cdb1905c89afe6973994f20d
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122932"
---
# <a name="deployment-strategies"></a>部署策略

在计划将大型 ASP.NET 应用程序迁移到 ASP.NET Core 时，需要考虑的一个因素是部署新应用程序的方式。 对于 ASP.NET，部署选项仅限于 Windows 上的 IIS。 使用 ASP.NET Core，可以使用更多的部署选项。

## <a name="cross-platform-options"></a>跨平台选项

由于 .NET Core 是在 Linux 上运行的，因此你会发现一些可用的托管选项，这些选项并不是以前考虑的。 基于 Linux 的托管可能会出于以下原因：

* 你的组织有基础结构或专业知识。
* 宿主提供程序为基于 Linux 的托管提供了引人注目的功能或定价。

.NET Core 会打开这些选项的门。

## <a name="cloud-native-development"></a>云本机开发

如今，大多数组织都在使用云平台来实现其中至少一些软件功能。 将应用迁移到 .NET Core 后，应考虑是否应在有意中使用云托管来编写应用。 这种 *云本机* 应用可以更好地应用云功能（如无服务器、微服务），并且可以更少地关注如何以及在何处部署它们。

在此免费电子书中了解云本机应用开发的详细信息， [为 Azure 构建云本机 .Net 应用程序](../cloud-native/index.md)。

## <a name="leverage-containers"></a>利用容器

新式应用程序通常使用容器作为减小宿主环境之间的差异的手段。 将应用部署到特定容器后，无论容器托管应用是在开发人员的便携式计算机上运行还是在生产环境中运行，容器托管的应用都将运行。 作为迁移到 .NET Core 的一部分，考虑应用是否将通过容器从部署中受益，无论是完整的单体架构，还是分为多个较小的容器化应用。

## <a name="side-by-side-deployment-options"></a>并行部署选项

将大型 .NET Framework 应用迁移到 .NET Core 通常需要大量精力。 大多数组织都希望能够以某种方式中断这项工作，这样就可以在整个迁移完成之前，将应用程序的各个部分迁移并部署到生产环境中。 并行运行原始 ASP.NET 应用及其新迁移的 ASP.NET Core 子应用 (s) ，这是一个经常引用的目标。 可以通过多种机制实现此目的，包括使用 IIS 路由，如 [5 章](deployment-scenarios.md)所述。 其他选项包括利用应用网关或云设计模式（如 [后端 for 前端](/azure/architecture/patterns/backends-for-frontends) ）来管理 ASP.NET Web api 集和 ASP.NET Core API 终结点。

## <a name="iis-on-windows"></a>Windows 上的 IIS

你可以继续在 Windows 上运行的 IIS 上托管你的应用程序。 如果客户想要利用 ASP.NET Core 功能，而不更改其当前部署模型，则这是一个不错的选择。 在移动到 ASP.NET Core 提供更多有关部署应用程序的方式和位置的选项，因此不需要在 Windows 上使用 IIS 的已证明组合从现状上改变。

## <a name="other-options-on-windows"></a>Windows 上的其他选项

如果需要，你还可以使用 Kestrel、HTTP.sys 和 IIS 主机的任意组合，在 Windows 上托管应用程序并行应用。 如果你的应用程序需要 Windows 和 Linux 服务的组合，则在包含 [WSL](/windows/wsl/about) 和/或 linux Docker 容器的 Windows server 上托管将提供单一服务器解决方案，用于托管应用的所有部分。

## <a name="references"></a>参考

- [托管和部署 ASP.NET Core](/aspnet/core/host-and-deploy/)
- [使用 IIS 在 Windows 上托管 ASP.NET Core](/aspnet/core/host-and-deploy/iis/)
- [Azure App Service 和 IIS 上的 ASP.NET Core 疑难解答](/aspnet/core/test/troubleshoot-azure-iis)

>[!div class="step-by-step"]
>[上一页](migrate-web-forms.md)
>[下一页](additional-migration-resources.md)
