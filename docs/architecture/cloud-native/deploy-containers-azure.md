---
title: 在 Azure 中部署容器
description: 在 Azure 中通过 Azure 容器注册表、Azure Kubernetes 服务和 Azure Dev Spaces 部署容器。
ms.date: 04/13/2020
ms.openlocfilehash: f8b029f31ba493b3a777e12ca72854145ef312c4
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497406"
---
# <a name="deploying-containers-in-azure"></a><span data-ttu-id="04e8c-103">在 Azure 中部署容器</span><span class="sxs-lookup"><span data-stu-id="04e8c-103">Deploying containers in Azure</span></span>

<span data-ttu-id="04e8c-104">本章节和第1章讨论了容器。</span><span class="sxs-lookup"><span data-stu-id="04e8c-104">We've discussed containers in this chapter and in chapter 1.</span></span> <span data-ttu-id="04e8c-105">我们发现，容器为云本机应用程序提供了许多好处，包括可移植性。</span><span class="sxs-lookup"><span data-stu-id="04e8c-105">We've seen that containers provide many benefits to cloud-native applications, including portability.</span></span> <span data-ttu-id="04e8c-106">在 Azure 云中，你可以在过渡环境和生产环境中部署相同的容器化服务。</span><span class="sxs-lookup"><span data-stu-id="04e8c-106">In the Azure cloud, you can deploy the same containerized services across staging and production environments.</span></span> <span data-ttu-id="04e8c-107">Azure 提供了多个用于托管容器化工作负荷的选项：</span><span class="sxs-lookup"><span data-stu-id="04e8c-107">Azure provides several options for hosting these containerized workloads:</span></span>

- <span data-ttu-id="04e8c-108">Azure Kubernetes 服务 (AKS)</span><span class="sxs-lookup"><span data-stu-id="04e8c-108">Azure Kubernetes Services (AKS)</span></span>
- <span data-ttu-id="04e8c-109">Azure 容器实例 (ACI)</span><span class="sxs-lookup"><span data-stu-id="04e8c-109">Azure Container Instance (ACI)</span></span>
- <span data-ttu-id="04e8c-110">适用于容器的 Azure Web 应用</span><span class="sxs-lookup"><span data-stu-id="04e8c-110">Azure Web Apps for Containers</span></span>

## <a name="azure-container-registry"></a><span data-ttu-id="04e8c-111">Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="04e8c-111">Azure Container Registry</span></span>

<span data-ttu-id="04e8c-112">容器化微服务时，首先构建一个容器 "image"。</span><span class="sxs-lookup"><span data-stu-id="04e8c-112">When containerizing a microservice, you first build a container "image."</span></span> <span data-ttu-id="04e8c-113">此图像是服务代码、依赖项和运行时的二进制表示形式。</span><span class="sxs-lookup"><span data-stu-id="04e8c-113">The image is a binary representation of the service code, dependencies, and runtime.</span></span> <span data-ttu-id="04e8c-114">尽管可以使用 `Docker Build` DOCKER API 中的命令手动创建映像，但更好的方法是将其创建为自动生成过程的一部分。</span><span class="sxs-lookup"><span data-stu-id="04e8c-114">While you can manually create an image using the `Docker Build` command from the Docker API, a better approach is to create it as part of an automated build process.</span></span>

<span data-ttu-id="04e8c-115">创建后，容器映像存储在容器注册表中。</span><span class="sxs-lookup"><span data-stu-id="04e8c-115">Once created, container images are stored in container registries.</span></span> <span data-ttu-id="04e8c-116">它们使你能够生成、存储和管理容器映像。</span><span class="sxs-lookup"><span data-stu-id="04e8c-116">They enable you to build, store, and manage container images.</span></span> <span data-ttu-id="04e8c-117">有很多可用的注册表项，无论是公共的还是私有的。</span><span class="sxs-lookup"><span data-stu-id="04e8c-117">There are many registries available, both public and private.</span></span> <span data-ttu-id="04e8c-118">Azure 容器注册表 (ACR) 是 Azure 云中完全托管的容器注册表服务。</span><span class="sxs-lookup"><span data-stu-id="04e8c-118">Azure Container Registry (ACR) is a fully managed container registry service in the Azure cloud.</span></span> <span data-ttu-id="04e8c-119">它将映像保存在 Azure 网络中，缩短将其部署到 Azure 容器主机的时间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-119">It persists your images inside the Azure network, reducing the time to deploy them to Azure container hosts.</span></span> <span data-ttu-id="04e8c-120">还可以使用与其他 Azure 资源相同的安全和标识过程来保护这些资源。</span><span class="sxs-lookup"><span data-stu-id="04e8c-120">You can also secure them using the same security and identity procedures that you use for other Azure resources.</span></span>

