---
title: 迁移新式桌面应用程序
description: 新式桌面应用程序迁移过程方面需要了解的所有内容。
ms.date: 01/19/2021
ms.openlocfilehash: b5bea6e601dc040adfd8ed410320a3416cb3372e
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "98615758"
---
# <a name="migrating-modern-desktop-applications"></a>迁移新式桌面应用程序

在本章中，我们将探讨在将现有应用程序从 .NET Framework 迁移到 .NET 时可能面临的最常见问题和挑战。

复杂的桌面应用程序不会孤立工作，需要与可能驻留在本地计算机或远程服务器上的子系统进行某种类型的交互。 可能需要某种类型的数据库作为持久性存储进行本地或远程连接。 随着 Internet 和面向服务的体系结构的兴起，将应用程序连接到驻留在远程服务器或云中的某种服务十分常见。 你可能需要访问计算机文件系统才能实现某种功能。 或者，你可能在使用驻留在应用程序外部 COM 对象中的一项功能，例如，如果你在应用中集成 Office 程序集，那么这是一种常见方案。

此外，在 .NET Framework 和 .NET 公开的 API 曲面上也存在差异，并且 .NET Framework 上可用的某些功能在 .NET 上不可用。 因此在规划迁移时，必须了解并考虑这些事项。

## <a name="configuration-files"></a>配置文件

通过配置文件可以存储在运行时读取的属性集并影响我们应用的行为，例如查找数据库的位置或执行循环的次数。 此方法的优点是，可以修改应用程序的某些方面，而无需新编码并重新编译。 例如，当相同应用代码使用一组特定配置值在开发环境中运行以及使用不同配置值在生产环境中运行时，这会十分方便。

### <a name="configuration-on-net-framework"></a>.NET Framework 上的配置

如果你有一个正常工作的 .NET Framework 桌面应用程序，那么很可能有一个 app.config 文件，可通过 <xref:System.Configuration.AppSettingsSection> 类从 `System.Configuration` 命名空间进行访问。

在 .NET Framework 基础结构中，有一个从其父级继承属性的配置文件的层次结构。 可以找到一个 machine.config 文件，它定义许多可在任何后代配置文件中使用或替代的属性和配置节。

### <a name="configuration-on-net"></a>.NET 上的配置

在 .NET 世界中没有 machine.config 文件。 即使可以继续使用旧式 <xref:System.Configuration> 命名空间，也可以考虑切换到新式 <xref:Microsoft.Extensions.Configuration>，这可提供很多增强功能。

配置 API 支持配置提供程序的概念，这种提供程序定义用于加载配置的数据源。 有不同种类的内置提供程序，例如：

- 内存中的 .NET 对象
- INI 文件
- JSON 文件
- XML 文件
- 命令行参数
- 环境变量
- 加密的用户存储

 也可以生成自己的提供程序。

新配置允许实现可分组到多级层次结构中的名称/值对的列表。 任何存储的值都会映射到字符串，并且提供内置绑定支持，使你可以将设置反序列化为自定义的普通旧 CLR 对象 (POCO) 对象。

<xref:Microsoft.Extensions.Configuration.ConfigurationBuilder> 对象使你可以通过使用优先级规则解析首选项，来添加应用程序需要的任意多个配置提供程序。 因此，在代码中添加的最后一个提供程序会替代其他提供程序。 这对于管理不同的执行环境是一种很好的功能，因为可以为开发、测试和生产环境定义不同的配置，然后通过代码中的单个函数管理它们。

### <a name="migrating-configuration-files"></a>迁移配置文件

你可以继续使用现有 app.config XML 文件。 但是，你可以利用此机会来迁移配置，以便从 .NET 的多种增强功能受益。

若要从旧样式 app.config 迁移到新的配置文件，应在 XML 格式与 JSON 格式之间进行选择。

如果选择 XML，则转换十分简单。 由于内容相同，因此只需保存 app.config 文件，并将 XML 作为类型即可。 然后，将引用 AppSettings 的代码更改为使用 `ConfigurationBuilder` 类。 此更改应很容易。

