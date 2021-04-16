---
title: ASP.NET Web Forms 和 Blazor 的体系结构比较
description: 了解 ASP.NET Web Forms 和 Blazor 的体系结构的区别。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: afb5d4025b81c2ddef782c462c94d32edc872a21
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509723"
---
# <a name="architecture-comparison-of-aspnet-web-forms-and-blazor"></a>ASP.NET Web Forms 和 Blazor 的体系结构比较

尽管 ASP.NET Web Forms 和 Blazor 有许多相似的概念，但它们的工作方式有所不同。 本章将介绍 ASP.NET Web Forms 和 Blazor 的内部工作情况。

## <a name="aspnet-web-forms"></a>ASP.NET Web 窗体

ASP.NET Web Forms 框架基于以页面为中心的体系结构。 对应用中某个位置的每个 HTTP 请求都是一个单独的页面，ASP.NET 使用页面进行响应。 请求页面时，浏览器的内容会替换为请求的页面的结果。

页面包含以下组件：

- HTML 标记
- C# 或 Visual Basic 代码
- 包含逻辑和事件处理功能的代码隐藏类
- 控制

控件是可重复使用的 Web UI 单元，可以以编程方式放置在页面上并与之交互。 页面由包含标记、控件和某些代码的以 .aspx 结尾的文件组成。 代码隐藏类位于具有相同基名称和 .aspx.cs 或 .aspx.vb 文件扩展名的文件中，具体取决于所使用的编程语言 。 有趣的是，Web 服务器解释 .aspx 文件的内容并在内容更改时对其进行编译。 即使 Web 服务器已经在运行，也会进行重新编译。

控件可用标记生成并作为用户控件提供。 用户控件派生自 `UserControl` 类，且具有与页面相似的结构。 用户控件的标记存储在 .ascx 文件中。 附带的代码隐藏类位于 .ascx.cs 或 .ascx.vb 文件中 。 还可通过从 `WebControl` 或 `CompositeControl` 基类继承来完全使用代码生成控件。

页面也有事件生命周期范围。 每个页面都可引发初始化、加载、预呈现和卸载事件，这些事件在 ASP.NET 运行时执行每个请求的页面代码时发生。

页面上的控件通常会回发到呈现控件的同一页面，并从名为 `ViewState` 的隐藏表单域中携带有效负载。 `ViewState` 字段包含有关控件在页面上呈现和显示时的状态信息，从而使 ASP.NET 运行时能够比较并标识提交给服务器的内容中的更改。

## Blazor

Blazor 是一个客户端 Web UI 框架，本质上类似于 JavaScript 前端框架（如 Angular 或 React）。 Blazor 处理用户交互，并呈现必要的 UI 更新。 Blazor 不基于请求-答复模式。 用户交互处理为不在任何特定 HTTP 请求上下文中的事件。

Blazor 应用由一个或多个在 HTML 页上呈现的根组件组成。

![HTML 中的 Blazor 组件](./media/architecture-comparison/blazor-components-in-html.png)

用户指定组件的呈现位置的方式，以及组件针对用户交互的连接方式，是[托管模型](hosting-models.md)特有的。

Blazor [组件](components.md)是表示可重复使用的 UI 部分的 .NET 类。 每个组件都维护其自己的状态，并指定其呈现逻辑，这可能包括呈现其他组件。 组件为特定用户交互指定事件处理程序，以更新组件的状态。

组件处理事件后，Blazor 会呈现该组件并跟踪在呈现的输出中更改的内容。 组件不会直接呈现为文档对象模型 (DOM)。 而是呈现为 DOM 的内存中表示形式（名为 `RenderTree`），以便 Blazor 可以跟踪更改。 Blazor 将新呈现的输出与以前的输出进行比较以计算 UI 差异，然后将其有效地应用于 DOM。

![Blazor DOM 交互](./media/architecture-comparison/blazor-dom-interaction.png)

还可手动指示组件应在其状态发生非正常 UI 事件的更改时呈现。 Blazor 使用 `SynchronizationContext` 来强制执行单个逻辑线程。 组件的生命周期方法和 Blazor 引发的任何事件回调都在此 `SynchronizationContext` 上执行。

>[!div class="step-by-step"]
>[上一页](introduction.md)
>[下一页](hosting-models.md)
