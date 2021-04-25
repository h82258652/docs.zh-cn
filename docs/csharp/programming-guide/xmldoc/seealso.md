---
title: <seealso> - C# 编程指南
description: 了解如何使用 XML <seealso> 标记。 使用此标记，可以指定想要在“另请参阅”部分中显示的文本。
ms.date: 07/20/2015
f1_keywords:
- cref
- <seealso>
- seealso
helpviewer_keywords:
- cref [C#], see also
- seealso C# XML tag
- cref [C#]
- cross-references [C#], tags
- <seealso> C# XML tag
ms.assetid: 8e157f3f-f220-4fcf-9010-88905b080b18
ms.openlocfilehash: 35420536ef14e91dcf8bf904573ab758d5448a63
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494091"
---
# <a name="seealso-c-programming-guide"></a>\<seealso>（C# 编程指南）

## <a name="syntax"></a>语法

```csharp
/// <seealso cref="member"/>
// or
/// <seealso href="link">Link Text</seealso>
```

## <a name="parameters"></a>参数

- `cref="member"`

  对可从当前编译环境调用的成员或字段的引用。 编译器检查是否存在给定的码位元素，并将 `member` 传递到输出 XML 中的元素名称。`member` 必须在双引号 (" ") 内。

  有关如何创建对泛型类型的 cref 引用的信息，请参阅 [cref 特性](./cref-attribute.md)。

- `href="link"`

  指向给定 URL 的可单击链接。 例如，`<seealso href="https://github.com">GitHub</seealso>` 生成一个可单击的链接，其中包含文本 :::no-loc text="GitHub":::，该文本链接到 `https://github.com`。

## <a name="remarks"></a>备注

使用 `<seealso>` 标记，可以指定想要在“另请参阅”部分中显示的文本。 使用 [\<see>](./see.md) 从文本内指定链接。

使用 [DocumentationFile](../../language-reference/compiler-options/output.md#documentationfile) 进行编译可以将文档注释处理到文件中。

## <a name="example"></a>示例

有关使用 \<seealso> 的示例，请参阅 [\<summary>](./summary.md)。

## <a name="see-also"></a>请参阅

- [C# 编程指南](../index.md)
- [建议的文档注释标记](./recommended-tags-for-documentation-comments.md)
