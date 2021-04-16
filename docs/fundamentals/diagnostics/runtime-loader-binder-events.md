---
title: 加载器和绑定器运行时事件
description: 查看收集特定于加载器和绑定器 ETW 事件的诊断信息的 .NET 运行时事件，ETW 事件收集有关程序集加载器和绑定器的信息。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- Assembly Loader events (CoreCLR)
- Assembly Binder events (CoreCLR)
- ETW, EventPipe, LTTng assembly loader and binder events (CoreCLR)
ms.openlocfilehash: 2284c580482f6b93a77f44649225ff7e5485666a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591079"
---
# <a name="net-runtime-loader-and-binder-events"></a><span data-ttu-id="1c0cc-103">.NET 运行时加载器和绑定器事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-103">.NET runtime loader and binder events</span></span>

<span data-ttu-id="1c0cc-104">这些事件收集有关加载和卸载程序集和模块的信息。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-104">These events collect information relating to loading and unloading assemblies and modules.</span></span> <span data-ttu-id="1c0cc-105">有关如何将这些事件用于诊断的详细信息，请参阅[对 .NET 应用程序进行日志记录和跟踪](../../core/diagnostics/logging-tracing.md)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-105">For more information about how to use these events for diagnostic purposes, see [logging and tracing .NET applications](../../core/diagnostics/logging-tracing.md)</span></span>

|<span data-ttu-id="1c0cc-106">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-106">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-107">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-107">Event</span></span>|<span data-ttu-id="1c0cc-108">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-108">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-109">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-109">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-110">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-110">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-111">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-111">Event</span></span>|<span data-ttu-id="1c0cc-112">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-112">Event ID</span></span>|<span data-ttu-id="1c0cc-113">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-113">Description</span></span>|
|-----------|--------------|-----------------|
|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-114">151</span><span class="sxs-lookup"><span data-stu-id="1c0cc-114">151</span></span>|<span data-ttu-id="1c0cc-115">在为应用程序域加载模块时引发。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-115">Raised when a module is loaded for an application domain.</span></span>|

## <a name="moduleload_v2-event"></a><span data-ttu-id="1c0cc-116">ModuleLoad_V2 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-116">ModuleLoad_V2 event</span></span>

|<span data-ttu-id="1c0cc-117">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-117">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-118">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-118">Event</span></span>|<span data-ttu-id="1c0cc-119">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-119">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-120">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-120">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-121">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-121">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-122">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-122">Event</span></span>|<span data-ttu-id="1c0cc-123">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-123">Event ID</span></span>|<span data-ttu-id="1c0cc-124">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-124">Description</span></span>|
|-----------|--------------|-----------------|
|`ModuleLoad_V2`|<span data-ttu-id="1c0cc-125">152</span><span class="sxs-lookup"><span data-stu-id="1c0cc-125">152</span></span>|<span data-ttu-id="1c0cc-126">在进程的生存期内加载模块时引发。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-126">Raised when a module is loaded during the lifetime of a process.</span></span>|

