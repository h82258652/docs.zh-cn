---
title: 类和对象 - C# 简介教程
description: 创建首个 C# 程序，并探索面向对象的概念
ms.date: 10/11/2017
ms.custom: mvc
ms.openlocfilehash: 2e3311d30c97ecda8531a8e24219e03c37be3361
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494709"
---
# <a name="explore-object-oriented-programming-with-classes-and-objects"></a><span data-ttu-id="5a400-103">使用类和对象探索面向对象的编程</span><span class="sxs-lookup"><span data-stu-id="5a400-103">Explore object oriented programming with classes and objects</span></span>

<span data-ttu-id="5a400-104">在本教程中，你将生成一个控制台应用程序，并了解 C# 语言中的面向对象的基本功能。</span><span class="sxs-lookup"><span data-stu-id="5a400-104">In this tutorial, you'll build a console application and see the basic object-oriented features that are part of the C# language.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a400-105">先决条件</span><span class="sxs-lookup"><span data-stu-id="5a400-105">Prerequisites</span></span>

<span data-ttu-id="5a400-106">本教程要求安装一台虚拟机，以用于本地开发。</span><span class="sxs-lookup"><span data-stu-id="5a400-106">The tutorial expects that you have a machine set up for local development.</span></span> <span data-ttu-id="5a400-107">在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="5a400-107">On Windows, Linux, or macOS, you can use the .NET CLI to create, build, and run applications.</span></span> <span data-ttu-id="5a400-108">在 Windows 上，可以使用 Visual Studio 2019。</span><span class="sxs-lookup"><span data-stu-id="5a400-108">On Windows, you can use Visual Studio 2019.</span></span> <span data-ttu-id="5a400-109">有关创建说明，请参阅[创建本地环境](../../tour-of-csharp/tutorials/local-environment.md)。</span><span class="sxs-lookup"><span data-stu-id="5a400-109">For setup instructions, see [Set up your local environment](../../tour-of-csharp/tutorials/local-environment.md).</span></span>

## <a name="create-your-application"></a><span data-ttu-id="5a400-110">创建应用程序</span><span class="sxs-lookup"><span data-stu-id="5a400-110">Create your application</span></span>

<span data-ttu-id="5a400-111">使用终端窗口，创建名为 classes 的目录。</span><span class="sxs-lookup"><span data-stu-id="5a400-111">Using a terminal window, create a directory named *classes*.</span></span> <span data-ttu-id="5a400-112">可以在其中生成应用程序。</span><span class="sxs-lookup"><span data-stu-id="5a400-112">You'll build your application there.</span></span> <span data-ttu-id="5a400-113">将此目录更改为当前目录，并在控制台窗口中键入 `dotnet new console`。</span><span class="sxs-lookup"><span data-stu-id="5a400-113">Change to that directory and type `dotnet new console` in the console window.</span></span> <span data-ttu-id="5a400-114">此命令可创建应用程序。</span><span class="sxs-lookup"><span data-stu-id="5a400-114">This command creates your application.</span></span> <span data-ttu-id="5a400-115">打开 Program.cs。</span><span class="sxs-lookup"><span data-stu-id="5a400-115">Open *Program.cs*.</span></span> <span data-ttu-id="5a400-116">应如下所示：</span><span class="sxs-lookup"><span data-stu-id="5a400-116">It should look like this:</span></span>

