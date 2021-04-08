---
title: C# 程序的构建基块
description: 了解 C# 成员、表达式和语句。 类型包含你编写的成员。 这些成员是根据语句和表达式生成的。
ms.date: 08/06/2020
ms.openlocfilehash: 87410697bbed305c746f1440cca61dcbe53f88a7
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288051"
---
# <a name="program-building-blocks"></a>程序构建基块

上文中介绍的类型是使用以下构建基块生成的：[成员*_](../programming-guide/classes-and-structs/members.md)、[表达式和语句_*](../programming-guide/statements-expressions-operators/index.md)。

## <a name="members"></a>成员

`class` 的成员要么是静态成员，要么是实例成员。 静态成员属于类，而实例成员则属于对象（类实例）。

以下列表概述了类可以包含的成员类型。

- **常量**：与类相关联的常量值
- **字段**：与类关联的变量
- **方法**：类可执行的操作
- **属性**：与读取和写入类的已命名属性相关联的操作
- **索引器**：与将类实例编入索引（像处理数组一样）相关联的操作
- **事件**：类可以生成的通知
- **运算符**：类支持的转换和表达式运算符
- **构造函数**：初始化类实例或类本身所需的操作
- **终结器**：永久放弃类实例之前执行的操作
- **类型**：类声明的嵌套类型

## <a name="accessibility"></a>辅助功能

每个类成员都有关联的可访问性，用于控制能够访问成员的程序文本区域。 可访问性有六种可能的形式。 以下内容对访问修饰符进行了汇总。

- `public`：访问不受限制。
- `private`：访问仅限于此类。
- `protected`：访问仅限于此类或派生自此类的类。
- `internal`：仅可访问当前程序集（`.exe` 或 `.dll`）。
- `protected internal`：仅可访问此类、从此类中派生的类，或者同一程序集中的类。
- `private protected`：仅可访问此类或同一程序集中从此类中派生的类。

## <a name="fields"></a>字段

*字段* 是与类或类实例相关联的变量。

使用静态修饰符声明的字段定义的是静态字段。 静态字段只指明一个存储位置。 无论创建多少个类实例，永远只有一个静态字段副本。

不使用静态修饰符声明的字段定义的是实例字段。 每个类实例均包含相应类的所有实例字段的单独副本。

在以下示例中，每个 `Color` 类实例均包含 `R`、`G` 和 `B` 实例字段的单独副本，但只包含 `Black`、`White`、`Red`、`Green` 和 `Blue` 静态字段的一个副本：

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ColorClassDefinition":::

如上面的示例所示，可以使用 `readonly` 修饰符声明 *只读字段*。 只能在字段声明期间或在同一个类的构造函数中向只读字段赋值。

## <a name="methods"></a>方法

*方法* 是实现对象或类可执行的计算或操作的成员。 *静态方法* 是通过类进行访问。 *实例方法* 是通过类实例进行访问。

方法可能包含一个参数列表，这些参数表示传递给方法的值或变量引用。 方法具有返回类型，它用于指定方法计算和返回的值的类型。 如果方法未返回值，则它的返回类型为 `void`。

方法可能也包含一组类型参数，必须在调用方法时指定类型自变量，这一点与类型一样。 与类型不同的是，通常可以根据方法调用的自变量推断出类型自变量，无需显式指定。

在声明方法的类中，方法的 *签名* 必须是唯一的。 方法签名包含方法名称、类型参数数量及其参数的数量、修饰符和类型。 方法签名不包含返回类型。

当方法主体是单个表达式时，可使用紧凑表达式格式定义方法，如下例中所示：

```csharp
public override string ToString() => "This is an object";
```

### <a name="parameters"></a>参数

参数用于将值或变量引用传递给方法。 方法参数从调用方法时指定的 *自变量* 中获取其实际值。 有四类参数：值参数、引用参数、输出参数和参数数组。

值参数用于传递输入自变量。 值参数对应于局部变量，从为其传递的自变量中获取初始值。 修改值形参不会影响为其传递的实参。

可以指定默认值，从而省略相应的自变量，这样值参数就是可选的。

引用参数用于按引用传递自变量。 为引用参数传递的自变量必须是一个带有明确值的变量。 在方法执行期间，引用参数指出的存储位置与自变量相同。 引用参数使用 `ref` 修饰符进行声明。 下面的示例展示了如何使用 `ref` 参数。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="RefExample":::

