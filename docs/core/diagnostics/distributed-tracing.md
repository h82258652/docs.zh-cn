---
title: 分布式跟踪 - .NET
description: 介绍了 .NET 分布式跟踪。
ms.date: 03/15/2021
ms.openlocfilehash: 274a058bf9daf096958813575901e83a3976a3a4
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111109"
---
# <a name="net-distributed-tracing"></a><span data-ttu-id="08505-103">.NET 分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="08505-103">.NET Distributed Tracing</span></span>

<span data-ttu-id="08505-104">分布式跟踪是一种诊断技术，可帮助工程师找出应用程序中的故障和性能问题，尤其是那些可能跨多个计算机或进程分布的问题。</span><span class="sxs-lookup"><span data-stu-id="08505-104">Distributed tracing is a diagnostic technique that helps engineers localize failures and performance issues within applications, especially those that may be distributed across multiple machines or processes.</span></span> <span data-ttu-id="08505-105">此技术通过应用程序跟踪请求，将不同应用程序组件完成的工作关联在一起，并将其与应用程序可能为并发请求所做的其他工作分开。</span><span class="sxs-lookup"><span data-stu-id="08505-105">This technique tracks requests through an application correlating together work done by different application components and separating it from other work the application may be doing for concurrent requests.</span></span> <span data-ttu-id="08505-106">例如，对典型 Web 服务的请求可能首先由负载均衡器接收，然后转发到 Web 服务器进程，后者随后会对数据库进行多次查询。</span><span class="sxs-lookup"><span data-stu-id="08505-106">For example a request to a typical web service might be first received by a load balancer, then forwarded to a web server process, which then makes several queries to a database.</span></span> <span data-ttu-id="08505-107">使用分布式跟踪，工程师可以区分这些步骤中的任何一项是否失败、每个步骤所用的时间，并有可能记录每个步骤运行时生成的消息。</span><span class="sxs-lookup"><span data-stu-id="08505-107">Using distributed tracing allows engineers to distinguish if any of those steps failed, how long each step took, and potentially logging messages produced by each step as it ran.</span></span>

## <a name="getting-started-for-net-app-developers"></a><span data-ttu-id="08505-108">.NET 应用开发人员入门</span><span class="sxs-lookup"><span data-stu-id="08505-108">Getting started for .NET app developers</span></span>

<span data-ttu-id="08505-109">检测关键的 .NET 库可自动生成分布式跟踪信息，但需要收集和存储此信息，以便以后能够查看。</span><span class="sxs-lookup"><span data-stu-id="08505-109">Key .NET libraries are instrumented to produce distributed tracing information automatically but this information needs to be collected and stored so that it will be available for review later.</span></span>
<span data-ttu-id="08505-110">通常，应用开发人员会选择存储这些跟踪信息的遥测服务，然后使用相应的库将分布式跟踪遥测传输到所选的服务。</span><span class="sxs-lookup"><span data-stu-id="08505-110">Typically app developers will select a telemetry service that stores this trace information for them and then use a corresponding library to transmit the distributed tracing telemetry to their chosen service.</span></span> <span data-ttu-id="08505-111">[OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md) 是一个支持多项服务的与供应商无关的库，[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) 是 Microsoft 提供的一项功能完整的服务，还有许多提供集成 .NET 解决方案的优质第三方 APM 供应商。</span><span class="sxs-lookup"><span data-stu-id="08505-111">[OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md) is a vendor neutral library that supports several services, [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) is a full featured service provided by Microsoft, and there are many high quality 3rd party APM vendors that offer integrated .NET solutions.</span></span>

- [<span data-ttu-id="08505-112">了解分布式跟踪概念</span><span class="sxs-lookup"><span data-stu-id="08505-112">Understand distributed tracing concepts</span></span>](distributed-tracing-concepts.md)
- <span data-ttu-id="08505-113">指南</span><span class="sxs-lookup"><span data-stu-id="08505-113">Guides</span></span>
  - [<span data-ttu-id="08505-114">使用 Application Insights 收集分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="08505-114">Collect distributed traces with Application Insights</span></span>](distributed-tracing-collection-walkthroughs.md#collect-traces-using-application-insights)
  - [<span data-ttu-id="08505-115">使用 OpenTelemetry 收集分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="08505-115">Collect distributed traces with OpenTelemetry</span></span>](distributed-tracing-collection-walkthroughs.md#collect-traces-using-opentelemetry)
  - [<span data-ttu-id="08505-116">使用自定义逻辑收集分布式跟踪</span><span class="sxs-lookup"><span data-stu-id="08505-116">Collect distributed traces with custom logic</span></span>](distributed-tracing-collection-walkthroughs.md#collect-traces-using-custom-logic)
  - [<span data-ttu-id="08505-117">添加自定义分布式跟踪检测</span><span class="sxs-lookup"><span data-stu-id="08505-117">Adding custom distributed trace instrumentation</span></span>](distributed-tracing-instrumentation-walkthroughs.md)

<span data-ttu-id="08505-118">对于第三方遥测收集服务，请按照供应商提供的设置说明进行操作。</span><span class="sxs-lookup"><span data-stu-id="08505-118">For 3rd party telemetry collection services follow the setup instructions provided by the vendor.</span></span>

## <a name="getting-started-for-net-library-developers"></a><span data-ttu-id="08505-119">.NET 库开发人员入门</span><span class="sxs-lookup"><span data-stu-id="08505-119">Getting started for .NET library developers</span></span>

<span data-ttu-id="08505-120">对于 .NET 库，我们不需要关心遥测数据最终是如何收集的，而只需要关心它是如何产生的。</span><span class="sxs-lookup"><span data-stu-id="08505-120">.NET libraries don't need to be concerned with how telemetry is ultimately collected, only with how it is produced.</span></span> <span data-ttu-id="08505-121">如果你认为使用库的 .NET 应用开发人员会希望看到它在分布式跟踪中的详细工作，则应添加分布式跟踪检测来支持它。</span><span class="sxs-lookup"><span data-stu-id="08505-121">If you believe .NET app developers that use your library would appreciate seeing the work that it does detailed in a distributed trace then you should add distributed tracing instrumentation to support it.</span></span>

- [<span data-ttu-id="08505-122">了解分布式跟踪概念</span><span class="sxs-lookup"><span data-stu-id="08505-122">Understand distributed tracing concepts</span></span>](distributed-tracing-concepts.md)
- <span data-ttu-id="08505-123">指南</span><span class="sxs-lookup"><span data-stu-id="08505-123">Guides</span></span>
  - [<span data-ttu-id="08505-124">添加自定义分布式跟踪检测</span><span class="sxs-lookup"><span data-stu-id="08505-124">Adding custom distributed trace instrumentation</span></span>](distributed-tracing-instrumentation-walkthroughs.md)