```csharp
using System;

namespace classes
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

<span data-ttu-id="5a400-117">在本教程中，将要新建表示银行帐户的类型。</span><span class="sxs-lookup"><span data-stu-id="5a400-117">In this tutorial, you're going to create new types that represent a bank account.</span></span> <span data-ttu-id="5a400-118">通常情况下，开发者都会在不同的文本文件中定义每个类。</span><span class="sxs-lookup"><span data-stu-id="5a400-118">Typically developers define each class in a different text file.</span></span> <span data-ttu-id="5a400-119">这样可以更轻松地管理不断增大的程序。</span><span class="sxs-lookup"><span data-stu-id="5a400-119">That makes it easier to manage as a program grows in size.</span></span> <span data-ttu-id="5a400-120">在 classes 目录中，新建名为 BankAccount.cs 的文件。</span><span class="sxs-lookup"><span data-stu-id="5a400-120">Create a new file named *BankAccount.cs* in the *classes* directory.</span></span>

<span data-ttu-id="5a400-121">此文件包含“银行帐户”定义。</span><span class="sxs-lookup"><span data-stu-id="5a400-121">This file will contain the definition of a \***bank account** _.</span></span> <span data-ttu-id="5a400-122">面向对象的编程通过创建类_\*\*\*形式的类型来组织代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-122">Object Oriented programming organizes code by creating types in the form of _\*_classes_\*\*.</span></span> <span data-ttu-id="5a400-123">这些类包含表示特定实体的代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-123">These classes contain the code that represents a specific entity.</span></span> <span data-ttu-id="5a400-124">`BankAccount` 类表示银行帐户。</span><span class="sxs-lookup"><span data-stu-id="5a400-124">The `BankAccount` class represents a bank account.</span></span> <span data-ttu-id="5a400-125">代码通过方法和属性实现特定操作。</span><span class="sxs-lookup"><span data-stu-id="5a400-125">The code implements specific operations through methods and properties.</span></span> <span data-ttu-id="5a400-126">在本教程中，银行帐户支持以下行为：</span><span class="sxs-lookup"><span data-stu-id="5a400-126">In this tutorial, the bank account supports this behavior:</span></span>

1. <span data-ttu-id="5a400-127">用一个 10 位数唯一标识银行帐户。</span><span class="sxs-lookup"><span data-stu-id="5a400-127">It has a 10-digit number that uniquely identifies the bank account.</span></span>
1. <span data-ttu-id="5a400-128">用字符串存储一个或多个所有者名称。</span><span class="sxs-lookup"><span data-stu-id="5a400-128">It has a string that stores the name or names of the owners.</span></span>
1. <span data-ttu-id="5a400-129">可以检索余额。</span><span class="sxs-lookup"><span data-stu-id="5a400-129">The balance can be retrieved.</span></span>
1. <span data-ttu-id="5a400-130">接受存款。</span><span class="sxs-lookup"><span data-stu-id="5a400-130">It accepts deposits.</span></span>
1. <span data-ttu-id="5a400-131">接受取款。</span><span class="sxs-lookup"><span data-stu-id="5a400-131">It accepts withdrawals.</span></span>
1. <span data-ttu-id="5a400-132">初始余额必须是正数。</span><span class="sxs-lookup"><span data-stu-id="5a400-132">The initial balance must be positive.</span></span>
1. <span data-ttu-id="5a400-133">取款后的余额不能是负数。</span><span class="sxs-lookup"><span data-stu-id="5a400-133">Withdrawals cannot result in a negative balance.</span></span>

## <a name="define-the-bank-account-type"></a><span data-ttu-id="5a400-134">定义银行帐户类型</span><span class="sxs-lookup"><span data-stu-id="5a400-134">Define the bank account type</span></span>

<span data-ttu-id="5a400-135">首先，创建定义此行为的类的基本设置。</span><span class="sxs-lookup"><span data-stu-id="5a400-135">You can start by creating the basics of a class that defines that behavior.</span></span> <span data-ttu-id="5a400-136">使用 File:New 命令创建新文件。</span><span class="sxs-lookup"><span data-stu-id="5a400-136">Create a new file using the **File:New** command.</span></span> <span data-ttu-id="5a400-137">将其命名为“BankAccount.cs”。</span><span class="sxs-lookup"><span data-stu-id="5a400-137">Name it *BankAccount.cs*.</span></span> <span data-ttu-id="5a400-138">将以下代码添加到 BankAccount.cs 文件：</span><span class="sxs-lookup"><span data-stu-id="5a400-138">Add the following code to your *BankAccount.cs* file:</span></span>

```csharp
using System;

namespace classes
{
    public class BankAccount
    {
        public string Number { get; }
        public string Owner { get; set; }
        public decimal Balance { get; }

        public void MakeDeposit(decimal amount, DateTime date, string note)
        {
        }

