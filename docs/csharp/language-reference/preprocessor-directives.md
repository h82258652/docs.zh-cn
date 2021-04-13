---
description: 了解控制条件编译、警告、可为空分析等的不同 C# 预处理器指令
title: C# 预处理器指令
ms.date: 03/17/2021
f1_keywords:
- cs.preprocessor
- '#nullable'
- '#if'
- '#else'
- '#elif'
- '#endif'
- '#define'
- '#undef'
- '#warning'
- '#error'
- '#line'
- '#region'
- '#endregion'
- '#pragma'
- '#pragma warning'
- '#pragma checksum'
helpviewer_keywords:
- preprocessor directives [C#]
- keywords [C#], preprocessor directives
- '#nullable directive [C#]'
- '#if directive [C#]'
- '#else directive [C#]'
- '#elif directive [C#]'
- '#endif directive [C#]'
- '#define directive [C#]'
- '#undef directive [C#]'
- '#warning directive [C#]'
- '#error directive [C#]'
- '#line directive [C#]'
- '#region directive [C#]'
- '#endregion directive [C#]'
- '#pragma directive [C#]'
- '#pragma warning [C#]'
- '#pragma checksum [C#]'
ms.openlocfilehash: 373952282a684da25414af9853e18b7bc4874108
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637655"
---
# <a name="c-preprocessor-directives"></a><span data-ttu-id="87114-103">C# 预处理器指令</span><span class="sxs-lookup"><span data-stu-id="87114-103">C# preprocessor directives</span></span>

<span data-ttu-id="87114-104">尽管编译器没有单独的预处理器，但本节中所述指令的处理方式与有预处理器时一样。</span><span class="sxs-lookup"><span data-stu-id="87114-104">Although the compiler doesn't have a separate preprocessor, the directives described in this section are processed as if there were one.</span></span> <span data-ttu-id="87114-105">可使用这些指令来帮助条件编译。</span><span class="sxs-lookup"><span data-stu-id="87114-105">You use them to help in conditional compilation.</span></span> <span data-ttu-id="87114-106">不同于 C 和 C++ 指令，不能使用这些指令来创建宏。</span><span class="sxs-lookup"><span data-stu-id="87114-106">Unlike C and C++ directives, you can't use these directives to create macros.</span></span> <span data-ttu-id="87114-107">预处理器指令必须是一行中唯一的说明。</span><span class="sxs-lookup"><span data-stu-id="87114-107">A preprocessor directive must be the only instruction on a line.</span></span>

## <a name="nullable-context"></a><span data-ttu-id="87114-108">可为空上下文</span><span class="sxs-lookup"><span data-stu-id="87114-108">Nullable context</span></span>

<span data-ttu-id="87114-109">`#nullable` 预处理器指令将设置可为空注释上下文和可为空警告上下文 。</span><span class="sxs-lookup"><span data-stu-id="87114-109">The `#nullable` preprocessor directive sets the *nullable annotation context* and *nullable warning context*.</span></span> <span data-ttu-id="87114-110">此指令控制是否可为空注释是否有效，以及是否给出为 Null 性警告。</span><span class="sxs-lookup"><span data-stu-id="87114-110">This directive controls whether nullable annotations have effect, and whether nullability warnings are given.</span></span> <span data-ttu-id="87114-111">每个上下文要么处于已禁用状态，要么处于已启用状态 。</span><span class="sxs-lookup"><span data-stu-id="87114-111">Each context is either *disabled* or *enabled*.</span></span>

<span data-ttu-id="87114-112">可在项目级别（C# 源代码之外）指定这两个上下文。</span><span class="sxs-lookup"><span data-stu-id="87114-112">Both contexts can be specified at the project level (outside of C# source code).</span></span> <span data-ttu-id="87114-113">`#nullable` 指令控制注释和警告上下文，并优先于项目级设置。</span><span class="sxs-lookup"><span data-stu-id="87114-113">The `#nullable` directive controls the annotation and warning contexts and takes precedence over the project-level settings.</span></span> <span data-ttu-id="87114-114">指令会设置其控制的上下文，直到另一个指令替代它，或直到源文件结束为止。</span><span class="sxs-lookup"><span data-stu-id="87114-114">A directive sets the context(s) it controls until another directive overrides it, or until the end of the source file.</span></span>

<span data-ttu-id="87114-115">指令的效果如下所示：</span><span class="sxs-lookup"><span data-stu-id="87114-115">The effect of the directives is as follows:</span></span>

- <span data-ttu-id="87114-116">`#nullable disable`：将可为空注释和警告上下文设置为“已禁用”。</span><span class="sxs-lookup"><span data-stu-id="87114-116">`#nullable disable`: Sets the nullable annotation and warning contexts to *disabled*.</span></span>
- <span data-ttu-id="87114-117">`#nullable enable`：将可为空注释和警告上下文设置为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="87114-117">`#nullable enable`: Sets the nullable annotation and warning contexts to *enabled*.</span></span>
- <span data-ttu-id="87114-118">`#nullable restore`：将可为空注释和警告上下文还原为项目设置。</span><span class="sxs-lookup"><span data-stu-id="87114-118">`#nullable restore`: Restores the nullable annotation and warning contexts to project settings.</span></span>
- <span data-ttu-id="87114-119">`#nullable disable annotations`：将可为空注释上下文设置为“已禁用”。</span><span class="sxs-lookup"><span data-stu-id="87114-119">`#nullable disable annotations`: Sets the nullable annotation context to *disabled*.</span></span>
- <span data-ttu-id="87114-120">`#nullable enable annotations`：将可为空注释上下文设置为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="87114-120">`#nullable enable annotations`: Sets the nullable annotation context to *enabled*.</span></span>
- <span data-ttu-id="87114-121">`#nullable restore annotations`：将可为空注释上下文还原为项目设置。</span><span class="sxs-lookup"><span data-stu-id="87114-121">`#nullable restore annotations`: Restores the nullable annotation context to project settings.</span></span>
- <span data-ttu-id="87114-122">`#nullable disable warnings`：将可为空警告上下文设置为“已禁用”。</span><span class="sxs-lookup"><span data-stu-id="87114-122">`#nullable disable warnings`: Sets the nullable warning context to *disabled*.</span></span>
- <span data-ttu-id="87114-123">`#nullable enable warnings`：将可为空警告上下文设置为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="87114-123">`#nullable enable warnings`: Sets the nullable warning context to *enabled*.</span></span>
- <span data-ttu-id="87114-124">`#nullable restore warnings`：将可为空警告上下文还原为项目设置。</span><span class="sxs-lookup"><span data-stu-id="87114-124">`#nullable restore warnings`: Restores the nullable warning context to project settings.</span></span>

## <a name="conditional-compilation"></a><span data-ttu-id="87114-125">条件编译</span><span class="sxs-lookup"><span data-stu-id="87114-125">Conditional compilation</span></span>

<span data-ttu-id="87114-126">使用四个预处理器指令来控制条件编译：</span><span class="sxs-lookup"><span data-stu-id="87114-126">You use four preprocessor directives to control conditional compilation:</span></span>

- <span data-ttu-id="87114-127">`#if`：打开条件编译，其中仅在定义了指定的符号时才会编译代码。</span><span class="sxs-lookup"><span data-stu-id="87114-127">`#if`: Opens a conditional compilation, where code is compiled only if the specified symbol is defined.</span></span>
- <span data-ttu-id="87114-128">`#elif`：关闭前面的条件编译，并基于是否定义了指定的符号打开一个新的条件编译。</span><span class="sxs-lookup"><span data-stu-id="87114-128">`#elif`: Closes the preceding conditional compilation and opens a new conditional compilation based on if the specified symbol is defined.</span></span>
- <span data-ttu-id="87114-129">`#else`：关闭前面的条件编译，如果没有定义前面指定的符号，打开一个新的条件编译。</span><span class="sxs-lookup"><span data-stu-id="87114-129">`#else`: Closes the preceding conditional compilation and opens a new conditional compilation if the previous specified symbol isn't defined.</span></span>
- <span data-ttu-id="87114-130">`#endif`：关闭前面的条件编译。</span><span class="sxs-lookup"><span data-stu-id="87114-130">`#endif`: Closes the preceding conditional compilation.</span></span>

<span data-ttu-id="87114-131">如果 C# 编译器遇到 `#if` 指令，最后跟着一个 `#endif` 指令，则仅当定义指定的符号时，它才编译这些指令之间的代码。</span><span class="sxs-lookup"><span data-stu-id="87114-131">When the C# compiler finds an `#if` directive, followed eventually by an `#endif` directive, it compiles the code between the directives only if the specified symbol is defined.</span></span> <span data-ttu-id="87114-132">与 C 和 C++ 不同，不能将数字值分配给符号。</span><span class="sxs-lookup"><span data-stu-id="87114-132">Unlike C and C++, you can't assign a numeric value to a symbol.</span></span> <span data-ttu-id="87114-133">C# 中的 `#if` 语句是布尔值，且仅测试是否已定义该符号。</span><span class="sxs-lookup"><span data-stu-id="87114-133">The `#if` statement in C# is Boolean and only tests whether the symbol has been defined or not.</span></span> <span data-ttu-id="87114-134">例如：</span><span class="sxs-lookup"><span data-stu-id="87114-134">For example:</span></span>

```csharp
#if DEBUG
    Console.WriteLine("Debug version");
#endif
```

<span data-ttu-id="87114-135">可以使用运算符 [`==`（相等）](operators/equality-operators.md#equality-operator-)和 [`!=`（不相等）](operators/equality-operators.md#inequality-operator-)来测试 [ `bool`](builtin-types/bool.md) 值是 `true` 还是 `false`。</span><span class="sxs-lookup"><span data-stu-id="87114-135">You can use the operators [`==` (equality)](operators/equality-operators.md#equality-operator-) and [`!=` (inequality)](operators/equality-operators.md#inequality-operator-) to test for the [`bool`](builtin-types/bool.md) values `true` or `false`.</span></span> <span data-ttu-id="87114-136">`true` 表示定义该符号。</span><span class="sxs-lookup"><span data-stu-id="87114-136">`true` means the symbol is defined.</span></span> <span data-ttu-id="87114-137">语句 `#if DEBUG` 具有与 `#if (DEBUG == true)` 相同的含义。</span><span class="sxs-lookup"><span data-stu-id="87114-137">The statement `#if DEBUG` has the same meaning as `#if (DEBUG == true)`.</span></span> <span data-ttu-id="87114-138">可以使用 [`&&` (and)](operators/boolean-logical-operators.md#conditional-logical-and-operator-)、[`||` (or)](operators/boolean-logical-operators.md#conditional-logical-or-operator-) 和 [`!`(not)](operators/boolean-logical-operators.md#logical-negation-operator-) 运算符来计算是否已定义多个符号。</span><span class="sxs-lookup"><span data-stu-id="87114-138">You can use the [`&&` (and)](operators/boolean-logical-operators.md#conditional-logical-and-operator-), [`||` (or)](operators/boolean-logical-operators.md#conditional-logical-or-operator-), and [`!` (not)](operators/boolean-logical-operators.md#logical-negation-operator-) operators to evaluate whether multiple symbols have been defined.</span></span> <span data-ttu-id="87114-139">还可以用括号对符号和运算符进行分组。</span><span class="sxs-lookup"><span data-stu-id="87114-139">You can also group symbols and operators with parentheses.</span></span>

<span data-ttu-id="87114-140">`#if` 以及 `#else`、`#elif`、`#endif`、`#define` 和 `#undef` 指令，允许基于是否存在一个或多个符号包括或排除代码。</span><span class="sxs-lookup"><span data-stu-id="87114-140">`#if`, along with the `#else`, `#elif`, `#endif`, `#define`, and `#undef` directives, lets you include or exclude code based on the existence of one or more symbols.</span></span> <span data-ttu-id="87114-141">条件编译在编译调试版本的代码或编译特定配置的代码时会很有用。</span><span class="sxs-lookup"><span data-stu-id="87114-141">Conditional compilation can be useful when compiling code for a debug build or when compiling for a specific configuration.</span></span>

<span data-ttu-id="87114-142">以 `#if` 指令开头的条件指令必须以 `#endif` 指令显式终止。</span><span class="sxs-lookup"><span data-stu-id="87114-142">A conditional directive beginning with an `#if` directive must explicitly be terminated with an `#endif` directive.</span></span> <span data-ttu-id="87114-143">`#define` 允许你定义一个符号。</span><span class="sxs-lookup"><span data-stu-id="87114-143">`#define` lets you define a symbol.</span></span> <span data-ttu-id="87114-144">通过将该符号用作传递给 `#if` 指令的表达式，该表达式的计算结果为 `true`。</span><span class="sxs-lookup"><span data-stu-id="87114-144">By using the symbol as the expression passed to the `#if` directive, the expression evaluates to `true`.</span></span> <span data-ttu-id="87114-145">还可以通过 [DefineConstants](compiler-options/language.md#defineconstants) 编译器选项来定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-145">You can also define a symbol with the [**DefineConstants**](compiler-options/language.md#defineconstants) compiler option.</span></span> <span data-ttu-id="87114-146">可以通过 `#undef` 取消定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-146">You can undefine a symbol with `#undef`.</span></span> <span data-ttu-id="87114-147">使用 `#define` 创建的符号的作用域是在其中定义它的文件。</span><span class="sxs-lookup"><span data-stu-id="87114-147">The scope of a symbol created with `#define` is the file in which it was defined.</span></span> <span data-ttu-id="87114-148">使用 DefineConstants 或 `#define` 定义的符号与具有相同名称的变量不冲突。</span><span class="sxs-lookup"><span data-stu-id="87114-148">A symbol that you define with **DefineConstants** or with `#define` doesn't conflict with a variable of the same name.</span></span> <span data-ttu-id="87114-149">也就是说，变量名称不应传递给预处理器指令，且符号仅能由预处理器指令评估。</span><span class="sxs-lookup"><span data-stu-id="87114-149">That is, a variable name shouldn't be passed to a preprocessor directive, and a symbol can only be evaluated by a preprocessor directive.</span></span>

<span data-ttu-id="87114-150">`#elif` 可以创建复合条件指令。</span><span class="sxs-lookup"><span data-stu-id="87114-150">`#elif` lets you create a compound conditional directive.</span></span> <span data-ttu-id="87114-151">如果之前的 `#if` 和任何之前的可选 `#elif` 指令表达式的值都不为 `true`，则计算 `#elif` 表达式。</span><span class="sxs-lookup"><span data-stu-id="87114-151">The `#elif` expression will be evaluated if neither the preceding `#if` nor any preceding, optional, `#elif` directive expressions evaluate to `true`.</span></span> <span data-ttu-id="87114-152">如果 `#elif` 表达式计算结果为 `true`，编译器将计算 `#elif` 和下一条件指令间的所有代码。</span><span class="sxs-lookup"><span data-stu-id="87114-152">If an `#elif` expression evaluates to `true`, the compiler evaluates all the code between the `#elif` and the next conditional directive.</span></span> <span data-ttu-id="87114-153">例如：</span><span class="sxs-lookup"><span data-stu-id="87114-153">For example:</span></span>

```csharp
#define VC7
//...
#if debug
    Console.WriteLine("Debug build");
#elif VC7
    Console.WriteLine("Visual Studio 7");
#endif
```

<span data-ttu-id="87114-154">`#else` 允许创建复合条件指令，因此，如果先前 `#if` 或（可选）`#elif` 指令中的任何表达式的计算结果都不是 `true`，则编译器将对介于 `#else` 和下一个 `#endif` 之间的所有代码进行求值。</span><span class="sxs-lookup"><span data-stu-id="87114-154">`#else` lets you create a compound conditional directive, so that, if none of the expressions in the preceding `#if` or (optional) `#elif` directives evaluate to `true`, the compiler will evaluate all code between `#else` and the next `#endif`.</span></span> <span data-ttu-id="87114-155">`#endif`(#endif) 必须是 `#else` 之后的下一个预处理器指令。</span><span class="sxs-lookup"><span data-stu-id="87114-155">`#endif`(#endif) must be the next preprocessor directive after `#else`.</span></span>

<span data-ttu-id="87114-156">`#endif` 指定条件指令的末尾，以 `#if` 指令开头。</span><span class="sxs-lookup"><span data-stu-id="87114-156">`#endif` specifies the end of a conditional directive, which began with the `#if` directive.</span></span>

<span data-ttu-id="87114-157">此外，生成系统还会感知表示 SDK 样式项目中不同[目标框架](../../standard/frameworks.md)的预定义预处理器符号。</span><span class="sxs-lookup"><span data-stu-id="87114-157">The build system is also aware of predefined preprocessor symbols representing different [target frameworks](../../standard/frameworks.md) in SDK-style projects.</span></span> <span data-ttu-id="87114-158">在创建可以面向多个 .NET 版本的应用程序时，这些符号会很有用。</span><span class="sxs-lookup"><span data-stu-id="87114-158">They're useful when creating applications that can target more than one .NET version.</span></span>

[!INCLUDE [Preprocessor symbols](~/includes/preprocessor-symbols.md)]

> [!NOTE]
> <span data-ttu-id="87114-159">对于传统的非 SDK 样式的项目，必须通过项目的属性页面在 Visual Studio 中为不同目标框架手动配置条件编译符号。</span><span class="sxs-lookup"><span data-stu-id="87114-159">For traditional, non-SDK-style projects, you have to manually configure the conditional compilation symbols for the different target frameworks in Visual Studio via the project's properties pages.</span></span>

<span data-ttu-id="87114-160">其他预定义符号包括 `DEBUG` 和 `TRACE` 常数。</span><span class="sxs-lookup"><span data-stu-id="87114-160">Other predefined symbols include the `DEBUG` and `TRACE` constants.</span></span> <span data-ttu-id="87114-161">你可以使用 `#define` 替代项目的值集。</span><span class="sxs-lookup"><span data-stu-id="87114-161">You can override the values set for the project using `#define`.</span></span> <span data-ttu-id="87114-162">例如，会根据生成配置属性（“调试”或者“发布”模式）自动设置 DEBUG 符号。</span><span class="sxs-lookup"><span data-stu-id="87114-162">The DEBUG symbol, for example, is automatically set depending on your build configuration properties ("Debug" or "Release" mode).</span></span>

<span data-ttu-id="87114-163">下例显示如何在文件上定义 `MYTEST` 符号，然后测试 `MYTEST` 和 `DEBUG` 符号的值。</span><span class="sxs-lookup"><span data-stu-id="87114-163">The following example shows you how to define a `MYTEST` symbol on a file and then test the values of the `MYTEST` and `DEBUG` symbols.</span></span> <span data-ttu-id="87114-164">此示例的输出取决于是在“调试”还是“发布”配置模式下生成项目 。</span><span class="sxs-lookup"><span data-stu-id="87114-164">The output of this example depends on whether you built the project on **Debug** or **Release** configuration mode.</span></span>

```csharp
#define MYTEST
using System;
public class MyClass
{
    static void Main()
    {
#if (DEBUG && !MYTEST)
        Console.WriteLine("DEBUG is defined");
#elif (!DEBUG && MYTEST)
        Console.WriteLine("MYTEST is defined");
#elif (DEBUG && MYTEST)
        Console.WriteLine("DEBUG and MYTEST are defined");  
#else
        Console.WriteLine("DEBUG and MYTEST are not defined");
#endif
    }
}
```

<span data-ttu-id="87114-165">下例显示如何针对不同的目标框架进行测试，以便在可能时使用较新的 API：</span><span class="sxs-lookup"><span data-stu-id="87114-165">The following example shows you how to test for different target frameworks so you can use newer APIs when possible:</span></span>

```csharp
public class MyClass
{
    static void Main()
    {
#if NET40
        WebClient _client = new WebClient();
#else
        HttpClient _client = new HttpClient();
#endif
    }
    //...
}
```

## <a name="defining-symbols"></a><span data-ttu-id="87114-166">定义符号</span><span class="sxs-lookup"><span data-stu-id="87114-166">Defining symbols</span></span>

<span data-ttu-id="87114-167">使用以下两个预处理器指令来定义或取消定义条件编译的符号：</span><span class="sxs-lookup"><span data-stu-id="87114-167">You use the following two preprocessor directives to define or undefine symbols for conditional compilation:</span></span>

- <span data-ttu-id="87114-168">`#define`：定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-168">`#define`: Define a symbol.</span></span>
- <span data-ttu-id="87114-169">`#undef`：取消定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-169">`#undef`: undefine a symbol.</span></span>

<span data-ttu-id="87114-170">使用 `#define` 来定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-170">You use `#define` to define a symbol.</span></span> <span data-ttu-id="87114-171">将符号用作传递给 `#if` 指令的表达式时，该表达式的计算结果为 `true`，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="87114-171">When you use the symbol as the expression that's passed to the `#if` directive, the expression will evaluate to `true`, as the following example shows:</span></span>

 ```csharp
 #define VERBOSE

#if VERBOSE
    Console.WriteLine("Verbose output version");
#endif

 ```

> [!NOTE]
> <span data-ttu-id="87114-172">`#define` 指令不能用于声明常量值，这与 C 和 C++ 中的通常做法一样。</span><span class="sxs-lookup"><span data-stu-id="87114-172">The `#define` directive cannot be used to declare constant values as is typically done in C and C++.</span></span> <span data-ttu-id="87114-173">C# 中的常量最好定义为类或结构的静态成员。</span><span class="sxs-lookup"><span data-stu-id="87114-173">Constants in C# are best defined as static members of a class or struct.</span></span> <span data-ttu-id="87114-174">如果具有多个此类常量，请考虑创建一个单独的“常量”类来容纳它们。</span><span class="sxs-lookup"><span data-stu-id="87114-174">If you have several such constants, consider creating a separate "Constants" class to hold them.</span></span>

<span data-ttu-id="87114-175">符号可用于指定编译的条件。</span><span class="sxs-lookup"><span data-stu-id="87114-175">Symbols can be used to specify conditions for compilation.</span></span> <span data-ttu-id="87114-176">可通过 `#if` 或 `#elif` 测试符号。</span><span class="sxs-lookup"><span data-stu-id="87114-176">You can test for the symbol with either `#if` or `#elif`.</span></span> <span data-ttu-id="87114-177">还可以使用 <xref:System.Diagnostics.ConditionalAttribute> 来执行条件编译。</span><span class="sxs-lookup"><span data-stu-id="87114-177">You can also use the <xref:System.Diagnostics.ConditionalAttribute> to perform conditional compilation.</span></span> <span data-ttu-id="87114-178">可以定义符号，但不能为符号分配值。</span><span class="sxs-lookup"><span data-stu-id="87114-178">You can define a symbol, but you can't assign a value to a symbol.</span></span> <span data-ttu-id="87114-179">文件中必须先出现 `#define` 指令，才能使用并非同时也是预处理器指令的任何指示。</span><span class="sxs-lookup"><span data-stu-id="87114-179">The `#define` directive must appear in the file before you use any instructions that aren't also preprocessor directives.</span></span> <span data-ttu-id="87114-180">还可以通过 [DefineConstants](compiler-options/language.md#defineconstants) 编译器选项来定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-180">You can also define a symbol with the [**DefineConstants**](compiler-options/language.md#defineconstants) compiler option.</span></span> <span data-ttu-id="87114-181">可以通过 `#undef` 取消定义符号。</span><span class="sxs-lookup"><span data-stu-id="87114-181">You can undefine a symbol with `#undef`.</span></span>

## <a name="defining-regions"></a><span data-ttu-id="87114-182">定义区域</span><span class="sxs-lookup"><span data-stu-id="87114-182">Defining regions</span></span>

<span data-ttu-id="87114-183">可以使用以下两个预处理器指令来定义可在大纲中折叠的代码区域：</span><span class="sxs-lookup"><span data-stu-id="87114-183">You can define regions of code that can be collapsed in an outline using the following two preprocessor directives:</span></span>

- <span data-ttu-id="87114-184">`#region`：启动区域。</span><span class="sxs-lookup"><span data-stu-id="87114-184">`#region`: Start a region.</span></span>
- <span data-ttu-id="87114-185">`#endregion`：结束区域</span><span class="sxs-lookup"><span data-stu-id="87114-185">`#endregion`: End a region</span></span>

<span data-ttu-id="87114-186">利用 `#region`，可以指定在使用代码编辑器的[大纲](/visualstudio/ide/outlining)功能时可展开或折叠的代码块。</span><span class="sxs-lookup"><span data-stu-id="87114-186">`#region` lets you specify a block of code that you can expand or collapse when using the [outlining](/visualstudio/ide/outlining) feature of the code editor.</span></span> <span data-ttu-id="87114-187">在较长的代码文件中，折叠或隐藏一个或多个区域十分便利，这样，可将精力集中于当前处理的文件部分。</span><span class="sxs-lookup"><span data-stu-id="87114-187">In longer code files, it's convenient to collapse or hide one or more regions so that you can focus on the part of the file that you're currently working on.</span></span> <span data-ttu-id="87114-188">下面的示例演示如何定义区域：</span><span class="sxs-lookup"><span data-stu-id="87114-188">The following example shows how to define a region:</span></span>

```csharp
#region MyClass definition
public class MyClass
{
    static void Main()
    {
    }
}
#endregion
```

<span data-ttu-id="87114-189">`#region` 块必须通过 `#endregion` 指令终止。</span><span class="sxs-lookup"><span data-stu-id="87114-189">A `#region` block must be terminated with an `#endregion` directive.</span></span> <span data-ttu-id="87114-190">`#region` 块不能与 `#if` 块重叠。</span><span class="sxs-lookup"><span data-stu-id="87114-190">A `#region` block can't overlap with an `#if` block.</span></span> <span data-ttu-id="87114-191">但是，可以将 `#region` 块嵌套在 `#if` 块内，或将 `#if` 块嵌套在 `#region` 块内。</span><span class="sxs-lookup"><span data-stu-id="87114-191">However, a `#region` block can be nested in an `#if` block, and an `#if` block can be nested in a `#region` block.</span></span>

## <a name="error-and-warning-information"></a><span data-ttu-id="87114-192">错误和警告信息</span><span class="sxs-lookup"><span data-stu-id="87114-192">Error and warning information</span></span>

<span data-ttu-id="87114-193">使用以下指令指示编译器生成用户定义的编译器错误和警告，并控制行信息：</span><span class="sxs-lookup"><span data-stu-id="87114-193">You instruct the compiler to generate user-defined compiler errors and warnings, and control line information using the following directives:</span></span>

- <span data-ttu-id="87114-194">`#error`：使用指定的消息生成编译器错误。</span><span class="sxs-lookup"><span data-stu-id="87114-194">`#error`: Generate a compiler error with a specified message.</span></span>
- <span data-ttu-id="87114-195">`#warning`：使用指定的消息生成编译器警告。</span><span class="sxs-lookup"><span data-stu-id="87114-195">`#warning`: Generate a compiler warning, with a specific message.</span></span>
- <span data-ttu-id="87114-196">`#line`：更改用编译器消息输出的行号。</span><span class="sxs-lookup"><span data-stu-id="87114-196">`#line`: Change the line number printed with compiler messages.</span></span>

<span data-ttu-id="87114-197">`#error` 可从代码中的特定位置生成 [CS1029](compiler-messages/cs1029.md) 用户定义的错误。</span><span class="sxs-lookup"><span data-stu-id="87114-197">`#error` lets you generate a [CS1029](compiler-messages/cs1029.md) user-defined error from a specific location in your code.</span></span> <span data-ttu-id="87114-198">例如：</span><span class="sxs-lookup"><span data-stu-id="87114-198">For example:</span></span>

```csharp
#error Deprecated code in this method.
```

> [!NOTE]
> <span data-ttu-id="87114-199">编译器以特殊的方式处理 `#error version` 并报告编译器错误 CS8304，消息中包含使用的编译器和语言版本。</span><span class="sxs-lookup"><span data-stu-id="87114-199">The compiler treats `#error version` in a special way and reports a compiler error, CS8304, with a message containing the used compiler and language versions.</span></span>

<span data-ttu-id="87114-200">`#warning` 允许你从代码中的特定位置生成 [ CS1030](../misc/cs1030.md) 第一级编译器警告。</span><span class="sxs-lookup"><span data-stu-id="87114-200">`#warning` lets you generate a [CS1030](../misc/cs1030.md) level one compiler warning from a specific location in your code.</span></span> <span data-ttu-id="87114-201">例如：</span><span class="sxs-lookup"><span data-stu-id="87114-201">For example:</span></span>

```csharp
#warning Deprecated code in this method.
```

<span data-ttu-id="87114-202">借助 `#line`，可修改编译器的行号及（可选）用于错误和警告的文件名输出。</span><span class="sxs-lookup"><span data-stu-id="87114-202">`#line` lets you modify the compiler's line numbering and (optionally) the file name output for errors and warnings.</span></span>

<span data-ttu-id="87114-203">以下示例演示如何报告与行号相关联的两个警告。</span><span class="sxs-lookup"><span data-stu-id="87114-203">The following example shows how to report two warnings associated with line numbers.</span></span> <span data-ttu-id="87114-204">`#line 200` 指令将下一行的行号强制设为 200（尽管默认值为 #6）；在执行下一个 `#line` 指令前，文件名都会报告为“特殊”。</span><span class="sxs-lookup"><span data-stu-id="87114-204">The `#line 200` directive forces the next line's number to be 200 (although the default is #6), and until the next `#line` directive, the filename will be reported as "Special".</span></span> <span data-ttu-id="87114-205">`#line default` 指令将行号恢复至默认行号，这会对上一指令重新编号的行进行计数。</span><span class="sxs-lookup"><span data-stu-id="87114-205">The `#line default` directive returns the line numbering to its default numbering, which counts the lines that were renumbered by the previous directive.</span></span>

```csharp
class MainClass
{
    static void Main()
    {
#line 200 "Special"
        int i;
        int j;
#line default
        char c;
        float f;
#line hidden // numbering not affected
        string s;
        double d;
    }
}
```

<span data-ttu-id="87114-206">编译产生以下输出：</span><span class="sxs-lookup"><span data-stu-id="87114-206">Compilation produces the following output:</span></span>

```console
Special(200,13): warning CS0168: The variable 'i' is declared but never used
Special(201,13): warning CS0168: The variable 'j' is declared but never used
MainClass.cs(9,14): warning CS0168: The variable 'c' is declared but never used
MainClass.cs(10,15): warning CS0168: The variable 'f' is declared but never used
MainClass.cs(12,16): warning CS0168: The variable 's' is declared but never used
MainClass.cs(13,16): warning CS0168: The variable 'd' is declared but never used
```

<span data-ttu-id="87114-207">可在生成过程的自动、中间步骤中使用 `#line` 指令。</span><span class="sxs-lookup"><span data-stu-id="87114-207">The `#line` directive might be used in an automated, intermediate step in the build process.</span></span> <span data-ttu-id="87114-208">例如，如果已从原始源代码文件中删除行，但仍希望编译器基于文件中的原始行号生成输出，可在删除行后，使用 `#line` 来模拟原始行号。</span><span class="sxs-lookup"><span data-stu-id="87114-208">For example, if lines were removed from the original source code file, but you still wanted the compiler to generate output based on the original line numbering in the file, you could remove lines and then simulate the original line numbering with `#line`.</span></span>

<span data-ttu-id="87114-209">`#line hidden` 指令能对调试程序隐藏连续行，当开发者逐行执行代码时，介于 `#line hidden` 和下一 `#line` 指令（假设它不是其他 `#line hidden` 指令）间的任何行都将被跳过。</span><span class="sxs-lookup"><span data-stu-id="87114-209">The `#line hidden` directive hides the successive lines from the debugger, such that when the developer steps through the code, any lines between a `#line hidden` and the next `#line` directive (assuming that it isn't another `#line hidden` directive) will be stepped over.</span></span> <span data-ttu-id="87114-210">还可通过此选项允许 ASP.NET 区分用户定义和计算机生成的代码。</span><span class="sxs-lookup"><span data-stu-id="87114-210">This option can also be used to allow ASP.NET to differentiate between user-defined and machine-generated code.</span></span> <span data-ttu-id="87114-211">虽然此功能主要用于 ASP.NET，但可能更多的源生成器会利用此功能。</span><span class="sxs-lookup"><span data-stu-id="87114-211">Although ASP.NET is the primary consumer of this feature, it's likely that more source generators will make use of it.</span></span>

<span data-ttu-id="87114-212">`#line hidden` 指令不影响错误报告中的文件名或行号。</span><span class="sxs-lookup"><span data-stu-id="87114-212">A `#line hidden` directive doesn't affect file names or line numbers in error reporting.</span></span> <span data-ttu-id="87114-213">也就是说，如果编译器在隐藏块中发现错误，编译器将报告错误的当前文件名和行号。</span><span class="sxs-lookup"><span data-stu-id="87114-213">That is, if the compiler finds an error in a hidden block, the compiler will report the current file name and line number of the error.</span></span>

<span data-ttu-id="87114-214">`#line filename` 指令可指定要在编译器输出中显示的文件名。</span><span class="sxs-lookup"><span data-stu-id="87114-214">The `#line filename` directive specifies the file name you want to appear in the compiler output.</span></span> <span data-ttu-id="87114-215">默认情况下，将使用源代码文件的实际名称。</span><span class="sxs-lookup"><span data-stu-id="87114-215">By default, the actual name of the source code file is used.</span></span> <span data-ttu-id="87114-216">该文件名必须以双引号 ("") 引起来，且必须位于行号之后。</span><span class="sxs-lookup"><span data-stu-id="87114-216">The file name must be in double quotation marks ("") and must be preceded by a line number.</span></span>

<span data-ttu-id="87114-217">下列示例演示调试程序如何忽略代码中的隐藏行。</span><span class="sxs-lookup"><span data-stu-id="87114-217">The following example shows how the debugger ignores the hidden lines in the code.</span></span> <span data-ttu-id="87114-218">运行示例时，它将显示三行文本。</span><span class="sxs-lookup"><span data-stu-id="87114-218">When you run the example, it will display three lines of text.</span></span> <span data-ttu-id="87114-219">但是，如果按照示例所示设置断点、并按 F10 逐行执行代码，调试程序将忽略隐藏行。</span><span class="sxs-lookup"><span data-stu-id="87114-219">However, when you set a break point, as shown in the example, and hit F10 to step through the code, the debugger ignores the hidden line.</span></span> <span data-ttu-id="87114-220">即使在隐藏行设置断点，调试程序仍将忽略它。</span><span class="sxs-lookup"><span data-stu-id="87114-220">Even if you set a break point at the hidden line, the debugger will still ignore it.</span></span>

```csharp
// preprocessor_linehidden.cs
using System;
class MainClass
{
    static void Main()
    {
        Console.WriteLine("Normal line #1."); // Set break point here.
#line hidden
        Console.WriteLine("Hidden line.");
#line default
        Console.WriteLine("Normal line #2.");
    }
}
```

## <a name="pragmas"></a><span data-ttu-id="87114-221">杂注</span><span class="sxs-lookup"><span data-stu-id="87114-221">Pragmas</span></span>

<span data-ttu-id="87114-222">`#pragma` 为编译器给出特殊指令以编译它所在的文件。</span><span class="sxs-lookup"><span data-stu-id="87114-222">`#pragma` gives the compiler special instructions for the compilation of the file in which it appears.</span></span> <span data-ttu-id="87114-223">这些指令必须受编译器支持。</span><span class="sxs-lookup"><span data-stu-id="87114-223">The instructions must be supported by the compiler.</span></span> <span data-ttu-id="87114-224">换句话说，不能使用 `#pragma` 创建自定义的预处理指令。</span><span class="sxs-lookup"><span data-stu-id="87114-224">In other words, you can't use `#pragma` to create custom preprocessing instructions.</span></span>
  
- <span data-ttu-id="87114-225">[`#pragma warning`](#pragma-warning)：启用或禁用警告。</span><span class="sxs-lookup"><span data-stu-id="87114-225">[`#pragma warning`](#pragma-warning): Enable or disable warnings.</span></span>
- <span data-ttu-id="87114-226">[`#pragma checksum`](#pragma-checksum)：生成校验和。</span><span class="sxs-lookup"><span data-stu-id="87114-226">[`#pragma checksum`](#pragma-checksum): Generate a checksum.</span></span>

```csharp
#pragma pragma-name pragma-arguments
```

<span data-ttu-id="87114-227">其中 `pragma-name` 是可识别 pragma 的名称，`pragma-arguments` 是特定于 pragma 的参数。</span><span class="sxs-lookup"><span data-stu-id="87114-227">Where `pragma-name` is the name of a recognized pragma and `pragma-arguments` is the pragma-specific arguments.</span></span>

### <a name="pragma-warning"></a><span data-ttu-id="87114-228">#pragma warning</span><span class="sxs-lookup"><span data-stu-id="87114-228">#pragma warning</span></span>

<span data-ttu-id="87114-229">`#pragma warning` 可以启用或禁用特定警告。</span><span class="sxs-lookup"><span data-stu-id="87114-229">`#pragma warning` can enable or disable certain warnings.</span></span>

```csharp
#pragma warning disable warning-list
#pragma warning restore warning-list
```

<span data-ttu-id="87114-230">其中 `warning-list` 是以逗号分隔的警告编号的列表。</span><span class="sxs-lookup"><span data-stu-id="87114-230">Where `warning-list` is a comma-separated list of warning numbers.</span></span> <span data-ttu-id="87114-231">“CS”前缀是可选的。</span><span class="sxs-lookup"><span data-stu-id="87114-231">The "CS" prefix is optional.</span></span> <span data-ttu-id="87114-232">未指定警告编号时，`disable` 会禁用所有警告，`restore` 会启用所有警告。</span><span class="sxs-lookup"><span data-stu-id="87114-232">When no warning numbers are specified, `disable` disables all warnings and `restore` enables all warnings.</span></span>

> [!NOTE]
> <span data-ttu-id="87114-233">若要在 Visual Studio 中查找警告编号，请生成项目，然后在“输出”窗口中查找警告编号。</span><span class="sxs-lookup"><span data-stu-id="87114-233">To find warning numbers in Visual Studio, build your project and then look for the warning numbers in the **Output** window.</span></span>

<span data-ttu-id="87114-234">`disable` 从源文件的下一行开始生效。</span><span class="sxs-lookup"><span data-stu-id="87114-234">The `disable` takes effect beginning on the next line of the source file.</span></span> <span data-ttu-id="87114-235">警告会在后面的 `restore` 行上还原。</span><span class="sxs-lookup"><span data-stu-id="87114-235">The warning is restored on the line following the `restore`.</span></span> <span data-ttu-id="87114-236">如果文件中没有 `restore`，则在同一编译中任何之后文件的第一行，警告将还原为其默认状态。</span><span class="sxs-lookup"><span data-stu-id="87114-236">If there's no `restore` in the file, the warnings are restored to their default state at the first line of any later files in the same compilation.</span></span>

```csharp
// pragma_warning.cs
using System;

#pragma warning disable 414, CS3021
[CLSCompliant(false)]
public class C
{
    int i = 1;
    static void Main()
    {
    }
}
#pragma warning restore CS3021
[CLSCompliant(false)]  // CS3021
public class D
{
    int i = 1;
    public static void F()
    {
    }
}
```

### <a name="pragma-checksum"></a><span data-ttu-id="87114-237">#pragma checksum</span><span class="sxs-lookup"><span data-stu-id="87114-237">#pragma checksum</span></span>

<span data-ttu-id="87114-238">生成源文件的校验和以帮助调试 ASP.NET 页面。</span><span class="sxs-lookup"><span data-stu-id="87114-238">Generates checksums for source files to aid with debugging ASP.NET pages.</span></span>

```csharp
#pragma checksum "filename" "{guid}" "checksum bytes"
```

<span data-ttu-id="87114-239">其中，`"filename"` 是需要监视更改或更新的文件的名称，`"{guid}"` 是哈希算法的全局唯一标识符 (GUID)，`"checksum_bytes"` 是表示校验和字节的十六进制数字的字符串。</span><span class="sxs-lookup"><span data-stu-id="87114-239">Where `"filename"` is the name of the file that requires monitoring for changes or updates, `"{guid}"` is the Globally Unique Identifier (GUID) for the hash algorithm, and `"checksum_bytes"` is the string of hexadecimal digits representing the bytes of the checksum.</span></span> <span data-ttu-id="87114-240">必须是偶数个十六进制数字。</span><span class="sxs-lookup"><span data-stu-id="87114-240">Must be an even number of hexadecimal digits.</span></span> <span data-ttu-id="87114-241">奇数个数字会导致编译时警告出现，且指令遭忽略。</span><span class="sxs-lookup"><span data-stu-id="87114-241">An odd number of digits results in a compile-time warning, and the directive is ignored.</span></span>

<span data-ttu-id="87114-242">Visual Studio 调试器使用校验和确保它可始终找到正确的源。</span><span class="sxs-lookup"><span data-stu-id="87114-242">The Visual Studio debugger uses a checksum to make sure  that it always finds the right source.</span></span> <span data-ttu-id="87114-243">编译器为源文件计算校验和，然后将输出发出到程序数据库 (PDB) 文件。</span><span class="sxs-lookup"><span data-stu-id="87114-243">The compiler computes the checksum for a source file, and then emits the output to the program database (PDB) file.</span></span> <span data-ttu-id="87114-244">调试器随后使用 PDB 针对它为源文件计算的校验和进行比较。</span><span class="sxs-lookup"><span data-stu-id="87114-244">The debugger then uses the PDB to compare against the checksum that it computes for the source file.</span></span>

<span data-ttu-id="87114-245">此解决方案不适用于 ASP.NET 项目，因为计算出的校验和是针对生成的源文件，而不是 .aspx 文件。</span><span class="sxs-lookup"><span data-stu-id="87114-245">This solution doesn't work for ASP.NET projects, because the computed checksum is for the generated source file, rather than the .aspx file.</span></span> <span data-ttu-id="87114-246">为解决此问题，`#pragma checksum` 为 ASP.NET 页面提供校验和支持。</span><span class="sxs-lookup"><span data-stu-id="87114-246">To address this problem, `#pragma checksum` provides checksum support for ASP.NET pages.</span></span>

<span data-ttu-id="87114-247">在 Visual C# 中创建 ASP.NET 项目时，生成的源文件包含 .aspx 文件（从该文件生成源）的校验和。</span><span class="sxs-lookup"><span data-stu-id="87114-247">When you create an ASP.NET project in Visual C#, the generated source file contains a checksum for the .aspx file, from which the source is generated.</span></span> <span data-ttu-id="87114-248">编译器随后将此信息写入 PDB 文件中。</span><span class="sxs-lookup"><span data-stu-id="87114-248">The compiler then writes this information into the PDB file.</span></span>

<span data-ttu-id="87114-249">如果编译器在文件中没有找到 `#pragma checksum` 指令，它将计算校验和，并将该值写入 PDB 文件。</span><span class="sxs-lookup"><span data-stu-id="87114-249">If the compiler doesn't find a `#pragma checksum` directive in the file, it computes the checksum and writes the value to the PDB file.</span></span>

```csharp
class TestClass
{
    static int Main()
    {
        #pragma checksum "file.cs" "{406EA660-64CF-4C82-B6F0-42D48172A799}" "ab007f1d23d9" // New checksum
    }
}
```
