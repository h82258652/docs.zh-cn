---
title: 文档规则（代码分析）
description: 了解代码分析规则“文档规则”
ms.date: 09/16/2019
ms.topic: reference
f1_keywords:
- vs.codeanalysis.documentationrules
helpviewer_keywords:
- documentation rules
- managed code analysis rules, documentation rules
- warnings, documentation
author: mavasani
ms.author: mavasani
ms.openlocfilehash: 29d1734eec29bd00d456b4b00c48c2e8ef223441
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590258"
---
# <a name="documentation-rules"></a>文档规则

文档规则支持通过正确为外部可见的 API 使用 [XML 文档注释](../../../csharp/codedoc.md)来编写记录详尽的库。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
| - | - |
| [CA1200:不要使用带前缀的 cref 标记](ca1200.md) | XML 文档标记中的 [cref](../../../csharp/programming-guide/xmldoc/cref-attribute.md) 属性是指“代码引用”。 它指定标记的内部文本是一个代码元素，例如类型、方法或属性。 避免使用带有前缀的 `cref` 标记，因为它会阻止编译器验证引用。 它还会阻止 Visual Studio 集成开发环境 (IDE) 在重构过程中查找和更新这些符号引用。 |
