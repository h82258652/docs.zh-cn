---
title: C# 编码约定 - C# 编程指南
description: 了解 C# 编码约定。 编码约定为代码创建一致的外观，并简化代码的复制、更改和维护过程。
ms.date: 03/31/2021
helpviewer_keywords:
- coding conventions, C#
- Visual C#, coding conventions
- C# language, coding conventions
ms.openlocfilehash: 019bf02ea3cdfec2c4ae0d73b5b375781c5fcd9a
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231318"
---
# <a name="c-coding-conventions-c-programming-guide"></a><span data-ttu-id="222d5-104">C# 编码约定（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="222d5-104">C# Coding Conventions (C# Programming Guide)</span></span>

<span data-ttu-id="222d5-105">编码约定可实现以下目的：</span><span class="sxs-lookup"><span data-stu-id="222d5-105">Coding conventions serve the following purposes:</span></span>  
  
- <span data-ttu-id="222d5-106">它们为代码创建一致的外观，以确保读取器专注于内容而非布局。</span><span class="sxs-lookup"><span data-stu-id="222d5-106">They create a consistent look to the code, so that readers can focus on content, not layout.</span></span>  
  
- <span data-ttu-id="222d5-107">它们使得读取器可以通过基于之前的经验进行的假设更快地理解代码。</span><span class="sxs-lookup"><span data-stu-id="222d5-107">They enable readers to understand the code more quickly by making assumptions based on previous experience.</span></span>  
  
- <span data-ttu-id="222d5-108">它们便于复制、更改和维护代码。</span><span class="sxs-lookup"><span data-stu-id="222d5-108">They facilitate copying, changing, and maintaining the code.</span></span>  
  
- <span data-ttu-id="222d5-109">它们展示 C# 最佳做法。</span><span class="sxs-lookup"><span data-stu-id="222d5-109">They demonstrate C# best practices.</span></span>  

<span data-ttu-id="222d5-110">Microsoft 根据本文中的准则来开发样本和文档。</span><span class="sxs-lookup"><span data-stu-id="222d5-110">The guidelines in this article are used by Microsoft to develop samples and documentation.</span></span>  
  
## <a name="naming-conventions"></a><span data-ttu-id="222d5-111">命名约定</span><span class="sxs-lookup"><span data-stu-id="222d5-111">Naming conventions</span></span>  
  
- <span data-ttu-id="222d5-112">在不包括 [using 指令](../../language-reference/keywords/using-directive.md)的短示例中，使用命名空间限定。</span><span class="sxs-lookup"><span data-stu-id="222d5-112">In short examples that don't include [using directives](../../language-reference/keywords/using-directive.md), use namespace qualifications.</span></span> <span data-ttu-id="222d5-113">如果你知道命名空间默认导入项目中，则不必完全限定来自该命名空间的名称。</span><span class="sxs-lookup"><span data-stu-id="222d5-113">If you know that a namespace is imported by default in a project, you don't have to fully qualify the names from that namespace.</span></span> <span data-ttu-id="222d5-114">如果对于单行来说过长，则可以在点 (.) 后中断限定名称，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-114">Qualified names can be broken after a dot (.) if they are too long for a single line, as shown in the following example.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet1":::

- <span data-ttu-id="222d5-115">你不必更改使用 Visual Studio 设计器工具创建的对象的名称以使它们适合其他准则。</span><span class="sxs-lookup"><span data-stu-id="222d5-115">You don't have to change the names of objects that were created by using the Visual Studio designer tools to make them fit other guidelines.</span></span>  
  
## <a name="layout-conventions"></a><span data-ttu-id="222d5-116">布局约定</span><span class="sxs-lookup"><span data-stu-id="222d5-116">Layout conventions</span></span>  

<span data-ttu-id="222d5-117">好的布局利用格式设置来强调代码的结构并使代码更便于阅读。</span><span class="sxs-lookup"><span data-stu-id="222d5-117">Good layout uses formatting to emphasize the structure of your code and to make the code easier to read.</span></span> <span data-ttu-id="222d5-118">Microsoft 示例和样本符合以下约定：</span><span class="sxs-lookup"><span data-stu-id="222d5-118">Microsoft examples and samples conform to the following conventions:</span></span>  
  
