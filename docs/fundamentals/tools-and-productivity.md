---
title: .NET 中的工具和诊断
author: IEvangelist
description: 了解可供 .NET 开发人员使用的开发和诊断工具。
ms.author: dapine
ms.date: 10/21/2020
ms.topic: overview
ms.openlocfilehash: 85d64c0d5857d8603316175d18f8c2f7eab3cc93
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498514"
---
# <a name="tools-and-diagnostics-in-net"></a><span data-ttu-id="3dc9a-103">.NET 中的工具和诊断</span><span class="sxs-lookup"><span data-stu-id="3dc9a-103">Tools and diagnostics in .NET</span></span>

<span data-ttu-id="3dc9a-104">在本文中，你将了解可供 .NET 开发人员使用的各种工具。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-104">In this article, you'll learn about the various tools available to .NET developers.</span></span> <span data-ttu-id="3dc9a-105">使用 .NET，将获得可靠的软件开发工具包 (SDK)，其中包含命令行接口 (CLI)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-105">With .NET, you have a robust software development kit (SDK) that includes a command-line interface (CLI).</span></span> <span data-ttu-id="3dc9a-106">.NET CLI 支持 .NET 集成开发环境 (IDE) 中的许多功能。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-106">The .NET CLI enables many of the features throughout the .NET-ready integrated development environments (IDEs).</span></span> <span data-ttu-id="3dc9a-107">本文还提供用于提高生产率的资源，例如用于诊断性能问题、内存泄漏、高 CPU、死锁的 .NET CLI 工具，以及用于代码分析的工具支持。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-107">This article also provides resources to productivity capabilities, such as .NET CLI tools for diagnosing performance issues, memory leaks, high CPU, deadlocks, and tooling support for code analysis.</span></span>

## <a name="net-sdk"></a><span data-ttu-id="3dc9a-108">.NET SDK</span><span class="sxs-lookup"><span data-stu-id="3dc9a-108">.NET SDK</span></span>

<span data-ttu-id="3dc9a-109">.NET SDK 同时包括 .NET 运行时和 .NET CLI。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-109">The .NET SDK includes both the .NET Runtime and the .NET CLI.</span></span> <span data-ttu-id="3dc9a-110">可下载适用于 Windows、Linux、macOS 或 Docker 的 [.NET SDK](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-110">You can download the [.NET SDK](https://dotnet.microsoft.com/download) for Windows, Linux, macOS, or Docker.</span></span> <span data-ttu-id="3dc9a-111">有关详细信息，请参阅 [.NET SDK 概述](../core/sdk.md)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-111">For more information, see [.NET SDK overview](../core/sdk.md).</span></span>

## <a name="net-cli"></a><span data-ttu-id="3dc9a-112">.NET CLI</span><span class="sxs-lookup"><span data-stu-id="3dc9a-112">.NET CLI</span></span>

<span data-ttu-id="3dc9a-113">.NET CLI 是用于开发、生成、运行和发布 .NET 应用程序的跨平台工具链。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-113">The .NET CLI is a cross-platform toolchain for developing, building, running, and publishing .NET applications.</span></span> <span data-ttu-id="3dc9a-114">.NET CLI 附带了 .NET SDK。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-114">The .NET CLI is included with the .NET SDK.</span></span> <span data-ttu-id="3dc9a-115">有关详细信息，请参阅 [.NET CLI 概述](../core/tools/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-115">For more information, see [.NET CLI overview](../core/tools/index.md).</span></span>

## <a name="ides"></a><span data-ttu-id="3dc9a-116">IDE</span><span class="sxs-lookup"><span data-stu-id="3dc9a-116">IDEs</span></span>

<span data-ttu-id="3dc9a-117">可在 [Visual Studio Code](https://code.visualstudio.com/docs)、[Visual Studio](/visualstudio/windows)或 [Visual Studio for Mac](/visualstudio/mac) 中写入 .NET 应用程序。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-117">You can write .NET applications in [Visual Studio Code](https://code.visualstudio.com/docs), [Visual Studio](/visualstudio/windows), or [Visual Studio for Mac](/visualstudio/mac).</span></span>

## <a name="additional-tools"></a><span data-ttu-id="3dc9a-118">其他工具</span><span class="sxs-lookup"><span data-stu-id="3dc9a-118">Additional tools</span></span>

<span data-ttu-id="3dc9a-119">除了更常用的工具外，.NET 还提供适用于特定方案的工具。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-119">In addition to the more common tools, .NET also provides tools for specific scenarios.</span></span> <span data-ttu-id="3dc9a-120">部分用例，包括卸载 .NET SDK 或 .NET 运行时、检索 Windows Communication Foundation (WCF) 元数据、生成代理源代码以及序列化 XML。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-120">Some of the use cases include uninstalling the .NET SDK or the .NET Runtime, retrieving Windows Communication Foundation (WCF) metadata, generating proxy source code, and serializing XML.</span></span> <span data-ttu-id="3dc9a-121">有关详细信息，请参阅 [.NET 其他工具概述](../core/additional-tools/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-121">For more information, see [.NET additional tools overview](../core/additional-tools/index.md).</span></span>

## <a name="diagnostics-and-instrumentation"></a><span data-ttu-id="3dc9a-122">诊断和检测</span><span class="sxs-lookup"><span data-stu-id="3dc9a-122">Diagnostics and instrumentation</span></span>

<span data-ttu-id="3dc9a-123">作为 .NET 开发人员，你可使用常见的性能诊断工具来监视应用性能、使用跟踪分析应用、收集性能指标以及分析转储文件。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-123">As a .NET developer, you can make use of common performance diagnostic tools to monitor app performance, profile apps with tracing, collect performance metrics, and analyze dump files.</span></span> <span data-ttu-id="3dc9a-124">可使用事件计数器收集性能指标，并使用分析工具深入了解应用的执行方式。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-124">You collect performance metrics with event counters, and use profiling tools to gain insights into how apps perform.</span></span> <span data-ttu-id="3dc9a-125">有关详细信息，请参阅 [.NET 诊断工具](../core/diagnostics/index.md)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-125">For more information, see [.NET diagnostics tools](../core/diagnostics/index.md).</span></span>

## <a name="code-analysis"></a><span data-ttu-id="3dc9a-126">代码分析</span><span class="sxs-lookup"><span data-stu-id="3dc9a-126">Code analysis</span></span>

<span data-ttu-id="3dc9a-127">.NET Compiler Platform (Roslyn) 分析器会检查 C# 或 Visual Basic 代码的代码质量和代码样式问题。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-127">The .NET compiler platform (Roslyn) analyzers inspect your C# or Visual Basic code for code quality and code style issues.</span></span> <span data-ttu-id="3dc9a-128">有关详细信息，请参阅 [.NET 源代码分析概述](code-analysis/overview.md)。</span><span class="sxs-lookup"><span data-stu-id="3dc9a-128">For more information, see [.NET source code analysis overview](code-analysis/overview.md).</span></span>