输出参数用于按引用传递自变量。 输出参数与引用参数类似，不同之处在于，不要求向调用方提供的自变量显式赋值。 输出参数使用 `out` 修饰符进行声明。 下面的示例演示如何通过 C# 7 中引入的语法使用 `out` 参数。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="OutExample":::

*参数数组* 允许向方法传递数量不定的自变量。 参数数组使用 `params` 修饰符进行声明。 参数数组只能是方法的最后一个参数，且参数数组的类型必须是一维数组类型。 <xref:System.Console?displayProperty=nameWithType> 类的 `Write` 和 `WriteLine` 方法是参数数组用法的典型示例。 它们的声明方式如下。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ConsoleExtract":::

在使用参数数组的方法中，参数数组的行为与数组类型的常规参数完全相同。 不过，在调用包含形参数组的方法时，要么可以传递形参数组类型的一个实参，要么可以传递形参数组的元素类型的任意数量实参。 在后一种情况中，数组实例会自动创建，并初始化为包含给定的自变量。 以下示例：

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UseParamsArgs":::

等同于编写以下代码：

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="CompilerParams":::

### <a name="method-body-and-local-variables"></a>方法主体和局部变量

方法主体指定了在调用方法时执行的语句。

方法主体可以声明特定于方法调用的变量。 此类变量称为 *局部变量*。 局部变量声明指定了类型名称、变量名称以及可能的初始值。 下面的示例声明了初始值为零的局部变量 `i` 和无初始值的局部变量 `j`。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="SquaresClass":::

C# 要求必须先 *明确赋值* 局部变量，然后才能获取其值。 例如，如果上述 `i` 的声明未包含初始值，那么编译器会在后续使用 `i` 时报告错误，因为在后续使用时 `i` 不会在程序中得到明确赋值。

方法可以使用 `return` 语句将控制权返回给调用方。 在返回 `void` 的方法中，`return` 语句无法指定表达式。 在不返回 void 的方法中，`return` 语句必须包括用于计算返回值的表达式。

### <a name="static-and-instance-methods"></a>静态和实例方法

使用 `static` 修饰符声明的方法是静态方法。 静态方法不对特定的实例起作用，只能直接访问静态成员。

未使用 `static` 修饰符声明的方法是实例方法。 实例方法对特定的实例起作用，并能够访问静态和实例成员。 其中调用实例方法的实例可以作为 `this` 显式访问。 在静态方法中引用 `this` 会生成错误。

以下 `Entity` 类包含静态和实例成员。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="EntityClass":::

每个 `Entity` 实例均有一个序列号（很可能包含此处未显示的其他一些信息）。 `Entity` 构造函数（类似于实例方法）将新实例初始化为包含下一个可用的序列号。 由于构造函数是实例成员，因此可以访问 `_serialNo` 实例字段和 `s_nextSerialNo` 静态字段。

`GetNextSerialNo` 和 `SetNextSerialNo` 静态方法可以访问 `s_nextSerialNo` 静态字段，但如果直接访问 `_serialNo` 实例字段，则会生成错误。

下例显示了 `Entity` 类的用法。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UsingEntity":::

`SetNextSerialNo` 和 `GetNextSerialNo` 静态方法在类中进行调用，而 `GetSerialNo` 实例方法则是在类实例中进行调用。

### <a name="virtual-override-and-abstract-methods"></a>虚方法、重写方法和抽象方法

如果实例方法声明中有 `virtual` 修饰符，可以将实例方法称为“*虚方法*”。 如果没有 virtual 修饰符，可以将实例方法称为“*非虚方法*”。

调用虚方法时，为其调用方法的实例的 *运行时类型* 决定了要调用的实际方法实现代码。 调用非虚方法时，实例的 *编译时类型* 是决定性因素。

可以在派生类中 *重写* 虚方法。 如果实例方法声明中有 override 修饰符，那么实例方法可以重写签名相同的继承虚方法。 虚方法声明引入了新方法。 重写方法声明通过提供现有继承的虚方法的新实现，专门针对该方法。

*抽象方法* 是没有实现代码的虚方法。 抽象方法使用 `abstract` 修饰符进行声明，仅可在抽象类中使用。 必须在所有非抽象派生类中重写抽象方法。

