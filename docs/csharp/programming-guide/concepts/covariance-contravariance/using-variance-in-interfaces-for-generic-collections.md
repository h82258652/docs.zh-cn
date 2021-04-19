---
title: 在泛型集合的接口中使用变体 (C#)
description: 了解如何对泛型集合使用协变和逆变接口。 请参阅转换和比较泛型集合的示例。
ms.date: 07/20/2015
ms.assetid: a44f0708-10fa-4c76-82cd-daa6e6b31e8e
ms.openlocfilehash: 39da866be3d1ead357485be31fee1c19f2b1ae27
ms.sourcegitcommit: 985c603cb21a085f8a8105f34ff5b87a44b76ab4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564852"
---
# <a name="using-variance-in-interfaces-for-generic-collections-c"></a><span data-ttu-id="6fc6e-104">在泛型集合的接口中使用变体 (C#)</span><span class="sxs-lookup"><span data-stu-id="6fc6e-104">Using Variance in Interfaces for Generic Collections (C#)</span></span>

<span data-ttu-id="6fc6e-105">协变接口允许其方法返回的派生类型多于接口中指定的派生类型。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-105">A covariant interface allows its methods to return more derived types than those specified in the interface.</span></span> <span data-ttu-id="6fc6e-106">逆变接口允许其方法接受派生类型少于接口中指定的类型的参数。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-106">A contravariant interface allows its methods to accept parameters of less derived types than those specified in the interface.</span></span>  
  
 <span data-ttu-id="6fc6e-107">在.NET Framework 4 中，多个现有接口已变为协变和逆变接口。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-107">In .NET Framework 4, several existing interfaces became covariant and contravariant.</span></span> <span data-ttu-id="6fc6e-108">包括 <xref:System.Collections.Generic.IEnumerable%601> 和 <xref:System.IComparable%601>。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-108">These include <xref:System.Collections.Generic.IEnumerable%601> and <xref:System.IComparable%601>.</span></span> <span data-ttu-id="6fc6e-109">这使你可将对基类型的泛型集合进行操作的那些方法重用于派生类型的集合。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-109">This enables you to reuse methods that operate with generic collections of base types for collections of derived types.</span></span>  
  
 <span data-ttu-id="6fc6e-110">有关 .NET 中变体接口的列表，请参阅[泛型接口中的变体 (C#)](./variance-in-generic-interfaces.md)。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-110">For a list of variant interfaces in .NET, see [Variance in Generic Interfaces (C#)](./variance-in-generic-interfaces.md).</span></span>  
  
## <a name="converting-generic-collections"></a><span data-ttu-id="6fc6e-111">转换泛型集合</span><span class="sxs-lookup"><span data-stu-id="6fc6e-111">Converting Generic Collections</span></span>  

 <span data-ttu-id="6fc6e-112">下例阐释了 <xref:System.Collections.Generic.IEnumerable%601> 接口中的协变支持的益处。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-112">The following example illustrates the benefits of covariance support in the <xref:System.Collections.Generic.IEnumerable%601> interface.</span></span> <span data-ttu-id="6fc6e-113">`PrintFullName` 方法接受 `IEnumerable<Person>` 类型的集合作为参数。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-113">The `PrintFullName` method accepts a collection of the `IEnumerable<Person>` type as a parameter.</span></span> <span data-ttu-id="6fc6e-114">但可将该方法重用于 `IEnumerable<Employee>` 类型的集合，因为 `Employee` 继承 `Person`。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-114">However, you can reuse it for a collection of the `IEnumerable<Employee>` type because `Employee` inherits `Person`.</span></span>  
  
```csharp  
// Simple hierarchy of classes.  
public class Person  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
}  
  
public class Employee : Person { }  
  
class Program  
{  
    // The method has a parameter of the IEnumerable<Person> type.  
    public static void PrintFullName(IEnumerable<Person> persons)  
    {  
        foreach (Person person in persons)  
        {  
            Console.WriteLine("Name: {0} {1}",  
            person.FirstName, person.LastName);  
        }  
    }  
  
    public static void Test()  
    {  
        IEnumerable<Employee> employees = new List<Employee>();  
  
        // You can pass IEnumerable<Employee>,
        // although the method expects IEnumerable<Person>.  
  
        PrintFullName(employees);  
  
    }  
}  
```  
  
## <a name="comparing-generic-collections"></a><span data-ttu-id="6fc6e-115">比较泛型集合</span><span class="sxs-lookup"><span data-stu-id="6fc6e-115">Comparing Generic Collections</span></span>  

 <span data-ttu-id="6fc6e-116">下例阐释了 <xref:System.Collections.Generic.IEqualityComparer%601> 接口中的逆变支持的益处。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-116">The following example illustrates the benefits of contravariance support in the <xref:System.Collections.Generic.IEqualityComparer%601> interface.</span></span> <span data-ttu-id="6fc6e-117">`PersonComparer` 类实现 `IEqualityComparer<Person>` 接口。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-117">The `PersonComparer` class implements the `IEqualityComparer<Person>` interface.</span></span> <span data-ttu-id="6fc6e-118">但可以重用此类来比较 `Employee` 类型的对象序列，因为 `Employee` 继承 `Person`。</span><span class="sxs-lookup"><span data-stu-id="6fc6e-118">However, you can reuse this class to compare a sequence of objects of the `Employee` type because `Employee` inherits `Person`.</span></span>  
  
```csharp  
// Simple hierarchy of classes.  
public class Person  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
}  
  
public class Employee : Person { }  
  
// The custom comparer for the Person type  
// with standard implementations of Equals()  
// and GetHashCode() methods.  
class PersonComparer : IEqualityComparer<Person>  
{  
    public bool Equals(Person x, Person y)  
    {
        if (Object.ReferenceEquals(x, y)) return true;  
        if (Object.ReferenceEquals(x, null) ||  
            Object.ReferenceEquals(y, null))  
            return false;
        return x.FirstName == y.FirstName && x.LastName == y.LastName;  
    }  
    public int GetHashCode(Person person)  
    {  
        if (Object.ReferenceEquals(person, null)) return 0;  
        int hashFirstName = person.FirstName == null  
            ? 0 : person.FirstName.GetHashCode();  
        int hashLastName = person.LastName.GetHashCode();  
        return hashFirstName ^ hashLastName;  
    }  
}  
  
class Program  
{  
  
    public static void Test()  
    {  
        List<Employee> employees = new List<Employee> {  
               new Employee() {FirstName = "Michael", LastName = "Alexander"},  
               new Employee() {FirstName = "Jeff", LastName = "Price"}  
            };  
  
        // You can pass PersonComparer,
        // which implements IEqualityComparer<Person>,  
        // although the method expects IEqualityComparer<Employee>.  
  
        IEnumerable<Employee> noduplicates =  
            employees.Distinct<Employee>(new PersonComparer());  
  
        foreach (var employee in noduplicates)  
            Console.WriteLine(employee.FirstName + " " + employee.LastName);  
    }  
}  
```  
  
## <a name="see-also"></a><span data-ttu-id="6fc6e-119">请参阅</span><span class="sxs-lookup"><span data-stu-id="6fc6e-119">See also</span></span>

- [<span data-ttu-id="6fc6e-120">泛型接口中的变体 (C#)</span><span class="sxs-lookup"><span data-stu-id="6fc6e-120">Variance in Generic Interfaces (C#)</span></span>](./variance-in-generic-interfaces.md)
