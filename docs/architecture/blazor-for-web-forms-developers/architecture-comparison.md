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
# <a name="architecture-comparison-of-aspnet-web-forms-and-blazor"></a><span data-ttu-id="74311-103">ASP.NET Web Forms 和 Blazor 的体系结构比较</span><span class="sxs-lookup"><span data-stu-id="74311-103">Architecture comparison of ASP.NET Web Forms and Blazor</span></span>

<span data-ttu-id="74311-104">尽管 ASP.NET Web Forms 和 Blazor 有许多相似的概念，但它们的工作方式有所不同。</span><span class="sxs-lookup"><span data-stu-id="74311-104">While ASP.NET Web Forms and Blazor have many similar concepts, there are differences in how they work.</span></span> <span data-ttu-id="74311-105">本章将介绍 ASP.NET Web Forms 和 Blazor 的内部工作情况。</span><span class="sxs-lookup"><span data-stu-id="74311-105">This chapter examines the inner workings and architectures of ASP.NET Web Forms and Blazor.</span></span>

## <a name="aspnet-web-forms"></a><span data-ttu-id="74311-106">ASP.NET Web 窗体</span><span class="sxs-lookup"><span data-stu-id="74311-106">ASP.NET Web Forms</span></span>

<span data-ttu-id="74311-107">ASP.NET Web Forms 框架基于以页面为中心的体系结构。</span><span class="sxs-lookup"><span data-stu-id="74311-107">The ASP.NET Web Forms framework is based on a page-centric architecture.</span></span> <span data-ttu-id="74311-108">对应用中某个位置的每个 HTTP 请求都是一个单独的页面，ASP.NET 使用页面进行响应。</span><span class="sxs-lookup"><span data-stu-id="74311-108">Each HTTP request for a location in the app is a separate page with which ASP.NET responds.</span></span> <span data-ttu-id="74311-109">请求页面时，浏览器的内容会替换为请求的页面的结果。</span><span class="sxs-lookup"><span data-stu-id="74311-109">As pages are requested, the contents of the browser are replaced with the results of the page requested.</span></span>

<span data-ttu-id="74311-110">页面包含以下组件：</span><span class="sxs-lookup"><span data-stu-id="74311-110">Pages consist of the following components:</span></span>

- <span data-ttu-id="74311-111">HTML 标记</span><span class="sxs-lookup"><span data-stu-id="74311-111">HTML markup</span></span>
- <span data-ttu-id="74311-112">C# 或 Visual Basic 代码</span><span class="sxs-lookup"><span data-stu-id="74311-112">C# or Visual Basic code</span></span>
- <span data-ttu-id="74311-113">包含逻辑和事件处理功能的代码隐藏类</span><span class="sxs-lookup"><span data-stu-id="74311-113">A code-behind class containing logic and event-handling capabilities</span></span>
- <span data-ttu-id="74311-114">控制</span><span class="sxs-lookup"><span data-stu-id="74311-114">Controls</span></span>

<span data-ttu-id="74311-115">控件是可重复使用的 Web UI 单元，可以以编程方式放置在页面上并与之交互。</span><span class="sxs-lookup"><span data-stu-id="74311-115">Controls are reusable units of web UI that can be programmatically placed and interacted with on a page.</span></span> <span data-ttu-id="74311-116">页面由包含标记、控件和某些代码的以 .aspx 结尾的文件组成。</span><span class="sxs-lookup"><span data-stu-id="74311-116">Pages are composed of files that end with *.aspx* containing markup, controls, and some code.</span></span> <span data-ttu-id="74311-117">代码隐藏类位于具有相同基名称和 .aspx.cs 或 .aspx.vb 文件扩展名的文件中，具体取决于所使用的编程语言 。</span><span class="sxs-lookup"><span data-stu-id="74311-117">The code-behind classes are in files with the same base name and an *.aspx.cs* or *.aspx.vb* extension, depending on the programming language used.</span></span> <span data-ttu-id="74311-118">有趣的是，Web 服务器解释 .aspx 文件的内容并在内容更改时对其进行编译。</span><span class="sxs-lookup"><span data-stu-id="74311-118">Interestingly, the web server interprets contents of the *.aspx* files and compiles them whenever they change.</span></span> <span data-ttu-id="74311-119">即使 Web 服务器已经在运行，也会进行重新编译。</span><span class="sxs-lookup"><span data-stu-id="74311-119">This recompilation occurs even if the web server is already running.</span></span>