<span data-ttu-id="04e8c-121">使用 [Azure 门户](/azure/container-registry/container-registry-get-started-portal)、 [Azure CLI](/azure/container-registry/container-registry-get-started-azure-cli)或 [PowerShell 工具](/azure/container-registry/container-registry-get-started-powershell)创建 Azure 容器注册表。</span><span class="sxs-lookup"><span data-stu-id="04e8c-121">You create an Azure Container Registry using the [Azure portal](/azure/container-registry/container-registry-get-started-portal), [Azure CLI](/azure/container-registry/container-registry-get-started-azure-cli), or [PowerShell tools](/azure/container-registry/container-registry-get-started-powershell).</span></span> <span data-ttu-id="04e8c-122">在 Azure 中创建注册表非常简单。</span><span class="sxs-lookup"><span data-stu-id="04e8c-122">Creating a registry in Azure is simple.</span></span> <span data-ttu-id="04e8c-123">它需要 Azure 订阅、资源组和唯一名称。</span><span class="sxs-lookup"><span data-stu-id="04e8c-123">It requires an Azure subscription, resource group, and a unique name.</span></span> <span data-ttu-id="04e8c-124">图3-11 显示了用于创建注册表的基本选项，这些选项将在上托管 `registryname.azurecr.io` 。</span><span class="sxs-lookup"><span data-stu-id="04e8c-124">Figure 3-11 shows the basic options for creating a registry, which will be hosted at `registryname.azurecr.io`.</span></span>

![创建容器注册表](./media/create-container-registry.png)

<span data-ttu-id="04e8c-126">**图 3-11**。</span><span class="sxs-lookup"><span data-stu-id="04e8c-126">**Figure 3-11**.</span></span> <span data-ttu-id="04e8c-127">创建容器注册表</span><span class="sxs-lookup"><span data-stu-id="04e8c-127">Create container registry</span></span>

<span data-ttu-id="04e8c-128">创建注册表后，需要先对其进行身份验证，然后才能使用。</span><span class="sxs-lookup"><span data-stu-id="04e8c-128">Once you've created the registry, you'll need to authenticate with it before you can use it.</span></span> <span data-ttu-id="04e8c-129">通常，你将使用 Azure CLI 命令登录到注册表：</span><span class="sxs-lookup"><span data-stu-id="04e8c-129">Typically, you'll log into the registry using the Azure CLI command:</span></span>

```azurecli
az acr login --name *registryname*
```

<span data-ttu-id="04e8c-130">经过身份验证后，可以使用 docker 命令将容器映像推送到其中。</span><span class="sxs-lookup"><span data-stu-id="04e8c-130">Once authenticated, you can use docker commands to push container images to it.</span></span> <span data-ttu-id="04e8c-131">但是，在执行此操作之前，必须用 ACR 登录服务器的完全限定名 (URL) 来标记映像。</span><span class="sxs-lookup"><span data-stu-id="04e8c-131">Before you can do so, however, you must tag your image with the fully qualified name (URL) of your ACR login server.</span></span> <span data-ttu-id="04e8c-132">它的格式为 *registryname*. azurecr.io。</span><span class="sxs-lookup"><span data-stu-id="04e8c-132">It will have the format *registryname*.azurecr.io.</span></span>

```console
docker tag mycontainer myregistry.azurecr.io/mycontainer:v1
```

