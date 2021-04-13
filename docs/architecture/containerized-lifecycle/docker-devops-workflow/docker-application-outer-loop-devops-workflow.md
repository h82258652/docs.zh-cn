---
title: Docker 应用程序的外部循环 DevOps 工作流步骤
description: 了解 DevOps 工作流的“外部循环”步骤
ms.date: 01/06/2021
ms.openlocfilehash: 8bca36d5aa0fef95d684a96a5c6017ec15956358
ms.sourcegitcommit: 5ce37699c2a51ed173171813be68ef7577b1aba5
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104881089"
---
# <a name="steps-in-the-outer-loop-devops-workflow-for-a-docker-application"></a>Docker 应用程序的外部循环 DevOps 工作流步骤

图 5-1 显示了包含 DevOps 外部循环工作流的步骤的端到端描述。 它显示了 DevOps 的“外部循环”。 将代码推送到存储库时，将启动 CI 管道，然后启动 CD 管道，在该管道中部署应用程序。 从已部署的应用程序中收集的指标将反馈到在其中发生“内部循环”的开发工作负载中，因此开发团队拥有实际数据来响应用户和业务需求。

![显示 DevOps 外部循环工作流的 6 个步骤的关系图。](./media/docker-application-outer-loop-devops-workflow/overview-dev-ops-outter-loop-workflow.png)

**图 5-1**。 使用 Microsoft 工具的 Docker 应用程序的 DevOps 外部循环工作流

现在，让我们更加详细地来研究这些步骤。

## <a name="step-1-inner-loop-development-workflow"></a>步骤 1：内部循环开发工作流

第 4 章详细介绍了此步骤，但概括来说，这是外部循环开始的地方，即开发人员将代码推送到源代码控制管理系统（如 Git）以启动 CI 管道操作的时刻。

## <a name="step-2-source-code-control-integration-and-management-with-azure-devops-services-and-git"></a>步骤 2：使用 Azure DevOps Services 和 Git 进行源代码控制集成和管理

在此步骤中，你需要一个版本控制系统来收集来自团队中不同开发人员的所有代码的统一版本。

尽管源代码控制 (SCC) 和源代码管理对于大多数开发人员来说似乎是次要的，但在 DevOps 生命周期中创建 Docker 应用程序时，必须强调，不能从开发人员的计算机上直接将带有应用程序的 Docker 映像提交到全局 Docker 注册表（如 Azure 容器注册表或 Docker Hub）。 相反，要发布和部署到生产环境中的 Docker 映像必须仅在基于源代码存储库（如 Git）集成到全局生成或 CI 管道的源代码上创建。

由开发人员生成的本地映像应仅供他们在自己的计算机中进行测试时使用。 这就是为什么从 SCC 代码激活 DevOps 管道至关重要的原因。

Azure DevOps Services 和 Team Foundation Server 支持 Git 和 Team Foundation 版本控制。 你可在它们之间进行选择，并将其用于端到端 Microsoft 体验。 但是，你还可以在外部存储库（如 GitHub、本地 Git 存储库或 Subversion）中管理代码，并且仍然能够连接到该存储库并获取代码作为 DevOps CI 管道的起点。

## <a name="step-3-build-ci-integrate-and-test-with-azure-devops-services-and-docker"></a>步骤 3：使用 Azure DevOps Services 和 Docker 生成、CI、集成和测试

CI 已成为新式软件测试和交付的标准。 Docker 解决方案在开发和运营团队之间保持着明确的关注点分离。 Docker 映像的不变性确保了在通过 CI 开发、测试的内容和在生产中运行的内容之间进行可重复部署。 跨开发人员便携式计算机和测试基础架构部署的 Docker 引擎使容器可以跨环境移植。

此时，在拥有提交了正确代码的版本控制系统后，需要“生成服务”来获取代码并运行全局生成和测试  。

此步骤（CI、构建、测试）的内部工作流关于 CI 管道的生成，该管道由代码存储库（Git 等）、生成服务器 (Azure DevOps Services)、Docker 引擎和 Docker 注册表组成。

