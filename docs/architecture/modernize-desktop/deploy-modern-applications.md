---
title: 部署新式桌面应用程序
description: 部署新式桌面应用程序时需要了解的所有内容。
ms.date: 05/12/2020
ms.openlocfilehash: ba47f09b27adf270734bbfff285fe44dd4175d29
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98615849"
---
# <a name="deploying-modern-desktop-applications"></a>部署新式桌面应用程序

当你开发桌面应用程序时，需要考虑的一个问题是如何打包应用程序并将其部署到用户的计算机上。 打包、部署和安装的问题在于，它通常由 IT 专业人员负责，而 IT 专业人员关注的点与开发人员不同。

如今，我们都很熟悉 DevOps 的概念，在 DevOps 中，开发人员和 IT 专业人员密切合作，将应用程序迁移到生产环境中。 但是，如果你身陷这场桌面战役已经超过 10 年，那么，你可能看到过下面这样的场景。 一支由开发人员组成的团队共同努力，以期按时完成项目。 业务人员很紧张，因为他们需要系统在多台用户计算机上正常运行，以维持公司运营。 在“D 日”，项目经理向每位开发人员确认了他们的代码运行良好，一切运转正常，他们可以交付应用了。 然后，包团队开始生成应用的安装程序，将其分发到每台用户计算机上，一组测试用户开始运行应用程序。 好吧，他们只是试了一下，因为还没显示任何 UI，应用程序就引发一个异常，指出“Method ~ of object ~ failed”。 恐慌情绪开始蔓延，经过简单调查后，矛头指向一个年轻而疲惫的开发人员，他引入了一个第三方控件，这无疑是“在开发计算机上工作”。

安装桌面应用程序历来就是一场噩梦，主要原因有两个：

- 开发团队和 IT 团队之间缺乏紧密的合作文化。
- 缺乏可以依赖的可靠打包和部署技术。

实际上，我们一直生活在这样的事实中，有时候你后悔安装了一个应用，因为：

- 它最终在你的计算机上产生了一些意想不到的副作用。
- 以前安装的某些应用程序停止工作。

此外，你无法通过卸载应用将系统还原到原始状态。 我们对这种情况习以为常，以至于创造出“DLL Hell”或“Winrot”这样的术语。

在本章中，我们将探讨 MSIX。 MSIX 是 Microsoft 推出的全新技术，它试图汲取以往技术的精华，为未来的打包技术打下坚实的基础。

打包技术和现代化有什么关系？ 事实证明，打包是企业 IT 的根本，企业的大量资金便投入于此。 现代化不仅与使用最新技术有关。 它还与缩短上市时间（从定义业务需求到公司向客户交付该功能）有关。

## <a name="the-modern-application-lifecycle"></a>新式应用程序的生命周期

如今，开发人员编写并生成应用代码，然后将生成的资产交给 IT 专业人员。 IT 专业人员通常采用 MSI 或最近的 App-V 打包格式，对应用进行重新配置和重新打包。 然后，应用通过各种渠道和工具进行部署。 这种方法的主要问题之一通常称为“打包瘫痪”。 其问题在于，每次有应用更新或操作系统更新时，都会重复这个打包周期。

你可以看看下图所示的过程：

![该图显示了新式 IT 生命周期](./media/deploy-modern-applications/modern-it-application-lifecycle.png)

公司需要采用一种方法，将这个打包周期分成三个独立的周期：

- OS 更新
- 应用程序更新
- 自定义

![该图显示了新式 IT 良性循环](./media/deploy-modern-applications/modern-it-virtuous-cycle.png)

上图说明你可以：

- 更新底层 OS，而无需重新打包应用。
- 实现 IT 自定义，而无需重新打包原始开发人员包。

这种根本性的变化将我们引向了新式 IT 生命周期，如下图所示：

![该图显示了使用 Microsoft 工具的应用程序生命周期](./media/deploy-modern-applications/microsoft-it-tools.png)

开发人员创建应用并生成 MSIX 包，IT 专业人员可以使用和配置该包，而无需重新打包。 除了 MSIX 技术，Microsoft 还创建了一些工具，让 IT 人员无需重新打包即可自定义和配置包。

## <a name="msix-the-next-generation-of-deployment"></a>MSIX：下一代部署

在 MSIX 之前，存在多种打包技术，例如设置向导、MSI、ClickOnce、App-V 和脚本。 每种技术都有其各自的优势，于是，Microsoft 决定集百家之长来构建 MSIX。 MSIX 建立在这些现有技术的基础上，并取其精华：

- App-V =\> 容器化
- ClickOnce =\> 自动更新
- MSI =\> 易于分发

有了 MSIX，就等于有了一项集这些功能于一身的安装程序技术。

![该图显示了影响 MSIX 构建的各种技术](./media/deploy-modern-applications/msix.png)

### <a name="benefits-of-msix"></a>MSIX 的好处

