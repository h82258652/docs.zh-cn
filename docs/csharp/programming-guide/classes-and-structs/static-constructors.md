---
title: 静态构造函数 - C# 编程指南
description: C# 中的静态构造函数初始化静态数据或执行仅需执行一次的操作。 它在创建第一个实例或者引用静态成员之前运行。
ms.date: 03/30/2021
helpviewer_keywords:
- static constructors [C#]
- constructors [C#], static
ms.openlocfilehash: f7ab97c3723c04b9d442daabb85f8d16967eb0e4
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122997"
---
# <a name="static-constructors-c-programming-guide"></a><span data-ttu-id="6e6bf-104">静态构造函数（C# 编程指南）</span><span class="sxs-lookup"><span data-stu-id="6e6bf-104">Static Constructors (C# Programming Guide)</span></span>

<span data-ttu-id="6e6bf-105">静态构造函数用于初始化任何[静态](../../language-reference/keywords/static.md)数据，或执行仅需执行一次的特定操作。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-105">A static constructor is used to initialize any [static](../../language-reference/keywords/static.md) data, or to perform a particular action that needs to be performed only once.</span></span> <span data-ttu-id="6e6bf-106">将在创建第一个实例或引用任何静态成员之前自动调用静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-106">It is called automatically before the first instance is created or any static members are referenced.</span></span>  
  
 [!code-csharp[SimpleClass#1](snippets/static-constructors/Program.cs#1)]

## <a name="remarks"></a><span data-ttu-id="6e6bf-107">备注</span><span class="sxs-lookup"><span data-stu-id="6e6bf-107">Remarks</span></span>

<span data-ttu-id="6e6bf-108">静态构造函数具有以下属性：</span><span class="sxs-lookup"><span data-stu-id="6e6bf-108">Static constructors have the following properties:</span></span>  
  
- <span data-ttu-id="6e6bf-109">静态构造函数不使用访问修饰符或不具有参数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-109">A static constructor doesn't take access modifiers or have parameters.</span></span>  

- <span data-ttu-id="6e6bf-110">类或结构只能有一个静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-110">A class or struct can only have one static constructor.</span></span>

- <span data-ttu-id="6e6bf-111">静态构造函数不能继承或重载。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-111">Static constructors cannot be inherited or overloaded.</span></span>

- <span data-ttu-id="6e6bf-112">静态构造函数不能直接调用，并且仅应由公共语言运行时 (CLR) 调用。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-112">A static constructor cannot be called directly and is only meant to be called by the common language runtime (CLR).</span></span> <span data-ttu-id="6e6bf-113">可以自动调用它们。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-113">It is invoked automatically.</span></span>

- <span data-ttu-id="6e6bf-114">用户无法控制在程序中执行静态构造函数的时间。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-114">The user has no control on when the static constructor is executed in the program.</span></span>
  
- <span data-ttu-id="6e6bf-115">自动调用静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-115">A static constructor is called automatically.</span></span> <span data-ttu-id="6e6bf-116">将在创建第一个实例或引用任何静态成员之前初始化[类](../../language-reference/keywords/class.md)。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-116">It initializes the [class](../../language-reference/keywords/class.md) before the first instance is created or any static members are referenced.</span></span> <span data-ttu-id="6e6bf-117">静态构造函数在实例构造函数之前运行。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-117">A static constructor runs before an instance constructor.</span></span> <span data-ttu-id="6e6bf-118">调用（而不是分配）分配给事件或委托的静态方法时，将调用类型的静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-118">A type's static constructor is called when a static method assigned to an event or a delegate is invoked and not when it is assigned.</span></span> <span data-ttu-id="6e6bf-119">如果静态构造函数类中存在静态字段变量初始值设定项，它们将以在类声明中显示的文本顺序执行。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-119">If static field variable initializers are present in the class of the static constructor, they're executed in the textual order in which they appear in the class declaration.</span></span> <span data-ttu-id="6e6bf-120">初始值设定项紧接着执行静态构造函数之前运行。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-120">The initializers run immediately prior to the execution of the static constructor.</span></span>

- <span data-ttu-id="6e6bf-121">如果未提供静态构造函数来初始化静态字段，会将所有静态字段初始化为其默认值，如 [C# 类型的默认值](../../language-reference/builtin-types/default-values.md)中所列。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-121">If you don't provide a static constructor to initialize static fields, all static fields are initialized to their default value as listed in [Default values of C# types](../../language-reference/builtin-types/default-values.md).</span></span>
  
- <span data-ttu-id="6e6bf-122">如果静态构造函数引发异常，运行时将不会再次调用该函数，并且类型在应用程序域的生存期内将保持未初始化。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-122">If a static constructor throws an exception, the runtime doesn't invoke it a second time, and the type will remain uninitialized for the lifetime of the application domain.</span></span> <span data-ttu-id="6e6bf-123">大多数情况下，当静态构造函数无法实例化一个类型时，或者当静态构造函数中发生未经处理的异常时，将引发 <xref:System.TypeInitializationException> 异常。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-123">Most commonly, a <xref:System.TypeInitializationException> exception is thrown when a static constructor is unable to instantiate a type or for an unhandled exception occurring within a static constructor.</span></span> <span data-ttu-id="6e6bf-124">对于未在源代码中显式定义的静态构造函数，故障排除可能需要检查中间语言 (IL) 代码。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-124">For static constructors that aren't explicitly defined in source code, troubleshooting may require inspection of the intermediate language (IL) code.</span></span>

- <span data-ttu-id="6e6bf-125">静态构造函数的存在将防止添加 <xref:System.Reflection.TypeAttributes.BeforeFieldInit> 类型属性。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-125">The presence of a static constructor prevents the addition of the <xref:System.Reflection.TypeAttributes.BeforeFieldInit> type attribute.</span></span> <span data-ttu-id="6e6bf-126">这将限制运行时优化。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-126">This limits runtime optimization.</span></span>

- <span data-ttu-id="6e6bf-127">声明为 `static readonly` 的字段可能仅被分配为其声明的一部分或在静态构造函数中。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-127">A field declared as `static readonly` may only be assigned as part of its declaration or in a static constructor.</span></span> <span data-ttu-id="6e6bf-128">如果不需要显式静态构造函数，请在声明时初始化静态字段，而不是通过静态构造函数，以实现更好的运行时优化。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-128">When an explicit static constructor isn't required, initialize static fields at declaration rather than through a static constructor for better runtime optimization.</span></span>

- <span data-ttu-id="6e6bf-129">运行时在单个应用程序域中多次调用静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-129">The runtime calls a static constructor no more than once in a single application domain.</span></span> <span data-ttu-id="6e6bf-130">该调用是基于特定类型的类在锁定区域中进行的。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-130">That call is made in a locked region based on the specific type of the class.</span></span> <span data-ttu-id="6e6bf-131">静态构造函数的主体中不需要其他锁定机制。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-131">No additional locking mechanisms are needed in the body of a static constructor.</span></span> <span data-ttu-id="6e6bf-132">若要避免死锁的风险，请勿阻止静态构造函数和初始值设定项中的当前线程。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-132">To avoid the risk of deadlocks, don't block the current thread in static constructors and initializers.</span></span> <span data-ttu-id="6e6bf-133">例如，不要等待任务、线程、等待句柄或事件，不要获取锁定，也不要执行阻止并行操作，如并行循环、`Parallel.Invoke` 和并行 LINQ 查询。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-133">For example, don't wait on tasks, threads, wait handles or events, don't acquire locks, and don't execute blocking parallel operations such as parallel loops, `Parallel.Invoke` and Parallel LINQ queries.</span></span>

> [!Note]
> <span data-ttu-id="6e6bf-134">尽管不可直接访问，但应记录显式静态构造函数的存在，以帮助故障排除初始化异常。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-134">Though not directly accessible, the presence of an explicit static constructor should be documented to assist with troubleshooting initialization exceptions.</span></span>

### <a name="usage"></a><span data-ttu-id="6e6bf-135">用法</span><span class="sxs-lookup"><span data-stu-id="6e6bf-135">Usage</span></span>

- <span data-ttu-id="6e6bf-136">静态构造函数的一种典型用法是在类使用日志文件且将构造函数用于将条目写入到此文件中时使用。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-136">A typical use of static constructors is when the class is using a log file and the constructor is used to write entries to this file.</span></span>  
- <span data-ttu-id="6e6bf-137">静态构造函数对于创建非托管代码的包装类也非常有用，这种情况下构造函数可调用 `LoadLibrary` 方法。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-137">Static constructors are also useful when creating wrapper classes for unmanaged code, when the constructor can call the `LoadLibrary` method.</span></span>  

- <span data-ttu-id="6e6bf-138">也可在静态构造函数中轻松地对无法在编译时通过类型参数约束检查的类型参数强制执行运行时检查。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-138">Static constructors are also a convenient place to enforce run-time checks on the type parameter that cannot be checked at compile time via type-parameter constraints.</span></span>

## <a name="example"></a><span data-ttu-id="6e6bf-139">示例</span><span class="sxs-lookup"><span data-stu-id="6e6bf-139">Example</span></span>

 <span data-ttu-id="6e6bf-140">在此示例中，类 `Bus` 具有静态构造函数。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-140">In this example, class `Bus` has a static constructor.</span></span> <span data-ttu-id="6e6bf-141">创建 `Bus` 的第一个实例 (`bus1`) 时，将调用该静态构造函数，以便初始化类。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-141">When the first instance of `Bus` is created (`bus1`), the static constructor is invoked to initialize the class.</span></span> <span data-ttu-id="6e6bf-142">示例输出验证即使创建了两个 `Bus` 的实例，静态构造函数也仅运行一次，并且在实例构造函数运行前运行。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-142">The sample output verifies that the static constructor runs only one time, even though two instances of `Bus` are created, and that it runs before the instance constructor runs.</span></span>  
  
 [!code-csharp[BusSample#2](snippets/static-constructors/Program.cs#2)]

## <a name="c-language-specification"></a><span data-ttu-id="6e6bf-143">C# 语言规范</span><span class="sxs-lookup"><span data-stu-id="6e6bf-143">C# language specification</span></span>

<span data-ttu-id="6e6bf-144">有关详细信息，请参阅 [C# 语言规范](~/_csharplang/spec/introduction.md)中的[静态构造函数](~/_csharplang/spec/classes.md#static-constructors)部分。</span><span class="sxs-lookup"><span data-stu-id="6e6bf-144">For more information, see the [Static constructors](~/_csharplang/spec/classes.md#static-constructors) section of the [C# language specification](~/_csharplang/spec/introduction.md).</span></span>
  
## <a name="see-also"></a><span data-ttu-id="6e6bf-145">请参阅</span><span class="sxs-lookup"><span data-stu-id="6e6bf-145">See also</span></span>

- [<span data-ttu-id="6e6bf-146">C# 编程指南</span><span class="sxs-lookup"><span data-stu-id="6e6bf-146">C# Programming Guide</span></span>](../index.md)
- [<span data-ttu-id="6e6bf-147">类和结构</span><span class="sxs-lookup"><span data-stu-id="6e6bf-147">Classes and Structs</span></span>](./index.md)
- [<span data-ttu-id="6e6bf-148">构造函数</span><span class="sxs-lookup"><span data-stu-id="6e6bf-148">Constructors</span></span>](./constructors.md)
- [<span data-ttu-id="6e6bf-149">静态类和静态类成员</span><span class="sxs-lookup"><span data-stu-id="6e6bf-149">Static Classes and Static Class Members</span></span>](./static-classes-and-static-class-members.md)
- [<span data-ttu-id="6e6bf-150">终结器</span><span class="sxs-lookup"><span data-stu-id="6e6bf-150">Finalizers</span></span>](./destructors.md)
- [<span data-ttu-id="6e6bf-151">构造函数设计准则</span><span class="sxs-lookup"><span data-stu-id="6e6bf-151">Constructor Design Guidelines</span></span>](../../../standard/design-guidelines/constructor.md#type-constructor-guidelines)
- [<span data-ttu-id="6e6bf-152">安全警告 - CA2121：静态构造函数应为私有</span><span class="sxs-lookup"><span data-stu-id="6e6bf-152">Security Warning - CA2121: Static constructors should be private</span></span>](/visualstudio/code-quality/ca2121-static-constructors-should-be-private)
