---
description: ref 关键字 - C# 参考
title: ref 关键字 - C# 参考
ms.date: 03/29/2021
f1_keywords:
- ref_CSharpKeyword
helpviewer_keywords:
- parameters [C#], ref
- ref keyword [C#]
ms.openlocfilehash: 3392e560eaf0bac39cf4e9707574fd2bb3d96057
ms.sourcegitcommit: 109507b6c16704ed041efe9598c70cd3438a9fbc
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/31/2021
ms.locfileid: "106079435"
---
# <a name="ref-c-reference"></a><span data-ttu-id="6537a-103">ref（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="6537a-103">ref (C# Reference)</span></span>

<span data-ttu-id="6537a-104">`ref` 关键字指示按引用传递值。</span><span class="sxs-lookup"><span data-stu-id="6537a-104">The `ref` keyword indicates that a value is passed by reference.</span></span> <span data-ttu-id="6537a-105">它用在四种不同的上下文中：</span><span class="sxs-lookup"><span data-stu-id="6537a-105">It is used in four different contexts:</span></span>

- <span data-ttu-id="6537a-106">在方法签名和方法调用中，按引用将参数传递给方法。</span><span class="sxs-lookup"><span data-stu-id="6537a-106">In a method signature and in a method call, to pass an argument to a method by reference.</span></span> <span data-ttu-id="6537a-107">有关详细信息，请参阅[按引用传递参数](#passing-an-argument-by-reference)。</span><span class="sxs-lookup"><span data-stu-id="6537a-107">For more information, see [Passing an argument by reference](#passing-an-argument-by-reference).</span></span>
- <span data-ttu-id="6537a-108">在方法签名中，按引用将值返回给调用方。</span><span class="sxs-lookup"><span data-stu-id="6537a-108">In a method signature, to return a value to the caller by reference.</span></span> <span data-ttu-id="6537a-109">有关详细信息，请参阅[引用返回值](#reference-return-values)。</span><span class="sxs-lookup"><span data-stu-id="6537a-109">For more information, see [Reference return values](#reference-return-values).</span></span>
- <span data-ttu-id="6537a-110">在成员正文中，指示引用返回值是否作为调用方欲修改的引用被存储在本地。</span><span class="sxs-lookup"><span data-stu-id="6537a-110">In a member body, to indicate that a reference return value is stored locally as a reference that the caller intends to modify.</span></span> <span data-ttu-id="6537a-111">或指示局部变量按引用访问另一个值。</span><span class="sxs-lookup"><span data-stu-id="6537a-111">Or to indicate that a local variable accesses another value by reference.</span></span> <span data-ttu-id="6537a-112">有关详细信息，请参阅 [Ref 局部变量](#ref-locals)。</span><span class="sxs-lookup"><span data-stu-id="6537a-112">For more information, see [Ref locals](#ref-locals).</span></span>
- <span data-ttu-id="6537a-113">在 `struct` 声明中，声明 `ref struct` 或 `readonly ref struct`。</span><span class="sxs-lookup"><span data-stu-id="6537a-113">In a `struct` declaration, to declare a `ref struct` or a `readonly ref struct`.</span></span> <span data-ttu-id="6537a-114">有关详细信息，请参阅[结构类型](../builtin-types/struct.md)一文中的 [`ref` 结构](../builtin-types/struct.md#ref-struct)一节。</span><span class="sxs-lookup"><span data-stu-id="6537a-114">For more information, see the [`ref` struct](../builtin-types/struct.md#ref-struct) section of the [Structure types](../builtin-types/struct.md) article.</span></span>

## <a name="passing-an-argument-by-reference"></a><span data-ttu-id="6537a-115">按引用传递参数</span><span class="sxs-lookup"><span data-stu-id="6537a-115">Passing an argument by reference</span></span>

<span data-ttu-id="6537a-116">在方法的参数列表中使用 `ref` 关键字时，它指示参数按引用传递，而非按值传递。</span><span class="sxs-lookup"><span data-stu-id="6537a-116">When used in a method's parameter list, the `ref` keyword indicates that an argument is passed by reference, not by value.</span></span> <span data-ttu-id="6537a-117">`ref` 关键字让形参成为实参的别名，这必须是变量。</span><span class="sxs-lookup"><span data-stu-id="6537a-117">The `ref` keyword makes the formal parameter an alias for the argument, which must be a variable.</span></span> <span data-ttu-id="6537a-118">换而言之，对形参执行的任何操作都是对实参执行的。</span><span class="sxs-lookup"><span data-stu-id="6537a-118">In other words, any operation on the parameter is made on the argument.</span></span>

<span data-ttu-id="6537a-119">例如，假设调用方传递了局部变量表达式或数组元素访问表达式。</span><span class="sxs-lookup"><span data-stu-id="6537a-119">For example, suppose the caller passes a local variable expression or an array element access expression.</span></span> <span data-ttu-id="6537a-120">然后，调用的方法可以替换 ref 参数引用的对象。</span><span class="sxs-lookup"><span data-stu-id="6537a-120">The called method can then replace the object to which the ref parameter refers.</span></span> <span data-ttu-id="6537a-121">在这种情况下，调用方的局部变量或数组元素会在该方法返回时引用新的对象。</span><span class="sxs-lookup"><span data-stu-id="6537a-121">In that case, the caller's local variable or the array element refers to the new object when the method returns.</span></span>

> [!NOTE]
> <span data-ttu-id="6537a-122">不要混淆通过引用传递的概念与引用类型的概念。</span><span class="sxs-lookup"><span data-stu-id="6537a-122">Don't confuse the concept of passing by reference with the concept of reference types.</span></span> <span data-ttu-id="6537a-123">这两种概念是不同的。</span><span class="sxs-lookup"><span data-stu-id="6537a-123">The two concepts are not the same.</span></span> <span data-ttu-id="6537a-124">无论方法参数是值类型还是引用类型，均可由 `ref` 修改。</span><span class="sxs-lookup"><span data-stu-id="6537a-124">A method parameter can be modified by `ref` regardless of whether it is a value type or a reference type.</span></span> <span data-ttu-id="6537a-125">当通过引用传递时，不会对值类型装箱。</span><span class="sxs-lookup"><span data-stu-id="6537a-125">There is no boxing of a value type when it is passed by reference.</span></span>  

<span data-ttu-id="6537a-126">若要使用 `ref` 参数，方法定义和调用方法均必须显式使用 `ref` 关键字，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="6537a-126">To use a `ref` parameter, both the method definition and the calling method must explicitly use the `ref` keyword, as shown in the following example.</span></span> <span data-ttu-id="6537a-127">（除了在进行 COM 调用时，调用方法可忽略 `ref`。）</span><span class="sxs-lookup"><span data-stu-id="6537a-127">(Except that the calling method can omit `ref` when making a COM call.)</span></span>

[!code-csharp-interactive[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#1)]

<span data-ttu-id="6537a-128">传递到 `ref` 或 `in` 形参的实参必须先经过初始化，然后才能传递。</span><span class="sxs-lookup"><span data-stu-id="6537a-128">An argument that is passed to a `ref` or `in` parameter must be initialized before it is passed.</span></span> <span data-ttu-id="6537a-129">该要求与 [out](out-parameter-modifier.md) 形参不同，在传递之前，不需要显式初始化该形参的实参。</span><span class="sxs-lookup"><span data-stu-id="6537a-129">This requirement differs from [out](out-parameter-modifier.md) parameters, whose arguments don't have to be explicitly initialized before they are passed.</span></span>

<span data-ttu-id="6537a-130">类的成员不能具有仅在 `ref`、`in` 或 `out` 方面不同的签名。</span><span class="sxs-lookup"><span data-stu-id="6537a-130">Members of a class can't have signatures that differ only by `ref`, `in`, or `out`.</span></span> <span data-ttu-id="6537a-131">如果类型的两个成员之间的唯一区别在于其中一个具有 `ref` 参数，而另一个具有 `out` 或 `in` 参数，则会发生编译器错误。</span><span class="sxs-lookup"><span data-stu-id="6537a-131">A compiler error occurs if the only difference between two members of a type is that one of them has a `ref` parameter and the other has an `out`, or `in` parameter.</span></span> <span data-ttu-id="6537a-132">例如，以下代码将不会编译。</span><span class="sxs-lookup"><span data-stu-id="6537a-132">The following code, for example, doesn't compile.</span></span>  

```csharp
class CS0663_Example
{
    // Compiler error CS0663: "Cannot define overloaded
    // methods that differ only on ref and out".
    public void SampleMethod(out int i) { }
    public void SampleMethod(ref int i) { }
}
```

<span data-ttu-id="6537a-133">但是，当一个方法具有 `ref`、`in` 或 `out` 参数，另一个方法具有值传递的参数时，则可以重载方法，如下面的示例所示。</span><span class="sxs-lookup"><span data-stu-id="6537a-133">However, methods can be overloaded when one method has a `ref`, `in`, or `out` parameter and the other has a parameter that is passed by value, as shown in the following example.</span></span>
  
[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#2)]
  
 <span data-ttu-id="6537a-134">在其他要求签名匹配的情况下（如隐藏或重写），`in`、`ref` 和 `out` 是签名的一部分，相互之间不匹配。</span><span class="sxs-lookup"><span data-stu-id="6537a-134">In other situations that require signature matching, such as hiding or overriding, `in`, `ref`, and `out` are part of the signature and don't match each other.</span></span>  
  
 <span data-ttu-id="6537a-135">属性不是变量。</span><span class="sxs-lookup"><span data-stu-id="6537a-135">Properties are not variables.</span></span> <span data-ttu-id="6537a-136">它们是方法，不能传递到 `ref` 参数。</span><span class="sxs-lookup"><span data-stu-id="6537a-136">They're methods, and cannot be passed to `ref` parameters.</span></span>  
  
 <span data-ttu-id="6537a-137">不能将 `ref`、`in` 和 `out` 关键字用于以下几种方法：</span><span class="sxs-lookup"><span data-stu-id="6537a-137">You can't use the `ref`, `in`, and `out` keywords for the following kinds of methods:</span></span>  
  
- <span data-ttu-id="6537a-138">异步方法，通过使用 [async](async.md) 修饰符定义。</span><span class="sxs-lookup"><span data-stu-id="6537a-138">Async methods, which you define by using the [async](async.md) modifier.</span></span>  
- <span data-ttu-id="6537a-139">迭代器方法，包括 [yield return](yield.md) 或 `yield break` 语句。</span><span class="sxs-lookup"><span data-stu-id="6537a-139">Iterator methods, which include a [yield return](yield.md) or `yield break` statement.</span></span>

<span data-ttu-id="6537a-140">[扩展方法](../../programming-guide/classes-and-structs/extension-methods.md)还限制了对以下这些关键字的使用：</span><span class="sxs-lookup"><span data-stu-id="6537a-140">[extension methods](../../programming-guide/classes-and-structs/extension-methods.md) also have restrictions on the use of these keywords:</span></span>

- <span data-ttu-id="6537a-141">不能对扩展方法的第一个参数使用 `out` 关键字。</span><span class="sxs-lookup"><span data-stu-id="6537a-141">The `out` keyword cannot be used on the first argument of an extension method.</span></span>
- <span data-ttu-id="6537a-142">当参数不是结构或是不被约束为结构的泛型类型时，不能对扩展方法的第一个参数使用 `ref` 关键字。</span><span class="sxs-lookup"><span data-stu-id="6537a-142">The `ref` keyword cannot be used on the first argument of an extension method when the argument is not a struct, or a generic type not constrained to be a struct.</span></span>
- <span data-ttu-id="6537a-143">除非第一个参数是结构，否则不能使用 `in` 关键字。</span><span class="sxs-lookup"><span data-stu-id="6537a-143">The `in` keyword cannot be used unless the first argument is a struct.</span></span> <span data-ttu-id="6537a-144">即使约束为结构，也不能对任何泛型类型使用 `in` 关键字。</span><span class="sxs-lookup"><span data-stu-id="6537a-144">The `in` keyword cannot be used on any generic type, even when constrained to be a struct.</span></span>

## <a name="passing-an-argument-by-reference-an-example"></a><span data-ttu-id="6537a-145">按引用传递参数：示例</span><span class="sxs-lookup"><span data-stu-id="6537a-145">Passing an argument by reference: An example</span></span>

<span data-ttu-id="6537a-146">前面的示例按引用传递值类型。</span><span class="sxs-lookup"><span data-stu-id="6537a-146">The previous examples pass value types by reference.</span></span> <span data-ttu-id="6537a-147">还可使用 `ref` 关键字按引用传递引用类型。</span><span class="sxs-lookup"><span data-stu-id="6537a-147">You can also use the `ref` keyword to pass reference types by reference.</span></span> <span data-ttu-id="6537a-148">按引用传递引用类型使所调用方能够替换调用方中引用参数引用的对象。</span><span class="sxs-lookup"><span data-stu-id="6537a-148">Passing a reference type by reference enables the called method to replace the object to which the reference parameter refers in the caller.</span></span> <span data-ttu-id="6537a-149">对象的存储位置按引用参数的值传递到方法。</span><span class="sxs-lookup"><span data-stu-id="6537a-149">The storage location of the object is passed to the method as the value of the reference parameter.</span></span> <span data-ttu-id="6537a-150">如果更改参数存储位置中的值（以指向新对象），你还可以将存储位置更改为调用方所引用的位置。</span><span class="sxs-lookup"><span data-stu-id="6537a-150">If you change the value in the storage location of the parameter (to point to a new object), you also change the storage location to which the caller refers.</span></span> <span data-ttu-id="6537a-151">下面的示例将引用类型的实例作为 `ref` 参数传递。</span><span class="sxs-lookup"><span data-stu-id="6537a-151">The following example passes an instance of a reference type as a `ref` parameter.</span></span>
  
[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#3)]

<span data-ttu-id="6537a-152">有关如何通过值和引用传递引用类型的详细信息，请参阅[传递引用类型参数](../../programming-guide/classes-and-structs/passing-reference-type-parameters.md)。</span><span class="sxs-lookup"><span data-stu-id="6537a-152">For more information about how to pass reference types by value and by reference, see [Passing Reference-Type Parameters](../../programming-guide/classes-and-structs/passing-reference-type-parameters.md).</span></span>
  
## <a name="reference-return-values"></a><span data-ttu-id="6537a-153">引用返回值</span><span class="sxs-lookup"><span data-stu-id="6537a-153">Reference return values</span></span>

<span data-ttu-id="6537a-154">引用返回值（或 ref 返回值）是由方法按引用向调用方返回的值。</span><span class="sxs-lookup"><span data-stu-id="6537a-154">Reference return values (or ref returns) are values that a method returns by reference to the caller.</span></span> <span data-ttu-id="6537a-155">即是说，调用方可以修改方法所返回的值，此更改反映在调用方法中的对象的状态中。</span><span class="sxs-lookup"><span data-stu-id="6537a-155">That is, the caller can modify the value returned by a method, and that change is reflected in the state of the object in the calling method.</span></span>

<span data-ttu-id="6537a-156">使用 `ref` 关键字来定义引用返回值：</span><span class="sxs-lookup"><span data-stu-id="6537a-156">A reference return value is defined by using the `ref` keyword:</span></span>

- <span data-ttu-id="6537a-157">在方法签名中。</span><span class="sxs-lookup"><span data-stu-id="6537a-157">In the method signature.</span></span> <span data-ttu-id="6537a-158">例如，下列方法签名指示 `GetCurrentPrice` 方法按引用返回了 <xref:System.Decimal> 值。</span><span class="sxs-lookup"><span data-stu-id="6537a-158">For example, the following method signature indicates that the `GetCurrentPrice` method returns a <xref:System.Decimal> value by reference.</span></span>

```csharp
public ref decimal GetCurrentPrice()
```

- <span data-ttu-id="6537a-159">在 `return` 标记和方法的 `return` 语句中返回的变量之间。</span><span class="sxs-lookup"><span data-stu-id="6537a-159">Between the `return` token and the variable returned in a `return` statement in the method.</span></span> <span data-ttu-id="6537a-160">例如：</span><span class="sxs-lookup"><span data-stu-id="6537a-160">For example:</span></span>

```csharp
return ref DecimalArray[0];
```

<span data-ttu-id="6537a-161">为方便调用方修改对象的状态，引用返回值必须存储在被显式定义为 [ref 局部变量](#ref-locals)的变量中。</span><span class="sxs-lookup"><span data-stu-id="6537a-161">In order for the caller to modify the object's state, the reference return value must be stored to a variable that is explicitly defined as a [ref local](#ref-locals).</span></span>

<span data-ttu-id="6537a-162">下面是一个更完整的 ref 返回示例，同时显示方法签名和方法主体。</span><span class="sxs-lookup"><span data-stu-id="6537a-162">Here's a more complete ref return example, showing both the method signature and method body.</span></span>

[!code-csharp[FindReturningRef](~/samples/snippets/csharp/new-in-7/MatrixSearch.cs#FindReturningRef "Find returning by reference")]

<span data-ttu-id="6537a-163">所调用方法还可能会将返回值声明为 `ref readonly` 以按引用返回值，并坚持调用代码无法修改返回的值。</span><span class="sxs-lookup"><span data-stu-id="6537a-163">The called method may also declare the return value as `ref readonly` to return the value by reference, and enforce that the calling code can't modify the returned value.</span></span> <span data-ttu-id="6537a-164">调用方法可以通过将返回值存储在本地 [ref readonly](#ref-readonly-locals) 变量中来避免复制该值。</span><span class="sxs-lookup"><span data-stu-id="6537a-164">The calling method can avoid copying the returned value by storing the value in a local [ref readonly](#ref-readonly-locals) variable.</span></span>

<span data-ttu-id="6537a-165">有关示例，请参阅 [ref 返回值和 ref 局部变量示例](#a-ref-returns-and-ref-locals-example)。</span><span class="sxs-lookup"><span data-stu-id="6537a-165">For an example, see [A ref returns and ref locals example](#a-ref-returns-and-ref-locals-example).</span></span>

## <a name="ref-locals"></a><span data-ttu-id="6537a-166">ref 局部变量</span><span class="sxs-lookup"><span data-stu-id="6537a-166">Ref locals</span></span>

<span data-ttu-id="6537a-167">ref 局部变量用于指代使用 `return ref` 返回的值。</span><span class="sxs-lookup"><span data-stu-id="6537a-167">A ref local variable is used to refer to values returned using `return ref`.</span></span> <span data-ttu-id="6537a-168">无法将 ref 局部变量初始化为非 ref 返回值。</span><span class="sxs-lookup"><span data-stu-id="6537a-168">A ref local variable cannot be initialized to a non-ref return value.</span></span> <span data-ttu-id="6537a-169">也就是说，初始化的右侧必须为引用。</span><span class="sxs-lookup"><span data-stu-id="6537a-169">In other words, the right-hand side of the initialization must be a reference.</span></span> <span data-ttu-id="6537a-170">任何对 ref 本地变量值的修改都将反映在对象的状态中，该对象的方法按引用返回值。</span><span class="sxs-lookup"><span data-stu-id="6537a-170">Any modifications to the value of the ref local are reflected in the state of the object whose method returned the value by reference.</span></span>

<span data-ttu-id="6537a-171">可在以下两个位置使用 `ref` 关键字来定义 ref 局部变量：</span><span class="sxs-lookup"><span data-stu-id="6537a-171">You define a ref local by using the `ref` keyword in two places:</span></span>

- <span data-ttu-id="6537a-172">在变量声明之前。</span><span class="sxs-lookup"><span data-stu-id="6537a-172">Before the variable declaration.</span></span>
- <span data-ttu-id="6537a-173">紧接在调用按引用返回值的方法之前。</span><span class="sxs-lookup"><span data-stu-id="6537a-173">Immediately before the call to the method that returns the value by reference.</span></span>

<span data-ttu-id="6537a-174">例如，下列语句定义名为 `GetEstimatedValue` 的方法返回的 ref 局部变量值：</span><span class="sxs-lookup"><span data-stu-id="6537a-174">For example, the following statement defines a ref local value that is returned by a method named `GetEstimatedValue`:</span></span>

```csharp
ref decimal estValue = ref Building.GetEstimatedValue();
```

<span data-ttu-id="6537a-175">可通过相同方式按引用访问值。</span><span class="sxs-lookup"><span data-stu-id="6537a-175">You can access a value by reference in the same way.</span></span> <span data-ttu-id="6537a-176">在某些情况下，按引用访问值可避免潜在的高开销复制操作，从而提高性能。</span><span class="sxs-lookup"><span data-stu-id="6537a-176">In some cases, accessing a value by reference increases performance by avoiding a potentially expensive copy operation.</span></span> <span data-ttu-id="6537a-177">例如，以下语句显示如何定义一个用于引用值的 ref 局部变量。</span><span class="sxs-lookup"><span data-stu-id="6537a-177">For example, the following statement shows how to define a ref local variable that is used to reference a value.</span></span>

```csharp
ref VeryLargeStruct reflocal = ref veryLargeStruct;
```

<span data-ttu-id="6537a-178">在这两个示例中，必须在两个位置同时使用 `ref` 关键字，否则编译器将生成错误 CS8172：“无法使用值对按引用变量进行初始化”。</span><span class="sxs-lookup"><span data-stu-id="6537a-178">In both examples the `ref` keyword must be used in both places, or the compiler generates error CS8172, "Cannot initialize a by-reference variable with a value."</span></span>

<span data-ttu-id="6537a-179">从 C# 7.3 开始，`foreach` 语句的迭代变量可以是 ref 局部变量，也可以是 ref readonly 局部变量。</span><span class="sxs-lookup"><span data-stu-id="6537a-179">Beginning with C# 7.3, the iteration variable of the `foreach` statement can be a ref local or ref readonly local variable.</span></span> <span data-ttu-id="6537a-180">有关详细信息，请参阅 [foreach 语句](foreach-in.md)一文。</span><span class="sxs-lookup"><span data-stu-id="6537a-180">For more information, see the [foreach statement](foreach-in.md) article.</span></span>

<span data-ttu-id="6537a-181">此外，从 C# 7.3 开始，可以使用 [ref 赋值运算符](../operators/assignment-operator.md#ref-assignment-operator)重新分配 ref local 或 ref readonly local 变量。</span><span class="sxs-lookup"><span data-stu-id="6537a-181">Also beginning with C# 7.3, you can reassign a ref local or ref readonly local variable with the [ref assignment operator](../operators/assignment-operator.md#ref-assignment-operator).</span></span>

## <a name="ref-readonly-locals"></a><span data-ttu-id="6537a-182">Ref readonly 局部变量</span><span class="sxs-lookup"><span data-stu-id="6537a-182">Ref readonly locals</span></span>

<span data-ttu-id="6537a-183">Ref readonly 局部变量用于指代在其签名中具有 `ref readonly` 并使用 `return ref` 的方法或属性返回的值。</span><span class="sxs-lookup"><span data-stu-id="6537a-183">A ref readonly local is used to refer to values returned by a method or property that has `ref readonly` in its signature and uses `return ref`.</span></span> <span data-ttu-id="6537a-184">`ref readonly` 变量将 `ref` 局部变量的属性与 `readonly` 变量结合使用：它是所分配到的存储的别名，且无法修改。</span><span class="sxs-lookup"><span data-stu-id="6537a-184">A `ref readonly` variable combines the properties of a `ref` local variable with a `readonly` variable: it's an alias to the storage it's assigned to, and it cannot be modified.</span></span>

## <a name="a-ref-returns-and-ref-locals-example"></a><span data-ttu-id="6537a-185">ref 返回值和 ref 局部变量示例</span><span class="sxs-lookup"><span data-stu-id="6537a-185">A ref returns and ref locals example</span></span>

<span data-ttu-id="6537a-186">下列示例定义一个具有两个 <xref:System.String> 字段（`Title` 和 `Author`）的 `Book` 类。</span><span class="sxs-lookup"><span data-stu-id="6537a-186">The following example defines a `Book` class that has two <xref:System.String> fields, `Title` and `Author`.</span></span> <span data-ttu-id="6537a-187">还定义包含 `Book` 对象的专用数组的 `BookCollection` 类。</span><span class="sxs-lookup"><span data-stu-id="6537a-187">It also defines a `BookCollection` class that includes a private array of `Book` objects.</span></span> <span data-ttu-id="6537a-188">通过调用 `GetBookByTitle` 方法，可按引用返回个别 book 对象。</span><span class="sxs-lookup"><span data-stu-id="6537a-188">Individual book objects are returned by reference by calling its `GetBookByTitle` method.</span></span>

[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#4)]

<span data-ttu-id="6537a-189">调用方将 `GetBookByTitle` 方法所返回的值存储为 ref 局部变量时，调用方对返回值所做的更改将反映在 `BookCollection` 对象中，如下例所示。</span><span class="sxs-lookup"><span data-stu-id="6537a-189">When the caller stores the value returned by the `GetBookByTitle` method as a ref local, changes that the caller makes to the return value are reflected in the `BookCollection` object, as the following example shows.</span></span>

[!code-csharp[csrefKeywordsMethodParams#6](~/samples/snippets/csharp/language-reference/keywords/in-ref-out-modifier/RefParameterModifier.cs#5)]

## <a name="c-language-specification"></a><span data-ttu-id="6537a-190">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="6537a-190">C# language specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a><span data-ttu-id="6537a-191">请参阅</span><span class="sxs-lookup"><span data-stu-id="6537a-191">See also</span></span>

- [<span data-ttu-id="6537a-192">编写安全高效的代码</span><span class="sxs-lookup"><span data-stu-id="6537a-192">Write safe efficient code</span></span>](../../write-safe-efficient-code.md)
- [<span data-ttu-id="6537a-193">ref 返回结果和局部变量</span><span class="sxs-lookup"><span data-stu-id="6537a-193">Ref returns and ref locals</span></span>](../../programming-guide/classes-and-structs/ref-returns.md)
- [<span data-ttu-id="6537a-194">条件 ref 表达式</span><span class="sxs-lookup"><span data-stu-id="6537a-194">Conditional ref expression</span></span>](../operators/conditional-operator.md#conditional-ref-expression)
- [<span data-ttu-id="6537a-195">传递参数</span><span class="sxs-lookup"><span data-stu-id="6537a-195">Passing Parameters</span></span>](../../programming-guide/classes-and-structs/passing-parameters.md)
- [<span data-ttu-id="6537a-196">方法参数</span><span class="sxs-lookup"><span data-stu-id="6537a-196">Method Parameters</span></span>](method-parameters.md)
- [<span data-ttu-id="6537a-197">C# 参考</span><span class="sxs-lookup"><span data-stu-id="6537a-197">C# Reference</span></span>](../index.md)
- [<span data-ttu-id="6537a-198">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="6537a-198">C# Programming Guide</span></span>](../../programming-guide/index.md)
- [<span data-ttu-id="6537a-199">C# 关键字</span><span class="sxs-lookup"><span data-stu-id="6537a-199">C# Keywords</span></span>](index.md)