如果要使用 JSON 格式，而不想手动迁移，则 .NET 上有一个名为 [dotnet-config2json](https://www.nuget.org/packages/dotnet-config2json/) 的工具可用，该工具可以将 app.config 文件转换为 JSON 配置文件。

使用 machine.config 文件中定义的配置节时，也可能会遇到一些问题。 例如，请考虑以下配置：

```xml
<configuration>
    <system.diagnostics>
        <switches>
            <add name="General" value="4" />
        </switches>
        <trace autoflush="true" indentsize="2">
            <listeners>
                <add name="myListener"
                     type="System.Diagnostics.TextWriterTraceListener,
                           System, Version=1.0.3300.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
                     initializeData="MyListener.log"
                     traceOutputOptions="ProcessId, LogicalOperationStack, Timestamp, ThreadId, Callstack, DateTime" />
            </listeners>
        </trace>
    </system.diagnostics>
</configuration>
```

如果将此配置引入 .NET，则会遇到异常：

> 无法识别的配置节 System.Diagnostics

发生此异常的原因是，该节和负责处理该节的程序集在 machine.config 文件中定义，而该文件现在不存在。

若要轻松解决此问题，可以将节定义从旧的 machine.config 复制到新的配置文件：

```xml
<configSections>
    <section name="system.diagnostics"
             type="System.Diagnostics.SystemDiagnosticsSection,
                   System, Version=4.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"/>
</configSections>
```

## <a name="accessing-databases"></a>访问数据库

几乎每个桌面应用程序都需要某种类型的数据库。 对于桌面，常常可找到在桌面应用与数据库引擎之间具有直接连接的客户端-服务器体系结构。 这些数据库可以是本地的，也可以是远程的，具体取决于是否需要在不同用户之间共享信息。

从代码的角度来看，已有许多技术和框架使开发人员能够连接、查询和更新数据库。

在讨论 Windows 桌面应用程序时，可以找到的最常见数据库示例是 Microsoft Access 和 Microsoft SQL Server。 如果你有超过 20 年的桌面编程经验，那么会对 ODBC、OLEDB、RDO、ADO、ADO.NET、LINQ 和实体框架等名称感到熟悉。

### <a name="odbc"></a>ODBC

由于 Microsoft 提供与 .NET Standard 2.0 兼容的 `System.Data.Odbc` 库，因此你可以继续在 .NET 上使用 ODBC。

### <a name="ole-db"></a>OLE DB

[OLE DB](/previous-versions/windows/desktop/ms722784(v=vs.85)) 是一种以统一方式访问各种数据源的好方法。 但它基于 COM，而后者是仅用于 Windows 的技术，因此不适合于跨平台技术（如 .NET）。 它在 SQL Server 版本 2014 及更高版本中也不受支持。 由于这些原因，.NET 不支持 OLE DB。

### <a name="adonet"></a>ADO.NET

你仍可以在 .NET 上通过现有桌面代码使用 ADO.NET。 只需更新一些 NuGet 包即可。

### <a name="ef-core-vs-ef6"></a>EF Core 与 EF6

当前支持两个版本的实体框架 (EF)，即实际框架 6 (EF6) 和 EF Core。

在 .NET Framework 世界中发布的最新技术是实体框架，6.4 是最新版本。 随着 .NET Core 的推出，Microsoft 也发布了基于实体框架的新数据访问堆栈，称为 Entity Framework Core。

可以从 .NET Framework 和 .NET 使用 EF 6.4 和 EF Core。 那么，有哪些决策驱动因素可帮助在这两者之间进行决策呢？

EF 6.3 是 EF6 的第一个版本，可以在 .NET 上运行并跨平台工作。 事实上，此版本的主要目标是为了更轻松地将使用 EF6 的现有应用程序迁移到 .NET。

EF Core 旨在提供类似于 EF6 的开发人员体验。 大多数顶级 API 保持不变，因此，用过 EF6 的开发人员都会对 EF Core 感到很熟悉。

尽管兼容，但在进行决策之前，应查看实现中的一些差异。
有关详细信息，请参阅[比较 EF Core 与 EF6](/ef/efcore-and-ef6/)。

在以下情况中，建议使用 EF Core：

* 应用需要.NET 的功能。
* EF Core 支持应用需要的所有功能。

如果以下两个条件都成立，请考虑使用 EF6：

* 应用将在 Windows 和 .NET Framework 4.0 或更高版本上运行。
* EF6 支持应用需要的所有功能。

### <a name="relational-databases"></a>关系数据库

#### <a name="sql-server"></a>SQL Server

如果你在几年前针对桌面进行开发，那么 SQL Server 是首选数据库之一。 通过使用 .NET Framework 中的 <xref:System.Data.SqlClient>（封装了特定于数据库的协议），你可以访问 SQL Server 的各个版本。

在 .NET 中，可以找到新的 `SqlClient` 类，它与 .NET Framework 中的现有类完全兼容，但位于 <xref:Microsoft.Data.SqlClient> 库中。 只需添加对 [Microsoft.Data.SqlClient](https://www.nuget.org/packages/Microsoft.Data.SqlClient/) NuGet 包的引用，并对命名空间进行一些重命名，所有内容便应按预期方式工作。

#### <a name="microsoft-access"></a>Microsoft Access

当不需要复杂但可伸缩性更高的 SQL Server 时，Microsoft Access 已使用了很多年。 你仍可以使用 <xref:System.Data.Odbc> 库连接到 Microsoft Access。

## <a name="consuming-services"></a>使用服务

随着面向服务的体系结构的兴起，桌面应用程序开始从客户端-服务器模型发展为三层方法。 在客户端-服务器方法中，会从客户端建立直接数据库连接，业务逻辑通常包含在单个 EXE 文件中。 另一方面，三层方法会建立一个实现业务逻辑和数据库访问的中间服务层，从而实现更好的安全性、可伸缩性和可重用性。 层方法不是直接使用数据的数据集，而是依赖于一组实现协定和类型对象的服务来实现数据传输。

如果你具有使用 WCF 服务的桌面应用程序，并且要将它迁移到 .NET，则需要考虑一些事项。

第一件事是如何解析配置以访问服务。 因为配置在 .NET 中是不同的，所以需要在配置文件中进行一些更新。

其次，需要通过 Visual Studio 2019 上提供的新工具重新生成服务客户端。 在此步骤中，必须考虑激活同步操作的生成，以使客户端与现有代码兼容。

迁移后，如果发现 .NET 上未提供所需的库，则可以添加对 [Microsoft.Windows.Compatibility](https://www.nuget.org/packages/Microsoft.Windows.Compatibility) NuGet 包的引用，并查看是否存在缺失的函数。

如果要使用 <xref:System.Net.WebRequest> 类执行 Web 服务调用，则可能会在 .NET 中发现一些差异。 建议改为使用 System.Net.Http.HttpClient。

## <a name="consuming-a-com-object"></a>使用 COM 对象

目前，无法从 Visual Studio 2019 添加对 COM 对象的引用，以便与 .NET 一起使用。 因此必须手动修改项目文件。

在项目文件中插入 `COMReference` 结构，如以下示例中所示：

```xml
<ItemGroup>
    <COMReference Include="MSHTML">
        <Guid>{3050F1C5-98B5-11CF-BB82-00AA00BDCE0B}\</Guid>
        <VersionMajor>4</VersionMajor>
        <VersionMinor>0</VersionMinor>
        <Lcid>0</Lcid>
        <WrapperTool>primary</WrapperTool>
        <Isolated>false</Isolated>
    </COMReference>
</ItemGroup>
```

## <a name="more-things-to-consider"></a>要考虑的更多事项

可用于 .NET Framework 库的多种技术对于 .NET Core 或 .NET 5 不可用。 如果代码依赖于其中一些技术，请考虑此部分中所述的替代方法。

[Windows 兼容性包](../../core/porting/windows-compat-pack.md)提供对以前仅对 .NET Framework 可用的 API 的访问权限。 它可用于 .NET Core 和 .NET Standard 项目。

有关 API 兼容性的详细信息，可以在 <https://docs.microsoft.com/dotnet/core/compatibility/fx-core> 中找到有关中断性变更和已弃用/旧的 API 的文档。

### <a name="appdomains"></a>AppDomain

应用程序域 (AppDomain) 可将应用相互隔离。 AppDomain 需要运行时支持，会耗费大量资源。 不支持创建其他应用域。 对于代码隔离，建议将流程分隔开来或将容器用作一种替代方法。 对于动态加载的程序集，我们建议使用新的 <xref:System.Runtime.Loader.AssemblyLoadContext> 类。

.NET 公开了一些 `AppDomain` API 曲面，以便可以更轻松地从 .NET Framework 进行代码迁移。 一些 API 可正常工作（例如 <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType>），一些成员不会执行任何操作（例如 <xref:System.AppDomain.SetCachePath%2A>），也有一些会引发 <xref:System.PlatformNotSupportedException>（例如 <xref:System.AppDomain.CreateDomain%2A>）。

### <a name="remoting"></a>远程处理

.NET 远程处理用于进行跨 AppDomain 的通信，该通信现已不再受支持。 同样，远程处理也需要运行时支持，进行维护的成本较高。 由于这些原因，.NET 不支持 .NET 远程处理。

对于跨进程通信，应将进程间通信 (IPC) 机制视为远程处理的备用方案，如 <xref:System.IO.Pipes?displayProperty=nameWithType> 或 <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> 类。

对于跨计算机的通信，可将基于网络的解决方案用作备用方案。 最好使用低开销纯文本协议，例如 HTTP。 此处，ASP.NET Core 使用的 Web 服务器 Kestrel Web 服务器是一个选择。

### <a name="code-access-security-cas"></a>代码访问安全性 (CAS)

沙盒依赖为托管应用程序或库使用或运行提供资源的运行时或框架进行限制，其在 .NET 上不受支持。

可使用操作系统提供的安全边界，例如虚拟化、容器或具有最少特权集的用于运行进程的用户帐户。

### <a name="security-transparency"></a>安全透明度

与 CAS 相似，借助安全透明度可以以声明性方式将沙盒代码与安全关键代码隔离，但是不再支持将它作为安全边界。

可使用操作系统提供的安全边界，例如虚拟化、容器或具有最少特权集的用于运行进程的用户帐户。

>[!div class="step-by-step"]
>[上一页](whats-new-dotnet.md )
>[下一页](windows-migration.md)
