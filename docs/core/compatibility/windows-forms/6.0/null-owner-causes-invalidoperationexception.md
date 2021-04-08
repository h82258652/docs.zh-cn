---
title: .NET 6 中断性变更：与 DataGridView 相关的 API 引发 InvalidOperationException
description: 了解 .NET 6 中的中断性变更：如果对象的 DataGridViewCellAccessibleObject.Owner 值为 null，则某些与 DataGridView 相关的 API 会引发异常。
ms.date: 03/29/2021
ms.openlocfilehash: 3726296bb93c88d438e45ea832bf9070ace69dd0
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106080661"
---
# <a name="datagridview-related-apis-now-throw-invalidoperationexception"></a><span data-ttu-id="8088a-103">与 DataGridView 相关的 API 现在引发 InvalidOperationException</span><span class="sxs-lookup"><span data-stu-id="8088a-103">DataGridView-related APIs now throw InvalidOperationException</span></span>

<span data-ttu-id="8088a-104">如果对象的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner?displayProperty=nameWithType> 值为 `null`，则与 <xref:System.Windows.Forms.DataGridView> 相关的某些 API 现在会引发 <xref:System.InvalidOperationException>。</span><span class="sxs-lookup"><span data-stu-id="8088a-104">Some APIs related to <xref:System.Windows.Forms.DataGridView> now throw an <xref:System.InvalidOperationException> if the object's <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner?displayProperty=nameWithType> value is `null`.</span></span>

## <a name="change-description"></a><span data-ttu-id="8088a-105">更改说明</span><span class="sxs-lookup"><span data-stu-id="8088a-105">Change description</span></span>

<span data-ttu-id="8088a-106">在以前的 .NET 版本中，受影响的 API 在被调用时会引发 <xref:System.NullReferenceException>，并且 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`。</span><span class="sxs-lookup"><span data-stu-id="8088a-106">In previous .NET versions, the affected APIs throw a <xref:System.NullReferenceException> when they are invoked and the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property value is `null`.</span></span> <span data-ttu-id="8088a-107">从 .NET 6 开始，如果在调用 API 时，<xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`，则这些 API 将引发 <xref:System.InvalidOperationException>，而不会引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="8088a-107">Starting in .NET 6, these APIs throw an <xref:System.InvalidOperationException> instead of a <xref:System.NullReferenceException> if the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property value is `null` when they're invoked.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="8088a-108">更改原因</span><span class="sxs-lookup"><span data-stu-id="8088a-108">Reason for change</span></span>

<span data-ttu-id="8088a-109">引发 <xref:System.InvalidOperationException> 符合 .NET 运行时的行为。</span><span class="sxs-lookup"><span data-stu-id="8088a-109">Throwing an <xref:System.InvalidOperationException> conforms to the behavior of the .NET runtime.</span></span> <span data-ttu-id="8088a-110">它还通过清楚地传达无效属性来改进调试体验。</span><span class="sxs-lookup"><span data-stu-id="8088a-110">It also improves the debugging experience by clearly communicating the invalid property.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="8088a-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="8088a-111">Version introduced</span></span>

<span data-ttu-id="8088a-112">.NET 6.0</span><span class="sxs-lookup"><span data-stu-id="8088a-112">.NET 6.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="8088a-113">建议操作</span><span class="sxs-lookup"><span data-stu-id="8088a-113">Recommended action</span></span>

<span data-ttu-id="8088a-114">查看你的代码，并在必要时对其进行更新，以防止使用属性为 `null` 的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 构造受影响的类型。</span><span class="sxs-lookup"><span data-stu-id="8088a-114">Review your code and, if necessary, update it to prevent constructing the affected types with the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property as `null`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="8088a-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="8088a-115">Affected APIs</span></span>

<span data-ttu-id="8088a-116">下表列出了受影响的属性和方法：</span><span class="sxs-lookup"><span data-stu-id="8088a-116">The following table lists the affected properties and methods:</span></span>

> [!div class="mx-tdBreakAll"]
> | <span data-ttu-id="8088a-117">受影响的方法或属性</span><span class="sxs-lookup"><span data-stu-id="8088a-117">Affected method or property</span></span> | <span data-ttu-id="8088a-118">新增的版本</span><span class="sxs-lookup"><span data-stu-id="8088a-118">Version added</span></span> |
> |-|-|
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Bounds?displayProperty=fullName> | <span data-ttu-id="8088a-119">预览版 4</span><span class="sxs-lookup"><span data-stu-id="8088a-119">Preview 4</span></span> |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.DefaultAction?displayProperty=fullName> | <span data-ttu-id="8088a-120">预览版 4</span><span class="sxs-lookup"><span data-stu-id="8088a-120">Preview 4</span></span> |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Name?displayProperty=fullName> | <span data-ttu-id="8088a-121">预览版 4</span><span class="sxs-lookup"><span data-stu-id="8088a-121">Preview 4</span></span> |
>| <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Navigate(System.Windows.Forms.AccessibleNavigation)?displayProperty=fullName> | <span data-ttu-id="8088a-122">预览版 4</span><span class="sxs-lookup"><span data-stu-id="8088a-122">Preview 4</span></span> |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.State?displayProperty=fullName> | <span data-ttu-id="8088a-123">预览版 4</span><span class="sxs-lookup"><span data-stu-id="8088a-123">Preview 4</span></span> |

## <a name="see-also"></a><span data-ttu-id="8088a-124">另请参阅</span><span class="sxs-lookup"><span data-stu-id="8088a-124">See also</span></span>

- [<span data-ttu-id="8088a-125">与 DataGridView 相关的 API 引发 InvalidOperationException (.NET 5)</span><span class="sxs-lookup"><span data-stu-id="8088a-125">DataGridView-related APIs throw InvalidOperationException (.NET 5)</span></span>](../5.0/null-owner-causes-invalidoperationexception.md)

<!--

### Affected APIs

- `P:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Bounds`
- `P:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.DefaultAction`
- `P:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Name`
- `M:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Navigate(System.Windows.Forms.AccessibleNavigation)`
- `P:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.State`

### Category

Windows Forms

-->
