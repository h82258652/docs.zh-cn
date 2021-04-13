---
title: 中断性变更：一些 API 引发 ArgumentNullException
description: 了解 .NET 6 中的中断性变更：一些 API 现在会验证参数并引发 ArgumentNullException。
ms.date: 01/29/2021
ms.openlocfilehash: 1c7b41bf5e367dca394130da66360e660173ba14
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106496847"
---
# <a name="some-apis-throw-argumentnullexception"></a><span data-ttu-id="67007-103">一些 API 引发 ArgumentNullException</span><span class="sxs-lookup"><span data-stu-id="67007-103">Some APIs throw ArgumentNullException</span></span>

<span data-ttu-id="67007-104">如果通过 `null` 输入参数进行调用，一些 API 现在会验证输入参数并引发 <xref:System.ArgumentNullException>，而此前会引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="67007-104">Some APIs now validate input parameters and throw an <xref:System.ArgumentNullException> where previously they threw a <xref:System.NullReferenceException>, if invoked with `null` input arguments.</span></span>

## <a name="change-description"></a><span data-ttu-id="67007-105">更改描述</span><span class="sxs-lookup"><span data-stu-id="67007-105">Change description</span></span>

<span data-ttu-id="67007-106">在以前的 .NET 版本中，在通过值为 `null` 的参数进行调用时，受影响的 API 会引发 <xref:System.NullReferenceException>。</span><span class="sxs-lookup"><span data-stu-id="67007-106">In previous .NET versions, the affected APIs throw a <xref:System.NullReferenceException> if invoked with an argument that's `null`.</span></span>

<span data-ttu-id="67007-107">从 .NET 6 开始，在通过值为 `null` 的参数进行调用时，受影响的 API 会引发 <xref:System.ArgumentNullException>。</span><span class="sxs-lookup"><span data-stu-id="67007-107">Starting in .NET 6, the affected APIs throw an <xref:System.ArgumentNullException> if invoked with an argument that's `null`.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="67007-108">更改原因</span><span class="sxs-lookup"><span data-stu-id="67007-108">Reason for change</span></span>

<span data-ttu-id="67007-109">引发 <xref:System.ArgumentNullException> 符合 .NET 运行时行为。</span><span class="sxs-lookup"><span data-stu-id="67007-109">Throwing <xref:System.ArgumentNullException> conforms to .NET Runtime behavior.</span></span> <span data-ttu-id="67007-110">它提供了更好的调试体验，清晰地传达了是哪个参数引起的异常。</span><span class="sxs-lookup"><span data-stu-id="67007-110">It provides a better debug experience by clearly communicating which argument caused the exception.</span></span>

## <a name="version-introduced"></a><span data-ttu-id="67007-111">引入的版本</span><span class="sxs-lookup"><span data-stu-id="67007-111">Version introduced</span></span>

<span data-ttu-id="67007-112">.NET 6.0</span><span class="sxs-lookup"><span data-stu-id="67007-112">.NET 6.0</span></span>

## <a name="recommended-action"></a><span data-ttu-id="67007-113">建议的操作</span><span class="sxs-lookup"><span data-stu-id="67007-113">Recommended action</span></span>

- <span data-ttu-id="67007-114">审查代码并在必要时更新，以防止向受影响的 API 传递 `null` 输入参数。</span><span class="sxs-lookup"><span data-stu-id="67007-114">Review and, if necessary, update your code to prevent passing `null` input arguments to the affected APIs.</span></span>
- <span data-ttu-id="67007-115">如果你的代码处理了 <xref:System.NullReferenceException>，请替换或增加一个 <xref:System.ArgumentNullException> 的处理程序。</span><span class="sxs-lookup"><span data-stu-id="67007-115">If your code handles <xref:System.NullReferenceException>, replace or add an additional handler for <xref:System.ArgumentNullException>.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="67007-116">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="67007-116">Affected APIs</span></span>

<span data-ttu-id="67007-117">下表列出了受影响的 API 和特定参数：</span><span class="sxs-lookup"><span data-stu-id="67007-117">The following table lists the affected APIs and specific parameters:</span></span>

| <span data-ttu-id="67007-118">方法/属性</span><span class="sxs-lookup"><span data-stu-id="67007-118">Method/property</span></span> | <span data-ttu-id="67007-119">参数名称</span><span class="sxs-lookup"><span data-stu-id="67007-119">Parameter name</span></span> | <span data-ttu-id="67007-120">版本已更改</span><span class="sxs-lookup"><span data-stu-id="67007-120">Version changed</span></span> |
|-|-|-|
| <xref:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)?displayProperty=fullName> | `index` | <span data-ttu-id="67007-121">预览版 1</span><span class="sxs-lookup"><span data-stu-id="67007-121">Preview 1</span></span> |
| <xref:System.Windows.Forms.DrawTreeNodeEventArgs.%23ctor(System.Drawing.Graphics,System.Windows.Forms.TreeNode,System.Drawing.Rectangle,System.Windows.Forms.TreeNodeStates)> | `graphics` | <span data-ttu-id="67007-122">预览版 3</span><span class="sxs-lookup"><span data-stu-id="67007-122">Preview 3</span></span> |
| <xref:System.Windows.Forms.DataGridViewRowStateChangedEventArgs.%23ctor(System.Windows.Forms.DataGridViewRow,System.Windows.Forms.DataGridViewElementStates)> | `dataGridViewRow` | <span data-ttu-id="67007-123">预览版 4</span><span class="sxs-lookup"><span data-stu-id="67007-123">Preview 4</span></span> |

## <a name="see-also"></a><span data-ttu-id="67007-124">另请参阅</span><span class="sxs-lookup"><span data-stu-id="67007-124">See also</span></span>

- [<span data-ttu-id="67007-125">如果节点被分配到其他地方，则 TreeNodeCollection.Item 抛出异常</span><span class="sxs-lookup"><span data-stu-id="67007-125">TreeNodeCollection.Item throws exception if node is assigned elsewhere</span></span>](treenodecollection-item-throws-argumentexception.md)

<!--

### Affected APIs

- `P:System.Windows.Forms.TreeNodeCollection.Item(System.Int32)`
- `M:System.Windows.Forms.DrawTreeNodeEventArgs.#ctor(System.Drawing.Graphics,System.Windows.Forms.TreeNode,System.Drawing.Rectangle,System.Windows.Forms.TreeNodeStates)`
- `M:System.Windows.Forms.DataGridViewRowStateChangedEventArgs.#ctor(System.Windows.Forms.DataGridViewRow,System.Windows.Forms.DataGridViewElementStates)`

### Category

Windows Forms

-->
