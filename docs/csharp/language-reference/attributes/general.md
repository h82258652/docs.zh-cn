---
title: C# 保留属性：其他
ms.date: 03/18/2021
description: 了解影响编译器生成的代码的属性：Conditional、Obsolete、AttributeUsage、ModuleInitializer 和 SkipLocalsInit 属性。
ms.openlocfilehash: 6b8cda658ec5b3f81a7f903d8cadae0fe30e8ac2
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111306"
---
# <a name="reserved-attributes-miscellaneous"></a><span data-ttu-id="693e7-103">保留属性：其他</span><span class="sxs-lookup"><span data-stu-id="693e7-103">Reserved attributes: Miscellaneous</span></span>

<span data-ttu-id="693e7-104">这些特性可应用于代码中的元素。</span><span class="sxs-lookup"><span data-stu-id="693e7-104">These attributes can be applied to elements in your code.</span></span> <span data-ttu-id="693e7-105">它们为这些元素添加语义。</span><span class="sxs-lookup"><span data-stu-id="693e7-105">They add semantic meaning to those elements.</span></span> <span data-ttu-id="693e7-106">编译器使用这些语义来更改其输出，并报告使用你的代码的开发人员可能犯的错误。</span><span class="sxs-lookup"><span data-stu-id="693e7-106">The compiler uses those semantic meanings to alter its output and report possible mistakes by developers using your code.</span></span>

## <a name="conditional-attribute"></a><span data-ttu-id="693e7-107">`Conditional` 特性</span><span class="sxs-lookup"><span data-stu-id="693e7-107">`Conditional` attribute</span></span>

<span data-ttu-id="693e7-108">`Conditional` 特性使得方法执行依赖于预处理标识符。</span><span class="sxs-lookup"><span data-stu-id="693e7-108">The `Conditional` attribute makes the execution of a method dependent on a preprocessing identifier.</span></span> <span data-ttu-id="693e7-109">`Conditional` 属性是 <xref:System.Diagnostics.ConditionalAttribute> 的别名，可以应用于方法或特性类。</span><span class="sxs-lookup"><span data-stu-id="693e7-109">The `Conditional` attribute is an alias for <xref:System.Diagnostics.ConditionalAttribute>, and can be applied to a method or an attribute class.</span></span>

<span data-ttu-id="693e7-110">在以下示例中，`Conditional` 应用于启用或禁用显示特定于程序的诊断信息的方法：</span><span class="sxs-lookup"><span data-stu-id="693e7-110">In the following example, `Conditional` is applied to a method to enable or disable the display of program-specific diagnostic information:</span></span>

:::code language="csharp" source="snippets/trace.cs" interactive="try-dotnet" :::

<span data-ttu-id="693e7-111">如果未定义 `TRACE_ON` 标识符，则不会显示跟踪输出。</span><span class="sxs-lookup"><span data-stu-id="693e7-111">If the `TRACE_ON` identifier isn't defined, the trace output isn't displayed.</span></span> <span data-ttu-id="693e7-112">在交互式窗口中自己探索。</span><span class="sxs-lookup"><span data-stu-id="693e7-112">Explore for yourself in the interactive window.</span></span>

<span data-ttu-id="693e7-113">`Conditional` 特性通常与 `DEBUG` 标识符一起使用，以启用调试生成（而非发布生成）中的跟踪和日志记录功能，如下例所示：</span><span class="sxs-lookup"><span data-stu-id="693e7-113">The `Conditional` attribute is often used with the `DEBUG` identifier to enable trace and logging features for debug builds but not in release builds, as shown in the following example:</span></span>

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditional" :::

