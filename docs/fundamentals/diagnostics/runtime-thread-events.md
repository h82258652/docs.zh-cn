---
title: ThreadPool 运行时事件
description: 查看在 .NET Core 中收集有关线程池的诊断信息的 .NET 运行时线程池事件。 线程池事件是工作线程池事件或 I/O 线程池事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- ThreadPool events (CoreCLR)
- ETW, thread pool events (CoreCLR)
ms.openlocfilehash: cdd6041c5842d4922c60e33daf6db366f7d35327
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591083"
---
# <a name="net-runtime-thread-pool-events"></a><span data-ttu-id="40baf-104">.NET 运行时线程池事件</span><span class="sxs-lookup"><span data-stu-id="40baf-104">.NET runtime thread pool events</span></span>

<span data-ttu-id="40baf-105">这些事件收集线程池中有关工作线程和 I/O 线程的信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-105">These events collect information about worker and I/O threads in the threadpool.</span></span> <span data-ttu-id="40baf-106">有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="40baf-106">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

## <a name="iothreadcreate_v1-event"></a><span data-ttu-id="40baf-107">IOThreadCreate_V1 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-107">IOThreadCreate_V1 event</span></span>

 <span data-ttu-id="40baf-108">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-108">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-109">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-109">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-110">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-110">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-111">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-111">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-112">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-112">Informational (4)</span></span>|

 <span data-ttu-id="40baf-113">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-113">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-114">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-114">Event</span></span>|<span data-ttu-id="40baf-115">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-115">Event ID</span></span>|<span data-ttu-id="40baf-116">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="40baf-116">Raised when</span></span>|
|-----------------------------------|-----------|
|`IOThreadCreate_V1`|<span data-ttu-id="40baf-117">44</span><span class="sxs-lookup"><span data-stu-id="40baf-117">44</span></span>|<span data-ttu-id="40baf-118">在线程池中创建 I/O 线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-118">An I/O thread is created in the thread pool.</span></span>|

 <span data-ttu-id="40baf-119">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-119">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-120">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-120">Field name</span></span>|<span data-ttu-id="40baf-121">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-121">Data type</span></span>|<span data-ttu-id="40baf-122">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-122">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|<span data-ttu-id="40baf-123">I/O 线程数，包括新创建的线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-123">Number of I/O threads, including the newly created thread.</span></span>|
|`NumRetired`|`win:UInt64`|<span data-ttu-id="40baf-124">已停用的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-124">Number of retired worker threads.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-125">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-125">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="iothreadterminate_v1-event"></a><span data-ttu-id="40baf-126">IOThreadTerminate_V1 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-126">IOThreadTerminate_V1 event</span></span>

 <span data-ttu-id="40baf-127">下表显示了关键字和级别</span><span class="sxs-lookup"><span data-stu-id="40baf-127">The following table shows the keyword and level</span></span>

|<span data-ttu-id="40baf-128">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-128">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-129">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-129">Level</span></span>
|-----------------------------------|-----------
|<span data-ttu-id="40baf-130">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-130">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-131">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-131">Informational (4)</span></span>

 <span data-ttu-id="40baf-132">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-132">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-133">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-133">Event</span></span>|<span data-ttu-id="40baf-134">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-134">Event ID</span></span>|<span data-ttu-id="40baf-135">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="40baf-135">Raised when</span></span>|
|-----------|--------------|-----------------|
|`IOThreadTerminate`|<span data-ttu-id="40baf-136">45</span><span class="sxs-lookup"><span data-stu-id="40baf-136">45</span></span>|<span data-ttu-id="40baf-137">在线程池中终止 I/O 线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-137">An I/O thread is terminated in the thread pool.</span></span>|

 <span data-ttu-id="40baf-138">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-138">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-139">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-139">Field name</span></span>|<span data-ttu-id="40baf-140">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-140">Data type</span></span>|<span data-ttu-id="40baf-141">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-141">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|<span data-ttu-id="40baf-142">线程池中剩余的 I/O 线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-142">Number of I/O threads remaining in the thread pool.</span></span>|