|<span data-ttu-id="1c0cc-127">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-127">Field name</span></span>|<span data-ttu-id="1c0cc-128">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-128">Data type</span></span>|<span data-ttu-id="1c0cc-129">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-129">Description</span></span>|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="1c0cc-130">模块的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-130">Unique ID for the module.</span></span>|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-131">此模块所驻留的程序集的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-131">ID of the assembly in which this module resides.</span></span>|
|`ModuleFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-132">0x1：非特定于域的模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-132">0x1: Domain neutral module.</span></span><br /><br /> <span data-ttu-id="1c0cc-133">0x2：模块具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-133">0x2: Module has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-134">0x4：动态模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-134">0x4: Dynamic module.</span></span><br /><br /> <span data-ttu-id="1c0cc-135">0x8：清单模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-135">0x8: Manifest module.</span></span>|
|`Reserved1`|`win:UInt32`|<span data-ttu-id="1c0cc-136">保留的字段。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-136">Reserved field.</span></span>|
|`ModuleILPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-137">模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-137">Path of the Common Intermediate Language (CIL) image for the module, or dynamic module name if it is a dynamic assembly (null-terminated).</span></span>|
|`ModuleNativePath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-138">如果存在（以 null 结尾），则为模块本机映像的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-138">Path of the module native image, if present (null-terminated).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-139">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-139">Unique ID for the instance of CLR or CoreCLR.</span></span>|
|`ManagedPdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-140">匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-140">GUID signature of the managed program database (PDB) that matches this module.</span></span>|
|`ManagedPdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-141">写入匹配此模块的托管 PDB 的年限数。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-141">Age number written to the managed PDB that matches this module.</span></span>|
|`ManagedPdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-142">生成匹配此模块的托管 PDB 的位置的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-142">Path to the location where the managed PDB that matches this module was built.</span></span> <span data-ttu-id="1c0cc-143">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-143">In some cases, this may just be a file name.</span></span>|
|`NativePdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-144">匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-144">GUID signature of the Native Image Generator (NGen) PDB that matches this module, if applicable.</span></span>|
|`NativePdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-145">写入匹配此模块的 NGen PDB 的年限数（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-145">Age number written to the NGen PDB that matches this module, if applicable.</span></span>|
|`NativePdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-146">生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-146">Path to the location where the NGen PDB that matches this module was built, if applicable.</span></span> <span data-ttu-id="1c0cc-147">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-147">In some cases, this may just be a file name.</span></span>|

## <a name="moduleunload_v2-event"></a><span data-ttu-id="1c0cc-148">ModuleUnload_V2 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-148">ModuleUnload_V2 event</span></span>

|<span data-ttu-id="1c0cc-149">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-149">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-150">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-150">Event</span></span>|<span data-ttu-id="1c0cc-151">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-151">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-152">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-152">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-153">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-153">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-154">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-154">Event</span></span>|<span data-ttu-id="1c0cc-155">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-155">Event ID</span></span>|<span data-ttu-id="1c0cc-156">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-156">Description</span></span>|
|-----------|--------------|-----------------|
|`ModuleUnload_V2`|<span data-ttu-id="1c0cc-157">153</span><span class="sxs-lookup"><span data-stu-id="1c0cc-157">153</span></span>|<span data-ttu-id="1c0cc-158">在进程的生存期内卸载模块时引发。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-158">Raised when a module is unloaded during the lifetime of a process.</span></span>|

|<span data-ttu-id="1c0cc-159">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-159">Field name</span></span>|<span data-ttu-id="1c0cc-160">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-160">Data type</span></span>|<span data-ttu-id="1c0cc-161">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-161">Description</span></span>|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="1c0cc-162">模块的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-162">Unique ID for the module.</span></span>|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-163">此模块所驻留的程序集的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-163">ID of the assembly in which this module resides.</span></span>|
|`ModuleFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-164">0x1：非特定于域的模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-164">0x1: Domain neutral module.</span></span><br /><br /> <span data-ttu-id="1c0cc-165">0x2：模块具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-165">0x2: Module has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-166">0x4：动态模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-166">0x4: Dynamic module.</span></span><br /><br /> <span data-ttu-id="1c0cc-167">0x8：清单模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-167">0x8: Manifest module.</span></span>|
|`Reserved1`|`win:UInt32`|<span data-ttu-id="1c0cc-168">保留的字段。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-168">Reserved field.</span></span>|
|`ModuleILPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-169">模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-169">Path of the Common Intermediate Language (CIL) image for the module, or dynamic module name if it is a dynamic assembly (null-terminated).</span></span>|
|`ModuleNativePath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-170">如果存在（以 null 结尾），则为模块本机映像的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-170">Path of the module native image, if present (null-terminated).</span></span>|
|`ClrInstanceID`|``win:UInt16``|<span data-ttu-id="1c0cc-171">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-171">Unique ID for the instance of CLR or CoreCLR.</span></span>|
|`ManagedPdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-172">匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-172">GUID signature of the managed program database (PDB) that matches this module.</span></span>|
|`ManagedPdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-173">写入匹配此模块的托管 PDB 的年限数。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-173">Age number written to the managed PDB that matches this module.</span></span>|
|`ManagedPdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-174">生成匹配此模块的托管 PDB 的位置的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-174">Path to the location where the managed PDB that matches this module was built.</span></span> <span data-ttu-id="1c0cc-175">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-175">In some cases, this may just be a file name.</span></span>|
|`NativePdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-176">匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-176">GUID signature of the Native Image Generator (NGen) PDB that matches this module, if applicable.</span></span>|
|`NativePdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-177">写入匹配此模块的 NGen PDB 的年限数（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-177">Age number written to the NGen PDB that matches this module, if applicable.</span></span>|
|`NativePdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-178">生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-178">Path to the location where the NGen PDB that matches this module was built, if applicable.</span></span> <span data-ttu-id="1c0cc-179">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-179">In some cases, this may just be a file name.</span></span>|

## <a name="moduledcstart_v2-event"></a><span data-ttu-id="1c0cc-180">ModuleDCStart_V2 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-180">ModuleDCStart_V2 event</span></span>

|<span data-ttu-id="1c0cc-181">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-181">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-182">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-182">Event</span></span>|<span data-ttu-id="1c0cc-183">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-183">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-184">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-184">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-185">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-185">Informational (4)</span></span>

|<span data-ttu-id="1c0cc-186">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-186">Event</span></span>|<span data-ttu-id="1c0cc-187">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-187">Event ID</span></span>|<span data-ttu-id="1c0cc-188">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-188">Description</span></span>
|-----------|--------------|-----------------|
|`ModuleDCStart_V2`|<span data-ttu-id="1c0cc-189">153</span><span class="sxs-lookup"><span data-stu-id="1c0cc-189">153</span></span>|<span data-ttu-id="1c0cc-190">在启动断开期间枚举模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-190">Enumerates modules during a start rundown.</span></span>|