<span data-ttu-id="693e7-114">当调用标记为条件的方法时，指定的预处理符号是否存在将决定是包含还是省略该调用。</span><span class="sxs-lookup"><span data-stu-id="693e7-114">When a method marked conditional is called, the presence or absence of the specified preprocessing symbol determines whether the call is included or omitted.</span></span> <span data-ttu-id="693e7-115">如果定义了符号，则将包括调用；否则，将忽略该调用。</span><span class="sxs-lookup"><span data-stu-id="693e7-115">If the symbol is defined, the call is included; otherwise, the call is omitted.</span></span> <span data-ttu-id="693e7-116">条件方法必须是类或结构声明中的方法，而且必须具有 `void` 返回类型。</span><span class="sxs-lookup"><span data-stu-id="693e7-116">A conditional method must be a method in a class or struct declaration and must have a `void` return type.</span></span> <span data-ttu-id="693e7-117">与将方法封闭在 `#if…#endif` 块内相比，`Conditional` 更简洁且较不容易出错。</span><span class="sxs-lookup"><span data-stu-id="693e7-117">Using `Conditional` is cleaner, more elegant, and less error-prone than enclosing methods inside `#if…#endif` blocks.</span></span>

<span data-ttu-id="693e7-118">如果某个方法具有多个 `Conditional` 特性，则如果定义了一个或多个条件符号（通过使用 OR 运算符将这些符号逻辑链接在一起），则包含对该方法的调用。</span><span class="sxs-lookup"><span data-stu-id="693e7-118">If a method has multiple `Conditional` attributes, a call to the method is included if at one or more conditional symbols is defined (the symbols are logically linked together by using the OR operator).</span></span> <span data-ttu-id="693e7-119">在以下示例中，存在 `A` 或 `B` 将导致方法调用：</span><span class="sxs-lookup"><span data-stu-id="693e7-119">In the following example, the presence of either `A` or `B` results in a method call:</span></span>

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetMultipleConditions" :::

### <a name="using-conditional-with-attribute-classes"></a><span data-ttu-id="693e7-120">使用带有特性类的 `Conditional`</span><span class="sxs-lookup"><span data-stu-id="693e7-120">Using `Conditional` with attribute classes</span></span>

<span data-ttu-id="693e7-121">`Conditional` 特性还可应用于特性类定义。</span><span class="sxs-lookup"><span data-stu-id="693e7-121">The `Conditional` attribute can also be applied to an attribute class definition.</span></span> <span data-ttu-id="693e7-122">在以下示例中，如果定义了 `DEBUG`，则自定义特性 `Documentation` 将仅向元数据添加信息。</span><span class="sxs-lookup"><span data-stu-id="693e7-122">In the following example, the custom attribute `Documentation` will only add information to the metadata if `DEBUG` is defined.</span></span>

:::code language="csharp" source="snippets/ConditionalExamples.cs" id="SnippetConditionalConditionalAttribute" :::

## <a name="obsolete-attribute"></a><span data-ttu-id="693e7-123">`Obsolete` 特性</span><span class="sxs-lookup"><span data-stu-id="693e7-123">`Obsolete` attribute</span></span>

<span data-ttu-id="693e7-124">`Obsolete` 特性将代码元素标记为不再推荐使用。</span><span class="sxs-lookup"><span data-stu-id="693e7-124">The `Obsolete` attribute marks a code element as no longer recommended for use.</span></span> <span data-ttu-id="693e7-125">使用标记为已过时的实体会生成警告或错误。</span><span class="sxs-lookup"><span data-stu-id="693e7-125">Use of an entity marked obsolete generates a warning or an error.</span></span> <span data-ttu-id="693e7-126">`Obsolete` 特性是一次性特性，可以应用于任何允许特性的实体。</span><span class="sxs-lookup"><span data-stu-id="693e7-126">The `Obsolete` attribute is a single-use attribute and can be applied to any entity that allows attributes.</span></span> <span data-ttu-id="693e7-127">`Obsolete` 是 <xref:System.ObsoleteAttribute> 的别名。</span><span class="sxs-lookup"><span data-stu-id="693e7-127">`Obsolete` is an alias for <xref:System.ObsoleteAttribute>.</span></span>

