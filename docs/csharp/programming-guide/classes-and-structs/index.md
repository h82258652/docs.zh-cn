---
title: 类、结构和记录 - C# 编程指南
description: 介绍如何在 C# 中使用类和结构和记录。
ms.date: 04/06/2021
helpviewer_keywords:
- structs [C#], about structs
- records [C#], about records
- classes [C#], overview
- C# language, structs
- C# language, records
- C# language, objects
- objects [C#]
- C# language, classes
ms.openlocfilehash: 69b62160699ef43343a89fe8bb3deaa2fb3523ba
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075454"
---
# <a name="classes-structs-and-records-c-programming-guide"></a><span data-ttu-id="4b47a-103">类、结构和记录（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="4b47a-103">Classes, structs, and records (C# programming guide)</span></span>

<span data-ttu-id="4b47a-104">类和结构是 .NET 通用类型系统的两种基本构造。</span><span class="sxs-lookup"><span data-stu-id="4b47a-104">Classes and structs are two of the basic constructs of the common type system in .NET.</span></span> <span data-ttu-id="4b47a-105">C# 9 添加记录，记录是一种类。</span><span class="sxs-lookup"><span data-stu-id="4b47a-105">C# 9 adds records, which are a kind of class.</span></span> <span data-ttu-id="4b47a-106">每种本质上都是一种数据结构，其中封装了同属一个逻辑单元的一组数据和行为。</span><span class="sxs-lookup"><span data-stu-id="4b47a-106">Each is essentially a data structure that encapsulates a set of data and behaviors that belong together as a logical unit.</span></span> <span data-ttu-id="4b47a-107">数据和行为是类、结构或记录的成员，包括方法、属性和事件等（本文稍后将具体列举）。</span><span class="sxs-lookup"><span data-stu-id="4b47a-107">The data and behaviors are the *members* of the class, struct, or record, and they include its methods, properties, events, and so on, as listed later in this article.</span></span>

<span data-ttu-id="4b47a-108">类、结构或记录声明类似于一张蓝图，用于在运行时创建实例或对象。</span><span class="sxs-lookup"><span data-stu-id="4b47a-108">A class, struct, or record declaration is like a blueprint that is used to create instances or objects at run time.</span></span> <span data-ttu-id="4b47a-109">如果定义名为 `Person` 的类、结构或记录，则 `Person` 是类型的名称。</span><span class="sxs-lookup"><span data-stu-id="4b47a-109">If you define a class, struct, or record named `Person`, `Person` is the name of the type.</span></span> <span data-ttu-id="4b47a-110">如果声明和初始化 `Person` 类型的变量 `p`，那么 `p` 就是所谓的 `Person` 对象或实例。</span><span class="sxs-lookup"><span data-stu-id="4b47a-110">If you declare and initialize a variable `p` of type `Person`, `p` is said to be an object or instance of `Person`.</span></span> <span data-ttu-id="4b47a-111">可以创建同一 `Person` 类型的多个实例，每个实例都可以有不同的属性和字段值。</span><span class="sxs-lookup"><span data-stu-id="4b47a-111">Multiple instances of the same `Person` type can be created, and each instance can have different values in its properties and fields.</span></span>  
  
<span data-ttu-id="4b47a-112">类或记录是引用类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-112">A class or a record is a reference type.</span></span> <span data-ttu-id="4b47a-113">创建类型的对象后，向其分配对象的变量仅保留对相应内存的引用。</span><span class="sxs-lookup"><span data-stu-id="4b47a-113">When an object of the type is created, the variable to which the object is assigned holds only a reference to that memory.</span></span> <span data-ttu-id="4b47a-114">将对象引用分配给新变量后，新变量会引用原始对象。</span><span class="sxs-lookup"><span data-stu-id="4b47a-114">When the object reference is assigned to a new variable, the new variable refers to the original object.</span></span> <span data-ttu-id="4b47a-115">通过一个变量所做的更改将反映在另一个变量中，因为它们引用相同的数据。</span><span class="sxs-lookup"><span data-stu-id="4b47a-115">Changes made through one variable are reflected in the other variable because they both refer to the same data.</span></span>
  
<span data-ttu-id="4b47a-116">结构是值类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-116">A struct is a value type.</span></span> <span data-ttu-id="4b47a-117">创建结构时，向其分配结构的变量保留结构的实际数据。</span><span class="sxs-lookup"><span data-stu-id="4b47a-117">When a struct is created, the variable to which the struct is assigned holds the struct's actual data.</span></span> <span data-ttu-id="4b47a-118">将结构分配给新变量时，会复制结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-118">When the struct is assigned to a new variable, it is copied.</span></span> <span data-ttu-id="4b47a-119">因此，新变量和原始变量包含相同数据的副本（共两个）。</span><span class="sxs-lookup"><span data-stu-id="4b47a-119">The new variable and the original variable therefore contain two separate copies of the same data.</span></span> <span data-ttu-id="4b47a-120">对一个副本所做的更改不会影响另一个副本。</span><span class="sxs-lookup"><span data-stu-id="4b47a-120">Changes made to one copy do not affect the other copy.</span></span>  
  
<span data-ttu-id="4b47a-121">一般来说，类用于对更复杂的行为或应在类对象创建后进行修改的数据建模。</span><span class="sxs-lookup"><span data-stu-id="4b47a-121">In general, classes are used to model more complex behavior, or data that is intended to be modified after a class object is created.</span></span> <span data-ttu-id="4b47a-122">结构最适用于所含大部分数据不得在结构创建后进行修改的小型数据结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-122">Structs are best suited for small data structures that contain primarily data that isn't intended to be modified after the struct is created.</span></span> <span data-ttu-id="4b47a-123">记录类型可用于所含大部分是不得在创建对象后修改的数据的大型数据结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-123">Record types are for larger data structures that contain primarily data that isn't intended to be modified after the object is created.</span></span>
  
## <a name="example"></a><span data-ttu-id="4b47a-124">示例</span><span class="sxs-lookup"><span data-stu-id="4b47a-124">Example</span></span>

 <span data-ttu-id="4b47a-125">在以下示例中，`ProgrammingGuide` 命名空间中的 `CustomClass` 有以下三个成员：实例构造函数、`Number` 属性和 `Multiply` 方法。</span><span class="sxs-lookup"><span data-stu-id="4b47a-125">In the following example, `CustomClass` in the `ProgrammingGuide` namespace has three members: an instance constructor, a property named `Number`, and a method named `Multiply`.</span></span> <span data-ttu-id="4b47a-126">`Program` 类中的 `Main` 方法创建 `CustomClass` 的实例（对象），此对象的方法和属性可使用点表示法进行访问。</span><span class="sxs-lookup"><span data-stu-id="4b47a-126">The `Main` method in the `Program` class creates an instance (object) of `CustomClass`, and the object's method and property are accessed by using dot notation.</span></span>
  
 :::code language="csharp" source="../../../../samples/snippets/csharp/programming-guide/classes-and-structs/class1.cs" id="Snippet1":::  
  
## <a name="encapsulation"></a><span data-ttu-id="4b47a-127">封装</span><span class="sxs-lookup"><span data-stu-id="4b47a-127">Encapsulation</span></span>  

 <span data-ttu-id="4b47a-128">*封装* 有时称为面向对象的编程的第一支柱或原则。</span><span class="sxs-lookup"><span data-stu-id="4b47a-128">*Encapsulation* is sometimes referred to as the first pillar or principle of object-oriented programming.</span></span> <span data-ttu-id="4b47a-129">根据封装原则，类或结构可以指定自己的每个成员对外部代码的可访问性。</span><span class="sxs-lookup"><span data-stu-id="4b47a-129">According to the principle of encapsulation, a class or struct can specify how accessible each of its members is to code outside of the class or struct.</span></span> <span data-ttu-id="4b47a-130">可以隐藏不得在类或程序集外部使用的方法和变量，以限制编码错误或恶意攻击发生的可能性。</span><span class="sxs-lookup"><span data-stu-id="4b47a-130">Methods and variables that are not intended to be used from outside of the class or assembly can be hidden to limit the potential for coding errors or malicious exploits.</span></span> <span data-ttu-id="4b47a-131">有关详细信息，请参阅[面向对象的编程](../../tutorials/intro-to-csharp/object-oriented-programming.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-131">For more information, see [Object-oriented programming](../../tutorials/intro-to-csharp/object-oriented-programming.md).</span></span>

## <a name="members"></a><span data-ttu-id="4b47a-132">成员</span><span class="sxs-lookup"><span data-stu-id="4b47a-132">Members</span></span>

<span data-ttu-id="4b47a-133">所有方法、字段、常量、属性和事件都必须在类型中进行声明；这些被称为类型的 *成员*。</span><span class="sxs-lookup"><span data-stu-id="4b47a-133">All methods, fields, constants, properties, and events must be declared within a type; these are called the *members* of the type.</span></span> <span data-ttu-id="4b47a-134">C# 没有全局变量或方法，这一点其他某些语言不同。</span><span class="sxs-lookup"><span data-stu-id="4b47a-134">In C#, there are no global variables or methods as there are in some other languages.</span></span> <span data-ttu-id="4b47a-135">即使是编程的入口点（`Main` 方法），也必须在类或结构中声明（对于[顶级语句](../main-and-command-args/top-level-statements.md)的情况，是隐式声明）。</span><span class="sxs-lookup"><span data-stu-id="4b47a-135">Even a program's entry point, the `Main` method, must be declared within a class or struct (implicitly in the case of [top-level statements](../main-and-command-args/top-level-statements.md)).</span></span>

<span data-ttu-id="4b47a-136">下面列出了所有可以在类、结构或记录中声明的各种成员。</span><span class="sxs-lookup"><span data-stu-id="4b47a-136">The following list includes all the various kinds of members that may be declared in a class, struct, or record.</span></span>
  
- [<span data-ttu-id="4b47a-137">字段</span><span class="sxs-lookup"><span data-stu-id="4b47a-137">Fields</span></span>](./fields.md)  
- [<span data-ttu-id="4b47a-138">常量</span><span class="sxs-lookup"><span data-stu-id="4b47a-138">Constants</span></span>](./constants.md)
- [<span data-ttu-id="4b47a-139">属性</span><span class="sxs-lookup"><span data-stu-id="4b47a-139">Properties</span></span>](./properties.md)
- [<span data-ttu-id="4b47a-140">方法</span><span class="sxs-lookup"><span data-stu-id="4b47a-140">Methods</span></span>](./methods.md)
- [<span data-ttu-id="4b47a-141">构造函数</span><span class="sxs-lookup"><span data-stu-id="4b47a-141">Constructors</span></span>](./constructors.md)
- [<span data-ttu-id="4b47a-142">事件</span><span class="sxs-lookup"><span data-stu-id="4b47a-142">Events</span></span>](../events/index.md)
- [<span data-ttu-id="4b47a-143">终结器</span><span class="sxs-lookup"><span data-stu-id="4b47a-143">Finalizers</span></span>](./destructors.md)
- [<span data-ttu-id="4b47a-144">索引器</span><span class="sxs-lookup"><span data-stu-id="4b47a-144">Indexers</span></span>](../indexers/index.md)
- [<span data-ttu-id="4b47a-145">运算符</span><span class="sxs-lookup"><span data-stu-id="4b47a-145">Operators</span></span>](../../language-reference/operators/index.md)
- [<span data-ttu-id="4b47a-146">嵌套类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-146">Nested Types</span></span>](./nested-types.md)
  
## <a name="accessibility"></a><span data-ttu-id="4b47a-147">可访问性</span><span class="sxs-lookup"><span data-stu-id="4b47a-147">Accessibility</span></span>  

 <span data-ttu-id="4b47a-148">一些方法和属性可供类或结构外部的代码（称为“*客户端代码*”）调用或访问。</span><span class="sxs-lookup"><span data-stu-id="4b47a-148">Some methods and properties are meant to be called or accessed from code outside a class or struct, known as *client code*.</span></span> <span data-ttu-id="4b47a-149">另一些方法和属性只能在类或结构本身中使用。</span><span class="sxs-lookup"><span data-stu-id="4b47a-149">Other methods and properties might be only for use in the class or struct itself.</span></span> <span data-ttu-id="4b47a-150">请务必限制代码的可访问性，仅供预期的客户端代码进行访问。</span><span class="sxs-lookup"><span data-stu-id="4b47a-150">It is important to limit the accessibility of your code so that only the intended client code can reach it.</span></span> <span data-ttu-id="4b47a-151">需要使用以下访问修饰符指定类型及其成员对客户端代码的可访问性：</span><span class="sxs-lookup"><span data-stu-id="4b47a-151">You specify how accessible your types and their members are to client code by using the following access modifiers:</span></span>

- [<span data-ttu-id="4b47a-152">public</span><span class="sxs-lookup"><span data-stu-id="4b47a-152">public</span></span>](../../language-reference/keywords/public.md)
- [<span data-ttu-id="4b47a-153">受保护</span><span class="sxs-lookup"><span data-stu-id="4b47a-153">protected</span></span>](../../language-reference/keywords/protected.md)
- [<span data-ttu-id="4b47a-154">internal</span><span class="sxs-lookup"><span data-stu-id="4b47a-154">internal</span></span>](../../language-reference/keywords/internal.md)
- [<span data-ttu-id="4b47a-155">protected internal</span><span class="sxs-lookup"><span data-stu-id="4b47a-155">protected internal</span></span>](../../language-reference/keywords/protected-internal.md)
- [<span data-ttu-id="4b47a-156">private</span><span class="sxs-lookup"><span data-stu-id="4b47a-156">private</span></span>](../../language-reference/keywords/private.md)
- <span data-ttu-id="4b47a-157">[专用受保护](../../language-reference/keywords/private-protected.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-157">[private protected](../../language-reference/keywords/private-protected.md).</span></span>

<span data-ttu-id="4b47a-158">可访问性的默认值为 `private`。</span><span class="sxs-lookup"><span data-stu-id="4b47a-158">The default accessibility is `private`.</span></span> <span data-ttu-id="4b47a-159">有关详细信息，请参阅[访问修饰符](./access-modifiers.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-159">For more information, see [Access Modifiers](./access-modifiers.md).</span></span>  
  
## <a name="inheritance"></a><span data-ttu-id="4b47a-160">继承</span><span class="sxs-lookup"><span data-stu-id="4b47a-160">Inheritance</span></span>  

<span data-ttu-id="4b47a-161">类（而非结构）支持继承的概念。</span><span class="sxs-lookup"><span data-stu-id="4b47a-161">Classes (but not structs) support the concept of inheritance.</span></span> <span data-ttu-id="4b47a-162">派生自另一个类（基类）的类自动包含基类的所有公共、受保护和内部成员（其构造函数和终结器除外）。</span><span class="sxs-lookup"><span data-stu-id="4b47a-162">A class that derives from another class (the *base class*) automatically contains all the public, protected, and internal members of the base class except its constructors and finalizers.</span></span> <span data-ttu-id="4b47a-163">有关详细信息，请参阅[继承](./inheritance.md)和[多态性](./polymorphism.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-163">For more information, see [Inheritance](./inheritance.md) and [Polymorphism](./polymorphism.md).</span></span>
  
<span data-ttu-id="4b47a-164">可以将类声明为 [abstract](../../language-reference/keywords/abstract.md)，即一个或多个方法没有实现代码。</span><span class="sxs-lookup"><span data-stu-id="4b47a-164">Classes may be declared as [abstract](../../language-reference/keywords/abstract.md), which means that one or more of their methods have no implementation.</span></span> <span data-ttu-id="4b47a-165">尽管抽象类无法直接实例化，但可以作为提供缺少实现代码的其他类的基类。</span><span class="sxs-lookup"><span data-stu-id="4b47a-165">Although abstract classes cannot be instantiated directly, they can serve as base classes for other classes that provide the missing implementation.</span></span> <span data-ttu-id="4b47a-166">类还可以声明为 [sealed](../../language-reference/keywords/sealed.md)，以阻止其他类继承。</span><span class="sxs-lookup"><span data-stu-id="4b47a-166">Classes can also be declared as [sealed](../../language-reference/keywords/sealed.md) to prevent other classes from inheriting from them.</span></span> <span data-ttu-id="4b47a-167">有关详细信息，请参阅[抽象类、密封类及类成员](./abstract-and-sealed-classes-and-class-members.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-167">For more information, see [Abstract and sealed classes and class members](./abstract-and-sealed-classes-and-class-members.md).</span></span>
  
## <a name="interfaces"></a><span data-ttu-id="4b47a-168">界面</span><span class="sxs-lookup"><span data-stu-id="4b47a-168">Interfaces</span></span>  

<span data-ttu-id="4b47a-169">类、结构和记录可以继承多个接口。</span><span class="sxs-lookup"><span data-stu-id="4b47a-169">Classes, structs, and records can inherit multiple interfaces.</span></span> <span data-ttu-id="4b47a-170">继承自接口意味着类型实现接口中定义的所有方法。</span><span class="sxs-lookup"><span data-stu-id="4b47a-170">To inherit from an interface means that the type implements all the methods defined in the interface.</span></span> <span data-ttu-id="4b47a-171">有关详细信息，请参阅[接口](../interfaces/index.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-171">For more information, see [Interfaces](../interfaces/index.md).</span></span>  
  
## <a name="generic-types"></a><span data-ttu-id="4b47a-172">泛型类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-172">Generic Types</span></span>  

 <span data-ttu-id="4b47a-173">类、结构和记录可以使用一个或多个类型参数进行定义。</span><span class="sxs-lookup"><span data-stu-id="4b47a-173">Classes, structs, and records can be defined with one or more type parameters.</span></span> <span data-ttu-id="4b47a-174">客户端代码在创建类型实例时提供类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-174">Client code supplies the type when it creates an instance of the type.</span></span> <span data-ttu-id="4b47a-175">例如，<xref:System.Collections.Generic> 命名空间中的 <xref:System.Collections.Generic.List%601> 类就是用一个类型参数进行定义的。</span><span class="sxs-lookup"><span data-stu-id="4b47a-175">For example The <xref:System.Collections.Generic.List%601> class in the <xref:System.Collections.Generic> namespace is defined with one type parameter.</span></span> <span data-ttu-id="4b47a-176">客户端代码创建 `List<string>` 或 `List<int>` 的实例来指定列表将包含的类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-176">Client code creates an instance of a `List<string>` or `List<int>` to specify the type that the list will hold.</span></span> <span data-ttu-id="4b47a-177">有关详细信息，请参阅[泛型](../generics/index.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-177">For more information, see [Generics](../generics/index.md).</span></span>  
  
## <a name="static-types"></a><span data-ttu-id="4b47a-178">静态类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-178">Static Types</span></span>  

<span data-ttu-id="4b47a-179">类（而非结构或记录）可以声明为[静态](../../language-reference/keywords/static.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-179">Classes (but not structs or records) can be declared as [static](../../language-reference/keywords/static.md).</span></span> <span data-ttu-id="4b47a-180">静态类只能包含静态成员，不能使用 `new` 关键字进行实例化。</span><span class="sxs-lookup"><span data-stu-id="4b47a-180">A static class can contain only static members and can't be instantiated with the `new` keyword.</span></span> <span data-ttu-id="4b47a-181">在程序加载时，类的一个副本会加载到内存中，而其成员则可通过类名进行访问。</span><span class="sxs-lookup"><span data-stu-id="4b47a-181">One copy of the class is loaded into memory when the program loads, and its members are accessed through the class name.</span></span> <span data-ttu-id="4b47a-182">类、结构和记录可以包含静态成员。</span><span class="sxs-lookup"><span data-stu-id="4b47a-182">Classes, structs, and records can contain static members.</span></span> <span data-ttu-id="4b47a-183">有关详细信息，请参阅[静态类和静态类成员](./static-classes-and-static-class-members.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-183">For more information, see [Static classes and static class members](./static-classes-and-static-class-members.md).</span></span>
  
## <a name="nested-types"></a><span data-ttu-id="4b47a-184">嵌套类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-184">Nested Types</span></span>

<span data-ttu-id="4b47a-185">类、结构和记录可以嵌套在其他类、结构和记录中。</span><span class="sxs-lookup"><span data-stu-id="4b47a-185">A class, struct, or record can be nested within another class, struct, or record.</span></span> <span data-ttu-id="4b47a-186">有关详细信息，请参阅[嵌套类型](./nested-types.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-186">For more information, see [Nested Types](./nested-types.md).</span></span>
  
## <a name="partial-types"></a><span data-ttu-id="4b47a-187">分部类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-187">Partial Types</span></span>  

<span data-ttu-id="4b47a-188">可以在一个代码文件中定义类、结构或方法的一部分，并在其他代码文件中定义另一部分。</span><span class="sxs-lookup"><span data-stu-id="4b47a-188">You can define part of a class, struct or method in one code file and another part in a separate code file.</span></span> <span data-ttu-id="4b47a-189">有关详细信息，请参阅[分部类和方法](./partial-classes-and-methods.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-189">For more information, see [Partial Classes and methods](./partial-classes-and-methods.md).</span></span>
  
## <a name="object-initializers"></a><span data-ttu-id="4b47a-190">对象初始值设定项</span><span class="sxs-lookup"><span data-stu-id="4b47a-190">Object Initializers</span></span>  

<span data-ttu-id="4b47a-191">无需显式调用构造函数，即可实例化和初始化类或结构对象和对象集合。</span><span class="sxs-lookup"><span data-stu-id="4b47a-191">You can instantiate and initialize class or struct objects, and collections of objects, without explicitly calling their constructor.</span></span> <span data-ttu-id="4b47a-192">有关详细信息，请参阅[对象和集合初始值设定项](./object-and-collection-initializers.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-192">For more information, see [Object and collection initializers](./object-and-collection-initializers.md).</span></span>  
  
## <a name="anonymous-types"></a><span data-ttu-id="4b47a-193">匿名类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-193">Anonymous Types</span></span>  

<span data-ttu-id="4b47a-194">如果不方便或没有必要创建已命名的类（例如，使用无需保留或传递给其他方法的数据结构填充列表时），可以使用匿名类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-194">In situations where it is not convenient or necessary to create a named class, for example when you are populating a list with data structures that you do not have to persist or pass to another method, you use anonymous types.</span></span> <span data-ttu-id="4b47a-195">有关详细信息，请参阅[匿名类型](./anonymous-types.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-195">For more information, see [Anonymous types](./anonymous-types.md).</span></span>
  
## <a name="extension-methods"></a><span data-ttu-id="4b47a-196">扩展方法</span><span class="sxs-lookup"><span data-stu-id="4b47a-196">Extension Methods</span></span>  

<span data-ttu-id="4b47a-197">可以单独创建类型（其方法可以调用，就像它们属于原始类型一样）来“扩展”类，而无需创建派生类。</span><span class="sxs-lookup"><span data-stu-id="4b47a-197">You can "extend" a class without creating a derived class by creating a separate type whose methods can be called as if they belonged to the original type.</span></span> <span data-ttu-id="4b47a-198">有关详细信息，请参阅[扩展方法](./extension-methods.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-198">For more information, see [Extension methods](./extension-methods.md).</span></span>
  
## <a name="implicitly-typed-local-variables"></a><span data-ttu-id="4b47a-199">隐式类型的局部变量</span><span class="sxs-lookup"><span data-stu-id="4b47a-199">Implicitly Typed Local Variables</span></span>  

<span data-ttu-id="4b47a-200">在类或结构方法中，可以使用隐式类型指示编译器在编译时确定变量类型。</span><span class="sxs-lookup"><span data-stu-id="4b47a-200">Within a class or struct method, you can use implicit typing to instruct the compiler to determine a variable's type at compile time.</span></span> <span data-ttu-id="4b47a-201">有关详细信息，请参阅[隐式类型局部变量](./implicitly-typed-local-variables.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-201">For more information, see [Implicitly Typed Local Variables](./implicitly-typed-local-variables.md).</span></span>

## <a name="records"></a><span data-ttu-id="4b47a-202">记录</span><span class="sxs-lookup"><span data-stu-id="4b47a-202">Records</span></span>

<span data-ttu-id="4b47a-203">C# 9 引入了 `record` 类型，可创建此引用类型而不创建类或结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-203">C# 9 introduces the `record` type, a reference type that you can create instead of a class or a struct.</span></span> <span data-ttu-id="4b47a-204">记录是带有内置行为的类，用于将数据封装在不可变类型中。</span><span class="sxs-lookup"><span data-stu-id="4b47a-204">Records are classes with built-in behavior for encapsulating data in immutable types.</span></span> <span data-ttu-id="4b47a-205">记录提供以下功能：</span><span class="sxs-lookup"><span data-stu-id="4b47a-205">A record provides the following features:</span></span>

* <span data-ttu-id="4b47a-206">用于创建具有不可变属性的引用类型的简明语法。</span><span class="sxs-lookup"><span data-stu-id="4b47a-206">Concise syntax for creating a reference type with immutable properties.</span></span>

* <span data-ttu-id="4b47a-207">值相等性。</span><span class="sxs-lookup"><span data-stu-id="4b47a-207">Value equality.</span></span>

  <span data-ttu-id="4b47a-208">两个记录类型的变量在它们的类型和值都相同时，它们是相等的。</span><span class="sxs-lookup"><span data-stu-id="4b47a-208">Two variables of a record type are equal if the record type definitions are identical, and if for every field, the values in both records are equal.</span></span> <span data-ttu-id="4b47a-209">记录与类不同，类使用引用相等性，即：如果两个类类型的变量引用相同的对象，则它们是相等的。</span><span class="sxs-lookup"><span data-stu-id="4b47a-209">This differs from classes, which use reference equality: two variables of a class type are equal if they refer to the same object.</span></span>

* <span data-ttu-id="4b47a-210">非破坏性变化的简明语法。</span><span class="sxs-lookup"><span data-stu-id="4b47a-210">Concise syntax for nondestructive mutation.</span></span>

  <span data-ttu-id="4b47a-211">使用 `with` 表达式，可以创建作为现有实例副本的新记录实例，但更改了指定的属性值。</span><span class="sxs-lookup"><span data-stu-id="4b47a-211">A `with` expression lets you create a new record instance that is a copy of an existing instance but with specified property values changed.</span></span>

* <span data-ttu-id="4b47a-212">显示的内置格式设置。</span><span class="sxs-lookup"><span data-stu-id="4b47a-212">Built-in formatting for display.</span></span>

  <span data-ttu-id="4b47a-213">`ToString` 方法输出记录类型名称以及公共属性的名称和值。</span><span class="sxs-lookup"><span data-stu-id="4b47a-213">The `ToString` method prints out the record type name and the names and values of public properties.</span></span>

* <span data-ttu-id="4b47a-214">支持继承层次结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-214">Support for inheritance hierarchies.</span></span>

  <span data-ttu-id="4b47a-215">支持继承，因为记录的本质是类，而不是结构。</span><span class="sxs-lookup"><span data-stu-id="4b47a-215">Inheritance is supported because a record is a class under the covers, not a struct.</span></span>

<span data-ttu-id="4b47a-216">有关详细信息，请参阅[记录](../../language-reference/builtin-types/record.md)。</span><span class="sxs-lookup"><span data-stu-id="4b47a-216">For more information, see [Records](../../language-reference/builtin-types/record.md).</span></span>

## <a name="c-language-specification"></a><span data-ttu-id="4b47a-217">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="4b47a-217">C# Language Specification</span></span>  

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]  
  
## <a name="see-also"></a><span data-ttu-id="4b47a-218">请参阅</span><span class="sxs-lookup"><span data-stu-id="4b47a-218">See also</span></span>

- [<span data-ttu-id="4b47a-219">类</span><span class="sxs-lookup"><span data-stu-id="4b47a-219">Classes</span></span>](./classes.md)
- [<span data-ttu-id="4b47a-220">对象</span><span class="sxs-lookup"><span data-stu-id="4b47a-220">Objects</span></span>](./objects.md)
- [<span data-ttu-id="4b47a-221">结构类型</span><span class="sxs-lookup"><span data-stu-id="4b47a-221">Structure types</span></span>](../../language-reference/builtin-types/struct.md)
- [<span data-ttu-id="4b47a-222">记录</span><span class="sxs-lookup"><span data-stu-id="4b47a-222">Records</span></span>](../../language-reference/builtin-types/record.md)  
- [<span data-ttu-id="4b47a-223">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="4b47a-223">C# Programming Guide</span></span>](../index.md)
