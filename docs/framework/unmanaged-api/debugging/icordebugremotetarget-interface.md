---
description: 了解详细信息： ICorDebugRemoteTarget 接口
title: ICorDebugRemoteTarget 接口
ms.date: 03/30/2017
api_name:
- ICorDebugRemoteTarget
api_location:
- CorDebug.dll
api_type:
- COM
f1_keywords:
- ICorDebugRemoteTarget
helpviewer_keywords:
- ICorDebugRemoteTarget interface [.NET Framework debugging]
ms.assetid: bd9936a6-cc24-4869-8761-0988664464e6
topic_type:
- apiref
ms.openlocfilehash: a14dc9d94d83a662d996ebb027e2e6ca3ef62c03
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494046"
---
# <a name="icordebugremotetarget-interface"></a><span data-ttu-id="83cd1-103">ICorDebugRemoteTarget 接口</span><span class="sxs-lookup"><span data-stu-id="83cd1-103">ICorDebugRemoteTarget Interface</span></span>

<span data-ttu-id="83cd1-104">介绍能够让开发人员在公共语言运行时 (CLR) 环境中调试基于 Silverlight 的应用程序的方法。</span><span class="sxs-lookup"><span data-stu-id="83cd1-104">Provides methods that enable developers to debug Silverlight-based applications in the common language runtime (CLR) environment.</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="83cd1-105">语法</span><span class="sxs-lookup"><span data-stu-id="83cd1-105">Syntax</span></span>  
  
```cpp  
interface ICorDebugRemoteTarget  : IUnknown  
{  
    HRESULT GetHostName (  
        [in]  ULONG32                    cchHostName,  
        [out] ULONG32*                   pcchHostName,  
        [out, size_is(cchHostName),  
              length_is(*pcchHostName)]  
                  WCHAR szHostName[]  
        );  
};  
```  
  
## <a name="methods"></a><span data-ttu-id="83cd1-106">方法</span><span class="sxs-lookup"><span data-stu-id="83cd1-106">Methods</span></span>  
  
|<span data-ttu-id="83cd1-107">方法</span><span class="sxs-lookup"><span data-stu-id="83cd1-107">Method</span></span>|<span data-ttu-id="83cd1-108">描述</span><span class="sxs-lookup"><span data-stu-id="83cd1-108">Description</span></span>|  
|------------|-----------------|  
|[<span data-ttu-id="83cd1-109">ICorDebugRemoteTarget::GetHostName 方法</span><span class="sxs-lookup"><span data-stu-id="83cd1-109">ICorDebugRemoteTarget::GetHostName Method</span></span>](icordebugremotetarget-gethostname-method.md)|<span data-ttu-id="83cd1-110">返回远程计算机的主机名或 IP 地址。</span><span class="sxs-lookup"><span data-stu-id="83cd1-110">Returns the host name or the IP address of a remote machine.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="83cd1-111">注解</span><span class="sxs-lookup"><span data-stu-id="83cd1-111">Remarks</span></span>  

 <span data-ttu-id="83cd1-112">混合模式 (即，非 x86 (平台（如 IA-64 和 AMD64) ）不支持) 调试的托管代码和本机代码。</span><span class="sxs-lookup"><span data-stu-id="83cd1-112">Mixed-mode (that is, managed and native code) debugging is not supported on non-x86 platforms (such as IA-64 and AMD64).</span></span>  
  
## <a name="requirements"></a><span data-ttu-id="83cd1-113">要求</span><span class="sxs-lookup"><span data-stu-id="83cd1-113">Requirements</span></span>  

 <span data-ttu-id="83cd1-114">**平台：** 请参阅 [系统要求](../../get-started/system-requirements.md)。</span><span class="sxs-lookup"><span data-stu-id="83cd1-114">**Platforms:** See [System Requirements](../../get-started/system-requirements.md).</span></span>  
  
 <span data-ttu-id="83cd1-115">**标头：** Cordebug.idl .idl</span><span class="sxs-lookup"><span data-stu-id="83cd1-115">**Header:** CorDebug.idl</span></span>  
  
 <span data-ttu-id="83cd1-116">**库：** ： corguids.lib</span><span class="sxs-lookup"><span data-stu-id="83cd1-116">**Library:** : CorGuids.lib</span></span>  
  
 <span data-ttu-id="83cd1-117">**.NET Framework 版本：** 3.5 SP1</span><span class="sxs-lookup"><span data-stu-id="83cd1-117">**.NET Framework Versions:** 3.5 SP1</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="83cd1-118">另请参阅</span><span class="sxs-lookup"><span data-stu-id="83cd1-118">See also</span></span>

- [<span data-ttu-id="83cd1-119">ICorDebugRemote 接口</span><span class="sxs-lookup"><span data-stu-id="83cd1-119">ICorDebugRemote Interface</span></span>](icordebugremote-interface.md)
- [<span data-ttu-id="83cd1-120">ICorDebug 接口</span><span class="sxs-lookup"><span data-stu-id="83cd1-120">ICorDebug Interface</span></span>](icordebug-interface.md)
- [<span data-ttu-id="83cd1-121">调试接口</span><span class="sxs-lookup"><span data-stu-id="83cd1-121">Debugging Interfaces</span></span>](debugging-interfaces.md)
