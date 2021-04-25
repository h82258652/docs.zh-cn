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
# <a name="explore-object-oriented-programming-with-classes-and-objects"></a>使用类和对象探索面向对象的编程

在本教程中，你将生成一个控制台应用程序，并了解 C# 语言中的面向对象的基本功能。

## <a name="prerequisites"></a>先决条件

本教程要求安装一台虚拟机，以用于本地开发。 在 Windows、Linux 或 macOS 上，可以使用 .NET CLI 创建、生成和运行应用程序。 在 Windows 上，可以使用 Visual Studio 2019。 有关创建说明，请参阅[创建本地环境](../../tour-of-csharp/tutorials/local-environment.md)。

## <a name="create-your-application"></a>创建应用程序

使用终端窗口，创建名为 classes 的目录。 可以在其中生成应用程序。 将此目录更改为当前目录，并在控制台窗口中键入 `dotnet new console`。 此命令可创建应用程序。 打开 Program.cs。 应如下所示：

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

在本教程中，将要新建表示银行帐户的类型。 通常情况下，开发者都会在不同的文本文件中定义每个类。 这样可以更轻松地管理不断增大的程序。 在 classes 目录中，新建名为 BankAccount.cs 的文件。

此文件包含“银行帐户”定义。 面向对象的编程通过创建类_***形式的类型来组织代码。 这些类包含表示特定实体的代码。 `BankAccount` 类表示银行帐户。 代码通过方法和属性实现特定操作。 在本教程中，银行帐户支持以下行为：

1. 用一个 10 位数唯一标识银行帐户。
1. 用字符串存储一个或多个所有者名称。
1. 可以检索余额。
1. 接受存款。
1. 接受取款。
1. 初始余额必须是正数。
1. 取款后的余额不能是负数。

## <a name="define-the-bank-account-type"></a>定义银行帐户类型

首先，创建定义此行为的类的基本设置。 使用 File:New 命令创建新文件。 将其命名为“BankAccount.cs”。 将以下代码添加到 BankAccount.cs 文件：

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

继续操作前，先来看看已经生成的内容。  借助 `namespace` 声明，可以按逻辑组织代码。 由于本教程的篇幅较小，因此所有代码都将添加到一个命名空间中。

`public class BankAccount` 定义要创建的类或类型。 类声明后面 `{` 和 `}` 中的所有内容定义了类的状态和行为。 `BankAccount` 类有五个成员。 前三个成员是属性。 属性是数据元素，可以包含强制执行验证或其他规则的代码。 最后两个是方法_***。 方法是执行一个函数的代码块。 读取每个成员的名称应该能够为自己或其他开发者提供了解类用途的足够信息。

## <a name="open-a-new-account"></a>打开新帐户

要实现的第一个功能是打开银行帐户。 打开帐户时，客户必须提供初始余额，以及此帐户的一个或多个所有者的相关信息。

新建 `BankAccount` 类型的对象意味着定义构造函数*_来赋值。 构造函数_*是与类同名的成员。 用于初始化相应类类型的对象。 将以下构造函数添加到 `BankAccount` 类型。 将下面的代码放在 `MakeDeposit` 声明的上方：

```csharp
public BankAccount(string name, decimal initialBalance)
{
    this.Owner = name;
    this.Balance = initialBalance;
}
```

构造函数是在使用 [`new`](../../language-reference/operators/new-operator.md) 创建对象时进行调用。 将 Program.cs 中的代码行 `Console.WriteLine("Hello World!");` 替换为以下代码行（将 `<name>` 替换为自己的名称）：

```csharp
var account = new BankAccount("<name>", 1000);
Console.WriteLine($"Account {account.Number} was created for {account.Owner} with {account.Balance} initial balance.");
```

我们来运行到目前为止已构建的内容。 如果使用的是 Visual Studio，请在“调试”菜单中选择“启动而不调试” 。 如果使用的是命令行，请在创建项目的目录中键入 `dotnet run`。

有没有注意到帐号为空？ 是时候解决这个问题了。 帐号应在构造对象时分配。 但不得由调用方负责创建。 `BankAccount` 类代码应了解如何分配新帐号。  这样做的简单方法是从一个 10 位数开始。 帐号随每个新建的帐户而递增。 最后，在构造对象时，存储当前的帐号。

将成员声明添加到 `BankAccount` 类。 将以下代码行放在 `BankAccount` 类开头的左括号 `{` 后面：

```csharp
private static int accountNumberSeed = 1234567890;
```

此为数据成员。 它是 `private`，这意味着只能通过 `BankAccount` 类中的代码访问它。 这是一种分离公共责任（如拥有帐号）与私有实现（如何生成帐号）的方法。 它也是 `static`，这意味着它由所有 `BankAccount` 对象共享。 非静态变量的值对于 `BankAccount` 对象的每个实例是唯一的。 将下面两行代码添加到构造函数，以分配帐号。 将它们放在 `this.Balance = initialBalance` 行后面：

```csharp
this.Number = accountNumberSeed.ToString();
accountNumberSeed++;
```

