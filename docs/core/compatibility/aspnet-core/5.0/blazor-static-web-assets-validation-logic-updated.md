---
title: 中断性变更：Blazor：更新的静态 Web 资产的验证逻辑
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：Blazor：更新的静态 Web 资产的验证逻辑
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: b6fb8e9a10157a97119fa07a95fb3dbb8b207214
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498147"
---
# <a name="blazor-updated-validation-logic-for-static-web-assets"></a><span data-ttu-id="3fe5f-103">Blazor：更新的静态 Web 资产的验证逻辑</span><span class="sxs-lookup"><span data-stu-id="3fe5f-103">Blazor: Updated validation logic for static web assets</span></span>

<span data-ttu-id="3fe5f-104">在 ASP.NET Core 3.1 和 Blazor WebAssembly 3.2 中，静态 Web 资产的冲突验证存在问题。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-104">There was an issue in conflict validation for static web assets in ASP.NET Core 3.1 and Blazor WebAssembly 3.2.</span></span> <span data-ttu-id="3fe5f-105">问题：</span><span class="sxs-lookup"><span data-stu-id="3fe5f-105">The issue:</span></span>

* <span data-ttu-id="3fe5f-106">阻碍了主机资产和 Razor 类库 (RCL) 及 Blazor WebAssembly 应用中资产之间的正常冲突检测。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-106">Prevented proper conflict detection between the host assets and assets from Razor Class Libraries (RCLs) and Blazor WebAssembly apps.</span></span>
* <span data-ttu-id="3fe5f-107">通常会影响 Blazor WebAssembly 应用，因为默认情况下，RCL 中的静态 Web 资产在 `_content/$(PackageId)` 前缀下提供。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-107">Mostly affects Blazor WebAssembly apps, since by default, static web assets in RCLs are served under the `_content/$(PackageId)` prefix.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="3fe5f-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="3fe5f-108">Version introduced</span></span>

<span data-ttu-id="3fe5f-109">5.0</span><span class="sxs-lookup"><span data-stu-id="3fe5f-109">5.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="3fe5f-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="3fe5f-110">Old behavior</span></span>

<span data-ttu-id="3fe5f-111">在开发过程中，RCL 的静态 Web 资产可能会被同一主机路径上的主机项目资产以静默方式重写。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-111">During development, an RCL's static web assets could be silently overridden with host project assets on the same host path.</span></span> <span data-ttu-id="3fe5f-112">假设有一个 RCL 定义了要在 /folder/file.txt 提供的静态 Web 资产。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-112">Consider an RCL that has defined a static web asset to be served at */folder/file.txt*.</span></span> <span data-ttu-id="3fe5f-113">如果主机将文件放在 wwwroot/folder/file.txt，则服务器上的文件会以静默方式重写 RCL 或 Blazor WebAssembly 应用上的文件。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-113">If the host placed a file at *wwwroot/folder/file.txt*, the file on the server silently overrode the file on the RCL or Blazor WebAssembly app.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="3fe5f-114">新行为</span><span class="sxs-lookup"><span data-stu-id="3fe5f-114">New behavior</span></span>

<span data-ttu-id="3fe5f-115">ASP.NET Core 正确检测何时发生此问题。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-115">ASP.NET Core correctly detects when this issue happens.</span></span> <span data-ttu-id="3fe5f-116">它会通知你（即用户）存在冲突，以便你可以采取适当的操作。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-116">It informs you, the user, of the conflict so that you can take the appropriate action.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="3fe5f-117">更改原因</span><span class="sxs-lookup"><span data-stu-id="3fe5f-117">Reason for change</span></span>

<span data-ttu-id="3fe5f-118">静态 Web 资产不应由项目的 wwwroot 主机上的文件替代。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-118">Static web assets weren't intended to be overridable by files on the *wwwroot* host of the project.</span></span> <span data-ttu-id="3fe5f-119">允许重写这些文件可能会导致难以诊断的错误。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-119">Allowing for the overriding of those files could lead to errors that are difficult to diagnose.</span></span> <span data-ttu-id="3fe5f-120">结果可能是已发布应用中未定义的行为更改。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-120">The result could be undefined behavior changes in published apps.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="3fe5f-121">建议操作</span><span class="sxs-lookup"><span data-stu-id="3fe5f-121">Recommended action</span></span>

<span data-ttu-id="3fe5f-122">默认情况下，RCL 文件没有理由与主机上的文件冲突。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-122">By default, there's no reason for an RCL file to conflict with a file on the host.</span></span> <span data-ttu-id="3fe5f-123">RCL 文件以 `_content/${PackageId}` 为前缀。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-123">RCL files are prefixed with `_content/${PackageId}`.</span></span> <span data-ttu-id="3fe5f-124">Blazor WebAssembly 文件位于主机 URL 空间的根目录中，这使得更容易出现冲突。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-124">Blazor WebAssembly files are placed at the root of the host URL space, which makes conflicts easier.</span></span> <span data-ttu-id="3fe5f-125">例如，Blazor WebAssembly 应用包含 favicon.ico 文件，主机可能包含在其 wwwroot 文件夹中 。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-125">For example, Blazor WebAssembly apps contain a *favicon.ico* file that the host might also include in its *wwwroot* folder.</span></span>

<span data-ttu-id="3fe5f-126">如果冲突的源是 RCL 文件，通常意味着代码将资产从库复制到项目的 wwwroot 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-126">If the conflict's source is an RCL file, it often means code is copying assets from the library into the project's *wwwroot* folder.</span></span> <span data-ttu-id="3fe5f-127">编写代码来复制文件会破坏静态 Web 资产的主要目标。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-127">Writing code to copy files defeats a primary goal of static web assets.</span></span> <span data-ttu-id="3fe5f-128">必须实现此目标，才能在更新内容时获取对浏览器的更新，而无需触发新的编译。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-128">This goal is fundamental to get updates on the browser when the contents are updated without having to trigger a new compilation.</span></span>

<span data-ttu-id="3fe5f-129">可以选择保留此行为并维护主机上的文件。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-129">You may choose to preserve this behavior and maintain the file on the host.</span></span> <span data-ttu-id="3fe5f-130">为此，请从具有自定义 MSBuild 目标的静态 Web 资产列表中删除该文件。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-130">To do so, remove the file from the list of static web assets with a custom MSBuild target.</span></span>

<span data-ttu-id="3fe5f-131">若要使用 RCL 的文件或 Blazor WebAssembly 应用的文件而不是主机项目的文件，请从主机项目中删除该文件。</span><span class="sxs-lookup"><span data-stu-id="3fe5f-131">To use the RCL's file or the Blazor WebAssembly app's file instead of the host project's file, remove the file from the host project.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="3fe5f-132">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="3fe5f-132">Affected APIs</span></span>

<span data-ttu-id="3fe5f-133">无</span><span class="sxs-lookup"><span data-stu-id="3fe5f-133">None</span></span>

<!--

### Category

ASP.NET Core

### Affected APIs

Not detectable via API analysis

-->