|<span data-ttu-id="1c0cc-191">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-191">Field name</span></span>|<span data-ttu-id="1c0cc-192">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-192">Data type</span></span>|<span data-ttu-id="1c0cc-193">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-193">Description</span></span>|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="1c0cc-194">模块的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-194">Unique ID for the module.</span></span>|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-195">此模块所驻留的程序集的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-195">ID of the assembly in which this module resides.</span></span>|
|`ModuleFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-196">0x1：非特定于域的模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-196">0x1: Domain neutral module.</span></span><br /><br /> <span data-ttu-id="1c0cc-197">0x2：模块具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-197">0x2: Module has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-198">0x4：动态模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-198">0x4: Dynamic module.</span></span><br /><br /> <span data-ttu-id="1c0cc-199">0x8：清单模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-199">0x8: Manifest module.</span></span>|
|`Reserved1`|`win:UInt32`|<span data-ttu-id="1c0cc-200">保留的字段。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-200">Reserved field.</span></span>|
|`ModuleILPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-201">模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-201">Path of the Common Intermediate Language (CIL) image for the module, or dynamic module name if it is a dynamic assembly (null-terminated).</span></span>|
|`ModuleNativePath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-202">如果存在（以 null 结尾），则为模块本机映像的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-202">Path of the module native image, if present (null-terminated).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-203">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-203">Unique ID for the instance of CLR or CoreCLR.</span></span>|
|`ManagedPdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-204">匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-204">GUID signature of the managed program database (PDB) that matches this module.</span></span>|
|`ManagedPdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-205">写入匹配此模块的托管 PDB 的年限数。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-205">Age number written to the managed PDB that matches this module.</span></span>|
|`ManagedPdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-206">生成匹配此模块的托管 PDB 的位置的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-206">Path to the location where the managed PDB that matches this module was built.</span></span> <span data-ttu-id="1c0cc-207">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-207">In some cases, this may just be a file name.</span></span>|
|`NativePdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-208">匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-208">GUID signature of the Native Image Generator (NGen) PDB that matches this module, if applicable.</span></span>|
|`NativePdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-209">写入匹配此模块的 NGen PDB 的年限数（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-209">Age number written to the NGen PDB that matches this module, if applicable.</span></span>|
|`NativePdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-210">生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-210">Path to the location where the NGen PDB that matches this module was built, if applicable.</span></span> <span data-ttu-id="1c0cc-211">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-211">In some cases, this may just be a file name.</span></span>|

## <a name="moduledcend_v2-event"></a><span data-ttu-id="1c0cc-212">ModuleDCEnd_V2 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-212">ModuleDCEnd_V2 event</span></span>

|<span data-ttu-id="1c0cc-213">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-213">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-214">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-214">Event</span></span>|<span data-ttu-id="1c0cc-215">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-215">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-216">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-216">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-217">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-217">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-218">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-218">Event</span></span>|<span data-ttu-id="1c0cc-219">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-219">Event ID</span></span>|<span data-ttu-id="1c0cc-220">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-220">Description</span></span>|
|-----------|--------------|-----------------|
|`ModuleDCEnd_V2`|<span data-ttu-id="1c0cc-221">154</span><span class="sxs-lookup"><span data-stu-id="1c0cc-221">154</span></span>|<span data-ttu-id="1c0cc-222">在结束断开期间枚举模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-222">Enumerates modules during an end rundown.</span></span>|

|<span data-ttu-id="1c0cc-223">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-223">Field name</span></span>|<span data-ttu-id="1c0cc-224">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-224">Data type</span></span>|<span data-ttu-id="1c0cc-225">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-225">Description</span></span>|
|----------------|---------------|-----------------|
|`ModuleID`|`win:UInt64`|<span data-ttu-id="1c0cc-226">模块的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-226">Unique ID for the module.</span></span>|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-227">此模块所驻留的程序集的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-227">ID of the assembly in which this module resides.</span></span>|
|`ModuleFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-228">0x1：非特定于域的模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-228">0x1: Domain neutral module.</span></span><br /><br /> <span data-ttu-id="1c0cc-229">0x2：模块具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-229">0x2: Module has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-230">0x4：动态模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-230">0x4: Dynamic module.</span></span><br /><br /> <span data-ttu-id="1c0cc-231">0x8：清单模块。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-231">0x8: Manifest module.</span></span>|
|`Reserved1`|`win:UInt32`|<span data-ttu-id="1c0cc-232">保留的字段。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-232">Reserved field.</span></span>|
|`ModuleILPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-233">模块的公共中间语言 (CIL) 映像的路径；如果是动态程序集（以 null 结尾），则为动态模块名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-233">Path of the Common Intermediate Language (CIL) image for the module, or dynamic module name if it is a dynamic assembly (null-terminated).</span></span>|
|`ModuleNativePath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-234">如果存在（以 null 结尾），则为模块本机映像的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-234">Path of the module native image, if present (null-terminated).</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-235">CLR 或 CoreCLR 的实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-235">Unique ID for the instance of CLR or CoreCLR.</span></span>|
|`ManagedPdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-236">匹配此模块的托管程序数据库 (PDB) 的 GUID 签名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-236">GUID signature of the managed program database (PDB) that matches this module.</span></span>|
|`ManagedPdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-237">写入匹配此模块的托管 PDB 的年限数。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-237">Age number written to the managed PDB that matches this module.</span></span>|
|`ManagedPdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-238">生成匹配此模块的托管 PDB 的位置的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-238">Path to the location where the managed PDB that matches this module was built.</span></span> <span data-ttu-id="1c0cc-239">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-239">In some cases, this may just be a file name.</span></span>|
|`NativePdbSignature`|`win:GUID`|<span data-ttu-id="1c0cc-240">匹配此模块的本机映像生成器 (NGen) PDB 的 GUID 签名（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-240">GUID signature of the Native Image Generator (NGen) PDB that matches this module, if applicable.</span></span>|
|`NativePdbAge`|`win:UInt32`|<span data-ttu-id="1c0cc-241">写入匹配此模块的 NGen PDB 的年限数（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-241">Age number written to the NGen PDB that matches this module, if applicable.</span></span>|
|`NativePdbBuildPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-242">生成匹配此模块的 NGen PDB 的位置的路径（如果适用）。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-242">Path to the location where the NGen PDB that matches this module was built, if applicable.</span></span> <span data-ttu-id="1c0cc-243">在某些情况下，这可能只是一个文件名。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-243">In some cases, this may just be a file name.</span></span>|