|`NumRetired`|`win:UInt64`|<span data-ttu-id="40baf-143">已停用的 I/O 线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-143">Number of retired I/O threads.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-144">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-144">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="iothreadretire_v1-event"></a><span data-ttu-id="40baf-145">IOThreadRetire_V1 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-145">IOThreadRetire_V1 event</span></span>

 <span data-ttu-id="40baf-146">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-146">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-147">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-147">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-148">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-148">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-149">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-149">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-150">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-150">Informational (4)</span></span>|

 <span data-ttu-id="40baf-151">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-151">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-152">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-152">Event</span></span>|<span data-ttu-id="40baf-153">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-153">Event ID</span></span>|<span data-ttu-id="40baf-154">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="40baf-154">Raised when</span></span>|
|-----------|--------------|-----------------|
|`IOThreadRetire_V1`|<span data-ttu-id="40baf-155">46</span><span class="sxs-lookup"><span data-stu-id="40baf-155">46</span></span>|<span data-ttu-id="40baf-156">I/O 线程变为停用候选项。</span><span class="sxs-lookup"><span data-stu-id="40baf-156">An I/O thread becomes a retirement candidate.</span></span>|

 <span data-ttu-id="40baf-157">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-157">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-158">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-158">Field name</span></span>|<span data-ttu-id="40baf-159">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-159">Data type</span></span>|<span data-ttu-id="40baf-160">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-160">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|<span data-ttu-id="40baf-161">线程池中剩余的 I/O 线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-161">Number of I/O threads remaining in the thread pool.</span></span>|
|`NumRetired`|`win:UInt64`|<span data-ttu-id="40baf-162">已停用的 I/O 线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-162">Number of retired I/O threads.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-163">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-163">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="iothreadunretire_v1-event"></a><span data-ttu-id="40baf-164">IOThreadUnretire_V1 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-164">IOThreadUnretire_V1 event</span></span>

 <span data-ttu-id="40baf-165">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-165">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-166">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-166">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-167">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-167">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-168">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-168">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-169">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-169">Informational (4)</span></span>|

 <span data-ttu-id="40baf-170">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-170">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-171">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-171">Event</span></span>|<span data-ttu-id="40baf-172">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-172">Event ID</span></span>|<span data-ttu-id="40baf-173">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="40baf-173">Raised when</span></span>|
|-----------|--------------|-----------------|
|`IOThreadUnretire_V1`|<span data-ttu-id="40baf-174">47</span><span class="sxs-lookup"><span data-stu-id="40baf-174">47</span></span>|<span data-ttu-id="40baf-175">I/O 线程因在该线程变为停用候选项后的等待期间内达到 I/O 而恢复使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-175">An I/O thread is unretired because of I/O that arrives within a waiting period after the thread becomes a retirement candidate.</span></span>|

 <span data-ttu-id="40baf-176">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-176">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-177">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-177">Field name</span></span>|<span data-ttu-id="40baf-178">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-178">Data type</span></span>|<span data-ttu-id="40baf-179">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-179">Description</span></span>|
|----------------|---------------|-----------------|
|`Count`|`win:UInt64`|<span data-ttu-id="40baf-180">线程池中的 I/O 线程数，包括这一个。</span><span class="sxs-lookup"><span data-stu-id="40baf-180">Number of I/O threads in the thread pool, including this one.</span></span>|
|`NumRetired`|`win:UInt64`|<span data-ttu-id="40baf-181">已停用的 I/O 线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-181">Number of retired I/O threads.</span></span>|
|`ClrInstanceID`|`Win:UInt16`|<span data-ttu-id="40baf-182">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-182">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadstart-event"></a><span data-ttu-id="40baf-183">ThreadPoolWorkerThreadStart 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-183">ThreadPoolWorkerThreadStart event</span></span>

|<span data-ttu-id="40baf-184">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-184">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-185">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-185">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="40baf-186">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-186">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-187">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-187">Informational (4)</span></span>|

|<span data-ttu-id="40baf-188">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-188">Event</span></span>|<span data-ttu-id="40baf-189">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-189">Event ID</span></span>|<span data-ttu-id="40baf-190">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-190">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadStart`|<span data-ttu-id="40baf-191">50</span><span class="sxs-lookup"><span data-stu-id="40baf-191">50</span></span>|<span data-ttu-id="40baf-192">创建工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-192">A worker thread is created.</span></span>|

