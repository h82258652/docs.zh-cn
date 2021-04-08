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
# <a name="choosing-azure-compute-platforms-for-container-based-applications"></a><span data-ttu-id="29fbb-103">为基于容器的应用程序选择 Azure 计算平台</span><span class="sxs-lookup"><span data-stu-id="29fbb-103">Choosing Azure compute platforms for container-based applications</span></span>

<span data-ttu-id="29fbb-104">正如你在阅读前面章节时所注意到的，Azure 是一个提供多项选择的开放云。</span><span class="sxs-lookup"><span data-stu-id="29fbb-104">As you have noticed after reading the previous sections, Azure is an open cloud that offers multiple choices.</span></span> <span data-ttu-id="29fbb-105">你可以充分利用它来满足你的需求，但是它也提出了一些关于你应该为容器化应用程序采用哪些产品/技术的问题。</span><span class="sxs-lookup"><span data-stu-id="29fbb-105">You can use the best fit for your needs, however, it also surfaces questions about what product/technology you should use for your containerized applications.</span></span>

<span data-ttu-id="29fbb-106">作为“默认”  建议，以下是本指南中建议的主要标准：</span><span class="sxs-lookup"><span data-stu-id="29fbb-106">As a *by-default* recommendation, the following is the main criteria recommended in this guidance:</span></span>

- <span data-ttu-id="29fbb-107">**单一整体式应用：** 选择 Azure 应用服务</span><span class="sxs-lookup"><span data-stu-id="29fbb-107">**Single monolithic app:** Choose Azure App Service</span></span>
- <span data-ttu-id="29fbb-108">**N 层应用：** 如果拥有单个或几个后端服务，请选择“业务流程协调程序”，如 Azure Kubernetes 服务 (AKS) 或应用服务</span><span class="sxs-lookup"><span data-stu-id="29fbb-108">**N-Tier app:** Choose orchestrators such as Azure Kubernetes Service (AKS) or App Service if you have a single or a few back-end services</span></span>
- <span data-ttu-id="29fbb-109">**微服务：** 为容器选择 AKS 或 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="29fbb-109">**Microservices:** Choose AKS or Azure Web Apps for Containers</span></span>
- <span data-ttu-id="29fbb-110">**无服务器函数和事件处理程序：** 选择“Azure Functions”</span><span class="sxs-lookup"><span data-stu-id="29fbb-110">**Serverless functions & event handlers:** Choose Azure Functions</span></span>
- <span data-ttu-id="29fbb-111">**大规模 Batch：** 选择“Azure Batch”</span><span class="sxs-lookup"><span data-stu-id="29fbb-111">**Large-scale Batch:** Choose Azure Batch</span></span>

<span data-ttu-id="29fbb-112">但是，这一建议并不完全可信，因为产品的选择取决于具体的应用程序需求和特性。</span><span class="sxs-lookup"><span data-stu-id="29fbb-112">However, this recommendation should be taken with a pinch of salt, as the product's selection will depend on your specific application's needs and characteristics.</span></span> <span data-ttu-id="29fbb-113">并非所有的应用程序都是相同的，即使它们最初可能看起来是类似的。</span><span class="sxs-lookup"><span data-stu-id="29fbb-113">Not all applications are the same even when initially they might look similar types.</span></span>

<span data-ttu-id="29fbb-114">在对应用程序的需求进行更深入的分析之后，所选择的产品可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="29fbb-114">After a deeper analysis of the application's needs, the product selected could be different.</span></span> <span data-ttu-id="29fbb-115">但最好一开始就获得初步指导，你可以基于特定的优先级从其中开始评估和测试。</span><span class="sxs-lookup"><span data-stu-id="29fbb-115">But, as a starting point, it is good to have initial guidance from where you can start evaluating and testing based on certain priority.</span></span>

<span data-ttu-id="29fbb-116">在下表中，可以看到不同种类应用及其可能和推荐的 Azure 托管方案的明细。</span><span class="sxs-lookup"><span data-stu-id="29fbb-116">In the following table, you can see a breakdown of different kinds of apps and their possible and recommended Azure hosting scenarios.</span></span>