## <a name="assemblyload_v1-event"></a><span data-ttu-id="1c0cc-244">AssemblyLoad_V1 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-244">AssemblyLoad_V1 event</span></span>

|<span data-ttu-id="1c0cc-245">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-245">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-246">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-246">Event</span></span>|<span data-ttu-id="1c0cc-247">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-247">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-248">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-248">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-249">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-249">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-250">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-250">Event</span></span>|<span data-ttu-id="1c0cc-251">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-251">Event ID</span></span>|<span data-ttu-id="1c0cc-252">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-252">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyLoad_V1`|<span data-ttu-id="1c0cc-253">154</span><span class="sxs-lookup"><span data-stu-id="1c0cc-253">154</span></span>|<span data-ttu-id="1c0cc-254">在加载程序集时引发。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-254">Raised when an assembly is loaded.</span></span>|

|<span data-ttu-id="1c0cc-255">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-255">Field name</span></span>|<span data-ttu-id="1c0cc-256">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-256">Data type</span></span>|<span data-ttu-id="1c0cc-257">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-257">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-258">程序集的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-258">Unique ID for the assembly.</span></span>|
|`AppDomainID`|`win:UInt64`|<span data-ttu-id="1c0cc-259">此程序集的域的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-259">ID of the domain of this assembly.</span></span>|
|`BindingID`|`win:UInt64`|<span data-ttu-id="1c0cc-260">唯一地标识程序集绑定的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-260">ID that uniquely identifies the assembly binding.</span></span>|
|`AssemblyFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-261">0x1：非特定于域的程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-261">0x1: Domain neutral assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-262">0x2：动态程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-262">0x2: Dynamic assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-263">0x4：程序集具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-263">0x4: Assembly has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-264">0x8：可回收程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-264">0x8: Collectible assembly.</span></span>|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-265">完全限定程序集名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-265">Fully qualified assembly name.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-266">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-266">Unique ID for the instance of CoreCLR.</span></span>

## <a name="assemblyunload_v1-event"></a><span data-ttu-id="1c0cc-267">AssemblyUnload_V1 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-267">AssemblyUnload_V1 event</span></span>

|<span data-ttu-id="1c0cc-268">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-268">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-269">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-269">Event</span></span>|<span data-ttu-id="1c0cc-270">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-270">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-271">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-271">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-272">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-272">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-273">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-273">Event</span></span>|<span data-ttu-id="1c0cc-274">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-274">Event ID</span></span>|<span data-ttu-id="1c0cc-275">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-275">Description</span></span>|
|-----------|--------------|-----------------|
|`FireAssemblyUnload_V1`|<span data-ttu-id="1c0cc-276">155</span><span class="sxs-lookup"><span data-stu-id="1c0cc-276">155</span></span>|<span data-ttu-id="1c0cc-277">在加载程序集时引发。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-277">Raised when an assembly is loaded.</span></span>|