#### <a name="never-regret-installing-an-app"></a>永远不会后悔安装了某个应用

MSIX 提供可预测、可靠、安全的部署。 包清单中包含的声明性方法使操作系统可以跟踪应用程序所需的每项资产。 它还提供真正的干净卸载，没有任何副作用。

#### <a name="disk-space-optimization"></a>磁盘空间优化

MSIX 经过优化，可减少应用程序对用户计算机磁盘空间的占用。 它会创建文件的单个实例存储。 也就是说，如果你有两个具有相同 DLL 的不同包，则不会安装该 DLL 两次。 该平台解决了这个问题，因为它知道特定应用安装的所有文件，这要归功于它的声明性质。 它还允许你并行运行不同版本的 DLL。

通过使用资源包，你可以轻松创建多语言应用，并且操作系统会安装所使用的应用。

#### <a name="network-optimization"></a>网络优化

MSIX 可在字节块级别检测到文件上的差异，从而实现了一项称为差异更新的功能。 这意味着在应用程序更新时只下载更新的字节块。

![该图显示了 MSIX 如何管理差异更新](./media/deploy-modern-applications/msix-managing-updates.png)

使用流式安装，用户可以在后台下载应用程序其他部分的同时，快速开始使用应用程序。 此功能有助于为用户带来引人入胜的体验。

借助可选包功能，你可以在应用部署上实现组件化，这样你就可以在需要时下载它们。

#### <a name="simple-packaging-and-deployment"></a>简单的打包和部署

AppManifest 以标准方式为每个应用程序声明版本控制、设备定位和标识。 它还提供一种对资产进行签名的方法，从而提供可靠的安全基础。

#### <a name="os-managed"></a>操作系统托管

操作系统负责处理安装、更新和删除应用程序的所有过程。 应用程序按用户安装，但仅下载一次，从而最大程度地减少了磁盘占用。 Microsoft 正致力于在 Windows 7 上也提供 MSIX 体验。

#### <a name="windows-provides-integrity-for-the-app"></a>Windows 为应用提供完整性

使用数字签名，可以确保不会从不受信任的源安装应用程序。 MSIX 还可以防止篡改，因为：

- 它会保留文件哈希的记录。
- 它会检测是否在安装后修改了文件。

#### <a name="works-for-the-entire-app-catalog"></a>适用于整个应用目录

MSIX 最酷的一点是它适用于整个应用程序目录（Windows 窗体、WPF、MFC/ATL、Delphi），即使你想执行 xCopy 部署、ClickOnce 或进入 Store，也可以使用同一个 MSIX 包。

### <a name="tools"></a>工具

#### <a name="windows-application-packaging-project"></a>Windows 应用程序打包项目

你可以使用 Visual Studio 中的 **Windows 应用程序打包项目** 项目为桌面应用生成程序包。 然后，你可以将该包发布到 Microsoft Store，或将其旁加载到一台或多台电脑上。

#### <a name="msix-packaging-tool"></a>MSIX 打包工具

使用 MSIX 打包工具可将现有的 Win32 应用程序重新打包为 MSIX 格式。 此工具提供交互式 UI 和命令行用于转换，并且可以在没有源代码的情况下转换应用程序。

![MSIX 打包工具](./media/deploy-modern-applications/msix-packaging-tool.png)

#### <a name="package-support-framework"></a>包支持框架

包支持框架是一个开源工具包，有助于在无权访问源代码时将修补程序应用于现有 Win32 应用程序，以便其在 MSIX 容器中运行。 包支持框架可帮助应用程序遵循新式运行时环境的最佳做法。

#### <a name="app-installer"></a>应用安装程序

利用应用安装程序，可以通过双击应用包来安装 Windows 10 应用。 这意味着用户无需使用 PowerShell 或其他开发人员工具，即可部署 Windows 10 应用。 应用安装程序还可以从 Web、可选包和相关的集中安装应用。

## <a name="how-to-create-an-msix-package-from-an-existing-win32-desktop-application"></a>如何从现有的 Win32 桌面应用程序创建 MSIX 包

让我们看一下从现有 Win32 应用程序创建 MSIX 包的过程。 在此示例中，我们将使用 Windows 窗体应用。

首先，将一个新项目添加到解决方案中，选择“Windows 应用程序打包项目”，并为其命名。

![添加新的 Windows 应用程序打包项目](./media/deploy-modern-applications/add-packaging-project.png)

你会看到打包项目的结构，并注意到一个名为 Applications 的特殊文件夹。 在此文件夹中，可以指定要包含在包中的应用程序。 可以指定多个。

![打包项目的结构](./media/deploy-modern-applications/packaging-project.png)

右键单击 Applications 文件夹，从 Visual Studio 解决方案中选择要打包的 Windows 窗体项目。

![将 Windows 窗体项目添加到打包项目中](./media/deploy-modern-applications/add-winforms-project.png)

