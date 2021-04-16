---
title: 不必要的代码规则
description: 了解代码分析不必要的代码规则
ms.date: 09/30/2020
ms.topic: reference
author: gewarren
ms.author: gewarren
dev_langs:
- CSharp
- VB
ms.openlocfilehash: 16c4ba0e4bee2388736bf9813a3e8290d8d4da32
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590995"
---
# <a name="unnecessary-code-rules"></a><span data-ttu-id="e7045-103">不必要的代码规则</span><span class="sxs-lookup"><span data-stu-id="e7045-103">Unnecessary code rules</span></span>

<span data-ttu-id="e7045-104">不必要的代码规则标识代码基础映像中不必要且可以重构或删除的不同部分。</span><span class="sxs-lookup"><span data-stu-id="e7045-104">Unnecessary code rules identify different parts of the code base that are unnecessary and can be refactored or removed.</span></span> <span data-ttu-id="e7045-105">出现不必要的代码表示存在以下一个或多个问题：</span><span class="sxs-lookup"><span data-stu-id="e7045-105">The presence of unnecessary code indicates one of more of the following problems:</span></span>

- <span data-ttu-id="e7045-106">可读性：代码不必降低可读性。</span><span class="sxs-lookup"><span data-stu-id="e7045-106">Readability: Code that is unnecessarily degrading readability.</span></span> <span data-ttu-id="e7045-107">例如，[IDE0001](ide0001.md) 标记不必要的类型名限定。</span><span class="sxs-lookup"><span data-stu-id="e7045-107">For example, [IDE0001](ide0001.md) flags unnecessary type-name qualifications.</span></span>
- <span data-ttu-id="e7045-108">可维护性：不必维护重构后不再使用的代码。</span><span class="sxs-lookup"><span data-stu-id="e7045-108">Maintainability: Code that is no longer used after refactoring and is unnecessarily being maintained.</span></span> <span data-ttu-id="e7045-109">例如，[IDE0051](ide0051.md) 标记未使用的专用字段、属性、事件和方法。</span><span class="sxs-lookup"><span data-stu-id="e7045-109">For example, [IDE0051](ide0051.md) flags unused private fields, properties, events, and methods.</span></span>
- <span data-ttu-id="e7045-110">性能：不必要的计算不会产生副作用，但会导致不必要的性能开销。</span><span class="sxs-lookup"><span data-stu-id="e7045-110">Performance: Unnecessary computation that has no side effects and leads to unnecessary performance overhead.</span></span> <span data-ttu-id="e7045-111">例如，在计算值的表达式不产生副作用的情况下，[IDE0059](ide0059.md) 会标记未使用的值赋值。</span><span class="sxs-lookup"><span data-stu-id="e7045-111">For example, [IDE0059](ide0059.md) flags unused value assignments where the expression to compute a value has no side effects.</span></span>
- <span data-ttu-id="e7045-112">功能性：代码中的功能性问题会导致所需的代码变得多余。</span><span class="sxs-lookup"><span data-stu-id="e7045-112">Functionality: Functional issue in code leading to required code being rendered redundant.</span></span> <span data-ttu-id="e7045-113">例如，在方法意外忽略输入参数的情况下，[IDE0060](ide0060.md) 会标记未使用的参数。</span><span class="sxs-lookup"><span data-stu-id="e7045-113">For example, [IDE0060](ide0060.md) flags unused parameters where the method accidentally ignores an input parameter.</span></span>

<span data-ttu-id="e7045-114">本节中的规则包括以下不必要的代码规则：</span><span class="sxs-lookup"><span data-stu-id="e7045-114">The rules in this section concern the following unnecessary code rules:</span></span>

