---
title: 面向 ASP.NET Core 开发人员的 DevOps
author: CamSoper
description: 提供有关为托管在 Azure 中的 ASP.NET Core 应用构建 DevOps 管道的端到端指导的指南。
ms.author: casoper
ms.date: 04/13/2021
ms.custom: devx-track-csharp, mvc, seodec18
no-loc:
- appsettings.json
- ASP.NET Core Identity
- cookie
- Cookie
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: azure/devops/index
ms.openlocfilehash: 5052c16e637260ede3a81578ba2280f760d086c4
ms.sourcegitcommit: 040b745ac19e4a5d23df17422ab30e72839f5754
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/21/2021
ms.locfileid: "107810509"
---
# <a name="devops-for-aspnet-core-developers"></a><span data-ttu-id="119cd-103">面向 ASP.NET Core 开发人员的 DevOps</span><span class="sxs-lookup"><span data-stu-id="119cd-103">DevOps for ASP.NET Core Developers</span></span>

<span data-ttu-id="119cd-104">[![封面图像](./media/cover-large.png)](https://aka.ms/devopsbook)</span><span class="sxs-lookup"><span data-stu-id="119cd-104">[![Cover Image](./media/cover-large.png)](https://aka.ms/devopsbook)</span></span>

<span data-ttu-id="119cd-105">**EDITION v1.1.0**</span><span class="sxs-lookup"><span data-stu-id="119cd-105">**EDITION v1.1.0**</span></span>

<span data-ttu-id="119cd-106">请参阅[更改记录](https://aka.ms/devops-ebook-changelog)了解书籍更新和社区贡献。</span><span class="sxs-lookup"><span data-stu-id="119cd-106">Refer [changelog](https://aka.ms/devops-ebook-changelog) for the book updates and community contributions.</span></span>

<span data-ttu-id="119cd-107">本指南以[可下载 PDF 电子书](https://aka.ms/devopsbook)的形式提供。</span><span class="sxs-lookup"><span data-stu-id="119cd-107">This guide is available as a [downloadable PDF e-book](https://aka.ms/devopsbook).</span></span>

<span data-ttu-id="119cd-108">发布者</span><span class="sxs-lookup"><span data-stu-id="119cd-108">PUBLISHED BY</span></span>

<span data-ttu-id="119cd-109">Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队</span><span class="sxs-lookup"><span data-stu-id="119cd-109">Microsoft Developer Division, .NET, and Visual Studio product teams</span></span>

<span data-ttu-id="119cd-110">Microsoft Corporation 的一个部门</span><span class="sxs-lookup"><span data-stu-id="119cd-110">A division of Microsoft Corporation</span></span>

<span data-ttu-id="119cd-111">One Microsoft Way</span><span class="sxs-lookup"><span data-stu-id="119cd-111">One Microsoft Way</span></span>

<span data-ttu-id="119cd-112">Redmond, Washington 98052-6399</span><span class="sxs-lookup"><span data-stu-id="119cd-112">Redmond, Washington 98052-6399</span></span>

<span data-ttu-id="119cd-113">版权所有 &copy; 2021 Microsoft Corporation</span><span class="sxs-lookup"><span data-stu-id="119cd-113">Copyright &copy; 2021 by Microsoft Corporation</span></span>

<span data-ttu-id="119cd-114">保留所有权利。</span><span class="sxs-lookup"><span data-stu-id="119cd-114">All rights reserved.</span></span> <span data-ttu-id="119cd-115">未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。</span><span class="sxs-lookup"><span data-stu-id="119cd-115">No part of the contents of this book may be reproduced or transmitted in any form or by any means without the written permission of the publisher.</span></span>

<span data-ttu-id="119cd-116">本书“按原样”提供，表达作者的观点和看法。</span><span class="sxs-lookup"><span data-stu-id="119cd-116">This book is provided "as-is" and expresses the author's views and opinions.</span></span> <span data-ttu-id="119cd-117">本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。</span><span class="sxs-lookup"><span data-stu-id="119cd-117">The views, opinions, and information expressed in this book, including URL and other Internet website references, may change without notice.</span></span>

<span data-ttu-id="119cd-118">本书中提及的一些示例仅用于说明，纯属虚构。</span><span class="sxs-lookup"><span data-stu-id="119cd-118">Some examples depicted herein are provided for illustration only and are fictitious.</span></span> <span data-ttu-id="119cd-119">不存在任何实际关联或联系，请勿妄加推断。</span><span class="sxs-lookup"><span data-stu-id="119cd-119">No real association or connection is intended or should be inferred.</span></span>

<span data-ttu-id="119cd-120">Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。</span><span class="sxs-lookup"><span data-stu-id="119cd-120">Microsoft and the trademarks listed at <https://www.microsoft.com> on the "Trademarks" webpage are trademarks of the Microsoft group of companies.</span></span>

<span data-ttu-id="119cd-121">Mac 和 macOS 是 Apple Inc. 的商标</span><span class="sxs-lookup"><span data-stu-id="119cd-121">Mac and macOS are trademarks of Apple Inc.</span></span>

<span data-ttu-id="119cd-122">Docker 的鲸鱼徽标是 Docker Inc. 的注册商标经许可方可使用。</span><span class="sxs-lookup"><span data-stu-id="119cd-122">The Docker whale logo is a registered trademark of Docker, Inc. Used by permission.</span></span>

<span data-ttu-id="119cd-123">所有其他标记和徽标均为其各自所有者的财产。</span><span class="sxs-lookup"><span data-stu-id="119cd-123">All other marks and logos are property of their respective owners.</span></span>

## <a name="credits"></a><span data-ttu-id="119cd-124">致谢</span><span class="sxs-lookup"><span data-stu-id="119cd-124">Credits</span></span>

<span data-ttu-id="119cd-125">作者：</span><span class="sxs-lookup"><span data-stu-id="119cd-125">Authors:</span></span>

> [<span data-ttu-id="119cd-126">Cam Soper</span><span class="sxs-lookup"><span data-stu-id="119cd-126">Cam Soper</span></span>](https://twitter.com/camsoper)
>
> [<span data-ttu-id="119cd-127">Scott Addie</span><span class="sxs-lookup"><span data-stu-id="119cd-127">Scott Addie</span></span>](https://twitter.com/scottaddie)
>
> [<span data-ttu-id="119cd-128">Colin Dembovsky</span><span class="sxs-lookup"><span data-stu-id="119cd-128">Colin Dembovsky</span></span>](https://twitter.com/colindembovsky)

## <a name="welcome"></a><span data-ttu-id="119cd-129">欢迎使用</span><span class="sxs-lookup"><span data-stu-id="119cd-129">Welcome</span></span>

<span data-ttu-id="119cd-130">欢迎使用适用于 .NET 的 Azure 开发生命周期指南！</span><span class="sxs-lookup"><span data-stu-id="119cd-130">Welcome to the Azure Development Lifecycle guide for .NET!</span></span> <span data-ttu-id="119cd-131">本指南介绍使用 .NET 工具和进程围绕 Azure 构建开发生命周期的基本概念。</span><span class="sxs-lookup"><span data-stu-id="119cd-131">This guide introduces the basic concepts of building a development lifecycle around Azure using .NET tools and processes.</span></span> <span data-ttu-id="119cd-132">读完本指南后，可以获得成熟 DevOps 工具链的优势。</span><span class="sxs-lookup"><span data-stu-id="119cd-132">After finishing this guide, you'll reap the benefits of a mature DevOps toolchain.</span></span>

## <a name="who-this-guide-is-for"></a><span data-ttu-id="119cd-133">本指南适用对象</span><span class="sxs-lookup"><span data-stu-id="119cd-133">Who this guide is for</span></span>

<span data-ttu-id="119cd-134">读者应是有经验的 ASP.NET Core 开发人员（200-300 级别）。</span><span class="sxs-lookup"><span data-stu-id="119cd-134">You should be an experienced ASP.NET Core developer (200-300 level).</span></span> <span data-ttu-id="119cd-135">无须了解有关 Azure 的任何内容，因为本指南中将进行介绍。</span><span class="sxs-lookup"><span data-stu-id="119cd-135">You don't need to know anything about Azure, as we'll cover that in this introduction.</span></span> <span data-ttu-id="119cd-136">本指南对于更侧重于操作（而不是开发）的 DevOps 工程师可能也很有用。</span><span class="sxs-lookup"><span data-stu-id="119cd-136">This guide may also be useful for DevOps engineers who are more focused on operations than development.</span></span>

<span data-ttu-id="119cd-137">本指南面向 Windows 开发人员。</span><span class="sxs-lookup"><span data-stu-id="119cd-137">This guide targets Windows developers.</span></span> <span data-ttu-id="119cd-138">但是，.NET Core 完全支持 Linux 和 macOS。</span><span class="sxs-lookup"><span data-stu-id="119cd-138">However, Linux and macOS are fully supported by .NET Core.</span></span> <span data-ttu-id="119cd-139">若要针对 Linux/macOS 调整本指南，请注意有关 Linux/macOS 差异的标注。</span><span class="sxs-lookup"><span data-stu-id="119cd-139">To adapt this guide for Linux/macOS, watch for callouts for Linux/macOS differences.</span></span>

## <a name="what-this-guide-doesnt-cover"></a><span data-ttu-id="119cd-140">本指南未涵盖的内容</span><span class="sxs-lookup"><span data-stu-id="119cd-140">What this guide doesn't cover</span></span>

<span data-ttu-id="119cd-141">本指南专注于 .NET 开发人员的端到端持续部署体验。</span><span class="sxs-lookup"><span data-stu-id="119cd-141">This guide is focused on an end-to-end continuous deployment experience for .NET developers.</span></span> <span data-ttu-id="119cd-142">这不是介绍 Azure 中所有内容的全面指南，并不特别着重于适用于 Azure 服务的 .NET API。</span><span class="sxs-lookup"><span data-stu-id="119cd-142">It's not an exhaustive guide to all things Azure, and it doesn't focus extensively on .NET APIs for Azure services.</span></span> <span data-ttu-id="119cd-143">介绍的重点在于持续集成、部署、监视和调试。</span><span class="sxs-lookup"><span data-stu-id="119cd-143">The emphasis is all around continuous integration, deployment, monitoring, and debugging.</span></span> <span data-ttu-id="119cd-144">本指南结尾附近提供了后续步骤建议。</span><span class="sxs-lookup"><span data-stu-id="119cd-144">Near the end of the guide, recommendations for next steps are offered.</span></span> <span data-ttu-id="119cd-145">建议包括对 ASP.NET Core 开发人员有用的 Azure 平台服务。</span><span class="sxs-lookup"><span data-stu-id="119cd-145">Included in the suggestions are Azure platform services that are useful to ASP.NET Core developers.</span></span>

## <a name="whats-in-this-guide"></a><span data-ttu-id="119cd-146">本指南内容</span><span class="sxs-lookup"><span data-stu-id="119cd-146">What's in this guide</span></span>

### <a name="tools-and-downloads"></a>[<span data-ttu-id="119cd-147">工具和下载</span><span class="sxs-lookup"><span data-stu-id="119cd-147">Tools and downloads</span></span>](xref:azure/devops/tools-and-downloads)

<span data-ttu-id="119cd-148">了解在何处获取本指南中使用的工具。</span><span class="sxs-lookup"><span data-stu-id="119cd-148">Learn where to acquire the tools used in this guide.</span></span>

### <a name="deploy-to-app-service"></a>[<span data-ttu-id="119cd-149">部署到应用服务</span><span class="sxs-lookup"><span data-stu-id="119cd-149">Deploy to App Service</span></span>](xref:azure/devops/deploy-to-app-service)

<span data-ttu-id="119cd-150">了解将 ASP.NET Core 应用部署到 Azure 应用服务的各种方法。</span><span class="sxs-lookup"><span data-stu-id="119cd-150">Learn the various methods for deploying an ASP.NET Core app to Azure App Service.</span></span>

### <a name="continuous-integration-and-deployment-with-azure-devops"></a>[<span data-ttu-id="119cd-151">使用 Azure DevOps 持续集成和持续部署</span><span class="sxs-lookup"><span data-stu-id="119cd-151">Continuous integration and deployment with Azure DevOps</span></span>](xref:azure/devops/cicd)

<span data-ttu-id="119cd-152">通过 GitHub、Azure DevOps Services 和 Azure，为 ASP.NET Core 应用生成端到端持续集成和部署解决方案。</span><span class="sxs-lookup"><span data-stu-id="119cd-152">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, Azure DevOps Services, and Azure.</span></span>

### <a name="continuous-integration-and-deployment-with-github-actions"></a>[<span data-ttu-id="119cd-153">使用 GitHub Actions 持续集成和持续部署</span><span class="sxs-lookup"><span data-stu-id="119cd-153">Continuous integration and deployment with GitHub Actions</span></span>](xref:azure/devops/github-actions)

<span data-ttu-id="119cd-154">使用 GitHub、GitHub Actions 和 Azure 为 ASP.NET Core 应用构建端到端持续集成和部署解决方案，包括使用 CodeQL 扫描代码来确保安全性和质量。</span><span class="sxs-lookup"><span data-stu-id="119cd-154">Build an end-to-end continuous integration and deployment solution for your ASP.NET Core app with GitHub, GitHub Actions, and Azure, including code scanning for security and quality using CodeQL.</span></span>

### <a name="monitor-and-debug"></a>[<span data-ttu-id="119cd-155">监视和调试</span><span class="sxs-lookup"><span data-stu-id="119cd-155">Monitor and debug</span></span>](xref:azure/devops/monitor)

<span data-ttu-id="119cd-156">使用 Azure 工具监视、故障排除和优化应用程序。</span><span class="sxs-lookup"><span data-stu-id="119cd-156">Use Azure's tools to monitor, troubleshoot, and tune your application.</span></span>

### <a name="next-steps"></a>[<span data-ttu-id="119cd-157">后续步骤</span><span class="sxs-lookup"><span data-stu-id="119cd-157">Next steps</span></span>](xref:azure/devops/next-steps)

<span data-ttu-id="119cd-158">ASP.NET Core 开发人员学习 Azure 的其他学习路径。</span><span class="sxs-lookup"><span data-stu-id="119cd-158">Other learning paths for the ASP.NET Core developer learning Azure.</span></span>

## <a name="additional-introductory-reading"></a><span data-ttu-id="119cd-159">其他介绍性读物</span><span class="sxs-lookup"><span data-stu-id="119cd-159">Additional introductory reading</span></span>

<span data-ttu-id="119cd-160">如果这是你首次接触云计算，这些文章介绍了基础知识。</span><span class="sxs-lookup"><span data-stu-id="119cd-160">If this is your first exposure to cloud computing, these articles explain the basics.</span></span>

* [<span data-ttu-id="119cd-161">什么是云计算？</span><span class="sxs-lookup"><span data-stu-id="119cd-161">What is Cloud Computing?</span></span>](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [<span data-ttu-id="119cd-162">云计算示例</span><span class="sxs-lookup"><span data-stu-id="119cd-162">Examples of Cloud Computing</span></span>](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [<span data-ttu-id="119cd-163">什么是 IaaS？</span><span class="sxs-lookup"><span data-stu-id="119cd-163">What is IaaS?</span></span>](https://azure.microsoft.com/overview/what-is-iaas/)
* [<span data-ttu-id="119cd-164">什么是 PaaS？</span><span class="sxs-lookup"><span data-stu-id="119cd-164">What is PaaS?</span></span>](https://azure.microsoft.com/overview/what-is-paas/)

>[!div class="step-by-step"]
>[<span data-ttu-id="119cd-165">下一页</span><span class="sxs-lookup"><span data-stu-id="119cd-165">Next</span></span>](tools-and-downloads.md)