<span data-ttu-id="693e7-128">在以下示例中，`Obsolete` 特性应用于类 `A` 和方法 `B.OldMethod`。</span><span class="sxs-lookup"><span data-stu-id="693e7-128">In the following example the `Obsolete` attribute is applied to class `A` and to method `B.OldMethod`.</span></span> <span data-ttu-id="693e7-129">因为应用于 `B.OldMethod` 的特性构造函数的第二个参数设置为 `true`，所以此方法将导致编译器错误，而使用类 `A` 只会生成警告。</span><span class="sxs-lookup"><span data-stu-id="693e7-129">Because the second argument of the attribute constructor applied to `B.OldMethod` is set to `true`, this method will cause a compiler error, whereas using class `A` will just produce a warning.</span></span> <span data-ttu-id="693e7-130">但是，调用 `B.NewMethod` 不会生成任何警告或错误。</span><span class="sxs-lookup"><span data-stu-id="693e7-130">Calling `B.NewMethod`, however, produces no warning or error.</span></span> <span data-ttu-id="693e7-131">例如，将其与先前的定义一起使用时，以下代码会生成两个警告和一个错误：</span><span class="sxs-lookup"><span data-stu-id="693e7-131">For example, when you use it with the previous definitions, the following code generates two warnings and one error:</span></span>

:::code language="csharp" source="snippets/ObsoleteExample.cs" interactive="try-dotnet" :::

<span data-ttu-id="693e7-132">作为特性构造函数的第一个参数提供的字符串将作为警告或错误的一部分显示。</span><span class="sxs-lookup"><span data-stu-id="693e7-132">The string provided as the first argument to the attribute constructor will be displayed as part of the warning or error.</span></span> <span data-ttu-id="693e7-133">将生成类 `A` 的两个警告：一个用于声明类引用，另一个用于类构造函数。</span><span class="sxs-lookup"><span data-stu-id="693e7-133">Two warnings for class `A` are generated: one for the declaration of the class reference, and one for the class constructor.</span></span> <span data-ttu-id="693e7-134">`Obsolete` 特性可以在不带参数的情况下使用，但建议说明改为使用哪个项目。</span><span class="sxs-lookup"><span data-stu-id="693e7-134">The `Obsolete` attribute can be used without arguments, but including an explanation what to use instead is recommended.</span></span>

## <a name="attributeusage-attribute"></a><span data-ttu-id="693e7-135">`AttributeUsage` 特性</span><span class="sxs-lookup"><span data-stu-id="693e7-135">`AttributeUsage` attribute</span></span>

<span data-ttu-id="693e7-136">`AttributeUsage` 特性确定自定义特性类的使用方式。</span><span class="sxs-lookup"><span data-stu-id="693e7-136">The `AttributeUsage` attribute determines how a custom attribute class can be used.</span></span> <span data-ttu-id="693e7-137"><xref:System.AttributeUsageAttribute> 是应用到自定义特性定义的特性。</span><span class="sxs-lookup"><span data-stu-id="693e7-137"><xref:System.AttributeUsageAttribute> is an attribute you apply to custom attribute definitions.</span></span> <span data-ttu-id="693e7-138">`AttributeUsage` 特性帮助控制：</span><span class="sxs-lookup"><span data-stu-id="693e7-138">The `AttributeUsage` attribute enables you to control:</span></span>

- <span data-ttu-id="693e7-139">可能应用到的具体程序元素特性。</span><span class="sxs-lookup"><span data-stu-id="693e7-139">Which program elements attribute may be applied to.</span></span> <span data-ttu-id="693e7-140">除非使用限制，否则特性可能应用到以下任意程序元素：</span><span class="sxs-lookup"><span data-stu-id="693e7-140">Unless you restrict its usage, an attribute may be applied to any of the following program elements:</span></span>
  - <span data-ttu-id="693e7-141">程序集 (assembly)</span><span class="sxs-lookup"><span data-stu-id="693e7-141">assembly</span></span>
  - <span data-ttu-id="693e7-142">name</span><span class="sxs-lookup"><span data-stu-id="693e7-142">module</span></span>
  - <span data-ttu-id="693e7-143">field</span><span class="sxs-lookup"><span data-stu-id="693e7-143">field</span></span>
  - <span data-ttu-id="693e7-144">event</span><span class="sxs-lookup"><span data-stu-id="693e7-144">event</span></span>
  - <span data-ttu-id="693e7-145">method</span><span class="sxs-lookup"><span data-stu-id="693e7-145">method</span></span>
  - <span data-ttu-id="693e7-146">param</span><span class="sxs-lookup"><span data-stu-id="693e7-146">param</span></span>
  - <span data-ttu-id="693e7-147">property</span><span class="sxs-lookup"><span data-stu-id="693e7-147">property</span></span>
  - <span data-ttu-id="693e7-148">return</span><span class="sxs-lookup"><span data-stu-id="693e7-148">return</span></span>
  - <span data-ttu-id="693e7-149">type</span><span class="sxs-lookup"><span data-stu-id="693e7-149">type</span></span>
