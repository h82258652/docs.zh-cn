---
description: 了解详细信息： CorPinvokeMap 枚举
title: CorPinvokeMap 枚举
ms.date: 03/30/2017
api_name:
- CorPinvokeMap
api_location:
- mscoree.dll
api_type:
- COM
f1_keywords:
- CorPinvokeMap
helpviewer_keywords:
- CorPinvokeMap enumeration [.NET Framework metadata]
ms.assetid: f14f986e-f6ce-42bc-aa23-18150c46d28c
topic_type:
- apiref
ms.openlocfilehash: e054b7e9c5a8baf0e59fc3c9ce9bc57a4d7ea4d0
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494579"
---
# <a name="corpinvokemap-enumeration"></a><span data-ttu-id="52a7c-103">CorPinvokeMap 枚举</span><span class="sxs-lookup"><span data-stu-id="52a7c-103">CorPinvokeMap Enumeration</span></span>

<span data-ttu-id="52a7c-104">指定 PInvoke 调用的选项。</span><span class="sxs-lookup"><span data-stu-id="52a7c-104">Specifies options for a PInvoke call.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="52a7c-105">语法</span><span class="sxs-lookup"><span data-stu-id="52a7c-105">Syntax</span></span>  
  
```cpp  
typedef enum  CorPinvokeMap {  
  
    pmNoMangle          = 0x0001,  
  
    pmCharSetMask       = 0x0006,  
    pmCharSetNotSpec    = 0x0000,  
    pmCharSetAnsi       = 0x0002,  
    pmCharSetUnicode    = 0x0004,  
    pmCharSetAuto       = 0x0006,  
  
    pmBestFitUseAssem   = 0x0000,  
    pmBestFitEnabled    = 0x0010,  
    pmBestFitDisabled   = 0x0020,  
    pmBestFitMask       = 0x0030,  
  
    pmThrowOnUnmappableCharUseAssem   = 0x0000,  
    pmThrowOnUnmappableCharEnabled    = 0x1000,  
    pmThrowOnUnmappableCharDisabled   = 0x2000,  
    pmThrowOnUnmappableCharMask       = 0x3000,  
  
    pmSupportsLastError = 0x0040,
  
    pmCallConvMask      = 0x0700,  
    pmCallConvWinapi    = 0x0100,  
    pmCallConvCdecl     = 0x0200,  
    pmCallConvStdcall   = 0x0300,  
    pmCallConvThiscall  = 0x0400,  
    pmCallConvFastcall  = 0x0500,  
  
    pmMaxValue          = 0xFFFF  
  
} CorPinvokeMap;  
```  
  
## <a name="members"></a><span data-ttu-id="52a7c-106">成员</span><span class="sxs-lookup"><span data-stu-id="52a7c-106">Members</span></span>  
  
