---
title: 中断性变更：SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 6851c4fdbaab61e7655dd71ba3c21b8835b2b855
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497107"
---
# <a name="signalr-messagepack-hub-protocol-moved-to-messagepack-2x-package"></a><span data-ttu-id="27cfd-103">SignalR：MessagePack 集线器协议已移至 MessagePack 2.x 包</span><span class="sxs-lookup"><span data-stu-id="27cfd-103">SignalR: MessagePack Hub Protocol moved to MessagePack 2.x package</span></span>

<span data-ttu-id="27cfd-104">ASP.NET Core SignalR [MessagePack 集线器协议](/aspnet/core/signalr/messagepackhubprotocol)使用 [MessagePack NuGet 包](https://www.nuget.org/packages/MessagePack)实现 MessagePack 序列化。</span><span class="sxs-lookup"><span data-stu-id="27cfd-104">The ASP.NET Core SignalR [MessagePack Hub Protocol](/aspnet/core/signalr/messagepackhubprotocol) uses the [MessagePack NuGet package](https://www.nuget.org/packages/MessagePack) for MessagePack serialization.</span></span> <span data-ttu-id="27cfd-105">ASP.NET Core 5.0 将包从 1.x 升级到最新的 2.x 包版本。</span><span class="sxs-lookup"><span data-stu-id="27cfd-105">ASP.NET Core 5.0 upgrades the package from 1.x to the latest 2.x package version.</span></span>

<span data-ttu-id="27cfd-106">有关此问题的讨论，请参阅 [dotnet/aspnetcore#18692](https://github.com/dotnet/aspnetcore/issues/18692)。</span><span class="sxs-lookup"><span data-stu-id="27cfd-106">For discussion on this issue, see [dotnet/aspnetcore#18692](https://github.com/dotnet/aspnetcore/issues/18692).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="27cfd-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="27cfd-107">Version introduced</span></span>

<span data-ttu-id="27cfd-108">5.0 预览版 1</span><span class="sxs-lookup"><span data-stu-id="27cfd-108">5.0 Preview 1</span></span>

## <a name="old-behavior"></a><span data-ttu-id="27cfd-109">旧行为</span><span class="sxs-lookup"><span data-stu-id="27cfd-109">Old behavior</span></span>

<span data-ttu-id="27cfd-110">ASP.NET Core SignalR 以前使用 MessagePack 1.x 包对 MessagePack 消息进行序列化和反序列化。</span><span class="sxs-lookup"><span data-stu-id="27cfd-110">ASP.NET Core SignalR used the MessagePack 1.x package to serialize and deserialize MessagePack messages.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="27cfd-111">新行为</span><span class="sxs-lookup"><span data-stu-id="27cfd-111">New behavior</span></span>

<span data-ttu-id="27cfd-112">ASP.NET Core SignalR 使用 MessagePack 2.x 包对 MessagePack 消息进行序列化和反序列化。</span><span class="sxs-lookup"><span data-stu-id="27cfd-112">ASP.NET Core SignalR uses the MessagePack 2.x package to serialize and deserialize MessagePack messages.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="27cfd-113">更改原因</span><span class="sxs-lookup"><span data-stu-id="27cfd-113">Reason for change</span></span>

<span data-ttu-id="27cfd-114">MessagePack 2.x 包中的最新改进添加了有用的功能。</span><span class="sxs-lookup"><span data-stu-id="27cfd-114">The latest improvements in the MessagePack 2.x package add useful functionality.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="27cfd-115">建议操作</span><span class="sxs-lookup"><span data-stu-id="27cfd-115">Recommended action</span></span>

<span data-ttu-id="27cfd-116">此重大更改适用于以下情况：</span><span class="sxs-lookup"><span data-stu-id="27cfd-116">This breaking change applies when:</span></span>

* <span data-ttu-id="27cfd-117">在 <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions> 上设置或配置值。</span><span class="sxs-lookup"><span data-stu-id="27cfd-117">Setting or configuring values on <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions>.</span></span>
* <span data-ttu-id="27cfd-118">直接使用 MessagePack API，以及在同一项目中使用 ASP.NET Core SignalR MessagePack 集线器协议。</span><span class="sxs-lookup"><span data-stu-id="27cfd-118">Using the MessagePack APIs directly and using the ASP.NET Core SignalR MessagePack Hub Protocol in the same project.</span></span> <span data-ttu-id="27cfd-119">将加载较新的版本，而不是以前的版本。</span><span class="sxs-lookup"><span data-stu-id="27cfd-119">The newer version will be loaded instead of the previous version.</span></span>

<span data-ttu-id="27cfd-120">若要获得包创建者的迁移指导，请参阅[从 MessagePack v1.x 迁移到 MessagePack v2.x](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md)。</span><span class="sxs-lookup"><span data-stu-id="27cfd-120">For migration guidance from the package authors, see [Migrating from MessagePack v1.x to MessagePack v2.x](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md).</span></span> <span data-ttu-id="27cfd-121">消息序列化和反序列化的某些方面会受到影响。</span><span class="sxs-lookup"><span data-stu-id="27cfd-121">Some aspects of message serialization and deserialization are affected.</span></span> <span data-ttu-id="27cfd-122">具体而言，[对 DateTime 值的序列化方式进行了行为上的更改](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md#behavioral-changes)。</span><span class="sxs-lookup"><span data-stu-id="27cfd-122">Specifically, there are [behavioral changes to how DateTime values are serialized](https://github.com/neuecc/MessagePack-CSharp/blob/master/doc/migration.md#behavioral-changes).</span></span>

## <a name="affected-apis"></a><span data-ttu-id="27cfd-123">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="27cfd-123">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions`

-->