<span data-ttu-id="04e8c-133">标记该映像后，使用 `docker push` 命令将该映像推送到 ACR 实例。</span><span class="sxs-lookup"><span data-stu-id="04e8c-133">After you've tagged the image, you use the `docker push` command to push the image to your ACR instance.</span></span>

```console
docker push myregistry.azurecr.io/mycontainer:v1
```

<span data-ttu-id="04e8c-134">将映像推送到注册表后，最好使用以下命令从本地 Docker 环境中删除映像：</span><span class="sxs-lookup"><span data-stu-id="04e8c-134">After you push an image to the registry, it's a good idea to remove the image from your local Docker environment, using this command:</span></span>

```console
docker rmi myregistry.azurecr.io/mycontainer:v1
```

<span data-ttu-id="04e8c-135">作为最佳做法，开发人员不应手动将映像推送到容器注册表。</span><span class="sxs-lookup"><span data-stu-id="04e8c-135">As a best practice, developers shouldn't manually push images to a container registry.</span></span> <span data-ttu-id="04e8c-136">相反，在 GitHub 或 Azure DevOps 之类的工具中定义的生成管道应负责此过程。</span><span class="sxs-lookup"><span data-stu-id="04e8c-136">Instead, a build pipeline defined in a tool like GitHub or Azure DevOps should be responsible for this process.</span></span> <span data-ttu-id="04e8c-137">有关详细信息，请参阅 [云-Native DevOps 章节](devops.md)。</span><span class="sxs-lookup"><span data-stu-id="04e8c-137">Learn more in the [Cloud-Native DevOps chapter](devops.md).</span></span>

## <a name="acr-tasks"></a><span data-ttu-id="04e8c-138">ACR 任务</span><span class="sxs-lookup"><span data-stu-id="04e8c-138">ACR Tasks</span></span>

<span data-ttu-id="04e8c-139">[ACR 任务](/azure/container-registry/container-registry-tasks-overview) 是一组 Azure 容器注册表中可用的功能。</span><span class="sxs-lookup"><span data-stu-id="04e8c-139">[ACR Tasks](/azure/container-registry/container-registry-tasks-overview) is a set of features available from the Azure Container Registry.</span></span> <span data-ttu-id="04e8c-140">它通过在 Azure 云中构建和管理容器映像来扩展你的 [内部循环开发周期](../containerized-lifecycle/design-develop-containerized-apps/docker-apps-inner-loop-workflow.md) 。</span><span class="sxs-lookup"><span data-stu-id="04e8c-140">It extends your [inner-loop development cycle](../containerized-lifecycle/design-develop-containerized-apps/docker-apps-inner-loop-workflow.md) by building and managing container images in the Azure cloud.</span></span> <span data-ttu-id="04e8c-141">它们不是在 `docker build` `docker push` 开发计算机上进行调用，而是由云中的 ACR 任务自动处理的。</span><span class="sxs-lookup"><span data-stu-id="04e8c-141">Instead of invoking a `docker build` and `docker push` locally on your development machine, they're automatically handled by ACR Tasks in the cloud.</span></span>

<span data-ttu-id="04e8c-142">以下 AZ CLI 命令都生成一个容器映像并将其推送到 ACR：</span><span class="sxs-lookup"><span data-stu-id="04e8c-142">The following AZ CLI command both builds a container image and pushes it to ACR:</span></span>

```azurecli
# create a container registry
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic

# build container image in ACR and push it into your container regsitry
az acr build --image sample/hello-world:v1  --registry myContainerRegistry008 --file Dockerfile .
```

<span data-ttu-id="04e8c-143">如前面的命令块中所示，无需在开发计算机上安装 Docker 桌面。</span><span class="sxs-lookup"><span data-stu-id="04e8c-143">As you can see from the previous command block, there's no need to install Docker Desktop on your development machine.</span></span> <span data-ttu-id="04e8c-144">此外，还可以配置 ACR 任务触发器，以便在源代码和基本映像更新上重新生成容器映像。</span><span class="sxs-lookup"><span data-stu-id="04e8c-144">Additionally, you can configure ACR Task triggers to rebuild containers images on both source code and base image updates.</span></span>

