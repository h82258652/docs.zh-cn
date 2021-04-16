---
title: 类型系统运行时事件
description: 请参阅收集特定于 .NET 类型系统的诊断信息的 .NET 运行时事件，如 TypeLoadStart 和 TypeLoadStop。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- type system events (CoreCLR)
- ETW, EventPipe, LTTng type system events (CoreCLR)
ms.openlocfilehash: 8eee89cddb0098da2cb449a4be21945adac725e3
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591055"
---
# <a name="net-runtime-type-events"></a><span data-ttu-id="3c1e0-103">.NET 运行时类型事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-103">.NET runtime type events</span></span>

<span data-ttu-id="3c1e0-104">这些事件收集与加载类型相关的信息。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-104">These events collect information relating to loading types.</span></span> <span data-ttu-id="3c1e0-105">有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="3c1e0-105">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

## <a name="typeloadstart-event"></a><span data-ttu-id="3c1e0-106">TypeLoadStart 事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-106">TypeLoadStart Event</span></span>

|<span data-ttu-id="3c1e0-107">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="3c1e0-107">Keyword for raising the event</span></span>|<span data-ttu-id="3c1e0-108">事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-108">Event</span></span>|<span data-ttu-id="3c1e0-109">Level</span><span class="sxs-lookup"><span data-stu-id="3c1e0-109">Level</span></span>|  
|-----------------------------------|-----------|-----------|  
|<span data-ttu-id="3c1e0-110">`TypeDiagnosticKeyword` (0x8000000000)</span><span class="sxs-lookup"><span data-stu-id="3c1e0-110">`TypeDiagnosticKeyword` (0x8000000000)</span></span>|<span data-ttu-id="3c1e0-111">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="3c1e0-111">Informational (4)</span></span>|  

|<span data-ttu-id="3c1e0-112">事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-112">Event</span></span>|<span data-ttu-id="3c1e0-113">事件 ID</span><span class="sxs-lookup"><span data-stu-id="3c1e0-113">Event ID</span></span>|<span data-ttu-id="3c1e0-114">说明</span><span class="sxs-lookup"><span data-stu-id="3c1e0-114">Description</span></span>|  
|-----------|--------------|-----------------|  
|`TypeLoadStart`|<span data-ttu-id="3c1e0-115">73</span><span class="sxs-lookup"><span data-stu-id="3c1e0-115">73</span></span>|<span data-ttu-id="3c1e0-116">类型加载已开始。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-116">A type load has started.</span></span>|

|<span data-ttu-id="3c1e0-117">字段名</span><span class="sxs-lookup"><span data-stu-id="3c1e0-117">Field name</span></span>|<span data-ttu-id="3c1e0-118">数据类型</span><span class="sxs-lookup"><span data-stu-id="3c1e0-118">Data type</span></span>|<span data-ttu-id="3c1e0-119">说明</span><span class="sxs-lookup"><span data-stu-id="3c1e0-119">Description</span></span>|  
|----------------|---------------|-----------------|  
|`TypeLoadStartID`|`win:UInt32`|<span data-ttu-id="3c1e0-120">类型加载操作的 ID。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-120">ID for the type load operation.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="3c1e0-121">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-121">Unique ID for the instance of CLR or CoreCLR.</span></span>|  

## <a name="typeloadstop-event"></a><span data-ttu-id="3c1e0-122">TypeLoadStop 事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-122">TypeLoadStop Event</span></span>

|<span data-ttu-id="3c1e0-123">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="3c1e0-123">Keyword for raising the event</span></span>|<span data-ttu-id="3c1e0-124">Level</span><span class="sxs-lookup"><span data-stu-id="3c1e0-124">Level</span></span>|  
|-----------------------------------|-----------|-----------|  
|<span data-ttu-id="3c1e0-125">`TypeDiagnosticKeyword` (0x8000000000)</span><span class="sxs-lookup"><span data-stu-id="3c1e0-125">`TypeDiagnosticKeyword` (0x8000000000)</span></span>|<span data-ttu-id="3c1e0-126">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="3c1e0-126">Informational (4)</span></span>|  

|<span data-ttu-id="3c1e0-127">事件</span><span class="sxs-lookup"><span data-stu-id="3c1e0-127">Event</span></span>|<span data-ttu-id="3c1e0-128">事件 ID</span><span class="sxs-lookup"><span data-stu-id="3c1e0-128">Event ID</span></span>|<span data-ttu-id="3c1e0-129">说明</span><span class="sxs-lookup"><span data-stu-id="3c1e0-129">Description</span></span>|  
|-----------|--------------|-----------------|  
|`TypeLoadStop`|<span data-ttu-id="3c1e0-130">74</span><span class="sxs-lookup"><span data-stu-id="3c1e0-130">74</span></span>|<span data-ttu-id="3c1e0-131">类型加载已完成。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-131">A type load is finished.</span></span>|

|<span data-ttu-id="3c1e0-132">字段名</span><span class="sxs-lookup"><span data-stu-id="3c1e0-132">Field name</span></span>|<span data-ttu-id="3c1e0-133">数据类型</span><span class="sxs-lookup"><span data-stu-id="3c1e0-133">Data type</span></span>|<span data-ttu-id="3c1e0-134">说明</span><span class="sxs-lookup"><span data-stu-id="3c1e0-134">Description</span></span>|  
|----------------|---------------|-----------------|  
|`TypeLoadStartID`|`win:UInt32`|<span data-ttu-id="3c1e0-135">类型加载操作的 ID（与对应的 TypeLoadStart 事件的 TypeLoadStartID 相匹配）。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-135">ID for the type load operation (matches the corresponding TypeLoadStart event's TypeLoadStartID).</span></span>|
|`LoadLevel`|`win:UInt16`|<span data-ttu-id="3c1e0-136">类型加载级别。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-136">Type load level.</span></span>|
|`TypeID`|`win:UInt64`|<span data-ttu-id="3c1e0-137">指向该类型句柄的指针。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-137">Pointer to the type handle.</span></span>|
|`TypeName`|`win:UnicodeString`|<span data-ttu-id="3c1e0-138">类型的名称。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-138">Name of the type.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="3c1e0-139">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="3c1e0-139">Unique ID for the instance of CLR or CoreCLR.</span></span>|  
