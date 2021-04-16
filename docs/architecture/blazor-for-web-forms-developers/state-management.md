---
title: 状态管理
description: 了解管理 ASP.NET Web Forms 和 Blazor 的状态的不同方法。
author: csharpfritz
ms.author: jefritz
ms.date: 05/15/2020
ms.openlocfilehash: 2ca811f886c6a245ac16c2bd66a68ff16e5bfc44
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "88267719"
---
# <a name="state-management"></a><span data-ttu-id="09ffd-103">状态管理</span><span class="sxs-lookup"><span data-stu-id="09ffd-103">State management</span></span>

<span data-ttu-id="09ffd-104">状态管理是 Web Forms 应用程序的关键概念，通过视图状态、会话状态、应用程序状态和回发等功能实现。</span><span class="sxs-lookup"><span data-stu-id="09ffd-104">State management is a key concept of Web Forms applications, facilitated through View State, Session State, Application State, and Postback features.</span></span> <span data-ttu-id="09ffd-105">框架的这些有状态功能有助于隐藏应用程序所需的状态管理，并使应用程序开发人员能够专注于交付其功能。</span><span class="sxs-lookup"><span data-stu-id="09ffd-105">These stateful features of the framework helped to hide the state management required for an application and allow application developers to focus on delivering their functionality.</span></span> <span data-ttu-id="09ffd-106">在 ASP.NET Core 和 Blazor 中，这些功能有一些已重新定位，有一些已彻底删除。</span><span class="sxs-lookup"><span data-stu-id="09ffd-106">With ASP.NET Core and Blazor, some of these features have been relocated and some have been removed altogether.</span></span> <span data-ttu-id="09ffd-107">本章介绍如何维护状态，并提供与 Blazor 的新功能相同的功能。</span><span class="sxs-lookup"><span data-stu-id="09ffd-107">This chapter reviews how to maintain state and deliver the same functionality with the new features in Blazor.</span></span>

## <a name="request-state-management-with-viewstate"></a><span data-ttu-id="09ffd-108">通过 ViewState 请求状态管理</span><span class="sxs-lookup"><span data-stu-id="09ffd-108">Request state management with ViewState</span></span>

<span data-ttu-id="09ffd-109">讨论 Web Forms 应用程序中的状态管理时，许多开发人员会立即想到 ViewState。</span><span class="sxs-lookup"><span data-stu-id="09ffd-109">When discussing state management in Web Forms application, many developers will immediately think of ViewState.</span></span> <span data-ttu-id="09ffd-110">在 Web Forms 中，ViewState 通过向浏览器来回发送大量经过编码的文本，来管理 HTTP 请求之间的内容状态。</span><span class="sxs-lookup"><span data-stu-id="09ffd-110">In Web Forms, ViewState manages the state of the content between HTTP requests by sending a large encoded block of text back and forth to the browser.</span></span> <span data-ttu-id="09ffd-111">ViewState 字段可能会被包含许多元素的页面的内容占用，从而可能扩展到几兆字节。</span><span class="sxs-lookup"><span data-stu-id="09ffd-111">The ViewState field could be overwhelmed with content from a page containing many elements, potentially expanding to several megabytes in size.</span></span>

<span data-ttu-id="09ffd-112">通过 Blazor Server，应用可以维护与服务器的持续连接。</span><span class="sxs-lookup"><span data-stu-id="09ffd-112">With Blazor Server, the app maintains an ongoing connection with the server.</span></span> <span data-ttu-id="09ffd-113">应用的状态（称为线路）会在连接处于活动状态时保留在服务器内存中。</span><span class="sxs-lookup"><span data-stu-id="09ffd-113">The app's state, called a *circuit*, is held in server memory while the connection is considered active.</span></span> <span data-ttu-id="09ffd-114">仅在用户离开应用或应用的特定页面时才会释放状态。</span><span class="sxs-lookup"><span data-stu-id="09ffd-114">State will only be disposed when the user navigates away from the app or a particular page in the app.</span></span> <span data-ttu-id="09ffd-115">活动组件的所有成员在与服务器的交互之间均可用。</span><span class="sxs-lookup"><span data-stu-id="09ffd-115">All members of the active components are available between interactions with the server.</span></span>

<span data-ttu-id="09ffd-116">此功能有以下优点：</span><span class="sxs-lookup"><span data-stu-id="09ffd-116">There are several advantages of this feature:</span></span>

- <span data-ttu-id="09ffd-117">组件状态随时可用并且不在交互之间重新生成。</span><span class="sxs-lookup"><span data-stu-id="09ffd-117">Component state is readily available and not rebuilt between interactions.</span></span>
- <span data-ttu-id="09ffd-118">状态不会传输到浏览器。</span><span class="sxs-lookup"><span data-stu-id="09ffd-118">State isn't transmitted to the browser.</span></span>

