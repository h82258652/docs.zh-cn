---
title: 异常运行时事件
description: 请参阅收集特定于异常和异常处理的诊断信息的 .NET 运行时事件。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- exception events (CoreCLR)
- exception handling events (CoreCLR)
- ETW, EventPipe, LTTng exception events (CoreCLR)
ms.openlocfilehash: f4f63c8469f9c734b872ddaec8d1d7f7f0427576
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591077"
---
# <a name="net-runtime-exception-events"></a><span data-ttu-id="06951-103">.NET 运行时异常事件</span><span class="sxs-lookup"><span data-stu-id="06951-103">.NET runtime exception events</span></span>

<span data-ttu-id="06951-104">这些运行时事件捕获有关引发的异常的信息。</span><span class="sxs-lookup"><span data-stu-id="06951-104">These runtime events capture information about exceptions that are thrown.</span></span> <span data-ttu-id="06951-105">有关如何将这些事件用于诊断的详细信息，请参阅 [.NET 应用程序日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="06951-105">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

## <a name="exceptionthrown_v1-event"></a><span data-ttu-id="06951-106">ExceptionThrown_V1 事件</span><span class="sxs-lookup"><span data-stu-id="06951-106">ExceptionThrown_V1 event</span></span>

|<span data-ttu-id="06951-107">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-107">Keyword for raising the event</span></span>|<span data-ttu-id="06951-108">Level</span><span class="sxs-lookup"><span data-stu-id="06951-108">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-109">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-109">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-110">错误 (1)</span><span class="sxs-lookup"><span data-stu-id="06951-110">Error (1)</span></span>|

 <span data-ttu-id="06951-111">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-111">The following table shows event information.</span></span>

|<span data-ttu-id="06951-112">事件</span><span class="sxs-lookup"><span data-stu-id="06951-112">Event</span></span>|<span data-ttu-id="06951-113">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-113">Event ID</span></span>|<span data-ttu-id="06951-114">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-114">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionThrown_V1`|<span data-ttu-id="06951-115">80</span><span class="sxs-lookup"><span data-stu-id="06951-115">80</span></span>|<span data-ttu-id="06951-116">引发托管异常。</span><span class="sxs-lookup"><span data-stu-id="06951-116">A managed exception is thrown.</span></span>|

|<span data-ttu-id="06951-117">字段名</span><span class="sxs-lookup"><span data-stu-id="06951-117">Field name</span></span>|<span data-ttu-id="06951-118">数据类型</span><span class="sxs-lookup"><span data-stu-id="06951-118">Data type</span></span>|<span data-ttu-id="06951-119">说明</span><span class="sxs-lookup"><span data-stu-id="06951-119">Description</span></span>|
|----------------|---------------|-----------------|
|`ExceptionType`|`win:UnicodeString`|<span data-ttu-id="06951-120">异常的类型，例如，`System.NullReferenceException`。</span><span class="sxs-lookup"><span data-stu-id="06951-120">Type of the exception; for example, `System.NullReferenceException`.</span></span>|
|`ExceptionMessage`|`win:UnicodeString`|<span data-ttu-id="06951-121">实际的异常消息。</span><span class="sxs-lookup"><span data-stu-id="06951-121">Actual exception message.</span></span>|
|`EIPCodeThrow`|`win:Pointer`|<span data-ttu-id="06951-122">指向异常发生位置的指令指针。</span><span class="sxs-lookup"><span data-stu-id="06951-122">Instruction pointer where exception occurred.</span></span>|
|`ExceptionHR`|`win:UInt32`|<span data-ttu-id="06951-123">异常 [HRESULT](/openspecs/windows_protocols/ms-erref/0642cb2f-2075-4469-918c-4441e69c548a)。</span><span class="sxs-lookup"><span data-stu-id="06951-123">Exception [HRESULT](/openspecs/windows_protocols/ms-erref/0642cb2f-2075-4469-918c-4441e69c548a).</span></span>|
|`ExceptionFlags`|`win:UInt16`|<span data-ttu-id="06951-124">`0x01`: HasInnerException。</span><span class="sxs-lookup"><span data-stu-id="06951-124">`0x01`: HasInnerException.</span></span><br /><br /> <span data-ttu-id="06951-125">`0x02`: IsNestedException。</span><span class="sxs-lookup"><span data-stu-id="06951-125">`0x02`: IsNestedException.</span></span><br /><br /> <span data-ttu-id="06951-126">`0x04`: IsRethrownException。</span><span class="sxs-lookup"><span data-stu-id="06951-126">`0x04`: IsRethrownException.</span></span><br /><br /> <span data-ttu-id="06951-127">`0x08`: IsCorruptedStateException（指示进程状态已损坏，请参阅[处理损坏状态异常](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)）。</span><span class="sxs-lookup"><span data-stu-id="06951-127">`0x08`: IsCorruptedStateException (indicates that the process state is corrupt; see [Handling Corrupted State Exceptions](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions)).</span></span><br /><br /> <span data-ttu-id="06951-128">`0x10`: IsCLSCompliant（从 <xref:System.Exception> 派生的异常符合 CLS，此外的其他异常均不符合 CLS）。</span><span class="sxs-lookup"><span data-stu-id="06951-128">`0x10`: IsCLSCompliant (an exception that derives from <xref:System.Exception> is CLS-compliant; otherwise, it is not CLS-compliant).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="06951-129">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="06951-129">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="exceptioncatchstart-event"></a><span data-ttu-id="06951-130">ExceptionCatchStart 事件</span><span class="sxs-lookup"><span data-stu-id="06951-130">ExceptionCatchStart event</span></span>

<span data-ttu-id="06951-131">当托管异常 catch 处理程序开始时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-131">This event is emitted when a managed exception catch handler begins.</span></span>

|<span data-ttu-id="06951-132">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-132">Keyword for raising the event</span></span>|<span data-ttu-id="06951-133">Level</span><span class="sxs-lookup"><span data-stu-id="06951-133">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-134">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-134">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-135">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-135">Informational (4)</span></span>|

 <span data-ttu-id="06951-136">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-136">The following table shows event information.</span></span>

|<span data-ttu-id="06951-137">事件</span><span class="sxs-lookup"><span data-stu-id="06951-137">Event</span></span>|<span data-ttu-id="06951-138">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-138">Event ID</span></span>|<span data-ttu-id="06951-139">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-139">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionCatchStart`|<span data-ttu-id="06951-140">250</span><span class="sxs-lookup"><span data-stu-id="06951-140">250</span></span>|<span data-ttu-id="06951-141">托管异常由运行时处理。</span><span class="sxs-lookup"><span data-stu-id="06951-141">A managed exception is handled by the runtime.</span></span>|

|<span data-ttu-id="06951-142">字段名</span><span class="sxs-lookup"><span data-stu-id="06951-142">Field name</span></span>|<span data-ttu-id="06951-143">数据类型</span><span class="sxs-lookup"><span data-stu-id="06951-143">Data type</span></span>|<span data-ttu-id="06951-144">说明</span><span class="sxs-lookup"><span data-stu-id="06951-144">Description</span></span>|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|<span data-ttu-id="06951-145">指向异常发生位置的指令指针。</span><span class="sxs-lookup"><span data-stu-id="06951-145">Instruction pointer where exception occurred.</span></span>|
|`MethodID`|`win:Pointer`|<span data-ttu-id="06951-146">指向发生异常的方法上的方法描述符的指针。</span><span class="sxs-lookup"><span data-stu-id="06951-146">Pointer to the method descriptor on the method where exception occurred.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="06951-147">发生异常的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="06951-147">Name of the method where exception occurred.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="06951-148">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="06951-148">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="exceptioncatchstop-event"></a><span data-ttu-id="06951-149">ExceptionCatchStop 事件</span><span class="sxs-lookup"><span data-stu-id="06951-149">ExceptionCatchStop event</span></span>

<span data-ttu-id="06951-150">当托管异常 catch 处理程序结束时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-150">This event is emitted when a managed exception catch handler ends.</span></span>

|<span data-ttu-id="06951-151">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-151">Keyword for raising the event</span></span>|<span data-ttu-id="06951-152">Level</span><span class="sxs-lookup"><span data-stu-id="06951-152">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-153">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-153">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-154">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-154">Informational (4)</span></span>|

 <span data-ttu-id="06951-155">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-155">The following table shows event information.</span></span>

|<span data-ttu-id="06951-156">事件</span><span class="sxs-lookup"><span data-stu-id="06951-156">Event</span></span>|<span data-ttu-id="06951-157">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-157">Event ID</span></span>|<span data-ttu-id="06951-158">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-158">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionCatchStop`|<span data-ttu-id="06951-159">251</span><span class="sxs-lookup"><span data-stu-id="06951-159">251</span></span>|<span data-ttu-id="06951-160">托管异常 catch 处理程序已完成。</span><span class="sxs-lookup"><span data-stu-id="06951-160">A managed exception catch handler is done.</span></span>|

## <a name="exceptionfinallystart-event"></a><span data-ttu-id="06951-161">ExceptionFinallyStart 事件</span><span class="sxs-lookup"><span data-stu-id="06951-161">ExceptionFinallyStart event</span></span>

<span data-ttu-id="06951-162">当托管异常 finally 处理程序开始时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-162">This event is emitted when a managed exception finally handler begins.</span></span>

|<span data-ttu-id="06951-163">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-163">Keyword for raising the event</span></span>|<span data-ttu-id="06951-164">Level</span><span class="sxs-lookup"><span data-stu-id="06951-164">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-165">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-165">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-166">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-166">Informational (4)</span></span>|

 <span data-ttu-id="06951-167">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-167">The following table shows event information.</span></span>

|<span data-ttu-id="06951-168">事件</span><span class="sxs-lookup"><span data-stu-id="06951-168">Event</span></span>|<span data-ttu-id="06951-169">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-169">Event ID</span></span>|<span data-ttu-id="06951-170">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-170">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionFinallyStart`|<span data-ttu-id="06951-171">252</span><span class="sxs-lookup"><span data-stu-id="06951-171">252</span></span>|<span data-ttu-id="06951-172">托管异常由运行时处理。</span><span class="sxs-lookup"><span data-stu-id="06951-172">A managed exception is handled by the runtime.</span></span>|

|<span data-ttu-id="06951-173">字段名</span><span class="sxs-lookup"><span data-stu-id="06951-173">Field name</span></span>|<span data-ttu-id="06951-174">数据类型</span><span class="sxs-lookup"><span data-stu-id="06951-174">Data type</span></span>|<span data-ttu-id="06951-175">说明</span><span class="sxs-lookup"><span data-stu-id="06951-175">Description</span></span>|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|<span data-ttu-id="06951-176">指向异常发生位置的指令指针。</span><span class="sxs-lookup"><span data-stu-id="06951-176">Instruction pointer where exception occurred.</span></span>|
|`MethodID`|`win:Pointer`|<span data-ttu-id="06951-177">指向发生异常的方法上的方法描述符的指针。</span><span class="sxs-lookup"><span data-stu-id="06951-177">Pointer to the method descriptor on the method where exception occurred.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="06951-178">发生异常的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="06951-178">Name of the method where exception occurred.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="06951-179">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="06951-179">Unique ID for the instance of CLR or CoreCLR.</span></span>|

## <a name="exceptionfinallystop-event"></a><span data-ttu-id="06951-180">ExceptionFinallyStop 事件</span><span class="sxs-lookup"><span data-stu-id="06951-180">ExceptionFinallyStop event</span></span>

<span data-ttu-id="06951-181">当托管异常 catch 处理程序结束时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-181">This event is emitted when a managed exception catch handler ends.</span></span>

|<span data-ttu-id="06951-182">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-182">Keyword for raising the event</span></span>|<span data-ttu-id="06951-183">Level</span><span class="sxs-lookup"><span data-stu-id="06951-183">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-184">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-184">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-185">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-185">Informational (4)</span></span>|

 <span data-ttu-id="06951-186">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-186">The following table shows event information.</span></span>

|<span data-ttu-id="06951-187">事件</span><span class="sxs-lookup"><span data-stu-id="06951-187">Event</span></span>|<span data-ttu-id="06951-188">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-188">Event ID</span></span>|<span data-ttu-id="06951-189">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-189">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionFinallyStop`|<span data-ttu-id="06951-190">253</span><span class="sxs-lookup"><span data-stu-id="06951-190">253</span></span>|<span data-ttu-id="06951-191">托管异常 finally 处理程序已完成。</span><span class="sxs-lookup"><span data-stu-id="06951-191">A managed exception finally handler is done.</span></span>|

## <a name="exceptionfilterstart-event"></a><span data-ttu-id="06951-192">ExceptionFilterStart 事件</span><span class="sxs-lookup"><span data-stu-id="06951-192">ExceptionFilterStart event</span></span>

<span data-ttu-id="06951-193">当托管异常筛选开始时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-193">This event is emitted when a managed exception filtering begins.</span></span>

|<span data-ttu-id="06951-194">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-194">Keyword for raising the event</span></span>|<span data-ttu-id="06951-195">Level</span><span class="sxs-lookup"><span data-stu-id="06951-195">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-196">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-196">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-197">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-197">Informational (4)</span></span>|

 <span data-ttu-id="06951-198">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-198">The following table shows event information.</span></span>

|<span data-ttu-id="06951-199">事件</span><span class="sxs-lookup"><span data-stu-id="06951-199">Event</span></span>|<span data-ttu-id="06951-200">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-200">Event ID</span></span>|<span data-ttu-id="06951-201">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-201">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionFilterStart`|<span data-ttu-id="06951-202">254</span><span class="sxs-lookup"><span data-stu-id="06951-202">254</span></span>|<span data-ttu-id="06951-203">托管异常筛选开始。</span><span class="sxs-lookup"><span data-stu-id="06951-203">A managed exception filtering begins.</span></span>|

|<span data-ttu-id="06951-204">字段名</span><span class="sxs-lookup"><span data-stu-id="06951-204">Field name</span></span>|<span data-ttu-id="06951-205">数据类型</span><span class="sxs-lookup"><span data-stu-id="06951-205">Data type</span></span>|<span data-ttu-id="06951-206">说明</span><span class="sxs-lookup"><span data-stu-id="06951-206">Description</span></span>|
|----------------|---------------|-----------------|
|`EIPCodeThrow`|`win:Pointer`|<span data-ttu-id="06951-207">指向异常发生位置的指令指针。</span><span class="sxs-lookup"><span data-stu-id="06951-207">Instruction pointer where exception occurred.</span></span>|
|`MethodID`|`win:Pointer`|<span data-ttu-id="06951-208">指向发生异常的方法上的方法描述符的指针。</span><span class="sxs-lookup"><span data-stu-id="06951-208">Pointer to the method descriptor on the method where exception occurred.</span></span>|
|`MethodName`|`win:UnicodeString`|<span data-ttu-id="06951-209">发生异常的方法的名称。</span><span class="sxs-lookup"><span data-stu-id="06951-209">Name of the method where exception occurred.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="06951-210">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="06951-210">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="exceptionfilterstop-event"></a><span data-ttu-id="06951-211">ExceptionFilterStop 事件</span><span class="sxs-lookup"><span data-stu-id="06951-211">ExceptionFilterStop event</span></span>

<span data-ttu-id="06951-212">当托管异常筛选结束时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-212">This event is emitted when a managed exception filtering ends.</span></span>

|<span data-ttu-id="06951-213">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-213">Keyword for raising the event</span></span>|<span data-ttu-id="06951-214">Level</span><span class="sxs-lookup"><span data-stu-id="06951-214">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-215">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-215">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-216">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-216">Informational (4)</span></span>|

 <span data-ttu-id="06951-217">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-217">The following table shows event information.</span></span>

|<span data-ttu-id="06951-218">事件</span><span class="sxs-lookup"><span data-stu-id="06951-218">Event</span></span>|<span data-ttu-id="06951-219">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-219">Event ID</span></span>|<span data-ttu-id="06951-220">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-220">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionFilteringStart`|<span data-ttu-id="06951-221">255</span><span class="sxs-lookup"><span data-stu-id="06951-221">255</span></span>|<span data-ttu-id="06951-222">托管异常筛选结束。</span><span class="sxs-lookup"><span data-stu-id="06951-222">A managed exception filtering ends.</span></span>|

## <a name="exceptionthrownstop-event"></a><span data-ttu-id="06951-223">ExceptionThrownStop 事件</span><span class="sxs-lookup"><span data-stu-id="06951-223">ExceptionThrownStop event</span></span>

<span data-ttu-id="06951-224">当运行时完成处理引发的托管异常时，将发出此事件。</span><span class="sxs-lookup"><span data-stu-id="06951-224">This event is emitted when the runtime is done handling a managed exception that was thrown.</span></span>

|<span data-ttu-id="06951-225">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="06951-225">Keyword for raising the event</span></span>|<span data-ttu-id="06951-226">Level</span><span class="sxs-lookup"><span data-stu-id="06951-226">Level</span></span>|
|-----------------------------------|-----------|
|<span data-ttu-id="06951-227">`ExceptionKeyword` (0x8000)</span><span class="sxs-lookup"><span data-stu-id="06951-227">`ExceptionKeyword` (0x8000)</span></span>|<span data-ttu-id="06951-228">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="06951-228">Informational (4)</span></span>|
  
 <span data-ttu-id="06951-229">下表显示了事件信息。</span><span class="sxs-lookup"><span data-stu-id="06951-229">The following table shows event information.</span></span>

|<span data-ttu-id="06951-230">事件</span><span class="sxs-lookup"><span data-stu-id="06951-230">Event</span></span>|<span data-ttu-id="06951-231">事件 ID</span><span class="sxs-lookup"><span data-stu-id="06951-231">Event ID</span></span>|<span data-ttu-id="06951-232">在发生以下情况时引发</span><span class="sxs-lookup"><span data-stu-id="06951-232">Raised when</span></span>|
|-----------|--------------|-----------------|
|`ExceptionThrownStop`|<span data-ttu-id="06951-233">256</span><span class="sxs-lookup"><span data-stu-id="06951-233">256</span></span>|<span data-ttu-id="06951-234">托管异常筛选结束。</span><span class="sxs-lookup"><span data-stu-id="06951-234">A managed exception filtering ends.</span></span>|