| <span data-ttu-id="29fbb-117">应用程序体系结构</span><span class="sxs-lookup"><span data-stu-id="29fbb-117">Application Architecture</span></span> | <span data-ttu-id="29fbb-118">VMs - Azure 虚拟机</span><span class="sxs-lookup"><span data-stu-id="29fbb-118">VMs - Azure Virtual Machines</span></span> | <span data-ttu-id="29fbb-119">ACI - Azure 容器实例</span><span class="sxs-lookup"><span data-stu-id="29fbb-119">ACI - Azure Container Instances</span></span> | <span data-ttu-id="29fbb-120">Azure 应用服务（带/不带容器）</span><span class="sxs-lookup"><span data-stu-id="29fbb-120">Azure App Service (w-w/o containers)</span></span> | <span data-ttu-id="29fbb-121">AKS - Azure Kubernetes 服务</span><span class="sxs-lookup"><span data-stu-id="29fbb-121">AKS - Azure Kubernetes Services</span></span> | <span data-ttu-id="29fbb-122">Azure Functions</span><span class="sxs-lookup"><span data-stu-id="29fbb-122">Azure Functions</span></span> | <span data-ttu-id="29fbb-123">Azure Batch</span><span class="sxs-lookup"><span data-stu-id="29fbb-123">Azure Batch</span></span> |
|:------------------------:|:--:|:--:|:--:|:--:|:--:|:--:|
| <span data-ttu-id="29fbb-124">**Web 应用（单片）**</span><span class="sxs-lookup"><span data-stu-id="29fbb-124">**Web apps (Monolithic)**</span></span>         | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | | |
| <span data-ttu-id="29fbb-129">**N 层应用（服务）**</span><span class="sxs-lookup"><span data-stu-id="29fbb-129">**N-Tier apps (Services)**</span></span>        | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 Azure Fuctions](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | |
| <span data-ttu-id="29fbb-135">**云本机（微服务）**</span><span class="sxs-lookup"><span data-stu-id="29fbb-135">**Cloud-Native (Microservices)**</span></span>  | | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) <br/> <span data-ttu-id="29fbb-138">（带有容器）&nbsp;</span><span class="sxs-lookup"><span data-stu-id="29fbb-138">(With&nbsp;containers)</span></span> | ![推荐使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> <span data-ttu-id="29fbb-140">（Linux&nbsp;容器）</span><span class="sxs-lookup"><span data-stu-id="29fbb-140">(Linux&nbsp;containers)</span></span>| ![推荐使用 Azure Functions](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> <span data-ttu-id="29fbb-142">（事件驱动）</span><span class="sxs-lookup"><span data-stu-id="29fbb-142">(Event&#x2011;driven)</span></span> | |
| <span data-ttu-id="29fbb-143">**批处理/作业（后台任务）**</span><span class="sxs-lookup"><span data-stu-id="29fbb-143">**Batch/Jobs (Background tasks)**</span></span> | ![可能使用 VM](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 ACI](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用应用服务](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![可能使用 AKS](media/choosing-azure-compute-options-for-container-based-applications/possible.png) | ![推荐使用 Azure Functions](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> <span data-ttu-id="29fbb-149">（后台任务）</span><span class="sxs-lookup"><span data-stu-id="29fbb-149">(Background&nbsp;tasks)</span></span> | ![建议使用 Azure Batch](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <br/> <span data-ttu-id="29fbb-151">（大规模）</span><span class="sxs-lookup"><span data-stu-id="29fbb-151">(Large&#x2011;scale)</span></span> |

<span data-ttu-id="29fbb-152">**图例**</span><span class="sxs-lookup"><span data-stu-id="29fbb-152">**Legend**</span></span>

![推荐的图标](media/choosing-azure-compute-options-for-container-based-applications/recommended.png) <span data-ttu-id="29fbb-154">推荐的 </span><span class="sxs-lookup"><span data-stu-id="29fbb-154">Recommended </span></span>\
![可能的图标](media/choosing-azure-compute-options-for-container-based-applications/possible.png) <span data-ttu-id="29fbb-156">可能</span><span class="sxs-lookup"><span data-stu-id="29fbb-156">Possible</span></span>

### <a name="additional-resources"></a><span data-ttu-id="29fbb-157">其他资源</span><span class="sxs-lookup"><span data-stu-id="29fbb-157">Additional resources</span></span>

- <span data-ttu-id="29fbb-158">选择用于应用程序的 Azure 计算服务</span><span class="sxs-lookup"><span data-stu-id="29fbb-158">**Choose an Azure compute service for your application**</span></span>

    <https://docs.microsoft.com/azure/architecture/guide/technology-choices/compute-decision-tree>

> [!div class="step-by-step"]
> <span data-ttu-id="29fbb-159">[上一页](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
> [下一页](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)</span><span class="sxs-lookup"><span data-stu-id="29fbb-159">[Previous](when-to-deploy-windows-containers-to-azure-container-service-kubernetes.md)
[Next](build-resilient-services-ready-for-the-cloud-embrace-transient-failures-in-the-cloud.md)</span></span>