        public void MakeWithdrawal(decimal amount, DateTime date, string note)
        {
        }
    }
}
```

<span data-ttu-id="5a400-139">继续操作前，先来看看已经生成的内容。</span><span class="sxs-lookup"><span data-stu-id="5a400-139">Before going on, let's take a look at what you've built.</span></span>  <span data-ttu-id="5a400-140">借助 `namespace` 声明，可以按逻辑组织代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-140">The `namespace` declaration provides a way to logically organize your code.</span></span> <span data-ttu-id="5a400-141">由于本教程的篇幅较小，因此所有代码都将添加到一个命名空间中。</span><span class="sxs-lookup"><span data-stu-id="5a400-141">This tutorial is relatively small, so you'll put all the code in one namespace.</span></span>

<span data-ttu-id="5a400-142">`public class BankAccount` 定义要创建的类或类型。</span><span class="sxs-lookup"><span data-stu-id="5a400-142">`public class BankAccount` defines the class, or type, you are creating.</span></span> <span data-ttu-id="5a400-143">类声明后面 `{` 和 `}` 中的所有内容定义了类的状态和行为。</span><span class="sxs-lookup"><span data-stu-id="5a400-143">Everything inside the `{` and `}` that follows the class declaration defines the state and behavior of the class.</span></span> <span data-ttu-id="5a400-144">`BankAccount` 类有五个成员。</span><span class="sxs-lookup"><span data-stu-id="5a400-144">There are five \***members** _ of the `BankAccount` class.</span></span> <span data-ttu-id="5a400-145">前三个成员是属性。</span><span class="sxs-lookup"><span data-stu-id="5a400-145">The first three are _*_properties_*_.</span></span> <span data-ttu-id="5a400-146">属性是数据元素，可以包含强制执行验证或其他规则的代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-146">Properties are data elements and can have code that enforces validation or other rules.</span></span> <span data-ttu-id="5a400-147">最后两个是方法_\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="5a400-147">The last two are _\*_methods_\*\*.</span></span> <span data-ttu-id="5a400-148">方法是执行一个函数的代码块。</span><span class="sxs-lookup"><span data-stu-id="5a400-148">Methods are blocks of code that perform a single function.</span></span> <span data-ttu-id="5a400-149">读取每个成员的名称应该能够为自己或其他开发者提供了解类用途的足够信息。</span><span class="sxs-lookup"><span data-stu-id="5a400-149">Reading the names of each of the members should provide enough information for you or another developer to understand what the class does.</span></span>

## <a name="open-a-new-account"></a><span data-ttu-id="5a400-150">打开新帐户</span><span class="sxs-lookup"><span data-stu-id="5a400-150">Open a new account</span></span>

<span data-ttu-id="5a400-151">要实现的第一个功能是打开银行帐户。</span><span class="sxs-lookup"><span data-stu-id="5a400-151">The first feature to implement is to open a bank account.</span></span> <span data-ttu-id="5a400-152">打开帐户时，客户必须提供初始余额，以及此帐户的一个或多个所有者的相关信息。</span><span class="sxs-lookup"><span data-stu-id="5a400-152">When a customer opens an account, they must supply an initial balance, and information about the owner or owners of that account.</span></span>

<span data-ttu-id="5a400-153">新建 `BankAccount` 类型的对象意味着定义构造函数\*_来赋值。</span><span class="sxs-lookup"><span data-stu-id="5a400-153">Creating a new object of the `BankAccount` type means defining a \***constructor** _ that assigns those values.</span></span> <span data-ttu-id="5a400-154">构造函数_\*是与类同名的成员。</span><span class="sxs-lookup"><span data-stu-id="5a400-154">A _ *_constructor_*\* is a member that has the same name as the class.</span></span> <span data-ttu-id="5a400-155">用于初始化相应类类型的对象。</span><span class="sxs-lookup"><span data-stu-id="5a400-155">It is used to initialize objects of that class type.</span></span> <span data-ttu-id="5a400-156">将以下构造函数添加到 `BankAccount` 类型。</span><span class="sxs-lookup"><span data-stu-id="5a400-156">Add the following constructor to the `BankAccount` type.</span></span> <span data-ttu-id="5a400-157">将下面的代码放在 `MakeDeposit` 声明的上方：</span><span class="sxs-lookup"><span data-stu-id="5a400-157">Place the following code above the declaration of `MakeDeposit`:</span></span>

```csharp
public BankAccount(string name, decimal initialBalance)
{
    this.Owner = name;
    this.Balance = initialBalance;
}
```

<span data-ttu-id="5a400-158">构造函数是在使用 [`new`](../../language-reference/operators/new-operator.md) 创建对象时进行调用。</span><span class="sxs-lookup"><span data-stu-id="5a400-158">Constructors are called when you create an object using [`new`](../../language-reference/operators/new-operator.md).</span></span> <span data-ttu-id="5a400-159">将 Program.cs 中的代码行 `Console.WriteLine("Hello World!");` 替换为以下代码行（将 `<name>` 替换为自己的名称）：</span><span class="sxs-lookup"><span data-stu-id="5a400-159">Replace the line `Console.WriteLine("Hello World!");` in *Program.cs* with the following code (replace `<name>` with your name):</span></span>

```csharp
var account = new BankAccount("<name>", 1000);
Console.WriteLine($"Account {account.Number} was created for {account.Owner} with {account.Balance} initial balance.");
```

<span data-ttu-id="5a400-160">我们来运行到目前为止已构建的内容。</span><span class="sxs-lookup"><span data-stu-id="5a400-160">Let's run what you've built so far.</span></span> <span data-ttu-id="5a400-161">如果使用的是 Visual Studio，请在“调试”菜单中选择“启动而不调试” 。</span><span class="sxs-lookup"><span data-stu-id="5a400-161">If you're using Visual Studio, Select **Start without debugging** from the **Debug** menu.</span></span> <span data-ttu-id="5a400-162">如果使用的是命令行，请在创建项目的目录中键入 `dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="5a400-162">If you're using a command line, type `dotnet run` in the directory where you've created your project.</span></span>