下面的示例声明了一个抽象类 `Expression`，用于表示表达式树节点；还声明了三个派生类（`Constant`、`VariableReference` 和 `Operation`），用于实现常量、变量引用和算术运算的表达式树节点。 （该示例与表达式树类型相似，但与它无关）。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="WorkingWithExpressions":::

上面的四个类可用于进行算术表达式建模。 例如，使用这些类的实例，可以按如下方式表示表达式 `x + 3`。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UseExpressions":::

调用 `Expression` 实例的 `Evaluate` 方法可以计算给定的表达式并生成 `double` 值。 此方法需要使用自变量 `Dictionary`，其中包含变量名称（作为项键）和值（作为项值）。 因为 `Evaluate` 是一个抽象方法，因此派生自 `Expression` 的非抽象类必须替代 `Evaluate`。

`Constant` 的 `Evaluate` 实现代码只返回存储的常量。 `VariableReference` 实现代码查找字典中的变量名称，并返回结果值。 `Operation` 实现代码先计算左右操作数（以递归方式调用其 `Evaluate` 方法），然后执行给定的算术运算。

以下程序使用 `Expression` 类根据不同的 `x` 和 `y` 值计算表达式 `x * (y + 2)`。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="UsingExpressions":::

### <a name="method-overloading"></a>方法重载

借助方法 *重载*，同一类中可以有多个同名的方法，只要这些方法具有唯一签名即可。 编译如何调用重载的方法时，编译器使用 *重载决策* 来确定要调用的特定方法。 重载决策会查找与自变量匹配度最高的一种方法。 如果找不到任何最佳匹配项，则会报告错误。 下面的示例展示了重载决策的实际工作方式。 `UsageExample` 方法中每个调用的注释指明了调用的方法。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="Overloading":::

如示例所示，可将自变量显式转换成确切的参数类型和类型自变量，随时选择特定的方法。

## <a name="other-function-members"></a>其他函数成员

包含可执行代码的成员统称为类的 *函数成员*。 上一部分介绍了作为主要函数成员类型的方法。 此部分将介绍 C# 支持的其他类型函数成员：构造函数、属性、索引器、事件、运算符和终结器。

下面的示例展示了 `MyList<T>` 泛型类，用于实现对象的可扩充列表。 此类包含最常见类型函数成员的多个示例。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListExample":::

### <a name="constructors"></a>构造函数

C# 支持实例和静态构造函数。 *实例构造函数* 是实现初始化类实例所需执行的操作的成员。 静态构造函数是实现在首次加载类时初始化类本身所需执行的操作的成员。

构造函数的声明方式与方法一样，都没有返回类型，且与所含类同名。 如果构造函数声明包含 `static` 修饰符，则声明的是静态构造函数。 否则，声明的是实例构造函数。

实例构造函数可重载并且可具有可选参数。 例如，`MyList<T>` 类声明一个具有单个可选 `int` 参数的实例构造函数。 实例构造函数使用 `new` 运算符进行调用。 下面的语句使用包含和不包含可选自变量的 `MyList` 类构造函数来分配两个 `MyList<string>` 实例。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="CreateLists":::

与其他成员不同，实例构造函数不会被继承。 类中只能包含实际上已在该类中声明的实例构造函数。 如果没有为类提供实例构造函数，则会自动提供不含参数的空实例构造函数。

### <a name="properties"></a>“属性”

*属性* 是字段的自然扩展。 两者都是包含关联类型的已命名成员，用于访问字段和属性的语法也是一样的。 不过，与字段不同的是，属性不指明存储位置。 相反，属性包含访问器，用于指定在读取或写入属性值时执行的语句。

属性的声明方式与字段相似，区别是属性声明以在分隔符 `{` 和 `}` 之间写入的 get 访问器或 set 访问器结束，而不是以分号结束。 同时具有 get 访问器和 set 访问器的属性是“读写属性”。 只有 get 访问器的属性是“只读属性”。 只有 set 访问器的属性是“只写属性”。

get 访问器对应于包含属性类型的返回值的无参数方法。 set 访问器对应于包含一个名为 value 的参数但不含返回类型的方法。 get 访问器会计算属性的值。 set 访问器会为属性提供新值。 当属性是赋值的目标，或者是 `++` 或 `--` 的操作数时，会调用 set 访问器。 在引用了属性的其他情况下，会调用 get 访问器。