- <span data-ttu-id="222d5-119">使用默认的代码编辑器设置（智能缩进、4 字符缩进、制表符保存为空格）。</span><span class="sxs-lookup"><span data-stu-id="222d5-119">Use the default Code Editor settings (smart indenting, four-character indents, tabs saved as spaces).</span></span> <span data-ttu-id="222d5-120">有关详细信息，请参阅[选项、文本编辑器、C#、格式设置](/visualstudio/ide/reference/options-text-editor-csharp-formatting)。</span><span class="sxs-lookup"><span data-stu-id="222d5-120">For more information, see [Options, Text Editor, C#, Formatting](/visualstudio/ide/reference/options-text-editor-csharp-formatting).</span></span>  
  
- <span data-ttu-id="222d5-121">每行只写一条语句。</span><span class="sxs-lookup"><span data-stu-id="222d5-121">Write only one statement per line.</span></span>  
  
- <span data-ttu-id="222d5-122">每行只写一个声明。</span><span class="sxs-lookup"><span data-stu-id="222d5-122">Write only one declaration per line.</span></span>  
  
- <span data-ttu-id="222d5-123">如果连续行未自动缩进，请将它们缩进一个制表符位（四个空格）。</span><span class="sxs-lookup"><span data-stu-id="222d5-123">If continuation lines are not indented automatically, indent them one tab stop (four spaces).</span></span>  
  
- <span data-ttu-id="222d5-124">在方法定义与属性定义之间添加至少一个空白行。</span><span class="sxs-lookup"><span data-stu-id="222d5-124">Add at least one blank line between method definitions and property definitions.</span></span>  
  
- <span data-ttu-id="222d5-125">使用括号突出表达式中的子句，如下面的代码所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-125">Use parentheses to make clauses in an expression apparent, as shown in the following code.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet2":::

## <a name="commenting-conventions"></a><span data-ttu-id="222d5-126">注释约定</span><span class="sxs-lookup"><span data-stu-id="222d5-126">Commenting conventions</span></span>  
  
- <span data-ttu-id="222d5-127">将注释放在单独的行上，而非代码行的末尾。</span><span class="sxs-lookup"><span data-stu-id="222d5-127">Place the comment on a separate line, not at the end of a line of code.</span></span>  
  
- <span data-ttu-id="222d5-128">以大写字母开始注释文本。</span><span class="sxs-lookup"><span data-stu-id="222d5-128">Begin comment text with an uppercase letter.</span></span>  
  
- <span data-ttu-id="222d5-129">以句点结束注释文本。</span><span class="sxs-lookup"><span data-stu-id="222d5-129">End comment text with a period.</span></span>  
  
- <span data-ttu-id="222d5-130">在注释分隔符 (//) 与注释文本之间插入一个空格，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-130">Insert one space between the comment delimiter (//) and the comment text, as shown in the following example.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet3":::

- <span data-ttu-id="222d5-131">请勿在注释周围创建格式化的星号块。</span><span class="sxs-lookup"><span data-stu-id="222d5-131">Don't create formatted blocks of asterisks around comments.</span></span>  
  
## <a name="language-guidelines"></a><span data-ttu-id="222d5-132">语言准则</span><span class="sxs-lookup"><span data-stu-id="222d5-132">Language guidelines</span></span>  

<span data-ttu-id="222d5-133">以下各节介绍 C# 遵循以准备代码示例和样本的做法。</span><span class="sxs-lookup"><span data-stu-id="222d5-133">The following sections describe practices that the C# team follows to prepare code examples and samples.</span></span>  
  
### <a name="string-data-type"></a><span data-ttu-id="222d5-134">字符串数据类型</span><span class="sxs-lookup"><span data-stu-id="222d5-134">String data type</span></span>  
  
- <span data-ttu-id="222d5-135">使用[字符串内插](../../language-reference/tokens/interpolated.md)来连接短字符串，如下面的代码所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-135">Use [string interpolation](../../language-reference/tokens/interpolated.md) to concatenate short strings, as shown in the following code.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet6":::

- <span data-ttu-id="222d5-136">若要在循环中追加字符串，尤其是在使用大量文本时，请使用 <xref:System.Text.StringBuilder> 对象。</span><span class="sxs-lookup"><span data-stu-id="222d5-136">To append strings in loops, especially when you're working with large amounts of text, use a <xref:System.Text.StringBuilder> object.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet7":::

### <a name="implicitly-typed-local-variables"></a><span data-ttu-id="222d5-137">隐式类型本地变量</span><span class="sxs-lookup"><span data-stu-id="222d5-137">Implicitly typed local variables</span></span>  
  
- <span data-ttu-id="222d5-138">当变量类型明显来自赋值的右侧时，或者当精度类型不重要时，请对本地变量进行[隐式类型化](../classes-and-structs/implicitly-typed-local-variables.md)。</span><span class="sxs-lookup"><span data-stu-id="222d5-138">Use [implicit typing](../classes-and-structs/implicitly-typed-local-variables.md) for local variables when the type of the variable is obvious from the right side of the assignment, or when the precise type is not important.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet8":::
  
- <span data-ttu-id="222d5-139">当类型并非明显来自赋值的右侧时，请勿使用 [var](../../language-reference/keywords/var.md)。</span><span class="sxs-lookup"><span data-stu-id="222d5-139">Don't use [var](../../language-reference/keywords/var.md) when the type is not apparent from the right side of the assignment.</span></span> <span data-ttu-id="222d5-140">请勿假设类型明显来自方法名称。</span><span class="sxs-lookup"><span data-stu-id="222d5-140">Don't assume the type is clear from a method name.</span></span> <span data-ttu-id="222d5-141">如果变量类型为 `new` 运算符或显式强制转换，则将其视为明显来自方法名称。</span><span class="sxs-lookup"><span data-stu-id="222d5-141">A variable type is considered clear if it's a `new` operator or an explicit cast.</span></span>
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet9":::

- <span data-ttu-id="222d5-142">请勿依靠变量名称来指定变量的类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-142">Don't rely on the variable name to specify the type of the variable.</span></span> <span data-ttu-id="222d5-143">它可能不正确。</span><span class="sxs-lookup"><span data-stu-id="222d5-143">It might not be correct.</span></span> <span data-ttu-id="222d5-144">在以下示例中，变量名称 `inputInt` 会产生误导性。</span><span class="sxs-lookup"><span data-stu-id="222d5-144">In the following example, the variable name `inputInt` is misleading.</span></span> <span data-ttu-id="222d5-145">它是字符串。</span><span class="sxs-lookup"><span data-stu-id="222d5-145">It's a string.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet10":::

- <span data-ttu-id="222d5-146">避免使用 `var` 来代替 [dynamic](../../language-reference/builtin-types/reference-types.md)。</span><span class="sxs-lookup"><span data-stu-id="222d5-146">Avoid the use of `var` in place of [dynamic](../../language-reference/builtin-types/reference-types.md).</span></span> <span data-ttu-id="222d5-147">如果想要进行运行时类型推理，请使用 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="222d5-147">Use `dynamic` when you want run-time type inference.</span></span> <span data-ttu-id="222d5-148">有关详细信息，请参阅[使用类型 dynamic（C# 编程指南）](../types/using-type-dynamic.md)。</span><span class="sxs-lookup"><span data-stu-id="222d5-148">For more information, see [Using type dynamic (C# Programming Guide)](../types/using-type-dynamic.md).</span></span>
  
- <span data-ttu-id="222d5-149">使用隐式类型化来确定 [for](../../language-reference/keywords/for.md) 循环中循环变量的类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-149">Use implicit typing to determine the type of the loop variable in [for](../../language-reference/keywords/for.md) loops.</span></span>  
  
  <span data-ttu-id="222d5-150">下面的示例在 `for` 语句中使用隐式类型化。</span><span class="sxs-lookup"><span data-stu-id="222d5-150">The following example uses implicit typing in a `for` statement.</span></span>  

    :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet7":::

- <span data-ttu-id="222d5-151">请勿使用隐式类型化来确定 [foreach](../../language-reference/keywords/foreach-in.md) 循环中循环变量的类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-151">Don't use implicit typing to determine the type of the loop variable in [foreach](../../language-reference/keywords/foreach-in.md) loops.</span></span>

  <span data-ttu-id="222d5-152">下面的示例在 `foreach` 语句中使用显式类型化。</span><span class="sxs-lookup"><span data-stu-id="222d5-152">The following example uses explicit typing in a `foreach` statement.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet12":::

  > [!NOTE]
  > <span data-ttu-id="222d5-153">注意不要意外更改可迭代集合的元素类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-153">Be careful not to accidentally change a type of an element of the iterable collection.</span></span> <span data-ttu-id="222d5-154">例如，在 `foreach` 语句中从 <xref:System.Linq.IQueryable?displayProperty=nameWithType> 切换到 <xref:System.Collections.IEnumerable?displayProperty=nameWithType> 很容易，这会更改查询的执行。</span><span class="sxs-lookup"><span data-stu-id="222d5-154">For example, it is easy to switch from <xref:System.Linq.IQueryable?displayProperty=nameWithType> to <xref:System.Collections.IEnumerable?displayProperty=nameWithType> in a `foreach` statement, which changes the execution of a query.</span></span>

### <a name="unsigned-data-types"></a><span data-ttu-id="222d5-155">无符号数据类型</span><span class="sxs-lookup"><span data-stu-id="222d5-155">Unsigned data types</span></span>  
  
<span data-ttu-id="222d5-156">通常，使用 `int` 而非无符号类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-156">In general, use `int` rather than unsigned types.</span></span> <span data-ttu-id="222d5-157">`int` 的使用在整个 C# 中都很常见，并且当你使用 `int` 时，更易于与其他库交互。</span><span class="sxs-lookup"><span data-stu-id="222d5-157">The use of `int` is common throughout C#, and it is easier to interact with other libraries when you use `int`.</span></span>  
  
### <a name="arrays"></a><span data-ttu-id="222d5-158">数组</span><span class="sxs-lookup"><span data-stu-id="222d5-158">Arrays</span></span>  
  
<span data-ttu-id="222d5-159">当在声明行上初始化数组时，请使用简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="222d5-159">Use the concise syntax when you initialize arrays on the declaration line.</span></span> <span data-ttu-id="222d5-160">在以下示例中，请注意不能使用 `var` 替代 `string[]`。</span><span class="sxs-lookup"><span data-stu-id="222d5-160">In the following example, note that you can't use `var` instead of `string[]`.</span></span>  
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13a":::

<span data-ttu-id="222d5-161">如果使用显式实例化，则可以使用 `var`。</span><span class="sxs-lookup"><span data-stu-id="222d5-161">If you use explicit instantiation, you can use `var`.</span></span>

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13b":::

<span data-ttu-id="222d5-162">如果指定数组大小，只能一次初始化一个元素。</span><span class="sxs-lookup"><span data-stu-id="222d5-162">If you specify an array size, you have to initialize the elements one at a time.</span></span>
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet13c":::
  
### <a name="delegates"></a><span data-ttu-id="222d5-163">委托</span><span class="sxs-lookup"><span data-stu-id="222d5-163">Delegates</span></span>  
  
<span data-ttu-id="222d5-164">使用 [`Func<>` 和 `Action<>`](../../../standard/delegates-lambdas.md)，而不是定义委托类型。</span><span class="sxs-lookup"><span data-stu-id="222d5-164">Use [`Func<>` and `Action<>`](../../../standard/delegates-lambdas.md) instead of defining delegate types.</span></span> <span data-ttu-id="222d5-165">在类中，定义委托方法。</span><span class="sxs-lookup"><span data-stu-id="222d5-165">In a class, define the delegate method.</span></span>  

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet14a":::

<span data-ttu-id="222d5-166">使用 `Func<>` 或 `Action<>` 委托定义的签名来调用方法。</span><span class="sxs-lookup"><span data-stu-id="222d5-166">Call the method using the signature defined by the `Func<>` or `Action<>` delegate.</span></span>

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15a":::

<span data-ttu-id="222d5-167">如果创建委托类型的实例，请使用简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="222d5-167">If you create instances of a delegate type, use the concise syntax.</span></span> <span data-ttu-id="222d5-168">在类中，定义委托类型和具有匹配签名的方法。</span><span class="sxs-lookup"><span data-stu-id="222d5-168">In a class, define the delegate type and a method that has a matching signature.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet14b":::

<span data-ttu-id="222d5-169">创建委托类型的实例，然后调用该实例。</span><span class="sxs-lookup"><span data-stu-id="222d5-169">Create an instance of the delegate type and call it.</span></span> <span data-ttu-id="222d5-170">以下声明显示了紧缩的语法。</span><span class="sxs-lookup"><span data-stu-id="222d5-170">The following declaration shows the condensed syntax.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15b":::

<span data-ttu-id="222d5-171">以下声明使用了完整的语法。</span><span class="sxs-lookup"><span data-stu-id="222d5-171">The following declaration uses the full syntax.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet15c":::

### <a name="try-catch-and-using-statements-in-exception-handling"></a><span data-ttu-id="222d5-172">`try`-`catch` 和 `using` 语句正在异常处理中</span><span class="sxs-lookup"><span data-stu-id="222d5-172">`try`-`catch` and `using` statements in exception handling</span></span>  
  
- <span data-ttu-id="222d5-173">对大多数异常处理使用 [try-catch](../../language-reference/keywords/try-catch.md) 语句。</span><span class="sxs-lookup"><span data-stu-id="222d5-173">Use a [try-catch](../../language-reference/keywords/try-catch.md) statement for most exception handling.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet16":::

- <span data-ttu-id="222d5-174">通过使用 C# [using 语句](../../language-reference/keywords/using-statement.md)简化你的代码。</span><span class="sxs-lookup"><span data-stu-id="222d5-174">Simplify your code by using the C# [using statement](../../language-reference/keywords/using-statement.md).</span></span> <span data-ttu-id="222d5-175">如果具有 [try-finally](../../language-reference/keywords/try-finally.md) 语句（该语句中 `finally` 块的唯一代码是对 <xref:System.IDisposable.Dispose%2A> 方法的调用），请使用 `using` 语句代替。</span><span class="sxs-lookup"><span data-stu-id="222d5-175">If you have a [try-finally](../../language-reference/keywords/try-finally.md) statement in which the only code in the `finally` block is a call to the <xref:System.IDisposable.Dispose%2A> method, use a `using` statement instead.</span></span>

  <span data-ttu-id="222d5-176">在以下示例中，`try`-`finally` 语句仅在 `finally` 块中调用 `Dispose`。</span><span class="sxs-lookup"><span data-stu-id="222d5-176">In the following example, the `try`-`finally` statement only calls `Dispose` in the `finally` block.</span></span>
  
   :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17a":::

  <span data-ttu-id="222d5-177">可以使用 `using` 语句执行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="222d5-177">You can do the same thing with a `using` statement.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17b":::

  <span data-ttu-id="222d5-178">在 C# 8 及更高版本中，使用无需大括号的新的 [`using` 语法](../../language-reference/keywords/using-statement.md)：</span><span class="sxs-lookup"><span data-stu-id="222d5-178">In C# 8 and later versions, use the new [`using` syntax](../../language-reference/keywords/using-statement.md) that doesn't require braces:</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet17c":::

### <a name="-and--operators"></a><span data-ttu-id="222d5-179">`&&` 和 `||` 运算符</span><span class="sxs-lookup"><span data-stu-id="222d5-179">`&&` and `||` operators</span></span>  
  
<span data-ttu-id="222d5-180">若要通过跳过不必要的比较来避免异常并提高性能，请在执行比较时使用 [`&&`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-)（而不是 [`&`](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-)）和 [`||`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-)（而不是 [`|`](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-)），如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-180">To avoid exceptions and increase performance by skipping unnecessary comparisons, use [`&&`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-and-operator-) instead of [`&`](../../language-reference/operators/boolean-logical-operators.md#logical-and-operator-) and [`||`](../../language-reference/operators/boolean-logical-operators.md#conditional-logical-or-operator-) instead of [`|`](../../language-reference/operators/boolean-logical-operators.md#logical-or-operator-) when you perform comparisons, as shown in the following example.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet18":::

<span data-ttu-id="222d5-181">如果除数为 0，则 `if` 语句中的第二个子句将导致运行时错误。</span><span class="sxs-lookup"><span data-stu-id="222d5-181">If the divisor is 0, the second clause in the `if` statement would cause a run-time error.</span></span> <span data-ttu-id="222d5-182">但是，当第一个表达式为 false 时，&& 运算符将发生短路。</span><span class="sxs-lookup"><span data-stu-id="222d5-182">But the && operator short-circuits when the first expression is false.</span></span> <span data-ttu-id="222d5-183">也就是说，它并不评估第二个表达式。</span><span class="sxs-lookup"><span data-stu-id="222d5-183">That is, it doesn't evaluate the second expression.</span></span> <span data-ttu-id="222d5-184">如果 `divisor` 为 0，则 & 运算符将同时计算这两个表达式，从而导致运行时错误。</span><span class="sxs-lookup"><span data-stu-id="222d5-184">The & operator would evaluate both, resulting in a run-time error when `divisor` is 0.</span></span>
  
### <a name="new-operator"></a><span data-ttu-id="222d5-185">`new` 运算符</span><span class="sxs-lookup"><span data-stu-id="222d5-185">`new` operator</span></span>  
  
- <span data-ttu-id="222d5-186">使用对象实例化的简洁形式之一，如以下声明中所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-186">Use one of the concise forms of object instantiation, as shown in the following declarations.</span></span> <span data-ttu-id="222d5-187">第二个示例显示了从 C# 9 开始可用的语法。</span><span class="sxs-lookup"><span data-stu-id="222d5-187">The second example shows syntax that is available starting in C# 9.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet19":::
  
  ```csharp
  ExampleClass instance2 = new();
  ```
  
  <span data-ttu-id="222d5-188">前面的声明等效于下面的声明。</span><span class="sxs-lookup"><span data-stu-id="222d5-188">The preceding declarations are equivalent to the following declaration.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet20":::

- <span data-ttu-id="222d5-189">使用对象初始值设定项简化对象创建，如以下示例中所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-189">Use object initializers to simplify object creation, as shown in the following example.</span></span>

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet21a":::

  <span data-ttu-id="222d5-190">下面的示例设置了与前面的示例相同的属性，但未使用初始值设定项。</span><span class="sxs-lookup"><span data-stu-id="222d5-190">The following example sets the same properties as the preceding example but doesn't use initializers.</span></span>
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet21b":::

### <a name="event-handling"></a><span data-ttu-id="222d5-191">事件处理</span><span class="sxs-lookup"><span data-stu-id="222d5-191">Event handling</span></span>  
  
<span data-ttu-id="222d5-192">如果你正在定义一个稍后不需要删除的事件处理程序，请使用 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="222d5-192">If you're defining an event handler that you don't need to remove later, use a lambda expression.</span></span>  
  
:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet22":::

<span data-ttu-id="222d5-193">Lambda 表达式缩短了以下传统定义。</span><span class="sxs-lookup"><span data-stu-id="222d5-193">The lambda expression shortens the following traditional definition.</span></span>

:::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet23":::

### <a name="static-members"></a><span data-ttu-id="222d5-194">静态成员</span><span class="sxs-lookup"><span data-stu-id="222d5-194">Static members</span></span>  
  
<span data-ttu-id="222d5-195">使用类名调用 [static](../../language-reference/keywords/static.md) 成员：ClassName.StaticMember。</span><span class="sxs-lookup"><span data-stu-id="222d5-195">Call [static](../../language-reference/keywords/static.md) members by using the class name: *ClassName.StaticMember*.</span></span> <span data-ttu-id="222d5-196">这种做法通过明确静态访问使代码更易于阅读。</span><span class="sxs-lookup"><span data-stu-id="222d5-196">This practice makes code more readable by making static access clear.</span></span>  <span data-ttu-id="222d5-197">请勿使用派生类的名称来限定基类中定义的静态成员。</span><span class="sxs-lookup"><span data-stu-id="222d5-197">Don't qualify a static member defined in a base class with the name of a derived class.</span></span>  <span data-ttu-id="222d5-198">编译该代码时，代码可读性具有误导性，如果向派生类添加具有相同名称的静态成员，代码可能会被破坏。</span><span class="sxs-lookup"><span data-stu-id="222d5-198">While that code compiles, the code readability is misleading, and the code may break in the future if you add a static member with the same name to the derived class.</span></span>  
  
### <a name="linq-queries"></a><span data-ttu-id="222d5-199">LINQ 查询</span><span class="sxs-lookup"><span data-stu-id="222d5-199">LINQ queries</span></span>  
  
- <span data-ttu-id="222d5-200">对查询变量使用有意义的名称。</span><span class="sxs-lookup"><span data-stu-id="222d5-200">Use meaningful names for query variables.</span></span> <span data-ttu-id="222d5-201">下面的示例为位于西雅图的客户使用 `seattleCustomers`。</span><span class="sxs-lookup"><span data-stu-id="222d5-201">The following example uses `seattleCustomers` for customers who are located in Seattle.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet25":::

- <span data-ttu-id="222d5-202">使用别名确保匿名类型的属性名称都使用 Pascal 大小写格式正确大写。</span><span class="sxs-lookup"><span data-stu-id="222d5-202">Use aliases to make sure that property names of anonymous types are correctly capitalized, using Pascal casing.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet26":::

- <span data-ttu-id="222d5-203">如果结果中的属性名称模棱两可，请对属性重命名。</span><span class="sxs-lookup"><span data-stu-id="222d5-203">Rename properties when the property names in the result would be ambiguous.</span></span> <span data-ttu-id="222d5-204">例如，如果你的查询返回客户名称和分销商 ID，而不是在结果中将它们保留为 `Name` 和 `ID`，请对它们进行重命名以明确 `Name` 是客户的名称，`ID` 是分销商的 ID。</span><span class="sxs-lookup"><span data-stu-id="222d5-204">For example, if your query returns a customer name and a distributor ID, instead of leaving them as `Name` and `ID` in the result, rename them to clarify that `Name` is the name of a customer, and `ID` is the ID of a distributor.</span></span>  
  
  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet27":::

- <span data-ttu-id="222d5-205">在查询变量和范围变量的声明中使用隐式类型化。</span><span class="sxs-lookup"><span data-stu-id="222d5-205">Use implicit typing in the declaration of query variables and range variables.</span></span>  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet25":::

- <span data-ttu-id="222d5-206">对齐 [from](../../language-reference/keywords/from-clause.md) 子句下的查询子句，如上面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="222d5-206">Align query clauses under the [from](../../language-reference/keywords/from-clause.md) clause, as shown in the previous examples.</span></span>  

- <span data-ttu-id="222d5-207">在其他查询子句之前使用 [where](../../language-reference/keywords/where-clause.md) 子句，以确保后面的查询子句作用于经过减少和筛选的数据集。</span><span class="sxs-lookup"><span data-stu-id="222d5-207">Use [where](../../language-reference/keywords/where-clause.md) clauses before other query clauses to ensure that later query clauses operate on the reduced, filtered set of data.</span></span>  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet29":::

- <span data-ttu-id="222d5-208">使用多行 `from` 子句代替 [join](../../language-reference/keywords/join-clause.md) 子句以访问内部集合。</span><span class="sxs-lookup"><span data-stu-id="222d5-208">Use multiple `from` clauses instead of a [join](../../language-reference/keywords/join-clause.md) clause to access inner collections.</span></span> <span data-ttu-id="222d5-209">例如，`Student` 对象的集合可能包含测验分数的集合。</span><span class="sxs-lookup"><span data-stu-id="222d5-209">For example, a collection of `Student` objects might each contain a collection of test scores.</span></span> <span data-ttu-id="222d5-210">当执行以下查询时，它返回高于 90 的分数，并返回得到该分数的学生的姓氏。</span><span class="sxs-lookup"><span data-stu-id="222d5-210">When the following query is executed, it returns each score that is over 90, along with the last name of the student who received the score.</span></span>  

  :::code language="csharp" source="../../../../samples/snippets/csharp/VS_Snippets_VBCSharp/csprogguidecodingconventions/cs/program.cs" id="Snippet30":::

## <a name="security"></a><span data-ttu-id="222d5-211">安全性</span><span class="sxs-lookup"><span data-stu-id="222d5-211">Security</span></span>  

<span data-ttu-id="222d5-212">请遵循[安全编码准则](../../../standard/security/secure-coding-guidelines.md)中的准则。</span><span class="sxs-lookup"><span data-stu-id="222d5-212">Follow the guidelines in [Secure Coding Guidelines](../../../standard/security/secure-coding-guidelines.md).</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="222d5-213">请参阅</span><span class="sxs-lookup"><span data-stu-id="222d5-213">See also</span></span>

- [<span data-ttu-id="222d5-214">.NET 运行时编码准则</span><span class="sxs-lookup"><span data-stu-id="222d5-214">.NET runtime coding guidelines</span></span>](https://github.com/dotnet/runtime/blob/main/docs/coding-guidelines/coding-style.md)
- [<span data-ttu-id="222d5-215">Visual Basic 编码约定</span><span class="sxs-lookup"><span data-stu-id="222d5-215">Visual Basic Coding Conventions</span></span>](../../../visual-basic/programming-guide/program-structure/coding-conventions.md)
- [<span data-ttu-id="222d5-216">安全编码准则</span><span class="sxs-lookup"><span data-stu-id="222d5-216">Secure Coding Guidelines</span></span>](../../../standard/security/secure-coding-guidelines.md)
