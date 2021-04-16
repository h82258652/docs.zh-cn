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
# <a name="tools-and-diagnostics-in-net"></a>.NET 中的工具和诊断

在本文中，你将了解可供 .NET 开发人员使用的各种工具。 使用 .NET，将获得可靠的软件开发工具包 (SDK)，其中包含命令行接口 (CLI)。 .NET CLI 支持 .NET 集成开发环境 (IDE) 中的许多功能。 本文还提供用于提高生产率的资源，例如用于诊断性能问题、内存泄漏、高 CPU、死锁的 .NET CLI 工具，以及用于代码分析的工具支持。

## <a name="net-sdk"></a>.NET SDK

.NET SDK 同时包括 .NET 运行时和 .NET CLI。 可下载适用于 Windows、Linux、macOS 或 Docker 的 [.NET SDK](https://dotnet.microsoft.com/download)。 有关详细信息，请参阅 [.NET SDK 概述](../core/sdk.md)。

## <a name="net-cli"></a>.NET CLI

.NET CLI 是用于开发、生成、运行和发布 .NET 应用程序的跨平台工具链。 .NET CLI 附带了 .NET SDK。 有关详细信息，请参阅 [.NET CLI 概述](../core/tools/index.md)。

## <a name="ides"></a>IDE

可在 [Visual Studio Code](https://code.visualstudio.com/docs)、[Visual Studio](/visualstudio/windows)或 [Visual Studio for Mac](/visualstudio/mac) 中写入 .NET 应用程序。

## <a name="additional-tools"></a>其他工具

除了更常用的工具外，.NET 还提供适用于特定方案的工具。 部分用例，包括卸载 .NET SDK 或 .NET 运行时、检索 Windows Communication Foundation (WCF) 元数据、生成代理源代码以及序列化 XML。 有关详细信息，请参阅 [.NET 其他工具概述](../core/additional-tools/index.md)。

## <a name="diagnostics-and-instrumentation"></a>诊断和检测

作为 .NET 开发人员，你可使用常见的性能诊断工具来监视应用性能、使用跟踪分析应用、收集性能指标以及分析转储文件。 可使用事件计数器收集性能指标，并使用分析工具深入了解应用的执行方式。 有关详细信息，请参阅 [.NET 诊断工具](../core/diagnostics/index.md)。

## <a name="code-analysis"></a>代码分析

.NET Compiler Platform (Roslyn) 分析器会检查 C# 或 Visual Basic 代码的代码质量和代码样式问题。 有关详细信息，请参阅 [.NET 源代码分析概述](code-analysis/overview.md)。