<span data-ttu-id="5a400-163">有没有注意到帐号为空？</span><span class="sxs-lookup"><span data-stu-id="5a400-163">Did you notice that the account number is blank?</span></span> <span data-ttu-id="5a400-164">是时候解决这个问题了。</span><span class="sxs-lookup"><span data-stu-id="5a400-164">It's time to fix that.</span></span> <span data-ttu-id="5a400-165">帐号应在构造对象时分配。</span><span class="sxs-lookup"><span data-stu-id="5a400-165">The account number should be assigned when the object is constructed.</span></span> <span data-ttu-id="5a400-166">但不得由调用方负责创建。</span><span class="sxs-lookup"><span data-stu-id="5a400-166">But it shouldn't be the responsibility of the caller to create it.</span></span> <span data-ttu-id="5a400-167">`BankAccount` 类代码应了解如何分配新帐号。</span><span class="sxs-lookup"><span data-stu-id="5a400-167">The `BankAccount` class code should know how to assign new account numbers.</span></span>  <span data-ttu-id="5a400-168">这样做的简单方法是从一个 10 位数开始。</span><span class="sxs-lookup"><span data-stu-id="5a400-168">A simple way to do this is to start with a 10-digit number.</span></span> <span data-ttu-id="5a400-169">帐号随每个新建的帐户而递增。</span><span class="sxs-lookup"><span data-stu-id="5a400-169">Increment it when each new account is created.</span></span> <span data-ttu-id="5a400-170">最后，在构造对象时，存储当前的帐号。</span><span class="sxs-lookup"><span data-stu-id="5a400-170">Finally, store the current account number when an object is constructed.</span></span>

<span data-ttu-id="5a400-171">将成员声明添加到 `BankAccount` 类。</span><span class="sxs-lookup"><span data-stu-id="5a400-171">Add a member declaration to the `BankAccount` class.</span></span> <span data-ttu-id="5a400-172">将以下代码行放在 `BankAccount` 类开头的左括号 `{` 后面：</span><span class="sxs-lookup"><span data-stu-id="5a400-172">Place the following line of code after the opening brace `{` at the beginning of the `BankAccount` class:</span></span>

```csharp
private static int accountNumberSeed = 1234567890;
```

