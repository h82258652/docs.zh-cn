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
# <a name="datagridview-related-apis-now-throw-invalidoperationexception"></a>与 DataGridView 相关的 API 现在引发 InvalidOperationException

如果对象的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner?displayProperty=nameWithType> 值为 `null`，则与 <xref:System.Windows.Forms.DataGridView> 相关的某些 API 现在会引发 <xref:System.InvalidOperationException>。

## <a name="change-description"></a>更改说明

在以前的 .NET 版本中，受影响的 API 在被调用时会引发 <xref:System.NullReferenceException>，并且 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`。 从 .NET 6 开始，如果在调用 API 时，<xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 属性值为 `null`，则这些 API 将引发 <xref:System.InvalidOperationException>，而不会引发 <xref:System.NullReferenceException>。

## <a name="reason-for-change"></a>更改原因

引发 <xref:System.InvalidOperationException> 符合 .NET 运行时的行为。 它还通过清楚地传达无效属性来改进调试体验。

## <a name="version-introduced"></a>引入的版本

.NET 6.0

## <a name="recommended-action"></a>建议操作

查看你的代码，并在必要时对其进行更新，以防止使用属性为 `null` 的 <xref:System.Windows.Forms.DataGridViewCell.DataGridViewCellAccessibleObject.Owner> 构造受影响的类型。

## <a name="affected-apis"></a>受影响的 API

下表列出了受影响的属性和方法：

> [!div class="mx-tdBreakAll"]
> | 受影响的方法或属性 | 新增的版本 |
> |-|-|
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Bounds?displayProperty=fullName> | 预览版 4 |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.DefaultAction?displayProperty=fullName> | 预览版 4 |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Name?displayProperty=fullName> | 预览版 4 |
>| <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.Navigate(System.Windows.Forms.AccessibleNavigation)?displayProperty=fullName> | 预览版 4 |
> | <xref:System.Windows.Forms.DataGridViewTopLeftHeaderCell.DataGridViewTopLeftHeaderCellAccessibleObject.State?displayProperty=fullName> | 预览版 4 |

## <a name="see-also"></a>另请参阅

- [与 DataGridView 相关的 API 引发 InvalidOperationException (.NET 5)](../5.0/null-owner-causes-invalidoperationexception.md)

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
