---
title: 全球化规则（代码分析）
description: 了解代码分析规则“全球化规则”
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.globalizationrules
helpviewer_keywords:
- rules, globalization
- globalization rules
- globalization [Visual Studio], rules
- managed code analysis rules, globalization rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: 83ecc52c5e20e074c6c1aed68204a574494f42d5
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590447"
---
# <a name="globalization-rules"></a>全球化规则

全球化规则支持世界通用库和应用程序。

## <a name="in-this-section"></a>在本节中

|规则|描述|
|----------|-----------------|
|[CA1303:请不要将文本作为本地化参数传递](ca1303.md)|某外部可见的方法将一个字符串字面量作为参数传递给 .NET 构造函数或方法，该字符串应该是可本地化的字符串。|
|[CA1304:指定 CultureInfo](ca1304.md)|某方法或构造函数调用的成员有一个接受 System.Globalization.CultureInfo 参数的重载，但该方法或构造函数没有调用接受 CultureInfo 参数的重载。 如果未提供 CultureInfo 或 System.IFormatProvider 对象，则重载成员提供的默认值可能不会在所有区域设置中产生您想要的效果。|
|[CA1305:指定 IFormatProvider](ca1305.md)|某方法或构造函数调用的一个或多个成员有接受 System.IFormatProvider 参数的重载，但该方法或构造函数没有调用接受 IFormatProvider 参数的重载。 如果未提供 System.Globalization.CultureInfo 或 IFormatProvider 对象，则重载成员提供的默认值可能不会在所有区域设置中产生您想要的效果。|
|[CA1307:为了清晰起见，请指定 StringComparison](ca1307.md)|字符串比较运算使用不设置 StringComparison 参数的方法重载。|
|[CA1308:将字符串规范化为大写](ca1308.md)|字符串应正常化为大写字母。 少量字符转换为小写字母后不能再转换回来。|
|[CA1309:使用按顺序的 StringComparison](ca1309.md)|非语义的字符串比较运算不会将 StringComparison 参数设置为 Ordinal 或 OrdinalIgnoreCase。 因此，通过将参数显式设置为 StringComparison.Ordinal 或 StringComparison.OrdinalIgnoreCase，通常可以提高代码的速度、正确性和可靠性。|
|[CA1310：为了确保正确，请指定 StringComparison](ca1310.md)|字符串比较操作使用未设置 StringComparison 参数的方法重载，并默认使用区域性特定的字符串比较。|
|[CA2101：指定对 P/Invoke 字符串参数进行封送处理](ca2101.md)|某平台调用成员允许部分信任的调用方，具有一个字符串参数，并且不显式封送该字符串。 这可能导致潜在的安全漏洞。|