|<span data-ttu-id="40baf-193">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-193">Field name</span></span>|<span data-ttu-id="40baf-194">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-194">Data type</span></span>|<span data-ttu-id="40baf-195">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-195">Description</span></span>|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-196">可用于处理工作的工作线程数，包括已在处理工作的工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-196">Number of worker threads available to process work, including those that are already processing work.</span></span>|
|`RetiredWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-197">不能用于处理工作但被保留以防之后需要更多线程的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-197">Number of worker threads that are not available to process work, but that are being held in reserve in case more threads are needed later.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-198">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-198">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadstop-event"></a><span data-ttu-id="40baf-199">ThreadPoolWorkerThreadStop 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-199">ThreadPoolWorkerThreadStop event</span></span>

|<span data-ttu-id="40baf-200">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-200">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-201">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-201">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="40baf-202">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-202">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-203">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-203">Informational (4)</span></span>|

|<span data-ttu-id="40baf-204">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-204">Event</span></span>|<span data-ttu-id="40baf-205">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-205">Event ID</span></span>|<span data-ttu-id="40baf-206">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-206">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadStop`|<span data-ttu-id="40baf-207">51</span><span class="sxs-lookup"><span data-stu-id="40baf-207">51</span></span>|<span data-ttu-id="40baf-208">停止工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-208">A worker thread is stopped.</span></span>|

|<span data-ttu-id="40baf-209">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-209">Field name</span></span>|<span data-ttu-id="40baf-210">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-210">Data type</span></span>|<span data-ttu-id="40baf-211">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-211">Description</span></span>|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-212">可用于处理工作的工作线程数，包括已在处理工作的工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-212">Number of worker threads available to process work, including those that are already processing work.</span></span>|
|`RetiredWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-213">不能用于处理工作但被保留以防之后需要更多线程的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-213">Number of worker threads that are not available to process work, but that are being held in reserve in case more threads are needed later.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-214">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-214">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadwait-event"></a><span data-ttu-id="40baf-215">ThreadPoolWorkerThreadWait 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-215">ThreadPoolWorkerThreadWait event</span></span>

|<span data-ttu-id="40baf-216">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-216">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-217">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-217">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="40baf-218">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-218">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-219">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-219">Informational (4)</span></span>|

|<span data-ttu-id="40baf-220">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-220">Event</span></span>|<span data-ttu-id="40baf-221">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-221">Event ID</span></span>|<span data-ttu-id="40baf-222">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-222">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadWait`|<span data-ttu-id="40baf-223">57</span><span class="sxs-lookup"><span data-stu-id="40baf-223">57</span></span>|<span data-ttu-id="40baf-224">工作线程开始等待运行。</span><span class="sxs-lookup"><span data-stu-id="40baf-224">A worker thread starts waiting for work.</span></span>|

|<span data-ttu-id="40baf-225">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-225">Field name</span></span>|<span data-ttu-id="40baf-226">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-226">Data type</span></span>|<span data-ttu-id="40baf-227">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-227">Description</span></span>|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-228">可用于处理工作的工作线程数，包括已在处理工作的工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-228">Number of worker threads available to process work, including those that are already processing work.</span></span>|
|`RetiredWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-229">不能用于处理工作但被保留以防之后需要更多线程的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-229">Number of worker threads that are not available to process work, but that are being held in reserve in case more threads are needed later.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-230">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-230">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadretirementstart-event"></a><span data-ttu-id="40baf-231">ThreadPoolWorkerThreadRetirementStart 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-231">ThreadPoolWorkerThreadRetirementStart event</span></span>

|<span data-ttu-id="40baf-232">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-232">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-233">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-233">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="40baf-234">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-234">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-235">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-235">Informational (4)</span></span>|

|<span data-ttu-id="40baf-236">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-236">Event</span></span>|<span data-ttu-id="40baf-237">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-237">Event ID</span></span>|<span data-ttu-id="40baf-238">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-238">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadRetirementStart`|<span data-ttu-id="40baf-239">52</span><span class="sxs-lookup"><span data-stu-id="40baf-239">52</span></span>|<span data-ttu-id="40baf-240">停用工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-240">A worker thread retires.</span></span>|

