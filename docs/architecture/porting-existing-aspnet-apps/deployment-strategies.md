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
# <a name="deployment-strategies"></a><span data-ttu-id="d9941-104">部署策略</span><span class="sxs-lookup"><span data-stu-id="d9941-104">Deployment strategies</span></span>

<span data-ttu-id="d9941-105">在计划将大型 ASP.NET 应用程序迁移到 ASP.NET Core 时，需要考虑的一个因素是部署新应用程序的方式。</span><span class="sxs-lookup"><span data-stu-id="d9941-105">One consideration as you plan the migration of your large ASP.NET app to ASP.NET Core is how you'll deploy the new app.</span></span> <span data-ttu-id="d9941-106">对于 ASP.NET，部署选项仅限于 Windows 上的 IIS。</span><span class="sxs-lookup"><span data-stu-id="d9941-106">With ASP.NET, deployment options were limited to IIS on Windows.</span></span> <span data-ttu-id="d9941-107">使用 ASP.NET Core，可以使用更多的部署选项。</span><span class="sxs-lookup"><span data-stu-id="d9941-107">With ASP.NET Core, a much wider array of deployment options is available.</span></span>

## <a name="cross-platform-options"></a><span data-ttu-id="d9941-108">跨平台选项</span><span class="sxs-lookup"><span data-stu-id="d9941-108">Cross-platform options</span></span>

<span data-ttu-id="d9941-109">由于 .NET Core 是在 Linux 上运行的，因此你会发现一些可用的托管选项，这些选项并不是以前考虑的。</span><span class="sxs-lookup"><span data-stu-id="d9941-109">Because .NET Core runs on Linux, you'll find some hosting options available that weren't a consideration previously.</span></span> <span data-ttu-id="d9941-110">基于 Linux 的托管可能会出于以下原因：</span><span class="sxs-lookup"><span data-stu-id="d9941-110">Linux-based hosting may be preferable for the following reasons:</span></span>

* <span data-ttu-id="d9941-111">你的组织有基础结构或专业知识。</span><span class="sxs-lookup"><span data-stu-id="d9941-111">Your organization has infrastructure or expertise.</span></span>
* <span data-ttu-id="d9941-112">宿主提供程序为基于 Linux 的托管提供了引人注目的功能或定价。</span><span class="sxs-lookup"><span data-stu-id="d9941-112">Hosting providers offer attractive features or pricing for Linux-based hosting.</span></span>

<span data-ttu-id="d9941-113">.NET Core 会打开这些选项的门。</span><span class="sxs-lookup"><span data-stu-id="d9941-113">.NET Core opens the door to these options.</span></span>

## <a name="cloud-native-development"></a><span data-ttu-id="d9941-114">云本机开发</span><span class="sxs-lookup"><span data-stu-id="d9941-114">Cloud native development</span></span>

<span data-ttu-id="d9941-115">如今，大多数组织都在使用云平台来实现其中至少一些软件功能。</span><span class="sxs-lookup"><span data-stu-id="d9941-115">Most organizations today are using cloud platforms for at least some of their software capabilities.</span></span> <span data-ttu-id="d9941-116">将应用迁移到 .NET Core 后，应考虑是否应在有意中使用云托管来编写应用。</span><span class="sxs-lookup"><span data-stu-id="d9941-116">With an app migration to .NET Core, it's a good time to consider whether the app should be purposefully written with cloud hosting in mind.</span></span> <span data-ttu-id="d9941-117">这种 *云本机* 应用可以更好地应用云功能（如无服务器、微服务），并且可以更少地关注如何以及在何处部署它们。</span><span class="sxs-lookup"><span data-stu-id="d9941-117">Such *cloud native* apps are better able to apply cloud capabilities like serverless, microservices, and can be less concerned with the low-level details of how and where they may be deployed.</span></span>

<span data-ttu-id="d9941-118">在此免费电子书中了解云本机应用开发的详细信息， [为 Azure 构建云本机 .Net 应用程序](../cloud-native/index.md)。</span><span class="sxs-lookup"><span data-stu-id="d9941-118">Learn more about cloud native app development in this free e-book, [Architecting Cloud Native .NET Applications for Azure](../cloud-native/index.md).</span></span>

## <a name="leverage-containers"></a><span data-ttu-id="d9941-119">利用容器</span><span class="sxs-lookup"><span data-stu-id="d9941-119">Leverage containers</span></span>

<span data-ttu-id="d9941-120">新式应用程序通常使用容器作为减小宿主环境之间的差异的手段。</span><span class="sxs-lookup"><span data-stu-id="d9941-120">Modern apps often leverage containers as a means of reducing variation between hosting environments.</span></span> <span data-ttu-id="d9941-121">将应用部署到特定容器后，无论容器托管应用是在开发人员的便携式计算机上运行还是在生产环境中运行，容器托管的应用都将运行。</span><span class="sxs-lookup"><span data-stu-id="d9941-121">By deploying an app to a particular container, the container-hosted app will run the same whether it's running on a developer's laptop or in production.</span></span> <span data-ttu-id="d9941-122">作为迁移到 .NET Core 的一部分，考虑应用是否将通过容器从部署中受益，无论是完整的单体架构，还是分为多个较小的容器化应用。</span><span class="sxs-lookup"><span data-stu-id="d9941-122">As part of a migration to .NET Core, it may make sense to consider whether the app would benefit from deployment via container, either as a full monolith or broken up into multiple smaller containerized apps.</span></span>

## <a name="side-by-side-deployment-options"></a><span data-ttu-id="d9941-123">并行部署选项</span><span class="sxs-lookup"><span data-stu-id="d9941-123">Side-by-side deployment options</span></span>

