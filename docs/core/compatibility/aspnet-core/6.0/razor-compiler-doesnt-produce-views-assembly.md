---
title: 中断性变更：Razor：编译器现在生成单个程序集
description: 了解 ASP.NET Core 6.0 中的中断性变更，其中，Razor 编译器不会再使用两步编译过程来生成两个单独的程序集。
no-loc:
- Razor
ms.date: 04/08/2021
ms.openlocfilehash: 8b6817563c4670aebfe681d5f24c8e309d2500ad
ms.sourcegitcommit: fdfa01f6cd3aa4c36b6e8a1830693ff22d35aeea
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/13/2021
ms.locfileid: "107299275"
---
# <a name="razor-compiler-no-longer-produces-a-views-assembly"></a><span data-ttu-id="a67df-103">Razor：编译器不再生成 Views 程序集</span><span class="sxs-lookup"><span data-stu-id="a67df-103">Razor: Compiler no longer produces a Views assembly</span></span>

<span data-ttu-id="a67df-104">Razor 编译器不再生成单独的 Views.dll 文件，该文件中包含在应用程序中定义的 CSHTML 视图。</span><span class="sxs-lookup"><span data-stu-id="a67df-104">The Razor compiler no longer produces a separate *Views.dll* file that contains the CSHTML views defined in an application.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="a67df-105">引入的版本</span><span class="sxs-lookup"><span data-stu-id="a67df-105">Version introduced</span></span>

<span data-ttu-id="a67df-106">ASP.NET Core 6.0 预览版 3</span><span class="sxs-lookup"><span data-stu-id="a67df-106">ASP.NET Core 6.0 Preview 3</span></span>

## <a name="old-behavior"></a><span data-ttu-id="a67df-107">旧行为</span><span class="sxs-lookup"><span data-stu-id="a67df-107">Old behavior</span></span>

<span data-ttu-id="a67df-108">在以前的版本中，Razor 编译器会利用两步编译过程，该过程将生成两个文件：</span><span class="sxs-lookup"><span data-stu-id="a67df-108">In previous versions, the Razor compiler utilizes a two-step compilation process that produces two files:</span></span>

- <span data-ttu-id="a67df-109">一个主 AppName.dll 程序集，其中包含应用程序类型。</span><span class="sxs-lookup"><span data-stu-id="a67df-109">A main *AppName.dll* assembly that contains application types.</span></span>
- <span data-ttu-id="a67df-110">一个 AppName.Views.dll 程序集，其中包含在应用中定义的生成的视图。</span><span class="sxs-lookup"><span data-stu-id="a67df-110">An *AppName.Views.dll* assembly that contains the generated views that are defined in the app.</span></span> <span data-ttu-id="a67df-111">生成的视图类型为 `public`，位于 `AspNetCore` 命名空间下。</span><span class="sxs-lookup"><span data-stu-id="a67df-111">Generated view types are `public` and under the `AspNetCore` namespace.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="a67df-112">新行为</span><span class="sxs-lookup"><span data-stu-id="a67df-112">New behavior</span></span>

<span data-ttu-id="a67df-113">视图和应用程序类型都包含在一个 AppName.dll 程序集中。</span><span class="sxs-lookup"><span data-stu-id="a67df-113">Both views and application types are included in a single *AppName.dll* assembly.</span></span> <span data-ttu-id="a67df-114">默认情况下，视图类型具有可访问性修饰符 `internal` 和 `sealed`，并包含在 `AspNetCoreGeneratedDocument` 名称空间下。</span><span class="sxs-lookup"><span data-stu-id="a67df-114">View types have the accessibility modifiers `internal` and `sealed`, by default, and are included under the `AspNetCoreGeneratedDocument` namespace.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="a67df-115">更改原因</span><span class="sxs-lookup"><span data-stu-id="a67df-115">Reason for change</span></span>

<span data-ttu-id="a67df-116">删除该两步编译过程：</span><span class="sxs-lookup"><span data-stu-id="a67df-116">Removing the two-step compilation process:</span></span>

* <span data-ttu-id="a67df-117">提高使用 Razor 视图的应用程序的生成性能。</span><span class="sxs-lookup"><span data-stu-id="a67df-117">Improves build performance for applications that use Razor views.</span></span>
* <span data-ttu-id="a67df-118">允许 Razor 视图参与 Visual Studio 的“热重载”体验。</span><span class="sxs-lookup"><span data-stu-id="a67df-118">Allows Razor views to participate in the "hot reload" experience for Visual Studio.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="a67df-119">建议操作</span><span class="sxs-lookup"><span data-stu-id="a67df-119">Recommended action</span></span>

<span data-ttu-id="a67df-120">无。</span><span class="sxs-lookup"><span data-stu-id="a67df-120">None.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="a67df-121">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="a67df-121">Affected APIs</span></span>

<span data-ttu-id="a67df-122">无。</span><span class="sxs-lookup"><span data-stu-id="a67df-122">None.</span></span>

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis

-->
