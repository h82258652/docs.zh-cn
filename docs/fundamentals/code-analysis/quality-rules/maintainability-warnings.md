---
title: 可维护性规则（代码分析）
description: 了解代码分析可维护性规则。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.maintainabilityrules
helpviewer_keywords:
- rules, maintainability
- managed code analysis rules, maintainability rules
- maintainability rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: fc2ef2b42eeba241b7af66b579a60365ad2b88c7
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590172"
---
# <a name="maintainability-rules"></a><span data-ttu-id="05d33-103">可维护性规则</span><span class="sxs-lookup"><span data-stu-id="05d33-103">Maintainability rules</span></span>

<span data-ttu-id="05d33-104">可维护性规则支持库和应用程序维护。</span><span class="sxs-lookup"><span data-stu-id="05d33-104">Maintainability rules support library and application maintenance.</span></span>

## <a name="in-this-section"></a><span data-ttu-id="05d33-105">在本节中</span><span class="sxs-lookup"><span data-stu-id="05d33-105">In this section</span></span>

| <span data-ttu-id="05d33-106">规则</span><span class="sxs-lookup"><span data-stu-id="05d33-106">Rule</span></span> | <span data-ttu-id="05d33-107">描述</span><span class="sxs-lookup"><span data-stu-id="05d33-107">Description</span></span> |
|-----------|-----------------------------------|
| [<span data-ttu-id="05d33-108">CA1501:避免过度继承</span><span class="sxs-lookup"><span data-stu-id="05d33-108">CA1501: Avoid excessive inheritance</span></span>](ca1501.md) | <span data-ttu-id="05d33-109">类型在继承层次结构中的深度超过四级。</span><span class="sxs-lookup"><span data-stu-id="05d33-109">A type is more than four levels deep in its inheritance hierarchy.</span></span> <span data-ttu-id="05d33-110">深度嵌套的类型层次结构可能很难遵循、理解和维护。</span><span class="sxs-lookup"><span data-stu-id="05d33-110">Deeply nested type hierarchies can be difficult to follow, understand, and maintain.</span></span> |
| [<span data-ttu-id="05d33-111">CA1502:避免过度复杂</span><span class="sxs-lookup"><span data-stu-id="05d33-111">CA1502: Avoid excessive complexity</span></span>](ca1502.md) | <span data-ttu-id="05d33-112">此规则通过方法来测量线性独立的路径的数量，该数量是由条件分支的数量和复杂度决定的。</span><span class="sxs-lookup"><span data-stu-id="05d33-112">This rule measures the number of linearly independent paths through the method, which is determined by the number and complexity of conditional branches.</span></span> |
| [<span data-ttu-id="05d33-113">CA1505:避免使用无法维护的代码</span><span class="sxs-lookup"><span data-stu-id="05d33-113">CA1505: Avoid unmaintainable code</span></span>](ca1505.md) | <span data-ttu-id="05d33-114">类型或方法具有较低的可维护性索引值。</span><span class="sxs-lookup"><span data-stu-id="05d33-114">A type or method has a low maintainability index value.</span></span> <span data-ttu-id="05d33-115">如果可维护性指数较低，则表示类型或方法可能难以维护，最好重新进行设计。</span><span class="sxs-lookup"><span data-stu-id="05d33-115">A low maintainability index indicates that a type or method is probably difficult to maintain and would be a good candidate for redesign.</span></span> |
| [<span data-ttu-id="05d33-116">CA1506:避免过度类耦合度</span><span class="sxs-lookup"><span data-stu-id="05d33-116">CA1506: Avoid excessive class coupling</span></span>](ca1506.md) | <span data-ttu-id="05d33-117">此规则通过计算类型或方法包含的唯一类型引用的个数来衡量类耦合。</span><span class="sxs-lookup"><span data-stu-id="05d33-117">This rule measures class coupling by counting the number of unique type references that a type or method contains.</span></span> |
| [<span data-ttu-id="05d33-118">CA1507：使用 nameof 代替字符串</span><span class="sxs-lookup"><span data-stu-id="05d33-118">CA1507: Use nameof in place of string</span></span>](ca1507.md) | <span data-ttu-id="05d33-119">字符串字面量用作参数，可在其中使用 `nameof` 表达式。</span><span class="sxs-lookup"><span data-stu-id="05d33-119">A string literal is used as an argument where a `nameof` expression could be used.</span></span> |
| [<span data-ttu-id="05d33-120">CA1508：避免死条件代码</span><span class="sxs-lookup"><span data-stu-id="05d33-120">CA1508: Avoid dead conditional code</span></span>](ca1508.md) | <span data-ttu-id="05d33-121">方法具有在运行时始终计算为 `true` 或 `false` 的条件代码。</span><span class="sxs-lookup"><span data-stu-id="05d33-121">A method has conditional code that always evaluates to `true` or `false` at runtime.</span></span> <span data-ttu-id="05d33-122">这会导致条件的 `false` 分支中出现死代码。</span><span class="sxs-lookup"><span data-stu-id="05d33-122">This leads to dead code in the `false` branch of the condition.</span></span> |
| [<span data-ttu-id="05d33-123">CA1509：代码度量配置文件中的条目无效</span><span class="sxs-lookup"><span data-stu-id="05d33-123">CA1509: Invalid entry in code metrics configuration file</span></span>](ca1509.md) | <span data-ttu-id="05d33-124">代码度量规则（如 [CA1501](ca1501.md)、[CA1502](ca1502.md)、[CA1505](ca1505.md) 和 [CA1506](ca1506.md)）提供了具有无效条目的名为 `CodeMetricsConfig.txt` 的配置文件。</span><span class="sxs-lookup"><span data-stu-id="05d33-124">Code metrics rules, such as [CA1501](ca1501.md), [CA1502](ca1502.md), [CA1505](ca1505.md) and [CA1506](ca1506.md), supplied a configuration file named `CodeMetricsConfig.txt` that has an invalid entry.</span></span> |

## <a name="see-also"></a><span data-ttu-id="05d33-125">另请参阅</span><span class="sxs-lookup"><span data-stu-id="05d33-125">See also</span></span>

- [<span data-ttu-id="05d33-126">测量托管代码的复杂性和可维护性</span><span class="sxs-lookup"><span data-stu-id="05d33-126">Measure Complexity and Maintainability of Managed Code</span></span>](/visualstudio/code-quality/code-metrics-values)
