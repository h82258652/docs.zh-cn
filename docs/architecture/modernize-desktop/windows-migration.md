---
title: Windows 10 迁移
description: 深入了解打包、XAML 岛等 Windows 10 功能。
ms.date: 12/29/2020
ms.openlocfilehash: 139a8f2354803dafeb0178b4dbfb57a95c4ddb34
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98615940"
---
# <a name="windows-10-migration"></a>Windows 10 迁移

设想一下：你有一个可正常运行的桌面应用程序，它是在 Windows 7 时代开发的。 它使用的是当时提供的 WPF 技术，并且运行良好。但是，当你在 Windows 10 上运行它时，它的 UI 和行为已经过时。 这就像你在《黑客帝国》这样的未来主义电影中，看到 Neo 使用 Nokia 8110 设备一样。 这部电影在 20 年后依然可圈可点，但是如果有设备现代化技术的加持，它会表现更出色。

随着 Windows 10 的发布，Microsoft 推出了许多创新，以支持平板电脑和触摸设备等场景，并为用户提供有史以来最棒的 Microsoft 操作系统体验。 例如，你能够：

- 使用 Windows Hello 通过人脸登录。
- 笔写或手写可自动识别并数字化的文本。
- 使用 WinML 在本地运行以云为基础构建的自定义 AI 模型。

通过 Windows 运行时 (WinRT) 库，可为 Windows 开发人员启用所有这些功能。 你可以在现有桌面应用中利用这些功能，因为 WinRT 库也同时对 .NET Framework 和 .NET 公开。 你甚至可以使用 XAML 岛来实现 UI 的现代化，并根据时代的发展来改善应用的视觉效果和行为。

这里有一点很重要，那就是你不需要放弃 .NET Framework 技术来走这条现代化道路。 你可以安全地留在原地，享受 Windows 10 带来的所有好处，而无需迁移到 .NET。 也就是说，你拥有了强大的功能和灵活性来选择你的现代化道路。

## <a name="winrt-apis"></a>WinRT API

WinRT API 是面向对象、结构良好的应用程序编程接口 (API)，可让 Windows 10 开发人员访问操作系统必须提供的所有功能。 通过 WinRT API，你可以在桌面应用上集成推送通知、设备 API、Microsoft Ink 和 WinML 等功能。

一般来说，WinRT API 可以从经典桌面应用中调用。 但是，有两个主要 API 打破了这项规则：

* 需要包标识的 API。
* 需要 XAML 或 Composition 等可视化效果的 API。

### <a name="universal-windows-platform-uwp-packages"></a>通用 Windows 平台 (UWP) 包

#### <a name="application-package-identity"></a>应用程序包标识

UWP 应用有一个部署系统，操作系统在其中管理应用程序的安装和卸载。 这要求安装必须是声明性的，也就是说在安装过程中不执行任何用户代码。 而应用想要与系统集成的一切，如协议、文件类型和扩展，都会在应用程序清单中声明。 在部署时，部署管道会配置这些集成点。 操作系统管理此功能并跟踪它的唯一方式是，让每个“包”都有一个标识，即，应用程序的唯一标识符。

一些 WinRT API 需要这个包标识才能按预期工作。 但是，本机 C++ 或 .NET 应用等经典桌面应用使用不同的部署系统，这类系统不需要包标识。 如果你想在桌面应用程序中使用这些 WinRT API，就需要为它们提供一个包标识。

一种方法是生成一个额外的打包项目。 在该打包项目中，指向原始源代码项目并指定要提供的标识信息。 如果安装包并运行已安装的应用，它会自动获取标识，使你的代码能够调用所有需要标识的 WinRT API。

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10">
    <Identity Name="YOUR-APP-GUID "
              Publisher="CN=YOUR COMPANY"
              Version="1.x.x.x" />
