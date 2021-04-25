---
title: .NET 库中断性变更
description: 列出 .NET Core 1.0-3.0 版核心 .NET 库中的中断性变更。
ms.date: 07/27/2020
ms.openlocfilehash: 99c72b5ff36d0c0cb821d50b1ec04fdef56189da
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494358"
---
# <a name="core-net-libraries-breaking-changes-in-net-core-10-30"></a><span data-ttu-id="cad4d-103">.NET Core 1.0-3.0 中的核心 .NET 库中断性变更</span><span class="sxs-lookup"><span data-stu-id="cad4d-103">Core .NET libraries breaking changes in .NET Core 1.0-3.0</span></span>

<span data-ttu-id="cad4d-104">核心 .NET 库提供了 .NET Core 使用的基元和其他常规类型。</span><span class="sxs-lookup"><span data-stu-id="cad4d-104">The core .NET libraries provide the primitives and other general types used by .NET Core.</span></span>

<span data-ttu-id="cad4d-105">本页记录了以下中断性变更：</span><span class="sxs-lookup"><span data-stu-id="cad4d-105">The following breaking changes are documented on this page:</span></span>

| <span data-ttu-id="cad4d-106">重大更改</span><span class="sxs-lookup"><span data-stu-id="cad4d-106">Breaking change</span></span> | <span data-ttu-id="cad4d-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="cad4d-107">Version introduced</span></span> |
| - | :-: |
| [<span data-ttu-id="cad4d-108">将 GroupCollection 传递到采用 IEnumerable\<T> 的扩展方法需要消除歧义</span><span class="sxs-lookup"><span data-stu-id="cad4d-108">Passing GroupCollection to extension methods taking IEnumerable\<T> requires disambiguation</span></span>](#passing-groupcollection-to-extension-methods-taking-ienumerablet-requires-disambiguation) | <span data-ttu-id="cad4d-109">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-109">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-110">报告版本的 API 现在报告产品版本而不是文件版本</span><span class="sxs-lookup"><span data-stu-id="cad4d-110">APIs that report version now report product and not file version</span></span>](#apis-that-report-version-now-report-product-and-not-file-version) | <span data-ttu-id="cad4d-111">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-111">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-112">自定义 EncoderFallbackBuffer 实例无法递归回退</span><span class="sxs-lookup"><span data-stu-id="cad4d-112">Custom EncoderFallbackBuffer instances cannot fall back recursively</span></span>](#custom-encoderfallbackbuffer-instances-cannot-fall-back-recursively) | <span data-ttu-id="cad4d-113">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-113">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-114">浮点格式设置和分析行为变更</span><span class="sxs-lookup"><span data-stu-id="cad4d-114">Floating point formatting and parsing behavior changes</span></span>](#floating-point-formatting-and-parsing-behavior-changed) | <span data-ttu-id="cad4d-115">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-115">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-116">浮点分析操作不再失败或引发 OverflowException</span><span class="sxs-lookup"><span data-stu-id="cad4d-116">Floating-point parsing operations no longer fail or throw an OverflowException</span></span>](#floating-point-parsing-operations-no-longer-fail-or-throw-an-overflowexception) | <span data-ttu-id="cad4d-117">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-117">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-118">InvalidAsynchronousStateException 已移到另一个程序集</span><span class="sxs-lookup"><span data-stu-id="cad4d-118">InvalidAsynchronousStateException moved to another assembly</span></span>](#invalidasynchronousstateexception-moved-to-another-assembly) | <span data-ttu-id="cad4d-119">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-119">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-120">替换格式错误的 UTF-8 字节序列将遵循 Unicode 准则</span><span class="sxs-lookup"><span data-stu-id="cad4d-120">Replacing ill-formed UTF-8 byte sequences follows Unicode guidelines</span></span>](#replacing-ill-formed-utf-8-byte-sequences-follows-unicode-guidelines) | <span data-ttu-id="cad4d-121">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-121">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-122">TypeDescriptionProviderAttribute 已移到另一个程序集</span><span class="sxs-lookup"><span data-stu-id="cad4d-122">TypeDescriptionProviderAttribute moved to another assembly</span></span>](#typedescriptionproviderattribute-moved-to-another-assembly) | <span data-ttu-id="cad4d-123">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-123">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-124">ZipArchiveEntry 不再处理条目大小不一致的存档</span><span class="sxs-lookup"><span data-stu-id="cad4d-124">ZipArchiveEntry no longer handles archives with inconsistent entry sizes</span></span>](#ziparchiveentry-no-longer-handles-archives-with-inconsistent-entry-sizes) | <span data-ttu-id="cad4d-125">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-125">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-126">FieldInfo.SetValue 将对静态、仅初始化字段引发异常</span><span class="sxs-lookup"><span data-stu-id="cad4d-126">FieldInfo.SetValue throws exception for static, init-only fields</span></span>](#fieldinfosetvalue-throws-exception-for-static-init-only-fields) | <span data-ttu-id="cad4d-127">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-127">.NET Core 3.0</span></span> |
| [<span data-ttu-id="cad4d-128">路径 API 对无效字符不引发异常</span><span class="sxs-lookup"><span data-stu-id="cad4d-128">Path APIs don't throw an exception for invalid characters</span></span>](#path-apis-dont-throw-an-exception-for-invalid-characters) | <span data-ttu-id="cad4d-129">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cad4d-129">.NET Core 2.1</span></span> |
| [<span data-ttu-id="cad4d-130">添加到内置结构类型的私有字段</span><span class="sxs-lookup"><span data-stu-id="cad4d-130">Private fields added to built-in struct types</span></span>](#private-fields-added-to-built-in-struct-types) | <span data-ttu-id="cad4d-131">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cad4d-131">.NET Core 2.1</span></span> |
| [<span data-ttu-id="cad4d-132">UseShellExecute 默认值更改</span><span class="sxs-lookup"><span data-stu-id="cad4d-132">Change in default value of UseShellExecute</span></span>](#change-in-default-value-of-useshellexecute) | <span data-ttu-id="cad4d-133">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cad4d-133">.NET Core 2.1</span></span> |
| [<span data-ttu-id="cad4d-134">macOS 上的 OpenSSL 版本</span><span class="sxs-lookup"><span data-stu-id="cad4d-134">OpenSSL versions on macOS</span></span>](#openssl-versions-on-macos) | <span data-ttu-id="cad4d-135">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cad4d-135">.NET Core 2.1</span></span> |
| [<span data-ttu-id="cad4d-136">FileSystemInfo.Attributes 引发的 UnauthorizedAccessException</span><span class="sxs-lookup"><span data-stu-id="cad4d-136">UnauthorizedAccessException thrown by FileSystemInfo.Attributes</span></span>](#unauthorizedaccessexception-thrown-by-filesysteminfoattributes) | <span data-ttu-id="cad4d-137">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-137">.NET Core 1.0</span></span> |
| [<span data-ttu-id="cad4d-138">不支持处理损坏的进程状态异常</span><span class="sxs-lookup"><span data-stu-id="cad4d-138">Handling corrupted-process-state exceptions is not supported</span></span>](#handling-corrupted-state-exceptions-is-not-supported) | <span data-ttu-id="cad4d-139">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-139">.NET Core 1.0</span></span> |
| [<span data-ttu-id="cad4d-140">UriBuilder 属性不再预置前导字符</span><span class="sxs-lookup"><span data-stu-id="cad4d-140">UriBuilder properties no longer prepend leading characters</span></span>](#uribuilder-properties-no-longer-prepend-leading-characters) | <span data-ttu-id="cad4d-141">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-141">.NET Core 1.0</span></span> |
| [<span data-ttu-id="cad4d-142">Process.StartInfo 对未启动的进程引发 InvalidOperationException</span><span class="sxs-lookup"><span data-stu-id="cad4d-142">Process.StartInfo throws InvalidOperationException for processes you didn't start</span></span>](#processstartinfo-throws-invalidoperationexception-for-processes-you-didnt-start) | <span data-ttu-id="cad4d-143">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-143">.NET Core 1.0</span></span> |

## <a name="net-core-30"></a><span data-ttu-id="cad4d-144">.NET Core 3.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-144">.NET Core 3.0</span></span>

[!INCLUDE [disambiguate-generic-type-for-groupcollection](../../../includes/core-changes/corefx/3.0/disambiguate-generic-type-for-groupcollection.md)]

***

[!INCLUDE[APIs that report version now report product and not file version](~/includes/core-changes/corefx/3.0/version-information-changes.md)]

***

[!INCLUDE[Custom EncoderFallbackBuffer instances cannot fall back recursively](~/includes/core-changes/corefx/3.0/custom-encoderfallbackbuffer-cannot-be-recursive.md)]

***

[!INCLUDE[Floating point formatting and parsing behavior changes](~/includes/core-changes/corefx/3.0/floating-point-changes.md)]

***

[!INCLUDE[Floating-point parsing operations no longer fail or throw an OverflowException](~/includes/core-changes/corefx/3.0/floating-point-parsing-does-not-overflow.md)]

***

[!INCLUDE[InvalidAsynchronousStateException moved to another assembly](~/includes/core-changes/corefx/3.0/move-invalidasynchronousstateexception.md)]

***

[!INCLUDE[NET Core 3.0 follows Unicode best practices when replacing ill-formed UTF-8 byte sequences](~/includes/core-changes/corefx/3.0/net-core-3-0-follows-unicode-utf8-best-practices.md)]

***

[!INCLUDE[TypeDescriptionProviderAttribute moved to another assembly](~/includes/core-changes/corefx/3.0/move-typedescriptionproviderattribute.md)]

***

[!INCLUDE[ZipArchiveEntry no longer handles archives with inconsistent entry sizes](~/includes/core-changes/corefx/3.0/ziparchiveentry-and-inconsistent-entry-sizes.md)]

***

[!INCLUDE [FieldInfo.SetValue throws exception for static, init-only fields](~/includes/core-changes/corefx/3.0/fieldinfo-setvalue-exception.md)]

***

## <a name="net-core-21"></a><span data-ttu-id="cad4d-145">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="cad4d-145">.NET Core 2.1</span></span>

[!INCLUDE [path-apis-dont-throw-exception-for-invalid-paths](../../../includes/core-changes/corefx/2.1/path-apis-dont-throw-exception-for-invalid-paths.md)]

***

[!INCLUDE[Private fields added to built-in struct types](~/includes/core-changes/corefx/2.1/instantiate-struct.md)]

***

[!INCLUDE[Change in default value of UseShellExecute](~/includes/core-changes/corefx/2.1/process-start-changes.md)]

***

[!INCLUDE [OpenSSL versions on macOS](../../../includes/core-changes/corefx/openssl-dependencies-macos.md)]

***

## <a name="net-core-10"></a><span data-ttu-id="cad4d-146">.NET Core 1.0</span><span class="sxs-lookup"><span data-stu-id="cad4d-146">.NET Core 1.0</span></span>

[!INCLUDE [UnauthorizedAccessException thrown by FileSystemInfo.Attributes](~/includes/core-changes/corefx/1.0/filesysteminfo-attributes-exceptions.md)]

***

[!INCLUDE [corrupted-state-exceptions](~/includes/core-changes/corefx/1.0/corrupted-state-exceptions.md)]

***

[!INCLUDE [uribuilder-behavior-changes](../../../includes/core-changes/corefx/1.0/uribuilder-behavior-changes.md)]

***

[!INCLUDE [startinfo-throws-exception](../../../includes/core-changes/corefx/1.0/startinfo-throws-exception.md)]

***