<span data-ttu-id="d9941-124">将大型 .NET Framework 应用迁移到 .NET Core 通常需要大量精力。</span><span class="sxs-lookup"><span data-stu-id="d9941-124">Migrating large .NET Framework apps to .NET Core often requires a substantial effort.</span></span> <span data-ttu-id="d9941-125">大多数组织都希望能够以某种方式中断这项工作，这样就可以在整个迁移完成之前，将应用程序的各个部分迁移并部署到生产环境中。</span><span class="sxs-lookup"><span data-stu-id="d9941-125">Most organizations will want to be able to break this effort up in some fashion, so that pieces of the app can be migrated and deployed in production before the entire migration is complete.</span></span> <span data-ttu-id="d9941-126">并行运行原始 ASP.NET 应用及其新迁移的 ASP.NET Core 子应用 (s) ，这是一个经常引用的目标。</span><span class="sxs-lookup"><span data-stu-id="d9941-126">Running both the original ASP.NET app and its newly-migrated ASP.NET Core sub-app(s) side by side is a frequently cited goal.</span></span> <span data-ttu-id="d9941-127">可以通过多种机制实现此目的，包括使用 IIS 路由，如 [5 章](deployment-scenarios.md)所述。</span><span class="sxs-lookup"><span data-stu-id="d9941-127">This can be achieved through a number of mechanisms including leveraging IIS routing, which is covered in [chapter 5](deployment-scenarios.md).</span></span> <span data-ttu-id="d9941-128">其他选项包括利用应用网关或云设计模式（如 [后端 for 前端](/azure/architecture/patterns/backends-for-frontends) ）来管理 ASP.NET Web api 集和 ASP.NET Core API 终结点。</span><span class="sxs-lookup"><span data-stu-id="d9941-128">Other options include leveraging app gateways or cloud design patterns like [backends for frontends](/azure/architecture/patterns/backends-for-frontends) to manage sets of ASP.NET Web APIs and ASP.NET Core API endpoints.</span></span>

## <a name="iis-on-windows"></a><span data-ttu-id="d9941-129">Windows 上的 IIS</span><span class="sxs-lookup"><span data-stu-id="d9941-129">IIS on Windows</span></span>

<span data-ttu-id="d9941-130">你可以继续在 Windows 上运行的 IIS 上托管你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="d9941-130">You can continue hosting your apps on IIS running on Windows.</span></span> <span data-ttu-id="d9941-131">如果客户想要利用 ASP.NET Core 功能，而不更改其当前部署模型，则这是一个不错的选择。</span><span class="sxs-lookup"><span data-stu-id="d9941-131">This is a fine option for customers who want to take advantage of ASP.NET Core features without changing their current deployment model.</span></span> <span data-ttu-id="d9941-132">在移动到 ASP.NET Core 提供更多有关部署应用程序的方式和位置的选项，因此不需要在 Windows 上使用 IIS 的已证明组合从现状上改变。</span><span class="sxs-lookup"><span data-stu-id="d9941-132">While moving to ASP.NET Core provides more options in terms of how and where to deploy your apps, it doesn't require that you change from the status quo of using the proven combination of IIS on Windows.</span></span>

## <a name="other-options-on-windows"></a><span data-ttu-id="d9941-133">Windows 上的其他选项</span><span class="sxs-lookup"><span data-stu-id="d9941-133">Other options on Windows</span></span>

<span data-ttu-id="d9941-134">如果需要，你还可以使用 Kestrel、HTTP.sys 和 IIS 主机的任意组合，在 Windows 上托管应用程序并行应用。</span><span class="sxs-lookup"><span data-stu-id="d9941-134">You can host apps side-by-side apps on Windows using any combination of Kestrel, HTTP.sys, and IIS hosts, in addition to Docker containers, if needed.</span></span> <span data-ttu-id="d9941-135">如果你的应用程序需要 Windows 和 Linux 服务的组合，则在包含 [WSL](/windows/wsl/about) 和/或 linux Docker 容器的 Windows server 上托管将提供单一服务器解决方案，用于托管应用的所有部分。</span><span class="sxs-lookup"><span data-stu-id="d9941-135">If your app requires a combination of Windows and Linux services, hosting on a Windows server with [WSL](/windows/wsl/about) and/or Linux Docker containers provides a single server solution to hosting all parts of the app.</span></span>

## <a name="references"></a><span data-ttu-id="d9941-136">参考</span><span class="sxs-lookup"><span data-stu-id="d9941-136">References</span></span>

- [<span data-ttu-id="d9941-137">托管和部署 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9941-137">Host and deploy ASP.NET Core</span></span>](/aspnet/core/host-and-deploy/)
- [<span data-ttu-id="d9941-138">使用 IIS 在 Windows 上托管 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d9941-138">Host ASP.NET Core on Windows with IIS</span></span>](/aspnet/core/host-and-deploy/iis/)
- [<span data-ttu-id="d9941-139">Azure App Service 和 IIS 上的 ASP.NET Core 疑难解答</span><span class="sxs-lookup"><span data-stu-id="d9941-139">Troubleshooting ASP.NET Core on Azure App Service and IIS</span></span>](/aspnet/core/test/troubleshoot-azure-iis)

>[!div class="step-by-step"]
><span data-ttu-id="d9941-140">[上一页](migrate-web-forms.md)
>[下一页](additional-migration-resources.md)</span><span class="sxs-lookup"><span data-stu-id="d9941-140">[Previous](migrate-web-forms.md)
[Next](additional-migration-resources.md)</span></span>