<span data-ttu-id="74311-120">控件可用标记生成并作为用户控件提供。</span><span class="sxs-lookup"><span data-stu-id="74311-120">Controls can be built with markup and delivered as user controls.</span></span> <span data-ttu-id="74311-121">用户控件派生自 `UserControl` 类，且具有与页面相似的结构。</span><span class="sxs-lookup"><span data-stu-id="74311-121">A user control derives from the `UserControl` class and has a similar structure to the Page.</span></span> <span data-ttu-id="74311-122">用户控件的标记存储在 .ascx 文件中。</span><span class="sxs-lookup"><span data-stu-id="74311-122">Markup for user controls is stored in an *.ascx* file.</span></span> <span data-ttu-id="74311-123">附带的代码隐藏类位于 .ascx.cs 或 .ascx.vb 文件中 。</span><span class="sxs-lookup"><span data-stu-id="74311-123">An accompanying code-behind class resides in an *.ascx.cs* or *.ascx.vb* file.</span></span> <span data-ttu-id="74311-124">还可通过从 `WebControl` 或 `CompositeControl` 基类继承来完全使用代码生成控件。</span><span class="sxs-lookup"><span data-stu-id="74311-124">Controls can also be built completely with code, by inheriting from either the `WebControl` or `CompositeControl` base class.</span></span>

<span data-ttu-id="74311-125">页面也有事件生命周期范围。</span><span class="sxs-lookup"><span data-stu-id="74311-125">Pages also have an extensive event lifecycle.</span></span> <span data-ttu-id="74311-126">每个页面都可引发初始化、加载、预呈现和卸载事件，这些事件在 ASP.NET 运行时执行每个请求的页面代码时发生。</span><span class="sxs-lookup"><span data-stu-id="74311-126">Each page raises events for the initialization, load, prerender, and unload events that occur as the ASP.NET runtime executes the page's code for each request.</span></span>

<span data-ttu-id="74311-127">页面上的控件通常会回发到呈现控件的同一页面，并从名为 `ViewState` 的隐藏表单域中携带有效负载。</span><span class="sxs-lookup"><span data-stu-id="74311-127">Controls on a Page typically post-back to the same page that presented the control, and carry along with them a payload from a hidden form field called `ViewState`.</span></span> <span data-ttu-id="74311-128">`ViewState` 字段包含有关控件在页面上呈现和显示时的状态信息，从而使 ASP.NET 运行时能够比较并标识提交给服务器的内容中的更改。</span><span class="sxs-lookup"><span data-stu-id="74311-128">The `ViewState` field contains information about the state of the controls at the time they were rendered and presented on the page, allowing the ASP.NET runtime to compare and identify changes in the content submitted to the server.</span></span>

## Blazor

<span data-ttu-id="74311-129">Blazor 是一个客户端 Web UI 框架，本质上类似于 JavaScript 前端框架（如 Angular 或 React）。</span><span class="sxs-lookup"><span data-stu-id="74311-129">Blazor is a client-side web UI framework similar in nature to JavaScript front-end frameworks like Angular or React.</span></span> <span data-ttu-id="74311-130">Blazor 处理用户交互，并呈现必要的 UI 更新。</span><span class="sxs-lookup"><span data-stu-id="74311-130">Blazor handles user interactions and renders the necessary UI updates.</span></span> <span data-ttu-id="74311-131">Blazor 不基于请求-答复模式。</span><span class="sxs-lookup"><span data-stu-id="74311-131">Blazor *isn't* based on a request-reply model.</span></span> <span data-ttu-id="74311-132">用户交互处理为不在任何特定 HTTP 请求上下文中的事件。</span><span class="sxs-lookup"><span data-stu-id="74311-132">User interactions are handled as events that aren't in the context of any particular HTTP request.</span></span>