<span data-ttu-id="5a400-173">此为数据成员。</span><span class="sxs-lookup"><span data-stu-id="5a400-173">This is a data member.</span></span> <span data-ttu-id="5a400-174">它是 `private`，这意味着只能通过 `BankAccount` 类中的代码访问它。</span><span class="sxs-lookup"><span data-stu-id="5a400-174">It's `private`, which means it can only be accessed by code inside the `BankAccount` class.</span></span> <span data-ttu-id="5a400-175">这是一种分离公共责任（如拥有帐号）与私有实现（如何生成帐号）的方法。</span><span class="sxs-lookup"><span data-stu-id="5a400-175">It's a way of separating the public responsibilities (like having an account number) from the private implementation (how account numbers are generated).</span></span> <span data-ttu-id="5a400-176">它也是 `static`，这意味着它由所有 `BankAccount` 对象共享。</span><span class="sxs-lookup"><span data-stu-id="5a400-176">It is also `static`, which means it is shared by all of the `BankAccount` objects.</span></span> <span data-ttu-id="5a400-177">非静态变量的值对于 `BankAccount` 对象的每个实例是唯一的。</span><span class="sxs-lookup"><span data-stu-id="5a400-177">The value of a non-static variable is unique to each instance of the `BankAccount` object.</span></span> <span data-ttu-id="5a400-178">将下面两行代码添加到构造函数，以分配帐号。</span><span class="sxs-lookup"><span data-stu-id="5a400-178">Add the following two lines to the constructor to assign the account number.</span></span> <span data-ttu-id="5a400-179">将它们放在 `this.Balance = initialBalance` 行后面：</span><span class="sxs-lookup"><span data-stu-id="5a400-179">Place them after the line that says `this.Balance = initialBalance`:</span></span>

```csharp
this.Number = accountNumberSeed.ToString();
accountNumberSeed++;
```

<span data-ttu-id="5a400-180">键入 `dotnet run` 看看结果如何。</span><span class="sxs-lookup"><span data-stu-id="5a400-180">Type `dotnet run` to see the results.</span></span>

## <a name="create-deposits-and-withdrawals"></a><span data-ttu-id="5a400-181">创建存款和取款</span><span class="sxs-lookup"><span data-stu-id="5a400-181">Create deposits and withdrawals</span></span>

<span data-ttu-id="5a400-182">银行帐户类必须接受存款和取款，才能正常运行。</span><span class="sxs-lookup"><span data-stu-id="5a400-182">Your bank account class needs to accept deposits and withdrawals to work correctly.</span></span> <span data-ttu-id="5a400-183">接下来，将为银行帐户创建每笔交易日记，实现存款和取款。</span><span class="sxs-lookup"><span data-stu-id="5a400-183">Let's implement deposits and withdrawals by creating a journal of every transaction for the account.</span></span> <span data-ttu-id="5a400-184">与仅更新每笔交易余额相比，这样做有一些优点。</span><span class="sxs-lookup"><span data-stu-id="5a400-184">That has a few advantages over simply updating the balance on each transaction.</span></span> <span data-ttu-id="5a400-185">历史记录可用于审核所有交易，并管理每日余额。</span><span class="sxs-lookup"><span data-stu-id="5a400-185">The history can be used to audit all transactions and manage daily balances.</span></span> <span data-ttu-id="5a400-186">通过在需要时根据所有交易的历史记录计算余额，单笔交易中修正的任何错误将会在下次计算余额时得到正确体现。</span><span class="sxs-lookup"><span data-stu-id="5a400-186">By computing the balance from the history of all transactions when needed, any errors in a single transaction that are fixed will be correctly reflected in the balance on the next computation.</span></span>

<span data-ttu-id="5a400-187">接下来，先新建表示交易的类型。</span><span class="sxs-lookup"><span data-stu-id="5a400-187">Let's start by creating a new type to represent a transaction.</span></span> <span data-ttu-id="5a400-188">这是一个没有任何责任的简单类型。</span><span class="sxs-lookup"><span data-stu-id="5a400-188">This is a simple type that doesn't have any responsibilities.</span></span> <span data-ttu-id="5a400-189">但需要有多个属性。</span><span class="sxs-lookup"><span data-stu-id="5a400-189">It needs a few properties.</span></span> <span data-ttu-id="5a400-190">新建名为 Transaction.cs 的文件。</span><span class="sxs-lookup"><span data-stu-id="5a400-190">Create a new file named *Transaction.cs*.</span></span> <span data-ttu-id="5a400-191">向新文件添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="5a400-191">Add the following code to it:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/Transaction.cs":::

<span data-ttu-id="5a400-192">现在，将 `Transaction` 对象的 <xref:System.Collections.Generic.List%601> 添加到 `BankAccount` 类中。</span><span class="sxs-lookup"><span data-stu-id="5a400-192">Now, let's add a <xref:System.Collections.Generic.List%601> of `Transaction` objects to the `BankAccount` class.</span></span> <span data-ttu-id="5a400-193">将以下声明放在 BankAccount.cs 文件中的构造函数后面：</span><span class="sxs-lookup"><span data-stu-id="5a400-193">Add the following declaration after the constructor in your *BankAccount.cs* file:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="TransactionDeclaration":::

