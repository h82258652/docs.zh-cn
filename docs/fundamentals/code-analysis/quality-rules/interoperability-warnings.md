---
title: 可移植性和互操作性规则（代码分析）
description: 了解代码分析规则“可移植性和互操作性规则”
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.Portablityrules
- vs.codeanalysis.Interoperabilityrules
helpviewer_keywords:
- managed code analysis rules, interoperability rules, portability rules
- portability rules
- warnings, portability
- interoperability rules
- warnings, interoperability
author: gewarren
ms.author: gewarren
ms.openlocfilehash: a20cd77e13c4a8b95633d129990667f0a8de3ee8
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590173"
---
# <a name="portability-and-interoperability-rules"></a><span data-ttu-id="bd590-103">可移植性和互操作性规则</span><span class="sxs-lookup"><span data-stu-id="bd590-103">Portability and interoperability rules</span></span>

<span data-ttu-id="bd590-104">可移植性规则支持跨不同平台的可移植性。</span><span class="sxs-lookup"><span data-stu-id="bd590-104">Portability rules support portability across different platforms.</span></span> <span data-ttu-id="bd590-105">互操作性规则支持与 COM 客户端交互。</span><span class="sxs-lookup"><span data-stu-id="bd590-105">Interoperability rules support interaction with COM clients.</span></span>

## <a name="in-this-section"></a><span data-ttu-id="bd590-106">在本节中</span><span class="sxs-lookup"><span data-stu-id="bd590-106">In this section</span></span>

| <span data-ttu-id="bd590-107">规则</span><span class="sxs-lookup"><span data-stu-id="bd590-107">Rule</span></span> | <span data-ttu-id="bd590-108">描述</span><span class="sxs-lookup"><span data-stu-id="bd590-108">Description</span></span> |
| - | - |
| [<span data-ttu-id="bd590-109">CA1401：P/Invokes 应为不可见</span><span class="sxs-lookup"><span data-stu-id="bd590-109">CA1401: P/Invokes should not be visible</span></span>](ca1401.md) | <span data-ttu-id="bd590-110">公共类型中的公共或受保护方法具有 System.Runtime.InteropServices.DllImportAttribute 属性（在 Visual Basic 中由 Declare 关键字实现）。</span><span class="sxs-lookup"><span data-stu-id="bd590-110">A public or protected method in a public type has the System.Runtime.InteropServices.DllImportAttribute attribute (also implemented by the Declare keyword in Visual Basic).</span></span> <span data-ttu-id="bd590-111">这些方法不能公开。</span><span class="sxs-lookup"><span data-stu-id="bd590-111">Such methods should not be exposed.</span></span> |
| [<span data-ttu-id="bd590-112">CA1416：验证平台兼容性</span><span class="sxs-lookup"><span data-stu-id="bd590-112">CA1416: Validate platform compatibility</span></span>](ca1416.md) | <span data-ttu-id="bd590-113">在组件上使用依赖于平台的 API 会使代码无法用于所有平台。</span><span class="sxs-lookup"><span data-stu-id="bd590-113">Using platform-dependent APIs on a component makes the code no longer work across all platforms.</span></span> |
| [<span data-ttu-id="bd590-114">CA1417：请勿对 P/Invokes 的字符串参数使用 `OutAttribute`</span><span class="sxs-lookup"><span data-stu-id="bd590-114">CA1417: Do not use `OutAttribute` on string parameters for P/Invokes</span></span>](ca1417.md) | <span data-ttu-id="bd590-115">如果该字符串为暂存的字符串，则通过包含 `OutAttribute` 的值传递的字符串参数可能使运行时变得不稳定。</span><span class="sxs-lookup"><span data-stu-id="bd590-115">String parameters passed by value with the `OutAttribute` can destabilize the runtime if the string is an interned string.</span></span> |
