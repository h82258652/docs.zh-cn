---
title: F# 代码格式设置准则
description: '了解设置 F # 代码格式的准则。'
ms.date: 08/31/2020
ms.openlocfilehash: 81956844e8f868d428d9bdfa9ed8afed1d850628
ms.sourcegitcommit: 02cc87f02c46e603ea5925de95af746b7ab46a35
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2021
ms.locfileid: "107954804"
---
# <a name="f-code-formatting-guidelines"></a>F# 代码格式设置准则

本文提供了有关如何设置代码格式以使 F # 代码为：

* 更清晰
* 按照 Visual Studio 和其他编辑器中的格式设置工具所应用的约定进行
* 类似于其他代码 online

这些准则基于通过[Anh-Dung Phan](https://github.com/dungpa)提供的[F # 格式设置约定的综合性指南](https://github.com/dungpa/fantomas/blob/master/docs/FormattingConventions.md)。

## <a name="general-rules-for-indentation"></a>缩进的一般规则

F # 默认使用有效空白。 以下准则旨在提供有关如何调整一些可能会带来的挑战的指导。

### <a name="using-spaces"></a>使用空格

需要缩进时，必须使用空格，而不是制表符。 至少需要一个空格。 你的组织可以创建编码标准来指定要用于缩进的空格数;典型情况下，每个级别都有两个、三个或四个空格的缩进。

**建议每个缩进包含四个空格。**

也就是说，程序的缩进是一种主观上的事。 变体正常，但应遵循的第一条规则是 *缩进的一致性*。 选择一种普遍接受的缩进样式，并在代码库中系统地使用它。

## <a name="formatting-white-space"></a>设置空格的格式

F # 区分空格。 尽管通过适当的缩进涵盖了空白中的大多数语义，但还有其他一些事项需要注意。

### <a name="formatting-operators-in-arithmetic-expressions"></a>在算术表达式中设置运算符的格式

在二进制算术表达式周围始终使用空格：

```fsharp
let subtractThenAdd x = x - 1 + 3
```

一元 `-` 运算符的后面应始终紧跟它们所取消的值：

```fsharp
// OK
let negate x = -x

// Bad
let negateBad x = - x
```

在运算符后面添加空白字符可能会对 `-` 其他人造成混淆。

总而言之，始终必须：

* 围绕空格环绕二元运算符
* 一元运算符后面永远没有尾随空格

二进制算术运算符的准则尤其重要。 如果无法将二元运算符括起来 `-` ，则在与某些格式设置选项结合使用时，可能会将其解释为一元运算 `-` 。

### <a name="surround-a-custom-operator-definition-with-white-space"></a>使用空格将自定义运算符定义括起来

始终使用空格括起运算符定义：

```fsharp
// OK
let ( !> ) x f = f x

// Bad
let (!>) x f = f x
```

对于以开头并且包含多个字符的任何自定义运算符 `*` ，需要在定义的开头添加一个空格，以避免编译器歧义。 出于此原因，我们建议你只需将所有运算符的定义括起来，只包含一个空白字符。

### <a name="surround-function-parameter-arrows-with-white-space"></a>用空格将函数参数箭头括起来

定义函数的签名时，使用符号周围的空格 `->` ：

```fsharp
// OK
type MyFun = int -> int -> string

// Bad
type MyFunBad = int->int->string
```

### <a name="surround-function-arguments-with-white-space"></a>用空格将函数参数括起来

定义函数时，请在每个参数周围使用空白。

```fsharp
// OK
let myFun (a: decimal) b c = a + b + c

// Bad
let myFunBad (a:decimal)(b)c = a + b + c
```

### <a name="avoid-name-sensitive-alignments"></a>避免名称敏感度对齐

通常，请设法避免对命名敏感的缩进和对齐方式：

```fsharp
// OK
let myLongValueName =
    someExpression
    |> anotherExpression


// Bad
let myLongValueName = someExpression
                      |> anotherExpression
```

这有时称为 "虚对齐" 或 "虚缩进"。 避免这种情况的主要原因是：

* 重要的代码将向右移动
* 实际代码的宽度不变
* 重命名可能会破坏对齐

为执行相同的操作，以便 `do` / `do!` 使缩进与保持一致 `let` / `let!` 。 下面是 `do` 在类中使用的示例：

```fsharp
// OK
type Foo () =
    let foo =
        fooBarBaz
        |> loremIpsumDolorSitAmet
        |> theQuickBrownFoxJumpedOverTheLazyDog
    do
        fooBarBaz
        |> loremIpsumDolorSitAmet
        |> theQuickBrownFoxJumpedOverTheLazyDog

// Bad - notice the "do" expression is indented one space less than the `let` expression
type Foo () =
    let foo =
        fooBarBaz
        |> loremIpsumDolorSitAmet
        |> theQuickBrownFoxJumpedOverTheLazyDog
    do fooBarBaz
       |> loremIpsumDolorSitAmet
       |> theQuickBrownFoxJumpedOverTheLazyDog
```

下面是一个示例，其中 `do!` 使用了2个空格的缩进 (，因为在 `do!` 使用4个空格的缩进) 时，这两种方法之间没有任何区别：

```fsharp
// OK
async {
  let! foo =
    fooBarBaz
    |> loremIpsumDolorSitAmet
    |> theQuickBrownFoxJumpedOverTheLazyDog
  do!
    fooBarBaz
    |> loremIpsumDolorSitAmet
    |> theQuickBrownFoxJumpedOverTheLazyDog
}

// Bad - notice the "do!" expression is indented two spaces more than the `let!` expression
async {
  let! foo =
    fooBarBaz
    |> loremIpsumDolorSitAmet
    |> theQuickBrownFoxJumpedOverTheLazyDog
  do! fooBarBaz
      |> loremIpsumDolorSitAmet
      |> theQuickBrownFoxJumpedOverTheLazyDog
}
```

### <a name="place-parameters-on-a-new-line-for-long-definitions"></a>为长定义将参数放在新行上

如果使用的是长函数定义，请将参数放在新行上，并缩进它们以匹配后续参数的缩进级别。

```fsharp
module M =
    let longFunctionWithLotsOfParameters
        (aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        =
        // ... the body of the method follows

    let longFunctionWithLotsOfParametersAndReturnType
        (aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        : ReturnType =
        // ... the body of the method follows

    let longFunctionWithLongTupleParameter
        (
            aVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse
        ) =
        // ... the body of the method follows

    let longFunctionWithLongTupleParameterAndReturnType
        (
            aVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse
        ) : ReturnType =
        // ... the body of the method follows
```

这同样适用于使用元组的成员、构造函数和参数：

```fsharp
type TM() =
    member _.LongMethodWithLotsOfParameters
        (
            aVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse,
            aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse
        ) =
        // ... the body of the method

type TC
    (
        aVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse,
        aSecondVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse,
        aThirdVeryLongCtorParam: AVeryLongTypeThatYouNeedToUse
    ) =
    // ... the body of the class follows
```

如果参数为 currified，则将 `=` 字符和任何返回类型放在新行上：

```fsharp
type C() =
    member _.LongMethodWithLotsOfCurrifiedParamsAndReturnType
        (aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        : ReturnType =
        // ... the body of the method
    member _.LongMethodWithLotsOfCurrifiedParams
        (aVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aSecondVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        (aThirdVeryLongParam: AVeryLongTypeThatYouNeedToUse)
        =
        // ... the body of the method
```

这是一种避免太长的行 (的方法，如果返回类型可能有长的名称) 并且在添加参数时具有更少的行损坏。

### <a name="type-annotations"></a>类型批注

#### <a name="right-pad-function-argument-type-annotations"></a>右填充函数参数类型批注

定义具有类型批注的参数时，请使用符号后面的空格 `:` ：

```fsharp
// OK
let complexFunction (a: int) (b: int) c = a + b + c

// Bad
let complexFunctionBad (a :int) (b :int) (c:int) = a + b + c
```

#### <a name="surround-return-type-annotations-with-white-space"></a>带有空格的环绕返回类型批注

在允许绑定函数或值类型批注中 (在函数) 情况下返回类型时，请使用符号前后的空格 `:` ：

```fsharp
// OK
let expensiveToCompute : int = 0 // Type annotation for let-bound value
let myFun (a: decimal) b c : decimal = a + b + c // Type annotation for the return type of a function
// Bad
let expensiveToComputeBad1:int = 1
let expensiveToComputeBad2 :int = 2
let myFunBad (a: decimal) b c:decimal = a + b + c
```

### <a name="formatting-bindings"></a>格式绑定

在所有情况下，绑定的右侧要么都在一行上，要么 (如果太长) 则进入缩进一个范围的新行。

例如，以下内容不符合：

```fsharp
let a = """
foobar, long string
"""

type File =
    member this.SaveAsync(path: string) : Async<unit> = async {
        // IO operation
        return ()
    }

let c = {
    Name = "Bilbo"
    Age = 111
    Region = "The Shire"
}

let d = while f do
    printfn "%A" x
```

以下内容符合：

```fsharp
let a =
    """
foobar, long string
"""

type File =
    member this.SaveAsync(path: string) : Async<unit> =
        async {
            // IO operation
            return ()
        }

let c =
    { Name = "Bilbo"
      Age = 111
      Region = "The Shire" }

let d =
    while f do
        printfn "%A" x
```

## <a name="formatting-blank-lines"></a>设置空行的格式

* 用两个空行分隔顶级函数和类定义。
* 类中的方法定义由一个空行分隔。
* 可以 (慎用额外的空白行) 分离相关函数组。 可能会在一组相关的 liners (（例如，一组虚拟实现) ）之间省略空行。
* 在函数中，应慎用空白行以指示逻辑部分。

## <a name="formatting-comments"></a>设置注释格式

通常，对于 ML 样式的块注释，使用多个双斜杠注释。

```fsharp
// Prefer this style of comments when you want
// to express written ideas on multiple lines.

(*
    ML-style comments are fine, but not a .NET-ism.
    They are useful when needing to modify multi-line comments, though.
*)
```

内联注释应为首字母大写。

```fsharp
let f x = x + 1 // Increment by one.
```

## <a name="formatting-string-literals-and-interpolated-strings"></a>格式化字符串文本和内插字符串

无论行的长度如何，字符串文本和内插字符串都可以保留在单个行中。

```fsharp
let serviceStorageConnection =
    $"DefaultEndpointsProtocol=https;AccountName=%s{serviceStorageAccount.Name};AccountKey=%s{serviceStorageAccountKey.Value}"
```

强烈建议不要采用多行内插表达式。 相反，将表达式结果绑定到值并将其用于内插字符串。

## <a name="naming-conventions"></a>命名约定

### <a name="use-camelcase-for-class-bound-expression-bound-and-pattern-bound-values-and-functions"></a>将 camelCase 用于类绑定、表达式绑定以及模式绑定值和函数

常见和接受的 F # 样式使用 camelCase 作为局部变量或模式匹配和函数定义中的所有名称。

```fsharp
// OK
let addIAndJ i j = i + j

// Bad
let addIAndJ I J = I+J

// Bad
let AddIAndJ i j = i + j
```

类中的本地绑定函数还应使用 camelCase。

```fsharp
type MyClass() =

    let doSomething () =

    let firstResult = ...

    let secondResult = ...

    member x.Result = doSomething()
```

### <a name="use-camelcase-for-module-bound-public-functions"></a>对模块绑定公共函数使用 camelCase

当模块绑定函数是公共 API 的一部分时，它应使用 camelCase：

```fsharp
module MyAPI =
    let publicFunctionOne param1 param2 param2 = ...

    let publicFunctionTwo param1 param2 param3 = ...
```

### <a name="use-camelcase-for-internal-and-private-module-bound-values-and-functions"></a>将 camelCase 用于内部和私有模块绑定值和函数

将 camelCase 用于专用模块绑定值，包括以下内容：

* 脚本中的即席函数

* 构成模块或类型的内部实现的值

```fsharp
let emailMyBossTheLatestResults =
    ...
```

### <a name="use-camelcase-for-parameters"></a>将 camelCase 用于参数

所有参数应根据 .NET 命名约定使用 camelCase。

```fsharp
module MyModule =
    let myFunction paramOne paramTwo = ...

type MyClass() =
    member this.MyMethod(paramOne, paramTwo) = ...
```

### <a name="use-pascalcase-for-modules"></a>将 PascalCase 用于模块

所有模块 (顶级、内部、私有、嵌套) 应使用 PascalCase。

```fsharp
module MyTopLevelModule

module Helpers =
    module private SuperHelpers =
        ...

    ...
```

### <a name="use-pascalcase-for-type-declarations-members-and-labels"></a>对类型声明、成员和标签使用 PascalCase

类、接口、结构、枚举、委托、记录和可区分联合都应以 PascalCase 命名。 记录和可区分联合的类型和标签内的成员还应使用 PascalCase。

```fsharp
type IMyInterface =
    abstract Something: int

type MyClass() =
    member this.MyMethod(x, y) = x + y

type MyRecord = { IntVal: int; StringVal: string }

type SchoolPerson =
    | Professor
    | Student
    | Advisor
    | Administrator
```

### <a name="use-pascalcase-for-constructs-intrinsic-to-net"></a>将 PascalCase 用于 .NET 内部构造

命名空间、异常、事件和项目/ `.dll` 名称还应使用 PascalCase。 这不仅会使其他 .NET 语言的使用变得对使用者更自然，还与你可能会遇到的 .NET 命名约定一致。

### <a name="avoid-underscores-in-names"></a>避免名称中有下划线

从历史上看，某些 F # 库在名称中使用了下划线。 但是，这并不能再被广泛接受，因为它与 .NET 命名约定冲突。 也就是说，某些 F # 程序员在很大程度上使用下划线，其中部分原因是出于历史原因，而容差和尊重非常重要。 但是，这种样式通常不喜欢于对是否使用该样式有选择的其他人。

一个例外包括与本机组件互操作，其中有下划线。

### <a name="use-standard-f-operators"></a>使用标准 F # 运算符

以下运算符是在 F # 标准库中定义的，应使用而不是定义等效运算符。 建议使用这些运算符，因为这往往会使代码更具可读性和惯用。 具有 OCaml 或其他功能编程语言的开发人员可能习惯于不同的惯例。 以下列表汇总了建议的 F # 运算符。

```fsharp
x |> f // Forward pipeline
f >> g // Forward composition
x |> ignore // Discard away a value
x + y // Overloaded addition (including string concatenation)
x - y // Overloaded subtraction
x * y // Overloaded multiplication
x / y // Overloaded division
x % y // Overloaded modulus
x && y // Lazy/short-cut "and"
x || y // Lazy/short-cut "or"
x <<< y // Bitwise left shift
x >>> y // Bitwise right shift
x ||| y // Bitwise or, also for working with “flags” enumeration
x &&& y // Bitwise and, also for working with “flags” enumeration
x ^^^ y // Bitwise xor, also for working with “flags” enumeration
```

### <a name="use-prefix-syntax-for-generics-foot-in-preference-to-postfix-syntax-t-foo"></a>对泛型 (使用前缀语法 `Foo<T>`) 优先使用后缀语法 (`T Foo`) 

F # 同时继承命名泛型类型的后缀 ML 形式 (例如， `int list`) 以及前缀 .net 样式 (例如 `list<int>`) 。 除了五个特定类型外，更喜欢 .NET 样式：

1. 对于 F # 列表，请使用后缀形式： `int list` 而不是 `list<int>` 。
2. 对于 F # 选项，请使用后缀形式： `int option` 而不是 `option<int>` 。
3. 对于 F # 值选项，请使用后缀形式： `int voption` 而不是 `voption<int>` 。
4. 对于 F # 数组，请使用句法名称， `int[]` 而不是 `int array` 或 `array<int>` 。
5. 对于引用单元格，请使用 `int ref` 而不是 `ref<int>` 或 `Ref<int>` 。

对于所有其他类型，请使用前缀形式。

## <a name="formatting-tuples"></a>格式化元组

元组实例化应带有括号，其中的分隔逗号后面应跟一个空格，例如： `(1, 2)` 、 `(x, y, z)` 。

通常会接受在元组的模式匹配中省略括号：

```fsharp
let (x, y) = z // Destructuring
let x, y = z // OK

// OK
match x, y with
| 1, _ -> 0
| x, 1 -> 0
| x, y -> 1
```

如果元组是函数的返回值，则通常也可以接受省略括号：

```fsharp
// OK
let update model msg =
    match msg with
    | 1 -> model + 1, []
    | _ -> model, [ msg ]
```

总而言之，使用带括号的元组实例化，但使用元组进行模式匹配或返回值时，可以将其视为非常好的以避免使用括号。

## <a name="formatting-discriminated-union-declarations"></a>设置可区分联合声明的格式

`|`在类型定义中按四个空格缩进：

```fsharp
// OK
type Volume =
    | Liter of float
    | FluidOunce of float
    | ImperialPint of float

// Not OK
type Volume =
| Liter of float
| USPint of float
| ImperialPint of float
```

如果有单个 short 联合，则可以省略前导 `|` 。

```fsharp
type Address = Address of string
```

对于较长或多行联合，请保留 `|` 。

```fsharp
[<NoEquality; NoComparison>]
type SynBinding =
    | SynBinding of
        accessibility: SynAccess option *
        kind: SynBindingKind *
        mustInline: bool *
        isMutable: bool *
        attributes: SynAttributes *
        xmlDoc: PreXmlDoc *
        valData: SynValData *
        headPat: SynPat *
        returnInfo: SynBindingReturnInfo option *
        expr: SynExpr *
        range: range *
        seqPoint: DebugPointAtBinding
```

你还可以使用三斜杠 `///` 注释。

```fsharp
type Foobar =
    /// Code comment
    | Foobar of int
```

## <a name="formatting-discriminated-unions"></a>设置可区分联合的格式

在可区分的联合用例前面使用带圆括号/元集参数的空格：

```fsharp
// OK
let opt = Some ("A", 1)

// Not OK
let opt = Some("A", 1)
```

跨多个行拆分的实例化的可区分联合应为包含的数据提供具有缩进的新范围：

```fsharp
let tree1 =
    BinaryNode
        (BinaryNode (BinaryValue 1, BinaryValue 2),
         BinaryNode (BinaryValue 3, BinaryValue 4))
```

右括号还可以位于新行上：

```fsharp
let tree1 =
    BinaryNode(
        BinaryNode (BinaryValue 1, BinaryValue 2),
        BinaryNode (BinaryValue 3, BinaryValue 4)
    )
```

## <a name="formatting-record-declarations"></a>设置记录声明格式

`{`在类型定义中按四个空格缩进，并在同一行中启动字段列表：

```fsharp
// OK
type PostalAddress =
    { Address: string
      City: string
      Zip: string }
    member x.ZipAndCity = $"{x.Zip} {x.City}"

// Not OK
type PostalAddress =
  { Address: string
    City: string
    Zip: string }
    member x.ZipAndCity = $"{x.Zip} {x.City}"

// Unusual in F#
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    }
```

如果要在记录上声明接口实现或成员，则在新行上放置打开标记，并在新行上放置结束标记更可取：

```fsharp
// Declaring additional members on PostalAddress
type PostalAddress =
    {
        Address: string
        City: string
        Zip: string
    }
    member x.ZipAndCity = $"{x.Zip} {x.City}"

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface
```

## <a name="formatting-records"></a>设置记录格式

可以在一行中写入短记录：

```fsharp
let point = { X = 1.0; Y = 0.0 }
```

更长时间的记录应使用标签的新行：

```fsharp
let rainbow =
    { Boss = "Jeffrey"
      Lackeys = ["Zippy"; "George"; "Bungle"] }
```

如果要执行以下操作，请在新行上放置打开标记，将 "内容" 选项卡在一个范围内，将结束标记放在新行上：

* 在具有不同缩进范围的代码中移动记录
* 将它们传递给函数

```fsharp
let rainbow =
    {
        Boss1 = "Jeffrey"
        Boss2 = "Jeffrey"
        Boss3 = "Jeffrey"
        Boss4 = "Jeffrey"
        Boss5 = "Jeffrey"
        Boss6 = "Jeffrey"
        Boss7 = "Jeffrey"
        Boss8 = "Jeffrey"
        Lackeys = ["Zippy"; "George"; "Bungle"]
    }

type MyRecord =
    {
        SomeField: int
    }
    interface IMyInterface

let foo a =
    a
    |> Option.map
           (fun x ->
                {
                    MyField = x
                })
```

相同的规则适用于列表和数组元素。

## <a name="formatting-copy-and-update-record-expressions"></a>设置复制和更新记录表达式的格式

复制和更新记录表达式仍是一条记录，因此应用类似的准则。

短表达式可以放在一行上：

```fsharp
let point2 = { point with X = 1; Y = 2 }
```

更长的表达式应使用新行：

```fsharp
let rainbow2 =
    { rainbow with
        Boss = "Jeffrey"
        Lackeys = [ "Zippy"; "George"; "Bungle" ] }
```

与记录指南一样，你可能想要将多个单独的行专用于大括号，并将一个范围向右缩进到表达式。 在某些特殊情况下，例如，使用不带括号的可选值包装值时，可能需要在一行上保留大括号：

```fsharp
type S = { F1: int; F2: string }
type State = { Foo: S option }

let state = { Foo = Some { F1 = 1; F2 = "Hello" } }
let newState =
    {
        state with
            Foo =
                Some {
                    F1 = 0
                    F2 = ""
                }
    }
```

## <a name="formatting-lists-and-arrays"></a>设置列表和数组的格式

`x :: l`运算符周围的空格写入 `::` (`::` 是内缀运算符，因此由空格括起来) 。

在单个行中声明的列表和数组应该在左方括号后面和右括号之前有一个空格：

```fsharp
let xs = [ 1; 2; 3 ]
let ys = [| 1; 2; 3; |]
```

请始终在两个不同的大括号运算符之间至少使用一个空格。 例如，在和之间留一个空格 `[` `{` 。

```fsharp
// OK
[ { IngredientName = "Green beans"; Quantity = 250 }
  { IngredientName = "Pine nuts"; Quantity = 250 }
  { IngredientName = "Feta cheese"; Quantity = 250 }
  { IngredientName = "Olive oil"; Quantity = 10 }
  { IngredientName = "Lemon"; Quantity = 1 } ]

// Not OK
[{ IngredientName = "Green beans"; Quantity = 250 }
 { IngredientName = "Pine nuts"; Quantity = 250 }
 { IngredientName = "Feta cheese"; Quantity = 250 }
 { IngredientName = "Olive oil"; Quantity = 10 }
 { IngredientName = "Lemon"; Quantity = 1 }]
```

相同的指导原则适用于元组的列表或数组。

跨多个行拆分的列表和数组将遵循类似的规则作为记录：

```fsharp
let pascalsTriangle =
    [|
        [| 1 |]
        [| 1; 1 |]
        [| 1; 2; 1 |]
        [| 1; 3; 3; 1 |]
        [| 1; 4; 6; 4; 1 |]
        [| 1; 5; 10; 10; 5; 1 |]
        [| 1; 6; 15; 20; 15; 6; 1 |]
        [| 1; 7; 21; 35; 35; 21; 7; 1 |]
        [| 1; 8; 28; 56; 70; 56; 28; 8; 1 |]
    |]
```

与记录一样，将左括号和右括号声明到其自己的行上，可以更轻松地移动代码和管道转换为函数。

以编程方式生成数组和列表时，优先 `->` 于 `do ... yield` 始终生成值的时间：

```fsharp
// Preferred
let squares = [ for x in 1..10 -> x * x ]

// Not preferred
let squares' = [ for x in 1..10 do yield x * x ]
```

旧版本的 F # 语言需要 `yield` 在可能有条件地生成数据的情况下指定，或者可能存在连续的表达式进行计算。 `yield`如果你必须使用较旧的 F # 语言版本进行编译，则优先于忽略这些关键字：

```fsharp
// Preferred
let daysOfWeek includeWeekend =
    [
        "Monday"
        "Tuesday"
        "Wednesday"
        "Thursday"
        "Friday"
        if includeWeekend then
            "Saturday"
            "Sunday"
    ]

// Not preferred
let daysOfWeek' includeWeekend =
    [
        yield "Monday"
        yield "Tuesday"
        yield "Wednesday"
        yield "Thursday"
        yield "Friday"
        if includeWeekend then
            yield "Saturday"
            yield "Sunday"
    ]
```

在某些情况下， `do...yield` 可能有助于提高可读性。 尽管应考虑主观，但应考虑到这些情况。

## <a name="formatting-if-expressions"></a>设置表达式的格式

条件的缩进取决于使其变得的表达式的大小和复杂程度。
在以下情况时，请将它们写入一行：

- `cond`、 `e1` 和 `e2` 为 short
- `e1` 和 `e2` 不是 `if/then/else` 表达式本身。

```fsharp
if cond then e1 else e2
```

如果任何表达式为多行或表达式，则为 `if/then/else` 。

```fsharp
if cond then
    e1
else
    e2
```

带有和的多个条件在 `elif` `else` `if` 遵循一个行表达式的规则时，将在与相同的范围内缩进 `if/then/else` 。

```fsharp
if cond1 then e1
elif cond2 then e2
elif cond3 then e3
else e4
```

如果有任何条件或表达式为多行，则整个 `if/then/else` 表达式为多行：

```fsharp
if cond1 then
    e1
elif cond2 then
    e2
elif cond3 then
    e3
else
    e4
```

### <a name="pattern-matching-constructs"></a>模式匹配构造

`|`对匹配的每个子句使用，无缩进。 如果表达式较短，则如果每个子表达式也简单，则可以考虑使用单个行。

```fsharp
// OK
match l with
| { him = x; her = "Posh" } :: tail -> x
| _ :: tail -> findDavid tail
| [] -> failwith "Couldn't find David"

// Not OK
match l with
    | { him = x; her = "Posh" } :: tail -> x
    | _ :: tail -> findDavid tail
    | [] -> failwith "Couldn't find David"
```

如果模式匹配箭头右侧的表达式太大，则将其移动到下面的行，并从中缩进一个步骤 `match` / `|` 。

```fsharp
match lam with
| Var v -> 1
| Abs(x, body) ->
    1 + sizeLambda body
| App(lam1, lam2) ->
    sizeLambda lam1 + sizeLambda lam2

```

从开始，从开始，对匿名函数进行的模式匹配 `function` 通常不应缩进太远。 例如，将一个作用域缩进如下所示：

```fsharp
lambdaList
|> List.map
       (function
            | Abs(x, body) -> 1 + sizeLambda 0 body
            | App(lam1, lam2) -> sizeLambda (sizeLambda 0 lam1) lam2
            | Var v -> 1)
```

即使使用关键字，使用定义的函数中的模式匹配 `let` 也 `let rec` 应缩进四个空格 `let` `function` ：

```fsharp
let rec sizeLambda acc =
    function
    | Abs(x, body) -> sizeLambda (succ acc) body
    | App(lam1, lam2) -> sizeLambda (sizeLambda acc lam1) lam2
    | Var v -> succ acc
```

建议不要对齐箭头。

## <a name="formatting-trywith-expressions"></a>格式 try/with 表达式

应将异常类型的模式匹配缩进到与相同的级别 `with` 。

```fsharp
try
    if System.DateTime.Now.Second % 3 = 0 then
        raise (new System.Exception())
    else
        raise (new System.ApplicationException())
with
| :? System.ApplicationException ->
    printfn "A second that was not a multiple of 3"
| _ ->
    printfn "A second that was a multiple of 3"
```

始终 `|` 为每个子句添加，即使只有一个子句时也是如此。

```fsharp
// OK
try
    persistState currentState
with
| ex -> 
    printfn "Something went wrong: %A" ex
    
// Not OK
try
    persistState currentState
with ex ->
    printfn "Something went wrong: %A" ex
```

## <a name="formatting-function-parameter-application"></a>格式化函数参数应用程序

通常，大多数参数在同一行中提供：

```fsharp
let x = sprintf "\t%s - %i\n\r" x.IngredientName x.Quantity

let printListWithOffset a list1 =
    List.iter (fun elem -> printfn $"%d{a + elem}") list1
```

当管道相关时，通常情况下也是如此，在这种情况下，扩充函数作为参数应用于同一行：

```
let printListWithOffsetPiped a list1 =
    list1
    |> List.iter (fun elem -> printfn $"%d{a + elem}")
```

但是，你可能希望在新行上将参数传递给函数，这是一个可读性，或者参数列表太长。 在这种情况下，将缩进一个作用域：

```fsharp

// OK
sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
sprintf
     "\t%s - %i\n\r"
     x.IngredientName x.Quantity

// OK
let printVolumes x =
    printf "Volume in liters = %f, in us pints = %f, in imperial = %f"
        (convertVolumeToLiter x)
        (convertVolumeUSPint x)
        (convertVolumeImperialPint x)
```

对于 lambda 表达式，你可能还需要考虑将 lambda 表达式的主体放置在新行上（如果足够长，则按一个范围缩进）：

```fsharp
let printListWithOffset a list1 =
    List.iter
        (fun elem ->
             printfn $"A very long line to format the value: %d{a + elem}")
        list1

let printListWithOffsetPiped a list1 =
    list1
    |> List.iter
           (fun elem ->
                printfn $"A very long line to format the value: %d{a + elem}")
```

如果 lambda 表达式的主体长度为多行，则应考虑将其重构为本地范围内的函数。

参数通常应相对于函数或关键字缩进 `fun` / `function` ，而不考虑该函数显示在哪个上下文中：

```fsharp
// With 4 spaces indentation
list1
|> List.fold
       someLongParam
       anotherLongParam

list1
|> List.iter
       (fun elem ->
            printfn $"A very long line to format the value: %d{elem}")

// With 2 spaces indentation
list1
|> List.fold
     someLongParam
     anotherLongParam

list1
|> List.iter
       (fun elem ->
          printfn $"A very long line to format the value: %d{elem}")
```

当函数采用单个多行元组参数时，适用于 [格式设置构造函数、静态成员和成员调用](#formatting-constructors-static-members-and-member-invocations) 的相同规则。

```fsharp
let myFunction (a: int, b: string, c: int, d: bool) =
    ()

myFunction(
    478815516,
    "A very long string making all of this multi-line",
    1515,
    false
)
```

### <a name="formatting-infix-operators"></a>格式化中缀运算符

用空格分隔运算符。 此规则的明显例外是 `!` 和 `.` 运算符。

中缀表达式可在同一列上进行阵容：

```fsharp
acc +
(sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity)

let function1 arg1 arg2 arg3 arg4 =
    arg1 + arg2 +
    arg3 + arg4
```

### <a name="formatting-pipeline-operators-or-mutable-assignments"></a>设置管道运算符或可变赋值的格式

管道 `|>` 运算符应位于它们所操作的表达式的下面。

```fsharp
// Preferred approach
let methods2 =
    System.AppDomain.CurrentDomain.GetAssemblies()
    |> List.ofArray
    |> List.map (fun assm -> assm.GetTypes())
    |> Array.concat
    |> List.ofArray
    |> List.map (fun t -> t.GetMethods())
    |> Array.concat

// Not OK
let methods2 = System.AppDomain.CurrentDomain.GetAssemblies()
            |> List.ofArray
            |> List.map (fun assm -> assm.GetTypes())
            |> Array.concat
            |> List.ofArray
            |> List.map (fun t -> t.GetMethods())
            |> Array.concat

// Not OK either
let methods2 = System.AppDomain.CurrentDomain.GetAssemblies()
               |> List.ofArray
               |> List.map (fun assm -> assm.GetTypes())
               |> Array.concat
               |> List.ofArray
               |> List.map (fun t -> t.GetMethods())
               |> Array.concat
```

这也适用于可变资源库：

```fsharp
// Preferred approach
ctx.Response.Headers.[HeaderNames.ContentType] <-
    Constants.jsonApiMediaType |> StringValues
ctx.Response.Headers.[HeaderNames.ContentLength] <-
    bytes.Length |> string |> StringValues

// Not OK
ctx.Response.Headers.[HeaderNames.ContentType] <- Constants.jsonApiMediaType
                                                  |> StringValues
ctx.Response.Headers.[HeaderNames.ContentLength] <- bytes.Length
                                                    |> string
                                                    |> StringValues
```

### <a name="formatting-modules"></a>格式化模块

本地模块中的代码必须相对于模块缩进，但不应缩进顶级模块中的代码。 命名空间元素不必缩进。

```fsharp
// A is a top-level module.
module A

let function1 a b = a - b * b
```

```fsharp
// A1 and A2 are local modules.
module A1 =
    let function1 a b = a * a + b * b

module A2 =
    let function2 a b = a * a - b * b
```

### <a name="formatting-object-expressions-and-interfaces"></a>设置对象表达式和接口的格式

对象表达式和接口的对齐方式应与在 `member` 四个空格后缩进的方式相同。

```fsharp
let comparer =
    { new IComparer<string> with
          member x.Compare(s1, s2) =
              let rev (s: String) =
                  new String (Array.rev (s.ToCharArray()))
              let reversed = rev s1
              reversed.CompareTo (rev s2) }
```

### <a name="formatting-white-space-in-expressions"></a>格式化表达式中的空白区域

避免 F # 表达式中有多余的空格。

```fsharp
// OK
spam (ham.[1])

// Not OK
spam ( ham.[ 1 ] )
```

命名参数的周围还不应包含间距 `=` ：

```fsharp
// OK
let makeStreamReader x = new System.IO.StreamReader(path=x)

// Not OK
let makeStreamReader x = new System.IO.StreamReader(path = x)
```

### <a name="formatting-constructors-static-members-and-member-invocations"></a>格式化构造函数、静态成员和成员调用

如果表达式为 short，则用空格分隔参数，并将其保留在一行中。

```fsharp
let person = new Person(a1, a2)

let myRegexMatch = Regex.Match(input, regex)

let untypedRes = checker.ParseFile(file, source, opts)
```

如果表达式过长，则使用换行符并缩进一个作用域，而不是缩进到方括号。

```fsharp
let person =
    new Person(
        argument1,
        argument2
    )

let myRegexMatch =
    Regex.Match(
        "my longer input string with some interesting content in it",
        "myRegexPattern"
    )

let untypedRes =
    checker.ParseFile(
        fileName,
        sourceText,
        parsingOptionsWithDefines
    )
```

即使只有一个多行参数，相同的规则也适用。

```fsharp
let poemBuilder = StringBuilder()
poemBuilder.AppendLine(
    """
The last train is nearly due
The Underground is closing soon
And in the dark, deserted station
Restless in anticipation
A man waits in the shadows
    """
)

Option.traverse(
    create
    >> Result.setError [ invalidHeader "Content-Checksum" ]
)
```

## <a name="formatting-generic-type-arguments-and-constraints"></a>设置泛型类型参数和约束的格式

下面的准则适用于函数、成员和类型定义。

如果泛型类型参数和约束不太长，请将它们保留在一行上：

```fsharp
let f<'a, 'b when 'a : equality and 'b : comparison> param =
    // function body
```

如果泛型类型参数/约束和函数参数均不适用，但仅有类型参数/约束，请将参数放置在新行上：

```fsharp
let f<'a, 'b when 'a : equality and 'b : comparison>
    param
    =
    // function body
```

如果类型参数或约束太长，请断开并对齐它们，如下所示。 将类型参数列表保留在函数所在的行上，而不考虑其长度。 对于约束，请将放 `when` 在第一行上，并在一行上保留每个约束，而不考虑其长度。 置于 `>` 最后一行的末尾。 将约束按一级缩进。

```fsharp
let inline f< ^a, ^b
    when ^a : (static member Foo1: unit -> ^b)
    and ^b : (member Foo2: unit -> int)
    and ^b : (member Foo3: string -> ^a option)>
    arg1
    arg2
    =
    // function body
```

如果类型参数/约束被分解，但没有普通函数参数，请将置于 `=` 新行，而不考虑：

```f#
let inline f<^a, ^b
    when ^a : (static member Foo1: unit -> ^b)
    and ^b : (member Foo2: unit -> int)
    and ^b : (member Foo3: string -> ^a option)>
    =
    // function body
```

## <a name="formatting-attributes"></a>格式设置特性

[特性](../language-reference/attributes.md) 位于构造之上：

```fsharp
[<SomeAttribute>]
type MyClass() = ...

[<RequireQualifiedAccess>]
module M =
    let f x = x

[<Struct>]
type MyRecord =
    { Label1: int
      Label2: string }
```

它们应该在任何 XML 文档之后：

```fsharp
/// Module with some things in it.
[<RequireQualifiedAccess>]
module M =
    let f x = x
```

### <a name="formatting-attributes-on-parameters"></a>格式化参数上的特性

还可以将属性放置在参数上。 在这种情况下，请将其放在与参数相同的行上，并在名称之前：

```fsharp
// Defines a class that takes an optional value as input defaulting to false.
type C() =
    member _.M([<Optional; DefaultParameterValue(false)>] doSomething: bool)
```

### <a name="formatting-multiple-attributes"></a>设置多个属性的格式

如果将多个特性应用于不是参数的构造，则应将它们放在每行中都有一个属性：

```fsharp
[<Struct>]
[<IsByRefLike>]
type MyRecord =
    { Label1: int
      Label2: string }
```

当应用于参数时，它们必须位于同一行上并由 `;` 分隔符分隔。

## <a name="formatting-literals"></a>设置文本格式

使用特性的[F # 文本](../language-reference/literals.md) `Literal` 应将属性置于其自己的行上，并使用 PascalCase 命名：

```fsharp
[<Literal>]
let Path = __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let MyUrl = "www.mywebsitethatiamworkingwith.com"
```

避免将属性放置在与值相同的行上。

## <a name="formatting-computation-expression-operations"></a>格式化计算表达式操作

为 [计算表达式](../language-reference/computation-expressions.md)创建自定义操作时，建议使用 camelCase 命名：

```fsharp
type MathBuilder () =
    member _.Yield _ = 0

    [<CustomOperation("addOne")>]
    member _.AddOne (state: int) =
        state + 1

    [<CustomOperation("subtractOne")>]
    member _.SubtractOne (state: int) =
        state - 1

    [<CustomOperation("divideBy")>]
    member _.DivideBy (state: int, divisor: int) =
        state / divisor

    [<CustomOperation("multiplyBy")>]
    member _.MultiplyBy (state: int, factor: int) =
        state * factor

let math = MathBuilder()

// 10
let myNumber =
    math {
        addOne
        addOne
        addOne

        subtractOne

        divideBy 2

        multiplyBy 10
    }
```

正在建模的域应最终驱动命名约定。
如果惯用使用不同的约定，应改为使用该约定。
