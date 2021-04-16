---
title: 可靠性规则（代码分析）
description: 了解代码分析可靠性规则。
ms.date: 11/04/2016
ms.topic: reference
f1_keywords:
- vs.codeanalysis.reliabilityrules
helpviewer_keywords:
- rules, reliability
- reliability rules
- managed code analysis rules, reliability rules
author: gewarren
ms.author: gewarren
ms.openlocfilehash: a747dd4dcda351a1ddb0f3d069bb7bac895c32f8
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590536"
---
# <a name="reliability-rules"></a>可靠性规则

支持库和应用程序可靠性（例如正确使用内存和线程）的可靠性规则。 可靠性规则包括：

|规则|描述|
|----------|-----------------|
|[CA2000:丢失范围之前释放对象](ca2000.md)|由于可能发生异常事件，导致对象的终结器无法运行，因此，应显式释放对象，以避免对该对象的所有引用超出范围。|
|[CA2002:不要锁定具有弱标识的对象](ca2002.md)|当可以跨应用程序域边界直接进行访问对象时，则认为该对象具有弱标识。 对于尝试获取对具有弱标识的对象的锁的线程，该线程可能会被其他应用程序域中持有对同一对象的锁的另一线程所阻止。|
|[CA2007：不直接等待任务](ca2007.md)|异步方法会直接[等待](../../../csharp/language-reference/operators/await.md) <xref:System.Threading.Tasks.Task>。|
|[CA2008：不要在未传递 TaskScheduler 的情况下创建任务](ca2008.md)|任务创建或延续操作使用未指定 <xref:System.Threading.Tasks.TaskScheduler> 参数的方法重载。|
|[CA2009：请勿对 ImmutableCollection 值调用 ToImmutableCollection](ca2009.md)|没有必要在 <xref:System.Collections.Immutable> 命名空间的不可变集合上调用 `ToImmutable` 方法。|
|[CA2011:请勿在其资源库中分配属性](ca2011.md) | 属性在自身的 [set 访问器](../../../csharp/programming-guide/classes-and-structs/using-properties.md#the-set-accessor)中被意外赋值。 |
|[CA2012:正确使用 ValueTask](ca2012.md) | 从成员调用中返回的 ValueTasks 旨在直接等待。  多次尝试使用 ValueTask 或在已知完成之前直接访问其结果可能会导致异常或损坏。  忽略此类 ValueTask 可能指示出现功能 Bug，还可能降低性能。 |
|[CA2013:请勿将 ReferenceEquals 与值类型结合使用](ca2013.md) | 使用 <xref:System.Object.ReferenceEquals%2A?displayProperty=fullName> 比较值时，如果 objA 和 objB 是值类型，则在将其传递给 <xref:System.Object.ReferenceEquals%2A> 方法之前将它们装箱。 这意味着，即使 objA 和 objB 都表示值类型的同一个实例，<xref:System.Object.ReferenceEquals%2A> 方法也会返回 false。 |
|[CA2014：请勿在循环中使用 stackalloc。](ca2014.md) | 仅在当前方法调用结束时，Stackalloc 分配的堆栈空间才会释放。  在循环中使用此方法可能导致无限堆栈增长，最终出现堆栈溢出的情况。 |
|[CA2015：请勿为派生自 MemoryManager&lt;T&gt;](ca2015.md) 的类型定义终结器 | 将终结器添加到派生自 <xref:System.Buffers.MemoryManager%601> 的类型可能使内存在仍被 <xref:System.Span%601> 使用时得到释放。 |
|[CA2016：将 CancellationToken 参数转发到采用一个该参数的方法](ca2016.md) | 将 `CancellationToken` 参数转发给方法来确保操作取消通知得到正确传播，或者在 `CancellationToken.None` 中显式传递，以指示有意不传播令牌。 |