|<span data-ttu-id="40baf-241">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-241">Field name</span></span>|<span data-ttu-id="40baf-242">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-242">Data type</span></span>|<span data-ttu-id="40baf-243">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-243">Description</span></span>|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-244">可用于处理工作的工作线程数，包括已在处理工作的工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-244">Number of worker threads available to process work, including those that are already processing work.</span></span>|
|`RetiredWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-245">不能用于处理工作但被保留以防之后需要更多线程的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-245">Number of worker threads that are not available to process work, but that are being held in reserve in case more threads are needed later.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-246">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-246">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadretirementstop-event"></a><span data-ttu-id="40baf-247">ThreadPoolWorkerThreadRetirementStop 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-247">ThreadPoolWorkerThreadRetirementStop event</span></span>

|<span data-ttu-id="40baf-248">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-248">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-249">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-249">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="40baf-250">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-250">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-251">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-251">Informational (4)</span></span>|

|<span data-ttu-id="40baf-252">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-252">Event</span></span>|<span data-ttu-id="40baf-253">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-253">Event ID</span></span>|<span data-ttu-id="40baf-254">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-254">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadRetirementStop`|<span data-ttu-id="40baf-255">53</span><span class="sxs-lookup"><span data-stu-id="40baf-255">53</span></span>|<span data-ttu-id="40baf-256">停用的工作线程再次变为活动状态。</span><span class="sxs-lookup"><span data-stu-id="40baf-256">A retired worker thread becomes active again.</span></span>|

|<span data-ttu-id="40baf-257">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-257">Field name</span></span>|<span data-ttu-id="40baf-258">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-258">Data type</span></span>|<span data-ttu-id="40baf-259">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-259">Description</span></span>|
|----------------|---------------|-----------------|
|`ActiveWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-260">可用于处理工作的工作线程数，包括已在处理工作的工作线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-260">Number of worker threads available to process work, including those that are already processing work.</span></span>|
|`RetiredWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-261">不能用于处理工作但被保留以防之后需要更多线程的工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-261">Number of worker threads that are not available to process work, but that are being held in reserve in case more threads are needed later.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-262">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-262">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadadjustmentsample-event"></a><span data-ttu-id="40baf-263">ThreadPoolWorkerThreadAdjustmentSample 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-263">ThreadPoolWorkerThreadAdjustmentSample event</span></span>

 <span data-ttu-id="40baf-264">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-264">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-265">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-265">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-266">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-266">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-267">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-267">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-268">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-268">Informational (4)</span></span>|

 <span data-ttu-id="40baf-269">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-269">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-270">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-270">Event</span></span>|<span data-ttu-id="40baf-271">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-271">Event ID</span></span>|<span data-ttu-id="40baf-272">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-272">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentSample`|<span data-ttu-id="40baf-273">54</span><span class="sxs-lookup"><span data-stu-id="40baf-273">54</span></span>|<span data-ttu-id="40baf-274">指一个示例的信息收集，即具有一定并发级别的即时吞吐量测量。</span><span class="sxs-lookup"><span data-stu-id="40baf-274">Refers to the collection of information for one sample; that is, a measurement of throughput with a certain concurrency level, in an instant of time.</span></span>|

 <span data-ttu-id="40baf-275">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-275">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-276">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-276">Field name</span></span>|<span data-ttu-id="40baf-277">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-277">Data type</span></span>|<span data-ttu-id="40baf-278">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-278">Description</span></span>|
|----------------|---------------|-----------------|
|`Throughput`|`win:Double`|<span data-ttu-id="40baf-279">每个时间单位的完成数。</span><span class="sxs-lookup"><span data-stu-id="40baf-279">Number of completions per unit of time.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-280">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-280">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadadjustmentadjustment-event"></a><span data-ttu-id="40baf-281">ThreadPoolWorkerThreadAdjustmentAdjustment 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-281">ThreadPoolWorkerThreadAdjustmentAdjustment event</span></span>

 <span data-ttu-id="40baf-282">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-282">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-283">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-283">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-284">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-284">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-285">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-285">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-286">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-286">Informational (4)</span></span>|

 <span data-ttu-id="40baf-287">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-287">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-288">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-288">Event</span></span>|<span data-ttu-id="40baf-289">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-289">Event ID</span></span>|<span data-ttu-id="40baf-290">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-290">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentAdjustment`|<span data-ttu-id="40baf-291">55</span><span class="sxs-lookup"><span data-stu-id="40baf-291">55</span></span>|<span data-ttu-id="40baf-292">当线程注入（爬山）算法确定并发级别发生更改时，在控件中记录更改。</span><span class="sxs-lookup"><span data-stu-id="40baf-292">Records a change in control, when the thread injection (hill-climbing) algorithm determines that a change in concurrency level is in place.</span></span>|

 <span data-ttu-id="40baf-293">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-293">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-294">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-294">Field name</span></span>|<span data-ttu-id="40baf-295">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-295">Data type</span></span>|<span data-ttu-id="40baf-296">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-296">Description</span></span>|