- <span data-ttu-id="693e7-150">某特性是否可多次应用于单个程序元素。</span><span class="sxs-lookup"><span data-stu-id="693e7-150">Whether an attribute can be applied to a single program element multiple times.</span></span>
- <span data-ttu-id="693e7-151">特性是否由派生类继承。</span><span class="sxs-lookup"><span data-stu-id="693e7-151">Whether attributes are inherited by derived classes.</span></span>

<span data-ttu-id="693e7-152">显式应用时，默认设置如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="693e7-152">The default settings look like the following example when applied explicitly:</span></span>

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageFirst" :::

<span data-ttu-id="693e7-153">在此示例中，`NewAttribute` 类可应用于任何受支持的程序元素。</span><span class="sxs-lookup"><span data-stu-id="693e7-153">In this example, the `NewAttribute` class can be applied to any supported program element.</span></span> <span data-ttu-id="693e7-154">但是它对每个实体仅能应用一次。</span><span class="sxs-lookup"><span data-stu-id="693e7-154">But it can be applied only once to each entity.</span></span> <span data-ttu-id="693e7-155">特性应用于基类时，它由派生类继承。</span><span class="sxs-lookup"><span data-stu-id="693e7-155">The attribute is inherited by derived classes when applied to a base class.</span></span>

<span data-ttu-id="693e7-156"><xref:System.AttributeUsageAttribute.AllowMultiple> 和 <xref:System.AttributeUsageAttribute.Inherited> 参数是可选的，因此以下代码具有相同效果：</span><span class="sxs-lookup"><span data-stu-id="693e7-156">The <xref:System.AttributeUsageAttribute.AllowMultiple> and <xref:System.AttributeUsageAttribute.Inherited> arguments are optional, so the following code has the same effect:</span></span>

:::code language="csharp" source="snippets/NewAttribute.cs" id="SnippetUsageSecond" :::

<span data-ttu-id="693e7-157">第一个 <xref:System.AttributeUsageAttribute> 参数必须是 <xref:System.AttributeTargets> 枚举的一个或多个元素。</span><span class="sxs-lookup"><span data-stu-id="693e7-157">The first <xref:System.AttributeUsageAttribute> argument must be one or more elements of the <xref:System.AttributeTargets> enumeration.</span></span> <span data-ttu-id="693e7-158">可将多个目标类型与 OR 运算符链接在一起，如下例所示：</span><span class="sxs-lookup"><span data-stu-id="693e7-158">Multiple target types can be linked together with the OR operator, like the following example shows:</span></span>

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetDefinePropertyAttribute" :::

<span data-ttu-id="693e7-159">从 C# 7.3 开始，特性可应用于自动实现的属性的属性或支持字段。</span><span class="sxs-lookup"><span data-stu-id="693e7-159">Beginning in C# 7.3, attributes can be applied to either the property or the backing field for an auto-implemented property.</span></span> <span data-ttu-id="693e7-160">特性应用于属性，除非在特性上指定 `field` 说明符。</span><span class="sxs-lookup"><span data-stu-id="693e7-160">The attribute applies to the property, unless you specify the `field` specifier on the attribute.</span></span> <span data-ttu-id="693e7-161">都在以下示例中进行了演示：</span><span class="sxs-lookup"><span data-stu-id="693e7-161">Both are shown in the following example:</span></span>

