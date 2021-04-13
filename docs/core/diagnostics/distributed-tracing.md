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
# <a name="net-distributed-tracing"></a>.NET 分布式跟踪

分布式跟踪是一种诊断技术，可帮助工程师找出应用程序中的故障和性能问题，尤其是那些可能跨多个计算机或进程分布的问题。 此技术通过应用程序跟踪请求，将不同应用程序组件完成的工作关联在一起，并将其与应用程序可能为并发请求所做的其他工作分开。 例如，对典型 Web 服务的请求可能首先由负载均衡器接收，然后转发到 Web 服务器进程，后者随后会对数据库进行多次查询。 使用分布式跟踪，工程师可以区分这些步骤中的任何一项是否失败、每个步骤所用的时间，并有可能记录每个步骤运行时生成的消息。

## <a name="getting-started-for-net-app-developers"></a>.NET 应用开发人员入门

检测关键的 .NET 库可自动生成分布式跟踪信息，但需要收集和存储此信息，以便以后能够查看。
通常，应用开发人员会选择存储这些跟踪信息的遥测服务，然后使用相应的库将分布式跟踪遥测传输到所选的服务。 [OpenTelemetry](https://github.com/open-telemetry/opentelemetry-dotnet/blob/main/docs/trace/getting-started/README.md) 是一个支持多项服务的与供应商无关的库，[Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/distributed-tracing) 是 Microsoft 提供的一项功能完整的服务，还有许多提供集成 .NET 解决方案的优质第三方 APM 供应商。

- [了解分布式跟踪概念](distributed-tracing-concepts.md)
- 指南
  - [使用 Application Insights 收集分布式跟踪](distributed-tracing-collection-walkthroughs.md#collect-traces-using-application-insights)
  - [使用 OpenTelemetry 收集分布式跟踪](distributed-tracing-collection-walkthroughs.md#collect-traces-using-opentelemetry)
  - [使用自定义逻辑收集分布式跟踪](distributed-tracing-collection-walkthroughs.md#collect-traces-using-custom-logic)
  - [添加自定义分布式跟踪检测](distributed-tracing-instrumentation-walkthroughs.md)

对于第三方遥测收集服务，请按照供应商提供的设置说明进行操作。

## <a name="getting-started-for-net-library-developers"></a>.NET 库开发人员入门

对于 .NET 库，我们不需要关心遥测数据最终是如何收集的，而只需要关心它是如何产生的。 如果你认为使用库的 .NET 应用开发人员会希望看到它在分布式跟踪中的详细工作，则应添加分布式跟踪检测来支持它。

- [了解分布式跟踪概念](distributed-tracing-concepts.md)
- 指南
  - [添加自定义分布式跟踪检测](distributed-tracing-instrumentation-walkthroughs.md)