<span data-ttu-id="09ffd-119">但需要注意内存中组件状态持久性的以下缺点：</span><span class="sxs-lookup"><span data-stu-id="09ffd-119">However, there are some disadvantages to in-memory component state persistence to be aware of:</span></span>

- <span data-ttu-id="09ffd-120">如果服务器在请求之间重启，则状态将丢失。</span><span class="sxs-lookup"><span data-stu-id="09ffd-120">If the server restarts between request, state is lost.</span></span>
- <span data-ttu-id="09ffd-121">应用程序 Web 服务器负载均衡解决方案必须包含粘滞会话，确保来自相同浏览器的所有请求都返回到相同的服务器。</span><span class="sxs-lookup"><span data-stu-id="09ffd-121">Your application web server load-balancing solution must include sticky sessions to ensure that all requests from the same browser return to the same server.</span></span> <span data-ttu-id="09ffd-122">如果请求发送到其他服务器，则状态将丢失。</span><span class="sxs-lookup"><span data-stu-id="09ffd-122">If a request goes to a different server, state will be lost.</span></span>
- <span data-ttu-id="09ffd-123">服务器上的组件状态的持久性可能导致 Web 服务器上产生内存压力。</span><span class="sxs-lookup"><span data-stu-id="09ffd-123">Persistence of component state on the server can lead to memory pressure on the web server.</span></span>

<span data-ttu-id="09ffd-124">出于上述原因，请勿仅依赖驻留在服务器内存中的组件状态。</span><span class="sxs-lookup"><span data-stu-id="09ffd-124">For the preceding reasons, don't rely on just the state of the component to reside in-memory on the server.</span></span> <span data-ttu-id="09ffd-125">应用程序还应为请求之间的数据提供一些后备数据存储。</span><span class="sxs-lookup"><span data-stu-id="09ffd-125">Your application should also include some backing data store for data between requests.</span></span> <span data-ttu-id="09ffd-126">下面是有关此策略的一些简单示例：</span><span class="sxs-lookup"><span data-stu-id="09ffd-126">Some simple examples of this strategy:</span></span>

- <span data-ttu-id="09ffd-127">在购物车应用程序中，将添加到购物车中的新项的内容保留在数据库记录中。</span><span class="sxs-lookup"><span data-stu-id="09ffd-127">In a shopping cart application, persist the content of new items added to the cart in a database record.</span></span> <span data-ttu-id="09ffd-128">如果服务器的状态丢失，可以根据数据库记录重建该状态。</span><span class="sxs-lookup"><span data-stu-id="09ffd-128">If the state on the server is lost, you can reconstitute it from the database records.</span></span>
- <span data-ttu-id="09ffd-129">在多部分的 Web 窗体中，用户希望应用程序记住每个请求之间的值。</span><span class="sxs-lookup"><span data-stu-id="09ffd-129">In a multi-part web form, your users will expect your application to remember values between each request.</span></span> <span data-ttu-id="09ffd-130">将用户每个帖子之间的数据写入数据存储区，以便在多部分窗体完成时，可以将这些数据提取并组合到最终的窗体响应结构中。</span><span class="sxs-lookup"><span data-stu-id="09ffd-130">Write the data between each of your user's posts to a data store so that they can be fetched and assembled into the final form response structure when the multi-part form is completed.</span></span>

<span data-ttu-id="09ffd-131">有关管理 Blazor 应用状态的更多详细信息，请参阅 [ASP.NET Core Blazor 状态管理](/aspnet/core/blazor/state-management)。</span><span class="sxs-lookup"><span data-stu-id="09ffd-131">For additional details on managing state in Blazor apps, see [ASP.NET Core Blazor state management](/aspnet/core/blazor/state-management).</span></span>

## <a name="maintain-state-with-session"></a><span data-ttu-id="09ffd-132">通过会话维护状态</span><span class="sxs-lookup"><span data-stu-id="09ffd-132">Maintain state with Session</span></span>

<span data-ttu-id="09ffd-133">Web Forms 开发人员可以通过 <xref:Microsoft.AspNetCore.Http.ISession?displayProperty=nameWithType> 字典对象维护当前操作用户的信息。</span><span class="sxs-lookup"><span data-stu-id="09ffd-133">Web Forms developers could maintain information about the currently acting user with the <xref:Microsoft.AspNetCore.Http.ISession?displayProperty=nameWithType> dictionary object.</span></span> <span data-ttu-id="09ffd-134">将带有字符串键的对象添加到 `Session` 非常简单，并且该对象稍后在用户与应用程序的交互过程中可用。</span><span class="sxs-lookup"><span data-stu-id="09ffd-134">It's easy enough to add an object with a string key to the `Session`, and that object would be available at a later time during the user's interactions with the application.</span></span> <span data-ttu-id="09ffd-135">在尝试消除与 HTTP 的管理交互的过程中，通过 `Session` 对象可以轻松维护状态。</span><span class="sxs-lookup"><span data-stu-id="09ffd-135">In an attempt to eliminate managing interacting with HTTP, the `Session` object made it easy to maintain state.</span></span>

