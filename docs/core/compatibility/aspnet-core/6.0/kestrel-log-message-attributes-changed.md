---
title: 中断性变更：Kestrel：日志消息属性已更改
description: 了解 ASP.NET Core 6.0 中的以下中断性变更：Kestrel：日志消息属性已更改
ms.author: scaddie
ms.date: 02/01/2021
ms.openlocfilehash: daeb9ae418f343a00e9563ef3e2b5090a7f016a9
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255163"
---
# <a name="kestrel-log-message-attributes-changed"></a><span data-ttu-id="c1df1-103">Kestrel：日志消息属性已更改</span><span class="sxs-lookup"><span data-stu-id="c1df1-103">Kestrel: Log message attributes changed</span></span>

<span data-ttu-id="c1df1-104">Kestrel 日志消息具有关联的 ID 和名称。</span><span class="sxs-lookup"><span data-stu-id="c1df1-104">Kestrel log messages have associated IDs and names.</span></span> <span data-ttu-id="c1df1-105">这些属性唯一地标识了不同类型的日志消息。</span><span class="sxs-lookup"><span data-stu-id="c1df1-105">These attributes uniquely identify different kinds of log messages.</span></span> <span data-ttu-id="c1df1-106">其中一些 ID 和名称是错误重复的。</span><span class="sxs-lookup"><span data-stu-id="c1df1-106">Some of those IDs and names were incorrectly duplicated.</span></span> <span data-ttu-id="c1df1-107">这个重复问题在 ASP.NET Core 6.0 中得到了修复。</span><span class="sxs-lookup"><span data-stu-id="c1df1-107">This duplication problem is fixed in ASP.NET Core 6.0.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="c1df1-108">引入的版本</span><span class="sxs-lookup"><span data-stu-id="c1df1-108">Version introduced</span></span>

<span data-ttu-id="c1df1-109">ASP.NET Core 6.0</span><span class="sxs-lookup"><span data-stu-id="c1df1-109">ASP.NET Core 6.0</span></span>

## <a name="old-behavior"></a><span data-ttu-id="c1df1-110">旧行为</span><span class="sxs-lookup"><span data-stu-id="c1df1-110">Old behavior</span></span>

<span data-ttu-id="c1df1-111">下表显示 ASP.NET Core 6.0 之前受影响的日志消息的状态。</span><span class="sxs-lookup"><span data-stu-id="c1df1-111">The following table shows the state of the affected log messages before ASP.NET Core 6.0.</span></span>

| <span data-ttu-id="c1df1-112">消息描述</span><span class="sxs-lookup"><span data-stu-id="c1df1-112">Message description</span></span>                   | <span data-ttu-id="c1df1-113">名称</span><span class="sxs-lookup"><span data-stu-id="c1df1-113">Name</span></span>                    | <span data-ttu-id="c1df1-114">ID</span><span class="sxs-lookup"><span data-stu-id="c1df1-114">ID</span></span> |
|---------------------------------------|-------------------------|----|
| <span data-ttu-id="c1df1-115">HTTP/2 连接关闭日志消息</span><span class="sxs-lookup"><span data-stu-id="c1df1-115">HTTP/2 connection closed log messages</span></span> | `Http2ConnectionClosed` | <span data-ttu-id="c1df1-116">36</span><span class="sxs-lookup"><span data-stu-id="c1df1-116">36</span></span> |
| <span data-ttu-id="c1df1-117">HTTP/2 帧发送日志消息</span><span class="sxs-lookup"><span data-stu-id="c1df1-117">HTTP/2 frame sending log messages</span></span>     | `Http2FrameReceived`    | <span data-ttu-id="c1df1-118">37</span><span class="sxs-lookup"><span data-stu-id="c1df1-118">37</span></span> |

## <a name="new-behavior"></a><span data-ttu-id="c1df1-119">新行为</span><span class="sxs-lookup"><span data-stu-id="c1df1-119">New behavior</span></span>

<span data-ttu-id="c1df1-120">下表显示 ASP.NET Core 6.0 中受影响的日志消息的状态。</span><span class="sxs-lookup"><span data-stu-id="c1df1-120">The following table shows the state of the affected log messages in ASP.NET Core 6.0.</span></span>

| <span data-ttu-id="c1df1-121">消息描述</span><span class="sxs-lookup"><span data-stu-id="c1df1-121">Message description</span></span>                   | <span data-ttu-id="c1df1-122">名称</span><span class="sxs-lookup"><span data-stu-id="c1df1-122">Name</span></span>                    | <span data-ttu-id="c1df1-123">ID</span><span class="sxs-lookup"><span data-stu-id="c1df1-123">ID</span></span> |
|---------------------------------------|-------------------------|----|
| <span data-ttu-id="c1df1-124">HTTP/2 连接关闭日志消息</span><span class="sxs-lookup"><span data-stu-id="c1df1-124">HTTP/2 connection closed log messages</span></span> | `Http2ConnectionClosed` | <span data-ttu-id="c1df1-125">48</span><span class="sxs-lookup"><span data-stu-id="c1df1-125">48</span></span> |
| <span data-ttu-id="c1df1-126">HTTP/2 帧发送日志消息</span><span class="sxs-lookup"><span data-stu-id="c1df1-126">HTTP/2 frame sending log messages</span></span>     | `Http2FrameSending`     | <span data-ttu-id="c1df1-127">49</span><span class="sxs-lookup"><span data-stu-id="c1df1-127">49</span></span> |

## <a name="reason-for-change"></a><span data-ttu-id="c1df1-128">更改原因</span><span class="sxs-lookup"><span data-stu-id="c1df1-128">Reason for change</span></span>

<span data-ttu-id="c1df1-129">日志 ID 和名称应是唯一的，这样才能识别不同的消息类型。</span><span class="sxs-lookup"><span data-stu-id="c1df1-129">Log IDs and names should be unique so different message types can be identified.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="c1df1-130">建议的操作</span><span class="sxs-lookup"><span data-stu-id="c1df1-130">Recommended action</span></span>

<span data-ttu-id="c1df1-131">如果你的代码或配置引用了旧的 ID 和名称，请更新这些引用以使用新的 ID 和名称。</span><span class="sxs-lookup"><span data-stu-id="c1df1-131">If you have code or configuration that references the old IDs and names, update those references to use the new IDs and names.</span></span>

<!--

## Category

ASP.NET Core

## Affected APIs

Not detectable via API analysis

-->
