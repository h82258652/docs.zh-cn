---
title: 中断性变更：SignalR：MessagePack 中心协议选项类型已更改
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：SignalR：MessagePack 中心协议选项类型已更改
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: 5a1a3728e4f842074c80f5063dcfe47ad8858674
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497913"
---
# <a name="signalr-messagepack-hub-protocol-options-type-changed"></a><span data-ttu-id="95564-103">SignalR：MessagePack 中心协议选项类型已更改</span><span class="sxs-lookup"><span data-stu-id="95564-103">SignalR: MessagePack Hub Protocol options type changed</span></span>

<span data-ttu-id="95564-104">ASP.NET Core SignalR MessagePack 中心协议选项类型已从 `IList<MessagePack.IFormatterResolver>` 更改为 [MessagePack](https://www.nuget.org/packages/MessagePack) 库的 `MessagePackSerializerOptions` 类型。</span><span class="sxs-lookup"><span data-stu-id="95564-104">The ASP.NET Core SignalR MessagePack Hub Protocol options type has changed from `IList<MessagePack.IFormatterResolver>` to the [MessagePack](https://www.nuget.org/packages/MessagePack) library's `MessagePackSerializerOptions` type.</span></span>

<span data-ttu-id="95564-105">有关此更改的讨论，请参阅 [dotnet/aspnetcore#20506](https://github.com/dotnet/aspnetcore/issues/20506)。</span><span class="sxs-lookup"><span data-stu-id="95564-105">For discussion on this change, see [dotnet/aspnetcore#20506](https://github.com/dotnet/aspnetcore/issues/20506).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="95564-106">引入的版本</span><span class="sxs-lookup"><span data-stu-id="95564-106">Version introduced</span></span>

<span data-ttu-id="95564-107">5.0 预览版 4</span><span class="sxs-lookup"><span data-stu-id="95564-107">5.0 Preview 4</span></span>

## <a name="old-behavior"></a><span data-ttu-id="95564-108">旧行为</span><span class="sxs-lookup"><span data-stu-id="95564-108">Old behavior</span></span>

<span data-ttu-id="95564-109">可以添加到选项，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="95564-109">You can add to the options as shown in the following example:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers.Add(MessagePack.Resolvers.StandardResolver.Instance);
    });
```

<span data-ttu-id="95564-110">并替换选项，如下所示：</span><span class="sxs-lookup"><span data-stu-id="95564-110">And replace the options as follows:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.FormatterResolvers = new List<MessagePack.IFormatterResolver>()
        {
            MessagePack.Resolvers.StandardResolver.Instance
        };
    });
```

## <a name="new-behavior"></a><span data-ttu-id="95564-111">新行为</span><span class="sxs-lookup"><span data-stu-id="95564-111">New behavior</span></span>

<span data-ttu-id="95564-112">可以添加到选项，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="95564-112">You can add to the options as shown in the following example:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions =
            options.SerializeOptions.WithResolver(MessagePack.Resolvers.StandardResolver.Instance);
    });
```

<span data-ttu-id="95564-113">并替换选项，如下所示：</span><span class="sxs-lookup"><span data-stu-id="95564-113">And replace the options as follows:</span></span>

```csharp
services.AddSignalR()
    .AddMessagePackProtocol(options =>
    {
        options.SerializerOptions = MessagePackSerializerOptions
                .Standard
                .WithResolver(MessagePack.Resolvers.StandardResolver.Instance)
                .WithSecurity(MessagePackSecurity.UntrustedData);
    });
```

## <a name="reason-for-change"></a><span data-ttu-id="95564-114">更改原因</span><span class="sxs-lookup"><span data-stu-id="95564-114">Reason for change</span></span>

<span data-ttu-id="95564-115">此更改是迁移到 MessagePack v2.x 的一部分，已在 [aspnet/Announcements#404](https://github.com/aspnet/Announcements/issues/404) 中公布。</span><span class="sxs-lookup"><span data-stu-id="95564-115">This change is part of moving to MessagePack v2.x, which was announced in [aspnet/Announcements#404](https://github.com/aspnet/Announcements/issues/404).</span></span> <span data-ttu-id="95564-116">v2.x 库添加了一个更易于使用的选项 API，提供的功能比之前公开的 `MessagePack.IFormatterResolver` 的列表更多。</span><span class="sxs-lookup"><span data-stu-id="95564-116">The v2.x library has added an options API that's easier to use and provides more features than the list of `MessagePack.IFormatterResolver` that was exposed before.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="95564-117">建议操作</span><span class="sxs-lookup"><span data-stu-id="95564-117">Recommended action</span></span>

<span data-ttu-id="95564-118">此重大更改将影响在 <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions> 上配置值的用户。</span><span class="sxs-lookup"><span data-stu-id="95564-118">This breaking change affects anyone who is configuring values on <xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions>.</span></span> <span data-ttu-id="95564-119">如果使用 ASP.NET Core SignalR MessagePack 中心协议并修改这些选项，请更新用法以使用新的选项 API，如上所示。</span><span class="sxs-lookup"><span data-stu-id="95564-119">If you're using the ASP.NET Core SignalR MessagePack Hub Protocol and modifying the options, update your usage to use the new options API as shown above.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="95564-120">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="95564-120">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions?displayProperty=nameWithType>

<!--

### Category

ASP.NET Core

### Affected APIs

`T:Microsoft.AspNetCore.SignalR.MessagePackHubProtocolOptions`

-->
