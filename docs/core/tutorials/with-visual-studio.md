---
title: 使用 Visual Studio 创建 .NET 控制台应用程序
description: 了解如何使用 Visual Studio 通过 C# 或 Visual Basic 创建 .NET 控制台应用程序。
ms.date: 03/26/2021
dev_langs:
- csharp
- vb
ms.custom: vs-dotnet,contperf-fy21q3
ms.openlocfilehash: e55927080ab30e7a24c54656b7f11a94a023bd65
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105636800"
---
# <a name="tutorial-create-a-net-console-application-using-visual-studio"></a>教程：使用 Visual Studio 创建 .NET 控制台应用程序

本教程演示如何在 Visual Studio 2019 中创建和运行 .NET 控制台应用程序。

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019 版本 16.9.2 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。 选择此工作负载时，将自动安装 .NET 5.0 SDK。

  有关详细信息，请参阅[使用 Visual Studio 安装 .NET SDK](../install/windows.md#install-with-visual-studio)。

## <a name="create-the-app"></a>创建应用

创建一个名为“HelloWorld”的 .NET 控制台应用项目。

1. 启动 Visual Studio 2019。

1. 在“开始”页上，选择“创建新项目”。

   :::image type="content" source="./media/with-visual-studio/start-window.png" alt-text="在 Visual Studio“开始”页选择“创建新项目”按钮":::

1. 在“创建新项目”页面，在搜索框中输入“控制台”。 接下来，从“语言”列表中选择“C#”或“Visual Basic”，然后从“平台”列表中选择“所有平台”  。 选择“控制台应用程序”模板，然后选择“下一步” 。

   :::image type="content" source="./media/with-visual-studio/create-new-project.png" alt-text="使用所选筛选器创建新项目窗口":::

   > [!TIP]
   > 如果看不到 .NET 模板，则可能缺少所需的工作负载。 在“找不到所需内容?”消息下，选择“安装更多工具和功能”链接。 Visual Studio 安装程序随即打开。 确保安装了“.NET Core 跨平台开发”工作负载。

1. 在“配置新项目”对话框中，在“项目名称”框中输入“HelloWorld”。 然后选择“下一步”  。

   :::image type="content" source="./media/with-visual-studio/configure-new-project.png" alt-text="为新项目窗口配置“项目名称”、“位置”和“解决方案名称”字段":::

1. 在“其他信息”对话框中，选择“.NET 5.0 (当前)”，然后选择“创建”  。

   :::image type="content" source="media/with-visual-studio/additional-info.png" alt-text="“其他信息”对话框":::

用于创建简单的“Hello World”应用程序的模板。 它会调用 <xref:System.Console.WriteLine(System.String)?displayProperty=nameWithType> 方法来显示“Hello World!” 显示文本字符串“Hello World!”。

模板代码将定义类 `Program`，其中包含一个需要将 <xref:System.String> 数组用作参数的方法 `Main`：

```csharp
using System;

namespace HelloWorld
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

```vb
Imports System

Module Program
    Sub Main(args As String())
        Console.WriteLine("Hello World!")
    End Sub
End Module
```

`Main` 是应用程序入口点，同时也是在应用程序启动时由运行时自动调用的方法。 *args* 数组中包含在应用程序启动时提供的所有命令行自变量。

如果未显示想要使用的语言，请更改页面顶部的语言选择器。

## <a name="run-the-app"></a>运行应用

1. 按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行程序而不进行调试。

   此时将打开在屏幕上显示文本“Hello World!” 。

   :::image type="content" source="./media/with-visual-studio/hello-world-console.png" alt-text="控制台窗口，其中显示 Hello World Press any key to continue":::

1. 按任意键关闭控制台窗口。

## <a name="enhance-the-app"></a>增强应用

改进应用程序，使其提示用户输入名字，并将其与日期和时间一同显示。

1. 在 Program.cs 或 Program.vb 中，将 `Main` 方法的内容（当前只是调用 `Console.WriteLine` 的行）替换为以下代码：

   :::code language="csharp" source="./snippets/with-visual-studio/csharp/Program.cs" id="MainMethod":::
   :::code language="vb" source="./snippets/with-visual-studio/vb/Program.vb" id="MainMethod":::

   此代码会在控制台窗口中显示一条提示，然后等待用户输入字符串并按 <kbd>Enter</kbd>。 它会将此字符串存储到名为 `name` 的变量中。 它还会检索 <xref:System.DateTime.Now?displayProperty=nameWithType> 属性的值（其中包含当前的本地时间），并将此值赋给 `currentDate` 变量。 同时会在控制台窗口中显示这些值。 最后会在控制台窗口中显示一条提示，并调用 <xref:System.Console.ReadKey(System.Boolean)?displayProperty=nameWithType> 方法来等待用户输入。

   <xref:System.Environment.NewLine> 是一种独立于平台和语言的表示换行符的方式。 替代方法是在 C# 中使用 `\n` 和在 Visual Basic 中使用 `vbCrLf`。

   字符串前面的美元符号 (`$`) 使你可以将表达式（如变量名称）放入字符串中的大括号内。 表达式值将代替表达式插入到字符串中。 此语法称为[内插字符串](../../csharp/language-reference/tokens/interpolated.md)。

1. 按 <kbd>Ctrl</kbd>+<kbd>F5</kbd> 运行程序而不进行调试。

1. 出现提示时，输入名称并按 Enter<kbd></kbd> 键。

   :::image type="content" source="./media/with-visual-studio/hello-world-update.png" alt-text="控制台窗口，含已修改程序的输出":::

1. 按任意键关闭控制台窗口。

## <a name="additional-resources"></a>其他资源

- [当前版本和长期支持版本](../releases-and-support.md#net-core-and-net-5-version-lifecycles)

## <a name="next-steps"></a>后续步骤

在本教程中，你创建了一个 .NET 控制台应用程序。 在下一教程中，你将调试该应用。

> [!div class="nextstepaction"]
> [使用 Visual Studio 调试 .NET 控制台应用程序](debugging-with-visual-studio.md)