</Package>
```

你可以通过检查包含 API 的类型是否标有 [DualApiPartition](xref:Windows.Foundation.Metadata.DualApiPartitionAttribute) 属性，来检查哪些 API 需要打包的应用程序标识。 如果是，则可以从未打包的传统桌面应用中调用它。 否则，必须借助打包项目将经典桌面应用转换为 UWP。

<https://docs.microsoft.com/windows/desktop/apiindex/uwp-apis-callable-from-a-classic-desktop-app>

#### <a name="benefits-of-packaging"></a>打包的好处

除了允许你访问这些 API，你还可以通过为桌面应用程序创建 Windows 应用包来获得一些额外的好处，其中包括：

* **简化的部署**。 应用具有出色的部署体验，确保用户可以放心地安装和更新应用程序。 如果用户选择卸载应用，应用将被彻底删除，不留任何痕迹，从而防止出现 Windows rot 问题。

* **自动更新和许可**。 你的应用程序可以加入 Microsoft Store 的内置许可和自动更新机制。 自动更新是高度可靠和高效的机制，因为仅下载文件的已更改部分。

* **扩大了覆盖范围，并简化了盈利**。 你的情况可能并非如此，但如果你选择通过 Microsoft Store 分发应用程序，你将接触到数以百万计的 Windows 10 用户。

* **添加 UWP 功能**。 你可以按照自己的进度将 UWP 功能添加到应用包中。

#### <a name="prepare-for-packaging"></a>准备打包

在继续打包桌面应用程序之前，必须先解决一些要点问题。 应用程序必须遵守 Microsoft Store 的任何规则和策略，并在 UWP 应用程序模型中运行。 例如，它必须在 .NET Framework 4.6.2 或更高版本上运行，并写入 `HKEY_CURRENT_USER` 注册表配置单元，AppData 文件夹将被虚拟化到用户特定的应用本地位置。

打包的设计目标是将应用程序状态与系统状态分离，同时保持与其他应用的兼容性。 Windows 10 通过将应用程序放入 UWP 包来实现这一目标。 它在运行时检测并重定向文件系统和注册表的一些更改，以履行通过打包提供可信、干净的应用程序安装和卸载行为的承诺。

你为桌面应用程序创建的包是仅适用于桌面且未沙盒化的完全信任应用程序，不过，针对对 `HKCU` 和 `AppData` 的写入，该应用程序已应用轻量级虚拟化。 此虚拟化使它们能够像经典桌面应用程序一样与其他应用进行交互。

##### <a name="installation"></a>安装

应用包安装在 %ProgramFiles%\\WindowsApps\\package_name 下面，具有名为 `app_name.exe` 的可执行文件。 每个包文件夹都包含一个名为 `AppxManifest.xml` 的清单，清单中包含打包应用的特殊 XML 命名空间。 该清单文件内部是一个 `<EntryPoint>` 元素，该元素引用完全信任的应用。 启动该应用程序时，它不会在应用容器内运行，而是像往常一样以用户身份运行。

部署后，软件包文件由操作系统标记为只读并严格锁定。 如果这些文件遭到篡改，Windows 将阻止应用启动。

##### <a name="file-system"></a>文件系统

操作系统根据文件夹位置的不同，支持对打包桌面应用程序执行不同级别的文件系统操作。

尝试访问用户的 AppData 文件夹时，系统会在后台创建一个专用的每用户、每应用位置。 这会造成打包应用程序正在编辑真正的 AppData 的假象，而实际上它是在修改一个专用副本。 通过以这种方式重定向写入，系统可以跟踪应用所作的所有文件修改。 这样，它就可以在卸载时清理所有这些文件，从而减少系统“rot”，为用户提供更好的应用程序删除体验。

##### <a name="registry"></a>注册表

应用包包含一个 registry.dat 文件，它在逻辑上相当于实际注册表中的 `HKLM\Software`。 在运行时，此虚拟注册表将此配置单元的内容合并到原生系统配置单元，以提供两者的单一视图。

所有写入都会在包升级过程中得到保留，只有在卸载应用程序时才会删除。

##### <a name="uninstallation"></a>卸载

当用户卸载包时，位于 `C:\Program Files\WindowsApps\package_name` 下的所有文件和文件夹都会被删除，在数据写入过程中捕获的对 AppData 或注册表的所有重定向写入也会被删除。

有关打包应用程序如何处理安装、文件访问、注册表和卸载的详细信息，请参阅 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-behind-the-scenes>。

你可以在 <https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-prepare> 上获取待查事项的完整列表。

## <a name="how-to-add-winrt-apis-to-your-desktop-project"></a>如何将 WinRT API 添加到桌面项目

在本部分中，你可以找到有关如何在现有 WPF 应用程序中集成 Toast 通知的演练。 虽然从代码的角度来看很简单，但它有助于阐释整个过程。 通知是众多可用 WinRT API 之一，可用于 .NET 应用。 在本例中，该 API 需要一个包标识。 如果 API 不需要包标识，此过程会更简单。

让我们以一个现有的 WPF 示例应用为例，该应用可读取文件并在屏幕上显示其内容。 我们的目标是在应用程序启动时显示 Toast 通知。

![示例应用程序运行的屏幕截图](./media/windows-migration/sample-application.png)

首先，你应该在下面的链接中检查你将使用的 Windows 10 API 是否需要包标识：

<https://docs.microsoft.com/windows/apps/desktop/modernize/desktop-to-uwp-supported-api>

我们的示例将使用需要打包标识的 <xref:Windows.UI.Notifications.Notification?displayProperty=nameWithType> API：

![Microsoft 文档中的 Notification 类](./media/windows-migration/notification-class-documentation.png)

若要访问该 WinRT API，请添加对 `Microsoft.Windows.SDK.Contracts` NuGet 包的引用，此包将在后台发挥神奇作用（详见 <https://blogs.windows.com/windowsdeveloper/2019/04/30/calling-windows-10-apis-from-a-desktop-application-just-got-easier/>）。

现在可以开始添加一些代码。

创建将在应用程序启动时调用的 `ShowToastNotification` 方法。 它将从 XML 模式生成一个 toast 通知：

```csharp
private void ShowNotification(string title, string content, string image)
{
    string xmlString = $@"<toast><visual><binding template = 'ToastGeneric'><text>{title}</text><text>{content}</text><image src=>'{image}'</image></binding></visual></toast>";
    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);
    ToastNotification toast = new ToastNotification(toastXml);
    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