|<span data-ttu-id="1c0cc-278">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-278">Field name</span></span>|<span data-ttu-id="1c0cc-279">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-279">Data type</span></span>|<span data-ttu-id="1c0cc-280">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-280">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-281">程序集的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-281">Unique ID for the assembly.</span></span>|
|`AppDomainID`|`win:UInt64`|<span data-ttu-id="1c0cc-282">此程序集的域的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-282">ID of the domain of this assembly.</span></span>|
|`BindingID`|`win:UInt64`|<span data-ttu-id="1c0cc-283">唯一地标识程序集绑定的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-283">ID that uniquely identifies the assembly binding.</span></span>|
|`AssemblyFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-284">0x1：非特定于域的程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-284">0x1: Domain neutral assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-285">0x2：动态程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-285">0x2: Dynamic assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-286">0x4：程序集具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-286">0x4: Assembly has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-287">0x8：可回收程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-287">0x8: Collectible assembly.</span></span>|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-288">完全限定程序集名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-288">Fully qualified assembly name.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-289">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-289">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="assemblydcstart_v1-event"></a><span data-ttu-id="1c0cc-290">AssemblyDCStart_V1 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-290">AssemblyDCStart_V1 event</span></span>

|<span data-ttu-id="1c0cc-291">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-291">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-292">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-292">Event</span></span>|<span data-ttu-id="1c0cc-293">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-293">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-294">`LoaderKeyword` (0x8)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-294">`LoaderKeyword` (0x8)</span></span>|`DomainModuleLoad_V1`|<span data-ttu-id="1c0cc-295">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-295">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-296">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-296">Event</span></span>|<span data-ttu-id="1c0cc-297">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-297">Event ID</span></span>|<span data-ttu-id="1c0cc-298">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-298">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyDCStart_V1`|<span data-ttu-id="1c0cc-299">155</span><span class="sxs-lookup"><span data-stu-id="1c0cc-299">155</span></span>|<span data-ttu-id="1c0cc-300">在启动断开期间枚举程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-300">Enumerates assemblies during a start rundown.</span></span>|

|<span data-ttu-id="1c0cc-301">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-301">Field name</span></span>|<span data-ttu-id="1c0cc-302">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-302">Data type</span></span>|<span data-ttu-id="1c0cc-303">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-303">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyID`|`win:UInt64`|<span data-ttu-id="1c0cc-304">程序集的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-304">Unique ID for the assembly.</span></span>|
|`AppDomainID`|`win:UInt64`|<span data-ttu-id="1c0cc-305">此程序集的域的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-305">ID of the domain of this assembly.</span></span>|
|`BindingID`|`win:UInt64`|<span data-ttu-id="1c0cc-306">唯一地标识程序集绑定的 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-306">ID that uniquely identifies the assembly binding.</span></span>|
|`AssemblyFlags`|`win:UInt32`|<span data-ttu-id="1c0cc-307">0x1：非特定于域的程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-307">0x1: Domain neutral assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-308">0x2：动态程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-308">0x2: Dynamic assembly.</span></span><br /><br /> <span data-ttu-id="1c0cc-309">0x4：程序集具有本机映像。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-309">0x4: Assembly has a native image.</span></span><br /><br /> <span data-ttu-id="1c0cc-310">0x8：可回收程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-310">0x8: Collectible assembly.</span></span>|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-311">完全限定程序集名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-311">Fully qualified assembly name.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-312">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-312">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="assemblyloadstart-event"></a><span data-ttu-id="1c0cc-313">AssemblyLoadStart 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-313">AssemblyLoadStart event</span></span>

|<span data-ttu-id="1c0cc-314">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-314">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-315">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-315">Event</span></span>|<span data-ttu-id="1c0cc-316">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-316">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-317">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-317">`Binder` (0x4)</span></span>|`AssemblyLoadStart`|<span data-ttu-id="1c0cc-318">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-318">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-319">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-319">Event</span></span>|<span data-ttu-id="1c0cc-320">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-320">Event ID</span></span>|<span data-ttu-id="1c0cc-321">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-321">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyLoadStart`|<span data-ttu-id="1c0cc-322">290</span><span class="sxs-lookup"><span data-stu-id="1c0cc-322">290</span></span>|<span data-ttu-id="1c0cc-323">已请求程序集加载。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-323">An assembly load has been requested.</span></span>

|<span data-ttu-id="1c0cc-324">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-324">Field name</span></span>|<span data-ttu-id="1c0cc-325">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-325">Data type</span></span>|<span data-ttu-id="1c0cc-326">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-326">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-327">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-327">Name of assembly name.</span></span>|
|`AssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-328">程序集名称的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-328">Path of assembly name.</span></span>|
|`RequestingAssembly`|`win:UnicodeString`|<span data-ttu-id="1c0cc-329">正在请求的（“父”）程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-329">Name of the requesting ("parent") assembly.</span></span>|
|`AssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-330">程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-330">Load context of the assembly.</span></span>|
|`RequestingAssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-331">正在请求的（“父”）程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-331">Load context of the requesting ("parent") assembly.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-332">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-332">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="assemblyloadstop-event"></a><span data-ttu-id="1c0cc-333">AssemblyLoadStop 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-333">AssemblyLoadStop event</span></span>

|<span data-ttu-id="1c0cc-334">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-334">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-335">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-335">Event</span></span>|<span data-ttu-id="1c0cc-336">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-336">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-337">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-337">`Binder` (0x4)</span></span>|`AssemblyLoadStart`|<span data-ttu-id="1c0cc-338">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-338">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-339">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-339">Event</span></span>|<span data-ttu-id="1c0cc-340">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-340">Event ID</span></span>|<span data-ttu-id="1c0cc-341">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-341">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyLoadStart`|<span data-ttu-id="1c0cc-342">291</span><span class="sxs-lookup"><span data-stu-id="1c0cc-342">291</span></span>|<span data-ttu-id="1c0cc-343">已请求程序集加载。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-343">An assembly load has been requested.</span></span>

