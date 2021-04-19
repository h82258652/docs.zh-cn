---
title: .NET 中的已知 EventCounters
description: 查看 .NET 运行时和库发布的 EventCounters。
ms.topic: reference
ms.date: 12/17/2020
ms.openlocfilehash: fc07ace22f1580d547c33b897bcd1bb9a1de15c3
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106496717"
---
# <a name="well-known-eventcounters-in-net"></a>.NET 中的已知 EventCounters

.NET 运行时和库实现并发布几个 [`EventCounter`](./event-counters.md)，可用于识别和诊断各种性能问题。 通过本文档，你可了解可用于监视这些 `EventCounters` 的提供程序及其描述。

## <a name="systemruntime-counters"></a>System.Runtime 计数器

以下计数器作为 .NET 运行时 (CoreCLR) 的一部分发布，并在 [`RuntimeEventSource.cs`](https://github.com/dotnet/runtime/blob/main/src/libraries/System.Private.CoreLib/src/System/Diagnostics/Tracing/RuntimeEventSource.cs) 中进行维护。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="% Time in GC since last GC"::: (`time-in-gc`) | 自上次 GC 以来 GC 的时间百分比 |
| :::no-loc text="Allocation Rate"::: (`alloc-rate`) | 每个更新间隔分配的字节数 |
| :::no-loc text="CPU Usage"::: (`cpu-usage`) | 相对于所有系统 CPU 资源进程的 CPU 使用率百分比 |
| :::no-loc text="Exception Count"::: (`exception-count`) | 已发生的异常数 |
| :::no-loc text="GC Heap Size"::: (`gc-heap-size`) | 根据 <xref:System.GC.GetTotalMemory(System.Boolean)?displayProperty=nameWithType> 认为要分配的字节数 |
| :::no-loc text="Gen 0 GC Count"::: (`gen-0-gc-count`) | 每个更新间隔发生的第 0 代 GC 次数 |
| :::no-loc text="Gen 0 Size"::: (`gen-0-size`) | 第 0 代 GC 的字节数 |
| :::no-loc text="Gen 1 GC Count"::: (`gen-1-gc-count`) | 每个更新间隔发生的第 1 代 GC 次数 |
| :::no-loc text="Gen 1 Size"::: (`gen-1-size`) | 第 1 代 GC 的字节数 |
| :::no-loc text="Gen 2 GC Count"::: (`gen-2-gc-count`) | 每个更新间隔发生的第 2 代 GC 次数 |
| :::no-loc text="Gen 2 Size"::: (`gen-2-size`) | 第 2 代 GC 的字节数 |
| :::no-loc text="LOH Size"::: (`loh-size`) | 大型对象堆的字节数 |
| :::no-loc text="POH Size"::: (`poh-size`) | 已固定对象堆的字节数（在 .NET 5 及更高版本中可用） |
| :::no-loc text="GC Fragmentation"::: (`gc-fragmentation`) | GC 堆碎片（在 .NET 5 及更高版本中可用） |
| :::no-loc text="Monitor Lock Contention Count"::: (`monitor-lock-contention-count`) | 尝试锁定监视器时出现争用的次数，基于 <xref:System.Threading.Monitor.LockContentionCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Active Timers"::: (`active-timer-count`) | 当前活动的 <xref:System.Threading.Timer> 实例的计数，基于 <xref:System.Threading.Timer.ActiveCount?displayProperty=nameWithType> |
| :::no-loc text="Number of Assemblies Loaded"::: (`assembly-count`) | 在某个时间点加载到进程中的 <xref:System.Reflection.Assembly> 实例的计数 |
| :::no-loc text="ThreadPool Completed Work Item Count"::: (`threadpool-completed-items-count`) | 迄今为止 <xref:System.Threading.ThreadPool> 中已处理的工作项数 |
| :::no-loc text="ThreadPool Queue Length"::: (`threadpool-queue-length`) | <xref:System.Threading.ThreadPool> 中当前已加入处理队列的工作项数 |
| :::no-loc text="ThreadPool Thread Count"::: (`threadpool-thread-count`) | <xref:System.Threading.ThreadPool> 中当前存在的线程池线程数，基于 <xref:System.Threading.ThreadPool.ThreadCount?displayProperty=nameWithType> |
| :::no-loc text="Working Set"::: (`working-set`) | 某个时间点映射到进程上下文的物理内存量，基于 <xref:System.Environment.WorkingSet?displayProperty=nameWithType> |
| :::no-loc text="IL Bytes Jitted"::: (`il-bytes-jitted`) | JIT 编译的 IL 的总大小，以字节为单位（在 .NET 5 及更高版本中可用） |
| :::no-loc text="Method Jitted Count"::: (`method-jitted-count`) | JIT 编译的方法数（在 .NET 5 及更高版本中可用） |
| :::no-loc text="GC Committed Bytes"::: (`gc-committed-bytes`) | GC 提交的字节数（在 .NET 6 及更高版本中提供） |

## <a name="microsoftaspnetcorehosting-counters"></a>“Microsoft.AspNetCore.Hosting”计数器

以下计数器作为 [ASP.NET Core](/aspnet/core) 的一部分发布，并在 [`HostingEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/main/src/Hosting/Hosting/src/Internal/HostingEventSource.cs) 中进行维护。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="Current Requests"::: (`current-requests`) | 已启动但尚未停止的请求总数 |
| :::no-loc text="Failed Requests"::: (`failed-requests`) | 应用生命周期内发生的失败请求总数 |
| :::no-loc text="Request Rate"::: (`requests-per-second`) | 每个更新间隔发生的请求数 |
| :::no-loc text="Total Requests"::: (`total-requests`) | 应用生命周期内发生的请求总数 |

## <a name="microsoftaspnetcorehttpconnections-counters"></a>“Microsoft.AspNetCore.Http.Connections”计数器

以下计数器作为 [ASP.NET Core SignalR](/aspnet/core/signalr/introduction) 的一部分发布，并在 [`HttpConnectionsEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/main/src/SignalR/common/Http.Connections/src/Internal/HttpConnectionsEventSource.cs) 中进行维护。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="Average Connection Duration"::: (`connections-duration`) | 连接的平均持续时间（毫秒） |
| :::no-loc text="Current Connections"::: (`current-connections`) | 已启动但尚未停止的活动连接数 |
| :::no-loc text="Total Connections Started"::: (`connections-started`) | 已启动的连接总数 |
| :::no-loc text="Total Connections Stopped"::: (`connections-stopped`) | 已停止的连接总数 |
| :::no-loc text="Total Connections Timed Out"::: (`connections-timed-out`) | 已超时的连接总数 |

## <a name="microsoft-aspnetcore-server-kestrel-counters"></a>“Microsoft-AspNetCore-Server-Kestrel”计数器

以下计数器作为 [ASP.NET Core Kestrel Web 服务器](/aspnet/core/fundamentals/servers/kestrel)的一部分发布，并在 [`KestrelEventSource.cs`](https://github.com/dotnet/aspnetcore/blob/main/src/Servers/Kestrel/Core/src/Internal/Infrastructure/KestrelEventSource.cs) 中进行维护。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="Connection Queue Length"::: (`connection-queue-length`) | 连接队列的当前长度 |
| :::no-loc text="Connection Rate"::: (`connections-per-second`) | 每个更新间隔与 Web 服务器的连接数 |
| :::no-loc text="Current Connections"::: (`current-connections`) | 到 Web 服务器的当前活动连接数 |
| :::no-loc text="Current TLS Handshakes"::: (`current-tls-handshakes`) | 当前 TLS 握手数 |
| :::no-loc text="Current Upgraded Requests (WebSockets)"::: (`current-upgraded-requests`) | 当前升级请求数 (WebSockets) |
| :::no-loc text="Failed TLS Handshakes"::: (`failed-tls-handshakes`) | 失败的 TLS 握手总数 |
| :::no-loc text="Request Queue Length"::: (`request-queue-length`) | 请求队列的当前长度 |
| :::no-loc text="TLS Handshake Rate"::: (`tls-handshakes-per-second`) | 每个更新间隔的 TLS 握手数 |
| :::no-loc text="Total Connections"::: (`total-connections`) | 到 Web 服务器的连接总数 |
| :::no-loc text="Total TLS Handshakes"::: (`total-tls-handshakes`) | Web 服务器的 TLS 握手总数 |

## <a name="systemnethttp-counters"></a>“System.Net.Http”计数器

以下计数器由 HTTP 堆栈发布。  这些计数器仅在 .NET 5 及更高版本上可用。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="Requests Started"::: (`requests-started`) | 自进程启动以来启动的请求数 |
| :::no-loc text="Requests Started Rate"::: (`requests-started-rate`) | 每个更新间隔启动的请求数 |
| :::no-loc text="Requests Failed"::: (`requests-failed`) | 自进程启动以来失败的请求数 |
| :::no-loc text="Requests Failed Rate"::: (`requests-failed-rate`) | 每个更新间隔的失败请求数 |
| :::no-loc text="Current Requests"::: (`current-requests`) | 当前已启动但尚未完成或失败的活动 HTTP 请求数 |
| :::no-loc text="Current HTTP 1.1 Connections"::: (`http11-connections-current-total`) | 当前已启动但尚未完成或失败的 HTTP 1.1 连接数 |
| :::no-loc text="Current HTTP 2.0 Connections"::: (`http20-connections-current-total`) | 当前已启动但尚未完成或失败的 HTTP 2.0 连接数 |
| :::no-loc text="HTTP 1.1 Requests Queue Duration"::: (`http11-requests-queue-duration`) | HTTP 1.1 请求在请求队列中花费的平均持续时间 |
| :::no-loc text="HTTP 2.0 Requests Queue Duration"::: (`http20-requests-queue-duration`) | HTTP 2.0 请求在请求队列中花费的平均持续时间 |

## <a name="systemnetnameresolution-counters"></a>“System.Net.NameResolution”计数器

以下计数器跟踪与 DNS 查找相关的指标。 这些计数器仅在 .NET 5 及更高版本上可用。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="DNS Lookups Requested"::: (`dns-lookups-requested`) | 自进程启动以来请求的 DNS 查找数 |
| :::no-loc text="Average DNS Lookup Duration"::: (`dns-lookups-duration`) | DNS 查找所用的平均时间 |

## <a name="systemnetsecurity-counters"></a>“System.Net.Security”计数器

以下计数器跟踪与传输层安全协议相关的指标。  这些计数器仅在 .NET 5 及更高版本上可用。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="TLS handshakes completed"::: (`tls-handshake-rate`) | 每个更新间隔完成的 TLS 握手数 |
| :::no-loc text="Total TLS handshakes completed"::: (`total-tls-handshakes`) | 自进程启动以来完成的 TLS 握手总数 |
| :::no-loc text="Current TLS handshakes"::: (`current-tls-handshakes`) | 当前已启动但尚未完成的 TLS 握手数 |
| :::no-loc text="Total TLS handshakes failed"::: (`failed-tls-handshakes`) | 自进程启动以来失败的 TLS 握手总数 |
| :::no-loc text="All TLS Sessions Active"::: (`all-tls-sessions-open`) | 任何版本的活动 TLS 会话数 |
| :::no-loc text="TLS 1.0 Sessions Active"::: (`tls10-sessions-open`) | 活动的 TLS 1.0 会话数 |
| :::no-loc text="TLS 1.1 Sessions Active"::: (`tls11-sessions-open`) | 活动的 TLS 1.1 会话数 |
| :::no-loc text="TLS 1.2 Sessions Active"::: (`tls12-sessions-open`) | 活动的 TLS 1.2 会话数 |
| :::no-loc text="TLS 1.3 Sessions Active"::: (`tls13-sessions-open`) | 活动的 TLS 1.3 会话数 |
| :::no-loc text="TLS Handshake Duration"::: (`all-tls-handshake-duration`) | 所有 TLS 握手的平均持续时间 |
| :::no-loc text="TLS 1.0 Handshake Duration"::: (`tls10-handshake-duration`) | TLS 1.0 握手的平均持续时间 |
| :::no-loc text="TLS 1.1 Handshake Duration"::: (`tls11-handshake-duration`) | TLS 1.1 握手的平均持续时间 |
| :::no-loc text="TLS 1.2 Handshake Duration"::: (`tls12-handshake-duration`) | TLS 1.2 握手的平均持续时间 |
| :::no-loc text="TLS 1.3 Handshake Duration"::: (`tls13-handshake-duration`) | TLS 1.3 握手的平均持续时间 |

## <a name="systemnetsockets-counters-available-on-net-5-and-later-versions"></a>“System.Net.Sockets”计数器（在 .NET 5 及更高版本上可用）

以下计数器跟踪与 <xref:System.Net.Sockets.Socket> 相关的指标。

| 计数器 | 说明 |
|--|--|
| :::no-loc text="Outgoing Connections Established"::: (`outgoing-connections-established`) | 自进程启动以来建立的传出连接总数 |
| :::no-loc text="Incoming Connections Established"::: (`incoming-connections-established`) | 自进程启动以来建立的传入连接总数 |
| :::no-loc text="Bytes Received"::: (`bytes-received`) | 自进程启动以来收到的字节总数 |
| :::no-loc text="Bytes Sent"::: (`bytes-sent`) | 自进程启动以来发送的字节总数 |
| :::no-loc text="Datagrams Received"::: (`datagrams-received`) | 自流程启动以来收到的数据报总数 |
| :::no-loc text="Datagrams Sent"::: (`datagrams-sent`) | 自流程启动以来发送的数据报总数 |
