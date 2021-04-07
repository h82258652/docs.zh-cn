---
title: 托管 ASP.NET MVC 与 ASP.NET Core 之间的差异
description: ASP.NET MVC 应用的托管方式与 ASP.NET Core 应用之间的差异的概述。
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 9881a40403f8109fa63e25167b753ed4ce8ade17
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122893"
---
# <a name="hosting-differences-between-aspnet-mvc-and-aspnet-core"></a>托管 ASP.NET MVC 与 ASP.NET Core 之间的差异

ASP.NET MVC 应用程序托管在 IIS 中，并可能依赖 IIS 配置来实现其行为。 这通常包括 [IIS 模块](/iis/get-started/introduction-to-iis/iis-modules-overview)。 在评审应用程序以准备将其从 ASP.NET MVC 移植到 ASP.NET Core 时，团队应从其服务器上安装的 IIS 模块列表中确定要使用的模块（如果有）。

[ASP.NET Core 应用可以在多个不同的服务器上运行](/aspnet/core/fundamentals/servers/)。 默认的跨平台服务器 Kestrel 是一个很好的默认选项。 在 Kestrel 中运行的应用可由 IIS 托管，在单独的进程中运行。 在 Windows 上，应用程序也可以在 IIS 上的进程中运行，也可以使用 HTTP.sys 运行。 为了获得最佳性能，通常建议使用 Kestrel。 在应用向 Internet 公开且所需功能受 HTTP.sys（而不是 Kestrel）支持的方案中，可以使用 HTTP.sys。

Kestrel 没有与 IIS 模块等效 (但在 IIS 中托管的应用仍可利用 IIS 模块) 。 若要实现等效行为，通常使用在 ASP.NET Core 应用程序中配置的 [中间件](/aspnet/core/fundamentals/middleware/) 。

## <a name="references"></a>参考

- [IIS 模块](/iis/get-started/introduction-to-iis/iis-modules-overview)
- [ASP.NET Core 中间件](/aspnet/core/fundamentals/middleware/)
- [ASP.NET Core 服务器](/aspnet/core/fundamentals/servers/)

>[!div class="step-by-step"]
>[上一页](app-startup-differences.md)
>[下一页](serving-static-files.md)