可以使用 Azure DevOps Services 作为生成应用程序和设置 CI 管道的基础，并将生成的“项目”发布到“项目存储库”（这将在下一步中进行说明）。

当使用 Docker 进行部署时，要部署的“最终项目”是其中嵌入了应用程序或服务的 Docker 映像。 这些映像被推送或发布到 Docker 注册表（一个如 Azure 容器注册表中的专用存储库，或者像 Docker Hub 注册表这样的公共存储库，通常用于官方基础映像）  。

下面是一个基本概念：CI 管道将通过到 Git 等的 SCC 存储库的提交来启动。 提交将导致 Azure DevOps Services 在 Docker 容器中运行生成作业，并在该作业成功完成后，将 Docker 映像推送到 Docker 注册表，如图 5-2 所示。 外部循环的第一部分涉及步骤 1 至 3，从代码、运行、调试和验证，到代码存储库，再一直到生成和测试 CI 步骤。

![显示 CI 工作流中所涉及的三个步骤的关系图。](./media/docker-application-outer-loop-devops-workflow/continuous-integration-steps.png)

**图 5-2**。 CI 中涉及的步骤

以下是使用 Docker 和 Azure DevOps Services 的基本 CI 工作流步骤：

1. 开发人员将提交推送到 SCC 存储库（Git/Azure DevOps Services、GitHub 等）。

2. 如果你使用的是 Azure DevOps Services 或 Git，则已内置 CI，这意味着它与在 Azure DevOps Services 中选择复选框一样简单。 如果使用外部 SCC（如 GitHub），则 `webhook` 将通知 Azure DevOps Services 更新或推送到 Git/GitHub。

3. Azure DevOps Services 拉取 SCC 存储库，包括描述映像的 Dockerfile，以及应用程序和测试代码。

4. Azure DevOps Services 生成 Docker 映像并使用生成号对其进行标记。

5. Azure DevOps Services 在配置的 Docker 主机中实例化 Docker 容器，并运行相应的测试。

6. 如果测试成功，则首先将映像重新标记为有意义的名称，以便了解它是一个“幸运的生成”（类似于“/1.0.0”或任何其他标签），然后将其推送到 Docker 注册表（Docker Hub、Azure 容器注册表、DTR 等）

### <a name="implementing-the-ci-pipeline-with-azure-devops-services-and-the-docker-extension-for-azure-devops-services"></a>使用 Azure DevOps Services 和 Azure DevOps Services 的 Docker 扩展实现 CI 管道

Visual Studio Azure DevOps Services 包含可在 CI/CD 管道中使用的生成和发布模板，可以使用这些模板生成 Docker 映像、将 Docker 映像推送到经过身份验证的 Docker 注册表、运行 Docker 映像或运行 Docker CLI 提供的其他操作。 它还添加了 Docker Compose 任务，可使用该任务来生成、推送和运行多容器 Docker 应用程序，或者运行 Docker Compose CLI 提供的其他操作，如图 5-3 所示。

![Azure DevOps 中 Docker CI 管道的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/docker-ci-pipeline-azure-devops.png)

**图 5-3**。 Azure DevOps Services 中的 Docker CI 管道，包括生成和发布模板以及相关任务。

可以使用这些模板和任务构建 CI/CD 项目，以在 Azure Service Fabric、Azure Kubernetes 服务和类似产品/服务中生成/测试和部署。

通过这些 Visual Studio Team Services 任务、在 Azure 中预配的生成 Linux Docker 主机/VM 和首选 Docker 注册表（Azure 容器注册表、Docker Hub、专用 Docker DTR 或任何其他 Docker 注册表），你可以非常一致的方式来组装 Docker CI 管道。

***要求：***

- Azure DevOps Services，或用于本地安装的 Team Foundation Server 2015 Update 3 或更高版本。

- 具有 Docker 二进制文件的 Azure DevOps Services 代理。

  创建其中某个代理的简单方法是使用 Docker 运行基于 Azure DevOps Services 代理 Docker 映像的容器。

