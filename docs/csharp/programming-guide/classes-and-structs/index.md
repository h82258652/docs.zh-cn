---
title: 类、结构和记录 - C# 编程指南
description: 介绍如何在 C# 中使用类和结构和记录。
ms.date: 04/06/2021
helpviewer_keywords:
- structs [C#], about structs
- records [C#], about records
- classes [C#], overview
- C# language, structs
- C# language, records
- C# language, objects
- objects [C#]
- C# language, classes
ms.openlocfilehash: 69b62160699ef43343a89fe8bb3deaa2fb3523ba
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075454"
---
# <a name="classes-structs-and-records-c-programming-guide"></a>类、结构和记录（C# 编程指南）

类和结构是 .NET 通用类型系统的两种基本构造。 C# 9 添加记录，记录是一种类。 每种本质上都是一种数据结构，其中封装了同属一个逻辑单元的一组数据和行为。 数据和行为是类、结构或记录的成员，包括方法、属性和事件等（本文稍后将具体列举）。

类、结构或记录声明类似于一张蓝图，用于在运行时创建实例或对象。 如果定义名为 `Person` 的类、结构或记录，则 `Person` 是类型的名称。 如果声明和初始化 `Person` 类型的变量 `p`，那么 `p` 就是所谓的 `Person` 对象或实例。 可以创建同一 `Person` 类型的多个实例，每个实例都可以有不同的属性和字段值。  
  
类或记录是引用类型。 创建类型的对象后，向其分配对象的变量仅保留对相应内存的引用。 将对象引用分配给新变量后，新变量会引用原始对象。 通过一个变量所做的更改将反映在另一个变量中，因为它们引用相同的数据。
  
结构是值类型。 创建结构时，向其分配结构的变量保留结构的实际数据。 将结构分配给新变量时，会复制结构。 因此，新变量和原始变量包含相同数据的副本（共两个）。 对一个副本所做的更改不会影响另一个副本。  
  
一般来说，类用于对更复杂的行为或应在类对象创建后进行修改的数据建模。 结构最适用于所含大部分数据不得在结构创建后进行修改的小型数据结构。 记录类型可用于所含大部分是不得在创建对象后修改的数据的大型数据结构。
  
## <a name="example"></a>示例

 在以下示例中，`ProgrammingGuide` 命名空间中的 `CustomClass` 有以下三个成员：实例构造函数、`Number` 属性和 `Multiply` 方法。 `Program` 类中的 `Main` 方法创建 `CustomClass` 的实例（对象），此对象的方法和属性可使用点表示法进行访问。
  
 :::code language="csharp" source="../../../../samples/snippets/csharp/programming-guide/classes-and-structs/class1.cs" id="Snippet1":::  
  
## <a name="encapsulation"></a>封装  

 *封装* 有时称为面向对象的编程的第一支柱或原则。 根据封装原则，类或结构可以指定自己的每个成员对外部代码的可访问性。 可以隐藏不得在类或程序集外部使用的方法和变量，以限制编码错误或恶意攻击发生的可能性。 有关详细信息，请参阅[面向对象的编程](../../tutorials/intro-to-csharp/object-oriented-programming.md)。

## <a name="members"></a>成员

所有方法、字段、常量、属性和事件都必须在类型中进行声明；这些被称为类型的 *成员*。 C# 没有全局变量或方法，这一点其他某些语言不同。 即使是编程的入口点（`Main` 方法），也必须在类或结构中声明（对于[顶级语句](../main-and-command-args/top-level-statements.md)的情况，是隐式声明）。

下面列出了所有可以在类、结构或记录中声明的各种成员。
  
- [字段](./fields.md)  
- [常量](./constants.md)
- [属性](./properties.md)
- [方法](./methods.md)
- [构造函数](./constructors.md)
- [事件](../events/index.md)
- [终结器](./destructors.md)
- [索引器](../indexers/index.md)
- [运算符](../../language-reference/operators/index.md)
- [嵌套类型](./nested-types.md)
  
## <a name="accessibility"></a>可访问性  

 一些方法和属性可供类或结构外部的代码（称为“*客户端代码*”）调用或访问。 另一些方法和属性只能在类或结构本身中使用。 请务必限制代码的可访问性，仅供预期的客户端代码进行访问。 需要使用以下访问修饰符指定类型及其成员对客户端代码的可访问性：

- [public](../../language-reference/keywords/public.md)
- [受保护](../../language-reference/keywords/protected.md)
- [internal](../../language-reference/keywords/internal.md)
- [protected internal](../../language-reference/keywords/protected-internal.md)
- [private](../../language-reference/keywords/private.md)
- [专用受保护](../../language-reference/keywords/private-protected.md)。

可访问性的默认值为 `private`。 有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。  
  
## <a name="inheritance"></a>继承  

类（而非结构）支持继承的概念。 派生自另一个类（基类）的类自动包含基类的所有公共、受保护和内部成员（其构造函数和终结器除外）。 有关详细信息，请参阅[继承](./inheritance.md)和[多态性](./polymorphism.md)。
  
可以将类声明为 [abstract](../../language-reference/keywords/abstract.md)，即一个或多个方法没有实现代码。 尽管抽象类无法直接实例化，但可以作为提供缺少实现代码的其他类的基类。 类还可以声明为 [sealed](../../language-reference/keywords/sealed.md)，以阻止其他类继承。 有关详细信息，请参阅[抽象类、密封类及类成员](./abstract-and-sealed-classes-and-class-members.md)。
  
## <a name="interfaces"></a>界面  

类、结构和记录可以继承多个接口。 继承自接口意味着类型实现接口中定义的所有方法。 有关详细信息，请参阅[接口](../interfaces/index.md)。  
  
## <a name="generic-types"></a>泛型类型  

 类、结构和记录可以使用一个或多个类型参数进行定义。 客户端代码在创建类型实例时提供类型。 例如，<xref:System.Collections.Generic> 命名空间中的 <xref:System.Collections.Generic.List%601> 类就是用一个类型参数进行定义的。 客户端代码创建 `List<string>` 或 `List<int>` 的实例来指定列表将包含的类型。 有关详细信息，请参阅[泛型](../generics/index.md)。  
  
## <a name="static-types"></a>静态类型  

类（而非结构或记录）可以声明为[静态](../../language-reference/keywords/static.md)。 静态类只能包含静态成员，不能使用 `new` 关键字进行实例化。 在程序加载时，类的一个副本会加载到内存中，而其成员则可通过类名进行访问。 类、结构和记录可以包含静态成员。 有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。
  
## <a name="nested-types"></a>嵌套类型

类、结构和记录可以嵌套在其他类、结构和记录中。 有关详细信息，请参阅[嵌套类型](./nested-types.md)。
  
## <a name="partial-types"></a>分部类型  

可以在一个代码文件中定义类、结构或方法的一部分，并在其他代码文件中定义另一部分。 有关详细信息，请参阅[分部类和方法](./partial-classes-and-methods.md)。
  
## <a name="object-initializers"></a>对象初始值设定项  

无需显式调用构造函数，即可实例化和初始化类或结构对象和对象集合。 有关详细信息，请参阅[对象和集合初始值设定项](./object-and-collection-initializers.md)。  
  
## <a name="anonymous-types"></a>匿名类型  

如果不方便或没有必要创建已命名的类（例如，使用无需保留或传递给其他方法的数据结构填充列表时），可以使用匿名类型。 有关详细信息，请参阅[匿名类型](./anonymous-types.md)。
  
## <a name="extension-methods"></a>扩展方法  

可以单独创建类型（其方法可以调用，就像它们属于原始类型一样）来“扩展”类，而无需创建派生类。 有关详细信息，请参阅[扩展方法](./extension-methods.md)。
  
## <a name="implicitly-typed-local-variables"></a>隐式类型的局部变量  

在类或结构方法中，可以使用隐式类型指示编译器在编译时确定变量类型。 有关详细信息，请参阅[隐式类型局部变量](./implicitly-typed-local-variables.md)。

## <a name="records"></a>记录

C# 9 引入了 `record` 类型，可创建此引用类型而不创建类或结构。 记录是带有内置行为的类，用于将数据封装在不可变类型中。 记录提供以下功能：

* 用于创建具有不可变属性的引用类型的简明语法。

* 值相等性。

  两个记录类型的变量在它们的类型和值都相同时，它们是相等的。 记录与类不同，类使用引用相等性，即：如果两个类类型的变量引用相同的对象，则它们是相等的。

* 非破坏性变化的简明语法。

  使用 `with` 表达式，可以创建作为现有实例副本的新记录实例，但更改了指定的属性值。

* 显示的内置格式设置。

  `ToString` 方法输出记录类型名称以及公共属性的名称和值。

* 支持继承层次结构。

  支持继承，因为记录的本质是类，而不是结构。

有关详细信息，请参阅[记录](../../language-reference/builtin-types/record.md)。

## <a name="c-language-specification"></a>C# 语言规范  

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a>请参阅

- [类](./classes.md)
- [对象](./objects.md)
- [结构类型](../../language-reference/builtin-types/struct.md)
- [记录](../../language-reference/builtin-types/record.md)  
- [C# 编程指南](../index.md)