<span data-ttu-id="09ffd-136">.NET Framework `Session` 对象的签名与 ASP.NET Core `Session` 对象的签名不同。</span><span class="sxs-lookup"><span data-stu-id="09ffd-136">The signature of the .NET Framework `Session` object isn't the same as the ASP.NET Core `Session` object.</span></span> <span data-ttu-id="09ffd-137">决定迁移并使用新会话状态功能之前，请参阅[新 ASP.NET Core 会话的文档](/dotnet/api/microsoft.aspnetcore.http.isession)。</span><span class="sxs-lookup"><span data-stu-id="09ffd-137">Consider [the documentation for the new ASP.NET Core Session](/dotnet/api/microsoft.aspnetcore.http.isession) before deciding to migrate and use the new session state feature.</span></span>

<span data-ttu-id="09ffd-138">会话在 ASP.NET Core 和 Blazor Server 中可用，但为了将数据适当存储在数据存储库中，不建议使用。</span><span class="sxs-lookup"><span data-stu-id="09ffd-138">Session is available in ASP.NET Core and Blazor Server, but is discouraged from use in favor of storing data in a data repository appropriately.</span></span> <span data-ttu-id="09ffd-139">如果访问者出于隐私考虑而拒绝使用应用程序中的 HTTP cookie，则会话状态也不起作用。</span><span class="sxs-lookup"><span data-stu-id="09ffd-139">Session state is also not functional if visitors decline the use HTTP cookies in your application due to privacy concerns.</span></span>