虽然成功生成项目，但出现了错误，因为通知 API 需要包标识，而你并未提供。 向解决方案添加 Windows 打包项目可以解决此问题：

![Visual Studio 中的“添加新项目”对话框的屏幕截图](./media/windows-migration/add-packaging-project.png)

选择你要支持的最低 Windows 版本和目标版本。 并非所有 Windows 10 版本都支持所有 WinRT API。 每个 Windows 10 更新都会添加新的 API，这些 API 只能从这个版本中获得；下级支持不可用。

![选择最低 Windows 版本](./media/windows-migration/select-versions.png)

下一步是通过添加项目引用，将 WPF 应用程序添加到 Windows 打包项目中：

![将 WPF 应用程序添加到 Windows 打包项目中](./media/windows-migration/add-application.png)

![引用管理器](./media/windows-migration/reference-manager.png)

一个 Windows 打包项目可以打包多个应用，因此应设置将哪个应用作为入口点：

![设置入口点](./media/windows-migration/set-entry-point.png)

下一步是在解决方案配置中将 WPF 项目设置为启动项目。 可以按 F5 键进行编译和生成，并查看结果。

![示例应用程序运行并显示结果](./media/windows-migration/sample-app-result.png)

让我们生成包，以便安装应用。 右键单击“Store” > “创建应用包”。

![“创建应用包”对话框](./media/windows-migration/create-app-packages.png)

选择旁加载选项以从计算机部署应用：

![选择旁加载选项](./media/windows-migration/select-sideloading-option.png)

选择应用的应用程序体系结构：

![选择应用程序体系结构](./media/windows-migration/select-app-architecture.png)

最后，单击“创建”创建包。

## <a name="xaml-islands"></a>XAML 岛

