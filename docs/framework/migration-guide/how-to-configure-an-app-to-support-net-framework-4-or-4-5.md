---
title: 如何：将应用配置为支持 .NET Framework 4 或更高版本
description: 使用包含的示例了解如何配置桌面应用以支持 .NET Framework 4 或更高版本。
ms.date: 03/30/2017
helpviewer_keywords:
- configuring apps to support .NET Framework
- .NET Framework, configuring apps
ms.assetid: 63c6b9a8-0088-4077-9aa3-521ab7290f79
ms.openlocfilehash: 66a6ec5f6cad0225e48f480eeae8b7d3862241dd
ms.sourcegitcommit: bbc724b72fb6c978905ac715e4033efa291f84dc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107369681"
---
# <a name="how-to-configure-an-app-to-support-net-framework-4-or-later-versions"></a>如何：将应用配置为支持 .NET Framework 4 或更高版本

托管公共语言运行时 (CLR) 的所有应用程序都必须启动（或 *激活*）CLR，才能运行托管代码。 通常，.NET Framework 应用在生成它的 CLR 版本上运行，但你可以使用应用程序配置文件（有时称为 app.config 文件）来更改桌面应用程序的此行为。 但是，您不能使用应用程序配置文件来更改 Windows 应用商店应用或 Windows Phone 应用程序的默认激活行为。 本文说明如何使桌面应用程序能够在 .NET Framework 的其他版本上运行，并提供了如何定位版本 4 或更高版本的示例。

 按下列顺序确定在其上运行应用程序的 .NET Framework 的版本：

- 配置文件。

     如果应用程序配置文件中的 [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 条目指定了一个或多个 .NET Framework 版本，且用户计算机安装的是其中一个版本，那么应用程序就会在相应版本中运行。 配置文件按列出顺序读取 [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 条目，并使用列出的第一个在用户计算机上安装的 .NET Framework 版本。 （对于版本 1.0，使用 [\<requiredRuntime> 元素](../configure-apps/file-schema/startup/requiredruntime-element.md)。）

- 编译的版本。

     如果不存在任何配置文件，但用户计算机上存在基于其构建应用程序的 .NET Framework 版本，则此应用程序将在此版本上运行。

- 已安装的最新版本。

     如果生成应用程序的 .NET Framework 版本不存在，且配置文件未在 [\<supportedRuntime> 元素](../configure-apps/file-schema/startup/supportedruntime-element.md)中指定版本，那么应用程序会尝试在用户计算机上安装的最新版 .NET Framework 中运行。

     但是，.NET Framework 1.0、1.1、2.0、3.0 和 3.5 应用程序不会自动在 .NET Framework 4 或更高版本上运行，在某些情况下，用户可能会收到错误，且系统可能会提示用户安装 .NET Framework 3.5。 由于不同版本的 Windows 系统包含的 .NET Framework 版本不同，因此激活行为还取决于用户的操作系统。 如果应用程序支持 .NET Framework 3.5 和 4 或更高版本，建议您在配置文件中使用多个条目来指明这一点，以避免 .NET Framework 初始化错误。 有关详细信息，请参见[版本和依赖关系](versions-and-dependencies.md)。

 为了利用版本 4 和更高版本中的性能改进，可能还需要将 .NET Framework 3.5 应用配置为在 .NET Framework 版本 4 或更高版本上运行，甚至在安装了 .NET Framework 3.5 的计算机上也是如此。

> [!IMPORTANT]
> 建议你始终在你支持的每个 .NET Framework 版本上测试应用程序。 若要了解如何将应用程序升级为支持更高版本的 .NET Framework，请参阅[版本兼容性](version-compatibility.md)。

 若要了解如何将 .NET Framework 1.0 和 1.1 应用程序修改为支持 Windows 7 和 Windows 8，请参阅[从 .NET Framework 1.1 迁移](migrating-from-the-net-framework-1-1.md)。

## <a name="to-configure-your-app-to-run-on-the-net-framework-4-or-later-versions"></a>将应用程序配置为在 .NET Framework 4 或更高版本上运行

1. 添加或查找 .NET Framework 项目的配置文件。 应用程序的配置文件与该应用程序位于相同的目录中，并且具有相同的名称，只不过它具有扩展名 .config。 例如，对于名为 MyExecutable.exe 的应用程序，应用程序配置文件的名称为 MyExecutable.exe.config。

     若要添加配置文件，请在 Visual Studio 菜单栏上依次选择“项目”和“添加新项”。 在左侧窗格中，依次选择“常规”和“配置文件”。 将配置文件命名为 App.config。这些菜单选项对于 Windows 应用商店应用或 Windows Phone 应用程序项目不可用，因为无法在这些平台上更改激活策略。

2. 在应用程序配置文件中添加如下 [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 元素：

    ```xml
    <configuration>
      <startup>
        <supportedRuntime version="version"/>
      </startup>
    </configuration>
    ```

     其中，\<version> 指定与你的应用程序支持的 .NET Framework 版本匹配的 CLR 版本。 使用以下字符串：

    - .NET Framework 1.0：“v1.0.3705”

    - .NET Framework 1.1：“v1.1.4322”

    - .NET Framework 2.0、3.0 和 3.5：“v2.0.50727”

    - .NET Framework 4 及更高版本：“v4.0”

     可以添加多个 [\<supportedRuntime>](../configure-apps/file-schema/startup/supportedruntime-element.md) 元素（按优先顺序列出）来指定对多个 .NET Framework 版本的支持。

 下表演示安装在计算机上的应用程序配置文件设置和 .NET Framework 版本如何确定在计算机上运行的 .NET Framework 3.5 应用程序的版本。 这些示例特定于 .NET Framework 3.5 应用程序，但您可以将类似逻辑用于使用早期版本的 .NET Framework 生成的目标应用程序。 请注意，.NET Framework 2.0 版本号 (v2.0.50727) 用于在应用程序配置文件中指定 .NET Framework 3.5。

|App.config 文件设置|在安装了 3.5 版的计算机上|在安装了版本 3.5 和 4 或更高版本的计算机上|在安装了版本 4 或更高版本的计算机上|
|-|-|-|-|
|None|在 3.5 上运行|在 3.5 上运行|显示提示用户安装正确版本的错误消息*|
|`<supportedRuntime version="v2.0.50727"/>`|在 3.5 上运行|在 3.5 上运行|显示提示用户安装正确版本的错误消息*|
|`<supportedRuntime version="v2.0.50727"/>` <br /> `<supportedRuntime version="v4.0"/>`|在 3.5 上运行|在 3.5 上运行|在 4 或更高版本上运行|
|`<supportedRuntime version="v4.0"/>` <br /> `<supportedRuntime version="v2.0.50727"/>`|在 3.5 上运行|在 4 或更高版本上运行|在 4 或更高版本上运行|
|`<supportedRuntime version="v4.0"/>`|显示提示用户安装正确版本的错误消息*|在 4 或更高版本上运行|在 4 或更高版本上运行|

 \*若要详细了解此错误消息以及如何避免它，请参阅 [.NET Framework 初始化错误：管理用户体验](../deployment/initialization-errors-managing-the-user-experience.md)。

## <a name="see-also"></a>请参阅

- [从 .NET Framework 1.1 迁移](migrating-from-the-net-framework-1-1.md)
- [迁移指南](index.md)