|<span data-ttu-id="1c0cc-344">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-344">Field name</span></span>|<span data-ttu-id="1c0cc-345">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-345">Data type</span></span>|<span data-ttu-id="1c0cc-346">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-346">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-347">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-347">Name of assembly name.</span></span>|
|`AssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-348">程序集名称的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-348">Path of assembly name.</span></span>|
|`RequestingAssembly`|`win:UnicodeString`|<span data-ttu-id="1c0cc-349">正在请求的（“父”）程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-349">Name of the requesting ("parent") assembly.</span></span>|
|`AssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-350">程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-350">Load context of the assembly.</span></span>|
|`RequestingAssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-351">正在请求的（“父”）程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-351">Load context of the requesting ("parent") assembly.</span></span>|
|`Success`|`win:Boolean`|<span data-ttu-id="1c0cc-352">程序集加载是否成功。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-352">Whether the assembly load succeeded.</span></span>|
|`ResultAssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-353">已加载的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-353">The name of assembly that was loaded.</span></span>|
|`ResultAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-354">从中加载的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-354">The path of the assembly that was loaded from.</span></span>|
|`Cached`|`win:UnicodeString`|<span data-ttu-id="1c0cc-355">是否缓存了加载。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-355">Whether the load was cached.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-356">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-356">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="resolutionattempted-event"></a><span data-ttu-id="1c0cc-357">ResolutionAttempted 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-357">ResolutionAttempted event</span></span>

|<span data-ttu-id="1c0cc-358">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-358">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-359">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-359">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-360">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-360">`Binder` (0x4)</span></span>|<span data-ttu-id="1c0cc-361">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-361">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-362">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-362">Event</span></span>|<span data-ttu-id="1c0cc-363">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-363">Event ID</span></span>|<span data-ttu-id="1c0cc-364">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-364">Description</span></span>|
|-----------|--------------|-----------------|
|`ResolutionAttempted`|<span data-ttu-id="1c0cc-365">292</span><span class="sxs-lookup"><span data-stu-id="1c0cc-365">292</span></span>|<span data-ttu-id="1c0cc-366">已请求程序集加载。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-366">An assembly load has been requested.</span></span>

|<span data-ttu-id="1c0cc-367">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-367">Field name</span></span>|<span data-ttu-id="1c0cc-368">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-368">Data type</span></span>|<span data-ttu-id="1c0cc-369">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-369">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-370">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-370">Name of assembly name.</span></span>|
|`Stage`|`win:UInt16`|<span data-ttu-id="1c0cc-371">解析阶段。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-371">The resolution stage.</span></span><br/><br/><span data-ttu-id="1c0cc-372">0：在加载中查找。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-372">0: Find in load.</span></span><br/><br/><span data-ttu-id="1c0cc-373">1：程序集加载上下文</span><span class="sxs-lookup"><span data-stu-id="1c0cc-373">1: Assembly Load Context</span></span></br><br/><span data-ttu-id="1c0cc-374">2：部署应用程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-374">2: Application assemblies.</span></span><br/><br/><span data-ttu-id="1c0cc-375">3：默认程序集加载上下文回退。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-375">3: Default assembly load context fallback.</span></span> <br/><br/><span data-ttu-id="1c0cc-376">4：解析附属程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-376">4: Resolve satellite assembly.</span></span> <br/><br/><span data-ttu-id="1c0cc-377">5：正在解析程序集加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-377">5: Assembly load context resolving.</span></span><br/><br/><span data-ttu-id="1c0cc-378">6：正在解析 AppDomain 程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-378">6: AppDomain assembly resolving.</span></span>
|`AssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-379">程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-379">Load context of the assembly.</span></span>|
|`Result`|`win:UInt16`|<span data-ttu-id="1c0cc-380">解析尝试的结果。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-380">The result of resolution attempt.</span></span><br/><br/><span data-ttu-id="1c0cc-381">0：成功</span><span class="sxs-lookup"><span data-stu-id="1c0cc-381">0: Success</span></span><br/><br/><span data-ttu-id="1c0cc-382">1：未找到程序集</span><span class="sxs-lookup"><span data-stu-id="1c0cc-382">1: Assembly NotFound</span></span><br/><br/><span data-ttu-id="1c0cc-383">2：版本不兼容</span><span class="sxs-lookup"><span data-stu-id="1c0cc-383">2: Incompatible Version</span></span><br/><br/><span data-ttu-id="1c0cc-384">3：程序集名称不匹配</span><span class="sxs-lookup"><span data-stu-id="1c0cc-384">3: Mismatched Assembly Name</span></span><br/><br/><span data-ttu-id="1c0cc-385">4：失败</span><span class="sxs-lookup"><span data-stu-id="1c0cc-385">4: Failure</span></span><br/><br/><span data-ttu-id="1c0cc-386">5：异常</span><span class="sxs-lookup"><span data-stu-id="1c0cc-386">5: Exception</span></span>|
|`ResultAssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-387">已解析的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-387">The name of assembly that was resolved.</span></span>|
|`ResultAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-388">从中解析的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-388">The path of the assembly that was resolved from.</span></span>|
|`ErrorMessage`|`win:UnicodeString`|<span data-ttu-id="1c0cc-389">发生异常时的错误消息。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-389">Error message if there is an exception.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-390">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-390">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="assemblyloadcontextresolvinghandlerinvoked-event"></a><span data-ttu-id="1c0cc-391">AssemblyLoadContextResolvingHandlerInvoked 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-391">AssemblyLoadContextResolvingHandlerInvoked event</span></span>

