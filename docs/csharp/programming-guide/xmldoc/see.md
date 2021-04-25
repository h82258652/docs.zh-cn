---
title: <see> - C# 编程指南
description: 了解 XML <see> 标记。 使用此标记，可以指定文本内的链接，例如通过使用 cref 属性。
ms.date: 07/20/2015
f1_keywords:
- <see>
- see
helpviewer_keywords:
- cref [C#], <see> tag
- <see> C# XML tag
- cross-references [C#]
- see C# XML tag
ms.assetid: 0200de01-7e2f-45c4-9094-829d61236383
ms.openlocfilehash: 351dd2e4c7dbc76efa2edbed708fb342c553ce70
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494852"
---
# <a name="see-c-programming-guide"></a>\<see>（C# 编程指南）

## <a name="syntax"></a>语法

```csharp
/// <see cref="member"/>
// or
/// <see href="link">Link Text</see>
// or
/// <see langword="keyword"/>
```

## <a name="parameters"></a>参数

- `cref="member"`

  对可从当前编译环境调用的成员或字段的引用。 编译器检查是否存在给定的码位元素，并将 `member` 传递到输出 XML 中的元素名称。 将成员置于双引号 (" ") 内。

- `href="link"`

  指向给定 URL 的可单击链接。 例如，`<see href="https://github.com">GitHub</see>` 生成一个可单击的链接，其中包含文本 :::no-loc text="GitHub":::，该文本链接到 `https://github.com`。

- `langword="keyword"`

  语言关键字，如 `true`。

## <a name="remarks"></a>备注

`<see>` 标记可用于从文本内指定链接。 使用 [\<seealso>](./seealso.md) 指示文本应该放在“另请参阅”部分中。 使用 [cref 属性](./cref-attribute.md)创建指向代码元素的文档页的内部超链接。 此外，``href`` 还是一个有效属性，将用作超链接。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

下面的示例展示摘要部分中的 `<see>` 标记。

[!code-csharp[csProgGuideDocComments#12](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#12)]

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