<span data-ttu-id="5a400-194"><xref:System.Collections.Generic.List%601> 类要求导入不同的命名空间。</span><span class="sxs-lookup"><span data-stu-id="5a400-194">The <xref:System.Collections.Generic.List%601> class requires you to import a different namespace.</span></span> <span data-ttu-id="5a400-195">在 BankAccount.cs 的开头，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="5a400-195">Add the following at the beginning of *BankAccount.cs*:</span></span>

```csharp
using System.Collections.Generic;
```

<span data-ttu-id="5a400-196">现在，让我们来正确计算 `Balance`。</span><span class="sxs-lookup"><span data-stu-id="5a400-196">Now, let's correctly compute the `Balance`.</span></span> <span data-ttu-id="5a400-197">可以通过对所有交易的值进行求和来计算当前余额。</span><span class="sxs-lookup"><span data-stu-id="5a400-197">The current balance can be found by summing the values of all transactions.</span></span> <span data-ttu-id="5a400-198">由于当前代码，你只能计算出帐户的初始余额，因此必须更新 `Balance` 属性。</span><span class="sxs-lookup"><span data-stu-id="5a400-198">As the code is currently, you can only get the initial balance of the account, so you'll have to update the `Balance` property.</span></span> <span data-ttu-id="5a400-199">将 BankAccount.cs 中的 `public decimal Balance { get; }` 行替换为以下代码：</span><span class="sxs-lookup"><span data-stu-id="5a400-199">Replace the line `public decimal Balance { get; }` in *BankAccount.cs* with the following code:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="BalanceComputation":::

<span data-ttu-id="5a400-200">此示例反映了属性的一个重要方面。</span><span class="sxs-lookup"><span data-stu-id="5a400-200">This example shows an important aspect of ***properties***.</span></span> <span data-ttu-id="5a400-201">现在，可以在其他程序员要求获取余额时计算值。</span><span class="sxs-lookup"><span data-stu-id="5a400-201">You're now computing the balance when another programmer asks for the value.</span></span> <span data-ttu-id="5a400-202">计算会枚举所有交易，总和即为当前余额。</span><span class="sxs-lookup"><span data-stu-id="5a400-202">Your computation enumerates all transactions, and provides the sum as the current balance.</span></span>

<span data-ttu-id="5a400-203">接下来，实现 `MakeDeposit` 和 `MakeWithdrawal` 方法。</span><span class="sxs-lookup"><span data-stu-id="5a400-203">Next, implement the `MakeDeposit` and `MakeWithdrawal` methods.</span></span> <span data-ttu-id="5a400-204">这些方法将强制执行最后两条规则：初始余额必须为正数，且取款后的余额不能是负数。</span><span class="sxs-lookup"><span data-stu-id="5a400-204">These methods will enforce the final two rules: that the initial balance must be positive, and that any withdrawal must not create a negative balance.</span></span>

<span data-ttu-id="5a400-205">这就引入了异常的概念。</span><span class="sxs-lookup"><span data-stu-id="5a400-205">This introduces the concept of ***exceptions***.</span></span> <span data-ttu-id="5a400-206">指明方法无法成功完成工作的标准方法是抛出异常。</span><span class="sxs-lookup"><span data-stu-id="5a400-206">The standard way of indicating that a method cannot complete its work successfully is to throw an exception.</span></span> <span data-ttu-id="5a400-207">异常类型及其关联消息描述了错误。</span><span class="sxs-lookup"><span data-stu-id="5a400-207">The type of exception and the message associated with it describe the error.</span></span> <span data-ttu-id="5a400-208">在此示例中，如果存款金额为负数，`MakeDeposit` 方法会抛出异常。</span><span class="sxs-lookup"><span data-stu-id="5a400-208">Here, the `MakeDeposit` method throws an exception if the amount of the deposit is negative.</span></span> <span data-ttu-id="5a400-209">如果取款金额为负数，或取款后的余额为负数，`MakeWithdrawal` 方法会抛出异常。</span><span class="sxs-lookup"><span data-stu-id="5a400-209">The `MakeWithdrawal` method throws an exception if the withdrawal amount is negative, or if applying the withdrawal results in a negative balance.</span></span> <span data-ttu-id="5a400-210">将以下代码添加到 `allTransactions` 列表的声明后面：</span><span class="sxs-lookup"><span data-stu-id="5a400-210">Add the following code after the declaration of the `allTransactions` list:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="DepositAndWithdrawal":::