XAML 岛是一组组件，它们使 Windows 桌面开发人员能够在现有 Win32 应用程序（包括 Windows 窗体和 WPF）上使用 UWP XAML 控件。

![XAML 岛的结构](./media/windows-migration/xaml-islands.png)

设想一下，你的 Win32 应用上有许多标准控件，其中有一个 UWP UI“岛”，包含现代世界中的控件。 这个概念类似于网页中有一个 iFrame，用于显示来自 `different page.` 的内容。

除了从 Windows 10 API 添加功能外，你还可以使用 XAML 岛在应用内添加 UWP XAML 片段。

Windows 10 1903 更新引入了一组 API，可用于在 Win32 窗口中托管 UWP XAML 内容。 只有在 Windows 10 1903 上运行的应用才能使用 XAML 岛。

### <a name="the-road-to-xaml-islands"></a>通往 XAML 岛之路

通往 XAML 岛之路始于 2012 年，当时 Microsoft 引入 WinRT API 作为框架，以实现 Win32 应用（Windows 窗体、WPF 和本机 Win32 应用）的现代化。 但是，WinRT 中的新 UI 控件可用于新应用程序，但不能用于现有应用程序。

2015 年，伴随 Windows 10 诞生的还有 UWP。 UWP 允许创建跨 Windows 设备（如 XBox、移动和桌面）工作的应用。 一年后，Microsoft 推出了 Desktop Bridge（原名 Project Centennial）。 Desktop Bridge 是让开发人员能够将现有 Win32 应用引入 Microsoft Store 的一组工具。 它在 2017 年添加了更多功能，使开发人员能够利用一些新的 Windows 10 API（例如实时磁贴和操作中心通知）来增强 Win32 应用。 但是，还是没有新的 UI 控件。

在 Build 2018 大会上，Microsoft 宣布了一种方法，让开发人员能够在当前的 Win32 应用中使用新的 Windows 10 XAML 控件，而无需将应用完全迁移到 UWP。 这种方法称作 UWP XAML 岛。

### <a name="how-it-works"></a>工作原理

Windows 10 1903 更新引入了几个 XAML 托管 API。 其中两个是 `WindowsXamlManager` 和 `DesktopWindowXamlSource`。

`WindowsXamlManager` 类处理 UWP XAML 框架。 它的 `InitializeForCurrentThread` 方法可在 Win32 应用的当前线程内加载 UWP XAML 框架。

`DesktopWindowXamlSource` 是 XAML 岛内容的实例。 它具有 `Content` 属性，你负责实例化和设置该属性。 `DesktopWindowXamlSource` 从 HWND 呈现并获取其输入。 它需要知道将 XAML 岛的 HWND 附加到哪个 HWND，你负责调整父 HWND 的大小和位置。

WPF 或 Windows 窗体开发人员通常不会在代码中处理 HWND，因此，可能很难理解和处理 HWND 指针以及用于在 Win32 和 UWP 之间建立通信的底层布线。

#### <a name="the-xaml-islands-net-wrappers"></a>XAML 岛 .NET 包装器

Windows 社区工具包有一套适用于 WPF 或 Windows 窗体的 XAML 岛 .NET 包装器，可以简化 XAML 岛的使用。 这些包装器可管理 HWND、焦点管理等。 Windows 窗体和 WPF 开发人员应使用这些包装器。

Windows 社区工具包提供两种类型的控件：包装控件和托管控件。

##### <a name="wrapped-controls"></a>包装控件

这些包装控件将一些 UWP 控件包装到 Windows 窗体或 WPF 控件中，为这些开发人员隐藏 UWP 概念。 这些控件包括：

* WebView 和 WebViewCompatible
* InkCanvas 和 InkToolbar
* MediaPlayerElement
* MapControl

将 `Microsoft.Toolkit.Wpf.UI.Controls` 包添加到项目中，添加对命名空间的引用，然后开始使用它们。

