---
description: 上下文关键字 - C# 参考
title: 上下文关键字 - C# 参考
ms.date: 04/05/2021
helpviewer_keywords:
- contextual keywords [C#]
ms.assetid: 7c76bc29-a754-4389-b0ab-f6b441018298
ms.openlocfilehash: 8e0c21ec6466a3d2f4536223929334ef3004098a
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075402"
---
# <a name="contextual-keywords-c-reference"></a>上下文关键字（C# 参考）

上下文关键字用于在代码中提供特定含义，但它不是 C# 中的保留字。 本部分介绍下面这些上下文关键字：  
  
|关键字|说明|  
|-------------|-----------------|  
|[add](./add.md)|定义一个自定义事件访问器，客户端代码订阅事件时会调用该访问器。|  
|[and](../operators/patterns.md#logical-patterns)|创建在两个嵌套模式均匹配时匹配的模式。|  
|[async](./async.md)|指示修改后的方法、lambda 表达式或匿名方法是异步的。|  
|[await](../operators/await.md)|挂起异步方法，直到等待的任务完成。|  
|[dynamic](../builtin-types/reference-types.md)|定义一个引用类型，实现发生绕过编译时类型检查的操作。|  
|[get](./get.md)|为属性或索引器定义访问器方法。|  
|[global](../operators/namespace-alias-qualifier.md)|未以其他方式命名的全局命名空间的别名。|  
|[init](./init.md)|为属性或索引器定义访问器方法。|  
|[nint](../builtin-types/nint-nuint.md)|定义本机大小的整数数据类型。|  
|[not](../operators/patterns.md#logical-patterns)|创建在否定模式不匹配时匹配的模式。|  
|[nuint](../builtin-types/nint-nuint.md)|定义本机大小的无符号整数数据类型。|  
|[or](../operators/patterns.md#logical-patterns)|创建在任一嵌套模式匹配时匹配的模式。|  
|[partial](./partial-type.md)|在整个同一编译单元内定义分部类、结构和接口。|  
|[record](../builtin-types/record.md)|用于定义记录类型。|  
|[remove](./remove.md)|定义一个自定义事件访问器，客户端代码取消订阅事件时会调用该访问器。|  
|[set](./set.md)|为属性或索引器定义访问器方法。|  
|[value](./value.md)|用于设置访问器和添加或删除事件处理程序。|  
|[var](./var.md)|使编译器能够确定在方法作用域中声明的变量类型。|  
|[when](when.md)|指定 `catch` 块的筛选条件或 `switch` 语句的 `case` 标签。|
|[where](./where-generic-type-constraint.md)|将约束添加到泛型声明。 （另请参阅 [where](./where-clause.md)）。|  
|[yield](./yield.md)|在迭代器块中使用，用于向枚举数对象返回值或用于表示迭代结束。|  
  
C# 3.0 中引入的所有查询关键字也都是上下文相关的。 有关详细信息，请参阅[查询关键字 (LINQ)](./query-keywords.md)。  
  
## <a name="see-also"></a>请参阅

- [C# 参考](../index.md)
- [C# 关键字](./index.md)
- [C# 运算符和表达式](../operators/index.md)