|<span data-ttu-id="52a7c-107">成员</span><span class="sxs-lookup"><span data-stu-id="52a7c-107">Member</span></span>|<span data-ttu-id="52a7c-108">说明</span><span class="sxs-lookup"><span data-stu-id="52a7c-108">Description</span></span>|  
|------------|-----------------|  
|`pmNoMangle`|<span data-ttu-id="52a7c-109">按指定使用每个成员名称。</span><span class="sxs-lookup"><span data-stu-id="52a7c-109">Use each member name as specified.</span></span>|  
|`pmCharSetMask`|<span data-ttu-id="52a7c-110">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-110">Reserved.</span></span>|  
|`pmCharSetNotSpec`|<span data-ttu-id="52a7c-111">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-111">Reserved.</span></span>|  
|`pmCharSetAnsi`|<span data-ttu-id="52a7c-112">以多字节字符串的形式封送字符串。</span><span class="sxs-lookup"><span data-stu-id="52a7c-112">Marshal strings as multiple-byte character strings.</span></span>|  
|`pmCharSetUnicode`|<span data-ttu-id="52a7c-113">以 Unicode 2 字节字符的形式封送字符串。</span><span class="sxs-lookup"><span data-stu-id="52a7c-113">Marshal strings as Unicode 2-byte characters.</span></span>|  
|`pmCharSetAuto`|<span data-ttu-id="52a7c-114">针对目标操作系统适当地自动封送字符串。</span><span class="sxs-lookup"><span data-stu-id="52a7c-114">Automatically marshal strings appropriately for the target operating system.</span></span> <span data-ttu-id="52a7c-115">默认值为 Windows 上的 Unicode。</span><span class="sxs-lookup"><span data-stu-id="52a7c-115">The default is Unicode on Windows.</span></span>|  
|`pmBestFitUseAssem`|<span data-ttu-id="52a7c-116">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-116">Reserved.</span></span>|  
|`pmBestFitEnabled`|<span data-ttu-id="52a7c-117">在 ANSI 字符集中执行与缺少完全匹配的 Unicode 字符的最佳映射。</span><span class="sxs-lookup"><span data-stu-id="52a7c-117">Perform best-fit mapping of Unicode characters that lack an exact match in the ANSI character set.</span></span>|  
|`pmBestFitDisabled`|<span data-ttu-id="52a7c-118">不要执行 Unicode 字符的最佳映射。</span><span class="sxs-lookup"><span data-stu-id="52a7c-118">Do not perform best-fit mapping of Unicode characters.</span></span> <span data-ttu-id="52a7c-119">在这种情况下，所有无法映射的字符都将替换为 "？"。</span><span class="sxs-lookup"><span data-stu-id="52a7c-119">In this case, all unmappable characters will be replaced by a ‘?’.</span></span>|  
|`pmBestFitMask`|<span data-ttu-id="52a7c-120">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-120">Reserved.</span></span>|  
|`pmThrowOnUnmappableCharUseAssem`|<span data-ttu-id="52a7c-121">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-121">Reserved.</span></span>|  
|`pmThrowOnUnmappableCharEnabled`|<span data-ttu-id="52a7c-122">当互操作封送拆收器遇到无法映射的字符时引发异常。</span><span class="sxs-lookup"><span data-stu-id="52a7c-122">Throw an exception when the interop marshaler encounters an unmappable character.</span></span>|  
|`pmThrowOnUnmappableCharDisabled`|<span data-ttu-id="52a7c-123">在互操作封送拆收器遇到无法映射的字符时不引发异常。</span><span class="sxs-lookup"><span data-stu-id="52a7c-123">Do not throw an exception when the interop marshaler encounters an unmappable character.</span></span>|  
|`pmThrowOnUnmappableCharMask`|<span data-ttu-id="52a7c-124">保留</span><span class="sxs-lookup"><span data-stu-id="52a7c-124">Reserved</span></span>|  
|`pmSupportsLastError`|<span data-ttu-id="52a7c-125">允许被调用方在 `SetLastError` 从属性化方法返回之前调用 Win32 函数。</span><span class="sxs-lookup"><span data-stu-id="52a7c-125">Allow the callee to call the Win32 `SetLastError` function before returning from the attributed method.</span></span>|  
|`pmCallConvMask`|<span data-ttu-id="52a7c-126">保留</span><span class="sxs-lookup"><span data-stu-id="52a7c-126">Reserved</span></span>|  
|`pmCallConvWinapi`|<span data-ttu-id="52a7c-127">使用默认平台调用约定。</span><span class="sxs-lookup"><span data-stu-id="52a7c-127">Use the default platform calling convention.</span></span> <span data-ttu-id="52a7c-128">例如，在 Windows 上，默认值为 `StdCall` ，在 Windows CE .net 上是 `Cdecl` 。</span><span class="sxs-lookup"><span data-stu-id="52a7c-128">For example, on Windows the default is `StdCall` and on Windows CE .NET it is `Cdecl`.</span></span>|  
|`pmCallConvCdecl`|<span data-ttu-id="52a7c-129">使用 `Cdecl` 调用约定。</span><span class="sxs-lookup"><span data-stu-id="52a7c-129">Use the `Cdecl` calling convention.</span></span> <span data-ttu-id="52a7c-130">在这种情况下，调用方会清理堆栈。</span><span class="sxs-lookup"><span data-stu-id="52a7c-130">In this case, the caller cleans the stack.</span></span> <span data-ttu-id="52a7c-131">这样就可以使用 (调用函数 `varargs` ，即接受数目可变的参数的函数) 。</span><span class="sxs-lookup"><span data-stu-id="52a7c-131">This enables calling functions with `varargs` (that is, functions that accept a variable number of parameters).</span></span>|  
|`pmCallConvStdcall`|<span data-ttu-id="52a7c-132">使用 `StdCall` 调用约定。</span><span class="sxs-lookup"><span data-stu-id="52a7c-132">Use the `StdCall` calling convention.</span></span> <span data-ttu-id="52a7c-133">在这种情况下，被调用方清理堆栈。</span><span class="sxs-lookup"><span data-stu-id="52a7c-133">In this case, the callee cleans the stack.</span></span> <span data-ttu-id="52a7c-134">这是使用平台 invoke 调用非托管函数的默认约定。</span><span class="sxs-lookup"><span data-stu-id="52a7c-134">This is the default convention for calling unmanaged functions with platform invoke.</span></span>|  
|`pmCallConvThiscall`|<span data-ttu-id="52a7c-135">使用 `ThisCall` 调用约定。</span><span class="sxs-lookup"><span data-stu-id="52a7c-135">Use the `ThisCall` calling convention.</span></span> <span data-ttu-id="52a7c-136">在这种情况下，第一个参数是 `this` 指针，它存储在寄存器 ECX 中。</span><span class="sxs-lookup"><span data-stu-id="52a7c-136">In this case, the first parameter is the `this` pointer and is stored in register ECX.</span></span> <span data-ttu-id="52a7c-137">其他参数被推送到堆栈上。</span><span class="sxs-lookup"><span data-stu-id="52a7c-137">Other parameters are pushed on the stack.</span></span> <span data-ttu-id="52a7c-138">`ThisCall`调用约定用于对从非托管 DLL 导出的类调用方法。</span><span class="sxs-lookup"><span data-stu-id="52a7c-138">The `ThisCall` calling convention is used to call methods on classes exported from an unmanaged DLL.</span></span>|  
|`pmCallConvFastcall`|<span data-ttu-id="52a7c-139">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-139">Reserved.</span></span>|  
|`pmMaxValue`|<span data-ttu-id="52a7c-140">保留。</span><span class="sxs-lookup"><span data-stu-id="52a7c-140">Reserved.</span></span>|  
  
## <a name="requirements"></a><span data-ttu-id="52a7c-141">要求</span><span class="sxs-lookup"><span data-stu-id="52a7c-141">Requirements</span></span>  

 <span data-ttu-id="52a7c-142">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="52a7c-142">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="52a7c-143">**标头：** Corhdr。h</span><span class="sxs-lookup"><span data-stu-id="52a7c-143">**Header:** CorHdr.h</span></span>  
  
 <span data-ttu-id="52a7c-144">**.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span><span class="sxs-lookup"><span data-stu-id="52a7c-144">**.NET Framework Versions:** [!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="52a7c-145">另请参阅</span><span class="sxs-lookup"><span data-stu-id="52a7c-145">See also</span></span>

- [<span data-ttu-id="52a7c-146">元数据枚举</span><span class="sxs-lookup"><span data-stu-id="52a7c-146">Metadata Enumerations</span></span>](metadata-enumerations.md)
