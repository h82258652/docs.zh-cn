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
# <a name="hosting-differences-between-aspnet-mvc-and-aspnet-core"></a><span data-ttu-id="6b340-103">托管 ASP.NET MVC 与 ASP.NET Core 之间的差异</span><span class="sxs-lookup"><span data-stu-id="6b340-103">Hosting differences between ASP.NET MVC and ASP.NET Core</span></span>

<span data-ttu-id="6b340-104">ASP.NET MVC 应用程序托管在 IIS 中，并可能依赖 IIS 配置来实现其行为。</span><span class="sxs-lookup"><span data-stu-id="6b340-104">ASP.NET MVC apps are hosted in IIS, and may rely on IIS configuration for their behavior.</span></span> <span data-ttu-id="6b340-105">这通常包括 [IIS 模块](/iis/get-started/introduction-to-iis/iis-modules-overview)。</span><span class="sxs-lookup"><span data-stu-id="6b340-105">This often includes [IIS modules](/iis/get-started/introduction-to-iis/iis-modules-overview).</span></span> <span data-ttu-id="6b340-106">在评审应用程序以准备将其从 ASP.NET MVC 移植到 ASP.NET Core 时，团队应从其服务器上安装的 IIS 模块列表中确定要使用的模块（如果有）。</span><span class="sxs-lookup"><span data-stu-id="6b340-106">As part of reviewing an app to prepare to port it from ASP.NET MVC to ASP.NET Core, teams should identify which modules, if any, they're using from the list of IIS Modules installed on their server.</span></span>

<span data-ttu-id="6b340-107">[ASP.NET Core 应用可以在多个不同的服务器上运行](/aspnet/core/fundamentals/servers/)。</span><span class="sxs-lookup"><span data-stu-id="6b340-107">[ASP.NET Core apps can run on a number of different servers](/aspnet/core/fundamentals/servers/).</span></span> <span data-ttu-id="6b340-108">默认的跨平台服务器 Kestrel 是一个很好的默认选项。</span><span class="sxs-lookup"><span data-stu-id="6b340-108">The default cross platform server, Kestrel, is a good default choice.</span></span> <span data-ttu-id="6b340-109">在 Kestrel 中运行的应用可由 IIS 托管，在单独的进程中运行。</span><span class="sxs-lookup"><span data-stu-id="6b340-109">Apps running in Kestrel can be hosted by IIS, running in a separate process.</span></span> <span data-ttu-id="6b340-110">在 Windows 上，应用程序也可以在 IIS 上的进程中运行，也可以使用 HTTP.sys 运行。</span><span class="sxs-lookup"><span data-stu-id="6b340-110">On Windows, apps can also run in process on IIS or using HTTP.sys.</span></span> <span data-ttu-id="6b340-111">为了获得最佳性能，通常建议使用 Kestrel。</span><span class="sxs-lookup"><span data-stu-id="6b340-111">Kestrel is generally recommended for best performance.</span></span> <span data-ttu-id="6b340-112">在应用向 Internet 公开且所需功能受 HTTP.sys（而不是 Kestrel）支持的方案中，可以使用 HTTP.sys。</span><span class="sxs-lookup"><span data-stu-id="6b340-112">HTTP.sys can be used in scenarios where the app is exposed to the Internet and required capabilities are supported by HTTP.sys but not Kestrel.</span></span>

<span data-ttu-id="6b340-113">Kestrel 没有与 IIS 模块等效 (但在 IIS 中托管的应用仍可利用 IIS 模块) 。</span><span class="sxs-lookup"><span data-stu-id="6b340-113">Kestrel does not have an equivalent to IIS modules (though apps hosted in IIS can still take advantage of IIS modules).</span></span> <span data-ttu-id="6b340-114">若要实现等效行为，通常使用在 ASP.NET Core 应用程序中配置的 [中间件](/aspnet/core/fundamentals/middleware/) 。</span><span class="sxs-lookup"><span data-stu-id="6b340-114">To achieve equivalent behavior, [middleware](/aspnet/core/fundamentals/middleware/) configured in the ASP.NET Core app itself is typically used.</span></span>

## <a name="references"></a><span data-ttu-id="6b340-115">参考</span><span class="sxs-lookup"><span data-stu-id="6b340-115">References</span></span>

- [<span data-ttu-id="6b340-116">IIS 模块</span><span class="sxs-lookup"><span data-stu-id="6b340-116">IIS Modules</span></span>](/iis/get-started/introduction-to-iis/iis-modules-overview)
- [<span data-ttu-id="6b340-117">ASP.NET Core 中间件</span><span class="sxs-lookup"><span data-stu-id="6b340-117">ASP.NET Core Middleware</span></span>](/aspnet/core/fundamentals/middleware/)
- [<span data-ttu-id="6b340-118">ASP.NET Core 服务器</span><span class="sxs-lookup"><span data-stu-id="6b340-118">ASP.NET Core Servers</span></span>](/aspnet/core/fundamentals/servers/)

>[!div class="step-by-step"]
><span data-ttu-id="6b340-119">[上一页](app-startup-differences.md)
>[下一页](serving-static-files.md)</span><span class="sxs-lookup"><span data-stu-id="6b340-119">[Previous](app-startup-differences.md)
[Next](serving-static-files.md)</span></span>
