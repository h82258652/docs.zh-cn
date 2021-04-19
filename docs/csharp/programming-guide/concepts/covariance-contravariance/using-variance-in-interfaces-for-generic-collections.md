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
# <a name="using-variance-in-interfaces-for-generic-collections-c"></a>在泛型集合的接口中使用变体 (C#)

协变接口允许其方法返回的派生类型多于接口中指定的派生类型。 逆变接口允许其方法接受派生类型少于接口中指定的类型的参数。  
  
 在.NET Framework 4 中，多个现有接口已变为协变和逆变接口。 包括 <xref:System.Collections.Generic.IEnumerable%601> 和 <xref:System.IComparable%601>。 这使你可将对基类型的泛型集合进行操作的那些方法重用于派生类型的集合。  
  
 有关 .NET 中变体接口的列表，请参阅[泛型接口中的变体 (C#)](./variance-in-generic-interfaces.md)。  
  
## <a name="converting-generic-collections"></a>转换泛型集合  

 下例阐释了 <xref:System.Collections.Generic.IEnumerable%601> 接口中的协变支持的益处。 `PrintFullName` 方法接受 `IEnumerable<Person>` 类型的集合作为参数。 但可将该方法重用于 `IEnumerable<Employee>` 类型的集合，因为 `Employee` 继承 `Person`。  
  
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
  
## <a name="comparing-generic-collections"></a>比较泛型集合  

 下例阐释了 <xref:System.Collections.Generic.IEqualityComparer%601> 接口中的逆变支持的益处。 `PersonComparer` 类实现 `IEqualityComparer<Person>` 接口。 但可以重用此类来比较 `Employee` 类型的对象序列，因为 `Employee` 继承 `Person`。  
  
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
  
## <a name="see-also"></a>请参阅

- [泛型接口中的变体 (C#)](./variance-in-generic-interfaces.md)
