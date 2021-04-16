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
# <a name="portability-and-interoperability-rules"></a>可移植性和互操作性规则

可移植性规则支持跨不同平台的可移植性。 互操作性规则支持与 COM 客户端交互。

## <a name="in-this-section"></a>在本节中

| 规则 | 描述 |
| - | - |
| [CA1401：P/Invokes 应为不可见](ca1401.md) | 公共类型中的公共或受保护方法具有 System.Runtime.InteropServices.DllImportAttribute 属性（在 Visual Basic 中由 Declare 关键字实现）。 这些方法不能公开。 |
| [CA1416：验证平台兼容性](ca1416.md) | 在组件上使用依赖于平台的 API 会使代码无法用于所有平台。 |
| [CA1417：请勿对 P/Invokes 的字符串参数使用 `OutAttribute`](ca1417.md) | 如果该字符串为暂存的字符串，则通过包含 `OutAttribute` 的值传递的字符串参数可能使运行时变得不稳定。 |