- [<span data-ttu-id="e7045-115">简化名称 (IDE0001)</span><span class="sxs-lookup"><span data-stu-id="e7045-115">Simplify name (IDE0001)</span></span>](ide0001.md)
- [<span data-ttu-id="e7045-116">简化成员访问 (IDE0002)</span><span class="sxs-lookup"><span data-stu-id="e7045-116">Simplify member access (IDE0002)</span></span>](ide0002.md)
- [<span data-ttu-id="e7045-117">删除不必要的强制转换 (IDE0004)</span><span class="sxs-lookup"><span data-stu-id="e7045-117">Remove unnecessary cast (IDE0004)</span></span>](ide0004.md)
- [<span data-ttu-id="e7045-118">删除不必要的导入 (IDE0005)</span><span class="sxs-lookup"><span data-stu-id="e7045-118">Remove unnecessary import (IDE0005)</span></span>](ide0005.md)
- [<span data-ttu-id="e7045-119">删除无法访问的代码 (IDE0035)</span><span class="sxs-lookup"><span data-stu-id="e7045-119">Remove unreachable code (IDE0035)</span></span>](ide0035.md)
- [<span data-ttu-id="e7045-120">删除未使用的私有成员 (IDE0051)</span><span class="sxs-lookup"><span data-stu-id="e7045-120">Remove unused private member (IDE0051)</span></span>](ide0051.md)
- [<span data-ttu-id="e7045-121">删除未读的私有成员 (IDE0052)</span><span class="sxs-lookup"><span data-stu-id="e7045-121">Remove unread private member (IDE0052)</span></span>](ide0052.md)
- [<span data-ttu-id="e7045-122">删除未使用的表达式值 (IDE0058)</span><span class="sxs-lookup"><span data-stu-id="e7045-122">Remove unused expression value (IDE0058)</span></span>](ide0058.md)
- [<span data-ttu-id="e7045-123">删除不必要的赋值 (IDE0059)</span><span class="sxs-lookup"><span data-stu-id="e7045-123">Remove unnecessary value assignment (IDE0059)</span></span>](ide0059.md)
- [<span data-ttu-id="e7045-124">删除未使用的参数 (IDE0060)</span><span class="sxs-lookup"><span data-stu-id="e7045-124">Remove unused parameter (IDE0060)</span></span>](ide0060.md)
- [<span data-ttu-id="e7045-125">删除不必要的抑制 (IDE0079)</span><span class="sxs-lookup"><span data-stu-id="e7045-125">Remove unnecessary suppression (IDE0079)</span></span>](ide0079.md)
- <span data-ttu-id="e7045-126">[删除不必要的抑制运算符 (IDE0080)](ide0080.md) - 仅限 C#。</span><span class="sxs-lookup"><span data-stu-id="e7045-126">[Remove unnecessary suppression operator (IDE0080)](ide0080.md) - C# only.</span></span>
- <span data-ttu-id="e7045-127">[删除 ByVal (IDE0081)](ide0081.md) - 仅限 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="e7045-127">[Remove 'ByVal' (IDE0081)](ide0081.md) - Visual Basic only.</span></span>
- [<span data-ttu-id="e7045-128">删除不必要的相等运算符 (IDE0100)</span><span class="sxs-lookup"><span data-stu-id="e7045-128">Remove unnecessary equality operator (IDE0100)</span></span>](ide0100.md)
- <span data-ttu-id="e7045-129">[删除不必要的弃元 (IDE0110)](ide0110.md) - 仅限 C#。</span><span class="sxs-lookup"><span data-stu-id="e7045-129">[Remove unnecessary discard (IDE0110)](ide0110.md) - C# only.</span></span>

<span data-ttu-id="e7045-130">其中某些规则包含配置规则行为的选项：</span><span class="sxs-lookup"><span data-stu-id="e7045-130">Some of these rules have options to configure the rule behavior:</span></span>

- [<span data-ttu-id="e7045-131">csharp_style_unused_value_expression_statement_preference (IDE0058)</span><span class="sxs-lookup"><span data-stu-id="e7045-131">csharp_style_unused_value_expression_statement_preference (IDE0058)</span></span>](ide0058.md#csharp_style_unused_value_expression_statement_preference)
- [<span data-ttu-id="e7045-132">visual_basic_style_unused_value_expression_statement_preference (IDE0058)</span><span class="sxs-lookup"><span data-stu-id="e7045-132">visual_basic_style_unused_value_expression_statement_preference (IDE0058)</span></span>](ide0058.md#visual_basic_style_unused_value_expression_statement_preference)
- [<span data-ttu-id="e7045-133">csharp_style_unused_value_assignment_preference (IDE0059)</span><span class="sxs-lookup"><span data-stu-id="e7045-133">csharp_style_unused_value_assignment_preference (IDE0059)</span></span>](ide0059.md#csharp_style_unused_value_assignment_preference)
- [<span data-ttu-id="e7045-134">visual_basic_style_unused_value_assignment_preference (IDE0059)</span><span class="sxs-lookup"><span data-stu-id="e7045-134">visual_basic_style_unused_value_assignment_preference (IDE0059)</span></span>](ide0059.md#visual_basic_style_unused_value_assignment_preference)
- [<span data-ttu-id="e7045-135">dotnet_code_quality_unused_parameters (IDE0060)</span><span class="sxs-lookup"><span data-stu-id="e7045-135">dotnet_code_quality_unused_parameters (IDE0060)</span></span>](ide0060.md#dotnet_code_quality_unused_parameters)
- [<span data-ttu-id="e7045-136">dotnet_remove_unnecessary_suppression_exclusions (IDE0079)</span><span class="sxs-lookup"><span data-stu-id="e7045-136">dotnet_remove_unnecessary_suppression_exclusions (IDE0079)</span></span>](ide0079.md#dotnet_remove_unnecessary_suppression_exclusions)

## <a name="see-also"></a><span data-ttu-id="e7045-137">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e7045-137">See also</span></span>

- [<span data-ttu-id="e7045-138">代码样式语言规则</span><span class="sxs-lookup"><span data-stu-id="e7045-138">Code style language rules</span></span>](language-rules.md)
- [<span data-ttu-id="e7045-139">代码样式规则参考</span><span class="sxs-lookup"><span data-stu-id="e7045-139">Code style rules reference</span></span>](index.md)