|<span data-ttu-id="1c0cc-392">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-392">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-393">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-393">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-394">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-394">`Binder` (0x4)</span></span>|<span data-ttu-id="1c0cc-395">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-395">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-396">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-396">Event</span></span>|<span data-ttu-id="1c0cc-397">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-397">Event ID</span></span>|<span data-ttu-id="1c0cc-398">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-398">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyLoadContextResolvingHandlerInvoked`|<span data-ttu-id="1c0cc-399">293</span><span class="sxs-lookup"><span data-stu-id="1c0cc-399">293</span></span>|<span data-ttu-id="1c0cc-400">已调用 [AssemblyLoadContext.Resolving](xref:System.Runtime.Loader.AssemblyLoadContext.Resolving) 处理程序。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-400">An [AssemblyLoadContext.Resolving](xref:System.Runtime.Loader.AssemblyLoadContext.Resolving) handler has been invoked.</span></span>|

|<span data-ttu-id="1c0cc-401">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-401">Field name</span></span>|<span data-ttu-id="1c0cc-402">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-402">Data type</span></span>|<span data-ttu-id="1c0cc-403">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-403">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-404">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-404">Name of assembly name.</span></span>|
|`HandlerName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-405">已调用的处理程序的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-405">Name of the handler invoked.</span></span>|
|`AssemblyLoadContext`|`win:UnicodeString`|<span data-ttu-id="1c0cc-406">程序集的加载上下文。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-406">Load context of the assembly.</span></span>|
|`ResultAssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-407">已解析的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-407">The name of assembly that was resolved.</span></span>|
|`ResultAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-408">从中解析的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-408">The path of the assembly that was resolved from.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-409">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-409">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="appdomainassemblyresolvehandlerinvoked-event"></a><span data-ttu-id="1c0cc-410">AppDomainAssemblyResolveHandlerInvoked 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-410">AppDomainAssemblyResolveHandlerInvoked event</span></span>

|<span data-ttu-id="1c0cc-411">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-411">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-412">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-412">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-413">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-413">`Binder` (0x4)</span></span>|<span data-ttu-id="1c0cc-414">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-414">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-415">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-415">Event</span></span>|<span data-ttu-id="1c0cc-416">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-416">Event ID</span></span>|<span data-ttu-id="1c0cc-417">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-417">Description</span></span>|
|-----------|--------------|-----------------|
|`AppDomainAssemblyResolveHandlerInvoked`|<span data-ttu-id="1c0cc-418">294</span><span class="sxs-lookup"><span data-stu-id="1c0cc-418">294</span></span>|<span data-ttu-id="1c0cc-419">已调用 <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> 处理程序。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-419">An <xref:System.AppDomain.AssemblyResolve?displayProperty=nameWithType> handler has been invoked.</span></span>|

