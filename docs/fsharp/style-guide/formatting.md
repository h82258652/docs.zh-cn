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
# <a name="f-code-formatting-guidelines"></a><span data-ttu-id="2ec75-103">F# 代码格式设置准则</span><span class="sxs-lookup"><span data-stu-id="2ec75-103">F# code formatting guidelines</span></span>

<span data-ttu-id="2ec75-104">本文提供了有关如何设置代码格式以使 F # 代码为：</span><span class="sxs-lookup"><span data-stu-id="2ec75-104">This article offers guidelines for how to format your code so that your F# code is:</span></span>

* <span data-ttu-id="2ec75-105">更清晰</span><span class="sxs-lookup"><span data-stu-id="2ec75-105">More legible</span></span>
* <span data-ttu-id="2ec75-106">按照 Visual Studio 和其他编辑器中的格式设置工具所应用的约定进行</span><span class="sxs-lookup"><span data-stu-id="2ec75-106">In accordance with conventions applied by formatting tools in Visual Studio and other editors</span></span>
* <span data-ttu-id="2ec75-107">类似于其他代码 online</span><span class="sxs-lookup"><span data-stu-id="2ec75-107">Similar to other code online</span></span>

<span data-ttu-id="2ec75-108">这些准则基于通过[Anh-Dung Phan](https://github.com/dungpa)提供的[F # 格式设置约定的综合性指南](https://github.com/dungpa/fantomas/blob/master/docs/FormattingConventions.md)。</span><span class="sxs-lookup"><span data-stu-id="2ec75-108">These guidelines are based on [A comprehensive guide to F# Formatting Conventions](https://github.com/dungpa/fantomas/blob/master/docs/FormattingConventions.md) by [Anh-Dung Phan](https://github.com/dungpa).</span></span>

## <a name="general-rules-for-indentation"></a><span data-ttu-id="2ec75-109">缩进的一般规则</span><span class="sxs-lookup"><span data-stu-id="2ec75-109">General rules for indentation</span></span>

<span data-ttu-id="2ec75-110">F # 默认使用有效空白。</span><span class="sxs-lookup"><span data-stu-id="2ec75-110">F# uses significant white space by default.</span></span> <span data-ttu-id="2ec75-111">以下准则旨在提供有关如何调整一些可能会带来的挑战的指导。</span><span class="sxs-lookup"><span data-stu-id="2ec75-111">The following guidelines are intended to provide guidance as to how to juggle some challenges this can impose.</span></span>

### <a name="using-spaces"></a><span data-ttu-id="2ec75-112">使用空格</span><span class="sxs-lookup"><span data-stu-id="2ec75-112">Using spaces</span></span>

<span data-ttu-id="2ec75-113">需要缩进时，必须使用空格，而不是制表符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-113">When indentation is required, you must use spaces, not tabs.</span></span> <span data-ttu-id="2ec75-114">至少需要一个空格。</span><span class="sxs-lookup"><span data-stu-id="2ec75-114">At least one space is required.</span></span> <span data-ttu-id="2ec75-115">你的组织可以创建编码标准来指定要用于缩进的空格数;典型情况下，每个级别都有两个、三个或四个空格的缩进。</span><span class="sxs-lookup"><span data-stu-id="2ec75-115">Your organization can create coding standards to specify the number of spaces to use for indentation; two, three, or four spaces of indentation at each level where indentation occurs is typical.</span></span>

<span data-ttu-id="2ec75-116">**建议每个缩进包含四个空格。**</span><span class="sxs-lookup"><span data-stu-id="2ec75-116">**We recommend four spaces per indentation.**</span></span>

<span data-ttu-id="2ec75-117">也就是说，程序的缩进是一种主观上的事。</span><span class="sxs-lookup"><span data-stu-id="2ec75-117">That said, indentation of programs is a subjective matter.</span></span> <span data-ttu-id="2ec75-118">变体正常，但应遵循的第一条规则是 *缩进的一致性*。</span><span class="sxs-lookup"><span data-stu-id="2ec75-118">Variations are OK, but the first rule you should follow is *consistency of indentation*.</span></span> <span data-ttu-id="2ec75-119">选择一种普遍接受的缩进样式，并在代码库中系统地使用它。</span><span class="sxs-lookup"><span data-stu-id="2ec75-119">Choose a generally accepted style of indentation and use it systematically throughout your codebase.</span></span>

## <a name="formatting-white-space"></a><span data-ttu-id="2ec75-120">设置空格的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-120">Formatting white space</span></span>

<span data-ttu-id="2ec75-121">F # 区分空格。</span><span class="sxs-lookup"><span data-stu-id="2ec75-121">F# is white space sensitive.</span></span> <span data-ttu-id="2ec75-122">尽管通过适当的缩进涵盖了空白中的大多数语义，但还有其他一些事项需要注意。</span><span class="sxs-lookup"><span data-stu-id="2ec75-122">Although most semantics from white space are covered by proper indentation, there are some other things to consider.</span></span>

### <a name="formatting-operators-in-arithmetic-expressions"></a><span data-ttu-id="2ec75-123">在算术表达式中设置运算符的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-123">Formatting operators in arithmetic expressions</span></span>

<span data-ttu-id="2ec75-124">在二进制算术表达式周围始终使用空格：</span><span class="sxs-lookup"><span data-stu-id="2ec75-124">Always use white space around binary arithmetic expressions:</span></span>

```fsharp
let subtractThenAdd x = x - 1 + 3
```

<span data-ttu-id="2ec75-125">一元 `-` 运算符的后面应始终紧跟它们所取消的值：</span><span class="sxs-lookup"><span data-stu-id="2ec75-125">Unary `-` operators should always be immediately followed by the value they are negating:</span></span>

```fsharp
// OK
let negate x = -x

// Bad
let negateBad x = - x
```

<span data-ttu-id="2ec75-126">在运算符后面添加空白字符可能会对 `-` 其他人造成混淆。</span><span class="sxs-lookup"><span data-stu-id="2ec75-126">Adding a white-space character after the `-` operator can lead to confusion for others.</span></span>

<span data-ttu-id="2ec75-127">总而言之，始终必须：</span><span class="sxs-lookup"><span data-stu-id="2ec75-127">In summary, it's important to always:</span></span>

* <span data-ttu-id="2ec75-128">围绕空格环绕二元运算符</span><span class="sxs-lookup"><span data-stu-id="2ec75-128">Surround binary operators with white space</span></span>
* <span data-ttu-id="2ec75-129">一元运算符后面永远没有尾随空格</span><span class="sxs-lookup"><span data-stu-id="2ec75-129">Never have trailing white space after a unary operator</span></span>

<span data-ttu-id="2ec75-130">二进制算术运算符的准则尤其重要。</span><span class="sxs-lookup"><span data-stu-id="2ec75-130">The binary arithmetic operator guideline is especially important.</span></span> <span data-ttu-id="2ec75-131">如果无法将二元运算符括起来 `-` ，则在与某些格式设置选项结合使用时，可能会将其解释为一元运算 `-` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-131">Failing to surround a binary `-` operator, when combined with certain formatting choices, could lead to interpreting it as a unary `-`.</span></span>

### <a name="surround-a-custom-operator-definition-with-white-space"></a><span data-ttu-id="2ec75-132">使用空格将自定义运算符定义括起来</span><span class="sxs-lookup"><span data-stu-id="2ec75-132">Surround a custom operator definition with white space</span></span>

<span data-ttu-id="2ec75-133">始终使用空格括起运算符定义：</span><span class="sxs-lookup"><span data-stu-id="2ec75-133">Always use white space to surround an operator definition:</span></span>

```fsharp
// OK
let ( !> ) x f = f x

// Bad
let (!>) x f = f x
```

<span data-ttu-id="2ec75-134">对于以开头并且包含多个字符的任何自定义运算符 `*` ，需要在定义的开头添加一个空格，以避免编译器歧义。</span><span class="sxs-lookup"><span data-stu-id="2ec75-134">For any custom operator that starts with `*` and that has more than one character, you need to add a white space to the beginning of the definition to avoid a compiler ambiguity.</span></span> <span data-ttu-id="2ec75-135">出于此原因，我们建议你只需将所有运算符的定义括起来，只包含一个空白字符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-135">Because of this, we recommend that you simply surround the definitions of all operators with a single white-space character.</span></span>

### <a name="surround-function-parameter-arrows-with-white-space"></a><span data-ttu-id="2ec75-136">用空格将函数参数箭头括起来</span><span class="sxs-lookup"><span data-stu-id="2ec75-136">Surround function parameter arrows with white space</span></span>

<span data-ttu-id="2ec75-137">定义函数的签名时，使用符号周围的空格 `->` ：</span><span class="sxs-lookup"><span data-stu-id="2ec75-137">When defining the signature of a function, use white space around the `->` symbol:</span></span>

```fsharp
// OK
type MyFun = int -> int -> string

// Bad
type MyFunBad = int->int->string
```

### <a name="surround-function-arguments-with-white-space"></a><span data-ttu-id="2ec75-138">用空格将函数参数括起来</span><span class="sxs-lookup"><span data-stu-id="2ec75-138">Surround function arguments with white space</span></span>

<span data-ttu-id="2ec75-139">定义函数时，请在每个参数周围使用空白。</span><span class="sxs-lookup"><span data-stu-id="2ec75-139">When defining a function, use white space around each argument.</span></span>

```fsharp
// OK
let myFun (a: decimal) b c = a + b + c

// Bad
let myFunBad (a:decimal)(b)c = a + b + c
```

### <a name="avoid-name-sensitive-alignments"></a><span data-ttu-id="2ec75-140">避免名称敏感度对齐</span><span class="sxs-lookup"><span data-stu-id="2ec75-140">Avoid name-sensitive alignments</span></span>

<span data-ttu-id="2ec75-141">通常，请设法避免对命名敏感的缩进和对齐方式：</span><span class="sxs-lookup"><span data-stu-id="2ec75-141">In general, seek to avoid indentation and alignment that is sensitive to naming:</span></span>

```fsharp
// OK
let myLongValueName =
    someExpression
    |> anotherExpression


// Bad
let myLongValueName = someExpression
                      |> anotherExpression
```

<span data-ttu-id="2ec75-142">这有时称为 "虚对齐" 或 "虚缩进"。</span><span class="sxs-lookup"><span data-stu-id="2ec75-142">This is sometimes called “vanity alignment” or “vanity indentation”.</span></span> <span data-ttu-id="2ec75-143">避免这种情况的主要原因是：</span><span class="sxs-lookup"><span data-stu-id="2ec75-143">The primary reasons for avoiding this are:</span></span>

* <span data-ttu-id="2ec75-144">重要的代码将向右移动</span><span class="sxs-lookup"><span data-stu-id="2ec75-144">Important code is moved far to the right</span></span>
* <span data-ttu-id="2ec75-145">实际代码的宽度不变</span><span class="sxs-lookup"><span data-stu-id="2ec75-145">There is less width left for the actual code</span></span>
* <span data-ttu-id="2ec75-146">重命名可能会破坏对齐</span><span class="sxs-lookup"><span data-stu-id="2ec75-146">Renaming can break the alignment</span></span>

<span data-ttu-id="2ec75-147">为执行相同的操作，以便 `do` / `do!` 使缩进与保持一致 `let` / `let!` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-147">Do the same for `do`/`do!` in order to keep the indentation consistent with `let`/`let!`.</span></span> <span data-ttu-id="2ec75-148">下面是 `do` 在类中使用的示例：</span><span class="sxs-lookup"><span data-stu-id="2ec75-148">Here is an example using `do` in a class:</span></span>

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

<span data-ttu-id="2ec75-149">下面是一个示例，其中 `do!` 使用了2个空格的缩进 (，因为在 `do!` 使用4个空格的缩进) 时，这两种方法之间没有任何区别：</span><span class="sxs-lookup"><span data-stu-id="2ec75-149">Here is an example with `do!` using 2 spaces of indentation (because with `do!` there is coincidentally no difference between the approaches when using 4 spaces of indentation):</span></span>

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

### <a name="place-parameters-on-a-new-line-for-long-definitions"></a><span data-ttu-id="2ec75-150">为长定义将参数放在新行上</span><span class="sxs-lookup"><span data-stu-id="2ec75-150">Place parameters on a new line for long definitions</span></span>

<span data-ttu-id="2ec75-151">如果使用的是长函数定义，请将参数放在新行上，并缩进它们以匹配后续参数的缩进级别。</span><span class="sxs-lookup"><span data-stu-id="2ec75-151">If you have a long function definition, place the parameters on new lines and indent them to match the indentation level of the subsequent parameter.</span></span>

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

<span data-ttu-id="2ec75-152">这同样适用于使用元组的成员、构造函数和参数：</span><span class="sxs-lookup"><span data-stu-id="2ec75-152">This also applies to members, constructors, and parameters using tuples:</span></span>

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

<span data-ttu-id="2ec75-153">如果参数为 currified，则将 `=` 字符和任何返回类型放在新行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-153">If the parameters are currified, place the `=` character along with any return type on a new line:</span></span>

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

<span data-ttu-id="2ec75-154">这是一种避免太长的行 (的方法，如果返回类型可能有长的名称) 并且在添加参数时具有更少的行损坏。</span><span class="sxs-lookup"><span data-stu-id="2ec75-154">This is a way to avoid too long lines (in case return type might have long name) and have less line-damage when adding parameters.</span></span>

### <a name="type-annotations"></a><span data-ttu-id="2ec75-155">类型批注</span><span class="sxs-lookup"><span data-stu-id="2ec75-155">Type annotations</span></span>

#### <a name="right-pad-function-argument-type-annotations"></a><span data-ttu-id="2ec75-156">右填充函数参数类型批注</span><span class="sxs-lookup"><span data-stu-id="2ec75-156">Right-pad function argument type annotations</span></span>

<span data-ttu-id="2ec75-157">定义具有类型批注的参数时，请使用符号后面的空格 `:` ：</span><span class="sxs-lookup"><span data-stu-id="2ec75-157">When defining arguments with type annotations, use white space after the `:` symbol:</span></span>

```fsharp
// OK
let complexFunction (a: int) (b: int) c = a + b + c

// Bad
let complexFunctionBad (a :int) (b :int) (c:int) = a + b + c
```

#### <a name="surround-return-type-annotations-with-white-space"></a><span data-ttu-id="2ec75-158">带有空格的环绕返回类型批注</span><span class="sxs-lookup"><span data-stu-id="2ec75-158">Surround return type annotations with white space</span></span>

<span data-ttu-id="2ec75-159">在允许绑定函数或值类型批注中 (在函数) 情况下返回类型时，请使用符号前后的空格 `:` ：</span><span class="sxs-lookup"><span data-stu-id="2ec75-159">In a let-bound function or value type annotation (return type in the case of a function), use white space before and after the `:` symbol:</span></span>

```fsharp
// OK
let expensiveToCompute : int = 0 // Type annotation for let-bound value
let myFun (a: decimal) b c : decimal = a + b + c // Type annotation for the return type of a function
// Bad
let expensiveToComputeBad1:int = 1
let expensiveToComputeBad2 :int = 2
let myFunBad (a: decimal) b c:decimal = a + b + c
```

### <a name="formatting-bindings"></a><span data-ttu-id="2ec75-160">格式绑定</span><span class="sxs-lookup"><span data-stu-id="2ec75-160">Formatting bindings</span></span>

<span data-ttu-id="2ec75-161">在所有情况下，绑定的右侧要么都在一行上，要么 (如果太长) 则进入缩进一个范围的新行。</span><span class="sxs-lookup"><span data-stu-id="2ec75-161">In all cases, the right-hand side of a binding either all goes on one line, or (if it's too long) goes on a new line indented one scope.</span></span>

<span data-ttu-id="2ec75-162">例如，以下内容不符合：</span><span class="sxs-lookup"><span data-stu-id="2ec75-162">For example, the following are non-compliant:</span></span>

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

<span data-ttu-id="2ec75-163">以下内容符合：</span><span class="sxs-lookup"><span data-stu-id="2ec75-163">The following are compliant:</span></span>

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

## <a name="formatting-blank-lines"></a><span data-ttu-id="2ec75-164">设置空行的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-164">Formatting blank lines</span></span>

* <span data-ttu-id="2ec75-165">用两个空行分隔顶级函数和类定义。</span><span class="sxs-lookup"><span data-stu-id="2ec75-165">Separate top-level function and class definitions with two blank lines.</span></span>
* <span data-ttu-id="2ec75-166">类中的方法定义由一个空行分隔。</span><span class="sxs-lookup"><span data-stu-id="2ec75-166">Method definitions inside a class are separated by a single blank line.</span></span>
* <span data-ttu-id="2ec75-167">可以 (慎用额外的空白行) 分离相关函数组。</span><span class="sxs-lookup"><span data-stu-id="2ec75-167">Extra blank lines may be used (sparingly) to separate groups of related functions.</span></span> <span data-ttu-id="2ec75-168">可能会在一组相关的 liners (（例如，一组虚拟实现) ）之间省略空行。</span><span class="sxs-lookup"><span data-stu-id="2ec75-168">Blank lines may be omitted between a bunch of related one-liners (for example, a set of dummy implementations).</span></span>
* <span data-ttu-id="2ec75-169">在函数中，应慎用空白行以指示逻辑部分。</span><span class="sxs-lookup"><span data-stu-id="2ec75-169">Use blank lines in functions, sparingly, to indicate logical sections.</span></span>

## <a name="formatting-comments"></a><span data-ttu-id="2ec75-170">设置注释格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-170">Formatting comments</span></span>

<span data-ttu-id="2ec75-171">通常，对于 ML 样式的块注释，使用多个双斜杠注释。</span><span class="sxs-lookup"><span data-stu-id="2ec75-171">Generally prefer multiple double-slash comments over ML-style block comments.</span></span>

```fsharp
// Prefer this style of comments when you want
// to express written ideas on multiple lines.

(*
    ML-style comments are fine, but not a .NET-ism.
    They are useful when needing to modify multi-line comments, though.
*)
```

<span data-ttu-id="2ec75-172">内联注释应为首字母大写。</span><span class="sxs-lookup"><span data-stu-id="2ec75-172">Inline comments should capitalize the first letter.</span></span>

```fsharp
let f x = x + 1 // Increment by one.
```

## <a name="formatting-string-literals-and-interpolated-strings"></a><span data-ttu-id="2ec75-173">格式化字符串文本和内插字符串</span><span class="sxs-lookup"><span data-stu-id="2ec75-173">Formatting string literals and interpolated strings</span></span>

<span data-ttu-id="2ec75-174">无论行的长度如何，字符串文本和内插字符串都可以保留在单个行中。</span><span class="sxs-lookup"><span data-stu-id="2ec75-174">String literals and interpolated strings can just be left on a single line, regardless of how long the line is.</span></span>

```fsharp
let serviceStorageConnection =
    $"DefaultEndpointsProtocol=https;AccountName=%s{serviceStorageAccount.Name};AccountKey=%s{serviceStorageAccountKey.Value}"
```

<span data-ttu-id="2ec75-175">强烈建议不要采用多行内插表达式。</span><span class="sxs-lookup"><span data-stu-id="2ec75-175">Multi-line interpolated expressions are strongly discouraged.</span></span> <span data-ttu-id="2ec75-176">相反，将表达式结果绑定到值并将其用于内插字符串。</span><span class="sxs-lookup"><span data-stu-id="2ec75-176">Instead, bind the expression result to a value and use that in the interpolated string.</span></span>

## <a name="naming-conventions"></a><span data-ttu-id="2ec75-177">命名约定</span><span class="sxs-lookup"><span data-stu-id="2ec75-177">Naming conventions</span></span>

### <a name="use-camelcase-for-class-bound-expression-bound-and-pattern-bound-values-and-functions"></a><span data-ttu-id="2ec75-178">将 camelCase 用于类绑定、表达式绑定以及模式绑定值和函数</span><span class="sxs-lookup"><span data-stu-id="2ec75-178">Use camelCase for class-bound, expression-bound, and pattern-bound values and functions</span></span>

<span data-ttu-id="2ec75-179">常见和接受的 F # 样式使用 camelCase 作为局部变量或模式匹配和函数定义中的所有名称。</span><span class="sxs-lookup"><span data-stu-id="2ec75-179">It is common and accepted F# style to use camelCase for all names bound as local variables or in pattern matches and function definitions.</span></span>

```fsharp
// OK
let addIAndJ i j = i + j

// Bad
let addIAndJ I J = I+J

// Bad
let AddIAndJ i j = i + j
```

<span data-ttu-id="2ec75-180">类中的本地绑定函数还应使用 camelCase。</span><span class="sxs-lookup"><span data-stu-id="2ec75-180">Locally bound functions in classes should also use camelCase.</span></span>

```fsharp
type MyClass() =

    let doSomething () =

    let firstResult = ...

    let secondResult = ...

    member x.Result = doSomething()
```

### <a name="use-camelcase-for-module-bound-public-functions"></a><span data-ttu-id="2ec75-181">对模块绑定公共函数使用 camelCase</span><span class="sxs-lookup"><span data-stu-id="2ec75-181">Use camelCase for module-bound public functions</span></span>

<span data-ttu-id="2ec75-182">当模块绑定函数是公共 API 的一部分时，它应使用 camelCase：</span><span class="sxs-lookup"><span data-stu-id="2ec75-182">When a module-bound function is part of a public API, it should use camelCase:</span></span>

```fsharp
module MyAPI =
    let publicFunctionOne param1 param2 param2 = ...

    let publicFunctionTwo param1 param2 param3 = ...
```

### <a name="use-camelcase-for-internal-and-private-module-bound-values-and-functions"></a><span data-ttu-id="2ec75-183">将 camelCase 用于内部和私有模块绑定值和函数</span><span class="sxs-lookup"><span data-stu-id="2ec75-183">Use camelCase for internal and private module-bound values and functions</span></span>

<span data-ttu-id="2ec75-184">将 camelCase 用于专用模块绑定值，包括以下内容：</span><span class="sxs-lookup"><span data-stu-id="2ec75-184">Use camelCase for private module-bound values, including the following:</span></span>

* <span data-ttu-id="2ec75-185">脚本中的即席函数</span><span class="sxs-lookup"><span data-stu-id="2ec75-185">Ad hoc functions in scripts</span></span>

* <span data-ttu-id="2ec75-186">构成模块或类型的内部实现的值</span><span class="sxs-lookup"><span data-stu-id="2ec75-186">Values making up the internal implementation of a module or type</span></span>

```fsharp
let emailMyBossTheLatestResults =
    ...
```

### <a name="use-camelcase-for-parameters"></a><span data-ttu-id="2ec75-187">将 camelCase 用于参数</span><span class="sxs-lookup"><span data-stu-id="2ec75-187">Use camelCase for parameters</span></span>

<span data-ttu-id="2ec75-188">所有参数应根据 .NET 命名约定使用 camelCase。</span><span class="sxs-lookup"><span data-stu-id="2ec75-188">All parameters should use camelCase in accordance with .NET naming conventions.</span></span>

```fsharp
module MyModule =
    let myFunction paramOne paramTwo = ...

type MyClass() =
    member this.MyMethod(paramOne, paramTwo) = ...
```

### <a name="use-pascalcase-for-modules"></a><span data-ttu-id="2ec75-189">将 PascalCase 用于模块</span><span class="sxs-lookup"><span data-stu-id="2ec75-189">Use PascalCase for modules</span></span>

<span data-ttu-id="2ec75-190">所有模块 (顶级、内部、私有、嵌套) 应使用 PascalCase。</span><span class="sxs-lookup"><span data-stu-id="2ec75-190">All modules (top-level, internal, private, nested) should use PascalCase.</span></span>

```fsharp
module MyTopLevelModule

module Helpers =
    module private SuperHelpers =
        ...

    ...
```

### <a name="use-pascalcase-for-type-declarations-members-and-labels"></a><span data-ttu-id="2ec75-191">对类型声明、成员和标签使用 PascalCase</span><span class="sxs-lookup"><span data-stu-id="2ec75-191">Use PascalCase for type declarations, members, and labels</span></span>

<span data-ttu-id="2ec75-192">类、接口、结构、枚举、委托、记录和可区分联合都应以 PascalCase 命名。</span><span class="sxs-lookup"><span data-stu-id="2ec75-192">Classes, interfaces, structs, enumerations, delegates, records, and discriminated unions should all be named with PascalCase.</span></span> <span data-ttu-id="2ec75-193">记录和可区分联合的类型和标签内的成员还应使用 PascalCase。</span><span class="sxs-lookup"><span data-stu-id="2ec75-193">Members within types and labels for records and discriminated unions should also use PascalCase.</span></span>

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

### <a name="use-pascalcase-for-constructs-intrinsic-to-net"></a><span data-ttu-id="2ec75-194">将 PascalCase 用于 .NET 内部构造</span><span class="sxs-lookup"><span data-stu-id="2ec75-194">Use PascalCase for constructs intrinsic to .NET</span></span>

<span data-ttu-id="2ec75-195">命名空间、异常、事件和项目/ `.dll` 名称还应使用 PascalCase。</span><span class="sxs-lookup"><span data-stu-id="2ec75-195">Namespaces, exceptions, events, and project/`.dll` names should also use PascalCase.</span></span> <span data-ttu-id="2ec75-196">这不仅会使其他 .NET 语言的使用变得对使用者更自然，还与你可能会遇到的 .NET 命名约定一致。</span><span class="sxs-lookup"><span data-stu-id="2ec75-196">Not only does this make consumption from other .NET languages feel more natural to consumers, it's also consistent with .NET naming conventions that you are likely to encounter.</span></span>

### <a name="avoid-underscores-in-names"></a><span data-ttu-id="2ec75-197">避免名称中有下划线</span><span class="sxs-lookup"><span data-stu-id="2ec75-197">Avoid underscores in names</span></span>

<span data-ttu-id="2ec75-198">从历史上看，某些 F # 库在名称中使用了下划线。</span><span class="sxs-lookup"><span data-stu-id="2ec75-198">Historically, some F# libraries have used underscores in names.</span></span> <span data-ttu-id="2ec75-199">但是，这并不能再被广泛接受，因为它与 .NET 命名约定冲突。</span><span class="sxs-lookup"><span data-stu-id="2ec75-199">However, this is no longer widely accepted, partly because it clashes with .NET naming conventions.</span></span> <span data-ttu-id="2ec75-200">也就是说，某些 F # 程序员在很大程度上使用下划线，其中部分原因是出于历史原因，而容差和尊重非常重要。</span><span class="sxs-lookup"><span data-stu-id="2ec75-200">That said, some F# programmers use underscores heavily, partly for historical reasons, and tolerance and respect is important.</span></span> <span data-ttu-id="2ec75-201">但是，这种样式通常不喜欢于对是否使用该样式有选择的其他人。</span><span class="sxs-lookup"><span data-stu-id="2ec75-201">However, the style is often disliked by others who have a choice about whether to use it.</span></span>

<span data-ttu-id="2ec75-202">一个例外包括与本机组件互操作，其中有下划线。</span><span class="sxs-lookup"><span data-stu-id="2ec75-202">One exception includes interoperating with native components, where underscores are common.</span></span>

### <a name="use-standard-f-operators"></a><span data-ttu-id="2ec75-203">使用标准 F # 运算符</span><span class="sxs-lookup"><span data-stu-id="2ec75-203">Use standard F# operators</span></span>

<span data-ttu-id="2ec75-204">以下运算符是在 F # 标准库中定义的，应使用而不是定义等效运算符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-204">The following operators are defined in the F# standard library and should be used instead of defining equivalents.</span></span> <span data-ttu-id="2ec75-205">建议使用这些运算符，因为这往往会使代码更具可读性和惯用。</span><span class="sxs-lookup"><span data-stu-id="2ec75-205">Using these operators is recommended as it tends to make code more readable and idiomatic.</span></span> <span data-ttu-id="2ec75-206">具有 OCaml 或其他功能编程语言的开发人员可能习惯于不同的惯例。</span><span class="sxs-lookup"><span data-stu-id="2ec75-206">Developers with a background in OCaml or other functional programming language may be accustomed to different idioms.</span></span> <span data-ttu-id="2ec75-207">以下列表汇总了建议的 F # 运算符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-207">The following list summarizes the recommended F# operators.</span></span>

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

### <a name="use-prefix-syntax-for-generics-foot-in-preference-to-postfix-syntax-t-foo"></a><span data-ttu-id="2ec75-208">对泛型 (使用前缀语法 `Foo<T>`) 优先使用后缀语法 (`T Foo`) </span><span class="sxs-lookup"><span data-stu-id="2ec75-208">Use prefix syntax for generics (`Foo<T>`) in preference to postfix syntax (`T Foo`)</span></span>

<span data-ttu-id="2ec75-209">F # 同时继承命名泛型类型的后缀 ML 形式 (例如， `int list`) 以及前缀 .net 样式 (例如 `list<int>`) 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-209">F# inherits both the postfix ML style of naming generic types (for example, `int list`) as well as the prefix .NET style (for example, `list<int>`).</span></span> <span data-ttu-id="2ec75-210">除了五个特定类型外，更喜欢 .NET 样式：</span><span class="sxs-lookup"><span data-stu-id="2ec75-210">Prefer the .NET style, except for five specific types:</span></span>

1. <span data-ttu-id="2ec75-211">对于 F # 列表，请使用后缀形式： `int list` 而不是 `list<int>` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-211">For F# Lists, use the postfix form: `int list` rather than `list<int>`.</span></span>
2. <span data-ttu-id="2ec75-212">对于 F # 选项，请使用后缀形式： `int option` 而不是 `option<int>` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-212">For F# Options, use the postfix form: `int option` rather than `option<int>`.</span></span>
3. <span data-ttu-id="2ec75-213">对于 F # 值选项，请使用后缀形式： `int voption` 而不是 `voption<int>` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-213">For F# Value Options, use the postfix form: `int voption` rather than `voption<int>`.</span></span>
4. <span data-ttu-id="2ec75-214">对于 F # 数组，请使用句法名称， `int[]` 而不是 `int array` 或 `array<int>` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-214">For F# arrays, use the syntactic name `int[]` rather than `int array` or `array<int>`.</span></span>
5. <span data-ttu-id="2ec75-215">对于引用单元格，请使用 `int ref` 而不是 `ref<int>` 或 `Ref<int>` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-215">For Reference Cells, use `int ref` rather than `ref<int>` or `Ref<int>`.</span></span>

<span data-ttu-id="2ec75-216">对于所有其他类型，请使用前缀形式。</span><span class="sxs-lookup"><span data-stu-id="2ec75-216">For all other types, use the prefix form.</span></span>

## <a name="formatting-tuples"></a><span data-ttu-id="2ec75-217">格式化元组</span><span class="sxs-lookup"><span data-stu-id="2ec75-217">Formatting tuples</span></span>

<span data-ttu-id="2ec75-218">元组实例化应带有括号，其中的分隔逗号后面应跟一个空格，例如： `(1, 2)` 、 `(x, y, z)` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-218">A tuple instantiation should be parenthesized, and the delimiting commas within it should be followed by a single space, for example: `(1, 2)`, `(x, y, z)`.</span></span>

<span data-ttu-id="2ec75-219">通常会接受在元组的模式匹配中省略括号：</span><span class="sxs-lookup"><span data-stu-id="2ec75-219">It is commonly accepted to omit parentheses in pattern matching of tuples:</span></span>

```fsharp
let (x, y) = z // Destructuring
let x, y = z // OK

// OK
match x, y with
| 1, _ -> 0
| x, 1 -> 0
| x, y -> 1
```

<span data-ttu-id="2ec75-220">如果元组是函数的返回值，则通常也可以接受省略括号：</span><span class="sxs-lookup"><span data-stu-id="2ec75-220">It is also commonly accepted to omit parentheses if the tuple is the return value of a function:</span></span>

```fsharp
// OK
let update model msg =
    match msg with
    | 1 -> model + 1, []
    | _ -> model, [ msg ]
```

<span data-ttu-id="2ec75-221">总而言之，使用带括号的元组实例化，但使用元组进行模式匹配或返回值时，可以将其视为非常好的以避免使用括号。</span><span class="sxs-lookup"><span data-stu-id="2ec75-221">In summary, prefer parenthesized tuple instantiations, but when using tuples for pattern matching or a return value, it is considered fine to avoid parentheses.</span></span>

## <a name="formatting-discriminated-union-declarations"></a><span data-ttu-id="2ec75-222">设置可区分联合声明的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-222">Formatting discriminated union declarations</span></span>

<span data-ttu-id="2ec75-223">`|`在类型定义中按四个空格缩进：</span><span class="sxs-lookup"><span data-stu-id="2ec75-223">Indent `|` in type definition by four spaces:</span></span>

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

<span data-ttu-id="2ec75-224">如果有单个 short 联合，则可以省略前导 `|` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-224">When there is a single short union, you can omit the leading `|`.</span></span>

```fsharp
type Address = Address of string
```

<span data-ttu-id="2ec75-225">对于较长或多行联合，请保留 `|` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-225">For a longer or multiline union, keep the `|`.</span></span>

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

<span data-ttu-id="2ec75-226">你还可以使用三斜杠 `///` 注释。</span><span class="sxs-lookup"><span data-stu-id="2ec75-226">You can also use triple-slash `///` comments.</span></span>

```fsharp
type Foobar =
    /// Code comment
    | Foobar of int
```

## <a name="formatting-discriminated-unions"></a><span data-ttu-id="2ec75-227">设置可区分联合的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-227">Formatting discriminated unions</span></span>

<span data-ttu-id="2ec75-228">在可区分的联合用例前面使用带圆括号/元集参数的空格：</span><span class="sxs-lookup"><span data-stu-id="2ec75-228">Use a space before parenthesized/tupled parameters to discriminated union cases:</span></span>

```fsharp
// OK
let opt = Some ("A", 1)

// Not OK
let opt = Some("A", 1)
```

<span data-ttu-id="2ec75-229">跨多个行拆分的实例化的可区分联合应为包含的数据提供具有缩进的新范围：</span><span class="sxs-lookup"><span data-stu-id="2ec75-229">Instantiated Discriminated Unions that split across multiple lines should give contained data a new scope with indentation:</span></span>

```fsharp
let tree1 =
    BinaryNode
        (BinaryNode (BinaryValue 1, BinaryValue 2),
         BinaryNode (BinaryValue 3, BinaryValue 4))
```

<span data-ttu-id="2ec75-230">右括号还可以位于新行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-230">The closing parenthesis can also be on a new line:</span></span>

```fsharp
let tree1 =
    BinaryNode(
        BinaryNode (BinaryValue 1, BinaryValue 2),
        BinaryNode (BinaryValue 3, BinaryValue 4)
    )
```

## <a name="formatting-record-declarations"></a><span data-ttu-id="2ec75-231">设置记录声明格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-231">Formatting record declarations</span></span>

<span data-ttu-id="2ec75-232">`{`在类型定义中按四个空格缩进，并在同一行中启动字段列表：</span><span class="sxs-lookup"><span data-stu-id="2ec75-232">Indent `{` in type definition by four spaces and start the field list on the same line:</span></span>

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

<span data-ttu-id="2ec75-233">如果要在记录上声明接口实现或成员，则在新行上放置打开标记，并在新行上放置结束标记更可取：</span><span class="sxs-lookup"><span data-stu-id="2ec75-233">Placing the opening token on a new line and the closing token on a new line is preferable if you are declaring interface implementations or members on the record:</span></span>

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

## <a name="formatting-records"></a><span data-ttu-id="2ec75-234">设置记录格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-234">Formatting records</span></span>

<span data-ttu-id="2ec75-235">可以在一行中写入短记录：</span><span class="sxs-lookup"><span data-stu-id="2ec75-235">Short records can be written in one line:</span></span>

```fsharp
let point = { X = 1.0; Y = 0.0 }
```

<span data-ttu-id="2ec75-236">更长时间的记录应使用标签的新行：</span><span class="sxs-lookup"><span data-stu-id="2ec75-236">Records that are longer should use new lines for labels:</span></span>

```fsharp
let rainbow =
    { Boss = "Jeffrey"
      Lackeys = ["Zippy"; "George"; "Bungle"] }
```

<span data-ttu-id="2ec75-237">如果要执行以下操作，请在新行上放置打开标记，将 "内容" 选项卡在一个范围内，将结束标记放在新行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-237">Placing the opening token on a new line, the contents tabbed over one scope, and the closing token on a new line is preferable if you are:</span></span>

* <span data-ttu-id="2ec75-238">在具有不同缩进范围的代码中移动记录</span><span class="sxs-lookup"><span data-stu-id="2ec75-238">Moving records around in code with different indentation scopes</span></span>
* <span data-ttu-id="2ec75-239">将它们传递给函数</span><span class="sxs-lookup"><span data-stu-id="2ec75-239">Piping them into a function</span></span>

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

<span data-ttu-id="2ec75-240">相同的规则适用于列表和数组元素。</span><span class="sxs-lookup"><span data-stu-id="2ec75-240">The same rules apply for list and array elements.</span></span>

## <a name="formatting-copy-and-update-record-expressions"></a><span data-ttu-id="2ec75-241">设置复制和更新记录表达式的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-241">Formatting copy-and-update record expressions</span></span>

<span data-ttu-id="2ec75-242">复制和更新记录表达式仍是一条记录，因此应用类似的准则。</span><span class="sxs-lookup"><span data-stu-id="2ec75-242">A copy-and-update record expression is still a record, so similar guidelines apply.</span></span>

<span data-ttu-id="2ec75-243">短表达式可以放在一行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-243">Short expressions can fit on one line:</span></span>

```fsharp
let point2 = { point with X = 1; Y = 2 }
```

<span data-ttu-id="2ec75-244">更长的表达式应使用新行：</span><span class="sxs-lookup"><span data-stu-id="2ec75-244">Longer expressions should use new lines:</span></span>

```fsharp
let rainbow2 =
    { rainbow with
        Boss = "Jeffrey"
        Lackeys = [ "Zippy"; "George"; "Bungle" ] }
```

<span data-ttu-id="2ec75-245">与记录指南一样，你可能想要将多个单独的行专用于大括号，并将一个范围向右缩进到表达式。</span><span class="sxs-lookup"><span data-stu-id="2ec75-245">And as with the record guidance, you may want to dedicate separate lines for the braces and indent one scope to the right with the expression.</span></span> <span data-ttu-id="2ec75-246">在某些特殊情况下，例如，使用不带括号的可选值包装值时，可能需要在一行上保留大括号：</span><span class="sxs-lookup"><span data-stu-id="2ec75-246">In some special cases, such as wrapping a value with an optional without parentheses, you may need to keep a brace on one line:</span></span>

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

## <a name="formatting-lists-and-arrays"></a><span data-ttu-id="2ec75-247">设置列表和数组的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-247">Formatting lists and arrays</span></span>

<span data-ttu-id="2ec75-248">`x :: l`运算符周围的空格写入 `::` (`::` 是内缀运算符，因此由空格括起来) 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-248">Write `x :: l` with spaces around the `::` operator (`::` is an infix operator, hence surrounded by spaces).</span></span>

<span data-ttu-id="2ec75-249">在单个行中声明的列表和数组应该在左方括号后面和右括号之前有一个空格：</span><span class="sxs-lookup"><span data-stu-id="2ec75-249">List and arrays declared on a single line should have a space after the opening bracket and before the closing bracket:</span></span>

```fsharp
let xs = [ 1; 2; 3 ]
let ys = [| 1; 2; 3; |]
```

<span data-ttu-id="2ec75-250">请始终在两个不同的大括号运算符之间至少使用一个空格。</span><span class="sxs-lookup"><span data-stu-id="2ec75-250">Always use at least one space between two distinct brace-like operators.</span></span> <span data-ttu-id="2ec75-251">例如，在和之间留一个空格 `[` `{` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-251">For example, leave a space between a `[` and a `{`.</span></span>

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

<span data-ttu-id="2ec75-252">相同的指导原则适用于元组的列表或数组。</span><span class="sxs-lookup"><span data-stu-id="2ec75-252">The same guideline applies for lists or arrays of tuples.</span></span>

<span data-ttu-id="2ec75-253">跨多个行拆分的列表和数组将遵循类似的规则作为记录：</span><span class="sxs-lookup"><span data-stu-id="2ec75-253">Lists and arrays that split across multiple lines follow a similar rule as records do:</span></span>

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

<span data-ttu-id="2ec75-254">与记录一样，将左括号和右括号声明到其自己的行上，可以更轻松地移动代码和管道转换为函数。</span><span class="sxs-lookup"><span data-stu-id="2ec75-254">And as with records, declaring the opening and closing brackets on their own line will make moving code around and piping into functions easier.</span></span>

<span data-ttu-id="2ec75-255">以编程方式生成数组和列表时，优先 `->` 于 `do ... yield` 始终生成值的时间：</span><span class="sxs-lookup"><span data-stu-id="2ec75-255">When generating arrays and lists programmatically, prefer `->` over `do ... yield` when a value is always generated:</span></span>

```fsharp
// Preferred
let squares = [ for x in 1..10 -> x * x ]

// Not preferred
let squares' = [ for x in 1..10 do yield x * x ]
```

<span data-ttu-id="2ec75-256">旧版本的 F # 语言需要 `yield` 在可能有条件地生成数据的情况下指定，或者可能存在连续的表达式进行计算。</span><span class="sxs-lookup"><span data-stu-id="2ec75-256">Older versions of the F# language required specifying `yield` in situations where data may be generated conditionally, or there may be consecutive expressions to be evaluated.</span></span> <span data-ttu-id="2ec75-257">`yield`如果你必须使用较旧的 F # 语言版本进行编译，则优先于忽略这些关键字：</span><span class="sxs-lookup"><span data-stu-id="2ec75-257">Prefer omitting these `yield` keywords unless you must compile with an older F# language version:</span></span>

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

<span data-ttu-id="2ec75-258">在某些情况下， `do...yield` 可能有助于提高可读性。</span><span class="sxs-lookup"><span data-stu-id="2ec75-258">In some cases, `do...yield` may aid in readability.</span></span> <span data-ttu-id="2ec75-259">尽管应考虑主观，但应考虑到这些情况。</span><span class="sxs-lookup"><span data-stu-id="2ec75-259">These cases, though subjective, should be taken into consideration.</span></span>

## <a name="formatting-if-expressions"></a><span data-ttu-id="2ec75-260">设置表达式的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-260">Formatting if expressions</span></span>

<span data-ttu-id="2ec75-261">条件的缩进取决于使其变得的表达式的大小和复杂程度。</span><span class="sxs-lookup"><span data-stu-id="2ec75-261">Indentation of conditionals depends on the size and complexity of the expressions that make them up.</span></span>
<span data-ttu-id="2ec75-262">在以下情况时，请将它们写入一行：</span><span class="sxs-lookup"><span data-stu-id="2ec75-262">Write them on one line when:</span></span>

- <span data-ttu-id="2ec75-263">`cond`、 `e1` 和 `e2` 为 short</span><span class="sxs-lookup"><span data-stu-id="2ec75-263">`cond`, `e1`, and `e2` are short</span></span>
- <span data-ttu-id="2ec75-264">`e1` 和 `e2` 不是 `if/then/else` 表达式本身。</span><span class="sxs-lookup"><span data-stu-id="2ec75-264">`e1` and `e2` are not `if/then/else` expressions themselves.</span></span>

```fsharp
if cond then e1 else e2
```

<span data-ttu-id="2ec75-265">如果任何表达式为多行或表达式，则为 `if/then/else` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-265">If any of the expressions are multi-line or `if/then/else` expressions.</span></span>

```fsharp
if cond then
    e1
else
    e2
```

<span data-ttu-id="2ec75-266">带有和的多个条件在 `elif` `else` `if` 遵循一个行表达式的规则时，将在与相同的范围内缩进 `if/then/else` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-266">Multiple conditionals with `elif` and `else` are indented at the same scope as the `if` when they follow the rules of the one line `if/then/else` expressions.</span></span>

```fsharp
if cond1 then e1
elif cond2 then e2
elif cond3 then e3
else e4
```

<span data-ttu-id="2ec75-267">如果有任何条件或表达式为多行，则整个 `if/then/else` 表达式为多行：</span><span class="sxs-lookup"><span data-stu-id="2ec75-267">If any of the conditions or expressions is multi-line, the entire `if/then/else` expression is multi-line:</span></span>

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

### <a name="pattern-matching-constructs"></a><span data-ttu-id="2ec75-268">模式匹配构造</span><span class="sxs-lookup"><span data-stu-id="2ec75-268">Pattern matching constructs</span></span>

<span data-ttu-id="2ec75-269">`|`对匹配的每个子句使用，无缩进。</span><span class="sxs-lookup"><span data-stu-id="2ec75-269">Use a `|` for each clause of a match with no indentation.</span></span> <span data-ttu-id="2ec75-270">如果表达式较短，则如果每个子表达式也简单，则可以考虑使用单个行。</span><span class="sxs-lookup"><span data-stu-id="2ec75-270">If the expression is short, you can consider using a single line if each subexpression is also simple.</span></span>

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

<span data-ttu-id="2ec75-271">如果模式匹配箭头右侧的表达式太大，则将其移动到下面的行，并从中缩进一个步骤 `match` / `|` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-271">If the expression on the right of the pattern matching arrow is too large, move it to the following line, indented one step from the `match`/`|`.</span></span>

```fsharp
match lam with
| Var v -> 1
| Abs(x, body) ->
    1 + sizeLambda body
| App(lam1, lam2) ->
    sizeLambda lam1 + sizeLambda lam2

```

<span data-ttu-id="2ec75-272">从开始，从开始，对匿名函数进行的模式匹配 `function` 通常不应缩进太远。</span><span class="sxs-lookup"><span data-stu-id="2ec75-272">Pattern matching of anonymous functions, starting by `function`, should generally not indent too far.</span></span> <span data-ttu-id="2ec75-273">例如，将一个作用域缩进如下所示：</span><span class="sxs-lookup"><span data-stu-id="2ec75-273">For example, indenting one scope as follows is fine:</span></span>

```fsharp
lambdaList
|> List.map
       (function
            | Abs(x, body) -> 1 + sizeLambda 0 body
            | App(lam1, lam2) -> sizeLambda (sizeLambda 0 lam1) lam2
            | Var v -> 1)
```

<span data-ttu-id="2ec75-274">即使使用关键字，使用定义的函数中的模式匹配 `let` 也 `let rec` 应缩进四个空格 `let` `function` ：</span><span class="sxs-lookup"><span data-stu-id="2ec75-274">Pattern matching in functions defined by `let` or `let rec` should be indented four spaces after starting of `let`, even if `function` keyword is used:</span></span>

```fsharp
let rec sizeLambda acc =
    function
    | Abs(x, body) -> sizeLambda (succ acc) body
    | App(lam1, lam2) -> sizeLambda (sizeLambda acc lam1) lam2
    | Var v -> succ acc
```

<span data-ttu-id="2ec75-275">建议不要对齐箭头。</span><span class="sxs-lookup"><span data-stu-id="2ec75-275">We do not recommend aligning arrows.</span></span>

## <a name="formatting-trywith-expressions"></a><span data-ttu-id="2ec75-276">格式 try/with 表达式</span><span class="sxs-lookup"><span data-stu-id="2ec75-276">Formatting try/with expressions</span></span>

<span data-ttu-id="2ec75-277">应将异常类型的模式匹配缩进到与相同的级别 `with` 。</span><span class="sxs-lookup"><span data-stu-id="2ec75-277">Pattern matching on the exception type should be indented at the same level as `with`.</span></span>

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

<span data-ttu-id="2ec75-278">始终 `|` 为每个子句添加，即使只有一个子句时也是如此。</span><span class="sxs-lookup"><span data-stu-id="2ec75-278">Always add a `|` for each clause, even when only having a single clause.</span></span>

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

## <a name="formatting-function-parameter-application"></a><span data-ttu-id="2ec75-279">格式化函数参数应用程序</span><span class="sxs-lookup"><span data-stu-id="2ec75-279">Formatting function parameter application</span></span>

<span data-ttu-id="2ec75-280">通常，大多数参数在同一行中提供：</span><span class="sxs-lookup"><span data-stu-id="2ec75-280">In general, most arguments are provided on the same line:</span></span>

```fsharp
let x = sprintf "\t%s - %i\n\r" x.IngredientName x.Quantity

let printListWithOffset a list1 =
    List.iter (fun elem -> printfn $"%d{a + elem}") list1
```

<span data-ttu-id="2ec75-281">当管道相关时，通常情况下也是如此，在这种情况下，扩充函数作为参数应用于同一行：</span><span class="sxs-lookup"><span data-stu-id="2ec75-281">When pipelines are concerned, the same is typically also true, where a curried function is applied as an argument on the same line:</span></span>

```
let printListWithOffsetPiped a list1 =
    list1
    |> List.iter (fun elem -> printfn $"%d{a + elem}")
```

<span data-ttu-id="2ec75-282">但是，你可能希望在新行上将参数传递给函数，这是一个可读性，或者参数列表太长。</span><span class="sxs-lookup"><span data-stu-id="2ec75-282">However, you may wish to pass arguments to a function on a new line, as a matter of readability or because the list of arguments or the argument names are too long.</span></span> <span data-ttu-id="2ec75-283">在这种情况下，将缩进一个作用域：</span><span class="sxs-lookup"><span data-stu-id="2ec75-283">In that case, indent with one scope:</span></span>

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

<span data-ttu-id="2ec75-284">对于 lambda 表达式，你可能还需要考虑将 lambda 表达式的主体放置在新行上（如果足够长，则按一个范围缩进）：</span><span class="sxs-lookup"><span data-stu-id="2ec75-284">For lambda expressions, you may also want to consider placing the body of a lambda expression on a new line, indented by one scope, if it is long enough:</span></span>

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

<span data-ttu-id="2ec75-285">如果 lambda 表达式的主体长度为多行，则应考虑将其重构为本地范围内的函数。</span><span class="sxs-lookup"><span data-stu-id="2ec75-285">If the body of a lambda expression is multiple lines long, you should consider refactoring it into a locally-scoped function.</span></span>

<span data-ttu-id="2ec75-286">参数通常应相对于函数或关键字缩进 `fun` / `function` ，而不考虑该函数显示在哪个上下文中：</span><span class="sxs-lookup"><span data-stu-id="2ec75-286">Parameters should generally be indented relative to the function or `fun`/`function` keyword, regardless of the context in which the function appears:</span></span>

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

<span data-ttu-id="2ec75-287">当函数采用单个多行元组参数时，适用于 [格式设置构造函数、静态成员和成员调用](#formatting-constructors-static-members-and-member-invocations) 的相同规则。</span><span class="sxs-lookup"><span data-stu-id="2ec75-287">When the function take a single multiline tuple argument, the same rules for [Formatting constructors, static members, and member invocations](#formatting-constructors-static-members-and-member-invocations) apply.</span></span>

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

### <a name="formatting-infix-operators"></a><span data-ttu-id="2ec75-288">格式化中缀运算符</span><span class="sxs-lookup"><span data-stu-id="2ec75-288">Formatting infix operators</span></span>

<span data-ttu-id="2ec75-289">用空格分隔运算符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-289">Separate operators by spaces.</span></span> <span data-ttu-id="2ec75-290">此规则的明显例外是 `!` 和 `.` 运算符。</span><span class="sxs-lookup"><span data-stu-id="2ec75-290">Obvious exceptions to this rule are the `!` and `.` operators.</span></span>

<span data-ttu-id="2ec75-291">中缀表达式可在同一列上进行阵容：</span><span class="sxs-lookup"><span data-stu-id="2ec75-291">Infix expressions are OK to lineup on same column:</span></span>

```fsharp
acc +
(sprintf "\t%s - %i\n\r"
     x.IngredientName x.Quantity)

let function1 arg1 arg2 arg3 arg4 =
    arg1 + arg2 +
    arg3 + arg4
```

### <a name="formatting-pipeline-operators-or-mutable-assignments"></a><span data-ttu-id="2ec75-292">设置管道运算符或可变赋值的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-292">Formatting pipeline operators or mutable assignments</span></span>

<span data-ttu-id="2ec75-293">管道 `|>` 运算符应位于它们所操作的表达式的下面。</span><span class="sxs-lookup"><span data-stu-id="2ec75-293">Pipeline `|>` operators should go underneath the expressions they operate on.</span></span>

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

<span data-ttu-id="2ec75-294">这也适用于可变资源库：</span><span class="sxs-lookup"><span data-stu-id="2ec75-294">This also applies to mutable setters:</span></span>

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

### <a name="formatting-modules"></a><span data-ttu-id="2ec75-295">格式化模块</span><span class="sxs-lookup"><span data-stu-id="2ec75-295">Formatting modules</span></span>

<span data-ttu-id="2ec75-296">本地模块中的代码必须相对于模块缩进，但不应缩进顶级模块中的代码。</span><span class="sxs-lookup"><span data-stu-id="2ec75-296">Code in a local module must be indented relative to the module, but code in a top-level module should not be indented.</span></span> <span data-ttu-id="2ec75-297">命名空间元素不必缩进。</span><span class="sxs-lookup"><span data-stu-id="2ec75-297">Namespace elements do not have to be indented.</span></span>

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

### <a name="formatting-object-expressions-and-interfaces"></a><span data-ttu-id="2ec75-298">设置对象表达式和接口的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-298">Formatting object expressions and interfaces</span></span>

<span data-ttu-id="2ec75-299">对象表达式和接口的对齐方式应与在 `member` 四个空格后缩进的方式相同。</span><span class="sxs-lookup"><span data-stu-id="2ec75-299">Object expressions and interfaces should be aligned in the same way with `member` being indented after four spaces.</span></span>

```fsharp
let comparer =
    { new IComparer<string> with
          member x.Compare(s1, s2) =
              let rev (s: String) =
                  new String (Array.rev (s.ToCharArray()))
              let reversed = rev s1
              reversed.CompareTo (rev s2) }
```

### <a name="formatting-white-space-in-expressions"></a><span data-ttu-id="2ec75-300">格式化表达式中的空白区域</span><span class="sxs-lookup"><span data-stu-id="2ec75-300">Formatting white space in expressions</span></span>

<span data-ttu-id="2ec75-301">避免 F # 表达式中有多余的空格。</span><span class="sxs-lookup"><span data-stu-id="2ec75-301">Avoid extraneous white space in F# expressions.</span></span>

```fsharp
// OK
spam (ham.[1])

// Not OK
spam ( ham.[ 1 ] )
```

<span data-ttu-id="2ec75-302">命名参数的周围还不应包含间距 `=` ：</span><span class="sxs-lookup"><span data-stu-id="2ec75-302">Named arguments should also not have space surrounding the `=`:</span></span>

```fsharp
// OK
let makeStreamReader x = new System.IO.StreamReader(path=x)

// Not OK
let makeStreamReader x = new System.IO.StreamReader(path = x)
```

### <a name="formatting-constructors-static-members-and-member-invocations"></a><span data-ttu-id="2ec75-303">格式化构造函数、静态成员和成员调用</span><span class="sxs-lookup"><span data-stu-id="2ec75-303">Formatting constructors, static members, and member invocations</span></span>

<span data-ttu-id="2ec75-304">如果表达式为 short，则用空格分隔参数，并将其保留在一行中。</span><span class="sxs-lookup"><span data-stu-id="2ec75-304">If the expression is short, separate arguments with spaces and keep it in one line.</span></span>

```fsharp
let person = new Person(a1, a2)

let myRegexMatch = Regex.Match(input, regex)

let untypedRes = checker.ParseFile(file, source, opts)
```

<span data-ttu-id="2ec75-305">如果表达式过长，则使用换行符并缩进一个作用域，而不是缩进到方括号。</span><span class="sxs-lookup"><span data-stu-id="2ec75-305">If the expression is long, use newlines and indent one scope, rather than indenting to the bracket.</span></span>

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

<span data-ttu-id="2ec75-306">即使只有一个多行参数，相同的规则也适用。</span><span class="sxs-lookup"><span data-stu-id="2ec75-306">The same rules apply even if there is only a single multiline argument.</span></span>

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

## <a name="formatting-generic-type-arguments-and-constraints"></a><span data-ttu-id="2ec75-307">设置泛型类型参数和约束的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-307">Formatting generic type arguments and constraints</span></span>

<span data-ttu-id="2ec75-308">下面的准则适用于函数、成员和类型定义。</span><span class="sxs-lookup"><span data-stu-id="2ec75-308">The guidelines below apply to both functions, members, and type definitions.</span></span>

<span data-ttu-id="2ec75-309">如果泛型类型参数和约束不太长，请将它们保留在一行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-309">Keep generic type arguments and constraints on a single line if it’s not too long:</span></span>

```fsharp
let f<'a, 'b when 'a : equality and 'b : comparison> param =
    // function body
```

<span data-ttu-id="2ec75-310">如果泛型类型参数/约束和函数参数均不适用，但仅有类型参数/约束，请将参数放置在新行上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-310">If both generic type arguments/constraints and function parameters don’t fit, but the type parameters/constraints alone do, place the parameters on new lines:</span></span>

```fsharp
let f<'a, 'b when 'a : equality and 'b : comparison>
    param
    =
    // function body
```

<span data-ttu-id="2ec75-311">如果类型参数或约束太长，请断开并对齐它们，如下所示。</span><span class="sxs-lookup"><span data-stu-id="2ec75-311">If the type parameters or constraints are too long, break and align them as shown below.</span></span> <span data-ttu-id="2ec75-312">将类型参数列表保留在函数所在的行上，而不考虑其长度。</span><span class="sxs-lookup"><span data-stu-id="2ec75-312">Keep the list of type parameters on the same line as the function, regardless of its length.</span></span> <span data-ttu-id="2ec75-313">对于约束，请将放 `when` 在第一行上，并在一行上保留每个约束，而不考虑其长度。</span><span class="sxs-lookup"><span data-stu-id="2ec75-313">For constraints, place `when` on the first line, and keep each constraint on a single line regardless of its length.</span></span> <span data-ttu-id="2ec75-314">置于 `>` 最后一行的末尾。</span><span class="sxs-lookup"><span data-stu-id="2ec75-314">Place `>` at the end of the last line.</span></span> <span data-ttu-id="2ec75-315">将约束按一级缩进。</span><span class="sxs-lookup"><span data-stu-id="2ec75-315">Indent the constraints by one level.</span></span>

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

<span data-ttu-id="2ec75-316">如果类型参数/约束被分解，但没有普通函数参数，请将置于 `=` 新行，而不考虑：</span><span class="sxs-lookup"><span data-stu-id="2ec75-316">If the type parameters/constraints are broken up, but there are no normal function parameters, place the `=` on a new line regardless:</span></span>

```f#
let inline f<^a, ^b
    when ^a : (static member Foo1: unit -> ^b)
    and ^b : (member Foo2: unit -> int)
    and ^b : (member Foo3: string -> ^a option)>
    =
    // function body
```

## <a name="formatting-attributes"></a><span data-ttu-id="2ec75-317">格式设置特性</span><span class="sxs-lookup"><span data-stu-id="2ec75-317">Formatting attributes</span></span>

<span data-ttu-id="2ec75-318">[特性](../language-reference/attributes.md) 位于构造之上：</span><span class="sxs-lookup"><span data-stu-id="2ec75-318">[Attributes](../language-reference/attributes.md) are placed above a construct:</span></span>

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

<span data-ttu-id="2ec75-319">它们应该在任何 XML 文档之后：</span><span class="sxs-lookup"><span data-stu-id="2ec75-319">They should go after any XML documentation:</span></span>

```fsharp
/// Module with some things in it.
[<RequireQualifiedAccess>]
module M =
    let f x = x
```

### <a name="formatting-attributes-on-parameters"></a><span data-ttu-id="2ec75-320">格式化参数上的特性</span><span class="sxs-lookup"><span data-stu-id="2ec75-320">Formatting attributes on parameters</span></span>

<span data-ttu-id="2ec75-321">还可以将属性放置在参数上。</span><span class="sxs-lookup"><span data-stu-id="2ec75-321">Attributes can also be placed on parameters.</span></span> <span data-ttu-id="2ec75-322">在这种情况下，请将其放在与参数相同的行上，并在名称之前：</span><span class="sxs-lookup"><span data-stu-id="2ec75-322">In this case, place then on the same line as the parameter and before the name:</span></span>

```fsharp
// Defines a class that takes an optional value as input defaulting to false.
type C() =
    member _.M([<Optional; DefaultParameterValue(false)>] doSomething: bool)
```

### <a name="formatting-multiple-attributes"></a><span data-ttu-id="2ec75-323">设置多个属性的格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-323">Formatting multiple attributes</span></span>

<span data-ttu-id="2ec75-324">如果将多个特性应用于不是参数的构造，则应将它们放在每行中都有一个属性：</span><span class="sxs-lookup"><span data-stu-id="2ec75-324">When multiple attributes are applied to a construct that is not a parameter, they should be placed such that there is one attribute per line:</span></span>

```fsharp
[<Struct>]
[<IsByRefLike>]
type MyRecord =
    { Label1: int
      Label2: string }
```

<span data-ttu-id="2ec75-325">当应用于参数时，它们必须位于同一行上并由 `;` 分隔符分隔。</span><span class="sxs-lookup"><span data-stu-id="2ec75-325">When applied to a parameter, they must be on the same line and separated by a `;` separator.</span></span>

## <a name="formatting-literals"></a><span data-ttu-id="2ec75-326">设置文本格式</span><span class="sxs-lookup"><span data-stu-id="2ec75-326">Formatting literals</span></span>

<span data-ttu-id="2ec75-327">使用特性的[F # 文本](../language-reference/literals.md) `Literal` 应将属性置于其自己的行上，并使用 PascalCase 命名：</span><span class="sxs-lookup"><span data-stu-id="2ec75-327">[F# literals](../language-reference/literals.md) using the `Literal` attribute should place the attribute on its own line and use PascalCase naming:</span></span>

```fsharp
[<Literal>]
let Path = __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let MyUrl = "www.mywebsitethatiamworkingwith.com"
```

<span data-ttu-id="2ec75-328">避免将属性放置在与值相同的行上。</span><span class="sxs-lookup"><span data-stu-id="2ec75-328">Avoid placing the attribute on the same line as the value.</span></span>

## <a name="formatting-computation-expression-operations"></a><span data-ttu-id="2ec75-329">格式化计算表达式操作</span><span class="sxs-lookup"><span data-stu-id="2ec75-329">Formatting computation expression operations</span></span>

<span data-ttu-id="2ec75-330">为 [计算表达式](../language-reference/computation-expressions.md)创建自定义操作时，建议使用 camelCase 命名：</span><span class="sxs-lookup"><span data-stu-id="2ec75-330">When creating custom operations for [computation expressions](../language-reference/computation-expressions.md), it is recommended to use camelCase naming:</span></span>

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

<span data-ttu-id="2ec75-331">正在建模的域应最终驱动命名约定。</span><span class="sxs-lookup"><span data-stu-id="2ec75-331">The domain that's being modeled should ultimately drive the naming convention.</span></span>
<span data-ttu-id="2ec75-332">如果惯用使用不同的约定，应改为使用该约定。</span><span class="sxs-lookup"><span data-stu-id="2ec75-332">If it is idiomatic to use a different convention, that convention should be used instead.</span></span>
