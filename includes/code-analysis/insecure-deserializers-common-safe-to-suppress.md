---
author: dotpaul
ms.author: paulming
ms.date: 04/17/2019
ms.topic: include
ms.openlocfilehash: 2b60e0e713745b1887ff148c5b3ef151584eb21d
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590078"
---
<span data-ttu-id="d62ae-101">在以下情况下，禁止显示此规则的警告是安全的：</span><span class="sxs-lookup"><span data-stu-id="d62ae-101">It's safe to suppress a warning from this rule if:</span></span>

- <span data-ttu-id="d62ae-102">已知输入受到信任。</span><span class="sxs-lookup"><span data-stu-id="d62ae-102">You know the input is trusted.</span></span> <span data-ttu-id="d62ae-103">考虑到应用程序的信任边界和数据流可能会随时间发生变化。</span><span class="sxs-lookup"><span data-stu-id="d62ae-103">Consider that your application's trust boundary and data flows may change over time.</span></span>
- <span data-ttu-id="d62ae-104">已采取了[如何修复冲突](#how-to-fix-violations)的某项预防措施。</span><span class="sxs-lookup"><span data-stu-id="d62ae-104">You've taken one of the precautions in [How to fix violations](#how-to-fix-violations).</span></span>
