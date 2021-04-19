---
title: 中断性变更：更改了 signalr-protocol-msgpack 包中的 MessagePack 库
description: 了解 ASP.NET Core 6.0 中的中断性变更：更改了 MessagePack 库，并在 @microsoft/signalr-protocol-msgpack 包中删除了两个选项。
ms.date: 04/07/2021
ms.openlocfilehash: 62f853cbed0904e278e25f231528845baac9f71d
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107260026"
---
# <a name="changed-messagepack-library-in-microsoftsignalr-protocol-msgpack"></a><span data-ttu-id="bd18c-103">更改了 @microsoft/signalr-protocol-msgpack 中的 MessagePack 库</span><span class="sxs-lookup"><span data-stu-id="bd18c-103">Changed MessagePack library in @microsoft/signalr-protocol-msgpack</span></span>

<span data-ttu-id="bd18c-104">npm 包 [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) 现在引用 `@msgpack/msgpack`，而不是引用 `msgpack5`。</span><span class="sxs-lookup"><span data-stu-id="bd18c-104">The [@microsoft/signalr-protocol-msgpack](https://www.npmjs.com/package/@microsoft/signalr-protocol-msgpack) npm package now references `@msgpack/msgpack` instead of `msgpack5`.</span></span> <span data-ttu-id="bd18c-105">此外，更改了可选择性地传递到 `MessagePackHubProtocol` 的可用选项。</span><span class="sxs-lookup"><span data-stu-id="bd18c-105">Additionally, the available options that can optionally be passed into the `MessagePackHubProtocol` have changed.</span></span> <span data-ttu-id="bd18c-106">删除了 `MessagePackOptions.disableTimestampEncoding` 和 `MessagePackOptions.forceFloat64` 属性，并添加了一些新选项。</span><span class="sxs-lookup"><span data-stu-id="bd18c-106">The `MessagePackOptions.disableTimestampEncoding` and `MessagePackOptions.forceFloat64` properties were removed, and some new options were added.</span></span>

<span data-ttu-id="bd18c-107">有关讨论内容，请参阅 <https://github.com/dotnet/aspnetcore/issues/30471>。</span><span class="sxs-lookup"><span data-stu-id="bd18c-107">For discussion, see <https://github.com/dotnet/aspnetcore/issues/30471>.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="bd18c-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="bd18c-108">Version introduced</span></span>

<span data-ttu-id="bd18c-109">ASP.NET Core 6.0</span><span class="sxs-lookup"><span data-stu-id="bd18c-109">ASP.NET Core 6.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="bd18c-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="bd18c-110">Old behavior</span></span>

<span data-ttu-id="bd18c-111">在以前的版本中，必须包含三个脚本引用才能在浏览器中使用 [MessagePack Hub 协议](/aspnet/core/signalr/messagepackhubprotocol)：</span><span class="sxs-lookup"><span data-stu-id="bd18c-111">In previous versions, you must include three script references to use the [MessagePack Hub Protocol](/aspnet/core/signalr/messagepackhubprotocol) in the browser:</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/msgpack5/msgpack5.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

## <a name="new-behavior"></a><span data-ttu-id="bd18c-112">新行为</span><span class="sxs-lookup"><span data-stu-id="bd18c-112">New behavior</span></span>

<span data-ttu-id="bd18c-113">从 ASP.NET Core 6 开始，只需要两个脚本引用即可在浏览器中使用 [MessagePack Hub 协议](/aspnet/core/signalr/messagepackhubprotocol)：</span><span class="sxs-lookup"><span data-stu-id="bd18c-113">Starting in ASP.NET Core 6, you only need two script references to use the [MessagePack Hub Protocol](/aspnet/core/signalr/messagepackhubprotocol) in the browser:</span></span>

```html
<script src="~/lib/signalr/signalr.js"></script>
<script src="~/lib/signalr/signalr-protocol-msgpack.js"></script>
```

<span data-ttu-id="bd18c-114">如果想要直接在应用中使用，则会将 `@msgpack/msgpack` 包（而不是 `msgpack5` 包）下载到 node_modules 目录中。</span><span class="sxs-lookup"><span data-stu-id="bd18c-114">Instead of the `msgpack5` package, the `@msgpack/msgpack` package is downloaded to your *node_modules* directory if you want to use it directly in your app.</span></span>

<span data-ttu-id="bd18c-115">最后，`MessagePackOptions` 具有新的附加属性，并且删除了 `disableTimestampEncoding` 和 `forceFloat64` 属性。</span><span class="sxs-lookup"><span data-stu-id="bd18c-115">Finally, `MessagePackOptions` has new, additional properties, and the `disableTimestampEncoding` and `forceFloat64` properties are removed.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="bd18c-116">更改原因</span><span class="sxs-lookup"><span data-stu-id="bd18c-116">Reason for change</span></span>

<span data-ttu-id="bd18c-117">进行此更改是为了减小资产大小、更简单地使用包并添加更多可定制性。</span><span class="sxs-lookup"><span data-stu-id="bd18c-117">This change was made to reduce asset size, make it simpler to consume the package, and add more customizability.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="bd18c-118">建议操作</span><span class="sxs-lookup"><span data-stu-id="bd18c-118">Recommended action</span></span>

<span data-ttu-id="bd18c-119">如果以前在应用中使用 `msgpack5`，则需要在 package.json 文件中添加对库的直接引用。</span><span class="sxs-lookup"><span data-stu-id="bd18c-119">If you were previously using `msgpack5` in your app, you'll need to add a direct reference to the library in your *package.json* file.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="bd18c-120">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="bd18c-120">Affected APIs</span></span>

<span data-ttu-id="bd18c-121">删除了以下 API：</span><span class="sxs-lookup"><span data-stu-id="bd18c-121">The following APIs were removed:</span></span>

- `MessagePackOptions.disableTimestampEncoding`
- `MessagePackOptions.forceFloat64`

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis.

-->