> [!TIP]
> 要了解有关组装 Azure DevOps Services Docker CI 管道的详细信息和查看演练，请访问以下站点：
>
> - 将 Visual Studio Team Services（现在是 Azure DevOps Services）代理作为 Docker 容器运行：\
>   <https://hub.docker.com/_/microsoft-azure-pipelines-vsts-agent>
>
> - 使用 Azure DevOps Services 生成 .NET Linux Docker 映像：
>   <https://docs.microsoft.com/archive/blogs/stevelasker/building-net-core-linux-docker-images-with-visual-studio-team-services>
>
> - 使用 Docker 支持生成基于 Linux 的 Visual Studio Team Service 生成计算机：\
>   <https://www.donovanbrown.com/post/Building-a-Linux-Based-Visual-Studio-Team-Service-Build-Machine-with-Docker-Support>

### <a name="integrate-test-and-validate-multi-container-docker-applications"></a>集成、测试和验证多容器 Docker 应用程序

通常，大多数 Docker 应用程序由多个容器而不是单个容器组成。 一个很好的示例就是每个微服务有一个容器的面向微服务的应用程序。 但是，即使没有严格遵循微服务方法模式，Docker 应用程序也可能由多个容器或服务组成。

因此，在 CI 管道中生成应用程序容器之后，还需要将应用程序与其在集成 Docker 主机中、甚至在容器分布的测试群集中的所有容器作为一个整体进行部署、集成和测试。

如果使用的是单个主机，则可以使用 Docker 命令（如 docker-compose）来生成和部署相关容器，以在单个 VM 中测试和验证 Docker 环境。 但是，如果你正在使用 DC/OS、Kubernetes 或 Docker Swarm 等业务流程协调程序群集，则需要通过不同的机制或业务流程协调程序部署容器，具体取决于所选择的群集/计划程序。

以下是可以针对 Docker 容器运行的几种类型的测试：

- Docker 容器的单元测试

- 测试相互关联的应用程序或微服务组

- 在生产和“canary”发布中进行测试

重要的是，在运行集成和功能测试时，必须从容器外部运行这些测试。 测试不包含在你正在部署的容器中或者在其中运行，因为容器基于应与将要部署到生产中的映像完全相同的静态映像。

在测试更高级的方案（例如包含多个群集（测试群集、暂存群集和生产群集））时，一个实用选项是将映像发布到注册表，以便可以在不同群集中对其进行测试。

### <a name="push-the-custom-application-docker-image-into-your-global-docker-registry"></a>将自定义应用程序 Docker 映像推送到全局 Docker 注册表中

在对 Docker 映像进行测试和验证后，需要将它们标记并发布到 Docker 注册表中。 Docker 注册表是 Docker 应用程序生命周期中的一个关键部分，因为它是存储要部署到 QA 和生产环境中的自定义测试（也称为“幸运映像”）的中心位置。

与存储在 SCC 存储库（Git 等）中的应用程序代码是你的“事实来源”类似，Docker 注册表是要部署到 QA 或生产环境中的二进制应用程序或位的“事实来源”。

通常，你可能想要在 Azure 容器注册表中的专用存储库中或 Docker 受信注册表等本地注册表中，或在具有有限访问权限的公共云注册表（如 Docker Hub）中，保留自定义映像的专用存储库，虽然在最后一种情况下，如果代码不是开放源代码，则必须信任供应商的安全性。 不管怎样，所使用的方法都类似，并且基于 `docker push` 命令，如图 5-4 所示。

![显示将自定义映像推送到容器注册表的关系图。](./media/docker-application-outer-loop-devops-workflow/docker-push-custom-images.png)

**图 5-4**。 将自定义映像发布到 Docker 注册表

在步骤 3 中，对于生成集成和测试 (CI)，可以将生成的 Docker 映像发布到专用或公共注册表。 云供应商提供了多种 Docker 注册表产品/服务，如 Azure 容器注册表、Amazon Web Services 容器注册表、Google 容器注册表、Quay 注册表等。

使用 Docker 任务，可以将具有多个标记的 `docker-compose.yml` 文件定义的一组服务映像推送到经过身份验证的 Docker 注册表（如 Azure 容器注册表），如图 5-5 所示。

