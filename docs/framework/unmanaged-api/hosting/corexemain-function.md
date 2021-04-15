---
description: 了解详细信息： _CorExeMain 函数
title: _CorExeMain 函数
ms.date: 03/30/2017
api_name:
- _CorExeMain
api_location:
- mscoree.dll
- clr.dll
- mscorwks.dll
- mscoreei.dll
api_type:
- DLLExport
f1_keywords:
- _CorExeMain
helpviewer_keywords:
- _CorExeMain function [.NET Framework hosting]
ms.assetid: 898f76e2-16f4-4a63-b7d9-dad2d3824d8a
topic_type:
- apiref
ms.openlocfilehash: e687cae4d267b97d1b9eb35be1ad5aabd8aec30b
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494111"
---
# <a name="_corexemain-function"></a>_CorExeMain 函数

 (CLR) 初始化公共语言运行时，找到可执行程序集的 CLR 标头中的托管入口点，然后开始执行。  
  
## <a name="syntax"></a>语法  
  
```cpp  
__int32 STDMETHODCALLTYPE _CorExeMain ();  
```  
  
## <a name="remarks"></a>备注  

 此函数由加载程序在从托管可执行程序集创建的进程中调用。 对于 DLL 程序集，加载程序将改为调用 [_CorDllMain](cordllmain-function.md) 函数。  
  
 操作系统加载程序将调用此方法，而不考虑映像文件中指定的入口点。
  
 有关其他信息，请参阅 [_CorValidateImage](corvalidateimage-function.md) 文章中的 "备注" 部分。  
  
## <a name="requirements"></a>要求  

 **平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。  
  
 **标头：** Cor  
  
 **库：** 作为中的资源包含 MsCorEE.dll  
  
 **.NET Framework 版本：**[!INCLUDE[net_current_v10plus](../../../../includes/net-current-v10plus-md.md)]  
  
## <a name="see-also"></a>另请参阅

- [元数据全局静态函数](../metadata/metadata-global-static-functions.md)