:::code language="csharp" source="snippets/NewPropertyOrFieldAttribute.cs" id="SnippetUsePropertyAttribute" :::

<span data-ttu-id="693e7-162">如果 <xref:System.AttributeUsageAttribute.AllowMultiple> 参数为 `true`，那么结果特性可多次应用于单个实体，如以下示例所示：</span><span class="sxs-lookup"><span data-stu-id="693e7-162">If the <xref:System.AttributeUsageAttribute.AllowMultiple> argument is `true`, then the resulting attribute can be applied more than once to a single entity, as shown in the following example:</span></span>

:::code language="csharp" source="snippets/MultiUseAttribute.cs" id="SnippetMultiUse" :::

<span data-ttu-id="693e7-163">在本例中，`MultiUseAttribute` 可重复应用，因为 `AllowMultiple` 设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="693e7-163">In this case, `MultiUseAttribute` can be applied repeatedly because `AllowMultiple` is set to `true`.</span></span> <span data-ttu-id="693e7-164">所显示的两种用于应用多个特性的格式均有效。</span><span class="sxs-lookup"><span data-stu-id="693e7-164">Both formats shown for applying multiple attributes are valid.</span></span>

<span data-ttu-id="693e7-165">如果 <xref:System.AttributeUsageAttribute.Inherited> 为 `false`，那么该特性不是由特性类派生的类继承。</span><span class="sxs-lookup"><span data-stu-id="693e7-165">If <xref:System.AttributeUsageAttribute.Inherited> is `false`, then the attribute isn't inherited by classes derived from an attributed class.</span></span> <span data-ttu-id="693e7-166">例如：</span><span class="sxs-lookup"><span data-stu-id="693e7-166">For example:</span></span>

:::code language="csharp" source="snippets/NonInheritedAttribute.cs" id="SnippetNonInherited" :::

<span data-ttu-id="693e7-167">在本例中，`NonInheritedAttribute` 不会通过继承应用于 `DClass`。</span><span class="sxs-lookup"><span data-stu-id="693e7-167">In this case `NonInheritedAttribute` isn't applied to `DClass` via inheritance.</span></span>

## <a name="moduleinitializer-attribute"></a><span data-ttu-id="693e7-168">`ModuleInitializer` 特性</span><span class="sxs-lookup"><span data-stu-id="693e7-168">`ModuleInitializer` attribute</span></span>

<span data-ttu-id="693e7-169">从 C# 9 开始，`ModuleInitializer` 属性标记程序集加载时运行时调用的方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-169">Starting with C# 9, the `ModuleInitializer` attribute marks a method that the runtime calls when the assembly loads.</span></span> <span data-ttu-id="693e7-170">`ModuleInitializer` 是 <xref:System.Runtime.CompilerServices.ModuleInitializerAttribute> 的别名。</span><span class="sxs-lookup"><span data-stu-id="693e7-170">`ModuleInitializer` is an alias for <xref:System.Runtime.CompilerServices.ModuleInitializerAttribute>.</span></span>

<span data-ttu-id="693e7-171">`ModuleInitializer` 属性只能应用于以下方法：</span><span class="sxs-lookup"><span data-stu-id="693e7-171">The `ModuleInitializer` attribute can only be applied to a method that:</span></span>

* <span data-ttu-id="693e7-172">静态方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-172">Is static.</span></span>
* <span data-ttu-id="693e7-173">无参数方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-173">Is parameterless.</span></span>
* <span data-ttu-id="693e7-174">返回 `void`。</span><span class="sxs-lookup"><span data-stu-id="693e7-174">Returns `void`.</span></span>
* <span data-ttu-id="693e7-175">能够从包含模块（即 `internal` 或 `public`）访问的方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-175">Is accessible from the containing module, that is, `internal` or `public`.</span></span>
* <span data-ttu-id="693e7-176">不是泛型的方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-176">Isn't a generic method.</span></span>
* <span data-ttu-id="693e7-177">没有包含在泛型类中的方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-177">Isn't contained in a generic class.</span></span>
* <span data-ttu-id="693e7-178">不是本地函数的方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-178">Isn't a local function.</span></span>

