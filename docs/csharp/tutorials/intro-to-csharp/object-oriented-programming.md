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
# <a name="object-oriented-programming-c"></a>面向对象的编程 (C#)

C# 是面向对象的编程语言。 面向对象编程的四项基本原则为：

- 抽象：将实体的相关特性和交互建模为类，以定义系统的抽象表示。
- 封装：隐藏对象的内部状态和功能，并仅允许通过一组公共函数进行访问。
- 继承：根据现有抽象创建新抽象的能力。
- 多形性：跨多个抽象以不同方式实现继承属性或方法的能力。

在前面的[类简介](introduction-to-classes.md) 教程中，我们介绍了抽象和封装。 `BankAccount` 类提供银行帐户这一概念的抽象。 你可以修改其实现，而不影响使用 `BankAccount` 类的任何代码。 `BankAccount` 和 `Transaction` 类都提供在代码中描述这些概念所需组件的封装。

在本教程中，你将扩展该应用程序以利用继承和多形性来添加新功能。  你还将利用在上一教程中学到的抽象和封装技术，向 `BankAccount`  类添加功能。

## <a name="create-different-types-of-accounts"></a>创建不同类型的帐户

生成此程序后，你将获取向其添加功能的请求。 在只有一种银行帐户类型的情况下，其效果良好。 随着时间的推移，需求会发生变化，因而请求了相关的帐户类型：

- 在每个月的月末获得利息的红利帐户。
- 余额可以为负，但存在余额时会产生每月利息的信用帐户。
- 以单笔存款开户且只能用于支付的预付礼品卡帐户。 可在每月初重新充值一次。

所有这些帐户都与前面的教程中定义的 `BankAccount` 类类似。 你可以复制该代码、重命名类并进行修改。 这种方法在短期内有效，但随着时间的推移，其效果会更为明显。 所有更改都将复制到所有受影响的类。

相反，你可以创建新的银行帐户类型，使其从上一教程中创建的 `BankAccount` 类继承方法和数据。 这些新类可以用每种类型所需的特定行为来扩展 `BankAccount` 类：

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

其中的每个类都从其共享的基类（ `BankAccount` 类）继承共享的行为。 为的每个派生类中的新增和不同功能编写实现。  这些派生类已具有 `BankAccount` 类中定义的所有行为。

