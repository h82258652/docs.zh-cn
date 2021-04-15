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
# <a name="corpinvokemap-enumeration"></a>CorPinvokeMap 枚举

指定 PInvoke 调用的选项。  
  
## <a name="syntax"></a>语法  
  
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
  
## <a name="members"></a>成员  
  
|成员|说明|  
|------------|-----------------|  
|`pmNoMangle`|按指定使用每个成员名称。|  
|`pmCharSetMask`|保留。|  
|`pmCharSetNotSpec`|保留。|  
|`pmCharSetAnsi`|以多字节字符串的形式封送字符串。|  
|`pmCharSetUnicode`|以 Unicode 2 字节字符的形式封送字符串。|  
|`pmCharSetAuto`|针对目标操作系统适当地自动封送字符串。 默认值为 Windows 上的 Unicode。|  
|`pmBestFitUseAssem`|保留。|  
|`pmBestFitEnabled`|在 ANSI 字符集中执行与缺少完全匹配的 Unicode 字符的最佳映射。|  
|`pmBestFitDisabled`|不要执行 Unicode 字符的最佳映射。 在这种情况下，所有无法映射的字符都将替换为 "？"。|  
|`pmBestFitMask`|保留。|  
|`pmThrowOnUnmappableCharUseAssem`|保留。|  
|`pmThrowOnUnmappableCharEnabled`|当互操作封送拆收器遇到无法映射的字符时引发异常。|  
|`pmThrowOnUnmappableCharDisabled`|在互操作封送拆收器遇到无法映射的字符时不引发异常。|  
|`pmThrowOnUnmappableCharMask`|保留|  
|`pmSupportsLastError`|允许被调用方在 `SetLastError` 从属性化方法返回之前调用 Win32 函数。|  
|`pmCallConvMask`|保留|  
|`pmCallConvWinapi`|使用默认平台调用约定。 例如，在 Windows 上，默认值为 `StdCall` ，在 Windows CE .net 上是 `Cdecl` 。|  
|`pmCallConvCdecl`|使用 `Cdecl` 调用约定。 在这种情况下，调用方会清理堆栈。 这样就可以使用 (调用函数 `varargs` ，即接受数目可变的参数的函数) 。|  
|`pmCallConvStdcall`|使用 `StdCall` 调用约定。 在这种情况下，被调用方清理堆栈。 这是使用平台 invoke 调用非托管函数的默认约定。|  
|`pmCallConvThiscall`|使用 `ThisCall` 调用约定。 在这种情况下，第一个参数是 `this` 指针，它存储在寄存器 ECX 中。 其他参数被推送到堆栈上。 `ThisCall`调用约定用于对从非托管 DLL 导出的类调用方法。|  
|`pmCallConvFastcall`|保留。|  
|`pmMaxValue`|保留。|  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Corhdr。h  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据枚举](metadata-enumerations.md)