|<span data-ttu-id="1c0cc-420">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-420">Field name</span></span>|<span data-ttu-id="1c0cc-421">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-421">Data type</span></span>|<span data-ttu-id="1c0cc-422">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-422">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-423">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-423">Name of assembly name.</span></span>|
|`HandlerName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-424">已调用的处理程序的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-424">Name of the handler invoked.</span></span>|
|`ResultAssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-425">已解析的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-425">The name of assembly that was resolved.</span></span>|
|`ResultAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-426">从中解析的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-426">The path of the assembly that was resolved from.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-427">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-427">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="assemblyloadfromresolvehandlerinvoked-event"></a><span data-ttu-id="1c0cc-428">AssemblyLoadFromResolveHandlerInvoked 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-428">AssemblyLoadFromResolveHandlerInvoked event</span></span>

|<span data-ttu-id="1c0cc-429">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-429">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-430">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-430">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-431">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-431">`Binder` (0x4)</span></span>|<span data-ttu-id="1c0cc-432">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-432">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-433">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-433">Event</span></span>|<span data-ttu-id="1c0cc-434">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-434">Event ID</span></span>|<span data-ttu-id="1c0cc-435">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-435">Description</span></span>|
|-----------|--------------|-----------------|
|`AssemblyLoadFromResolveHandlerInvoked`|<span data-ttu-id="1c0cc-436">295</span><span class="sxs-lookup"><span data-stu-id="1c0cc-436">295</span></span>|<span data-ttu-id="1c0cc-437">已调用 <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> 处理程序。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-437">An <xref:System.Reflection.Assembly.LoadFrom%2A?displayProperty=nameWithType> handler has been invoked.</span></span>|

|<span data-ttu-id="1c0cc-438">字段名</span><span class="sxs-lookup"><span data-stu-id="1c0cc-438">Field name</span></span>|<span data-ttu-id="1c0cc-439">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-439">Data type</span></span>|<span data-ttu-id="1c0cc-440">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-440">Description</span></span>|
|----------------|---------------|-----------------|
|`AssemblyName`|`win:UnicodeString`|<span data-ttu-id="1c0cc-441">程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-441">Name of assembly name.</span></span>|
|`IsTrackedLoad`|`win:Boolean`|<span data-ttu-id="1c0cc-442">是否跟踪程序集加载。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-442">Whether the assembly load is tracked.</span></span>|
|`RequestingAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-443">正在请求的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-443">The path of the requesting assembly.</span></span>|
|`ComputedRequestedAssemblyPath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-444">已请求的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-444">The path of the assembly that was requested.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-445">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-445">Unique ID for the instance of CoreCLR.</span></span>|

## <a name="knownpathprobed-event"></a><span data-ttu-id="1c0cc-446">KnownPathProbed 事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-446">KnownPathProbed event</span></span>

|<span data-ttu-id="1c0cc-447">引发事件的关键字</span><span class="sxs-lookup"><span data-stu-id="1c0cc-447">Keyword for raising the event</span></span>|<span data-ttu-id="1c0cc-448">Level</span><span class="sxs-lookup"><span data-stu-id="1c0cc-448">Level</span></span>|
|-----------------------------------|-----------|-----------|
|<span data-ttu-id="1c0cc-449">`Binder` (0x4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-449">`Binder` (0x4)</span></span>|<span data-ttu-id="1c0cc-450">信息性 (4)</span><span class="sxs-lookup"><span data-stu-id="1c0cc-450">Informational (4)</span></span>|

|<span data-ttu-id="1c0cc-451">事件</span><span class="sxs-lookup"><span data-stu-id="1c0cc-451">Event</span></span>|<span data-ttu-id="1c0cc-452">事件 ID</span><span class="sxs-lookup"><span data-stu-id="1c0cc-452">Event ID</span></span>|<span data-ttu-id="1c0cc-453">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-453">Description</span></span>|
|-----------|--------------|-----------------|
|`KnownPathProbed`|<span data-ttu-id="1c0cc-454">296</span><span class="sxs-lookup"><span data-stu-id="1c0cc-454">296</span></span>|<span data-ttu-id="1c0cc-455">探测到了程序集的一个已知路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-455">A known path was probed for an assembly.</span></span>|

|<span data-ttu-id="1c0cc-456">字段名称</span><span class="sxs-lookup"><span data-stu-id="1c0cc-456">Field Name</span></span>|<span data-ttu-id="1c0cc-457">数据类型</span><span class="sxs-lookup"><span data-stu-id="1c0cc-457">Data Type</span></span>|<span data-ttu-id="1c0cc-458">说明</span><span class="sxs-lookup"><span data-stu-id="1c0cc-458">Description</span></span>|
|----------------|---------------|-----------------|
|`FilePath`|`win:UnicodeString`|<span data-ttu-id="1c0cc-459">已探测的路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-459">Path probed.</span></span>|
|`Source`|`win:UInt16`|<span data-ttu-id="1c0cc-460">已探测的路径的源。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-460">Source of the path probed.</span></span><br/><br/><span data-ttu-id="1c0cc-461">0x0：应用程序集。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-461">0x0:Application Assemblies.</span></span><br/><br/><span data-ttu-id="1c0cc-462">0x1：应用本机映像路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-462">0x1:App native image path.</span></span><br/><br/><span data-ttu-id="1c0cc-463">0x2：应用路径。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-463">0x2:App path.</span></span></br><br/><span data-ttu-id="1c0cc-464">0x3：平台资源根。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-464">0x3:Platform resource roots.</span></span><br/><br/><span data-ttu-id="1c0cc-465">0x4：附属子目录。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-465">0x4:Satellite Subdirectory.</span></span></br>|
|`Result`|`win:UInt32`|<span data-ttu-id="1c0cc-466">探测的 HRESULT。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-466">HRESULT for the probe.</span></span>|
|`ClrInstanceID`|`win:UInt16`|<span data-ttu-id="1c0cc-467">CoreCLR 实例的唯一 ID。</span><span class="sxs-lookup"><span data-stu-id="1c0cc-467">Unique ID for the instance of CoreCLR.</span></span>|
