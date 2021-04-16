---
title: 监视器锁争用运行时事件
description: 查看收集特定于监视器锁争用的信息的 ETW 事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- monitor lock contention events (CoreCLR)
- ETW, EventPipe, LTTng contention events (CoreCLR)
ms.openlocfilehash: 38e5f493d4b223b4839dbd6f814cf656722189b2
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591060"
---
# <a name="net-runtime-contention-events"></a><span data-ttu-id="f98f0-103">.NET 运行时争用事件</span><span class="sxs-lookup"><span data-stu-id="f98f0-103">.NET runtime contention events</span></span>

<span data-ttu-id="f98f0-104">这些运行时事件使用 `Monitor.Enter` 或 C# 锁关键字等捕获有关监视器锁争用的信息。</span><span class="sxs-lookup"><span data-stu-id="f98f0-104">These runtime events capture information about monitor lock contentions such as with `Monitor.Enter` or the C# lock keyword.</span></span> <span data-ttu-id="f98f0-105">有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="f98f0-105">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

## <a name="contentionstart_v1-event"></a><span data-ttu-id="f98f0-106">ContentionStart_V1 事件</span><span class="sxs-lookup"><span data-stu-id="f98f0-106">ContentionStart_V1 event</span></span>

<span data-ttu-id="f98f0-107">在监视器锁争用开始时发出此事件。</span><span class="sxs-lookup"><span data-stu-id="f98f0-107">This event is emitted at the start of a monitor lock contention.</span></span>

|<span data-ttu-id="f98f0-108">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="f98f0-108">Keyword for raising the event</span></span>|<span data-ttu-id="f98f0-109">Level</span><span class="sxs-lookup"><span data-stu-id="f98f0-109">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="f98f0-110">`ContentionKeyword` (0x4000)</span><span class="sxs-lookup"><span data-stu-id="f98f0-110">`ContentionKeyword` (0x4000)</span></span>|<span data-ttu-id="f98f0-111">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="f98f0-111">Informational (4)</span></span>|

 <span data-ttu-id="f98f0-112">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="f98f0-112">The following table shows event information.</span></span>

|<span data-ttu-id="f98f0-113">事件</span><span class="sxs-lookup"><span data-stu-id="f98f0-113">Event</span></span>|<span data-ttu-id="f98f0-114">事件 ID</span><span class="sxs-lookup"><span data-stu-id="f98f0-114">Event ID</span></span>|<span data-ttu-id="f98f0-115">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="f98f0-115">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ContentionStart_V1`|<span data-ttu-id="f98f0-116">81</span><span class="sxs-lookup"><span data-stu-id="f98f0-116">81</span></span>|<span data-ttu-id="f98f0-117">监视器锁争用开始。</span><span class="sxs-lookup"><span data-stu-id="f98f0-117">A monitor lock contention starts.</span></span>|

|<span data-ttu-id="f98f0-118">字段名</span><span class="sxs-lookup"><span data-stu-id="f98f0-118">Field name</span></span>|<span data-ttu-id="f98f0-119">数据类型</span><span class="sxs-lookup"><span data-stu-id="f98f0-119">Data type</span></span>|<span data-ttu-id="f98f0-120">说明</span><span class="sxs-lookup"><span data-stu-id="f98f0-120">Description</span></span>|
|----------------|---------------|-----------------|
|`Flags`|`win:UInt8`|<span data-ttu-id="f98f0-121">`0` 表示托管；`1` 表示本机。</span><span class="sxs-lookup"><span data-stu-id="f98f0-121">`0` for managed; `1` for native.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="f98f0-122">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="f98f0-122">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="contentionstop_v1-event"></a><span data-ttu-id="f98f0-123">ContentionStop_V1 事件</span><span class="sxs-lookup"><span data-stu-id="f98f0-123">ContentionStop_V1 event</span></span>

<span data-ttu-id="f98f0-124">在监视器锁争用结束时发出此事件。</span><span class="sxs-lookup"><span data-stu-id="f98f0-124">This event is emitted at the end of a monitor lock contention.</span></span>

|<span data-ttu-id="f98f0-125">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="f98f0-125">Keyword for raising the event</span></span>|<span data-ttu-id="f98f0-126">Level</span><span class="sxs-lookup"><span data-stu-id="f98f0-126">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="f98f0-127">`ContentionKeyword` (0x4000)</span><span class="sxs-lookup"><span data-stu-id="f98f0-127">`ContentionKeyword` (0x4000)</span></span>|<span data-ttu-id="f98f0-128">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="f98f0-128">Informational (4)</span></span>|

 <span data-ttu-id="f98f0-129">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="f98f0-129">The following table shows event information.</span></span>

|<span data-ttu-id="f98f0-130">事件</span><span class="sxs-lookup"><span data-stu-id="f98f0-130">Event</span></span>|<span data-ttu-id="f98f0-131">事件 ID</span><span class="sxs-lookup"><span data-stu-id="f98f0-131">Event ID</span></span>|<span data-ttu-id="f98f0-132">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="f98f0-132">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ContentionStop_V1`|<span data-ttu-id="f98f0-133">91</span><span class="sxs-lookup"><span data-stu-id="f98f0-133">91</span></span>|<span data-ttu-id="f98f0-134">监视器锁争用结束。</span><span class="sxs-lookup"><span data-stu-id="f98f0-134">A monitor lock contention ends.</span></span>|

|<span data-ttu-id="f98f0-135">字段名</span><span class="sxs-lookup"><span data-stu-id="f98f0-135">Field name</span></span>|<span data-ttu-id="f98f0-136">数据类型</span><span class="sxs-lookup"><span data-stu-id="f98f0-136">Data type</span></span>|<span data-ttu-id="f98f0-137">说明</span><span class="sxs-lookup"><span data-stu-id="f98f0-137">Description</span></span>|
|----------------|---------------|-----------------|
|`Flags`|`win:UInt8`|<span data-ttu-id="f98f0-138">`0` 表示托管；`1` 表示本机。</span><span class="sxs-lookup"><span data-stu-id="f98f0-138">`0` for managed; `1` for native.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="f98f0-139">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="f98f0-139">Unique ID for the instance of CoreCLR.</span></span>|
|`DurationNs`|`win:Double`|<span data-ttu-id="f98f0-140">争用的持续时间（以纳秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="f98f0-140">Duration of the contention in nanoseconds.</span></span>|