`MyList<T>` 类声明以下两个属性：`Count` 和 `Capacity`（分别为只读和读写）。 以下示例代码展示了如何使用这些属性：

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="AccessProperties":::

类似于字段和方法，C# 支持实例属性和静态属性。 静态属性使用静态修饰符进行声明，而实例属性则不使用静态修饰符进行声明。

属性的访问器可以是虚的。 如果属性声明包含 `virtual`、`abstract` 或 `override` 修饰符，则适用于属性的访问器。

### <a name="indexers"></a>索引器

借助 *索引器* 成员，可以将对象编入索引（像处理数组一样）。 索引器的声明方式与属性类似，不同之处在于，索引器成员名称格式为 `this` 后跟在分隔符 `[` 和 `]` 内写入的参数列表。 这些参数在索引器的访问器中可用。 类似于属性，索引器分为读写、只读和只写索引器，且索引器的访问器可以是虚的。

`MyList<T>` 类声明一个需要使用 `int` 参数的读写索引器。 借助索引器，可以使用 `int` 值将 `MyList<T>` 实例编入索引。 例如：

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListAccess":::

索引器可被重载。 一个类可声明多个索引器，只要其参数的数量或类型不同即可。

### <a name="events"></a>事件

借助 *事件* 成员，类或对象可以提供通知。 事件的声明方式与字段类似，区别是事件声明包括 `event` 关键字，且类型必须是委托类型。

在声明事件成员的类中，事件的行为与委托类型的字段完全相同（前提是事件不是抽象的，且不声明访问器）。 字段存储对委托的引用，委托表示已添加到事件的事件处理程序。 如果没有任何事件处理程序，则字段为 `null`。

`MyList<T>` 类声明一个 `Changed` 事件成员，指明已向列表添加了新项。 Changed 事件由 `OnChanged` 虚方法引发，此方法会先检查事件是否是 `null`（即不含任何处理程序）。 引发事件的概念恰恰等同于调用由事件表示的委托。 不存在用于引发事件的特殊语言构造。

客户端通过 *事件处理程序* 响应事件。 使用 `+=` 和 `-=` 运算符分别可以附加和删除事件处理程序。 下面的示例展示了如何向 `MyList<string>` 的 `Changed` 事件附加事件处理程序。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="RespondToEvents":::

对于需要控制事件的基础存储的高级方案，事件声明可以显式提供 `add` 和 `remove` 访问器，这与属性的 `set` 访问器类似。

### <a name="operators"></a>运算符

*运算符* 是定义向类实例应用特定表达式运算符的含义的成员。 可以定义三种类型的运算符：一元运算符、二元运算符和转换运算符。 所有运算符都必须声明为 `public` 和 `static`。

`MyList<T>` 类会声明两个运算符：`operator ==` 和 `operator !=`。 对于向 `MyList` 实例应用这些运算符的表达式来说，这些重写的运算符向它们赋予了新的含义。 具体而言，这些运算符定义的是两个 `MyList<T>` 实例的相等性（使用其 `Equals` 方法比较所包含的每个对象）。 下面的示例展示了如何使用 `==` 运算符比较两个 `MyList<int>` 实例。

:::code language="csharp" source="./snippets/shared/ClassesObjects.cs" ID="ListAddition":::

第一个 `Console.WriteLine` 输出 `True`，因为两个列表包含的对象不仅数量相同，而且值和顺序也相同。 如果 `MyList<T>` 未定义 `operator ==`，那么第一个 `Console.WriteLine` 会输出 `False`，因为 `a` 和 `b` 引用不同的 `MyList<int>` 实例。

### <a name="finalizers"></a>终结器

*终结器* 是实现完成类实例所需的操作的成员。 通常，需要使用终结器来释放非托管资源。 终结器既不能包含参数和可访问性修饰符，也不能进行显式调用。 实例的终结器在垃圾回收期间自动调用。 有关详细信息，请参阅[终结器](../programming-guide/classes-and-structs/destructors.md)一文。

垃圾回收器在决定何时收集对象和运行终结器时有很大自由度。 具体而言，终结器的调用时间具有不确定性，可以在任意线程上执行终结器。 因为这样或那样的原因，只有在没有其他可行的解决方案时，类才能实现终结器。

