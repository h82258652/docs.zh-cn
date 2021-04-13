---
title: 什么是托管代码？
description: 了解托管代码为何是其执行由运行时（公共语言运行时 (CLR)）管理的代码。
ms.date: 06/20/2016
ms.assetid: 20bb7ea8-192e-4a96-8ef3-e10e1950fd3d
ms.openlocfilehash: 10036ab92718aa56a068c7515dec4302a5c3ac9c
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106498512"
---
# <a name="what-is-managed-code"></a>什么是“托管代码”？

使用 .NET 时，我们经常会遇到“托管代码”这个术语。 本文档解释这个术语的含义及其更多相关信息。

简而言之，托管代码就是执行过程交由运行时管理的代码。 在这种情况下，相关的运行时称为公共语言运行时 (CLR)，不管使用的是哪种实现（例如 [Mono](https://www.mono-project.com/)、.NET Framework 或 .NET Core/.NET 5+）。 CLR 负责提取托管代码、将其编译成机器代码，然后执行它。 除此之外，运行时还提供多个重要服务，例如自动内存管理、安全边界、类型安全，等等。

相反，如果运行 C/C++ 程序，则运行的代码也称为“非托管代码”。 在非托管环境中，程序员需要亲自负责处理相当多的事情。 实际的程序在本质上是操作系统 (OS) 载入内存，然后启动的二进制代码。 其他任何工作 - 从内存管理到安全考虑因素 - 对于程序员来说是一个不小的负担。

托管代码是使用可在 .NET 上运行的一种高级语言（例如 C#、Visual Basic、F# 等）编写的。 使用相应的编译器编译以这些语言编写的代码时，无法获得机器代码， 而是获得 **中间语言** 代码，然后运行时会对其进行编译并将其执行。 C++ 是这条规则的一个例外，因为它也能够生成可在 Windows 上运行的本机非托管二进制代码。

## <a name="intermediate-language--execution"></a>中间语言和执行

什么是“中间语言”（简称 IL）？ 中间语言是编译使用高级 .NET 语言编写的代码后获得的结果。 对使用其中一种语言编写的代码进行编译后，即可获得 IL 所生成的二进制代码。 必须注意，IL 独立于在运行时顶层运行的任何特定语言；行业甚至为它单独制定了规范，如果有需要，你可以阅读该规范。

从高级代码生成 IL 后，你很有可能想要运行它。 CLR 此时将接管工作，启动 **实时** (JIT) 编译过程，或者将代码从 IL **实时** 编译成可以真正在 CPU 上运行的机器代码。 这样，CLR 就能确切地知道代码的作用，并可以有效地 _管理_ 代码。

中间语言有时也称为公共中间语言 (CIL) 或 Microsoft 中间语言 (MSIL)。

## <a name="unmanaged-code-interoperability"></a>托管代码互操作性

当然，CLR 允许越过托管与非托管环境之间的边界，同时，即使在[基类库](framework-libraries.md)中，也有很多代码可以做到这一点。 这称为 **互操作性**，简称 **interop**。 例如，使用这些机制可以包装某个非托管库以及调用该库。 但是，请务必注意，如果采取这种方法，当代码越过运行时的边界时，实际的执行管理将再次交接到托管代码，因而需要遵守相同的限制。

与此类似，C# 语言可让你利用所谓的 **不安全上下文**（指定执行过程不由 CLR 管理的代码片段），在代码中直接使用非托管构造，例如指针。

## <a name="more-resources"></a>更多资源

* [.NET Framework 概述](../framework/get-started/overview.md)
* [不安全代码和指针](../csharp/language-reference/unsafe-code.md)
* [本机互操作性](./native-interop/index.md)
