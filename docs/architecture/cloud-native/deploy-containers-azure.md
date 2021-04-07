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
# <a name="deploying-containers-in-azure"></a>在 Azure 中部署容器

本章节和第1章讨论了容器。 我们发现，容器为云本机应用程序提供了许多好处，包括可移植性。 在 Azure 云中，你可以在过渡环境和生产环境中部署相同的容器化服务。 Azure 提供了多个用于托管容器化工作负荷的选项：

- Azure Kubernetes 服务 (AKS)
- Azure 容器实例 (ACI)
- 适用于容器的 Azure Web 应用

## <a name="azure-container-registry"></a>Azure 容器注册表

容器化微服务时，首先构建一个容器 "image"。 此图像是服务代码、依赖项和运行时的二进制表示形式。 尽管可以使用 `Docker Build` DOCKER API 中的命令手动创建映像，但更好的方法是将其创建为自动生成过程的一部分。

创建后，容器映像存储在容器注册表中。 它们使你能够生成、存储和管理容器映像。 有很多可用的注册表项，无论是公共的还是私有的。 Azure 容器注册表 (ACR) 是 Azure 云中完全托管的容器注册表服务。 它将映像保存在 Azure 网络中，缩短将其部署到 Azure 容器主机的时间。 还可以使用与其他 Azure 资源相同的安全和标识过程来保护这些资源。

使用 [Azure 门户](/azure/container-registry/container-registry-get-started-portal)、 [Azure CLI](/azure/container-registry/container-registry-get-started-azure-cli)或 [PowerShell 工具](/azure/container-registry/container-registry-get-started-powershell)创建 Azure 容器注册表。 在 Azure 中创建注册表非常简单。 它需要 Azure 订阅、资源组和唯一名称。 图3-11 显示了用于创建注册表的基本选项，这些选项将在上托管 `registryname.azurecr.io` 。

![创建容器注册表](./media/create-container-registry.png)

**图 3-11**。 创建容器注册表

创建注册表后，需要先对其进行身份验证，然后才能使用。 通常，你将使用 Azure CLI 命令登录到注册表：

```azurecli
az acr login --name *registryname*
```

经过身份验证后，可以使用 docker 命令将容器映像推送到其中。 但是，在执行此操作之前，必须用 ACR 登录服务器的完全限定名 (URL) 来标记映像。 它的格式为 *registryname*. azurecr.io。

```console
docker tag mycontainer myregistry.azurecr.io/mycontainer:v1
```

标记该映像后，使用 `docker push` 命令将该映像推送到 ACR 实例。

```console
docker push myregistry.azurecr.io/mycontainer:v1
```

将映像推送到注册表后，最好使用以下命令从本地 Docker 环境中删除映像：

```console
docker rmi myregistry.azurecr.io/mycontainer:v1
```

作为最佳做法，开发人员不应手动将映像推送到容器注册表。 相反，在 GitHub 或 Azure DevOps 之类的工具中定义的生成管道应负责此过程。 有关详细信息，请参阅 [云-Native DevOps 章节](devops.md)。

## <a name="acr-tasks"></a>ACR 任务

[ACR 任务](/azure/container-registry/container-registry-tasks-overview) 是一组 Azure 容器注册表中可用的功能。 它通过在 Azure 云中构建和管理容器映像来扩展你的 [内部循环开发周期](../containerized-lifecycle/design-develop-containerized-apps/docker-apps-inner-loop-workflow.md) 。 它们不是在 `docker build` `docker push` 开发计算机上进行调用，而是由云中的 ACR 任务自动处理的。

以下 AZ CLI 命令都生成一个容器映像并将其推送到 ACR：

```azurecli
# create a container registry
az acr create --resource-group myResourceGroup --name myContainerRegistry008 --sku Basic

# build container image in ACR and push it into your container regsitry
az acr build --image sample/hello-world:v1  --registry myContainerRegistry008 --file Dockerfile .
```

如前面的命令块中所示，无需在开发计算机上安装 Docker 桌面。 此外，还可以配置 ACR 任务触发器，以便在源代码和基本映像更新上重新生成容器映像。

## <a name="azure-kubernetes-service"></a>Azure Kubernetes 服务

我们在本章中讨论了 Azure Kubernetes Service (AKS) 。 我们已了解到，事实容器 orchestrator 正在管理容器化的云本机应用程序。