<span data-ttu-id="693e7-179">`ModuleInitializer` 属性可应用于多种方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-179">The `ModuleInitializer` attribute can be applied to multiple methods.</span></span> <span data-ttu-id="693e7-180">在这种情况下，运行时调用它们的顺序是确定的，但未指定。</span><span class="sxs-lookup"><span data-stu-id="693e7-180">In that case, the order in which the runtime calls them is deterministic but not specified.</span></span>

<span data-ttu-id="693e7-181">下面的示例阐释了如何使用多个模块初始化表达式方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-181">The following example illustrates use of multiple module initializer methods.</span></span> <span data-ttu-id="693e7-182">`Init1` 和 `Init2` 方法在 `Main` 之前运行，并且每种方法都将一个字符串添加到 `Text` 属性。</span><span class="sxs-lookup"><span data-stu-id="693e7-182">The `Init1` and `Init2` methods run before `Main`, and each adds a string to the `Text` property.</span></span> <span data-ttu-id="693e7-183">因此，当 `Main` 运行时，`Text` 属性已具有来自两个初始化表达式方法中的字符串。</span><span class="sxs-lookup"><span data-stu-id="693e7-183">So when `Main` runs, the `Text` property already has strings from both initializer methods.</span></span>

:::code language="csharp" source="snippets/ModuleInitializerExampleMain.cs" :::

:::code language="csharp" source="snippets/ModuleInitializerExampleModule.cs" :::

<span data-ttu-id="693e7-184">源代码生成器有时需要生成初始化代码。</span><span class="sxs-lookup"><span data-stu-id="693e7-184">Source code generators sometimes need to generate initialization code.</span></span> <span data-ttu-id="693e7-185">模块初始化表达式为该代码提供了一个标准的驻留位置。</span><span class="sxs-lookup"><span data-stu-id="693e7-185">Module initializers provide a standard place for that code to reside.</span></span>

## <a name="skiplocalsinit-attribute"></a><span data-ttu-id="693e7-186">`SkipLocalsInit` 特性</span><span class="sxs-lookup"><span data-stu-id="693e7-186">`SkipLocalsInit` attribute</span></span>

<span data-ttu-id="693e7-187">从 C# 9 开始，`SkipLocalsInit` 属性可防止编译器在发出到元数据时设置 `.locals init` 标志。</span><span class="sxs-lookup"><span data-stu-id="693e7-187">Starting in C# 9, the `SkipLocalsInit` attribute prevents the compiler from setting the `.locals init` flag when emitting to metadata.</span></span> <span data-ttu-id="693e7-188">`SkipLocalsInit` 属性是一个单用途属性，可应用于方法、属性、类、结构、接口或模块，但不能应用于程序集。</span><span class="sxs-lookup"><span data-stu-id="693e7-188">The `SkipLocalsInit` attribute is a single-use attribute and can be applied to a method, a property, a class, a struct, an interface, or a module, but not to an assembly.</span></span> <span data-ttu-id="693e7-189">`SkipLocalsInit` 是 <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute> 的别名。</span><span class="sxs-lookup"><span data-stu-id="693e7-189">`SkipLocalsInit` is an alias for <xref:System.Runtime.CompilerServices.SkipLocalsInitAttribute>.</span></span>

