---
title: 记录 - C# 参考
description: 了解 C# 中的记录类型
ms.date: 02/25/2021
f1_keywords:
- record_CSharpKeyword
helpviewer_keywords:
- record keyword [C#]
- record type [C#]
ms.openlocfilehash: 067046a66c6eb1f314c123de8bde2d023f1f374a
ms.sourcegitcommit: e7e0921d0a10f85e9cb12f8b87cc1639a6c8d3fe
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/09/2021
ms.locfileid: "107255111"
---
# <a name="records-c-reference"></a><span data-ttu-id="9ed22-103">记录（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="9ed22-103">Records (C# reference)</span></span>

<span data-ttu-id="9ed22-104">从 C# 9 开始，可以使用 `record` 关键字定义一个[引用类型](reference-types.md)，用来提供用于封装数据的内置功能。</span><span class="sxs-lookup"><span data-stu-id="9ed22-104">Beginning with C# 9, you use the `record` keyword to define a [reference type](reference-types.md) that provides built-in functionality for encapsulating data.</span></span> <span data-ttu-id="9ed22-105">通过使用位置参数或标准属性语法，可以创建具有不可变属性的记录类型：</span><span class="sxs-lookup"><span data-stu-id="9ed22-105">You can create record types with immutable properties by using positional parameters or standard property syntax:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalRecord":::
:::code language="csharp" source="snippets/shared/RecordType.cs" id="ImmutableRecord":::

<span data-ttu-id="9ed22-106">此外，还可以创建具有可变属性和字段的记录类型：</span><span class="sxs-lookup"><span data-stu-id="9ed22-106">You can also create record types with mutable properties and fields:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="MutableRecord":::

<span data-ttu-id="9ed22-107">虽然记录可以是可变的，但它们主要用于支持不可变的数据模型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-107">While records can be mutable, they are primarily intended for supporting immutable data models.</span></span> <span data-ttu-id="9ed22-108">记录类型提供以下功能：</span><span class="sxs-lookup"><span data-stu-id="9ed22-108">The record type offers the following features:</span></span>

* [<span data-ttu-id="9ed22-109">用于创建具有不可变属性的引用类型的简明语法</span><span class="sxs-lookup"><span data-stu-id="9ed22-109">Concise syntax for creating a reference type with immutable properties</span></span>](#positional-syntax-for-property-definition)
* <span data-ttu-id="9ed22-110">内置行为对于以数据为中心的引用类型非常有用：</span><span class="sxs-lookup"><span data-stu-id="9ed22-110">Built-in behavior useful for a data-centric reference type:</span></span>
  * [<span data-ttu-id="9ed22-111">值相等性</span><span class="sxs-lookup"><span data-stu-id="9ed22-111">Value equality</span></span>](#value-equality)
  * [<span data-ttu-id="9ed22-112">非破坏性变化的简明语法</span><span class="sxs-lookup"><span data-stu-id="9ed22-112">Concise syntax for nondestructive mutation</span></span>](#nondestructive-mutation)
  * [<span data-ttu-id="9ed22-113">用于显示的内置格式设置</span><span class="sxs-lookup"><span data-stu-id="9ed22-113">Built-in formatting for display</span></span>](#built-in-formatting-for-display)
* [<span data-ttu-id="9ed22-114">支持继承层次结构</span><span class="sxs-lookup"><span data-stu-id="9ed22-114">Support for inheritance hierarchies</span></span>](#inheritance)

<span data-ttu-id="9ed22-115">你还可以使用[结构类型](struct.md)来设计以数据为中心的类型，这些类型提供值相等性，并且很少或没有任何行为。</span><span class="sxs-lookup"><span data-stu-id="9ed22-115">You can also use [structure types](struct.md) to design data-centric types that provide value equality and little or no behavior.</span></span> <span data-ttu-id="9ed22-116">但对于相对较大的数据模型，结构类型有一些缺点：</span><span class="sxs-lookup"><span data-stu-id="9ed22-116">But for relatively large data models, structure types have some disadvantages:</span></span>

* <span data-ttu-id="9ed22-117">它们不支持继承。</span><span class="sxs-lookup"><span data-stu-id="9ed22-117">They don't support inheritance.</span></span>
* <span data-ttu-id="9ed22-118">它们在确定值相等性时效率较低。</span><span class="sxs-lookup"><span data-stu-id="9ed22-118">They're less efficient at determining value equality.</span></span> <span data-ttu-id="9ed22-119">对于值类型，<xref:System.ValueType.Equals%2A?displayProperty=nameWithType> 方法使用反射来查找所有字段。</span><span class="sxs-lookup"><span data-stu-id="9ed22-119">For value types, the <xref:System.ValueType.Equals%2A?displayProperty=nameWithType> method uses reflection to find all fields.</span></span> <span data-ttu-id="9ed22-120">对于记录，编译器将生成 `Equals` 方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-120">For records, the compiler generates the `Equals` method.</span></span> <span data-ttu-id="9ed22-121">实际上，记录中的值相等性实现的速度明显更快。</span><span class="sxs-lookup"><span data-stu-id="9ed22-121">In practice, the implementation of value equality in records is measurably faster.</span></span>
* <span data-ttu-id="9ed22-122">在某些情况下，它们会占用更多内存，因为每个实例都有所有数据的完整副本。</span><span class="sxs-lookup"><span data-stu-id="9ed22-122">They use more memory in some scenarios, since every instance has a complete copy of all of the data.</span></span> <span data-ttu-id="9ed22-123">记录类型是[引用类型](reference-types.md)，因此，记录实例只包含对数据的引用。</span><span class="sxs-lookup"><span data-stu-id="9ed22-123">Record types are [reference types](reference-types.md), so a record instance contains only a reference to the data.</span></span>

## <a name="positional-syntax-for-property-definition"></a><span data-ttu-id="9ed22-124">属性定义的位置语法</span><span class="sxs-lookup"><span data-stu-id="9ed22-124">Positional syntax for property definition</span></span>

<span data-ttu-id="9ed22-125">在创建实例时，可以使用位置参数来声明记录的属性，并初始化属性值：</span><span class="sxs-lookup"><span data-stu-id="9ed22-125">You can use positional parameters to declare properties of a record and to initialize the property values when you create an instance:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="InstantiatePositional":::

<span data-ttu-id="9ed22-126">当你为属性定义使用位置语法时，编译器将创建以下内容：</span><span class="sxs-lookup"><span data-stu-id="9ed22-126">When you use the positional syntax for property definition, the compiler creates:</span></span>

* <span data-ttu-id="9ed22-127">为记录声明中提供的每个位置参数提供一个公共的 init-only 自动实现的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-127">A public init-only auto-implemented property for each positional parameter provided in the record declaration.</span></span> <span data-ttu-id="9ed22-128">[init-only](../keywords/init.md) 属性只能在构造函数中或使用属性初始值设定项来设置。</span><span class="sxs-lookup"><span data-stu-id="9ed22-128">An [init-only](../keywords/init.md) property can only be set in the constructor or by using a property initializer.</span></span>
* <span data-ttu-id="9ed22-129">主构造函数，它的参数与记录声明上的位置参数匹配。</span><span class="sxs-lookup"><span data-stu-id="9ed22-129">A primary constructor whose parameters match the positional parameters on the record declaration.</span></span>
* <span data-ttu-id="9ed22-130">一个 `Deconstruct` 方法，对记录声明中提供的每个位置参数都有一个 `out` 参数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-130">A `Deconstruct` method with an `out` parameter for each positional parameter provided in the record declaration.</span></span> <span data-ttu-id="9ed22-131">只有当有两个或更多的位置参数时，才会提供这个方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-131">This method is provided only if there are two or more positional parameters.</span></span> <span data-ttu-id="9ed22-132">此方法解构了使用位置语法定义的属性；它忽略了使用标准属性语法定义的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-132">The method deconstructs properties defined by using positional syntax; it ignores properties that are defined by using standard property syntax.</span></span>

<span data-ttu-id="9ed22-133">如果生成的自动实现的属性定义并不是你所需要的，你可以自行定义同名的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-133">If the generated auto-implemented property definition isn't what you want, you can define your own property of the same name.</span></span> <span data-ttu-id="9ed22-134">如果这样做，生成的构造函数和解构函数将使用你的属性定义。</span><span class="sxs-lookup"><span data-stu-id="9ed22-134">If you do that, the generated constructor and deconstructor will use your property definition.</span></span> <span data-ttu-id="9ed22-135">例如，下面的示例定义了 `FirstName` 位置属性 `internal` 而不是 `public`。</span><span class="sxs-lookup"><span data-stu-id="9ed22-135">For instance, the following example makes the `FirstName` positional property `internal` instead of `public`.</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalWithManualProperty":::

<span data-ttu-id="9ed22-136">记录类型不需要声明任何位置属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-136">A record type doesn't have to declare any positional properties.</span></span> <span data-ttu-id="9ed22-137">你可以在没有任何位置属性的情况下声明一个记录，也可以声明其他字段和属性，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="9ed22-137">You can declare a record without any positional properties, and you can declare additional fields and properties, as in the following example:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="MixedSyntax":::

<span data-ttu-id="9ed22-138">如果使用标准属性语法来定义属性，但省略了访问修饰符，这些属性将隐式地 `public`。</span><span class="sxs-lookup"><span data-stu-id="9ed22-138">If you define properties by using standard property syntax but omit the access modifier, the properties are implicitly `public`.</span></span>
<!-- Todo -- Explain issues surrounding use of attributes on positional properties. -->

## <a name="immutability"></a><span data-ttu-id="9ed22-139">不可变性</span><span class="sxs-lookup"><span data-stu-id="9ed22-139">Immutability</span></span>

<span data-ttu-id="9ed22-140">记录类型不一定是不可变的。</span><span class="sxs-lookup"><span data-stu-id="9ed22-140">A record type is not necessarily immutable.</span></span> <span data-ttu-id="9ed22-141">可以用 `set` 访问器和非 `readonly` 的字段来声明属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-141">You can declare properties with `set` accessors and fields that aren't `readonly`.</span></span> <span data-ttu-id="9ed22-142">虽然记录可以是可变的，但它们使创建不可变的数据模型变得更容易。</span><span class="sxs-lookup"><span data-stu-id="9ed22-142">But while records can be mutable, they make it easier to create immutable data models.</span></span>

<span data-ttu-id="9ed22-143">如果你需要一个以数据为中心的类型是线程安全的，或者需要使哈希表中的哈希代码保持不变，那么不可变性很有用。</span><span class="sxs-lookup"><span data-stu-id="9ed22-143">Immutability can be useful when you need a data-centric type to be thread-safe or you're depending on a hash code remaining the same in a hash table.</span></span> <span data-ttu-id="9ed22-144">但不可变性并不是适用于所有的数据场景。</span><span class="sxs-lookup"><span data-stu-id="9ed22-144">Immutability isn't appropriate for all data scenarios, however.</span></span> <span data-ttu-id="9ed22-145">例如，[Entity Framework Core](/ef/core/) 就不支持通过不可变实体类型进行更新。</span><span class="sxs-lookup"><span data-stu-id="9ed22-145">[Entity Framework Core](/ef/core/), for example, doesn't support updating with immutable entity types.</span></span>

<span data-ttu-id="9ed22-146">init-only 属性无论是通过位置参数创建的，还是通过指定 `init` 访问器创建的，都具有浅的不可变性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-146">Init-only properties, whether created from positional parameters or by specifying `init` accessors, have *shallow immutability*.</span></span> <span data-ttu-id="9ed22-147">初始化后，将不能更改值型属性的值或引用型属性的引用。</span><span class="sxs-lookup"><span data-stu-id="9ed22-147">After initialization, you can't change the value of value-type properties or the reference of reference-type properties.</span></span> <span data-ttu-id="9ed22-148">不过，引用型属性引用的数据是可以更改的。</span><span class="sxs-lookup"><span data-stu-id="9ed22-148">However, the data that a reference-type property refers to can be changed.</span></span> <span data-ttu-id="9ed22-149">下面的示例展示了引用型不可变属性的内容（本例中是数组）是可变的：</span><span class="sxs-lookup"><span data-stu-id="9ed22-149">The following example shows that the content of a reference-type immutable property (an array in this case) is mutable:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ShallowImmutability":::

<span data-ttu-id="9ed22-150">记录类型特有的功能是由编译器合成的方法实现的，这些方法都不会通过修改对象状态来影响不可变性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-150">The features unique to record types are implemented by compiler-synthesized methods, and none of these methods compromises immutability by modifying object state.</span></span>

## <a name="value-equality"></a><span data-ttu-id="9ed22-151">值相等性</span><span class="sxs-lookup"><span data-stu-id="9ed22-151">Value equality</span></span>

<span data-ttu-id="9ed22-152">值相等性是指如果记录类型的两个变量类型相匹配，且所有属性和字段值都一致，那么记录类型的两个变量是相等的。</span><span class="sxs-lookup"><span data-stu-id="9ed22-152">Value equality means that two variables of a record type are equal if the types match and all property and field values match.</span></span> <span data-ttu-id="9ed22-153">对于其他引用类型，相等是指标识。</span><span class="sxs-lookup"><span data-stu-id="9ed22-153">For other reference types, equality means identity.</span></span> <span data-ttu-id="9ed22-154">也就是说，如果一个引用类型的两个变量引用同一个对象，那么这两个变量是相等的。</span><span class="sxs-lookup"><span data-stu-id="9ed22-154">That is, two variables of a reference type are equal if they refer to the same object.</span></span>

<span data-ttu-id="9ed22-155">一些数据模型需要引用相等性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-155">Reference equality is required for some data models.</span></span> <span data-ttu-id="9ed22-156">例如，[Entity Framework Core](/ef/core/) 依赖于引用相等性来确保它对概念上是一个实体的实体类型只使用一个实例。</span><span class="sxs-lookup"><span data-stu-id="9ed22-156">For example, [Entity Framework Core](/ef/core/) depends on reference equality to ensure that it uses only one instance of an entity type for what is conceptually one entity.</span></span> <span data-ttu-id="9ed22-157">因此，记录类型不适合用作 Entity Framework Core 中的实体类型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-157">For this reason, record types aren't appropriate for use as entity types in Entity Framework Core.</span></span>

<span data-ttu-id="9ed22-158">下面的示例说明了记录类型的值相等性：</span><span class="sxs-lookup"><span data-stu-id="9ed22-158">The following example illustrates value equality of record types:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="Equality":::

<span data-ttu-id="9ed22-159">为了实现值相等性，编译器合成了以下方法：</span><span class="sxs-lookup"><span data-stu-id="9ed22-159">To implement value equality, the compiler synthesizes the following methods:</span></span>

* <span data-ttu-id="9ed22-160"><xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 的替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-160">An override of <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType>.</span></span>

  <span data-ttu-id="9ed22-161">当两个参数都为非 NULL 时，此方法被用作 <xref:System.Object.Equals(System.Object,System.Object)?displayProperty=nameWithType> 静态方法的基础。</span><span class="sxs-lookup"><span data-stu-id="9ed22-161">This method is used as the basis for the <xref:System.Object.Equals(System.Object,System.Object)?displayProperty=nameWithType> static method when both parameters are non-null.</span></span>

* <span data-ttu-id="9ed22-162">一个虚拟的 `Equals` 方法，其参数为记录类型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-162">A virtual `Equals` method whose parameter is the record type.</span></span> <span data-ttu-id="9ed22-163">此方法实现 <xref:System.IEquatable%601>。</span><span class="sxs-lookup"><span data-stu-id="9ed22-163">This method implements <xref:System.IEquatable%601>.</span></span>

* <span data-ttu-id="9ed22-164"><xref:System.Object.GetHashCode?displayProperty=nameWithType> 的替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-164">An override of <xref:System.Object.GetHashCode?displayProperty=nameWithType>.</span></span>

* <span data-ttu-id="9ed22-165">运算符 `==` 和 `!=` 的替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-165">Overrides of operators `==` and `!=`.</span></span>

<span data-ttu-id="9ed22-166">在 `class` 类型中，可以手动替代相等性方法和运算符以实现值相等性，但开发和测试这种代码会非常耗时，而且容易出错。</span><span class="sxs-lookup"><span data-stu-id="9ed22-166">In `class` types, you could manually override equality methods and operators to achieve value equality, but developing and testing that code would be time-consuming and error-prone.</span></span> <span data-ttu-id="9ed22-167">内置此功能可防止在添加或更改属性或字段时忘记更新自定义替代代码导致的 bug。</span><span class="sxs-lookup"><span data-stu-id="9ed22-167">Having this functionality built-in prevents bugs that would result from forgetting to update custom override code when properties or fields are added or changed.</span></span>

<span data-ttu-id="9ed22-168">你可以编写自己的实现来替换这些合成的方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-168">You can write your own implementations to replace any of these synthesized methods.</span></span> <span data-ttu-id="9ed22-169">如果记录类型的方法与任何合成方法的签名匹配，则编译器不会合成该方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-169">If a record type has a method that matches the signature of any synthesized method, the compiler doesn't synthesize that method.</span></span>

<span data-ttu-id="9ed22-170">如果在记录类型中提供自己的 `Equals` 实现，则还应提供 `GetHashCode` 的实现。</span><span class="sxs-lookup"><span data-stu-id="9ed22-170">If you provide your own implementation of `Equals` in a record type, provide an implementation of `GetHashCode` also.</span></span>

## <a name="nondestructive-mutation"></a><span data-ttu-id="9ed22-171">非破坏性变化</span><span class="sxs-lookup"><span data-stu-id="9ed22-171">Nondestructive mutation</span></span>

<span data-ttu-id="9ed22-172">如果需要改变记录实例的不可变属性，可以使用 `with` 表达式来实现非破坏性变化。</span><span class="sxs-lookup"><span data-stu-id="9ed22-172">If you need to mutate immutable properties of a record instance, you can use a `with` expression to achieve *nondestructive mutation*.</span></span> <span data-ttu-id="9ed22-173">`with` 表达式创建一个新的记录实例，该实例是现有记录实例的一个副本，修改了指定属性和字段。</span><span class="sxs-lookup"><span data-stu-id="9ed22-173">A `with` expression makes a new record instance that is a copy of an existing record instance, with specified properties and fields modified.</span></span> <span data-ttu-id="9ed22-174">使用[对象初始值设定项](../../programming-guide/classes-and-structs/object-and-collection-initializers.md)语法来指定要更改的值，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="9ed22-174">You use [object initializer](../../programming-guide/classes-and-structs/object-and-collection-initializers.md) syntax to specify the values to be changed, as shown in the following example:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="WithExpressions":::

<span data-ttu-id="9ed22-175">`with` 表达式可以设置位置属性或使用标准属性语法创建的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-175">The `with` expression can set positional properties or properties created by using standard property syntax.</span></span> <span data-ttu-id="9ed22-176">非位置属性必须有一个 `init` 或 `set` 访问器才能在 `with` 表达式中进行更改。</span><span class="sxs-lookup"><span data-stu-id="9ed22-176">Non-positional properties must have an `init` or `set` accessor to be changed in a `with` expression.</span></span>

<span data-ttu-id="9ed22-177">`with` 表达式的结果是一个浅的副本，这意味着对于引用属性，只复制对实例的引用。</span><span class="sxs-lookup"><span data-stu-id="9ed22-177">The result of a `with` expression is a *shallow copy*, which means that for a reference property, only the reference to an instance is copied.</span></span> <span data-ttu-id="9ed22-178">原始记录和副本最终都具有对同一实例的引用。</span><span class="sxs-lookup"><span data-stu-id="9ed22-178">Both the original record and the copy end up with a reference to the same instance.</span></span>

<span data-ttu-id="9ed22-179">为了实现此功能，编译器合成了一个克隆方法和一个复制构造函数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-179">To implement this feature, the compiler synthesizes a clone method and a copy constructor.</span></span> <span data-ttu-id="9ed22-180">构造函数接收一个要复制的记录的实例，然后调用克隆方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-180">The constructor takes an instance of the record to be copied and calls the clone method.</span></span> <span data-ttu-id="9ed22-181">当你使用 `with` 表达式时，编译器将创建调用复制构造函数的代码，然后设置在 `with` 表达式中指定的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-181">When you use a `with` expression, the compiler creates code that calls the copy constructor and then sets the properties that are specified in the `with` expression.</span></span>

<span data-ttu-id="9ed22-182">如果你需要不同的复制行为，可以编写自己的复制构造函数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-182">If you need different copying behavior, you can write your own copy constructor.</span></span> <span data-ttu-id="9ed22-183">如果你这样做，编译器将不会合成复制构造函数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-183">If you do that, the compiler won't synthesize one.</span></span> <span data-ttu-id="9ed22-184">如果记录是 `sealed`，则使构造函数为 `private`，否则使其为 `protected`。</span><span class="sxs-lookup"><span data-stu-id="9ed22-184">Make your constructor `private` if the record is `sealed`, otherwise make it `protected`.</span></span>

<span data-ttu-id="9ed22-185">你不能替代克隆方法，也不能创建名为 `Clone` 的成员。</span><span class="sxs-lookup"><span data-stu-id="9ed22-185">You can't override the clone method, and you can't create a member named `Clone`.</span></span> <span data-ttu-id="9ed22-186">克隆方法的实际名称是由编译器生成的。</span><span class="sxs-lookup"><span data-stu-id="9ed22-186">The actual name of the clone method is compiler-generated.</span></span>

## <a name="built-in-formatting-for-display"></a><span data-ttu-id="9ed22-187">用于显示的内置格式设置</span><span class="sxs-lookup"><span data-stu-id="9ed22-187">Built-in formatting for display</span></span>

<span data-ttu-id="9ed22-188">记录类型具有编译器生成的 <xref:System.Object.ToString%2A> 方法，可显示公共属性和字段的名称和值。</span><span class="sxs-lookup"><span data-stu-id="9ed22-188">Record types have a compiler-generated <xref:System.Object.ToString%2A> method that displays the names and values of public properties and fields.</span></span> <span data-ttu-id="9ed22-189">`ToString` 方法返回一个格式如下的字符串：</span><span class="sxs-lookup"><span data-stu-id="9ed22-189">The `ToString` method returns a string of the following format:</span></span>

> <span data-ttu-id="9ed22-190">\<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}</span><span class="sxs-lookup"><span data-stu-id="9ed22-190">\<record type name> { \<property name> = \<value>, \<property name> = \<value>, ...}</span></span>

<span data-ttu-id="9ed22-191">对于引用类型，将显示属性所引用的对象的类型名称，而不是属性值。</span><span class="sxs-lookup"><span data-stu-id="9ed22-191">For reference types, the type name of the object that the property refers to is displayed instead of the property value.</span></span> <span data-ttu-id="9ed22-192">在下面的示例中，数组是一个引用类型，因此显示的是 `System.String[]`，而不是实际的数组元素值：</span><span class="sxs-lookup"><span data-stu-id="9ed22-192">In the following example, the array is a reference type, so `System.String[]` is displayed instead of the actual array element values:</span></span>

```
Person { FirstName = Nancy, LastName = Davolio, ChildNames = System.String[] }
```

<span data-ttu-id="9ed22-193">为了实现此功能，编译器合成了一个虚拟 `PrintMembers` 方法和一个 <xref:System.Object.ToString%2A> 替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-193">To implement this feature, the compiler synthesizes a virtual `PrintMembers` method and a <xref:System.Object.ToString%2A> override.</span></span>
<span data-ttu-id="9ed22-194">`ToString` 替代创建了一个 <xref:System.Text.StringBuilder> 对象，它的类型名称后跟一个左括号。</span><span class="sxs-lookup"><span data-stu-id="9ed22-194">The `ToString` override creates a <xref:System.Text.StringBuilder> object with the type name followed by an opening bracket.</span></span> <span data-ttu-id="9ed22-195">它调用 `PrintMembers` 以添加属性名称和值，然后添加右括号。</span><span class="sxs-lookup"><span data-stu-id="9ed22-195">It calls `PrintMembers` to add property names and values, then adds the closing bracket.</span></span> <span data-ttu-id="9ed22-196">下面的示例展示了类似于合成替代中包含的代码：</span><span class="sxs-lookup"><span data-stu-id="9ed22-196">The following example shows code similar to what the synthesized override contains:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ToStringOverrideDefault":::

<span data-ttu-id="9ed22-197">你可以提供自己的 `PrintMembers` 实现或 `ToString` 替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-197">You can provide your own implementation of `PrintMembers` or the `ToString` override.</span></span> <span data-ttu-id="9ed22-198">本文后面的[派生记录中的 `PrintMembers` 格式设置](#printmembers-formatting-in-derived-records)一节中提供了示例。</span><span class="sxs-lookup"><span data-stu-id="9ed22-198">Examples are provided in the [`PrintMembers` formatting in derived records](#printmembers-formatting-in-derived-records) section later in this article.</span></span>

## <a name="inheritance"></a><span data-ttu-id="9ed22-199">继承</span><span class="sxs-lookup"><span data-stu-id="9ed22-199">Inheritance</span></span>

<span data-ttu-id="9ed22-200">一条记录可以从另一条记录继承。</span><span class="sxs-lookup"><span data-stu-id="9ed22-200">A record can inherit from another record.</span></span> <span data-ttu-id="9ed22-201">但是，记录不能从类继承，类也不能从记录继承。</span><span class="sxs-lookup"><span data-stu-id="9ed22-201">However, a record can't inherit from a class, and a class can't inherit from a record.</span></span>

### <a name="positional-parameters-in-derived-record-types"></a><span data-ttu-id="9ed22-202">派生记录类型中的位置参数</span><span class="sxs-lookup"><span data-stu-id="9ed22-202">Positional parameters in derived record types</span></span>

<span data-ttu-id="9ed22-203">派生记录为基本记录主构造函数中的所有参数声明位置参数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-203">The derived record declares positional parameters for all the parameters in the base record primary constructor.</span></span> <span data-ttu-id="9ed22-204">基本记录声明并初始化这些属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-204">The base record declares and initializes those properties.</span></span> <span data-ttu-id="9ed22-205">派生记录不会隐藏它们，而只会创建和初始化未在其基本记录中声明的参数的属性。</span><span class="sxs-lookup"><span data-stu-id="9ed22-205">The derived record doesn't hide them, but only creates and initializes properties for parameters that aren't declared in its base record.</span></span>

<span data-ttu-id="9ed22-206">下面的示例说明了具有位置属性语法的继承：</span><span class="sxs-lookup"><span data-stu-id="9ed22-206">The following example illustrates inheritance with positional property syntax:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PositionalInheritance":::

### <a name="equality-in-inheritance-hierarchies"></a><span data-ttu-id="9ed22-207">继承层次结构中的相等性</span><span class="sxs-lookup"><span data-stu-id="9ed22-207">Equality in inheritance hierarchies</span></span>

<span data-ttu-id="9ed22-208">要使两个记录变量相等，运行时类型必须相等。</span><span class="sxs-lookup"><span data-stu-id="9ed22-208">For two record variables to be equal, the run-time type must be equal.</span></span> <span data-ttu-id="9ed22-209">包含变量的类型可能不同。</span><span class="sxs-lookup"><span data-stu-id="9ed22-209">The types of the containing variables might be different.</span></span> <span data-ttu-id="9ed22-210">下面的代码示例中说明了这一点：</span><span class="sxs-lookup"><span data-stu-id="9ed22-210">This is illustrated in the following code example:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="InheritanceEquality":::

<span data-ttu-id="9ed22-211">在本示例中，所有实例都具有相同的属性和相同的属性值。</span><span class="sxs-lookup"><span data-stu-id="9ed22-211">In the example, all instances have the same properties and the same property values.</span></span> <span data-ttu-id="9ed22-212">但 `student == teacher` 返回 `False`（尽管这两个都是 `Person` 型变量），`student == student2` 返回 `True`（尽管一个是 `Person` 变量，而另一个是 `Student` 变量）。</span><span class="sxs-lookup"><span data-stu-id="9ed22-212">But `student == teacher` returns `False` although both are `Person`-type variables, and `student == student2` returns `True` although one is a `Person` variable and one is a `Student` variable.</span></span>

<span data-ttu-id="9ed22-213">为实现此行为，编译器合成了一个 `EqualityContract` 属性，该属性返回一个与记录类型匹配的 <xref:System.Type> 对象。</span><span class="sxs-lookup"><span data-stu-id="9ed22-213">To implement this behavior, the compiler synthesizes an `EqualityContract` property that returns a <xref:System.Type> object that matches the type of the record.</span></span> <span data-ttu-id="9ed22-214">这样，相等性方法在检查相等性时，就可以比较对象的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-214">This enables the equality methods to compare the runtime type of objects when they are checking for equality.</span></span> <span data-ttu-id="9ed22-215">如果记录的基类型为 `object`，则此属性为 `virtual`。</span><span class="sxs-lookup"><span data-stu-id="9ed22-215">If the base type of a record is `object`, this property is `virtual`.</span></span> <span data-ttu-id="9ed22-216">如果基类型是其他记录类型，则此属性是一个替代。</span><span class="sxs-lookup"><span data-stu-id="9ed22-216">If the base type is another record type, this property is an override.</span></span> <span data-ttu-id="9ed22-217">如果记录类型为 `sealed`，则此属性为 `sealed`。</span><span class="sxs-lookup"><span data-stu-id="9ed22-217">If the record type is `sealed`, this property is `sealed`.</span></span>

<span data-ttu-id="9ed22-218">在比较派生类型的两个实例时，合成的相等性方法会检查基类型和派生类型的所有属性是否相等。</span><span class="sxs-lookup"><span data-stu-id="9ed22-218">When comparing two instances of a derived type, the synthesized equality methods check all properties of the base and derived types for equality.</span></span> <span data-ttu-id="9ed22-219">合成的 `GetHashCode` 方法从基类型和派生的记录类型中声明的所有属性和字段中使用 `GetHashCode` 方法。</span><span class="sxs-lookup"><span data-stu-id="9ed22-219">The synthesized `GetHashCode` method uses the `GetHashCode` method from all properties and fields declared in the base type and the derived record type.</span></span>

### <a name="with-expressions-in-derived-records"></a><span data-ttu-id="9ed22-220">派生记录中的 `with` 表达式</span><span class="sxs-lookup"><span data-stu-id="9ed22-220">`with` expressions in derived records</span></span>

<span data-ttu-id="9ed22-221">由于合成的克隆方法使用[协变返回类型](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)，因此 `with` 表达式的结果与表达式的操作数具有相同的运行时类型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-221">Because the synthesized clone method uses a [covariant return type](~/_csharplang/proposals/csharp-9.0/covariant-returns.md), the result of a `with` expression has the same run-time type as the expression's operand.</span></span> <span data-ttu-id="9ed22-222">运行时类型的所有属性都会被复制，但你只能设置编译时类型的属性，如下面的示例所示：</span><span class="sxs-lookup"><span data-stu-id="9ed22-222">All properties of the run-time type get copied, but you can only set properties of the compile-time type, as the following example shows:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="WithExpressionInheritance":::

### <a name="printmembers-formatting-in-derived-records"></a><span data-ttu-id="9ed22-223">派生记录中的 `PrintMembers` 格式设置</span><span class="sxs-lookup"><span data-stu-id="9ed22-223">`PrintMembers` formatting in derived records</span></span>

<span data-ttu-id="9ed22-224">派生记录类型的合成 `PrintMembers` 方法调用基实现。</span><span class="sxs-lookup"><span data-stu-id="9ed22-224">The synthesized `PrintMembers` method of a derived record type calls the base implementation.</span></span> <span data-ttu-id="9ed22-225">结果是派生类型和基类型的所有公共属性和字段都包含在 `ToString` 输出中，如以下示例中所示：</span><span class="sxs-lookup"><span data-stu-id="9ed22-225">The result is that all public properties and fields of both derived and base types are included in the `ToString` output, as shown in the following example:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="ToStringInheritance":::

<span data-ttu-id="9ed22-226">你可以提供自己的 `PrintMembers` 方法的实现。</span><span class="sxs-lookup"><span data-stu-id="9ed22-226">You can provide your own implementation of the `PrintMembers` method.</span></span> <span data-ttu-id="9ed22-227">如果这样做，请使用以下签名：</span><span class="sxs-lookup"><span data-stu-id="9ed22-227">If you do that, use the following signature:</span></span>

* <span data-ttu-id="9ed22-228">对于派生自 `object` 的 `sealed` 记录（不声明基本记录）：使用 `private bool PrintMembers(StringBuilder builder)`；</span><span class="sxs-lookup"><span data-stu-id="9ed22-228">For a `sealed` record that derives from `object` (doesn't declare a base record): `private bool PrintMembers(StringBuilder builder)`;</span></span>
* <span data-ttu-id="9ed22-229">对于派生自其他记录的 `sealed` 记录：使用 `protected sealed override bool PrintMembers(StringBuilder builder)`；</span><span class="sxs-lookup"><span data-stu-id="9ed22-229">For a `sealed` record that derives from another record: `protected sealed override bool PrintMembers(StringBuilder builder)`;</span></span>
* <span data-ttu-id="9ed22-230">对于非 `sealed` 且派生自对象的记录：使用 `protected virtual bool PrintMembers(StringBuilder builder);`</span><span class="sxs-lookup"><span data-stu-id="9ed22-230">For a record that isn't `sealed` and derives from object: `protected virtual bool PrintMembers(StringBuilder builder);`</span></span>
* <span data-ttu-id="9ed22-231">对于非 `sealed` 且派生自其他记录的记录：使用 `protected override bool PrintMembers(StringBuilder builder);`</span><span class="sxs-lookup"><span data-stu-id="9ed22-231">For a record that isn't `sealed` and derives from another record: `protected override bool PrintMembers(StringBuilder builder);`</span></span>

<span data-ttu-id="9ed22-232">下面的代码示例替换了合成 `PrintMembers` 方法，一个是派生自对象的记录类型，另一个是派生自另一条记录的记录类型：</span><span class="sxs-lookup"><span data-stu-id="9ed22-232">Here is an example of code that replaces the synthesized `PrintMembers` methods, one for a record type that derives from object, and one for a record type that derives from another record:</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="PrintMembersImplementation":::

### <a name="deconstructor-behavior-in-derived-records"></a><span data-ttu-id="9ed22-233">派生记录中的解构函数行为</span><span class="sxs-lookup"><span data-stu-id="9ed22-233">Deconstructor behavior in derived records</span></span>

<span data-ttu-id="9ed22-234">派生记录的 `Deconstruct` 方法返回编译时类型所有位置属性的值。</span><span class="sxs-lookup"><span data-stu-id="9ed22-234">The `Deconstruct` method of a derived record returns the values of all positional properties of the compile-time type.</span></span> <span data-ttu-id="9ed22-235">如果变量类型为基本记录，则只解构基本记录属性，除非该对象强制转换为派生类型。</span><span class="sxs-lookup"><span data-stu-id="9ed22-235">If the variable type is a base record, only the base record properties are deconstructed unless the object is cast to the derived type.</span></span> <span data-ttu-id="9ed22-236">下面的示例演示了如何对派生记录调用解构函数。</span><span class="sxs-lookup"><span data-stu-id="9ed22-236">The following example demonstrates calling a deconstructor on a derived record.</span></span>

:::code language="csharp" source="snippets/shared/RecordType.cs" id="DeconstructorInheritance":::

## <a name="generic-constraints"></a><span data-ttu-id="9ed22-237">泛型约束</span><span class="sxs-lookup"><span data-stu-id="9ed22-237">Generic constraints</span></span>

<span data-ttu-id="9ed22-238">没有任何泛型约束要求类型是记录。</span><span class="sxs-lookup"><span data-stu-id="9ed22-238">There is no generic constraint that requires a type to be a record.</span></span> <span data-ttu-id="9ed22-239">记录满足 `class` 约束条件。</span><span class="sxs-lookup"><span data-stu-id="9ed22-239">Records satisfy the `class` constraint.</span></span> <span data-ttu-id="9ed22-240">要对记录类型的特定层次结构进行约束，请像对基类一样对基本记录进行约束。</span><span class="sxs-lookup"><span data-stu-id="9ed22-240">To make a constraint on a specific hierarchy of record types, put the constraint on the base record as you would a base class.</span></span> <span data-ttu-id="9ed22-241">有关详细信息，请参阅[类型参数的约束](../../programming-guide/generics/constraints-on-type-parameters.md)。</span><span class="sxs-lookup"><span data-stu-id="9ed22-241">For more information, see [Constraints on type parameters](../../programming-guide/generics/constraints-on-type-parameters.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="9ed22-242">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="9ed22-242">C# language specification</span></span>

<span data-ttu-id="9ed22-243">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[类](~/_csharplang/spec/classes.md)部分。</span><span class="sxs-lookup"><span data-stu-id="9ed22-243">For more information, see the [Classes](~/_csharplang/spec/classes.md) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>

<span data-ttu-id="9ed22-244">若要详细了解 C# 9 及更高版本中引入的功能，请参阅以下功能建议说明：</span><span class="sxs-lookup"><span data-stu-id="9ed22-244">For more information about features introduced in C# 9 and later, see the following feature proposal notes:</span></span>

- [<span data-ttu-id="9ed22-245">记录</span><span class="sxs-lookup"><span data-stu-id="9ed22-245">Records</span></span>](~/_csharplang/proposals/csharp-9.0/records.md)
- [<span data-ttu-id="9ed22-246">init-only 资源库</span><span class="sxs-lookup"><span data-stu-id="9ed22-246">Init-only setters</span></span>](~/_csharplang/proposals/csharp-9.0/init.md)
- [<span data-ttu-id="9ed22-247">协变返回结果</span><span class="sxs-lookup"><span data-stu-id="9ed22-247">Covariant returns</span></span>](~/_csharplang/proposals/csharp-9.0/covariant-returns.md)

## <a name="see-also"></a><span data-ttu-id="9ed22-248">另请参阅</span><span class="sxs-lookup"><span data-stu-id="9ed22-248">See also</span></span>

- [<span data-ttu-id="9ed22-249">C# 参考</span><span class="sxs-lookup"><span data-stu-id="9ed22-249">C# reference</span></span>](../index.md)
- [<span data-ttu-id="9ed22-250">设计指南 - 在类和结构之间选择</span><span class="sxs-lookup"><span data-stu-id="9ed22-250">Design guidelines - Choosing between class and struct</span></span>](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [<span data-ttu-id="9ed22-251">设计指南 - 结构设计</span><span class="sxs-lookup"><span data-stu-id="9ed22-251">Design guidelines - Struct design</span></span>](../../../standard/design-guidelines/struct.md)
- [<span data-ttu-id="9ed22-252">类和结构</span><span class="sxs-lookup"><span data-stu-id="9ed22-252">Classes and structs</span></span>](../../programming-guide/classes-and-structs/index.md)
- [<span data-ttu-id="9ed22-253">`with`表达式</span><span class="sxs-lookup"><span data-stu-id="9ed22-253">`with` expression</span></span>](../operators/with-expression.md)