键入 `dotnet run` 看看结果如何。

## <a name="create-deposits-and-withdrawals"></a>创建存款和取款

银行帐户类必须接受存款和取款，才能正常运行。 接下来，将为银行帐户创建每笔交易日记，实现存款和取款。 与仅更新每笔交易余额相比，这样做有一些优点。 历史记录可用于审核所有交易，并管理每日余额。 通过在需要时根据所有交易的历史记录计算余额，单笔交易中修正的任何错误将会在下次计算余额时得到正确体现。

接下来，先新建表示交易的类型。 这是一个没有任何责任的简单类型。 但需要有多个属性。 新建名为 Transaction.cs 的文件。 向新文件添加以下代码：

:::code language="csharp" source="./snippets/introduction-to-classes/Transaction.cs":::

现在，将 `Transaction` 对象的 <xref:System.Collections.Generic.List%601> 添加到 `BankAccount` 类中。 将以下声明放在 BankAccount.cs 文件中的构造函数后面：

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="TransactionDeclaration":::

<xref:System.Collections.Generic.List%601> 类要求导入不同的命名空间。 在 BankAccount.cs 的开头，添加以下代码：

```csharp
using System.Collections.Generic;
```

现在，让我们来正确计算 `Balance`。 可以通过对所有交易的值进行求和来计算当前余额。 由于当前代码，你只能计算出帐户的初始余额，因此必须更新 `Balance` 属性。 将 BankAccount.cs 中的 `public decimal Balance { get; }` 行替换为以下代码：

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="BalanceComputation":::

此示例反映了属性的一个重要方面。 现在，可以在其他程序员要求获取余额时计算值。 计算会枚举所有交易，总和即为当前余额。

接下来，实现 `MakeDeposit` 和 `MakeWithdrawal` 方法。 这些方法将强制执行最后两条规则：初始余额必须为正数，且取款后的余额不能是负数。

这就引入了异常的概念。 指明方法无法成功完成工作的标准方法是抛出异常。 异常类型及其关联消息描述了错误。 在此示例中，如果存款金额为负数，`MakeDeposit` 方法会抛出异常。 如果取款金额为负数，或取款后的余额为负数，`MakeWithdrawal` 方法会抛出异常。 将以下代码添加到 `allTransactions` 列表的声明后面：

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="DepositAndWithdrawal":::

[`throw`](../../language-reference/keywords/throw.md) 语句将引发异常。 当前块执行结束，将控制权移交给在调用堆栈中发现的第一个匹配的 `catch` 块。 添加 `catch` 块可以稍后再测试一下此代码。

构造函数应进行一处更改，更改为添加初始交易，而不是直接更新余额。 由于已编写 `MakeDeposit` 方法，因此通过构造函数调用它。 完成的构造函数应如下所示：

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="Constructor":::

<xref:System.DateTime.Now?displayProperty=nameWithType> 是返回当前日期和时间的属性。 在创建新 `BankAccount` 的代码后面，在 `Main` 方法中添加几个存款和取款，对此进行测试：

```csharp
account.MakeWithdrawal(500, DateTime.Now, "Rent payment");
Console.WriteLine(account.Balance);
account.MakeDeposit(100, DateTime.Now, "Friend paid me back");
Console.WriteLine(account.Balance);
```

接下来，尝试创建初始余额为负数的帐户，测试能否捕获到错误条件。 在刚刚添加的上述代码后面，添加以下代码：

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

使用 [`try` 和 `catch` 语句](../../language-reference/keywords/try-catch.md)，标记可能会引发异常的代码块，并捕获预期错误。 可以使用相同的技术，测试代码能否在取款后余额为负数时引发异常。 在 `Main` 方法的末尾添加以下代码：

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

保存此文件，并键入 `dotnet run`，试运行看看。

## <a name="challenge---log-all-transactions"></a>挑战 - 记录所有交易

为了完成本教程，可以编写 `GetAccountHistory` 方法，为交易历史记录创建 `string`。 将此方法添加到 `BankAccount` 类型中：

:::code language="csharp" source="./snippets/introduction-to-classes/BankAccount.cs" id="History":::

上面的代码使用 <xref:System.Text.StringBuilder> 类，设置包含每个交易行的字符串的格式。 在前面的教程中，也遇到过字符串格式设置代码。 新增的一个字符为 `\t`。 这用于插入选项卡，从而设置输出格式。

添加以下代码行，在 Program.cs 中对它进行测试：

```csharp
Console.WriteLine(account.GetAccountHistory());
```

运行程序以查看结果。

## <a name="next-steps"></a>后续步骤

如果遇到问题，可以在 [GitHub 存储库](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/introduction-to-classes)中查看本教程的源代码。

可继续学习[面向对象的编程](object-oriented-programming.md)教程。

若要详细了解这些概念，请参阅下列文章：

- [If 和 else 语句](../../language-reference/keywords/if-else.md)
- [While 语句](../../language-reference/keywords/while.md)
- [Do 语句](../../language-reference/keywords/do.md)
- [For 语句](../../language-reference/keywords/for.md)
