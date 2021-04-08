---
title: 为基于容器的应用程序选择 Azure 计算平台
description: 通过 Azure 云和 Windows 容器现代化现有 .NET 应用程序 | 为基于容器的应用程序选择 Azure 计算平台
ms.date: 02/18/2020
ms.openlocfilehash: fcb9a0e7277f5ce31309f52aff63d579b0bb9a02
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079604"
---
# <a name="choosing-azure-compute-platforms-for-container-based-applications"></a>为基于容器的应用程序选择 Azure 计算平台

正如你在阅读前面章节时所注意到的，Azure 是一个提供多项选择的开放云。 你可以充分利用它来满足你的需求，但是它也提出了一些关于你应该为容器化应用程序采用哪些产品/技术的问题。

作为“默认”  建议，以下是本指南中建议的主要标准：

- **单一整体式应用：** 选择 Azure 应用服务
- **N 层应用：** 如果拥有单个或几个后端服务，请选择“业务流程协调程序”，如 Azure Kubernetes 服务 (AKS) 或应用服务
- **微服务：** 为容器选择 AKS 或 Azure Web 应用
- **无服务器函数和事件处理程序：** 选择“Azure Functions”
- **大规模 Batch：** 选择“Azure Batch”

但是，这一建议并不完全可信，因为产品的选择取决于具体的应用程序需求和特性。 并非所有的应用程序都是相同的，即使它们最初可能看起来是类似的。

在对应用程序的需求进行更深入的分析之后，所选择的产品可能会有所不同。 但最好一开始就获得初步指导，你可以基于特定的优先级从其中开始评估和测试。

在下表中，可以看到不同种类应用及其可能和推荐的 Azure 托管方案的明细。

| 应用程序体系结构 | VMs - Azure 虚拟机 | ACI - Azure 容器实例 | Azure 应用服务（带/不带容器） | AKS - Azure Kubernetes 服务 | Azure Functions | Azure Batch |
|:------------------------:|:--:|:--:|:--:|:--:|:--:|:--:|
| **Web 应用（单片）**         | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | | |
| **N 层应用（服务）**        | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 Azure Fuctions](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | |
| **云本机（微服务）**  | | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) <br/> （带有容器）&nbsp; | ![推荐使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> （Linux&nbsp;容器）| ![推荐使用 Azure Functions](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> （事件驱动） | |
| **批处理/作业（后台任务）** | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用 Azure Functions](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> （后台任务） | ![建议使用 Azure Batch](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> （大规模） |

**图例**

![推荐的图标](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) 推荐的 \
![可能的图标](media/choosing-azure-compute-options-for-container-based-applications/possible.png) 可能

### <a name="additional-resources"></a>其他资源

- 选择用于应用程序的 Azure 计算服务

    <https://docs.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree>

> [!div class="step-by-step"]
> [上一页](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
> [下一页](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)