<span data-ttu-id="74311-133">Blazor 应用由一个或多个在 HTML 页上呈现的根组件组成。</span><span class="sxs-lookup"><span data-stu-id="74311-133">Blazor apps consist of one or more root components that are rendered on an HTML page.</span></span>

![HTML 中的 Blazor 组件](./media/architecture-comparison/blazor-components-in-html.png)

<span data-ttu-id="74311-135">用户指定组件的呈现位置的方式，以及组件针对用户交互的连接方式，是[托管模型](hosting-models.md)特有的。</span><span class="sxs-lookup"><span data-stu-id="74311-135">How the user specifies where components should render and how the components are then wired up for user interactions is [hosting model](hosting-models.md) specific.</span></span>

<span data-ttu-id="74311-136">Blazor [组件](components.md)是表示可重复使用的 UI 部分的 .NET 类。</span><span class="sxs-lookup"><span data-stu-id="74311-136">Blazor [components](components.md) are .NET classes that represent a reusable piece of UI.</span></span> <span data-ttu-id="74311-137">每个组件都维护其自己的状态，并指定其呈现逻辑，这可能包括呈现其他组件。</span><span class="sxs-lookup"><span data-stu-id="74311-137">Each component maintains its own state and specifies its own rendering logic, which can include rendering other components.</span></span> <span data-ttu-id="74311-138">组件为特定用户交互指定事件处理程序，以更新组件的状态。</span><span class="sxs-lookup"><span data-stu-id="74311-138">Components specify event handlers for specific user interactions to update the component's state.</span></span>

<span data-ttu-id="74311-139">组件处理事件后，Blazor 会呈现该组件并跟踪在呈现的输出中更改的内容。</span><span class="sxs-lookup"><span data-stu-id="74311-139">After a component handles an event, Blazor renders the component and keeps track of what changed in the rendered output.</span></span> <span data-ttu-id="74311-140">组件不会直接呈现为文档对象模型 (DOM)。</span><span class="sxs-lookup"><span data-stu-id="74311-140">Components don't render directly to the Document Object Model (DOM).</span></span> <span data-ttu-id="74311-141">而是呈现为 DOM 的内存中表示形式（名为 `RenderTree`），以便 Blazor 可以跟踪更改。</span><span class="sxs-lookup"><span data-stu-id="74311-141">They instead render to an in-memory representation of the DOM called a `RenderTree` so that Blazor can track the changes.</span></span> <span data-ttu-id="74311-142">Blazor 将新呈现的输出与以前的输出进行比较以计算 UI 差异，然后将其有效地应用于 DOM。</span><span class="sxs-lookup"><span data-stu-id="74311-142">Blazor compares the newly rendered output with the previous output to calculate a UI diff that it then applies efficiently to the DOM.</span></span>

![Blazor DOM 交互](./media/architecture-comparison/blazor-dom-interaction.png)

<span data-ttu-id="74311-144">还可手动指示组件应在其状态发生非正常 UI 事件的更改时呈现。</span><span class="sxs-lookup"><span data-stu-id="74311-144">Components can also manually indicate that they should be rendered if their state changes outside of a normal UI event.</span></span> <span data-ttu-id="74311-145">Blazor 使用 `SynchronizationContext` 来强制执行单个逻辑线程。</span><span class="sxs-lookup"><span data-stu-id="74311-145">Blazor uses a `SynchronizationContext` to enforce a single logical thread of execution.</span></span> <span data-ttu-id="74311-146">组件的生命周期方法和 Blazor 引发的任何事件回调都在此 `SynchronizationContext` 上执行。</span><span class="sxs-lookup"><span data-stu-id="74311-146">A component's lifecycle methods and any event callbacks that are raised by Blazor are executed on this `SynchronizationContext`.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="74311-147">[上一页](introduction.md)
>[下一页](hosting-models.md)</span><span class="sxs-lookup"><span data-stu-id="74311-147">[Previous](introduction.md)
[Next](hosting-models.md)</span></span>
