---
title: 运行时事件
description: 查看 .NET 运行时 (CoreCLR) 发出的诊断事件，这些事件可与 ETW、LTTng 或 EventPipe 一起使用。
ms.date: 11/13/2020
ms.topic: reference
helpviewer_keywords:
- runtime events (CoreCLR)
- ETW, EventPipe runtime events (CoreCLR)
ms.openlocfilehash: 33fa7275ce40934ce043b4d0dba5749c37515755
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591059"
---
# <a name="net-runtime-events"></a>.NET 运行时事件

.NET 运行时 (CoreCLR) 发出各种事件，这些事件可用于诊断 .NET 应用程序的问题，并且可通过各种机制（例如 `ETW`、`LTTng` 和 `EventPipe`）来使用。

本文档作为对 .NET Core 运行时触发的事件的参考。

有关 .NET Framework 中的运行时事件，请参阅 [CLR ETW 事件](../../framework/performance/clr-etw-events.md)。

## <a name="in-this-section"></a>在本节中

[争用事件](runtime-contention-events.md)\
这些事件收集有关监视器锁争用的信息。

[垃圾回收事件](runtime-garbage-collection-events.md)\
这些事件可收集有关垃圾回收的信息。 它们可帮助进行诊断和调试，包括确定垃圾回收执行的次数、垃圾回收期间释放的内存量等。

[异常事件](runtime-exception-events.md)\
这些运行时事件捕获有关引发的异常的信息。

[互操作事件](runtime-interop-events.md)\
这些运行时事件捕获有关公共中间语言 (CIL) 存根生成的信息。

[加载器和绑定器事件](runtime-loader-binder-events.md)\
这些事件收集有关加载和卸载程序集和模块的信息。

[方法事件](runtime-method-events.md)\
这些事件收集特定于方法的信息。 符号解析需要这些事件的负载。 此外，这些事件还提供如调用方法的次数等有用信息。

[线程事件](runtime-thread-events.md)\
这些事件收集有关工作线程和 I/O 线程的信息。

[类型事件](runtime-type-events.md)\
这些事件收集有关类型系统的信息。