|----------------|---------------|-----------------|
|`AverageThroughput`|`win:Double`|<span data-ttu-id="40baf-297">测量示例的平均吞吐量。</span><span class="sxs-lookup"><span data-stu-id="40baf-297">Average throughput of a sample of measurements.</span></span>|
|`NewWorkerThreadCount`|`win:UInt32`|<span data-ttu-id="40baf-298">新的活动工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-298">New number of active worker threads.</span></span>|
|`Reason`|`win:UInt32`|<span data-ttu-id="40baf-299">调整的原因。</span><span class="sxs-lookup"><span data-stu-id="40baf-299">Reason for the adjustment.</span></span><br /><br /> <span data-ttu-id="40baf-300">`0x0` - 预热。</span><span class="sxs-lookup"><span data-stu-id="40baf-300">`0x0` - Warmup.</span></span><br /><br /> <span data-ttu-id="40baf-301">`0x1` - 正在初始化。</span><span class="sxs-lookup"><span data-stu-id="40baf-301">`0x1` - Initializing.</span></span><br /><br /> <span data-ttu-id="40baf-302">`0x2` - 随机移动。</span><span class="sxs-lookup"><span data-stu-id="40baf-302">`0x2` - Random move.</span></span><br /><br /> <span data-ttu-id="40baf-303">`0x3` - 攀移。</span><span class="sxs-lookup"><span data-stu-id="40baf-303">`0x3` - Climbing move.</span></span><br /><br /> <span data-ttu-id="40baf-304">`0x4` - 更改点。</span><span class="sxs-lookup"><span data-stu-id="40baf-304">`0x4` - Change point.</span></span><br /><br /> <span data-ttu-id="40baf-305">`0x5` - 正在稳定。</span><span class="sxs-lookup"><span data-stu-id="40baf-305">`0x5` - Stabilizing.</span></span><br /><br /> <span data-ttu-id="40baf-306">`0x6` - 匮乏。</span><span class="sxs-lookup"><span data-stu-id="40baf-306">`0x6` - Starvation.</span></span><br /><br /> <span data-ttu-id="40baf-307">`0x7` - 线程已超时。</span><span class="sxs-lookup"><span data-stu-id="40baf-307">`0x7` - Thread timed out.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-308">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-308">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolworkerthreadadjustmentstats-event"></a><span data-ttu-id="40baf-309">ThreadPoolWorkerThreadAdjustmentStats 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-309">ThreadPoolWorkerThreadAdjustmentStats event</span></span>

 <span data-ttu-id="40baf-310">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-310">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-311">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-311">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-312">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-312">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-313">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-313">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-314">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-314">Verbose (5)</span></span>

 <span data-ttu-id="40baf-315">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-315">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-316">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-316">Event</span></span>|<span data-ttu-id="40baf-317">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-317">Event ID</span></span>|<span data-ttu-id="40baf-318">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-318">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolWorkerThreadAdjustmentStats`|<span data-ttu-id="40baf-319">56</span><span class="sxs-lookup"><span data-stu-id="40baf-319">56</span></span>|<span data-ttu-id="40baf-320">在线程池上收集数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-320">Gathers data on the thread pool.</span></span>|

 <span data-ttu-id="40baf-321">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-321">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-322">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-322">Field name</span></span>|<span data-ttu-id="40baf-323">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-323">Data type</span></span>|<span data-ttu-id="40baf-324">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-324">Description</span></span>|
