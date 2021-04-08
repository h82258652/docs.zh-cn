---
title: 记录 - C# 编程指南
description: 了解记录类型及其创建方式
ms.date: 02/26/2021
helpviewer_keywords:
- records [C#]
- C# language, records
ms.openlocfilehash: 99370e398d5e607def58334b33ae20aa59e21962
ms.sourcegitcommit: 872ca41d1c26f39d0aef57cc365d09503bac780d
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/03/2021
ms.locfileid: "106288025"
---
# <a name="records-c-programming-guide"></a><span data-ttu-id="4fe67-103">记录（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="4fe67-103">Records (C# Programming Guide)</span></span>

<span data-ttu-id="4fe67-104">[记录](../../language-reference/builtin-types/record.md)是一个[类](../../language-reference/keywords/class.md)，它为使用数据模型提供特定的语法和行为。</span><span class="sxs-lookup"><span data-stu-id="4fe67-104">A [record](../../language-reference/builtin-types/record.md) is a [class](../../language-reference/keywords/class.md) that provides special syntax and behavior for working with data models.</span></span> <span data-ttu-id="4fe67-105">有关类的详细信息，请查看[类（C# 编程指南）](classes.md)。</span><span class="sxs-lookup"><span data-stu-id="4fe67-105">For information about classes, see [Classes (C# Programming Guide)](classes.md).</span></span>

## <a name="when-to-use-records"></a><span data-ttu-id="4fe67-106">何时使用记录</span><span class="sxs-lookup"><span data-stu-id="4fe67-106">When to use records</span></span>

<span data-ttu-id="4fe67-107">在下列情况下，请考虑使用记录而不是类：</span><span class="sxs-lookup"><span data-stu-id="4fe67-107">Consider using a record in place of a class in the following scenarios:</span></span>

* <span data-ttu-id="4fe67-108">你想要定义对象不可变的引用类型。</span><span class="sxs-lookup"><span data-stu-id="4fe67-108">You want to define a reference type for which objects are immutable.</span></span>
* <span data-ttu-id="4fe67-109">你想要定义依赖[值相等性](../statements-expressions-operators/equality-comparisons.md#value-equality)的数据模型。</span><span class="sxs-lookup"><span data-stu-id="4fe67-109">You want to define a data model that depends on [value equality](../statements-expressions-operators/equality-comparisons.md#value-equality).</span></span>

### <a name="immutability"></a><span data-ttu-id="4fe67-110">不可变性</span><span class="sxs-lookup"><span data-stu-id="4fe67-110">Immutability</span></span>

<span data-ttu-id="4fe67-111">不可变类型会阻止你在对象实例化后更改该对象的任何属性或字段值。</span><span class="sxs-lookup"><span data-stu-id="4fe67-111">An immutable type is one that prevents you from changing any property or field values of an object after it's instantiated.</span></span> <span data-ttu-id="4fe67-112">如果你需要一个类型是线程安全的，或者需要哈希代码在哈希表中国能保持不变，那么不可变性很有用。</span><span class="sxs-lookup"><span data-stu-id="4fe67-112">Immutability can be useful when you need a type to be thread-safe or you're depending on a hash code remaining the same in a hash table.</span></span> <span data-ttu-id="4fe67-113">记录为创建和使用不可变类型提供了简洁的语法。</span><span class="sxs-lookup"><span data-stu-id="4fe67-113">Records provide concise syntax for creating and working with immutable types.</span></span>

<span data-ttu-id="4fe67-114">不可变性并不适用于所有数据方案。</span><span class="sxs-lookup"><span data-stu-id="4fe67-114">Immutability isn't appropriate for all data scenarios.</span></span> <span data-ttu-id="4fe67-115">例如，[Entity Framework Core](/ef/core/) 不支持通过不可变实体类型进行更新。</span><span class="sxs-lookup"><span data-stu-id="4fe67-115">[Entity Framework Core](/ef/core/), for example, doesn't support updating with immutable entity types.</span></span>

### <a name="value-equality"></a><span data-ttu-id="4fe67-116">值相等性</span><span class="sxs-lookup"><span data-stu-id="4fe67-116">Value equality</span></span>

<span data-ttu-id="4fe67-117">对记录来说，值相等性表示当类型匹配且所有属性和字段值都匹配时，记录类型的两个变量相等。</span><span class="sxs-lookup"><span data-stu-id="4fe67-117">For records, value equality means that two variables of a record type are equal if the types match and all property and field values match.</span></span> <span data-ttu-id="4fe67-118">对于其他引用类型（例如类），相等性是指[引用相等性](../statements-expressions-operators/equality-comparisons.md#reference-equality)。</span><span class="sxs-lookup"><span data-stu-id="4fe67-118">For other reference types such as classes, equality means [reference equality](../statements-expressions-operators/equality-comparisons.md#reference-equality).</span></span> <span data-ttu-id="4fe67-119">也就是说，如果类的两个变量引用同一个对象，则这两个变量是相等的。</span><span class="sxs-lookup"><span data-stu-id="4fe67-119">That is, two variables of a class type are equal if they refer to the same object.</span></span> <span data-ttu-id="4fe67-120">确定两个记录实例的相等性的方法和运算符使用值相等性。</span><span class="sxs-lookup"><span data-stu-id="4fe67-120">Methods and operators that determine equality of two record instances use value equality.</span></span>

<span data-ttu-id="4fe67-121">并非所有数据模型都适合使用值相等性。</span><span class="sxs-lookup"><span data-stu-id="4fe67-121">Not all data models work well with value equality.</span></span> <span data-ttu-id="4fe67-122">例如，[Entity Framework Core](/ef/core/) 依赖引用相等性，来确保它对概念上是一个实体的实体类型只使用一个实例。</span><span class="sxs-lookup"><span data-stu-id="4fe67-122">For example, [Entity Framework Core](/ef/core/) depends on reference equality to ensure that it uses only one instance of an entity type for what is conceptually one entity.</span></span> <span data-ttu-id="4fe67-123">因此，记录类型不适合用作 Entity Framework Core 中的实体类型。</span><span class="sxs-lookup"><span data-stu-id="4fe67-123">For this reason, record types aren't appropriate for use as entity types in Entity Framework Core.</span></span>

## <a name="how-records-differ-from-classes"></a><span data-ttu-id="4fe67-124">记录与类的区别</span><span class="sxs-lookup"><span data-stu-id="4fe67-124">How records differ from classes</span></span>

<span data-ttu-id="4fe67-125">[声明](classes.md#declaring-classes)和[实例化](classes.md#creating-objects)类时使用的语法与操作记录时的相同。</span><span class="sxs-lookup"><span data-stu-id="4fe67-125">The same syntax that [declares](classes.md#declaring-classes) and [instantiates](classes.md#creating-objects) classes can be used with records.</span></span> <span data-ttu-id="4fe67-126">只需将 `record` 关键字替换为 `class` 关键字即可。</span><span class="sxs-lookup"><span data-stu-id="4fe67-126">Just substitute the `record` keyword for the `class` keyword.</span></span> <span data-ttu-id="4fe67-127">同样地，记录支持相同的表示继承关系的语法。</span><span class="sxs-lookup"><span data-stu-id="4fe67-127">Likewise, the same syntax for expressing inheritance relationships is supported by records.</span></span> <span data-ttu-id="4fe67-128">记录与类的区别如下所示：</span><span class="sxs-lookup"><span data-stu-id="4fe67-128">Records differ from classes in the following ways:</span></span>

* <span data-ttu-id="4fe67-129">可使用[位置参数](../../language-reference/builtin-types/record.md#positional-syntax-for-property-definition)创建和实例化具有不可变属性的类型。</span><span class="sxs-lookup"><span data-stu-id="4fe67-129">You can use [positional parameters](../../language-reference/builtin-types/record.md#positional-syntax-for-property-definition) to create and instantiate a type with immutable properties.</span></span>
* <span data-ttu-id="4fe67-130">在类中指示引用相等性或不相等的方法和运算符（例如 <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> 和 `==`）在记录中指示[值相等性或不相等](../../language-reference/builtin-types/record.md#value-equality)。</span><span class="sxs-lookup"><span data-stu-id="4fe67-130">The same methods and operators that indicate reference equality or inequality in classes (such as <xref:System.Object.Equals(System.Object)?displayProperty=nameWithType> and `==`), indicate [value equality or inequality](../../language-reference/builtin-types/record.md#value-equality) in records.</span></span>
* <span data-ttu-id="4fe67-131">可使用 [`with` 表达式](../../language-reference/builtin-types/record.md#nondestructive-mutation)对不可变对象创建在所选属性中具有新值的副本。</span><span class="sxs-lookup"><span data-stu-id="4fe67-131">You can use a [`with` expression](../../language-reference/builtin-types/record.md#nondestructive-mutation) to create a copy of an immutable object with new values in selected properties.</span></span>
* <span data-ttu-id="4fe67-132">记录的 `ToString` 方法会创建一个格式字符串，它显示对象的类型名称及其所有公共属性的名称和值。</span><span class="sxs-lookup"><span data-stu-id="4fe67-132">A record's `ToString` method creates a formatted string that shows an object's type name and the names and values of all its public properties.</span></span>
* <span data-ttu-id="4fe67-133">记录可[从另一个记录继承](../../language-reference/builtin-types/record.md#inheritance)。</span><span class="sxs-lookup"><span data-stu-id="4fe67-133">A record can [inherit from another record](../../language-reference/builtin-types/record.md#inheritance).</span></span> <span data-ttu-id="4fe67-134">但记录不可从类继承，类也不可从记录继承。</span><span class="sxs-lookup"><span data-stu-id="4fe67-134">A record can't inherit from a class, and a class can't inherit from a record.</span></span>

## <a name="examples"></a><span data-ttu-id="4fe67-135">示例</span><span class="sxs-lookup"><span data-stu-id="4fe67-135">Examples</span></span>

<span data-ttu-id="4fe67-136">下面的示例定义了一个公共记录，它使用位置参数来声明和实例化记录。</span><span class="sxs-lookup"><span data-stu-id="4fe67-136">The following example defines a public record that uses positional parameters to declare and instantiate a record.</span></span> <span data-ttu-id="4fe67-137">然后，它会输出类型名称和属性值：</span><span class="sxs-lookup"><span data-stu-id="4fe67-137">It then prints out the type name and property values:</span></span>

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="InstantiatePositional":::

<span data-ttu-id="4fe67-138">下面的示例演示了记录中的值相等性：</span><span class="sxs-lookup"><span data-stu-id="4fe67-138">The following example demonstrates value equality in records:</span></span>

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="Equality":::

<span data-ttu-id="4fe67-139">下面的示例演示如何使用 `with` 表达式来复制不可变对象和更改其中的一个属性：</span><span class="sxs-lookup"><span data-stu-id="4fe67-139">The following example demonstrates use of a `with` expression to copy an immutable object and change one of the properties:</span></span>

:::code language="csharp" source="../../language-reference/builtin-types/snippets/shared/RecordType.cs" id="WithExpressions":::

<span data-ttu-id="4fe67-140">有关详细信息，请查看[记录（C# 参考）](../../language-reference/builtin-types/record.md)。</span><span class="sxs-lookup"><span data-stu-id="4fe67-140">For more information, see [Records (C# reference)](../../language-reference/builtin-types/record.md).</span></span>
  
## <a name="c-language-specification"></a><span data-ttu-id="4fe67-141">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="4fe67-141">C# Language Specification</span></span>

[!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a><span data-ttu-id="4fe67-142">请参阅</span><span class="sxs-lookup"><span data-stu-id="4fe67-142">See also</span></span>

- [<span data-ttu-id="4fe67-143">类（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="4fe67-143">Classes (C# Programming Guide)</span></span>](classes.md)
- [<span data-ttu-id="4fe67-144">记录（C# 参考）</span><span class="sxs-lookup"><span data-stu-id="4fe67-144">Records (C# reference)</span></span>](../../language-reference/builtin-types/record.md)
- [<span data-ttu-id="4fe67-145">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="4fe67-145">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="4fe67-146">面向对象的编程</span><span class="sxs-lookup"><span data-stu-id="4fe67-146">Object-Oriented Programming</span></span>](../../tutorials/intro-to-csharp/object-oriented-programming.md)
- [<span data-ttu-id="4fe67-147">多态性</span><span class="sxs-lookup"><span data-stu-id="4fe67-147">Polymorphism</span></span>](polymorphism.md)
- [<span data-ttu-id="4fe67-148">标识符名称</span><span class="sxs-lookup"><span data-stu-id="4fe67-148">Identifier names</span></span>](../inside-a-program/identifier-names.md)
- [<span data-ttu-id="4fe67-149">成员</span><span class="sxs-lookup"><span data-stu-id="4fe67-149">Members</span></span>](members.md)
- [<span data-ttu-id="4fe67-150">方法</span><span class="sxs-lookup"><span data-stu-id="4fe67-150">Methods</span></span>](methods.md)
- [<span data-ttu-id="4fe67-151">构造函数</span><span class="sxs-lookup"><span data-stu-id="4fe67-151">Constructors</span></span>](constructors.md)
- [<span data-ttu-id="4fe67-152">终结器</span><span class="sxs-lookup"><span data-stu-id="4fe67-152">Finalizers</span></span>](destructors.md)
- [<span data-ttu-id="4fe67-153">对象</span><span class="sxs-lookup"><span data-stu-id="4fe67-153">Objects</span></span>](objects.md)
