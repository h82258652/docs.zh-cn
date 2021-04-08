---
title: .NET 5 中断性变更：与 DataGridView 相关的 API 引发 InvalidOperationException
description: 了解 .NET 5 中的中断性变更：如果对象的 DataGridViewCellAccessibleObject.Owner 值为 null，则某些与 DataGridView 相关的 API 会引发异常。
ms.date: 09/18/2020
ms.openlocfilehash: 57ff50d7bdc83286d4d452746e8f64bace187edb
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079591"
---
# <a name="datagridview-related-apis-throw-invalidoperationexception"></a><span data-ttu-id="e9c0b-103">与 DataGridView 相关的 API 引发 InvalidOperationException</span><span class="sxs-lookup"><span data-stu-id="e9c0b-103">DataGridView-related APIs throw InvalidOperationException</span></span>

<span data-ttu-id="e9c0b-104">如果对象的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner?displayProperty=nameWithType> 值为 `null`，则与 <xref:System.Windows.Forms.DataGridView> 相关的某些 API 现在会引发 <xref:System.InvalidOperationException>。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-104">Some APIs related to <xref:System.Windows.Forms.DataGridView> now throw an <xref:System.InvalidOperationException> if the object's <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner?displayProperty=nameWithType> value is `null`.</span></span>

## <a name="change-description"></a><span data-ttu-id="e9c0b-105">更改说明</span><span class="sxs-lookup"><span data-stu-id="e9c0b-105">Change description</span></span>

<span data-ttu-id="e9c0b-106">在以前的 .NET 版本中，受影响的 API 在被调用时会引发 <xref:System.NullReferenceException>，并且 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-106">In previous .NET versions, the affected APIs throw a <xref:System.NullReferenceException> when they are invoked and the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property value is `null`.</span></span> <span data-ttu-id="e9c0b-107">从 .NET 5 开始，如果在调用 API 时，<xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`，则这些 API 将引发 <xref:System.InvalidOperationException>，而不会引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-107">Starting in .NET 5, these APIs throw an <xref:System.InvalidOperationException> instead of a <xref:System.NullReferenceException> if the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property value is `null` when they're invoked.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="e9c0b-108">更改原因</span><span class="sxs-lookup"><span data-stu-id="e9c0b-108">Reason for change</span></span>

<span data-ttu-id="e9c0b-109">引发 <xref:System.InvalidOperationException> 符合 .NET 运行时的行为。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-109">Throwing an <xref:System.InvalidOperationException> conforms to the behavior of the .NET runtime.</span></span> <span data-ttu-id="e9c0b-110">它还通过清楚地传达无效属性来改进调试体验。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-110">It also improves the debugging experience by clearly communicating the invalid property.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="e9c0b-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="e9c0b-111">Version introduced</span></span>

<span data-ttu-id="e9c0b-112">.NET 5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-112">.NET 5.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="e9c0b-113">建议操作</span><span class="sxs-lookup"><span data-stu-id="e9c0b-113">Recommended action</span></span>

<span data-ttu-id="e9c0b-114">查看你的代码，并在必要时对其进行更新，以防止使用属性为 `null` 的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 构造受影响的类型。</span><span class="sxs-lookup"><span data-stu-id="e9c0b-114">Review your code and, if necessary, update it to prevent constructing the affected types with the <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> property as `null`.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="e9c0b-115">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="e9c0b-115">Affected APIs</span></span>

<span data-ttu-id="e9c0b-116">下表列出了受影响的 API：</span><span class="sxs-lookup"><span data-stu-id="e9c0b-116">The following table lists the affected APIs:</span></span>

> [!div class="mx-tdBreakAll"]
> | <span data-ttu-id="e9c0b-117">受影响的方法或属性</span><span class="sxs-lookup"><span data-stu-id="e9c0b-117">Affected method or property</span></span> | <span data-ttu-id="e9c0b-118">已验证的属性</span><span class="sxs-lookup"><span data-stu-id="e9c0b-118">Validated property</span></span> | <span data-ttu-id="e9c0b-119">新增的版本</span><span class="sxs-lookup"><span data-stu-id="e9c0b-119">Version added</span></span> |
> |-|-|-|
> | <xref:System.Windows.Forms.DataGridViewButtonCell.DataGridViewButtonCellAccessibleObject.DoDefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-120">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-120">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.DefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-121">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-121">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.State?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-122">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-122">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.DoDefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-123">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-123">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Bounds?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-124">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-124">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.DefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-125">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-125">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Name?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-126">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-126">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Parent?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-127">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-127">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.DoDefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-128">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-128">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Navigate(System.Windows.Forms.AccessibleNavigation)?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-129">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-129">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewImageCell.DataGridViewImageCellAccessibleObject.DoDefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-130">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-130">5.0</span></span> |
> | <xref:System.Windows.Forms.DataGridViewLinkCell.DataGridViewLinkCellAccessibleObject.DoDefaultAction?displayProperty=nameWithType> | <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> | <span data-ttu-id="e9c0b-131">5.0</span><span class="sxs-lookup"><span data-stu-id="e9c0b-131">5.0</span></span> |

## <a name="see-also"></a><span data-ttu-id="e9c0b-132">另请参阅</span><span class="sxs-lookup"><span data-stu-id="e9c0b-132">See also</span></span>

- [<span data-ttu-id="e9c0b-133">与 DataGridView 相关的 API 引发 InvalidOperationException (.NET 6)</span><span class="sxs-lookup"><span data-stu-id="e9c0b-133">DataGridView-related APIs throw InvalidOperationException (.NET 6)</span></span>](../6.0/null-owner-causes-invalidoperationexception.md)

<!--

### Affected APIs

- `M:System.Windows.Forms.DataGridViewButtonCell.DataGridViewButtonCellAccessibleObject.DoDefaultAction`
- `P:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.DefaultAction`
- `P:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.State`
- `M:System.Windows.Forms.DataGridViewCheckBoxCell.DataGridViewCheckBoxCellAccessibleObject.DoDefaultAction`
- `P:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Bounds`
- `P:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.DefaultAction`
- `P:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Name`
- `P:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Parent`
- `M:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.DoDefaultAction`
- `M:System.Windows.Forms.DataGridViewColumnHeaderCell.DataGridViewColumnHeaderCellAccessibleObject.Navigate(System.Windows.Forms.AccessibleNavigation)`
- `M:System.Windows.Forms.DataGridViewImageCell.DataGridViewImageCellAccessibleObject.DoDefaultAction`
- `M:System.Windows.Forms.DataGridViewLinkCell.DataGridViewLinkCellAccessibleObject.DoDefaultAction`

### Category

Windows Forms

-->
