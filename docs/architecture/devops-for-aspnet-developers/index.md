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
# <a name="devops-for-aspnet-core-developers"></a>面向 ASP.NET Core 开发人员的 DevOps

[![封面图像](./media/cover-large.png)](https://aka.ms/devopsbook)

**EDITION v1.1.0**

请参阅[更改记录](https://aka.ms/devops-ebook-changelog)了解书籍更新和社区贡献。

本指南以[可下载 PDF 电子书](https://aka.ms/devopsbook)的形式提供。

发布者

Microsoft 开发人员部门、.NET 和 Visual Studio 产品团队

Microsoft Corporation 的一个部门

One Microsoft Way

Redmond, Washington 98052-6399

版权所有 &copy; 2021 Microsoft Corporation

保留所有权利。 未经发布者书面许可，不得以任何形式或任何方式复制或传播本书中的任何内容。

本书“按原样”提供，表达作者的观点和看法。 本书中表达的观点、看法和信息（包括 URL 和其他 Internet 网站引用）如有更改，恕不另行通知。

本书中提及的一些示例仅用于说明，纯属虚构。 不存在任何实际关联或联系，请勿妄加推断。

Microsoft 和 <https://www.microsoft.com> 上“商标”网页列出的商标是 Microsoft 集团公司的商标。

Mac 和 macOS 是 Apple Inc. 的商标

Docker 的鲸鱼徽标是 Docker Inc. 的注册商标经许可方可使用。

所有其他标记和徽标均为其各自所有者的财产。

## <a name="credits"></a>致谢

作者：

> [Cam Soper](https://twitter.com/camsoper)
>
> [Scott Addie](https://twitter.com/scottaddie)
>
> [Colin Dembovsky](https://twitter.com/colindembovsky)

## <a name="welcome"></a>欢迎使用

欢迎使用适用于 .NET 的 Azure 开发生命周期指南！ 本指南介绍使用 .NET 工具和进程围绕 Azure 构建开发生命周期的基本概念。 读完本指南后，可以获得成熟 DevOps 工具链的优势。

## <a name="who-this-guide-is-for"></a>本指南适用对象

读者应是有经验的 ASP.NET Core 开发人员（200-300 级别）。 无须了解有关 Azure 的任何内容，因为本指南中将进行介绍。 本指南对于更侧重于操作（而不是开发）的 DevOps 工程师可能也很有用。

本指南面向 Windows 开发人员。 但是，.NET Core 完全支持 Linux 和 macOS。 若要针对 Linux/macOS 调整本指南，请注意有关 Linux/macOS 差异的标注。

## <a name="what-this-guide-doesnt-cover"></a>本指南未涵盖的内容

本指南专注于 .NET 开发人员的端到端持续部署体验。 这不是介绍 Azure 中所有内容的全面指南，并不特别着重于适用于 Azure 服务的 .NET API。 介绍的重点在于持续集成、部署、监视和调试。 本指南结尾附近提供了后续步骤建议。 建议包括对 ASP.NET Core 开发人员有用的 Azure 平台服务。

## <a name="whats-in-this-guide"></a>本指南内容

### <a name="tools-and-downloads"></a>[工具和下载](xref:azure/devops/tools-and-downloads)

了解在何处获取本指南中使用的工具。

### <a name="deploy-to-app-service"></a>[部署到应用服务](xref:azure/devops/deploy-to-app-service)

了解将 ASP.NET Core 应用部署到 Azure 应用服务的各种方法。

### <a name="continuous-integration-and-deployment-with-azure-devops"></a>[使用 Azure DevOps 持续集成和持续部署](xref:azure/devops/cicd)

通过 GitHub、Azure DevOps Services 和 Azure，为 ASP.NET Core 应用生成端到端持续集成和部署解决方案。

### <a name="continuous-integration-and-deployment-with-github-actions"></a>[使用 GitHub Actions 持续集成和持续部署](xref:azure/devops/github-actions)

使用 GitHub、GitHub Actions 和 Azure 为 ASP.NET Core 应用构建端到端持续集成和部署解决方案，包括使用 CodeQL 扫描代码来确保安全性和质量。

### <a name="monitor-and-debug"></a>[监视和调试](xref:azure/devops/monitor)

使用 Azure 工具监视、故障排除和优化应用程序。

### <a name="next-steps"></a>[后续步骤](xref:azure/devops/next-steps)

ASP.NET Core 开发人员学习 Azure 的其他学习路径。

## <a name="additional-introductory-reading"></a>其他介绍性读物

如果这是你首次接触云计算，这些文章介绍了基础知识。

* [什么是云计算？](https://azure.microsoft.com/overview/what-is-cloud-computing/)
* [云计算示例](https://azure.microsoft.com/overview/examples-of-cloud-computing/)
* [什么是 IaaS？](https://azure.microsoft.com/overview/what-is-iaas/)
* [什么是 PaaS？](https://azure.microsoft.com/overview/what-is-paas/)

>[!div class="step-by-step"]
>[下一页](tools-and-downloads.md)