```xml
<Window
        ...
        xmlns:uwpControls="clr-namespace:Microsoft.Toolkit.Wpf.UI.Controls;assembly=Microsoft.Toolkit.Wpf.UI.Controls">
<Grid>
    <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="\*"/>
    </Grid.RowDefinitions>
    <uwpControls:InkToolbar TargetInkCanvas="{x:Reference Name=inkCanvas}"/>
    <uwpControls:InkCanvas Grid.Row="1" x:Name="inkCanvas" />
</Grid>
```

##### <a name="hosting-controls"></a>托管控件

XAML 岛的强大功能延伸到大多数第一方控件、第三方控件以及为 UWP 开发的自定义控件，这些控件可以作为“岛”集成到 Windows 窗体和 WPF 中，并具有功能完备的 UI。 WPF 和 Windows 窗体的 `WindowsXamlHost` 控件允许这样做。

例如，若要使用 WPF 中的 `WindowsXamlHost` 控件，请添加对 Windows 社区工具包提供的 `Microsoft.Toolkit.Wpf.UI.XamlHost` 包的引用。

一旦将 `WindowsXamlHost` 放入 UI 代码后，就可以指定要加载的 UWP 类型。 你可以选择使用像 `Button` 这样的包装控件，也可以使用由几个不同控件组成的更复杂的控件，即自定义 UWP 控件。

以下示例演示如何添加 UWP `Button`：

```xml
<Window
        ...
        xmlns:xamlhost="clr-namespace:Microsoft.Toolkit.Wpf.UI.XamlHost;assembly=Microsoft.Toolkit.Wpf.UI.XamlHost">

<xamlhost:WindowsXamlHost x:Name="myUwpButton"
                          InitialTypeName="Windows.UI.Xaml.Controls.Button" />
```

对于如何实现此操作，有一个明确的建议：最好是使用一个包含自定义复合控件的较大的 XAML 岛，而不是使用几个岛，每个岛上一个控件。

XAML 的核心功能之一是绑定，它适用于 Win32 代码和岛。 例如，你可以将 Win32 `Textbox` 与 UWP `Textbox` 绑定。 你需要考虑的一个重要问题是，这些绑定是从 UWP 到 Win32 的单向绑定，因此，如果更新 XAML 岛中的 `Textbox`，Win32 Textbox 也会更新，反过来却不会。

若要查看有关如何使用 XAML 岛的演练，请参阅：

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-standard-control-with-xaml-islands>

#### <a name="adding-uwp-xaml-custom-controls"></a>添加 UWP XAML 自定义控件

XAML 自定义控件是由你或第三方创建的控件（或用户控件），其中包括 WinUI 2.x 控件。 若要在 Windows 窗体或 WPF 应用中托管自定义 UWP 控件，你需要：

- 在 .NET 应用中使用 `WindowsXamlHost` UWP 控件。
- 创建一个 UWP 应用项目来定义 `XamlApplication` 对象。

WPF 或 Windows 窗体项目必须有权访问 Windows 社区工具包提供的 `Microsoft.Toolkit.Win32.UI.XamlHost.XamlApplication` 类的实例。 此对象充当根元数据提供程序，用于为应用程序当前目录中的程序集中的自定义 UWP XAML 类型加载元数据。 若要实现此目的，建议的方法是向 WPF 或 Windows 窗体项目所在的解决方案添加一个空白应用（通用 Windows）项目，并修改此项目中的默认 App 类。

自定义 UWP XAML 控件可以包含在此 UWP 应用中，也可以包含在独立的 UWP 类库项目中，你可以在 WPF 或 Windows 窗体项目所在的解决方案中引用该项目。

你可以在以下位置查看详细的分步过程说明：

<https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands>

#### <a name="the-windows-ui-library-winui-2"></a>Windows UI 库 (WinUI 2)

除了操作系统自带的收件箱 Windows 10 控件外，同一 UWP XAML 团队还在 Windows UI 库 (WinUI 2) 中提供了其他控件。 WinUI 2 为 Windows UWP 应用提供官方的本机 Microsoft UI 控件和功能，这些控件可以在 XAML 岛内使用。

WinUI 2 是一个开源项目，你可以在 <https://github.com/microsoft/microsoft-ui-xaml> 中找到相关信息。