<span data-ttu-id="5a400-211">[`throw`](../../language-reference/keywords/throw.md) 语句将引发异常。</span><span class="sxs-lookup"><span data-stu-id="5a400-211">The [`throw`](../../language-reference/keywords/throw.md) statement **throws** an exception.</span></span> <span data-ttu-id="5a400-212">当前块执行结束，将控制权移交给在调用堆栈中发现的第一个匹配的 `catch` 块。</span><span class="sxs-lookup"><span data-stu-id="5a400-212">Execution of the current block ends, and control transfers to the first matching `catch` block found in the call stack.</span></span> <span data-ttu-id="5a400-213">添加 `catch` 块可以稍后再测试一下此代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-213">You'll add a `catch` block to test this code a little later on.</span></span>

<span data-ttu-id="5a400-214">构造函数应进行一处更改，更改为添加初始交易，而不是直接更新余额。</span><span class="sxs-lookup"><span data-stu-id="5a400-214">The constructor should get one change so that it adds an initial transaction, rather than updating the balance directly.</span></span> <span data-ttu-id="5a400-215">由于已编写 `MakeDeposit` 方法，因此通过构造函数调用它。</span><span class="sxs-lookup"><span data-stu-id="5a400-215">Since you already wrote the `MakeDeposit` method, call it from your constructor.</span></span> <span data-ttu-id="5a400-216">完成的构造函数应如下所示：</span><span class="sxs-lookup"><span data-stu-id="5a400-216">The finished constructor should look like this:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="Constructor":::

<span data-ttu-id="5a400-217"><xref:System.DateTime.Now?displayProperty=nameWithType> 是返回当前日期和时间的属性。</span><span class="sxs-lookup"><span data-stu-id="5a400-217"><xref:System.DateTime.Now?displayProperty=nameWithType> is a property that returns the current date and time.</span></span> <span data-ttu-id="5a400-218">在创建新 `BankAccount` 的代码后面，在 `Main` 方法中添加几个存款和取款，对此进行测试：</span><span class="sxs-lookup"><span data-stu-id="5a400-218">Test this by adding a few deposits and withdrawals in your `Main` method, following the code that creates a new `BankAccount`:</span></span>

```csharp
account.MakeWithdrawal(500, DateTime.Now, "Rent payment");
Console.WriteLine(account.Balance);
account.MakeDeposit(100, DateTime.Now, "Friend paid me back");
Console.WriteLine(account.Balance);
```

<span data-ttu-id="5a400-219">接下来，尝试创建初始余额为负数的帐户，测试能否捕获到错误条件。</span><span class="sxs-lookup"><span data-stu-id="5a400-219">Next, test that you are catching error conditions by trying to create an account with a negative balance.</span></span> <span data-ttu-id="5a400-220">在刚刚添加的上述代码后面，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="5a400-220">Add the following code after the preceding code you just added:</span></span>

```csharp
// Test that the initial balances must be positive.
try
{
    var invalidAccount = new BankAccount("invalid", -55);
}
catch (ArgumentOutOfRangeException e)
{
    Console.WriteLine("Exception caught creating account with negative balance");
    Console.WriteLine(e.ToString());
}
```

<span data-ttu-id="5a400-221">使用 [`try` 和 `catch` 语句](../../language-reference/keywords/try-catch.md)，标记可能会引发异常的代码块，并捕获预期错误。</span><span class="sxs-lookup"><span data-stu-id="5a400-221">You use the [`try` and `catch` statements](../../language-reference/keywords/try-catch.md) to mark a block of code that may throw exceptions and to catch those errors that you expect.</span></span> <span data-ttu-id="5a400-222">可以使用相同的技术，测试代码能否在取款后余额为负数时引发异常。</span><span class="sxs-lookup"><span data-stu-id="5a400-222">You can use the same technique to test the code that throws an exception for a negative balance.</span></span> <span data-ttu-id="5a400-223">在 `Main` 方法的末尾添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="5a400-223">Add the following code at the end of your `Main` method:</span></span>

