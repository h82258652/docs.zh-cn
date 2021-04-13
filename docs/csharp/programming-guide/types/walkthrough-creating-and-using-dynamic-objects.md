---
title: 演练：创建并使用动态对象（C# 和 Visual Basic）
description: 在本演练中，了解如何创建和使用动态对象。 创建自定义动态对象和使用“IronPython”库的项目。
ms.date: 03/24/2021
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dynamic objects [Visual Basic]
- dynamic objects
- dynamic objects [C#]
ms.openlocfilehash: 3b342f23a2974affdcd7a1e91093aaef504afb63
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105111018"
---
# <a name="walkthrough-creating-and-using-dynamic-objects-c-and-visual-basic"></a><span data-ttu-id="05176-104">演练：创建并使用动态对象（C# 和 Visual Basic）</span><span class="sxs-lookup"><span data-stu-id="05176-104">Walkthrough: Creating and Using Dynamic Objects (C# and Visual Basic)</span></span>

<span data-ttu-id="05176-105">动态对象会在运行时（而非编译时）公开属性和方法等成员。</span><span class="sxs-lookup"><span data-stu-id="05176-105">Dynamic objects expose members such as properties and methods at run time, instead of at compile time.</span></span> <span data-ttu-id="05176-106">这使你能够创建对象以处理与静态类型或格式不匹配的结构。</span><span class="sxs-lookup"><span data-stu-id="05176-106">This enables you to create objects to work with structures that do not match a static type or format.</span></span> <span data-ttu-id="05176-107">例如，可以使用动态对象来引用 HTML 文档对象模型 (DOM)，该模型包含有效 HTML 标记元素和特性的任意组合。</span><span class="sxs-lookup"><span data-stu-id="05176-107">For example, you can use a dynamic object to reference the HTML Document Object Model (DOM), which can contain any combination of valid HTML markup elements and attributes.</span></span> <span data-ttu-id="05176-108">由于每个 HTML 文档都是唯一的，因此在运行时将确定特定 HTML 文档的成员。</span><span class="sxs-lookup"><span data-stu-id="05176-108">Because each HTML document is unique, the members for a particular HTML document are determined at run time.</span></span> <span data-ttu-id="05176-109">引用 HTML 元素的特性的常用方法是，将该特性的名称传递给该元素的 `GetProperty` 方法。</span><span class="sxs-lookup"><span data-stu-id="05176-109">A common method to reference an attribute of an HTML element is to pass the name of the attribute to the `GetProperty` method of the element.</span></span> <span data-ttu-id="05176-110">若要引用 HTML 元素 `<div id="Div1">` 的 `id` 特性，首先获取对 `<div>` 元素的引用，然后使用 `divElement.GetProperty("id")`。</span><span class="sxs-lookup"><span data-stu-id="05176-110">To reference the `id` attribute of the HTML element `<div id="Div1">`, you first obtain a reference to the `<div>` element, and then use `divElement.GetProperty("id")`.</span></span> <span data-ttu-id="05176-111">如果使用动态对象，则可以将 `id` 特性引用为 `divElement.id`。</span><span class="sxs-lookup"><span data-stu-id="05176-111">If you use a dynamic object, you can reference the `id` attribute as `divElement.id`.</span></span>

 <span data-ttu-id="05176-112">动态对象还提供对 IronPython 和 IronRuby 等动态语言的便捷访问。</span><span class="sxs-lookup"><span data-stu-id="05176-112">Dynamic objects also provide convenient access to dynamic languages such as IronPython and IronRuby.</span></span> <span data-ttu-id="05176-113">可以使用动态对象来引用在运行时解释的动态脚本。</span><span class="sxs-lookup"><span data-stu-id="05176-113">You can use a dynamic object to refer to a dynamic script that is interpreted at run time.</span></span>

 <span data-ttu-id="05176-114">使用晚期绑定引用动态对象。</span><span class="sxs-lookup"><span data-stu-id="05176-114">You reference a dynamic object by using late binding.</span></span> <span data-ttu-id="05176-115">在 C# 中，将晚期绑定对象的类型指定为 `dynamic`。</span><span class="sxs-lookup"><span data-stu-id="05176-115">In C#, you specify the type of a late-bound object as `dynamic`.</span></span> <span data-ttu-id="05176-116">在 Visual Basic 中，将晚期绑定对象的类型指定为 `Object`。</span><span class="sxs-lookup"><span data-stu-id="05176-116">In Visual Basic, you specify the type of a late-bound object as `Object`.</span></span> <span data-ttu-id="05176-117">有关详细信息，请参阅[动态](../../language-reference/builtin-types/reference-types.md)和[早期绑定和晚期绑定](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)。</span><span class="sxs-lookup"><span data-stu-id="05176-117">For more information, see [dynamic](../../language-reference/builtin-types/reference-types.md) and [Early and Late Binding](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md).</span></span>

 <span data-ttu-id="05176-118">可以使用 <xref:System.Dynamic?displayProperty=nameWithType> 命名空间中的类来创建自定义动态对象。</span><span class="sxs-lookup"><span data-stu-id="05176-118">You can create custom dynamic objects by using the classes in the <xref:System.Dynamic?displayProperty=nameWithType> namespace.</span></span> <span data-ttu-id="05176-119">例如，可以创建 <xref:System.Dynamic.ExpandoObject> 并在运行时指定该对象的成员。</span><span class="sxs-lookup"><span data-stu-id="05176-119">For example, you can create an <xref:System.Dynamic.ExpandoObject> and specify the members of that object at run time.</span></span> <span data-ttu-id="05176-120">还可以创建继承 <xref:System.Dynamic.DynamicObject> 类的自己的类型。</span><span class="sxs-lookup"><span data-stu-id="05176-120">You can also create your own type that inherits the <xref:System.Dynamic.DynamicObject> class.</span></span> <span data-ttu-id="05176-121">然后，可以替代 <xref:System.Dynamic.DynamicObject> 类的成员以提供运行时动态功能。</span><span class="sxs-lookup"><span data-stu-id="05176-121">You can then override the members of the <xref:System.Dynamic.DynamicObject> class to provide run-time dynamic functionality.</span></span>

 <span data-ttu-id="05176-122">本文包含两个独立的演练：</span><span class="sxs-lookup"><span data-stu-id="05176-122">This article contains two independent walkthroughs:</span></span>

- <span data-ttu-id="05176-123">创建一个自定义对象，该对象会将文本文件的内容作为对象的属性动态公开。</span><span class="sxs-lookup"><span data-stu-id="05176-123">Create a custom object that dynamically exposes the contents of a text file as properties of an object.</span></span>

- <span data-ttu-id="05176-124">创建使用 `IronPython` 库的项目。</span><span class="sxs-lookup"><span data-stu-id="05176-124">Create a project that uses an `IronPython` library.</span></span>

<span data-ttu-id="05176-125">你可以完成其中一个，也可以同时完成两个；如果要完成两个，则顺序无关紧要。</span><span class="sxs-lookup"><span data-stu-id="05176-125">You can do either one of these or both of them, and if you do both, the order doesn't matter.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="05176-126">先决条件</span><span class="sxs-lookup"><span data-stu-id="05176-126">Prerequisites</span></span>

* <span data-ttu-id="05176-127">安装了具有 .NET Core 桌面开发工作负载的 [Visual Studio 2019 版本 16.9 或更高版本](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019)。</span><span class="sxs-lookup"><span data-stu-id="05176-127">[Visual Studio 2019 version 16.9 or a later version](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) with the **.NET desktop development** workload installed.</span></span> <span data-ttu-id="05176-128">选择此工作负载时，将自动安装 .NET 5.0 SDK。</span><span class="sxs-lookup"><span data-stu-id="05176-128">The .NET 5.0 SDK is automatically installed when you select this workload.</span></span>

[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]

* <span data-ttu-id="05176-129">对于第二个演练，安装 [IronPython](https://ironpython.net/) for .NET。</span><span class="sxs-lookup"><span data-stu-id="05176-129">For the second walkthrough, install [IronPython](https://ironpython.net/) for .NET.</span></span> <span data-ttu-id="05176-130">转到其[下载页](https://ironpython.net/download/)以获取最新版本。</span><span class="sxs-lookup"><span data-stu-id="05176-130">Go to their [Download page](https://ironpython.net/download/) to obtain the latest version.</span></span>

## <a name="create-a-custom-dynamic-object"></a><span data-ttu-id="05176-131">创建自定义动态对象</span><span class="sxs-lookup"><span data-stu-id="05176-131">Create a Custom Dynamic Object</span></span>

<span data-ttu-id="05176-132">第一个演练定义了搜索文本文件内容的自定义动态对象。</span><span class="sxs-lookup"><span data-stu-id="05176-132">The first walkthrough defines a custom dynamic object that searches the contents of a text file.</span></span> <span data-ttu-id="05176-133">动态属性指定要搜索的文本。</span><span class="sxs-lookup"><span data-stu-id="05176-133">A dynamic property specifies the text to search for.</span></span> <span data-ttu-id="05176-134">例如，如果调用代码指定 `dynamicFile.Sample`，则动态类将返回一个字符串泛型列表，其中包含该文件中以“Sample”开头的所有行。</span><span class="sxs-lookup"><span data-stu-id="05176-134">For example, if calling code specifies `dynamicFile.Sample`, the dynamic class returns a generic list of strings that contains all of the lines from the file that begin with "Sample".</span></span> <span data-ttu-id="05176-135">搜索不区分大小写。</span><span class="sxs-lookup"><span data-stu-id="05176-135">The search is case-insensitive.</span></span> <span data-ttu-id="05176-136">动态类还支持两个可选参数。</span><span class="sxs-lookup"><span data-stu-id="05176-136">The dynamic class also supports two optional arguments.</span></span> <span data-ttu-id="05176-137">第一个参数是一个搜索选项枚举值，它指定动态类应在行的开头、行的结尾或行中任意位置搜索匹配项。</span><span class="sxs-lookup"><span data-stu-id="05176-137">The first argument is a search option enum value that specifies that the dynamic class should search for matches at the start of the line, the end of the line, or anywhere in the line.</span></span> <span data-ttu-id="05176-138">第二个参数指定动态类应在搜索之前去除每行中的前导空格和尾部空格。</span><span class="sxs-lookup"><span data-stu-id="05176-138">The second argument specifies that the dynamic class should trim leading and trailing spaces from each line before searching.</span></span> <span data-ttu-id="05176-139">例如，如果调用代码指定 `dynamicFile.Sample(StringSearchOption.Contains)`，则动态类将在行中的任意位置搜索“Sample”。</span><span class="sxs-lookup"><span data-stu-id="05176-139">For example, if calling code specifies `dynamicFile.Sample(StringSearchOption.Contains)`, the dynamic class searches for "Sample" anywhere in a line.</span></span> <span data-ttu-id="05176-140">如果调用代码指定 `dynamicFile.Sample(StringSearchOption.StartsWith, false)`，则动态类将在每行的开头搜索“Sample”，但不会删除前导空格和尾部空格。</span><span class="sxs-lookup"><span data-stu-id="05176-140">If calling code specifies `dynamicFile.Sample(StringSearchOption.StartsWith, false)`, the dynamic class searches for "Sample" at the start of each line, and does not remove leading and trailing spaces.</span></span> <span data-ttu-id="05176-141">动态类的默认行为是在每行的开头搜索匹配项，并删除前导空格和尾部空格。</span><span class="sxs-lookup"><span data-stu-id="05176-141">The default behavior of the dynamic class is to search for a match at the start of each line and to remove leading and trailing spaces.</span></span>

### <a name="to-create-a-custom-dynamic-class"></a><span data-ttu-id="05176-142">创建自定义动态类</span><span class="sxs-lookup"><span data-stu-id="05176-142">To create a custom dynamic class</span></span>

1. <span data-ttu-id="05176-143">启动 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="05176-143">Start Visual Studio.</span></span>

1. <span data-ttu-id="05176-144">选择“创建新项目”。</span><span class="sxs-lookup"><span data-stu-id="05176-144">Select **Create a new project**.</span></span>

1. <span data-ttu-id="05176-145">在“创建新项目”对话框中，选择 C# 或 Visual Basic，然后依次选择“控制台应用程序”和“下一步”。</span><span class="sxs-lookup"><span data-stu-id="05176-145">In the **Create a new project** dialog, select C# or Visual Basic, select **Console Application**, and then select **Next**.</span></span>

1. <span data-ttu-id="05176-146">在“配置新项目”对话框中，输入 `DynamicSample` 作为“项目名称”，然后选择“下一步”  。</span><span class="sxs-lookup"><span data-stu-id="05176-146">In the **Configure your new project** dialog, enter `DynamicSample` for the **Project name**, and then select **Next**.</span></span>

1. <span data-ttu-id="05176-147">在“其他信息”对话框中，为“目标框架”选择“.NET 5.0 (当前)”，然后选择“创建”。</span><span class="sxs-lookup"><span data-stu-id="05176-147">In the **Additional information** dialog, select **.NET 5.0 (Current)** for the **Target Framework**, and then select **Create**.</span></span>

   <span data-ttu-id="05176-148">创建新项目。</span><span class="sxs-lookup"><span data-stu-id="05176-148">The new project is created.</span></span>

1. <span data-ttu-id="05176-149">在“解决方案资源管理器”中，右键单击 DynamicSample 项目，然后选择“添加” > “类”。</span><span class="sxs-lookup"><span data-stu-id="05176-149">In **Solution Explorer**, right-click the DynamicSample project and select **Add** > **Class**.</span></span> <span data-ttu-id="05176-150">在“名称”框中，键入 `ReadOnlyFile`，然后选择“添加”。</span><span class="sxs-lookup"><span data-stu-id="05176-150">In the **Name** box, type `ReadOnlyFile`, and then select **Add**.</span></span>

   <span data-ttu-id="05176-151">这将添加一个包含 ReadOnlyFile 类的新文件。</span><span class="sxs-lookup"><span data-stu-id="05176-151">A new file is added that contains the ReadOnlyFile class.</span></span>

1. <span data-ttu-id="05176-152">在 ReadOnlyFile.cs 或 ReadOnlyFile.vb 文件的顶部，添加以下代码以导入 <xref:System.IO?displayProperty=nameWithType> 和 <xref:System.Dynamic?displayProperty=nameWithType> 命名空间。</span><span class="sxs-lookup"><span data-stu-id="05176-152">At the top of the *ReadOnlyFile.cs* or *ReadOnlyFile.vb* file, add the following code to import the <xref:System.IO?displayProperty=nameWithType> and <xref:System.Dynamic?displayProperty=nameWithType> namespaces.</span></span>

    [!code-csharp[VbDynamicWalkthrough#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#1)]
    [!code-vb[VbDynamicWalkthrough#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#1)]

1. <span data-ttu-id="05176-153">自定义动态对象使用一个枚举来确定搜索条件。</span><span class="sxs-lookup"><span data-stu-id="05176-153">The custom dynamic object uses an enum to determine the search criteria.</span></span> <span data-ttu-id="05176-154">在类语句的前面，添加以下枚举定义。</span><span class="sxs-lookup"><span data-stu-id="05176-154">Before the class statement, add the following enum definition.</span></span>

    [!code-csharp[VbDynamicWalkthrough#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#2)]
    [!code-vb[VbDynamicWalkthrough#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#2)]

1. <span data-ttu-id="05176-155">更新类语句以继承 `DynamicObject` 类，如以下代码示例所示。</span><span class="sxs-lookup"><span data-stu-id="05176-155">Update the class statement to inherit the `DynamicObject` class, as shown in the following code example.</span></span>

    [!code-csharp[VbDynamicWalkthrough#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#3)]
    [!code-vb[VbDynamicWalkthrough#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#3)]

1. <span data-ttu-id="05176-156">将以下代码添加到 `ReadOnlyFile` 类，定义一个用于文件路径的私有字段，并定义 `ReadOnlyFile` 类的构造函数。</span><span class="sxs-lookup"><span data-stu-id="05176-156">Add the following code to the `ReadOnlyFile` class to define a private field for the file path and a constructor for the `ReadOnlyFile` class.</span></span>

    [!code-csharp[VbDynamicWalkthrough#4](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#4)]
    [!code-vb[VbDynamicWalkthrough#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#4)]

1. <span data-ttu-id="05176-157">将下面的 `GetPropertyValue` 方法添加到 `ReadOnlyFile` 类。</span><span class="sxs-lookup"><span data-stu-id="05176-157">Add the following `GetPropertyValue` method to the `ReadOnlyFile` class.</span></span> <span data-ttu-id="05176-158">`GetPropertyValue` 方法接收搜索条件作为输入，并返回文本文件中符合该搜索条件的行。</span><span class="sxs-lookup"><span data-stu-id="05176-158">The `GetPropertyValue` method takes, as input, search criteria and returns the lines from a text file that match that search criteria.</span></span> <span data-ttu-id="05176-159">由 `ReadOnlyFile` 类提供的动态方法将调用 `GetPropertyValue` 方法以检索其各自的结果。</span><span class="sxs-lookup"><span data-stu-id="05176-159">The dynamic methods provided by the `ReadOnlyFile` class call the `GetPropertyValue` method to retrieve their respective results.</span></span>

    [!code-csharp[VbDynamicWalkthrough#5](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#5)]
    [!code-vb[VbDynamicWalkthrough#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#5)]

1. <span data-ttu-id="05176-160">在 `GetPropertyValue` 方法后，添加以下代码以替代 <xref:System.Dynamic.DynamicObject> 类的 <xref:System.Dynamic.DynamicObject.TryGetMember%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="05176-160">After the `GetPropertyValue` method, add the following code to override the <xref:System.Dynamic.DynamicObject.TryGetMember%2A> method of the <xref:System.Dynamic.DynamicObject> class.</span></span> <span data-ttu-id="05176-161">请求动态类的成员且未指定任何参数时，将调用 <xref:System.Dynamic.DynamicObject.TryGetMember%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="05176-161">The <xref:System.Dynamic.DynamicObject.TryGetMember%2A> method is called when a member of a dynamic class is requested and no arguments are specified.</span></span> <span data-ttu-id="05176-162">`binder` 参数包含有关被引用成员的信息，而 `result` 参数则引用为指定的成员返回的结果。</span><span class="sxs-lookup"><span data-stu-id="05176-162">The `binder` argument contains information about the referenced member, and the `result` argument references the result returned for the specified member.</span></span> <span data-ttu-id="05176-163"><xref:System.Dynamic.DynamicObject.TryGetMember%2A> 方法会返回一个布尔值，如果请求的成员存在，则返回的布尔值为 `true`，否则返回的布尔值为 `false`。</span><span class="sxs-lookup"><span data-stu-id="05176-163">The <xref:System.Dynamic.DynamicObject.TryGetMember%2A> method returns a Boolean value that returns `true` if the requested member exists; otherwise it returns `false`.</span></span>

    [!code-csharp[VbDynamicWalkthrough#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#6)]
    [!code-vb[VbDynamicWalkthrough#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#6)]

1. <span data-ttu-id="05176-164">在 `TryGetMember` 方法后，添加以下代码以替代 <xref:System.Dynamic.DynamicObject> 类的 <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="05176-164">After the `TryGetMember` method, add the following code to override the <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> method of the <xref:System.Dynamic.DynamicObject> class.</span></span> <span data-ttu-id="05176-165">使用参数请求动态类的成员时，将调用 <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> 方法。</span><span class="sxs-lookup"><span data-stu-id="05176-165">The <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> method is called when a member of a dynamic class is requested with arguments.</span></span> <span data-ttu-id="05176-166">`binder` 参数包含有关被引用成员的信息，而 `result` 参数则引用为指定的成员返回的结果。</span><span class="sxs-lookup"><span data-stu-id="05176-166">The `binder` argument contains information about the referenced member, and the `result` argument references the result returned for the specified member.</span></span> <span data-ttu-id="05176-167">`args` 参数包含一个传递给成员的参数的数组。</span><span class="sxs-lookup"><span data-stu-id="05176-167">The `args` argument contains an array of the arguments that are passed to the member.</span></span> <span data-ttu-id="05176-168"><xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> 方法会返回一个布尔值，如果请求的成员存在，则返回的布尔值为 `true`，否则返回的布尔值为 `false`。</span><span class="sxs-lookup"><span data-stu-id="05176-168">The <xref:System.Dynamic.DynamicObject.TryInvokeMember%2A> method returns a Boolean value that returns `true` if the requested member exists; otherwise it returns `false`.</span></span>

    <span data-ttu-id="05176-169">`TryInvokeMember` 方法的自定义版本期望第一个参数为上一步骤中定义的 `StringSearchOption` 枚举中的值。</span><span class="sxs-lookup"><span data-stu-id="05176-169">The custom version of the `TryInvokeMember` method expects the first argument to be a value from the `StringSearchOption` enum that you defined in a previous step.</span></span> <span data-ttu-id="05176-170">`TryInvokeMember` 方法期望第二个参数为一个布尔值。</span><span class="sxs-lookup"><span data-stu-id="05176-170">The `TryInvokeMember` method expects the second argument to be a Boolean value.</span></span> <span data-ttu-id="05176-171">如果这两个参数有一个或全部为有效值，则将它们传递给 `GetPropertyValue` 方法以检索结果。</span><span class="sxs-lookup"><span data-stu-id="05176-171">If one or both arguments are valid values, they are passed to the `GetPropertyValue` method to retrieve the results.</span></span>

    [!code-csharp[VbDynamicWalkthrough#7](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/readonlyfile.cs#7)]
    [!code-vb[VbDynamicWalkthrough#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/readonlyfile.vb#7)]

1. <span data-ttu-id="05176-172">保存并关闭文件。</span><span class="sxs-lookup"><span data-stu-id="05176-172">Save and close the file.</span></span>

### <a name="to-create-a-sample-text-file"></a><span data-ttu-id="05176-173">创建示例文本文件</span><span class="sxs-lookup"><span data-stu-id="05176-173">To create a sample text file</span></span>

1. <span data-ttu-id="05176-174">在“解决方案资源管理器”中，右键单击 DynamicSample 项目，然后选择“添加” > “新建项目”  。</span><span class="sxs-lookup"><span data-stu-id="05176-174">In **Solution Explorer**, right-click the DynamicSample project and select **Add** > **New Item**.</span></span> <span data-ttu-id="05176-175">在“已安装的模板”窗格中，选择“常规”，然后选择“文本文件”模板。</span><span class="sxs-lookup"><span data-stu-id="05176-175">In the **Installed Templates** pane, select **General**, and then select the **Text File** template.</span></span> <span data-ttu-id="05176-176">保留“名称”框中的默认名称 TextFile1.txt，然后单击“添加”。</span><span class="sxs-lookup"><span data-stu-id="05176-176">Leave the default name of *TextFile1.txt* in the **Name** box, and then click **Add**.</span></span> <span data-ttu-id="05176-177">这会将一个新的文本文件添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="05176-177">A new text file is added to the project.</span></span>

1. <span data-ttu-id="05176-178">将以下文本复制到 TextFile1.txt 文件。</span><span class="sxs-lookup"><span data-stu-id="05176-178">Copy the following text to the *TextFile1.txt* file.</span></span>

    ```text
    List of customers and suppliers

    Supplier: Lucerne Publishing (https://www.lucernepublishing.com/)
    Customer: Preston, Chris
    Customer: Hines, Patrick
    Customer: Cameron, Maria
    Supplier: Graphic Design Institute (https://www.graphicdesigninstitute.com/)
    Supplier: Fabrikam, Inc. (https://www.fabrikam.com/)
    Customer: Seubert, Roxanne
    Supplier: Proseware, Inc. (http://www.proseware.com/)
    Customer: Adolphi, Stephan
    Customer: Koch, Paul
    ```

1. <span data-ttu-id="05176-179">保存并关闭文件。</span><span class="sxs-lookup"><span data-stu-id="05176-179">Save and close the file.</span></span>

### <a name="to-create-a-sample-application-that-uses-the-custom-dynamic-object"></a><span data-ttu-id="05176-180">创建一个使用自定义动态对象的示例应用程序</span><span class="sxs-lookup"><span data-stu-id="05176-180">To create a sample application that uses the custom dynamic object</span></span>

1. <span data-ttu-id="05176-181">在“解决方案资源管理器”中，双击 Program.vb 文件（如果使用 Visual Basic）或 Program.cs 文件（如果使用 Visual C#）。</span><span class="sxs-lookup"><span data-stu-id="05176-181">In **Solution Explorer**, double-click the *Program.vb* file if you're using Visual Basic or the *Program.cs* file if you're using Visual C#.</span></span>

2. <span data-ttu-id="05176-182">将以下代码添加到 `Main` 过程，为 TextFile1.txt 文件创建 `ReadOnlyFile` 类的实例。</span><span class="sxs-lookup"><span data-stu-id="05176-182">Add the following code to the `Main` procedure to create an instance of the `ReadOnlyFile` class for the *TextFile1.txt* file.</span></span> <span data-ttu-id="05176-183">代码将使用晚期绑定来调用动态成员，并检索包含字符串“Customer”的文本行。</span><span class="sxs-lookup"><span data-stu-id="05176-183">The code uses late binding to call dynamic members and retrieve lines of text that contain the string "Customer".</span></span>

     [!code-csharp[VbDynamicWalkthrough#8](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthrough/cs/program.cs#8)]
     [!code-vb[VbDynamicWalkthrough#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthrough/vb/Program.vb#8)]

3. <span data-ttu-id="05176-184">保存文件，然后按 <kbd>Ctrl</kdb>+<kbd>F5</kbd> 生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="05176-184">Save the file and press <kbd>Ctrl</kdb>+<kbd>F5</kbd> to build and run the application.</span></span>

## <a name="call-a-dynamic-language-library"></a><span data-ttu-id="05176-185">调用动态语言库</span><span class="sxs-lookup"><span data-stu-id="05176-185">Call a dynamic language library</span></span>

<span data-ttu-id="05176-186">以下演练创建的项目将访问以动态语言 IronPython 编写的库。</span><span class="sxs-lookup"><span data-stu-id="05176-186">The following walkthrough creates a project that accesses a library that is written in the dynamic language IronPython.</span></span>

### <a name="to-create-a-custom-dynamic-class"></a><span data-ttu-id="05176-187">创建自定义动态类</span><span class="sxs-lookup"><span data-stu-id="05176-187">To create a custom dynamic class</span></span>

1. <span data-ttu-id="05176-188">在 Visual Studio 中，选择“文件” > “新建” > “项目”。</span><span class="sxs-lookup"><span data-stu-id="05176-188">In Visual Studio, select **File** > **New** > **Project**.</span></span>

1. <span data-ttu-id="05176-189">在“创建新项目”对话框中，选择 C# 或 Visual Basic，然后依次选择“控制台应用程序”和“下一步”。</span><span class="sxs-lookup"><span data-stu-id="05176-189">In the **Create a new project** dialog, select C# or Visual Basic, select **Console Application**, and then select **Next**.</span></span>

1. <span data-ttu-id="05176-190">在“配置新项目”对话框中，输入 `DynamicIronPythonSample` 作为“项目名称”，然后选择“下一步”  。</span><span class="sxs-lookup"><span data-stu-id="05176-190">In the **Configure your new project** dialog, enter `DynamicIronPythonSample` for the **Project name**, and then select **Next**.</span></span>

1. <span data-ttu-id="05176-191">在“其他信息”对话框中，为“目标框架”选择“.NET 5.0 (当前)”，然后选择“创建”。</span><span class="sxs-lookup"><span data-stu-id="05176-191">In the **Additional information** dialog, select **.NET 5.0 (Current)** for the **Target Framework**, and then select **Create**.</span></span>

   <span data-ttu-id="05176-192">创建新项目。</span><span class="sxs-lookup"><span data-stu-id="05176-192">The new project is created.</span></span>

1. <span data-ttu-id="05176-193">安装 [IronPython](https://www.nuget.org/packages/IronPython) NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="05176-193">Install the [IronPython](https://www.nuget.org/packages/IronPython) NuGet package.</span></span>

1. <span data-ttu-id="05176-194">如果使用 Visual Basic，请编辑 Program.vb 文件。</span><span class="sxs-lookup"><span data-stu-id="05176-194">If you're using Visual Basic, edit the *Program.vb* file.</span></span> <span data-ttu-id="05176-195">如果使用 Visual C#，请编辑 Program.cs 文件。</span><span class="sxs-lookup"><span data-stu-id="05176-195">If you're using Visual C#, edit the *Program.cs* file.</span></span>

1. <span data-ttu-id="05176-196">在文件的顶部，添加以下代码以从 IronPython 库和 `System.Linq` 命名空间导入 `Microsoft.Scripting.Hosting` 和 `IronPython.Hosting` 命名空间。</span><span class="sxs-lookup"><span data-stu-id="05176-196">At the top of the file, add the following code to import the `Microsoft.Scripting.Hosting` and `IronPython.Hosting` namespaces from the IronPython libraries and the `System.Linq` namespace.</span></span>

    [!code-csharp[VbDynamicWalkthroughIronPython#1](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#1)]
    [!code-vb[VbDynamicWalkthroughIronPython#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/Program.vb#1)]

1. <span data-ttu-id="05176-197">在 Main 方法中，添加以下代码以创建用于托管 IronPython 库的新 `Microsoft.Scripting.Hosting.ScriptRuntime` 对象。</span><span class="sxs-lookup"><span data-stu-id="05176-197">In the Main method, add the following code to create a new `Microsoft.Scripting.Hosting.ScriptRuntime` object to host the IronPython libraries.</span></span> <span data-ttu-id="05176-198">`ScriptRuntime` 对象加载 IronPython 库模块 random.py。</span><span class="sxs-lookup"><span data-stu-id="05176-198">The `ScriptRuntime` object loads the IronPython library module random.py.</span></span>

     [!code-csharp[VbDynamicWalkthroughIronPython#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#2)]
     [!code-vb[VbDynamicWalkthroughIronPython#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/Program.vb#2)]

1. <span data-ttu-id="05176-199">在用于加载 random.py 模块的代码之后，添加以下代码以创建一个整数数组。</span><span class="sxs-lookup"><span data-stu-id="05176-199">After the code to load the random.py module, add the following code to create an array of integers.</span></span> <span data-ttu-id="05176-200">数组传递给 random.py 模块的 `shuffle` 方法，该方法对数组中的值进行随机排序。</span><span class="sxs-lookup"><span data-stu-id="05176-200">The array is passed to the `shuffle` method of the random.py module, which randomly sorts the values in the array.</span></span>

     [!code-csharp[VbDynamicWalkthroughIronPython#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/cs/program.cs#3)]
     [!code-vb[VbDynamicWalkthroughIronPython#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbdynamicwalkthroughironpython/vb/Program.vb#3)]

1. <span data-ttu-id="05176-201">保存文件，然后按 <kbd>Ctrl</kdb>+<kbd>F5</kbd> 生成并运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="05176-201">Save the file and press <kbd>Ctrl</kdb>+<kbd>F5</kbd> to build and run the application.</span></span>

## <a name="see-also"></a><span data-ttu-id="05176-202">请参阅</span><span class="sxs-lookup"><span data-stu-id="05176-202">See also</span></span>

- <xref:System.Dynamic?displayProperty=nameWithType>
- <xref:System.Dynamic.DynamicObject?displayProperty=nameWithType>
- [<span data-ttu-id="05176-203">使用类型 dynamic</span><span class="sxs-lookup"><span data-stu-id="05176-203">Using Type dynamic</span></span>](./using-type-dynamic.md)
- [<span data-ttu-id="05176-204">早期绑定和后期绑定</span><span class="sxs-lookup"><span data-stu-id="05176-204">Early and Late Binding</span></span>](../../../visual-basic/programming-guide/language-features/early-late-binding/index.md)
- [<span data-ttu-id="05176-205">dynamic</span><span class="sxs-lookup"><span data-stu-id="05176-205">dynamic</span></span>](../../language-reference/builtin-types/reference-types.md)
- [<span data-ttu-id="05176-206">实现动态接口（可从 Microsoft TechNet 下载 PDF）</span><span class="sxs-lookup"><span data-stu-id="05176-206">Implementing Dynamic Interfaces (downloadable PDF from Microsoft TechNet)</span></span>](https://download.microsoft.com/download/5/4/B/54B83DFE-D7AA-4155-9687-B0CF58FF65D7/implementing-dynamic-interfaces.pdf)