下面的文章演示了如何托管 WinUI 2 库中的 UWP XAML 控件：<https://docs.microsoft.com/windows/apps/desktop/modernize/host-custom-control-with-xaml-islands>

### <a name="do-you-need-xaml-islands"></a>是否需要 XAML 岛

XAML 岛适用于现有的 Win32 应用，这些应用希望利用新的 UWP 控件和行为来改善用户体验，而无需完全重写应用。 你已经可以[使用 Windows 10 API](/windows/uwp/porting/desktop-to-uwp-enhance)，但在 XAML 岛出现之前，你只能使用非 UI 相关的 API。

如果要开发新的 Windows 应用，[UWP 应用](/windows/uwp/get-started/universal-application-platform-guide)可能是正确的方法。

### <a name="the-road-ahead-xaml-islands-winui-30"></a>通往 XAML 岛之路：WinUI 3.0

自 Windows 8 以来，Windows UI 平台（包括 XAML UI 框架、视觉对象组合层和输入处理）已作为 Windows 不可或缺的一部分提供。 这意味着，若要从 UI 技术的最新改进中获益，就必须升级到 UI 的最新版本，这样会放慢应用开发的创新步伐。 为了分离这两个发展周期，促进创新，Microsoft 正在积极开发 WinUI 项目。

从 2018 年的 WinUI 2 开始，Microsoft 开始将一些新的 XAML UI 控件和功能作为构建在 UWP SDK 之上的独立 NuGet 包发布。

![WinUI 2.0 的结构](./media/windows-migration/winui2.png)

WinUI 3 正在积极开发中，它将大大扩展 WinUI 的范围，以包括完整的 UI 平台（将与 UWP SDK 完全分离）：

![WinUI 3.0 的结构](./media/windows-migration/winui3.png)

XAML 框架现在将在 GitHub 上开发，并以 [NuGet](/nuget/what-is-nuget) 包的形式进行带外发布。

作为 OS 的一部分发布的现有 UWP XAML API 将不会再收到新的功能更新。 它们会在 Windows 10 支持生命周期内继续收到安全更新程序和关键修复程序。

通用 Windows 平台不仅包含 XAML 框架，还包含其他功能（例如应用程序和安全模型、媒体管道、Xbox 和 Windows 10 shell 集成、广泛的设备支持），并且还将继续发展。 所有新的 XAML 功能都将作为 WinUI 的一部分进行开发和交付。

#### <a name="winui-3-in-desktop-app-and-winui-xaml-islands"></a>桌面应用中的 WinUI 3 和 WinUI XAML 岛

如你所见，WinUI 3 是由 UWP XAML 演变而来，它可以在 UWP 应用模型及其所有必备组件（MSIX 打包 ID、沙盒、CoreWindow 等）中自然运行。 若要在 Win32 应用模型中仅使用 WinUI 3，应由另一个 UI 框架（Windows 窗体、WPF 等）使用 WinUI XAML 岛托管 WinUI 内容。 若要改进应用并实现技术混用，这是正确的做法。 但是，若要为 WinUI 替换掉整个旧 UI，应用不应加载仅用于托管 WinUI 的 UI 框架。

WinUI 3 将解决此关键反馈，在桌面应用中添加 WinUI。 这将允许 Win32 应用使用 WinUI 3 作为独立的 UI 框架；无需加载 Windows 窗体或 WPF。

在此聚合内，WinUI 3 将让开发人员轻松混搭出合适的组合：

* 应用模型：UWP、Win32
* 平台：.NET 或本机
* 语言：.NET（C\#、Visual Basic）、标准 C++
* 打包：MSIX、用于 Microsoft Store 的 AppX、未打包
* 互操作：使用 WinUI 3 通过 WinUI XAML 岛扩展现有 WPF、WinForms 和 MFC 应用。

若要了解更多详细信息，Microsoft 正在 <https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md> 中分享此路线图。

>[!div class="step-by-step"]
>[上一页](migrate-modern-applications.md)
>[下一页](example-migration.md)