此时，你可以编译并生成包，但我们要确认几件事。 为了获得更好的用户体验，Visual Studio 可以自动生成新式应用程序处理磁贴栏和开始菜单的图标和磁贴资产所需的所有视觉资产。 打开 Package.appxmanifest 文件以访问清单设计器。 然后，只需单击“创建”，即可根据项目中的给定图片生成所有视觉资产。

![Visual Studio 中清单设计器的屏幕截图](./media/deploy-modern-applications/manifest-designer.png)

如果打开 Package.appxmanifest 文件的代码，你会看到一些有趣的事情。

就在 `<Package>` 下面，有一个 `<Identity>` 节点。 这是打包应用程序获取标识的地方，它将由操作系统管理。

![Identity 节点](./media/deploy-modern-applications/identity-node.png)

在 `<Capabilities>` 节点中，你可以找到应用程序所需的所有必备组件，特别注意 `<rescap:Capability Name="runFullTrust" \>`，它会指示操作系统以完全信任模式运行应用，因为这是一个 Win32 应用程序。

![Capabilities 节点](./media/deploy-modern-applications/capabilities-node.png)

将打包项目设置为解决方案的启动项目，然后选择“运行”。 此操作将：

- 编译 Windows 窗体应用程序。
- 根据生成结果创建 MSIX 包。
- 部署包。
- 在开发计算机上进行本地安装。
- 启动应用。

![我们安装的应用程序](./media/deploy-modern-applications/our-installed-application.png)

你将借此获得 MSIX 提供的完全集成到 Windows 10 的干净安装和卸载体验。

最后阶段介绍如何将 MSIX 包部署到另一台计算机。

右键单击打包项目，选择“Store”菜单，然后选择“创建应用包”选项。

然后，可以选择是创建包以上传到 Store，还是创建包以进行旁加载。 在大多数现代化方案中，你将选择“我想创建包以进行旁加载”。

![配置包](./media/deploy-modern-applications/configuring-packages.png)

在这里，你可以选择要作为目标的不同体系结构，因为你可以在同一个 MSIX 包中包含任意数量的体系结构。

最后一步是声明要在何处部署最终安装资产。

![配置更新设置](./media/deploy-modern-applications/configure-update-settings.png)

你可以选择使用企业文件服务器上的共享 UNC 路径的 Web 服务器。 请注意用于指定应用程序更新方式的设置。 下一部分将介绍应用程序更新。

有关详细的分步指南，请参阅[使用 Visual Studio 从源代码打包桌面应用](/windows/msix/desktop/desktop-to-uwp-packaging-dot-net)。

## <a name="auto-updates-in-msix"></a>MSIX 中的自动更新

Windows 应用商店具有使用 Windows 更新的强大更新机制。 在大多数企业方案中，你不会使用应用商店来分发桌面应用。 因此，你需要采用一种类似的方法来为应用程序配置更新并将其拉取给用户。

结合使用 Windows 10 功能和 MSIX 包，你可以为用户提供出色的更新体验。 事实上，用户不需要掌握任何技术，也能从无缝的应用程序更新体验中受益。

你可以配置更新，以便通过两种不同的方式与用户交互：

- 用户提示的更新：操作系统显示一些自动生成的精美 UI，以通知用户即将安装应用程序。 它基于你在安装文件上指定的属性来生成此 UI。

- 后台无提示更新。 使用此选项，你的用户无需知道这些更新。

你还可以配置何时执行更新，是应用程序启动时更新还是定期更新。 凭借旁加载功能，你甚至可以在应用程序运行时获取这些更新。

使用这种类型的部署时，系统会创建一个名为 .appinstaller 的特殊文件。 这个简单文件包含以下几部分：

- .appinstaller 文件的位置
- 应用程序的主要 MSIX 包属性
- 更新行为

![.appinstaller 文件](./media/deploy-modern-applications/appinstaller-file.png)

结合此文件，Microsoft 设计了一个特殊的 URL 协议，以从链接启动安装过程：

```xml
<a href="ms-appinstaller:?source=http://mywebservice.azureedge.net/MyApp.msix">Install app package </a>
```

此协议适用于所有浏览器，并在 Windows 10 上以良好的用户体验启动安装过程。 由于操作系统负责管理安装过程，因此它知道此应用程序的安装位置，并跟踪受该过程影响的所有文件。

MSIX 会创建一个用于安装的用户界面，以自动显示包的某些属性。 这样可以为每个应用提供通用的安装体验。

生成新的 MSIX 包并将其迁移到部署服务器后，只需编辑 .appinstaller 文件以反映这些更改，主要包括版本和新 MSIX 文件的路径。 下次用户启动应用程序时，系统将检测到更改并在后台下载新版本的文件。 完成此操作后，将在新应用程序启动时以对用户透明的方式执行安装。

>[!div class="step-by-step"]
>[上一页](example-migration.md)