![显示将映像发布到注册表的步骤的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/publish-custom-image-to-docker-registry.png)

**图 5-5**。 使用 Azure DevOps Services 将自定义映像发布到 Docker 注册表

> [!TIP]
> 有关 Azure 容器注册表的详细信息，请参阅 <https://aka.ms/azurecontainerregistry>。

## <a name="step-4-cd-deploy"></a>步骤 4：CD、部署

Docker 映像的不变性确保了使用通过 CI 开发、测试的内容和在生产中运行的内容进行可重复部署。 在 Docker 注册表（专用或公共）中发布应用程序 Docker 映像之后，可以使用 Azure DevOps Services 管道任务或 Azure DevOps Services Release Management，将它们从 CD 管道部署到你可能具有的几个环境中（生产、QA、暂存等）。

但是，此时它取决于正在部署的 Docker 应用程序类型。 部署简单的应用程序（例如包含几个容器或服务并部署到几个服务器或 VM 的整体式应用程序）（从组合和部署的角度来看）不同于部署更加复杂的应用程序（例如具有超大规模功能的面向微服务的应用程序）。 以下部分将介绍这两种情况。

### <a name="deploying-composed-docker-applications-to-multiple-docker-environments"></a>将组合的 Docker 应用程序部署到多个 Docker 环境

让我们首先来看一下不太复杂的情况：部署到单个环境或多个环境（QA、暂存和生产）中的简单 Docker 主机（VM 或服务器）。 在此情况下，在内部，CD 管道可以使用 docker-compose（来自 Azure DevOps Services 部署任务）来部署 Docker 应用程序及其相关容器或服务集，如图 5-6 所示。

![显示部署到三个环境的 CD 部署步骤的示意图。](./media/docker-application-outer-loop-devops-workflow/deploy-app-containers-to-docker-host-environments.png)

**图 5-6**。 将应用程序容器部署到简单的 Docker 主机环境注册表

图 5-7 突出显示了如何通过单击“添加任务”对话框中的“Docker Compose”，通过 Azure DevOps Services 将生成 CI 连接到 QA/测试环境。 但是，在部署到暂存或生产环境时，通常会使用 Release Management 功能处理多个环境（如 QA、暂存和生产）。 如果要部署到单个 Docker 主机，它将使用 Azure DevOps Services“Docker Compose”任务（在内部调用 `docker-compose up` 命令）。 如果要部署到 Azure Kubernetes 服务 (AKS)，它将使用 Docker 部署任务，如以下部分所述。

![显示 Docker Compose 任务的“添加任务”对话框的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/add-tasks-docker-compose.png)

**图 5-7**。 在 Azure DevOps Services 管道中添加 Docker Compose 任务

在 Azure DevOps Services 中创建发布时，它会采用一组输入项目。 这些项目确定在发布生存期内在所有环境中都不可变。 引入容器时，输入项目会标识要部署的注册表中的映像。 根据这些映像的标识方式，无法保证它们在整个发布过程中保持不变，最明显的情况就是在从 `docker-compose` 文件引用 `myimage:latest` 时。

Azure DevOps Services 模板使你能够生成包含特定注册表映像摘要的生成项目，这些摘要可保证唯一标识相同映像二进制文件。 这些是你真正想要用作发布的输入的内容。

### <a name="managing-releases-to-docker-environments-by-using-azure-devops-services-release-management"></a>使用 Azure DevOps Services Release Management 管理到 Docker 环境的发布

通过 Azure DevOps Services 模板，可以生成新映像，将该映像发布到 Docker 注册表，在 Linux 或 Windows 主机上运行该映像，以及使用 `docker-compose` 等命令将多个容器部署为整个应用程序，所有这些都通过适用于多个环境的 Azure DevOps Services Release Management 功能实现，如图 5-8 所示。

![显示 Docker 撰写版配置的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/configure-docker-compose-release.png)

**图 5-8**。 从 Azure DevOps Services Release Management 配置 Azure DevOps Services Docker Compose 任务