<span data-ttu-id="09ffd-140">有关 ASP.NET Core 和会话状态的配置，请参阅 [ASP.NET Core 中的会话和状态管理](/aspnet/core/fundamentals/app-state#session-state)一文。</span><span class="sxs-lookup"><span data-stu-id="09ffd-140">Configuration for ASP.NET Core and Session state is available in the [Session and state management in ASP.NET Core article](/aspnet/core/fundamentals/app-state#session-state).</span></span>

## <a name="application-state"></a><span data-ttu-id="09ffd-141">应用程序状态</span><span class="sxs-lookup"><span data-stu-id="09ffd-141">Application state</span></span>

<span data-ttu-id="09ffd-142">Web Forms 框架中的 `Application` 对象为与应用程序范围的配置和状态进行交互提供了一个庞大的跨请求存储库。</span><span class="sxs-lookup"><span data-stu-id="09ffd-142">The `Application` object in the Web Forms framework provides a massive, cross-request repository for interacting with application-scope configuration and state.</span></span> <span data-ttu-id="09ffd-143">应用程序状态是存储各种应用程序配置属性的理想位置，这些属性由所有请求引用，而无论用户是否发出请求。</span><span class="sxs-lookup"><span data-stu-id="09ffd-143">Application state was an ideal place to store various application configuration properties that would be referenced by all requests, regardless of the user making the request.</span></span> <span data-ttu-id="09ffd-144">`Application` 对象的问题在于，数据不会在多个服务器之间保持不变。</span><span class="sxs-lookup"><span data-stu-id="09ffd-144">The problem with the `Application` object was that data didn't persist across multiple servers.</span></span> <span data-ttu-id="09ffd-145">应用程序对象的状态会在重启间丢失。</span><span class="sxs-lookup"><span data-stu-id="09ffd-145">The state of the application object was lost between restarts.</span></span>

<span data-ttu-id="09ffd-146">对于 `Session`，建议将数据移动到多个服务器实例可以访问的永久性后备存储。</span><span class="sxs-lookup"><span data-stu-id="09ffd-146">As with `Session`, it's recommended that data move to a persistent backing store that could be accessed by multiple server instances.</span></span> <span data-ttu-id="09ffd-147">如果存在要跨请求和用户访问的易失性数据，可直接将其存储在单一服务中，该服务可注入到需要此信息或交互的组件中。</span><span class="sxs-lookup"><span data-stu-id="09ffd-147">If there is volatile data that you would like to be able to access across requests and users, you could easily store it in a singleton service that can be injected into components that require this information or interaction.</span></span>

<span data-ttu-id="09ffd-148">维护应用程序状态及其消耗的对象结构与以下实现类似：</span><span class="sxs-lookup"><span data-stu-id="09ffd-148">The construction of an object to maintain application state and its consumption could resemble the following implementation:</span></span>

```csharp
public class MyApplicationState
{
    public int VisitorCounter { get; private set; } = 0;

    public void IncrementCounter() => VisitorCounter += 1;
}
```

```csharp
app.AddSingleton<MyApplicationState>();
```

```razor
@inject MyApplicationState AppState

<label>Total Visitors: @AppState.VisitorCounter</label>
```

<span data-ttu-id="09ffd-149">`MyApplicationState` 对象仅在服务器上创建一次，值 `VisitorCounter` 被提取后会输出到组件的标签中。</span><span class="sxs-lookup"><span data-stu-id="09ffd-149">The `MyApplicationState` object is created only once on the server, and the value `VisitorCounter` is fetched and output in the component's label.</span></span> <span data-ttu-id="09ffd-150">`VisitorCounter` 值应保留，并应从后备数据存储中进行检索以实现持久性和可伸缩性。</span><span class="sxs-lookup"><span data-stu-id="09ffd-150">The `VisitorCounter` value should be persisted and retrieved from a backing data store for durability and scalability.</span></span>

## <a name="in-the-browser"></a><span data-ttu-id="09ffd-151">在浏览器中</span><span class="sxs-lookup"><span data-stu-id="09ffd-151">In the browser</span></span>

<span data-ttu-id="09ffd-152">应用程序数据还可以存储在用户设备的客户端，以便以后可用。</span><span class="sxs-lookup"><span data-stu-id="09ffd-152">Application data can also be stored client-side on the user's device so that is available later.</span></span> <span data-ttu-id="09ffd-153">可通过以下两种浏览器功能在用户浏览器的不同范围内实现数据持久性：</span><span class="sxs-lookup"><span data-stu-id="09ffd-153">There are two browser features that allow for persistence of data in different scopes of the user's browser:</span></span>

- <span data-ttu-id="09ffd-154">`localStorage` - 范围是用户的整个浏览器。</span><span class="sxs-lookup"><span data-stu-id="09ffd-154">`localStorage` - scoped to the user's entire browser.</span></span> <span data-ttu-id="09ffd-155">如果页面被重新加载，浏览器将关闭并重新打开；如果使用相同的 URL 打开另一个选项卡，浏览器将提供相同的 `localStorage`</span><span class="sxs-lookup"><span data-stu-id="09ffd-155">If the page is reloaded, the browser is closed and reopened, or another tab is opened with the same URL then the same `localStorage` is provided by the browser</span></span>
- <span data-ttu-id="09ffd-156">`sessionStorage` - 范围为用户当前的浏览器选项卡。如果该选项卡被重载，状态将保持不变。</span><span class="sxs-lookup"><span data-stu-id="09ffd-156">`sessionStorage` - scoped to the user's current browser tab. If the tab is reloaded, the state persists.</span></span> <span data-ttu-id="09ffd-157">但是，如果用户打开应用程序的另一个选项卡，或者关闭并重新打开浏览器，则状态将丢失。</span><span class="sxs-lookup"><span data-stu-id="09ffd-157">However, if the user opens another tab to your application or closes and reopens the browser the state is lost.</span></span>

<span data-ttu-id="09ffd-158">你可以写入一些与这些功能进行交互的自定义 JavaScript 代码，或者使用一些提供此功能的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="09ffd-158">You can write some custom JavaScript code to interact with these features, or there are a number of NuGet packages that you can use that provide this functionality.</span></span> <span data-ttu-id="09ffd-159">例如 [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage) 包。</span><span class="sxs-lookup"><span data-stu-id="09ffd-159">One such package is [Microsoft.AspNetCore.ProtectedBrowserStorage](https://www.nuget.org/packages/Microsoft.AspNetCore.ProtectedBrowserStorage).</span></span>

<span data-ttu-id="09ffd-160">有关使用此包与 `localStorage` 和 `sessionStorage` 进行交互的说明，请参阅 [Blazor 状态管理](/aspnet/core/blazor/state-management#protected-browser-storage-experimental-package)一文。</span><span class="sxs-lookup"><span data-stu-id="09ffd-160">For instructions on utilizing this package to interact with `localStorage` and `sessionStorage`, see the [Blazor State Management](/aspnet/core/blazor/state-management#protected-browser-storage-experimental-package) article.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="09ffd-161">[上一页](pages-routing-layouts.md)
>[下一页](forms-validation.md)</span><span class="sxs-lookup"><span data-stu-id="09ffd-161">[Previous](pages-routing-layouts.md)
[Next](forms-validation.md)</span></span>