|----------------|---------------|-----------------|
|`Duration`|`win:Double`|<span data-ttu-id="40baf-325">收集这些统计信息的时间量（以秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="40baf-325">Amount of time, in seconds, during which these statistics were collected.</span></span>|
|`Throughput`|`win:Double`|<span data-ttu-id="40baf-326">在此间隔期间每秒完成的平均数量。</span><span class="sxs-lookup"><span data-stu-id="40baf-326">Average number of completions per second during this interval.</span></span>|
|`ThreadWave`|`win:Double`|<span data-ttu-id="40baf-327">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-327">Reserved for internal use.</span></span>|
|`ThroughputWave`|`win:Double`|<span data-ttu-id="40baf-328">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-328">Reserved for internal use.</span></span>|
|`ThroughputErrorEstimate`|`win:Double`|<span data-ttu-id="40baf-329">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-329">Reserved for internal use.</span></span>|
|`AverageThroughputErrorEstimate`|`win:Double`|<span data-ttu-id="40baf-330">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-330">Reserved for internal use.</span></span>|
|`ThroughputRatio`|`win:Double`|<span data-ttu-id="40baf-331">在此间隔期间因活动工作线程计数变化导致的相对吞吐量变化。</span><span class="sxs-lookup"><span data-stu-id="40baf-331">The relative improvement in throughput caused by variations in active worker thread count during this interval.</span></span>|
|`Confidence`|`win:Double`|<span data-ttu-id="40baf-332">ThroughputRatio 字段的有效性测量。</span><span class="sxs-lookup"><span data-stu-id="40baf-332">A measure of the validity of the ThroughputRatio field.</span></span>|
|`NewcontrolSetting`|`win:Double`|<span data-ttu-id="40baf-333">将作为活动线程计数未来变化基线的活动工作线程数。</span><span class="sxs-lookup"><span data-stu-id="40baf-333">The number of active worker threads that will serve as the baseline for future variations in active thread count.</span></span>|
|`NewThreadWaveMagnitude`|`win:UInt16`|<span data-ttu-id="40baf-334">活动线程计数的未来变化量值。</span><span class="sxs-lookup"><span data-stu-id="40baf-334">The magnitude of future variations in active thread count.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-335">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-335">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="threadpoolenqueue-event"></a><span data-ttu-id="40baf-336">ThreadPoolEnqueue 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-336">ThreadPoolEnqueue event</span></span>

 <span data-ttu-id="40baf-337">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-337">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-338">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-338">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-339">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-339">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-340">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-340">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-341">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-341">Verbose (5)</span></span>

 <span data-ttu-id="40baf-342">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-342">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-343">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-343">Event</span></span>|<span data-ttu-id="40baf-344">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-344">Event ID</span></span>|<span data-ttu-id="40baf-345">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-345">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolEnqueue`|<span data-ttu-id="40baf-346">61</span><span class="sxs-lookup"><span data-stu-id="40baf-346">61</span></span>|<span data-ttu-id="40baf-347">工作项已排入线程池队列。</span><span class="sxs-lookup"><span data-stu-id="40baf-347">A work item was enqueued in the thread pool queue.</span></span>|

 <span data-ttu-id="40baf-348">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-348">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-349">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-349">Field name</span></span>|<span data-ttu-id="40baf-350">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-350">Data type</span></span>|<span data-ttu-id="40baf-351">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-351">Description</span></span>|
|----------------|---------------|-----------------|
|`WorkID`|`win:Pointer`|<span data-ttu-id="40baf-352">指向工作请求的指针。</span><span class="sxs-lookup"><span data-stu-id="40baf-352">Pointer to the work request.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-353">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-353">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadpooldequeue-event"></a><span data-ttu-id="40baf-354">ThreadPoolDequeue 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-354">ThreadPoolDequeue event</span></span>

 <span data-ttu-id="40baf-355">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-355">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-356">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-356">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-357">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-357">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-358">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-358">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-359">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-359">Verbose (5)</span></span>

 <span data-ttu-id="40baf-360">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-360">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-361">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-361">Event</span></span>|<span data-ttu-id="40baf-362">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-362">Event ID</span></span>|<span data-ttu-id="40baf-363">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-363">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolDequeue`|<span data-ttu-id="40baf-364">62</span><span class="sxs-lookup"><span data-stu-id="40baf-364">62</span></span>|<span data-ttu-id="40baf-365">工作项已从线程池队列中取消排队。</span><span class="sxs-lookup"><span data-stu-id="40baf-365">A work item was dequeued from the thread pool queue.</span></span>|

 <span data-ttu-id="40baf-366">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-366">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-367">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-367">Field name</span></span>|<span data-ttu-id="40baf-368">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-368">Data type</span></span>|<span data-ttu-id="40baf-369">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-369">Description</span></span>|