最好在不同的源文件中创建每个新类。 在 [Visual Studio](https://visualstudio.com)中，可以右键单击项目，然后选择“添加类”以在新文件中添加新类。 在 [Visual Studio Code](https://code.visualstudio.com) 中，选择“文件”，然后选择“新建”以创建新的源文件。  在任一工具中，将文件命名为与类匹配：InterestEarningAccount.cs、LineOfCreditAccount.cs 和 GiftCardAccount.cs。  

如前一示例中所示创建类时，你会发现派生类都没有编译。 构造函数负责初始化对象。 派生类构造函数必须初始化派生类，并提供有关如何初始化派生类中所包含基类对象的说明。 一般无需任何额外的代码即可正常初始化。 `BankAccount` 类声明一个具有以下签名的公共构造函数：

```csharp
public BankAccount(string name, decimal initialBalance)
```

当你自行定义构造函数时，编译器不会生成默认构造函数。 这意味着每个派生类必须显式调用此构造函数。 声明可将自变量传递到基类构造函数的构造函数。  下面的代码显示了 `InterestEarningAccount` 的构造函数：

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="DerivedConstructor":::

此新构造函数的参数与基类构造函数的参数类型和名称匹配。 使用 `: base()` 语法来指示对基类构造函数的调用。 某些类定义多个构造函数，此语法让你可以选取调用的基类构造函数。 更新构造函数后，可以为每个派生类开发代码。 可以这样描述对新类的要求：

- 红利帐户：
  - 将获得月末余额 2% 的额度。
- 信用帐户：
  - 余额可以为负，但不能大于信用限额的绝对值。
  - 如果月末余额不为 0，每个月都会产生利息。
  - 将在超过信用限额的每次提款后收取费用。
- 礼品卡帐户：
  - 每月最后一天，可以充值一次指定的金额。

可以看到，这三种帐户类型都有一个在每月月末发生的操作。 但是，每种帐户类型负责不同的任务。 可以使用多态性来实现此代码。 在 `BankAccount` 类中创建单个 `virtual` 方法：

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="DeclareMonthEndTransactions":::

前面的代码演示如何使用 `virtual` 关键字在基类中声明一个方法，让派生类可以为该方法提供不同的实现。 在 `virtual` 方法种，任何派生类都可以选择重新实现。 派生类使用 `override` 关键字定义新的实现。 通常将其称为“重写基类实现”。 `virtual` 关键字指定派生类可以重写此行为。 还可以声明 `abstract` 方法，让派生类必须在其中重写此行为。 基类不提供 `abstract` 方法的实现。 接下来，需要为你创建的两个新类定义实现。 从 `InterestEarningAccount` 开始：

:::code language="csharp" source="./snippets/object-oriented-programming/InterestEarningAccount.cs" ID="ApplyMonthendInterest":::

将以下代码添加到 `LineOfCreditAccount`。 此代码会取余额的相反数，计算从该帐户提取的正利息：

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ApplyMonthendInterest":::

`GiftCardAccount` 类需要通过两项更改来实现其月末功能。 首先，将构造函数修改为包含每个月要充值的可选金额：

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="GiftCardAccountConstruction":::

构造函数为 `monthlyDeposit` 值提供默认值，因此调用方可以省略 `0` 以表示不进行每月存款。 接下来，如果已在构造函数中将 `PerformMonthEndTransactions` 方法设置为非零值，则重写该方法以添加每月存款：

:::code language="csharp" source="./snippets/object-oriented-programming/GiftCardAccount.cs" ID="AddMonthlyDeposit":::

重写将在构造函数中应用每月存款设置。 将以下代码添加到 `Main` 方法，以便为 `GiftCardAccount` 和 `InterestEarningAccount` 测试这些更改：

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="FirstTests":::

验证结果。 现在，为 `LineOfCreditAccount` 添加一组类似的测试代码：

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

添加前面的代码并运行程序时，你将看到类似于以下错误的内容：

```console
Unhandled exception. System.ArgumentOutOfRangeException: Amount of deposit must be positive (Parameter 'amount')
   at OOProgramming.BankAccount.MakeDeposit(Decimal amount, DateTime date, String note) in BankAccount.cs:line 42
   at OOProgramming.BankAccount..ctor(String name, Decimal initialBalance) in BankAccount.cs:line 31
   at OOProgramming.LineOfCreditAccount..ctor(String name, Decimal initialBalance) in LineOfCreditAccount.cs:line 9
   at OOProgramming.Program.Main(String[] args) in Program.cs:line 29
```

> [!NOTE]
> 实际输出包含项目文件夹的完整路径。 为简洁起见，省略了文件夹名称。 此外，根据你的代码格式，行号可能略有不同。

此代码会失败，因为 `BankAccount` 假设初始余额必须大于 0。 `BankAccount` 类固有的另一假设是余额不能为负。 相反，将拒绝透支帐户的任何提款。 这两个假设都需要更改。 信用帐户从 0 开始，一般情况下余额为负。 此外，如果客户借款量过大，将产生费用。 会接受该交易，只是会增加费用。 可以通过向 `BankAccount` 构造函数添加用于指定最小余额的可选参数来实现第一条规则。 默认为 `0`。 第二条规则需要允许派生类修改默认算法的机制。 在某种意义上，基类会“询问”派生类型在透支时应该怎么做。 默认行为是通过引发异常来拒绝交易。

首先，让我们添加包含可选 `minimumBalance` 参数的第二个构造函数。 此新构造函数会执行现有构造函数完成的所有操作。 此外，它还会设置最小余额属性。 可以复制现有构造函数的主体。 但这意味着将来需要更改两个位置。 相反，可以使用构造函数链接，让一个构造函数调用另一个构造函数。 下面的代码显示了两个构造函数和新的附加字段：

 :::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="ConstructorModifications":::

前面的代码演示了两种新方法。 首先，`minimumBalance` 字段被标记为 `readonly`。 这意味着构造对象之后不能更改值。 创建 `BankAccount` 后，`minimumBalance` 不可更改。 其次，采用两个参数的构造函数使用 `: this(name, initialBalance, 0) { }` 作为其实现。 `: this()` 表达式调用另一个构造函数（它具有三个参数）。 即使客户端代码可以从多个构造函数中进行选择，你也可以使用单个实现来初始化对象。

仅当初始余额大于 `0` 时，此实现才会调用 `MakeDeposit`。 这将保留存款必须为正的规则，同时允许信用帐户以 `0` 余额开户。

既然 `BankAccount` 类具有用于规定最小余额的只读字段，最终更改就是在 `MakeWithdrawal` 方法中将硬编码的 `0` 更改为 `minimumBalance`：

```csharp
if (Balance - amount < minimumBalance)
```

扩展 `BankAccount` 类后，可以修改 `LineOfCreditAccount` 构造函数以调用基构造函数，如以下代码中所示：

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="ConstructLineOfCredit":::

请注意，`LineOfCreditAccount` 构造函数会更改 `creditLimit` 参数的标志，使其与 `minimumBalance` 参数的意义匹配。

## <a name="different-overdraft-rules"></a>不同的透支规则

要添加的最后一项功能让 `LineOfCreditAccount` 可以对超过限额的取款进行收费，而不是拒绝交易。

一种方法是定义一个虚函数，并在其中实现所需的行为。 `BankAccount` 类将 `MakeWithdrawal` 方法重构为两个方法。 当取款使余额低于最小值时，新方法将执行指定的操作。 现有的 `MakeWithdrawal` 方法具有以下代码：

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

将其替换为以下代码：

:::code language="csharp" source="./snippets/object-oriented-programming/BankAccount.cs" ID="RefactoredMakeWithdrawal":::

添加的方法是 `protected`，这意味着只能从派生类中调用它。 该声明会阻止其他客户端调用该方法。 它还是 `virtual` 的，因此派生类可以更改行为。 返回类型为 `Transaction?`。 `?` 批注指示该方法可能返回 `null`。 在 `LineOfCreditAccount` 中添加以下实现，以在超过取款限额时收取费用：

:::code language="csharp" source="./snippets/object-oriented-programming/LineOfCreditAccount.cs" ID="AddOverdraftFee":::

当帐户透支时，该重写将返回费用交易。 如果取款未超出限额，则该方法将返回 `null` 交易。 这表明不收取任何费用。 通过将以下代码添加到 `Program` 类中的 `Main` 方法来测试这些更改：

:::code language="csharp" source="./snippets/object-oriented-programming/Program.cs" ID="TestLineOfCredit":::

运行该程序，并检查结果。

## <a name="summary"></a>摘要

如果遇到问题，可以在 [GitHub 存储库](https://github.com/dotnet/docs/tree/main/docs/csharp/tutorials/intro-to-csharp/snippets/object-oriented-programming)中查看本教程的源代码。

本教程演示了面向对象的编程中使用的多种技术：

- 为每个不同的帐户类型定义了类时，你使用了抽象。 这些类描述了该帐户类型的行为。
- 在每个类中将许多详细信息保留为 `private` 时，你使用了封装。
- 使用已在 `BankAccount` 类中创建的实现来保存代码时，你使用了继承。
- 创建 `virtual` 方法，派生类可以重写它们来创建该帐户类型的特定行为时，你使用了多形性。

恭喜，你已完成我们的所有 C# 简介教程。 若要了解详细信息，请继续学习更多[教程](introduction-to-classes.md)。