## <a name="azure-kubernetes-service"></a><span data-ttu-id="04e8c-145">Azure Kubernetes 服务</span><span class="sxs-lookup"><span data-stu-id="04e8c-145">Azure Kubernetes Service</span></span>

<span data-ttu-id="04e8c-146">我们在本章中讨论了 Azure Kubernetes Service (AKS) 。</span><span class="sxs-lookup"><span data-stu-id="04e8c-146">We discussed Azure Kubernetes Service (AKS) at length in this chapter.</span></span> <span data-ttu-id="04e8c-147">我们已了解到，事实容器 orchestrator 正在管理容器化的云本机应用程序。</span><span class="sxs-lookup"><span data-stu-id="04e8c-147">We've seen that it's the de facto container orchestrator managing containerized cloud-native applications.</span></span>

<span data-ttu-id="04e8c-148">将映像部署到注册表（如 ACR）后，可将 AKS 配置为自动拉取和部署。</span><span class="sxs-lookup"><span data-stu-id="04e8c-148">Once you deploy an image to a registry, such as ACR, you can configure AKS to automatically pull and deploy it.</span></span> <span data-ttu-id="04e8c-149">使用 CI/CD 管道时，你可以配置一种不限的  [发布](https://martinfowler.com/bliki/CanaryRelease.html) 策略，以最大程度地减少快速部署更新时所涉及的风险。</span><span class="sxs-lookup"><span data-stu-id="04e8c-149">With a CI/CD pipeline in place, you might configure a  [canary release](https://martinfowler.com/bliki/CanaryRelease.html) strategy to minimize the risk involved when rapidly deploying updates.</span></span> <span data-ttu-id="04e8c-150">新版本的应用程序最初在生产环境中进行了配置，但未将流量路由到该应用程序。</span><span class="sxs-lookup"><span data-stu-id="04e8c-150">The new version of the app is initially configured in production with no traffic routed to it.</span></span> <span data-ttu-id="04e8c-151">然后，系统会将少量用户路由到新部署的版本。</span><span class="sxs-lookup"><span data-stu-id="04e8c-151">Then, the system will route a small percentage of users to the newly deployed version.</span></span> <span data-ttu-id="04e8c-152">随着团队在新版本中获得信心，它可以推出更多实例并停用旧版本。</span><span class="sxs-lookup"><span data-stu-id="04e8c-152">As the team gains confidence in the new version, it can roll out more instances and retire the old.</span></span> <span data-ttu-id="04e8c-153">AKS 轻松支持这种部署样式。</span><span class="sxs-lookup"><span data-stu-id="04e8c-153">AKS easily supports this style of deployment.</span></span>

<span data-ttu-id="04e8c-154">与 Azure 中的大多数资源一样，可以使用门户、命令行或自动化工具（例如 Helm 或 Terraform）创建 Azure Kubernetes 服务群集。</span><span class="sxs-lookup"><span data-stu-id="04e8c-154">As with most resources in Azure, you can create an Azure Kubernetes Service cluster using the portal, command-line, or automation tools like Helm or Terraform.</span></span> <span data-ttu-id="04e8c-155">若要开始使用新群集，需要提供以下信息：</span><span class="sxs-lookup"><span data-stu-id="04e8c-155">To get started with a new cluster, you need to provide the following information:</span></span>

- <span data-ttu-id="04e8c-156">Azure 订阅</span><span class="sxs-lookup"><span data-stu-id="04e8c-156">Azure subscription</span></span>
- <span data-ttu-id="04e8c-157">资源组</span><span class="sxs-lookup"><span data-stu-id="04e8c-157">Resource group</span></span>
- <span data-ttu-id="04e8c-158">Kubernetes 群集名称</span><span class="sxs-lookup"><span data-stu-id="04e8c-158">Kubernetes cluster name</span></span>
- <span data-ttu-id="04e8c-159">区域</span><span class="sxs-lookup"><span data-stu-id="04e8c-159">Region</span></span>
- <span data-ttu-id="04e8c-160">Kubernetes 版本</span><span class="sxs-lookup"><span data-stu-id="04e8c-160">Kubernetes version</span></span>
- <span data-ttu-id="04e8c-161">DNS 名称前缀</span><span class="sxs-lookup"><span data-stu-id="04e8c-161">DNS name prefix</span></span>
- <span data-ttu-id="04e8c-162">节点大小</span><span class="sxs-lookup"><span data-stu-id="04e8c-162">Node size</span></span>
- <span data-ttu-id="04e8c-163">节点计数</span><span class="sxs-lookup"><span data-stu-id="04e8c-163">Node count</span></span>

<span data-ttu-id="04e8c-164">此信息足以开始。</span><span class="sxs-lookup"><span data-stu-id="04e8c-164">This information is sufficient to get started.</span></span> <span data-ttu-id="04e8c-165">作为 Azure 门户中创建过程的一部分，你还可以配置以下群集功能的选项：</span><span class="sxs-lookup"><span data-stu-id="04e8c-165">As part of the creation process in the Azure portal, you can also configure options for the following features of your cluster:</span></span>

- <span data-ttu-id="04e8c-166">缩放</span><span class="sxs-lookup"><span data-stu-id="04e8c-166">Scale</span></span>
- <span data-ttu-id="04e8c-167">身份验证</span><span class="sxs-lookup"><span data-stu-id="04e8c-167">Authentication</span></span>
- <span data-ttu-id="04e8c-168">网络</span><span class="sxs-lookup"><span data-stu-id="04e8c-168">Networking</span></span>
- <span data-ttu-id="04e8c-169">监视</span><span class="sxs-lookup"><span data-stu-id="04e8c-169">Monitoring</span></span>
- <span data-ttu-id="04e8c-170">Tags</span><span class="sxs-lookup"><span data-stu-id="04e8c-170">Tags</span></span>

<span data-ttu-id="04e8c-171">本 [快速入门演示如何使用 Azure 门户部署 AKS 群集](/azure/aks/kubernetes-walkthrough-portal)。</span><span class="sxs-lookup"><span data-stu-id="04e8c-171">This [quickstart walks through deploying an AKS cluster using the Azure portal](/azure/aks/kubernetes-walkthrough-portal).</span></span>

## <a name="azure-dev-spaces"></a><span data-ttu-id="04e8c-172">Azure Dev Spaces</span><span class="sxs-lookup"><span data-stu-id="04e8c-172">Azure Dev Spaces</span></span>

<span data-ttu-id="04e8c-173">云本机应用程序可以快速增长大而复杂，需要大量计算资源才能运行。</span><span class="sxs-lookup"><span data-stu-id="04e8c-173">Cloud-native applications can quickly grow large and complex, requiring significant compute resources to run.</span></span> <span data-ttu-id="04e8c-174">在这些情况下，整个应用程序不能在开发计算机上承载 (尤其是便携式计算机) 。</span><span class="sxs-lookup"><span data-stu-id="04e8c-174">In these scenarios, the entire application can't be hosted on a development machine (especially a laptop).</span></span> <span data-ttu-id="04e8c-175">Azure Dev Spaces 旨在使用 AKS 解决此问题。</span><span class="sxs-lookup"><span data-stu-id="04e8c-175">Azure Dev Spaces is designed to address this problem using AKS.</span></span> <span data-ttu-id="04e8c-176">它使开发人员能够在 AKS 开发群集中托管应用程序的其余部分，并使用其服务的本地版本。</span><span class="sxs-lookup"><span data-stu-id="04e8c-176">It enables developers to work with a local version of their services while hosting the rest of the application in an AKS development cluster.</span></span>

<span data-ttu-id="04e8c-177">开发人员在 AKS 群集中共享正在运行 (开发) 实例，其中包含整个容器化应用程序。</span><span class="sxs-lookup"><span data-stu-id="04e8c-177">Developers share a running (development) instance in an AKS cluster that contains the entire containerized application.</span></span> <span data-ttu-id="04e8c-178">但它们使用在计算机上设置的个人空间来本地开发其服务。</span><span class="sxs-lookup"><span data-stu-id="04e8c-178">But they use personal spaces set up on their machine to locally develop their services.</span></span> <span data-ttu-id="04e8c-179">准备就绪后，它们将在 AKS 群集中从端到端测试-无需复制依赖项。</span><span class="sxs-lookup"><span data-stu-id="04e8c-179">When ready, they test from end-to-end in the AKS cluster - without replicating dependencies.</span></span> <span data-ttu-id="04e8c-180">Azure Dev Spaces 将本地计算机中的代码合并到 AKS 中的服务。</span><span class="sxs-lookup"><span data-stu-id="04e8c-180">Azure Dev Spaces merges code from the local machine with services in AKS.</span></span> <span data-ttu-id="04e8c-181">开发人员可使用 Visual Studio 或 Visual Studio Code 直接在 Kubernetes 中快速循环访问和调试代码。</span><span class="sxs-lookup"><span data-stu-id="04e8c-181">Developers can rapidly iterate and debug code directly in Kubernetes using Visual Studio or Visual Studio Code.</span></span>

<span data-ttu-id="04e8c-182">为了理解 Azure Dev Spaces 的价值，让我在 Microsoft Azure 的容器的 Gabe Monroy 中共享此报价：</span><span class="sxs-lookup"><span data-stu-id="04e8c-182">To understand the value of Azure Dev Spaces, let me share this quotation from Gabe Monroy, PM Lead of Containers at Microsoft Azure:</span></span>

> <span data-ttu-id="04e8c-183">"假设您是一名新员工，尝试修复由数十个组件组成的复杂的微服务应用程序中的 bug，每个组件都有其自己的配置和支持服务。</span><span class="sxs-lookup"><span data-stu-id="04e8c-183">"Imagine you're a new employee trying to fix a bug in a complex microservices application consisting of dozens of components, each with their own configuration and backing services.</span></span> <span data-ttu-id="04e8c-184">若要开始，必须配置本地开发环境，使其可以模拟生产环境，包括设置 IDE、构建工具链、容器化服务依赖项、本地 Kubernetes 环境、模拟用于支持服务等。</span><span class="sxs-lookup"><span data-stu-id="04e8c-184">To get started, you must configure your local development environment so that it can mimic production including setting up your IDE, building tool chain, containerized service dependencies, a local Kubernetes environment, mocks for backing services, and more.</span></span> <span data-ttu-id="04e8c-185">在设置开发环境的过程中，解决第一个 bug 可能需要数天的时间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-185">With all the time involved setting up your development environment, fixing that first bug could take days.</span></span>
> <span data-ttu-id="04e8c-186">或者，可以使用 Dev Spaces 和 AKS。 "</span><span class="sxs-lookup"><span data-stu-id="04e8c-186">Or you could use Dev Spaces and AKS."</span></span>

<span data-ttu-id="04e8c-187">使用 Azure Dev Spaces 的过程涉及以下步骤：</span><span class="sxs-lookup"><span data-stu-id="04e8c-187">The process for working with Azure Dev Spaces involves the following steps:</span></span>

1. <span data-ttu-id="04e8c-188">创建 dev 空间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-188">Create the dev space.</span></span>
2. <span data-ttu-id="04e8c-189">配置根开发人员空间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-189">Configure the root dev space.</span></span>
3. <span data-ttu-id="04e8c-190">为您自己的系统版本)  (配置子开发人员空间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-190">Configure a child dev space (for your own version of the system).</span></span>
4. <span data-ttu-id="04e8c-191">连接到 dev 空间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-191">Connect to the dev space.</span></span>

<span data-ttu-id="04e8c-192">所有这些步骤都可以使用 Azure CLI 和新的  `azds` 命令行工具来执行。</span><span class="sxs-lookup"><span data-stu-id="04e8c-192">All of these steps can be performed using the Azure CLI and new  `azds` command-line tools.</span></span> <span data-ttu-id="04e8c-193">例如，若要为给定的 Kubernetes 群集创建新的 Azure Dev 空间，请使用如下所示的命令：</span><span class="sxs-lookup"><span data-stu-id="04e8c-193">For example, to create a new Azure Dev Space for a given Kubernetes cluster, you would use a command like this one:</span></span>

```azurecli
az aks use-dev-spaces -g my-aks-resource-group -n MyAKSCluster
```

<span data-ttu-id="04e8c-194">接下来，可以使用 `azds prep` 命令生成用于运行应用程序的必需 Docker 和 Helm 图表资产。</span><span class="sxs-lookup"><span data-stu-id="04e8c-194">Next, you can use the `azds prep` command to generate the necessary Docker and Helm chart assets for running the application.</span></span> <span data-ttu-id="04e8c-195">然后，使用在 AKS 中运行你的代码 `azds up` 。</span><span class="sxs-lookup"><span data-stu-id="04e8c-195">Then you run your code in AKS using `azds up`.</span></span> <span data-ttu-id="04e8c-196">首次运行此命令时，将安装 Helm 图表。</span><span class="sxs-lookup"><span data-stu-id="04e8c-196">The first time you run this command, the Helm chart will be installed.</span></span> <span data-ttu-id="04e8c-197">容器将根据说明进行生成和部署。</span><span class="sxs-lookup"><span data-stu-id="04e8c-197">The containers will be built and deployed according to your instructions.</span></span> <span data-ttu-id="04e8c-198">此任务在首次运行时可能需要几分钟时间。</span><span class="sxs-lookup"><span data-stu-id="04e8c-198">This task may take a few minutes the first time it's run.</span></span> <span data-ttu-id="04e8c-199">但是，在进行更改后，可以使用连接到自己的子开发人员空间， `azds space select` 然后在隔离的子开发人员空间中部署和调试更新。</span><span class="sxs-lookup"><span data-stu-id="04e8c-199">However, after you make changes, you can connect to your own child dev space using `azds space select` and then deploy and debug your updates in your isolated child dev space.</span></span> <span data-ttu-id="04e8c-200">开发人员空间启动并运行后，可以通过重新发出命令向其发送更新， `azds up` 或者在 Visual Studio 中使用内置工具或 Visual Studio Code。</span><span class="sxs-lookup"><span data-stu-id="04e8c-200">Once you have your dev space up and running, you can send updates to it by reissuing the `azds up` command or you can use built-in tooling in Visual Studio or Visual Studio Code.</span></span> <span data-ttu-id="04e8c-201">使用 VS Code，可以使用命令面板连接到开发环境。</span><span class="sxs-lookup"><span data-stu-id="04e8c-201">With VS Code, you use the command palette to connect to your dev space.</span></span> <span data-ttu-id="04e8c-202">图3-12 演示了如何使用 Visual Studio 中的 Azure Dev Spaces 启动 web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="04e8c-202">Figure 3-12 shows how to launch your web application using Azure Dev Spaces in Visual Studio.</span></span>

<span data-ttu-id="04e8c-203">![在 Visual Studio 中连接到 Azure Dev Spaces ](./media/azure-dev-spaces-visual-studio-launchsettings.png)
 **图 3-12**。</span><span class="sxs-lookup"><span data-stu-id="04e8c-203">![Connect to Azure Dev Spaces in Visual Studio](./media/azure-dev-spaces-visual-studio-launchsettings.png)
**Figure 3-12**.</span></span> <span data-ttu-id="04e8c-204">在 Visual Studio 中连接到 Azure Dev Spaces</span><span class="sxs-lookup"><span data-stu-id="04e8c-204">Connect to Azure Dev Spaces in Visual Studio</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="04e8c-205">[上一页](combine-containers-serverless-approaches.md)
>[下一页](scale-containers-serverless.md)</span><span class="sxs-lookup"><span data-stu-id="04e8c-205">[Previous](combine-containers-serverless-approaches.md)
[Next](scale-containers-serverless.md)</span></span>