处理对象析构的更好方法是使用 `using` 语句。

## <a name="expressions"></a>表达式

*表达式* 是在 *操作数* 和 *运算符* 的基础之上构造而成。 表达式的运算符指明了向操作数应用的运算。 运算符的示例包括 `+`、`-`、`*`、`/` 和 `new`。 操作数的示例包括文本、字段、局部变量和表达式。

如果某个表达式包含多个运算符，则运算符的优先顺序控制各个运算符的计算顺序。 例如，表达式 `x + y * z` 相当于计算 `x + (y * z)`，因为 `*` 运算符的优先级高于 `+` 运算符。

如果操作数两边的两个运算符的优先级相同，那么运算符的 *结合性* 决定了运算的执行顺序：

* 除了赋值运算符和 null 合并运算符之外，所有二元运算符均为左结合运算符，即从左向右执行运算。 例如，`x + y + z` 将计算为 `(x + y) + z`。
* 赋值运算符、null 合并 `??` 和 `??=` 运算符和条件运算符 `?:` 为右结合运算符，即从右向左执行运算。 例如，`x = y = z` 将计算为 `x = (y = z)`。

可以使用括号控制优先级和结合性。 例如，`x + y * z` 先计算 `y` 乘 `z`，并将结果与 `x` 相加，而 `(x + y) * z` 则先计算 `x` 加 `y`，然后将结果与 `z` 相乘。

大部分运算符可 [*重载*](../language-reference/operators/operator-overloading.md)。 借助运算符重载，可以为一个或两个操作数为用户定义类或结构类型的运算指定用户定义运算符实现代码。

C# 提供多个运算符用于执行[算术](../language-reference/operators/arithmetic-operators.md)、[逻辑](../language-reference/operators/boolean-logical-operators.md)、[按位和移位](../language-reference/operators/bitwise-and-shift-operators.md)运算以及[相等](../language-reference/operators/equality-operators.md)和[排序](../language-reference/operators/comparison-operators.md)比较。

要了解按优先级排序的完整 C# 运算符列表，请参阅 [C# 运算符](../language-reference/operators/index.md)。

## <a name="statements"></a>语句

程序操作使用 *语句* 进行表示。 C# 支持几种不同的语句，其中许多语句是从嵌入语句的角度来定义的。

- 使用 *代码块*，可以在允许编写一个语句的上下文中编写多个语句。 代码块是由一系列在分隔符 `{` 和 `}` 内编写的语句组成。
- *声明语句* 用于声明局部变量和常量。
- *表达式语句* 用于计算表达式。 可用作语句的表达式包括方法调用、使用 `new` 运算符的对象分配、使用 `=` 和复合赋值运算符的赋值、使用 `++` 和 `--` 运算符和 `await` 表达式的递增和递减运算。
- *选择语句* 用于根据一些表达式的值从多个可能的语句中选择一个以供执行。 此组包含 `if` 和 `switch` 语句。
- *迭代语句* 用于重复执行嵌入语句。 此组包含 `while`、`do`、`for` 和 `foreach` 语句。
- *跳转语句* 用于转移控制权。 此组包含 `break`、`continue`、`goto`、`throw`、`return` 和 `yield` 语句。
- `try`...`catch` 语句用于捕获在代码块执行期间发生的异常，`try`...`finally` 语句用于指定始终执行的最终代码，无论异常发生与否。
- `checked` 和 `unchecked` 语句用于控制整型类型算术运算和转换的溢出检查上下文。
- `lock` 语句用于获取给定对象的相互排斥锁定，执行语句，然后解除锁定。
- `using` 语句用于获取资源，执行语句，然后释放资源。

下面列出了可使用的语句类型：

* 局部变量声明。
* 局部常量声明。
* 表达式语句。
* `if` 语句。
* `switch` 语句。
* `while` 语句。
* `do` 语句。
* `for` 语句。
* `foreach` 语句。
* `break` 语句。
* `continue` 语句。
* `goto` 语句。
* `return` 语句。
* `yield` 语句。
* `throw` 和 `try` 语句。
* `checked` 和 `unchecked` 语句。
* `lock` 语句。
* `using` 语句。

>[!div class="step-by-step"]
>[上一页](types.md)
>[下一页](features.md)