<span data-ttu-id="693e7-190">`.locals init` 标志会导致 CLR 将方法中声明的所有局部变量初始化为其默认值。</span><span class="sxs-lookup"><span data-stu-id="693e7-190">The `.locals init` flag causes the CLR to initialize all of the local variables declared in a method to their default values.</span></span> <span data-ttu-id="693e7-191">由于编译器还可以确保在为变量赋值之前永远不使用变量，因此通常不需要使用 `.locals init`。</span><span class="sxs-lookup"><span data-stu-id="693e7-191">Since the compiler also makes sure that you never use a variable before assigning some value to it, `.locals init` is typically not necessary.</span></span> <span data-ttu-id="693e7-192">但是，在某些情况下，额外的零初始化可能会对性能产生显著影响，例如使用 [stackalloc](../operators/stackalloc.md) 在堆栈上分配一个数组时。</span><span class="sxs-lookup"><span data-stu-id="693e7-192">However, the extra zero-initialization may have measurable performance impact in some scenarios, such as when you use [stackalloc](../operators/stackalloc.md) to allocate an array on the stack.</span></span> <span data-ttu-id="693e7-193">在这些情况下，可添加 `SkipLocalsInit` 属性。</span><span class="sxs-lookup"><span data-stu-id="693e7-193">In those cases, you can add the `SkipLocalsInit` attribute.</span></span> <span data-ttu-id="693e7-194">如果直接应用于方法，该属性会影响该方法及其所有嵌套函数，包括 lambda 和局部函数。</span><span class="sxs-lookup"><span data-stu-id="693e7-194">If applied to a method directly, the attribute affects that method and all its nested functions, including lambdas and local functions.</span></span> <span data-ttu-id="693e7-195">如果应用于类型或模块，则它会影响嵌套在内的所有方法。</span><span class="sxs-lookup"><span data-stu-id="693e7-195">If applied to a type or module, it affects all methods nested inside.</span></span> <span data-ttu-id="693e7-196">此属性不会影响抽象方法，但会影响为实现生成的代码。</span><span class="sxs-lookup"><span data-stu-id="693e7-196">This attribute does not affect abstract methods, but it does affect code generated for the implementation.</span></span>

<span data-ttu-id="693e7-197">此属性需要 [AllowUnsafeBlocks](../compiler-options/language.md#allowunsafeblocks) 编译器选项。</span><span class="sxs-lookup"><span data-stu-id="693e7-197">This attribute requires the [AllowUnsafeBlocks](../compiler-options/language.md#allowunsafeblocks) compiler option.</span></span> <span data-ttu-id="693e7-198">这是为了发出信号，在某些情况下，代码可以查看未分配的内存（例如，读取未初始化的堆栈分配的内存）。</span><span class="sxs-lookup"><span data-stu-id="693e7-198">This is to signal that in some cases code could view unassigned memory (for example, reading from uninitialized stack-allocated memory).</span></span>

<span data-ttu-id="693e7-199">下面的示例阐释 `SkipLocalsInit` 属性对使用 `stackalloc` 的方法的影响。</span><span class="sxs-lookup"><span data-stu-id="693e7-199">The following example illustrates the effect of `SkipLocalsInit` attribute on a method that uses `stackalloc`.</span></span> <span data-ttu-id="693e7-200">该方法显示分配整数数组后内存中的任何内容。</span><span class="sxs-lookup"><span data-stu-id="693e7-200">The method displays whatever was in memory when the array of integers was allocated.</span></span>

:::code language="csharp" source="snippets/SkipLocalsInitExample.cs" id="ReadUninitializedMemory":::

<span data-ttu-id="693e7-201">若要亲自尝试此代码，请在 .csproj 文件中设置 `AllowUnsafeBlocks` 编译器选项：</span><span class="sxs-lookup"><span data-stu-id="693e7-201">To try this code yourself, set the `AllowUnsafeBlocks` compiler option in your *.csproj* file:</span></span>

```xml
<PropertyGroup>
  ...
  <AllowUnsafeBlocks>true</AllowUnsafeBlocks>
</PropertyGroup>
```

## <a name="see-also"></a><span data-ttu-id="693e7-202">请参阅</span><span class="sxs-lookup"><span data-stu-id="693e7-202">See also</span></span>

- <xref:System.Attribute>
- <xref:System.Reflection>
- [<span data-ttu-id="693e7-203">特性</span><span class="sxs-lookup"><span data-stu-id="693e7-203">Attributes</span></span>](../../../standard/attributes/index.md)
- [<span data-ttu-id="693e7-204">反射</span><span class="sxs-lookup"><span data-stu-id="693e7-204">Reflection</span></span>](../../programming-guide/concepts/reflection.md)