|----------------|---------------|-----------------|
|`WorkID`|`win:Pointer`|<span data-ttu-id="40baf-370">指向工作请求的指针。</span><span class="sxs-lookup"><span data-stu-id="40baf-370">Pointer to the work request.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-371">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-371">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadpoolioenqueue-event"></a><span data-ttu-id="40baf-372">ThreadPoolIOEnqueue 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-372">ThreadPoolIOEnqueue event</span></span>

 <span data-ttu-id="40baf-373">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-373">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-374">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-374">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-375">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-375">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-376">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-376">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-377">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-377">Verbose (5)</span></span>

 <span data-ttu-id="40baf-378">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-378">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-379">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-379">Event</span></span>|<span data-ttu-id="40baf-380">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-380">Event ID</span></span>|<span data-ttu-id="40baf-381">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-381">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolIOEnqueue`|<span data-ttu-id="40baf-382">63</span><span class="sxs-lookup"><span data-stu-id="40baf-382">63</span></span>|<span data-ttu-id="40baf-383">异步 IO 完成后，线程对 IO 完成通知进行排队。</span><span class="sxs-lookup"><span data-stu-id="40baf-383">A thread enqueues an IO completion notification after an async IO completion occurs.</span></span>|

 <span data-ttu-id="40baf-384">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-384">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-385">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-385">Field name</span></span>|<span data-ttu-id="40baf-386">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-386">Data type</span></span>|<span data-ttu-id="40baf-387">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-387">Description</span></span>|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|<span data-ttu-id="40baf-388">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-388">Reserved for internal use.</span></span>|
|`Overlapped`|`win:Pointer`|<span data-ttu-id="40baf-389">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-389">Reserved for internal use.</span></span>|
|`MultiDequeues`|`win:Boolean`|<span data-ttu-id="40baf-390">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-390">Reserved for internal use.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-391">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-391">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadpooliodequeue-event"></a><span data-ttu-id="40baf-392">ThreadPoolIODequeue 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-392">ThreadPoolIODequeue event</span></span>

 <span data-ttu-id="40baf-393">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-393">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-394">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-394">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-395">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-395">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-396">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-396">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-397">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-397">Verbose (5)</span></span>

 <span data-ttu-id="40baf-398">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-398">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-399">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-399">Event</span></span>|<span data-ttu-id="40baf-400">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-400">Event ID</span></span>|<span data-ttu-id="40baf-401">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-401">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolIODequeue`|<span data-ttu-id="40baf-402">64</span><span class="sxs-lookup"><span data-stu-id="40baf-402">64</span></span>|<span data-ttu-id="40baf-403">线程对 IO 完成通知取消排队。</span><span class="sxs-lookup"><span data-stu-id="40baf-403">A thread dequeues the IO completion notification.</span></span>|

 <span data-ttu-id="40baf-404">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-404">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-405">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-405">Field name</span></span>|<span data-ttu-id="40baf-406">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-406">Data type</span></span>|<span data-ttu-id="40baf-407">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-407">Description</span></span>|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|<span data-ttu-id="40baf-408">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-408">Reserved for internal use.</span></span>|
|`Overlapped`|`win:Pointer`|<span data-ttu-id="40baf-409">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-409">Reserved for internal use.</span></span>|
|`MultiDequeues`|`win:Boolean`|<span data-ttu-id="40baf-410">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-410">Reserved for internal use.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-411">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-411">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadpooliopack-event"></a><span data-ttu-id="40baf-412">ThreadPoolIOPack 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-412">ThreadPoolIOPack event</span></span>

 <span data-ttu-id="40baf-413">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-413">The following table shows the keyword and level.</span></span>

|<span data-ttu-id="40baf-414">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-414">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-415">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-415">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-416">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-416">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-417">详细级别 (5)</span><span class="sxs-lookup"><span data-stu-id="40baf-417">Verbose (5)</span></span>|

 <span data-ttu-id="40baf-418">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-418">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-419">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-419">Event</span></span>|<span data-ttu-id="40baf-420">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-420">Event ID</span></span>|<span data-ttu-id="40baf-421">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-421">Description</span></span>|
|-----------|--------------|-----------------|
|`ThreadPoolIOPack`|<span data-ttu-id="40baf-422">65</span><span class="sxs-lookup"><span data-stu-id="40baf-422">65</span></span>|<span data-ttu-id="40baf-423">调用了 ThreadPool 重叠的 IO 包。</span><span class="sxs-lookup"><span data-stu-id="40baf-423">ThreadPool overlapped IO pack is called.</span></span>|

 <span data-ttu-id="40baf-424">下表显示了事件数据</span><span class="sxs-lookup"><span data-stu-id="40baf-424">The following table shows the event data</span></span>

