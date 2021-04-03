---
title: 比较 ASP.NET SignalR 和 ASP.NET Core SignalR
description: ASP.NET Core SignalR 的不同之处在于 ASP.NET SignalR？
author: ardalis
ms.date: 11/13/2020
ms.openlocfilehash: 4a8680d8a28faaa07687b2c5835ebbf428032fbe
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122648"
---
# <a name="compare-aspnet-signalr-and-aspnet-core-signalr"></a><span data-ttu-id="018c6-103">比较 ASP.NET SignalR 和 ASP.NET Core SignalR</span><span class="sxs-lookup"><span data-stu-id="018c6-103">Compare ASP.NET SignalR and ASP.NET Core SignalR</span></span>

<span data-ttu-id="018c6-104">ASP.NET Core SignalR 与使用 ASP.NET SignalR 的客户端或服务器不兼容。</span><span class="sxs-lookup"><span data-stu-id="018c6-104">ASP.NET Core SignalR is incompatible with clients or servers using ASP.NET SignalR.</span></span> <span data-ttu-id="018c6-105">需要更新客户端和服务器以使用 ASP.NET Core SignalR。</span><span class="sxs-lookup"><span data-stu-id="018c6-105">You'll need to update both clients and server to use ASP.NET Core SignalR.</span></span> <span data-ttu-id="018c6-106">此部分中介绍了一些差异，而 [文档](/aspnet/core/signalr/version-differences)中提供了完整列表。ASP.NET Core SignalR 需要 .NET Core 2.1 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="018c6-106">Some differences are described in this section, while the full list is available in the [docs](/aspnet/core/signalr/version-differences). ASP.NET Core SignalR requires .NET Core 2.1 or greater.</span></span>

## <a name="feature-differences"></a><span data-ttu-id="018c6-107">功能差异</span><span class="sxs-lookup"><span data-stu-id="018c6-107">Feature differences</span></span>

- <span data-ttu-id="018c6-108">ASP.NET SignalR 自动尝试重新连接已断开的连接;此行为适用于 ASP.NET Core SignalR 客户端</span><span class="sxs-lookup"><span data-stu-id="018c6-108">ASP.NET SignalR automatically attempts to reconnect dropped connections; this behavior is opt-in for ASP.NET Core SignalR clients</span></span>
- <span data-ttu-id="018c6-109">这两个框架都支持 JSON;ASP.NET Core SignalR 还支持基于 MessagePack 的二进制协议，并可创建自定义协议。</span><span class="sxs-lookup"><span data-stu-id="018c6-109">Both frameworks support JSON; ASP.NET Core SignalR also supports a binary protocol based on MessagePack, and custom protocols can be created.</span></span>
- <span data-ttu-id="018c6-110">ASP.NET Core SignalR 不支持 ASP.NET SignalR 支持的永久帧传输。</span><span class="sxs-lookup"><span data-stu-id="018c6-110">The Forever Frame transport, supported by ASP.NET SignalR, isn't supported in ASP.NET Core SignalR.</span></span>
- <span data-ttu-id="018c6-111">ASP.NET Core SignalR 必须通过 `services.AddSignalR()` `app.UseEndpoints` 分别在和中添加和进行配置 `Startup.ConfigureServices` `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="018c6-111">ASP.NET Core SignalR must be configured by adding `services.AddSignalR()` and `app.UseEndpoints` in `Startup.ConfigureServices` and `Startup.Configure`, respectively.</span></span>
- <span data-ttu-id="018c6-112">ASP.NET Core SignalR 需要粘滞会话;ASP.NET SignalR。</span><span class="sxs-lookup"><span data-stu-id="018c6-112">ASP.NET Core SignalR requires sticky sessions; ASP.NET SignalR doesn't.</span></span>
- <span data-ttu-id="018c6-113">ASP.NET Core 简化了连接模型;仅连接到单个集线器。</span><span class="sxs-lookup"><span data-stu-id="018c6-113">ASP.NET Core simplifies the connection model; connections are only made to a single hub.</span></span>
- <span data-ttu-id="018c6-114">ASP.NET Core SignalR 支持从集线器向客户端流式传输数据。</span><span class="sxs-lookup"><span data-stu-id="018c6-114">ASP.NET Core SignalR supports streaming data from the hub to the client.</span></span>
- <span data-ttu-id="018c6-115">ASP.NET Core SignalR 不支持在客户端和中心 (间传递状态，但方法调用仍允许在集线器和客户端) 之间传递信息。</span><span class="sxs-lookup"><span data-stu-id="018c6-115">ASP.NET Core SignalR doesn't support passing state between clients and the hub (but method calls still allow passing information between hubs and clients).</span></span>
- <span data-ttu-id="018c6-116">`PersistentConnection`类在 ASP.NET Core SignalR 中不存在。</span><span class="sxs-lookup"><span data-stu-id="018c6-116">The `PersistentConnection` class doesn't exist in ASP.NET Core SignalR.</span></span>
- <span data-ttu-id="018c6-117">ASP.NET SignalR 支持 SQL Server 和 Redis。</span><span class="sxs-lookup"><span data-stu-id="018c6-117">ASP.NET SignalR supports SQL Server and Redis.</span></span> <span data-ttu-id="018c6-118">ASP.NET Core SignalR 支持 [Azure SignalR](/azure/azure-signalr/) 和 Redis。</span><span class="sxs-lookup"><span data-stu-id="018c6-118">ASP.NET Core SignalR supports [Azure SignalR](/azure/azure-signalr/) and Redis.</span></span>

## <a name="references"></a><span data-ttu-id="018c6-119">参考</span><span class="sxs-lookup"><span data-stu-id="018c6-119">References</span></span>

- [<span data-ttu-id="018c6-120">ASP.NET SignalR 和 ASP.NET Core SignalR 之间的差异</span><span class="sxs-lookup"><span data-stu-id="018c6-120">Differences between ASP.NET SignalR and ASP.NET Core SignalR</span></span>](/aspnet/core/signalr/version-differences)
- [<span data-ttu-id="018c6-121">Azure SignalR 服务</span><span class="sxs-lookup"><span data-stu-id="018c6-121">Azure SignalR Service</span></span>](/azure/azure-signalr/)

>[!div class="step-by-step"]
><span data-ttu-id="018c6-122">[上一页](razor-differences.md)
>[下一页](testing-differences.md)</span><span class="sxs-lookup"><span data-stu-id="018c6-122">[Previous](razor-differences.md)
[Next](testing-differences.md)</span></span>