将映像部署到注册表（如 ACR）后，可将 AKS 配置为自动拉取和部署。 使用 CI/CD 管道时，你可以配置一种不限的  [发布](https://martinfowler.com/bliki/CanaryRelease.html) 策略，以最大程度地减少快速部署更新时所涉及的风险。 新版本的应用程序最初在生产环境中进行了配置，但未将流量路由到该应用程序。 然后，系统会将少量用户路由到新部署的版本。 随着团队在新版本中获得信心，它可以推出更多实例并停用旧版本。 AKS 轻松支持这种部署样式。

与 Azure 中的大多数资源一样，可以使用门户、命令行或自动化工具（例如 Helm 或 Terraform）创建 Azure Kubernetes 服务群集。 若要开始使用新群集，需要提供以下信息：

- Azure 订阅
- 资源组
- Kubernetes 群集名称
- 区域
- Kubernetes 版本
- DNS 名称前缀
- 节点大小
- 节点计数

此信息足以开始。 作为 Azure 门户中创建过程的一部分，你还可以配置以下群集功能的选项：

- 缩放
- 身份验证
- 网络
- 监视
- Tags

本 [快速入门演示如何使用 Azure 门户部署 AKS 群集](/azure/aks/kubernetes-walkthrough-portal)。

## <a name="azure-dev-spaces"></a>Azure Dev Spaces

云本机应用程序可以快速增长大而复杂，需要大量计算资源才能运行。 在这些情况下，整个应用程序不能在开发计算机上承载 (尤其是便携式计算机) 。 Azure Dev Spaces 旨在使用 AKS 解决此问题。 它使开发人员能够在 AKS 开发群集中托管应用程序的其余部分，并使用其服务的本地版本。

开发人员在 AKS 群集中共享正在运行 (开发) 实例，其中包含整个容器化应用程序。 但它们使用在计算机上设置的个人空间来本地开发其服务。 准备就绪后，它们将在 AKS 群集中从端到端测试-无需复制依赖项。 Azure Dev Spaces 将本地计算机中的代码合并到 AKS 中的服务。 开发人员可使用 Visual Studio 或 Visual Studio Code 直接在 Kubernetes 中快速循环访问和调试代码。

为了理解 Azure Dev Spaces 的价值，让我在 Microsoft Azure 的容器的 Gabe Monroy 中共享此报价：

> "假设您是一名新员工，尝试修复由数十个组件组成的复杂的微服务应用程序中的 bug，每个组件都有其自己的配置和支持服务。 若要开始，必须配置本地开发环境，使其可以模拟生产环境，包括设置 IDE、构建工具链、容器化服务依赖项、本地 Kubernetes 环境、模拟用于支持服务等。 在设置开发环境的过程中，解决第一个 bug 可能需要数天的时间。
> 或者，可以使用 Dev Spaces 和 AKS。 "

使用 Azure Dev Spaces 的过程涉及以下步骤：

1. 创建 dev 空间。
2. 配置根开发人员空间。
3. 为您自己的系统版本)  (配置子开发人员空间。
4. 连接到 dev 空间。

所有这些步骤都可以使用 Azure CLI 和新的  `azds` 命令行工具来执行。 例如，若要为给定的 Kubernetes 群集创建新的 Azure Dev 空间，请使用如下所示的命令：

```azurecli
az aks use-dev-spaces -g my-aks-resource-group -n MyAKSCluster
```

接下来，可以使用 `azds prep` 命令生成用于运行应用程序的必需 Docker 和 Helm 图表资产。 然后，使用在 AKS 中运行你的代码 `azds up` 。 首次运行此命令时，将安装 Helm 图表。 容器将根据说明进行生成和部署。 此任务在首次运行时可能需要几分钟时间。 但是，在进行更改后，可以使用连接到自己的子开发人员空间， `azds space select` 然后在隔离的子开发人员空间中部署和调试更新。 开发人员空间启动并运行后，可以通过重新发出命令向其发送更新， `azds up` 或者在 Visual Studio 中使用内置工具或 Visual Studio Code。 使用 VS Code，可以使用命令面板连接到开发环境。 图3-12 演示了如何使用 Visual Studio 中的 Azure Dev Spaces 启动 web 应用程序。

![在 Visual Studio 中连接到 Azure Dev Spaces ](./media/azure-dev-spaces-visual-studio-launchsettings.png)
 **图 3-12**。 在 Visual Studio 中连接到 Azure Dev Spaces

>[!div class="step-by-step"]
>[上一页](combine-containers-serverless-approaches.md)
>[下一页](scale-containers-serverless.md)
