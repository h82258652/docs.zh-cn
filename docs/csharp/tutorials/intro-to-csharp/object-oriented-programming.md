---
title: 面向对象的编程 (C#)
description: C# 提供针对面向对象的编程（包括抽象、封装、继承和多态性）的完整支持。
ms.date: 09/30/2020
ms.openlocfilehash: 8ed304233fe29b46e1da9185b6e079e614a55c79
ms.sourcegitcommit: 23e2bc267872ce6ea22db4bf6478fe57bcba4bc8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/17/2021
ms.locfileid: "107592248"
---
# <a name="object-oriented-programming-c"></a><span data-ttu-id="ae1dd-103">面向对象的编程 (C#)</span><span class="sxs-lookup"><span data-stu-id="ae1dd-103">Object-Oriented programming (C#)</span></span>

<span data-ttu-id="ae1dd-104">C# 是面向对象的编程语言。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-104">C# is an object-oriented programming language.</span></span> <span data-ttu-id="ae1dd-105">面向对象编程的四项基本原则为：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-105">The four basic principles of object-oriented programming are:</span></span>

- <span data-ttu-id="ae1dd-106">抽象：将实体的相关特性和交互建模为类，以定义系统的抽象表示。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-106">*Abstraction* Modeling the relevant attributes and interactions of entities as classes to define an abstract representation of a system.</span></span>
- <span data-ttu-id="ae1dd-107">封装：隐藏对象的内部状态和功能，并仅允许通过一组公共函数进行访问。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-107">*Encapsulation* Hiding the internal state and functionality of an object and only allowing access through a public set of functions.</span></span>
- <span data-ttu-id="ae1dd-108">继承：根据现有抽象创建新抽象的能力。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-108">*Inheritance* Ability to create new abstractions based on existing abstractions.</span></span>
- <span data-ttu-id="ae1dd-109">多形性：跨多个抽象以不同方式实现继承属性或方法的能力。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-109">*Polymorphism* Ability to implement inherited properties or methods in different ways across multiple abstractions.</span></span>

<span data-ttu-id="ae1dd-110">在前面的[类简介](introduction-to-classes.md) 教程中，我们介绍了抽象和封装。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-110">In the preceding tutorial, [introduction to classes](introduction-to-classes.md) you saw both *abstraction* and *encapsulation*.</span></span> <span data-ttu-id="ae1dd-111">`BankAccount` 类提供银行帐户这一概念的抽象。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-111">The `BankAccount` class provided an abstraction for the concept of a bank account.</span></span> <span data-ttu-id="ae1dd-112">你可以修改其实现，而不影响使用 `BankAccount` 类的任何代码。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-112">You could modify its implementation without affecting any of the code that used the `BankAccount` class.</span></span> <span data-ttu-id="ae1dd-113">`BankAccount` 和 `Transaction` 类都提供在代码中描述这些概念所需组件的封装。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-113">Both the `BankAccount` and `Transaction` classes provide encapsulation of the components needed to describe those concepts in code.</span></span>

<span data-ttu-id="ae1dd-114">在本教程中，你将扩展该应用程序以利用继承和多形性来添加新功能。 </span><span class="sxs-lookup"><span data-stu-id="ae1dd-114">In this tutorial, you'll extend that application to make use of *inheritance* and *polymorphism* to add new features.</span></span> <span data-ttu-id="ae1dd-115">你还将利用在上一教程中学到的抽象和封装技术，向 `BankAccount`  类添加功能。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-115">You'll also add features to the `BankAccount` class, taking advantage of the *abstraction* and *encapsulation* techniques you learned in the preceding tutorial.</span></span>

## <a name="create-different-types-of-accounts"></a><span data-ttu-id="ae1dd-116">创建不同类型的帐户</span><span class="sxs-lookup"><span data-stu-id="ae1dd-116">Create different types of accounts</span></span>

<span data-ttu-id="ae1dd-117">生成此程序后，你将获取向其添加功能的请求。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-117">After building this program, you get requests to add features to it.</span></span> <span data-ttu-id="ae1dd-118">在只有一种银行帐户类型的情况下，其效果良好。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-118">It works great in the situation where there is only one bank account type.</span></span> <span data-ttu-id="ae1dd-119">随着时间的推移，需求会发生变化，因而请求了相关的帐户类型：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-119">Over time, needs change, and related account types are requested:</span></span>

- <span data-ttu-id="ae1dd-120">在每个月的月末获得利息的红利帐户。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-120">An interest earning account that accrues interest at the end of each month.</span></span>
- <span data-ttu-id="ae1dd-121">余额可以为负，但存在余额时会产生每月利息的信用帐户。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-121">A line of credit that can have a negative balance, but when there's a balance, there's an interest charge each month.</span></span>
- <span data-ttu-id="ae1dd-122">以单笔存款开户且只能用于支付的预付礼品卡帐户。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-122">A pre-paid gift card account that starts with a single deposit, and only can be paid off.</span></span> <span data-ttu-id="ae1dd-123">可在每月初重新充值一次。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-123">It can be refilled once at the start of each month.</span></span>

<span data-ttu-id="ae1dd-124">所有这些帐户都与前面的教程中定义的 `BankAccount` 类类似。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-124">All of these different accounts are similar to `BankAccount` class defined in the earlier tutorial.</span></span> <span data-ttu-id="ae1dd-125">你可以复制该代码、重命名类并进行修改。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-125">You could copy that code, rename the classes, and make modifications.</span></span> <span data-ttu-id="ae1dd-126">这种方法在短期内有效，但随着时间的推移，其效果会更为明显。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-126">That technique would work in the short term, but it would be more work over time.</span></span> <span data-ttu-id="ae1dd-127">所有更改都将复制到所有受影响的类。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-127">Any changes would be copied across all the affected classes.</span></span>

<span data-ttu-id="ae1dd-128">相反，你可以创建新的银行帐户类型，使其从上一教程中创建的 `BankAccount` 类继承方法和数据。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-128">Instead, you can create new bank account types that inherit methods and data from the `BankAccount` class created in the preceding tutorial.</span></span> <span data-ttu-id="ae1dd-129">这些新类可以用每种类型所需的特定行为来扩展 `BankAccount` 类：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-129">These new classes can extend the `BankAccount` class with the specific behavior needed for each type:</span></span>

```csharp
public class InterestEarningAccount : BankAccount
{
}

public class LineOfCreditAccount : BankAccount
{
}

public class GiftCardAccount : BankAccount
{
}
```

<span data-ttu-id="ae1dd-130">其中的每个类都从其共享的基类（ `BankAccount` 类）继承共享的行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-130">Each of these classes *inherits* the shared behavior from their shared *base class*, the `BankAccount` class.</span></span> <span data-ttu-id="ae1dd-131">为的每个派生类中的新增和不同功能编写实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-131">Write the implementations for new and different functionality in each of the *derived classes*.</span></span>  <span data-ttu-id="ae1dd-132">这些派生类已具有 `BankAccount` 类中定义的所有行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-132">These derived classes already have all the behavior defined in the `BankAccount` class.</span></span>

<span data-ttu-id="ae1dd-133">最好在不同的源文件中创建每个新类。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-133">It's a good practice to create each new class in a different source file.</span></span> <span data-ttu-id="ae1dd-134">在 [Visual Studio](https://visualstudio.com)中，可以右键单击项目，然后选择“添加类”以在新文件中添加新类。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-134">In [Visual Studio](https://visualstudio.com), you can right-click on the project, and select *add class* to add a new class in a new file.</span></span> <span data-ttu-id="ae1dd-135">在 [Visual Studio Code](https://code.visualstudio.com) 中，选择“文件”，然后选择“新建”以创建新的源文件。 </span><span class="sxs-lookup"><span data-stu-id="ae1dd-135">In [Visual Studio Code](https://code.visualstudio.com), select *File* then *New* to create a new source file.</span></span> <span data-ttu-id="ae1dd-136">在任一工具中，将文件命名为与类匹配：InterestEarningAccount.cs、LineOfCreditAccount.cs 和 GiftCardAccount.cs。  </span><span class="sxs-lookup"><span data-stu-id="ae1dd-136">In either tool, name the file to match the class: *InterestEarningAccount.cs*, *LineOfCreditAccount.cs*, and *GiftCardAccount.cs*.</span></span>

<span data-ttu-id="ae1dd-137">如前一示例中所示创建类时，你会发现派生类都没有编译。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-137">When you create the classes as shown in the preceding sample, you'll find that none of your derived classes compile.</span></span> <span data-ttu-id="ae1dd-138">构造函数负责初始化对象。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-138">A constructor is responsible for initializing an object.</span></span> <span data-ttu-id="ae1dd-139">派生类构造函数必须初始化派生类，并提供有关如何初始化派生类中所包含基类对象的说明。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-139">A derived class constructor must initialize the derived class, and provide instructions on how to initialize the base class object included in the derived class.</span></span> <span data-ttu-id="ae1dd-140">一般无需任何额外的代码即可正常初始化。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-140">The proper initialization normally happens without any extra code.</span></span> <span data-ttu-id="ae1dd-141">`BankAccount` 类声明一个具有以下签名的公共构造函数：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-141">The `BankAccount` class declares one public constructor with the following signature:</span></span>

```csharp
public BankAccount(string name, decimal initialBalance)
```

<span data-ttu-id="ae1dd-142">当你自行定义构造函数时，编译器不会生成默认构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-142">The compiler doesn't generate a default constructor when you define a constructor yourself.</span></span> <span data-ttu-id="ae1dd-143">这意味着每个派生类必须显式调用此构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-143">That means each derived class must explicitly call this constructor.</span></span> <span data-ttu-id="ae1dd-144">声明可将自变量传递到基类构造函数的构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-144">You declare a constructor that can pass arguments to the base class constructor.</span></span>  <span data-ttu-id="ae1dd-145">下面的代码显示了 `InterestEarningAccount` 的构造函数：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-145">The following code shows the constructor for the `InterestEarningAccount`:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="DerivedConstructor":::

<span data-ttu-id="ae1dd-146">此新构造函数的参数与基类构造函数的参数类型和名称匹配。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-146">The parameters to this new constructor match the parameter type and names of the base class constructor.</span></span> <span data-ttu-id="ae1dd-147">使用 `: base()` 语法来指示对基类构造函数的调用。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-147">You use the `: base()` syntax to indicate a call to a base class constructor.</span></span> <span data-ttu-id="ae1dd-148">某些类定义多个构造函数，此语法让你可以选取调用的基类构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-148">Some classes define multiple constructors, and this syntax enables you to pick which base class constructor you call.</span></span> <span data-ttu-id="ae1dd-149">更新构造函数后，可以为每个派生类开发代码。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-149">Once you've updated the constructors, you can develop the code for each of the derived classes.</span></span> <span data-ttu-id="ae1dd-150">可以这样描述对新类的要求：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-150">The requirements for the new classes can be stated as follows:</span></span>

- <span data-ttu-id="ae1dd-151">红利帐户：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-151">An interest earning account:</span></span>
  - <span data-ttu-id="ae1dd-152">将获得月末余额 2% 的额度。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-152">Will get a credit of 2% of the month-ending-balance.</span></span>
- <span data-ttu-id="ae1dd-153">信用帐户：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-153">A line of credit:</span></span>
  - <span data-ttu-id="ae1dd-154">余额可以为负，但不能大于信用限额的绝对值。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-154">Can have a negative balance, but not be greater in absolute value than the credit limit.</span></span>
  - <span data-ttu-id="ae1dd-155">如果月末余额不为 0，每个月都会产生利息。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-155">Will incur an interest charge each month where the end of month balance isn't 0.</span></span>
  - <span data-ttu-id="ae1dd-156">将在超过信用限额的每次提款后收取费用。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-156">Will incur a fee on each withdrawal that goes over the credit limit.</span></span>
- <span data-ttu-id="ae1dd-157">礼品卡帐户：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-157">A gift card account:</span></span>
  - <span data-ttu-id="ae1dd-158">每月最后一天，可以充值一次指定的金额。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-158">Can be refilled with a specified amount once each month, on the last day of the month.</span></span>

<span data-ttu-id="ae1dd-159">可以看到，这三种帐户类型都有一个在每月月末发生的操作。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-159">You can see that all three of these account types have an action that takes places at the end of each month.</span></span> <span data-ttu-id="ae1dd-160">但是，每种帐户类型负责不同的任务。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-160">However, each account type does different tasks.</span></span> <span data-ttu-id="ae1dd-161">可以使用多态性来实现此代码。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-161">You use *polymorphism* to implement this code.</span></span> <span data-ttu-id="ae1dd-162">在 `BankAccount` 类中创建单个 `virtual` 方法：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-162">Create a single `virtual` method in the `BankAccount` class:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="DeclareMonthEndTransactions":::

<span data-ttu-id="ae1dd-163">前面的代码演示如何使用 `virtual` 关键字在基类中声明一个方法，让派生类可以为该方法提供不同的实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-163">The preceding code shows how you use the `virtual` keyword to declare a method in the base class that a derived class may provide a different implementation for.</span></span> <span data-ttu-id="ae1dd-164">在 `virtual` 方法种，任何派生类都可以选择重新实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-164">A `virtual` method is a method where any derived class may choose to reimplement.</span></span> <span data-ttu-id="ae1dd-165">派生类使用 `override` 关键字定义新的实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-165">The derived classes use the `override` keyword to define the new implementation.</span></span> <span data-ttu-id="ae1dd-166">通常将其称为“重写基类实现”。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-166">Typically you refer to this as "overriding the base class implementation".</span></span> <span data-ttu-id="ae1dd-167">`virtual` 关键字指定派生类可以重写此行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-167">The `virtual` keyword specifies that derived classes may override the behavior.</span></span> <span data-ttu-id="ae1dd-168">还可以声明 `abstract` 方法，让派生类必须在其中重写此行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-168">You can also declare `abstract` methods where derived classes must override the behavior.</span></span> <span data-ttu-id="ae1dd-169">基类不提供 `abstract` 方法的实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-169">The base class does not provide an implementation for an `abstract` method.</span></span> <span data-ttu-id="ae1dd-170">接下来，需要为你创建的两个新类定义实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-170">Next, you need to define the implementation for two of the new classes you've created.</span></span> <span data-ttu-id="ae1dd-171">从 `InterestEarningAccount` 开始：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-171">Start with the `InterestEarningAccount`:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="ApplyMonthendInterest":::

<span data-ttu-id="ae1dd-172">将以下代码添加到 `LineOfCreditAccount`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-172">Add the following code to the `LineOfCreditAccount`.</span></span> <span data-ttu-id="ae1dd-173">此代码会取余额的相反数，计算从该帐户提取的正利息：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-173">The code negates the balance to compute a positive interest charge that is withdrawn from the account:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ApplyMonthendInterest":::

<span data-ttu-id="ae1dd-174">`GiftCardAccount` 类需要通过两项更改来实现其月末功能。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-174">The `GiftCardAccount` class needs two changes to implement its month-end functionality.</span></span> <span data-ttu-id="ae1dd-175">首先，将构造函数修改为包含每个月要充值的可选金额：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-175">First, modify the constructor to include an optional amount to add each month:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="GiftCardAccountConstruction":::

<span data-ttu-id="ae1dd-176">构造函数为 `monthlyDeposit` 值提供默认值，因此调用方可以省略 `0` 以表示不进行每月存款。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-176">The constructor provides a default value for the `monthlyDeposit` value so callers can omit a `0` for no monthly deposit.</span></span> <span data-ttu-id="ae1dd-177">接下来，如果已在构造函数中将 `PerformMonthEndTransactions` 方法设置为非零值，则重写该方法以添加每月存款：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-177">Next, override the `PerformMonthEndTransactions` method to add the monthly deposit, if it was set to a non-zero value in the constructor:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="AddMonthlyDeposit":::

<span data-ttu-id="ae1dd-178">重写将在构造函数中应用每月存款设置。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-178">The override applies the monthly deposit set in the constructor.</span></span> <span data-ttu-id="ae1dd-179">将以下代码添加到 `Main` 方法，以便为 `GiftCardAccount` 和 `InterestEarningAccount` 测试这些更改：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-179">Add the following code to the `Main` method to test these changes for the `GiftCardAccount` and the `InterestEarningAccount`:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="FirstTests":::

<span data-ttu-id="ae1dd-180">验证结果。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-180">Verify the results.</span></span> <span data-ttu-id="ae1dd-181">现在，为 `LineOfCreditAccount` 添加一组类似的测试代码：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-181">Now, add a similar set of test code for the `LineOfCreditAccount`:</span></span>

```
    var lineOfCredit = new LineOfCreditAccount("line of credit", 0);
    // How much is too much to borrow?
    lineOfCredit.MakeWithdrawal(1000m, DateTime.Now, "Take out monthly advance");
    lineOfCredit.MakeDeposit(50m, DateTime.Now, "Pay back small amount");
    lineOfCredit.MakeWithdrawal(5000m, DateTime.Now, "Emergency funds for repairs");
    lineOfCredit.MakeDeposit(150m, DateTime.Now, "Partial restoration on repairs");
    lineOfCredit.PerformMonthEndTransactions();
    Console.WriteLine(lineOfCredit.GetAccountHistory());
 ```

<span data-ttu-id="ae1dd-182">添加前面的代码并运行程序时，你将看到类似于以下错误的内容：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-182">When you add the preceding code and run the program, you'll see something like the following error:</span></span>

```console
Unhandled exception. System.ArgumentOutOfRangeException: Amount of deposit must be positive (Parameter 'amount')
   at OOProgramming.BankAccount.MakeDeposit(Decimal amount, DateTime date, String note) in BankAccount.cs:line 42
   at OOProgramming.BankAccount..ctor(String name, Decimal initialBalance) in BankAccount.cs:line 31
   at OOProgramming.LineOfCreditAccount..ctor(String name, Decimal initialBalance) in LineOfCreditAccount.cs:line 9
   at OOProgramming.Program.Main(String[] args) in Program.cs:line 29
```

> [!NOTE]
> <span data-ttu-id="ae1dd-183">实际输出包含项目文件夹的完整路径。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-183">The actual output includes the full path to the folder with the project.</span></span> <span data-ttu-id="ae1dd-184">为简洁起见，省略了文件夹名称。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-184">The folder names were omitted for brevity.</span></span> <span data-ttu-id="ae1dd-185">此外，根据你的代码格式，行号可能略有不同。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-185">Also, depending on your code format, the line numbers may be slightly different.</span></span>

<span data-ttu-id="ae1dd-186">此代码会失败，因为 `BankAccount` 假设初始余额必须大于 0。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-186">This code fails because the `BankAccount` assumes that the initial balance must be greater than 0.</span></span> <span data-ttu-id="ae1dd-187">`BankAccount` 类固有的另一假设是余额不能为负。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-187">Another assumption baked into the `BankAccount` class is that the balance can't go negative.</span></span> <span data-ttu-id="ae1dd-188">相反，将拒绝透支帐户的任何提款。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-188">Instead, any withdrawal that overdraws the account is rejected.</span></span> <span data-ttu-id="ae1dd-189">这两个假设都需要更改。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-189">Both of those assumptions need to change.</span></span> <span data-ttu-id="ae1dd-190">信用帐户从 0 开始，一般情况下余额为负。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-190">The line of credit account starts at 0, and generally will have a negative balance.</span></span> <span data-ttu-id="ae1dd-191">此外，如果客户借款量过大，将产生费用。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-191">Also, if a customer borrows too much money, they incur a fee.</span></span> <span data-ttu-id="ae1dd-192">会接受该交易，只是会增加费用。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-192">The transaction is accepted, it just costs more.</span></span> <span data-ttu-id="ae1dd-193">可以通过向 `BankAccount` 构造函数添加用于指定最小余额的可选参数来实现第一条规则。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-193">The first rule can be implemented by adding an optional argument to the `BankAccount` constructor that specifies the minimum balance.</span></span> <span data-ttu-id="ae1dd-194">默认为 `0`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-194">The default is `0`.</span></span> <span data-ttu-id="ae1dd-195">第二条规则需要允许派生类修改默认算法的机制。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-195">The second rule requires a mechanism that enables derived classes to modify the default algorithm.</span></span> <span data-ttu-id="ae1dd-196">在某种意义上，基类会“询问”派生类型在透支时应该怎么做。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-196">In a sense, the base class "asks" the derived type what should happen when there's an overdraft.</span></span> <span data-ttu-id="ae1dd-197">默认行为是通过引发异常来拒绝交易。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-197">The default behavior is to reject the transaction by throwing an exception.</span></span>

<span data-ttu-id="ae1dd-198">首先，让我们添加包含可选 `minimumBalance` 参数的第二个构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-198">Let's start by adding a second constructor that includes an optional `minimumBalance` parameter.</span></span> <span data-ttu-id="ae1dd-199">此新构造函数会执行现有构造函数完成的所有操作。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-199">This new constructor does all the actions done by the existing constructor.</span></span> <span data-ttu-id="ae1dd-200">此外，它还会设置最小余额属性。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-200">Also, it sets the minimum balance property.</span></span> <span data-ttu-id="ae1dd-201">可以复制现有构造函数的主体。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-201">You could copy the body of the existing constructor.</span></span> <span data-ttu-id="ae1dd-202">但这意味着将来需要更改两个位置。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-202">but that means two locations to change in the future.</span></span> <span data-ttu-id="ae1dd-203">相反，可以使用构造函数链接，让一个构造函数调用另一个构造函数。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-203">Instead, you can use *constructor chaining* to have one constructor call another.</span></span> <span data-ttu-id="ae1dd-204">下面的代码显示了两个构造函数和新的附加字段：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-204">The following code shows the two constructors and the new additional field:</span></span>

 :::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="ConstructorModifications":::

<span data-ttu-id="ae1dd-205">前面的代码演示了两种新方法。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-205">The preceding code shows two new techniques.</span></span> <span data-ttu-id="ae1dd-206">首先，`minimumBalance` 字段被标记为 `readonly`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-206">First, the `minimumBalance` field is marked as `readonly`.</span></span> <span data-ttu-id="ae1dd-207">这意味着构造对象之后不能更改值。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-207">That means the value cannot be changed after the object is constructed.</span></span> <span data-ttu-id="ae1dd-208">创建 `BankAccount` 后，`minimumBalance` 不可更改。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-208">Once a `BankAccount` is created, the `minimumBalance` can't change.</span></span> <span data-ttu-id="ae1dd-209">其次，采用两个参数的构造函数使用 `: this(name, initialBalance, 0) { }` 作为其实现。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-209">Second, the constructor that takes two parameters uses `: this(name, initialBalance, 0) { }` as its implementation.</span></span> <span data-ttu-id="ae1dd-210">`: this()` 表达式调用另一个构造函数（它具有三个参数）。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-210">The `: this()` expression calls the other constructor, the one with three parameters.</span></span> <span data-ttu-id="ae1dd-211">即使客户端代码可以从多个构造函数中进行选择，你也可以使用单个实现来初始化对象。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-211">This technique allows you to have a single implementation for initializing an object even though client code can choose one of many constructors.</span></span>

<span data-ttu-id="ae1dd-212">仅当初始余额大于 `0` 时，此实现才会调用 `MakeDeposit`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-212">This implementation calls `MakeDeposit` only if the initial balance is greater than `0`.</span></span> <span data-ttu-id="ae1dd-213">这将保留存款必须为正的规则，同时允许信用帐户以 `0` 余额开户。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-213">That preserves the rule that deposits must be positive, yet lets the credit account open with a `0` balance.</span></span>

<span data-ttu-id="ae1dd-214">既然 `BankAccount` 类具有用于规定最小余额的只读字段，最终更改就是在 `MakeWithdrawal` 方法中将硬编码的 `0` 更改为 `minimumBalance`：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-214">Now that the `BankAccount` class has a read-only field for the minimum balance, the final change is to change the hard code `0` to `minimumBalance` in the `MakeWithdrawal` method:</span></span>

```csharp
if (Balance - amount < minimumBalance)
```

<span data-ttu-id="ae1dd-215">扩展 `BankAccount` 类后，可以修改 `LineOfCreditAccount` 构造函数以调用基构造函数，如以下代码中所示：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-215">After extending the `BankAccount` class, you can modify the `LineOfCreditAccount` constructor to call the new base constructor, as shown in the following code:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ConstructLineOfCredit":::

<span data-ttu-id="ae1dd-216">请注意，`LineOfCreditAccount` 构造函数会更改 `creditLimit` 参数的标志，使其与 `minimumBalance` 参数的意义匹配。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-216">Notice that the `LineOfCreditAccount` constructor changes the sign of the `creditLimit` parameter so it matches the meaning of the `minimumBalance` parameter.</span></span>

## <a name="different-overdraft-rules"></a><span data-ttu-id="ae1dd-217">不同的透支规则</span><span class="sxs-lookup"><span data-stu-id="ae1dd-217">Different overdraft rules</span></span>

<span data-ttu-id="ae1dd-218">要添加的最后一项功能让 `LineOfCreditAccount` 可以对超过限额的取款进行收费，而不是拒绝交易。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-218">The last feature to add enables the `LineOfCreditAccount` to charge a fee for going over the credit limit instead of refusing the transaction.</span></span>

<span data-ttu-id="ae1dd-219">一种方法是定义一个虚函数，并在其中实现所需的行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-219">One technique is to define a virtual function where you implement the required behavior.</span></span> <span data-ttu-id="ae1dd-220">`BankAccount` 类将 `MakeWithdrawal` 方法重构为两个方法。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-220">The `BankAccount` class refactors the `MakeWithdrawal` method into two methods.</span></span> <span data-ttu-id="ae1dd-221">当取款使余额低于最小值时，新方法将执行指定的操作。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-221">The new method does the specified action when the withdrawal takes the balance below the minimum.</span></span> <span data-ttu-id="ae1dd-222">现有的 `MakeWithdrawal` 方法具有以下代码：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-222">The existing `MakeWithdrawal` method has the following code:</span></span>

```csharp
public void MakeWithdrawal(decimal amount, DateTime date, string note)
{
    if (amount <= 0)
    {
        throw new ArgumentOutOfRangeException(nameof(amount), "Amount of withdrawal must be positive");
    }
    if (Balance - amount < minimumBalance)
    {
        throw new InvalidOperationException("Not sufficient funds for this withdrawal");
    }
    var withdrawal = new Transaction(-amount, date, note);
    allTransactions.Add(withdrawal);
}
```

<span data-ttu-id="ae1dd-223">将其替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-223">Replace it with the following code:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="RefactoredMakeWithdrawal":::

<span data-ttu-id="ae1dd-224">添加的方法是 `protected`，这意味着只能从派生类中调用它。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-224">The added method is `protected`, which means that it can be called only from derived classes.</span></span> <span data-ttu-id="ae1dd-225">该声明会阻止其他客户端调用该方法。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-225">That declaration prevents other clients from calling the method.</span></span> <span data-ttu-id="ae1dd-226">它还是 `virtual` 的，因此派生类可以更改行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-226">It's also `virtual` so that derived classes can change the behavior.</span></span> <span data-ttu-id="ae1dd-227">返回类型为 `Transaction?`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-227">The return type is a `Transaction?`.</span></span> <span data-ttu-id="ae1dd-228">`?` 批注指示该方法可能返回 `null`。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-228">The `?` annotation indicates that the method may return `null`.</span></span> <span data-ttu-id="ae1dd-229">在 `LineOfCreditAccount` 中添加以下实现，以在超过取款限额时收取费用：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-229">Add the following implementation in the `LineOfCreditAccount` to charge a fee when the withdrawal limit is exceeded:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="AddOverdraftFee":::

<span data-ttu-id="ae1dd-230">当帐户透支时，该重写将返回费用交易。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-230">The override returns a fee transaction when the account is overdrawn.</span></span> <span data-ttu-id="ae1dd-231">如果取款未超出限额，则该方法将返回 `null` 交易。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-231">If the withdrawal doesn't go over the limit, the method returns a `null` transaction.</span></span> <span data-ttu-id="ae1dd-232">这表明不收取任何费用。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-232">That indicates there's no fee.</span></span> <span data-ttu-id="ae1dd-233">通过将以下代码添加到 `Program` 类中的 `Main` 方法来测试这些更改：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-233">Test these changes by adding the following code to your `Main` method in the `Program` class:</span></span>

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="TestLineOfCredit":::

<span data-ttu-id="ae1dd-234">运行该程序，并检查结果。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-234">Run the program, and check the results.</span></span>

## <a name="summary"></a><span data-ttu-id="ae1dd-235">摘要</span><span class="sxs-lookup"><span data-stu-id="ae1dd-235">Summary</span></span>

<span data-ttu-id="ae1dd-236">如果遇到问题，可以在 [GitHub 存储库](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/object-oriented-programming)中查看本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-236">If you got stuck, you can see the source for this tutorial [in our GitHub repo](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/object-oriented-programming).</span></span>

<span data-ttu-id="ae1dd-237">本教程演示了面向对象的编程中使用的多种技术：</span><span class="sxs-lookup"><span data-stu-id="ae1dd-237">This tutorial demonstrated many of the techniques used in Object-Oriented programming:</span></span>

- <span data-ttu-id="ae1dd-238">为每个不同的帐户类型定义了类时，你使用了抽象。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-238">You used *Abstraction* when you defined classes for each of the different account types.</span></span> <span data-ttu-id="ae1dd-239">这些类描述了该帐户类型的行为。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-239">Those classes described the behavior for that type of account.</span></span>
- <span data-ttu-id="ae1dd-240">在每个类中将许多详细信息保留为 `private` 时，你使用了封装。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-240">You used *Encapsulation* when you kept many details `private` in each class.</span></span>
- <span data-ttu-id="ae1dd-241">使用已在 `BankAccount` 类中创建的实现来保存代码时，你使用了继承。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-241">You used *Inheritance* when you leveraged the implementation already created in the `BankAccount` class to save code.</span></span>
- <span data-ttu-id="ae1dd-242">创建 `virtual` 方法，派生类可以重写它们来创建该帐户类型的特定行为时，你使用了多形性。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-242">You used *Polymorphism* when you created `virtual` methods that derived classes could override to create specific behavior for that account type.</span></span>

<span data-ttu-id="ae1dd-243">恭喜，你已完成我们的所有 C# 简介教程。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-243">Congratulations, you've finished all of our introduction to C# tutorials.</span></span> <span data-ttu-id="ae1dd-244">若要了解详细信息，请继续学习更多[教程](introduction-to-classes.md)。</span><span class="sxs-lookup"><span data-stu-id="ae1dd-244">To learn more, try more of our [tutorials](introduction-to-classes.md).</span></span>