|<span data-ttu-id="40baf-425">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-425">Field name</span></span>|<span data-ttu-id="40baf-426">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-426">Data type</span></span>|<span data-ttu-id="40baf-427">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-427">Description</span></span>|
|----------------|---------------|-----------------|
|`NativeOverlapped`|`win:Pointer`|<span data-ttu-id="40baf-428">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-428">Reserved for internal use.</span></span>|
|`Overlapped`|`win:Pointer`|<span data-ttu-id="40baf-429">保留以供内部使用。</span><span class="sxs-lookup"><span data-stu-id="40baf-429">Reserved for internal use.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-430">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-430">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadcreating-event"></a><span data-ttu-id="40baf-431">ThreadCreating 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-431">ThreadCreating event</span></span>

 <span data-ttu-id="40baf-432">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-432">The following table shows the keywords and level.</span></span>

|<span data-ttu-id="40baf-433">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-433">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-434">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-434">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-435">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-435">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-436">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-436">Informational (4)</span></span>|

 <span data-ttu-id="40baf-437">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-437">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-438">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-438">Event</span></span>|<span data-ttu-id="40baf-439">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-439">Event ID</span></span>|<span data-ttu-id="40baf-440">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-440">Description</span></span>|
|----------------|---------------|-----------------|
|`ThreadCreating`|<span data-ttu-id="40baf-441">70</span><span class="sxs-lookup"><span data-stu-id="40baf-441">70</span></span>|<span data-ttu-id="40baf-442">已创建线程。</span><span class="sxs-lookup"><span data-stu-id="40baf-442">Thread has been created.</span></span>|

 <span data-ttu-id="40baf-443">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-443">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-444">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-444">Field name</span></span>|<span data-ttu-id="40baf-445">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-445">Data type</span></span>|<span data-ttu-id="40baf-446">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-446">Description</span></span>|
|----------------|---------------|-----------------|
|`ID`|`win:Pointer`|<span data-ttu-id="40baf-447">线程 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-447">Thread ID</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-448">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-448">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="threadrunning-event"></a><span data-ttu-id="40baf-449">ThreadRunning 事件</span><span class="sxs-lookup"><span data-stu-id="40baf-449">ThreadRunning event</span></span>

 <span data-ttu-id="40baf-450">下表显示了关键字和级别。</span><span class="sxs-lookup"><span data-stu-id="40baf-450">The following table shows the keywords and level.</span></span>

|<span data-ttu-id="40baf-451">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="40baf-451">Keyword for raising the event</span></span>|<span data-ttu-id="40baf-452">Level</span><span class="sxs-lookup"><span data-stu-id="40baf-452">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="40baf-453">`ThreadingKeyword` (0x10000)</span><span class="sxs-lookup"><span data-stu-id="40baf-453">`ThreadingKeyword` (0x10000)</span></span>|<span data-ttu-id="40baf-454">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="40baf-454">Informational (4)</span></span>|

 <span data-ttu-id="40baf-455">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="40baf-455">The following table shows the event information.</span></span>

|<span data-ttu-id="40baf-456">事件</span><span class="sxs-lookup"><span data-stu-id="40baf-456">Event</span></span>|<span data-ttu-id="40baf-457">事件 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-457">Event ID</span></span>|<span data-ttu-id="40baf-458">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-458">Description</span></span>|
|----------------|---------------|-----------------|
|`ThreadRunning`|<span data-ttu-id="40baf-459">71</span><span class="sxs-lookup"><span data-stu-id="40baf-459">71</span></span>|<span data-ttu-id="40baf-460">线程已开始运行。</span><span class="sxs-lookup"><span data-stu-id="40baf-460">Thread has started running.</span></span>|

 <span data-ttu-id="40baf-461">下表显示了事件数据。</span><span class="sxs-lookup"><span data-stu-id="40baf-461">The following table shows the event data.</span></span>

|<span data-ttu-id="40baf-462">字段名</span><span class="sxs-lookup"><span data-stu-id="40baf-462">Field name</span></span>|<span data-ttu-id="40baf-463">数据类型</span><span class="sxs-lookup"><span data-stu-id="40baf-463">Data type</span></span>|<span data-ttu-id="40baf-464">说明</span><span class="sxs-lookup"><span data-stu-id="40baf-464">Description</span></span>|
|----------------|---------------|-----------------|
|`ID`|`win:Pointer`|<span data-ttu-id="40baf-465">线程 ID</span><span class="sxs-lookup"><span data-stu-id="40baf-465">Thread ID</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="40baf-466">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="40baf-466">Unique ID for the instance of CoreCLR.</span></span>|