但是，请记住，图 5-6 中所示并于图 5-8 中实现的方案是一种简单方案（它部署到单个 Docker 主机和 VM，每个映像将有一个容器或实例），并且可能应仅用于开发或测试方案。 在大多数企业生产方案中，你可能想要通过跨多个节点、服务器和 VM 的负载均衡以及“智能故障转移”实现高可用性 (HA) 和易于管理的可伸缩性，以便在服务器或节点发生故障时，其服务和容器将移动到另一主机服务器或 VM。 此情况需要更高级的技术，例如容器群集、业务流程协调程序和计划程序。 因此，部署到这些群集的方法是处理下一部分中介绍的高级方案。

### <a name="deploying-docker-applications-to-docker-clusters"></a>将 Docker 应用程序部署到 Docker 群集

分布式应用程序的本质需要也是分布式的计算资源。 要拥有生产规模功能，需要具有基于池资源提供高可伸缩性和高可用性的群集功能。

可从 CLI 工具或 Web UI 手动将容器部署到这些群集，但应保留这种手动工作，以发现部署测试或管理目的，例如横向扩展或监视。

从 CD（尤其是 Azure DevOps Services）的角度来看，可从 Azure DevOps Services Release Management 环境运行特制的部署任务，这些环境将容器化应用程序部署到容器服务中的分布式群集，如图 5-9 所示。

![显示部署到业务流程协调程序的 CD 部署步骤的示意图。](./media/docker-application-outer-loop-devops-workflow/cd-deploy-to-orchestrators.png)

**图 5-9**。 将分布式应用程序部署到容器服务

最初，当部署到特定群集或业务流程协调程序时，通常会为每个业务流程协调程序使用特定的部署脚本和机制（即 Kubernetes 和 Service Fabric 具有不同部署机制），而不是使用基于 `docker-compose.yml` 定义文件的简单易用的 `docker-compose` 工具。 但是，由于图 5-10 所示的 Azure DevOps Services Docker 部署任务，现在还可以通过使用熟悉的 `docker-compose.yml` 文件部署到受支持的业务流程协调程序，因为该工具会为你执行该“转换”（从 `docker-compose.yml` 文件转换为业务流程协调程序所需的格式）。

![显示“部署到 Kubernetes”任务的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/add-deploy-to-kubernetes-task.png)

**图 5-10**。 将“部署到 Kubernetes”任务添加到环境

图 5-11 演示了如何使用可用于配置的部分编辑“部署到 Kubernetes”任务。 此任务将检索要在群集中作为容器部署的即用型自定义 Docker 映像。

![显示“部署到 Kubernetes”任务配置的屏幕截图。](./media/docker-application-outer-loop-devops-workflow/edit-deploy-to-kubernetes-task.png)

**图 5-11**。 部署到 ACS DC/OS 的 Docker 部署任务定义

> [!TIP]
> 如需了解有关 Azure DevOps Services 和 Docker 的 CD 管道的更多信息，请访问 <https://azure.microsoft.com/services/devops/pipelines>

## <a name="step-5-run-and-manage"></a>步骤 5：运行和管理

因为在企业生产级别运行和管理应用程序本身就是一个重要主题，并且由于操作类型和在该级别（IT 操作）工作的人员以及这一领域的广泛范围，整个下一章节将对其进行专门的介绍。

## <a name="step-6-monitor-and-diagnose"></a>步骤 6：监视和诊断

下一章节还会将此主题作为 IT 在生产系统中执行的任务的一部分进行介绍；但是，务必要强调的是，在此步骤中获得的见解必须反馈给开发团队，以便不断改进应用程序。 从该角度来看，它也是 DevOps 的一部分，尽管任务和操作通常由 IT 执行。

只有 100% 在 DevOps 领域内进行监视和诊断时，开发团队才会针对测试或 beta 环境执行监视流程和分析。 这可以通过执行负载测试或监视 beta 或 QA 环境（其中 beta 测试人员正在尝试新版本）来完成。

>[!div class="step-by-step"]
>[上一页](index.md)
>[下一页](create-ci-cd-pipelines-azure-devops-services-aspnetcore-kubernetes.md)
