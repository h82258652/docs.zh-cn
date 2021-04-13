---
title: 符号和运算符参考
description: 了解 F# 编程语言中使用的符号和运算符。
ms.date: 08/15/2020
fl_keywords:
- '|>_FS'
ms.openlocfilehash: 2fddf538f007a971ee369b9118f02deb8408d3a6
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075545"
---
# <a name="symbol-and-operator-reference"></a>符号和运算符参考

本文提供 F# 语言中使用的符号和运算符表。

## <a name="table-of-symbols-and-operators"></a>符号和运算符表

下表描述 F# 语言中使用的符号，简要介绍符号的一些用途，并提供更多信息的链接。 符号按 ASCII 字符集顺序排序。

|符号或运算符|链接|描述|
|------------------|-----|-----------|
|`!`|[引用单元格](../reference-cells.md)<br /><br />[计算表达式](../computation-expressions.md)|<ul><li>取消引用引用单元格。<br /></li><li>在关键字后，指示由工作流控制的关键字行为的修改版本。<br /></li></ul>|
|`!=`||<ul><li>F# 中不使用。 使用 `<>` 进行不等运算。<br /></li></ul>|
|`"`|[文字](../literals.md)<br /><br />[字符串](../strings.md)|<ul><li>分隔文本字符串。<br /></li></ul>|
|`"""`|[字符串](../strings.md)|分隔逐字文本字符串。 区别你可以在字符串中使用单引号来指示引号字符的 `@"..."`。|
|`#`|[编译器指令](../compiler-directives.md)<br /><br />[可变类型](../flexible-types.md)|<ul><li>给预处理器指令或编译器指令加上前缀，如 `#light`。<br /></li><li>当用于类型时，指示“灵活类型”，即某一类型或该类型的任何一个派生类型。<br /></li></ul>|
|`$`||<ul><li>在内部用于特定编译器生成的变量和函数名称。<br /></li></ul>|
|`%`|[算术运算符](arithmetic-operators.md)<br /><br />[代码引用](../code-quotations.md)|<ul><li>计算整数余数。<br /></li><li>用于将表达式拼接到类型化的代码引号中。<br /></li></ul>|
|`%%`|[代码引用](../code-quotations.md)|<ul><li>用于将表达式拼接到非类型化的代码引号中。<br /></li></ul>|
|`%?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算整数余数。<br /></li></ul>|
|`&`|[Match 表达式](../match-expressions.md)|<ul><li>计算可变值的地址，以便在与其他语言进行互操作时使用。<br /></li><li>在“与”模式下使用。<br /></li></ul>|
|`&&`|[布尔运算符](boolean-operators.md)|<ul><li>计算“布尔与”运算。<br /></li></ul>|
|`&&&`|[位运算符](bitwise-operators.md)|<ul><li>计算“位与”运算。<br /></li></ul>|
|`'`|[文字](../literals.md)<br /><br />[自动泛化](../generics/automatic-generalization.md)|<ul><li>分隔单字符文本。<br /></li><li>指示泛型类型参数。<br /></li></ul>|
|<code>&#96;&#96;...&#96;&#96;</code>||<ul><li>分隔标识符，否则它将不是合法的标识符，如语言关键字。<br /></li></ul>|
|`( )`|[unit 类型](../unit-type.md)|<ul><li>表示单位类型的单个值。<br /></li></ul>|
|`(...)`|[元组](../tuples.md)<br /><br />[运算符重载](../operator-overloading.md)|<ul><li>指示表达式的计算顺序。<br /></li><li>分隔元组。<br /></li><li>在运算符定义中使用。<br /></li></ul>|
|`(*...*)`||<ul><li>分隔跨多行的注释。<br /></li></ul>|
|<code>(&#124;...&#124;)</code>|[活动模式](../active-patterns.md)|<ul><li>分隔活动模式。 也称为“香蕉夹”。<br /></li></ul>|
|`*`|[算术运算符](arithmetic-operators.md)<br /><br />[元组](../tuples.md)<br /><br />[度量单位](../units-of-measure.md)|<ul><li>当用作二元运算符时，使左右两侧相乘。<br /></li><li>在类型中，指示元组中的配对。<br /></li><li>用于度量单位类型。<br /></li></ul>|
|`*?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，使左右两侧相乘。<br /></li></ul>|
|`**`|[算术运算符](arithmetic-operators.md)|<ul><li>计算幂运算（`x ** y` 表示 `x` 的 `y` 次方）。<br /></li></ul>|
|`+`|[算术运算符](arithmetic-operators.md)|<ul><li>当用作二元运算符时，使左右两侧相加。<br /></li><li>当用作二元运算符时，指示正数。 （正式情况下，它生成相同的值且符号保持不变。）<br /></li></ul>|
|`+?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，使左右两侧相加。<br /></li></ul>|
|`,`|[元组](../tuples.md)|<ul><li>分隔元组的元素或类型参数。<br /></li></ul>|
|`-`|[算术运算符](arithmetic-operators.md)|<ul><li>当用作二元运算符时，使左侧减去右侧。<br /></li><li>当用作二元运算符时，执行求反运算。<br /></li></ul>|
|`-?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，使左侧减去右侧。<br /></li></ul>|
|`->`|[函数](../functions/index.md)<br /><br />[Match 表达式](../match-expressions.md)|<ul><li>在函数类型中，分隔自变量和返回值。<br /></li><li>生成表达式（序列表达式）；等效于 `yield` 关键字。<br /></li><li>在匹配表达式中使用<br /></li></ul>|
|`.`|[成员](../members/index.md)<br /><br />[基元类型](../basic-types.md)|<ul><li>访问成员并分隔完全限定名称中的各个名称。<br /></li><li>指定浮点数中的小数点。<br /></li></ul>|
|`..`|[循环：`for...in` 表达式](../loops-for-in-expression.md)|<ul><li>指定范围。<br /></li></ul>|
|`.. ..`|[循环：`for...in` 表达式](../loops-for-in-expression.md)|<ul><li>指定范围和增量。<br /></li></ul>|
|`.[...]`|[数组](../arrays.md)|<ul><li>访问数组元素。<br /></li></ul>|
|`/`|[算术运算符](arithmetic-operators.md)<br /><br />[度量单位](../units-of-measure.md)|<ul><li>使左侧（分子）除以右侧（分母）。<br /></li><li>用于度量单位类型。<br /></li></ul>|
|`/?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，使左侧除以右侧。<br /></li></ul>|
|`//`||<ul><li>指示单行注释的开头。<br /></li></ul>|
|`///`|[XML 文档](../xml-documentation.md)|<ul><li>指示 XML 注释。<br /></li></ul>|
|`:`|[函数](../functions/index.md)|<ul><li>在类型批注中，用于将参数或成员名称与其类型分隔开来。<br /></li></ul>|
|`::`|[列表](../lists.md)<br /><br />[Match 表达式](../match-expressions.md)|<ul><li>创建列表。 将左侧的元素追加到右侧的列表。<br /></li><li>在模式匹配中用于分隔列表的各个部分。<br /></li></ul>|
|`:=`|[引用单元格](../reference-cells.md)|<ul><li>对引用单元格赋值。<br /></li></ul>|
|`:>`|[强制转换和转换](../casting-and-conversions.md)|<ul><li>将类型转换为层次结构中级别更高的类型。<br /></li></ul>|
|`:?`|[Match 表达式](../match-expressions.md)|<ul><li>如果值与指定类型（包括子类型）匹配，则返回 `true`；否则，返回 `false`（类型测试运算符）。<br /></li></ul>|
|`:?>`|[强制转换和转换](../casting-and-conversions.md)|<ul><li>将类型转换为层次结构中级别更低的类型。<br /></li></ul>|
|`;`|[详细语法](../verbose-syntax.md)<br /><br />[列表](../lists.md)<br /><br />[记录](../records.md)|<ul><li>分隔表达式（主要用于详细语法）。<br /></li><li>分隔列表中的元素。<br /></li><li>分隔记录的字段。<br /></li></ul>|
|`<`|[算术运算符](arithmetic-operators.md)|<ul><li>计算“小于”运算。<br /></li></ul>|
|`<?`|[可以为 null 的运算符](nullable-operators.md)|当右侧是可以为 null 的类型时，计算“小于”运算。|
|`<<`|[函数](../functions/index.md)|<ul><li>以相反的顺序组合两个函数；先执行第二个函数（反向组合运算符）。<br /></li></ul>|
|`<<<`|[位运算符](bitwise-operators.md)|<ul><li>将左侧的位数向左移动右侧指定的位数。<br /></li></ul>|
|`<-`|[值](../values/index.md)|<ul><li>对变量赋值。<br /></li></ul>|
|`<...>`|[自动泛化](../generics/automatic-generalization.md)|<ul><li>分隔类型参数。<br /></li></ul>|
|`<>`|[算术运算符](arithmetic-operators.md)|<ul><li>如果左侧不等于右侧，则返回 `true`；否则，返回 false。<br /></li></ul>|
|`<>?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算“不等于”运算。<br /></li></ul>|
|`<=`|[算术运算符](arithmetic-operators.md)|<ul><li>如果左侧小于或等于右侧，则返回 `true`；否则返回 `false`。<br /></li></ul>|
|`<=?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算“小于或等于”运算。<br /></li></ul>|
|<code>&#124;></code>|[函数](../functions/index.md)|<ul><li>将左侧表达式的结果传递给右侧的函数（前置竖线运算符）。<br /></li></ul>|
|<code>&#124;&#124;></code>|[&#40; &#124;&#124;&#62; &#41;&#60;'T1,'T2,'U&#62; 函数](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#(%20%7C%7C%3E%20))|<ul><li>将左侧两个自变量的元组传递给右侧的函数。<br /></li></ul>|
|<code>&#124;&#124;&#124;></code>|[&#40; &#124;&#124;&#124;&#62; &#41;&#60;'T1,'T2,'T3,'U&#62; 函数](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#(%20%7C%7C%7C%3E%20))|<ul><li>将左侧三个自变量的元组传递给右侧的函数。<br /></li></ul>|
|<code>&lt;&#124;</code>|[函数](../functions/index.md)|<ul><li>将左侧表达式的结果传递给右侧的函数（后置竖线运算符）。<br /></li></ul>|
|<code>&lt;&#124;&#124;</code>|[&#40; &#60;&#124;&#124; &#41;&#60;'T1,'T2,'U&#62; 函数](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#(%20%3C%7C%7C%20))|<ul><li>将右侧两个自变量的元组传递给左侧的函数。<br /></li></ul>|
|<code>&lt;&#124;&#124;&#124;</code>|[&#40; &#60;&#124;&#124;&#124; &#41;&#60;'T1,'T2,'T3,'U&#62; 函数](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-operators.html#(%20%3C%7C%7C%7C%20))|<ul><li>将右侧三个自变量的元组传递给左侧的函数。<br /></li></ul>|
|`<@...@>`|[代码引用](../code-quotations.md)|<ul><li>分隔类型化代码引号。<br /></li></ul>|
|`<@@...@@>`|[代码引用](../code-quotations.md)|<ul><li>分隔非类型化代码引号。<br /></li></ul>|
|`=`|[算术运算符](arithmetic-operators.md)|<ul><li>如果左侧等于右侧，则返回 `true`；否则返回 `false`。<br /></li></ul>|
|`=?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算“等于”运算。<br /></li></ul>|
|`==`||<ul><li>F# 中不使用。 使用 `=` 进行相等运算。<br /></li></ul>|
|`>`|[算术运算符](arithmetic-operators.md)|<ul><li>如果左侧大于右侧，则返回 `true`；否则返回 `false`。<br /></li></ul>|
|`>?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算“大于”运算。<br /></li></ul>|
|`>>`|[函数](../functions/index.md)|<ul><li>组合两个函数（正向组合运算符）。<br /></li></ul>|
|`>>>`|[位运算符](bitwise-operators.md)|<ul><li>将左侧的位数向右移动右侧指定的位数。<br /></li></ul>|
|`>=`|[算术运算符](arithmetic-operators.md)|<ul><li>如果左侧大于或等于右侧，则返回 `true`；否则返回 `false`。<br /></li></ul>|
|`>=?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>当右侧是可以为 null 的类型时，计算“大于或等于”运算。<br /></li></ul>|
|`?`|[形参和实参](../parameters-and-arguments.md)|<ul><li>指定可选自变量。<br /></li><li>用作动态方法和属性调用的运算符。 你必须提供自己的实现。<br /></li></ul>|
|`? ... <- ...`||<ul><li>用作设置动态属性的运算符。 你必须提供自己的实现。<br /></li></ul>|
|`?>=`, `?>`, `?<=`, `?<`, `?=`, `?<>`, `?+`, `?-`, `?*`, `?/`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>如果左侧是可以为 null 的类型，等效于不带 ?  前缀的对应运算符。<br /></li></ul>|
|`>=?`, `>?`, `<=?`, `<?`, `=?`, `<>?`, `+?`, `-?`, `*?`, `/?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>如果左侧是可以为 null 的类型，等效于不带 ?  后缀的对应运算符。<br /></li></ul>|
|`?>=?`, `?>?`, `?<=?`, `?<?`, `?=?`, `?<>?`, `?+?`, `?-?`, `?*?`, `?/?`|[可以为 null 的运算符](nullable-operators.md)|<ul><li>如果两侧都是可以为 null 的类型，等效于周围没有问号的对应运算符。<br /></li></ul>|
|`@`|[列表](../lists.md)<br /><br />[字符串](../strings.md)|<ul><li>连接两个列表。<br /></li><li>当放置在字符串文本之前时，指示将逐字解释字符串，而不会解释转义符。<br /></li></ul>|
|`[...]`|[列表](../lists.md)|<ul><li>分隔列表中的元素。<br /></li></ul>|
|<code>[&#124;...&#124;]</code>|[数组](../arrays.md)|<ul><li>分隔数组中的元素。<br /></li></ul>|
|`[<...>]`|[特性](../attributes.md)|<ul><li>分隔特性。<br /></li></ul>|
|`\`|[字符串](../strings.md)|<ul><li>对下一个字符转义，在字符和字符串文本中使用。<br /></li></ul>|
|`^`|[静态解析的类型参数](../generics/statically-resolved-type-parameters.md)<br /><br />[字符串](../strings.md)|<ul><li>指定在编译时（而非运行时）必须解析的类型参数。<br /></li><li>连接字符串。<br /></li></ul>|
|`^^^`|[位运算符](bitwise-operators.md)|<ul><li>计算“位异或”运算。<br /></li></ul>|
|`_`|[Match 表达式](../match-expressions.md)<br /><br />[泛型](../generics/index.md)|<ul><li>指示通配符模式。<br /></li><li>指定匿名泛型参数。<br /></li></ul>|
|<code>&#96;</code>|[自动泛化](../generics/automatic-generalization.md)|<ul><li>在内部用于指示泛型类型参数。<br /></li></ul>|
|`{...}`|[序列](../sequences.md)<br /><br />[记录](../records.md)|<ul><li>分隔序列表达式和计算表达式。<br /></li><li>在记录定义中使用。<br /></li></ul>|
|<code>&#124;</code>|[Match 表达式](../match-expressions.md)|<ul><li>分隔单个匹配用例、单个可区分联合用例和枚举值。<br /></li></ul>|
|<code>&#124;&#124;</code>|[布尔运算符](boolean-operators.md)|<ul><li>计算“布尔或”运算。<br /></li></ul>|
|<code>&#124;&#124;&#124;</code>|[位运算符](bitwise-operators.md)|<ul><li>计算“位或”运算。<br /></li></ul>|
|`~~`|[运算符重载](../operator-overloading.md)|<ul><li>用于声明一元求反运算符的重载。<br /></li></ul>|
|`~~~`|[位运算符](bitwise-operators.md)|<ul><li>计算“位非”运算。<br /></li></ul>|
|`~-`|[运算符重载](../operator-overloading.md)|<ul><li>用于声明一元减运算符的重载。<br /></li></ul>|
|`~+`|[运算符重载](../operator-overloading.md)|<ul><li>用于声明一元加运算符的重载。<br /></li></ul>|

## <a name="operator-precedence"></a>运算符优先级

下表显示 F# 语言中运算符及其他表达式关键字的优先顺序，从最低优先级到最高优先级。 适当时，还会列出关联性。

|运算符|结合性|
|--------|-------------|
|`as`|Right|
|`when`|Right|
|<code>&#124;</code>（竖线）|Left|
|`;`|Right|
|`let`|不相关|
|`function`, `fun`, `match`, `try`|不相关|
|`if`|不相关|
|`not`|Right|
|`->`|Right|
|`:=`|Right|
|`,`|不相关|
|`or`, <code>&#124;&#124;</code>|Left|
|`&`, `&&`|Left|
|`:>`, `:?>`|Right|
|`<`*op*, `>`*op*, `=`, <code>&#124;</code>*op*, `&`*op*, `&`, `$`<br /><br />（包括 `<<<`, `>>>`, <code>&#124;&#124;&#124;</code>, `&&&`）|Left|
|`^`*op*<br /><br />（包括 `^^^`）|Right|
|`::`|Right|
|`:?`|未关联|
|`-`*op*，`+`*op*|适用于最为这些符号的中缀使用|
|`*`*op*，`/`*op*，`%`*op*|Left|
|`**`*op*|Right|
|`f x`（函数应用程序）<br /><br />（包括 `lazy x`、`assert x`）|Left|
|<code>&#124;</code>（模式匹配）|Right|
|前缀运算符（`+`*op*，`-`*op*，`%`，`%%`，`&`，`&&``!`*op*，`~`*op*）|Left|
|`.`|Left|
|`f(x)`|Left|
|`f<`*types*`>`|Left|

F# 支持自定义运算符重载。 这意味着你可以自定义自己的运算符。 在上表中，*op* 可以是任何有效的运算符字符序列（可能为空），它既可以内嵌，也可以由用户定义。 因此，你可以使用此表来确定用于自定义运算符以实现所需优先级级别的字符顺序。 当编译器确定优先级时，会忽略前面的 `.` 字符。

## <a name="see-also"></a>另请参阅

- [F# 语言参考](../index.md)
- [运算符重载](../operator-overloading.md)