```csharp
// Test for a negative balance.
try
{
    account.MakeWithdrawal(750, DateTime.Now, "Attempt to overdraw");
}
catch (InvalidOperationException e)
{
    Console.WriteLine("Exception caught trying to overdraw");
    Console.WriteLine(e.ToString());
}
```

<span data-ttu-id="5a400-224">保存此文件，并键入 `dotnet run`，试运行看看。</span><span class="sxs-lookup"><span data-stu-id="5a400-224">Save the file and type `dotnet run` to try it.</span></span>

## <a name="challenge---log-all-transactions"></a><span data-ttu-id="5a400-225">挑战 - 记录所有交易</span><span class="sxs-lookup"><span data-stu-id="5a400-225">Challenge - log all transactions</span></span>

<span data-ttu-id="5a400-226">为了完成本教程，可以编写 `GetAccountHistory` 方法，为交易历史记录创建 `string`。</span><span class="sxs-lookup"><span data-stu-id="5a400-226">To finish this tutorial, you can write the `GetAccountHistory` method that creates a `string` for the transaction history.</span></span> <span data-ttu-id="5a400-227">将此方法添加到 `BankAccount` 类型中：</span><span class="sxs-lookup"><span data-stu-id="5a400-227">Add this method to the `BankAccount` type:</span></span>

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="History":::

<span data-ttu-id="5a400-228">上面的代码使用 <xref:System.Text.StringBuilder> 类，设置包含每个交易行的字符串的格式。</span><span class="sxs-lookup"><span data-stu-id="5a400-228">This uses the <xref:System.Text.StringBuilder> class to format a string that contains one line for each transaction.</span></span> <span data-ttu-id="5a400-229">在前面的教程中，也遇到过字符串格式设置代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-229">You've seen the string formatting code earlier in these tutorials.</span></span> <span data-ttu-id="5a400-230">新增的一个字符为 `\t`。</span><span class="sxs-lookup"><span data-stu-id="5a400-230">One new character is `\t`.</span></span> <span data-ttu-id="5a400-231">这用于插入选项卡，从而设置输出格式。</span><span class="sxs-lookup"><span data-stu-id="5a400-231">That inserts a tab to format the output.</span></span>

<span data-ttu-id="5a400-232">添加以下代码行，在 Program.cs 中对它进行测试：</span><span class="sxs-lookup"><span data-stu-id="5a400-232">Add this line to test it in *Program.cs*:</span></span>

```csharp
Console.WriteLine(account.GetAccountHistory());
```

<span data-ttu-id="5a400-233">运行程序以查看结果。</span><span class="sxs-lookup"><span data-stu-id="5a400-233">Run your program to see the results.</span></span>

## <a name="next-steps"></a><span data-ttu-id="5a400-234">后续步骤</span><span class="sxs-lookup"><span data-stu-id="5a400-234">Next steps</span></span>

<span data-ttu-id="5a400-235">如果遇到问题，可以在 [GitHub 存储库](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/introduction-to-classes)中查看本教程的源代码。</span><span class="sxs-lookup"><span data-stu-id="5a400-235">If you got stuck, you can see the source for this tutorial [in our GitHub repo](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/introduction-to-classes).</span></span>

<span data-ttu-id="5a400-236">可继续学习[面向对象的编程](object-oriented-programming.md)教程。</span><span class="sxs-lookup"><span data-stu-id="5a400-236">You can continue with the [object oriented programming](object-oriented-programming.md) tutorial.</span></span>

<span data-ttu-id="5a400-237">若要详细了解这些概念，请参阅下列文章：</span><span class="sxs-lookup"><span data-stu-id="5a400-237">You can learn more about these concepts in these articles:</span></span>

- [<span data-ttu-id="5a400-238">If 和 else 语句</span><span class="sxs-lookup"><span data-stu-id="5a400-238">If and else statement</span></span>](../../language-reference/keywords/if-else.md)
- [<span data-ttu-id="5a400-239">While 语句</span><span class="sxs-lookup"><span data-stu-id="5a400-239">While statement</span></span>](../../language-reference/keywords/while.md)
- [<span data-ttu-id="5a400-240">Do 语句</span><span class="sxs-lookup"><span data-stu-id="5a400-240">Do statement</span></span>](../../language-reference/keywords/do.md)
- [<span data-ttu-id="5a400-241">For 语句</span><span class="sxs-lookup"><span data-stu-id="5a400-241">For statement</span></span>](../../language-reference/keywords/for.md)
