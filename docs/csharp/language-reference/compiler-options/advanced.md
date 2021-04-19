---
description: 高级 C# 编译器选项。 这些选项在高级方案中使用。
title: C# 编译器选项 - 高级方案
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- BaseAddress compiler option [C#]
- ChecksumAlgorithm compiler option [C#]
- CodePage compiler option [C#]
- Utf8Output compiler option [C#]
- MainEntryPoint compiler option [C#]
- GenerateFullPaths compiler option [C#]
- FileAlignment compiler option [C#]
- PathMap compiler option [C#]
- PdbFile compiler option [C#]
- ErrorEndLocation compiler option [C#]
- NoStandardLib compiler option [C#]
- PreferredUILang compiler option [C#]
- SubsystemVersion compiler option [C#]
- AdditionalLibPaths compiler option [C#]
- ApplicationConfiguration compiler option [C#]
- ModuleAssemblyName compiler option [C#]
ms.openlocfilehash: 50108fa18127c55bd791d70520c8dfd90331f86f
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494086"
---
# <a name="advanced-c-compiler-options"></a>高级 C# 编译器选项

以下选项支持高级方案。 新的 MSBuild 语法以粗体显示。 旧的 `csc.exe` 语法以 `code style` 显示。

- **MainEntryPoint**、**StartupObject** / `-main`：指定包含入口点的类型。
- **PdbFile** / `-pdb`：指定调试信息文件名。
- **PathMap** / `-pathmap`：指定编译器输出的源路径名的映射。
- **ApplicationConfiguration** / `-appconfig`：指定包含程序集绑定设置的应用程序配置文件。
- **AdditionalLibPaths** / `-lib`：指定要在其中搜索引用的其他目录。
- **GenerateFullPaths** / `-fullpath`：编译器生成完全限定的路径。
- **PreferredUILang** / `-preferreduilang`：指定首选输出语言名称。
- **BaseAddress** / `-baseaddress`：指定要生成的库的基址。
- **ChecksumAlgorithm** / `-checksumalgorithm`：指定用于计算 PDB 中存储的源文件校验和的算法。
- **CodePage** / `-codepage`：指定在打开源文件时使用的代码页。
- **Utf8Output** / `-utf8output`：以 UTF-8 编码输出编译器消息。
- **FileAlignment** / `-filealign`：指定用于输出文件节的对齐方式。
- **ErrorEndLocation** / `-errorendlocation`：输出每个错误结尾位置的行和列。
- **NoStandardLib** / `-nostdlib`：不引用标准库 mscorlib.dll。
- **SubsystemVersion** / `-subsystemversion`：指定此程序集的子系统版本。
- **ModuleAssemblyName** / `-moduleassemblyname`：此模块所属程序集的名称。

## <a name="mainentrypoint-or-startupobject"></a>MainEntryPoint 或 StartupObject

如果多个类包含 `Main` 方法，此选项将指定包含程序入口点的类。

```xml
<StartupObject>MyNamespace.Program</StartupObject>
```

或

```xml
<MainEntryPoint>MyNamespace.Program</MainEntryPoint>
```

其中 `Program` 是包含 `Main` 方法的类型。 提供的类名必须是完全限定类名；它必须包括完整命名空间（包含类），后跟类名。 例如，当 `Main` 方法位于 `MyApplication.Core` 命名空间中的 `Program` 类中时，编译器选项必须为 `-main:MyApplication.Core.Program`。 如果编译包含具有 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法的多个类型，则可以指定哪个类型包含 `Main` 方法。

> [!NOTE]
> 此选项不能用于包含[顶级语句](../../programming-guide/main-and-command-args/top-level-statements.md)的项目，即使该项目包含一个或多个 `Main` 方法也是如此。

## <a name="pdbfile"></a>PdbFile

PdbFile 编译器选项指定调试符号文件的名称和位置。  `filename` 值指定调试符号文件的名称和位置。  

```xml
<PdbFile>filename</PdbFile>
```

指定 [DebugType](code-generation.md#debugtype) 后，编译器将在创建输出文件（.exe 或 .dll）的相同目录中创建 .pdb 文件。 .pdb 文件与输出文件的名称具有相同的基文件名。 PdbFile 允许为 .pdb 文件指定非默认的文件名和位置。 不能在 Visual Studio 开发环境中设置此编译器选项，也不能以编程方式对其进行更改。  

## <a name="pathmap"></a>PathMap

PathMap 编译器选项指定如何将物理路径映射到编译器输出的源路径名称。 此选项将编译器在其上运行的计算机上的每个物理路径映射到应写入输出文件的相应路径。 在下面的示例中，`path1` 是当前环境中源文件的完整路径，`sourcePath1` 是任何输出文件中替代 `path1` 的源路径。 若要指定多个映射的源路径，请用分号分隔每个路径。

```xml
<PathMap>path1=sourcePath1;path2=sourcePath2</PathMap>
```

编译器将源路径写入其输出，原因如下：

1. 将 <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 应用于可选参数时，会将源路径替换为参数。
1. PDB 文件中嵌入的源路径。
1. PDB 文件的路径嵌入到 PE（可移植的可执行文件）文件中。

## <a name="applicationconfiguration"></a>ApplicationConfiguration

ApplicationConfiguration 编译器选项使 C# 应用程序能够在程序集绑定时将程序集的应用程序配置 (app.config) 文件的位置指定为公共语言运行时 (CLR)。

```xml
<ApplicationConfiguration>file</ApplicationConfiguration>
```

其中 `file` 是包含程序集绑定设置的应用程序配置文件。 ApplicationConfiguration 的一种用途是处理高级方案，在这种方案中，程序集必须同时引用特定引用程序集的 .NET Framework 版本和 .NET Framework for Silverlight 版本。 例如，在 Windows Presentation Foundation (WPF) 中编写的 XAML 设计器可能需要为设计器用户界面引用 WPF 桌面以及随附于 Silverlight 的 WPF 子集。 同一设计器程序集必须访问这两个程序集。 默认情况下，单独引用会导致编译器错误，因为程序集绑定将这两个程序集视为等效。 借助 ApplicationConfiguration 编译器选项，可通过使用 `<supportPortability>` 标记指定禁用默认行为的 app.config 文件的位置，如以下示例所示。

```xml
<supportPortability PKT="7cec85d7bea7798e" enable="false"/>
```

编译器将文件的位置传递给 CLR 的程序集绑定逻辑。

> [!NOTE]
> 若要使用已在项目中设置的 app.config 文件，请将属性标记 `<UseAppConfigForCompiler>` 添加到 .csproj 文件，并将其值设置为 `true`。 若要指定不同的 app.config 文件，请添加属性标记 `<AppConfigForCompiler>` 并将其值设置为该文件的位置。

以下示例展示一个 app.config 文件，通过使用该文件，应用程序能够同时引用任何 .NET Framework 程序集（同时存在于后述两个实现中）的 .NET Framework 实现和 .NET Framework for Silverlight 实现。 ApplicationConfiguration 编译器选项指定此 app.config 文件的位置。

```xml
<configuration>
  <runtime>
    <assemblyBinding>
      <supportPortability PKT="7cec85d7bea7798e" enable="false"/>
      <supportPortability PKT="31bf3856ad364e35" enable="false"/>
    </assemblyBinding>
  </runtime>
</configuration>
```

## <a name="additionallibpaths"></a>AdditionalLibPaths

AdditionalLibPaths 选项指定通过 [References](inputs.md#references) 选项引用的程序集的位置。

```xml
<AdditionalLibPaths>dir1[,dir2]</AdditionalLibPaths>
```

其中 `dir1` 是在当前工作目录（从中调用编译器的目录）或公共语言运行时的系统目录中未找到引用程序集时，编译器将在其中进行查找的目录。 `dir2` 是要在其中搜索程序集引用的一个或多个其他目录。 用逗号分隔每个目录名称，且中间没有空格。 编译器按以下顺序搜索未完全限定的程序集引用：  

1. 当前工作目录。
1. 公共语言运行时系统目录。
1. 由 AdditionalLibPaths 指定的目录。
1. 由 LIB 环境变量指定的目录。

使用 Reference 指定程序集引用。 AdditionalLibPaths 是累加的。 每一次指定的值都追加到以前的值中。 由于程序集清单中未指定依赖程序集的路径，因此应用程序将查找并使用全局程序集缓存中的程序集。 编译器可以引用程序集并不表示公共语言运行时可以在运行时找到并加载程序集。 有关运行时如何搜索引用的程序集的详细信息，请参阅[运行时如何定位程序集](../../../framework/deployment/how-the-runtime-locates-assemblies.md)。  

## <a name="generatefullpaths"></a>GenerateFullPaths

GenerateFullPaths 选项导致编译器在列出编译错误和警告时指定文件的完整路径。
  
```Xml
<GenerateFullPaths>true</GenerateFullPaths>
```

默认情况下，由编译所产生的错误和警告指定其中发现错误的文件的名称。 GenerateFullPaths 选项导致编译器指定文件的完整路径。 此编译器选项在 Visual Studio 中不可用，并且无法以编程方式更改。

## <a name="preferreduilang"></a>PreferredUILang

通过使用 PreferredUILang 编译器选项，可指定 C# 编译器用于显示输出（如错误消息）的语言。

```xml
<PreferredUILang>language</PreferredUILang>
```

其中 `language` 是用于编译器输出的语言的[语言名称](/windows/desktop/Intl/language-names)。 可以使用 PreferredUILang 编译器选项指定 C# 编译器用于错误消息和其他命令行输出的语言。 如果未安装针对该语言的语言包，将改用操作系统的语言设置。

## <a name="baseaddress"></a>BaseAddress

通过 BaseAddress 选项，可指定加载 DLL 的首选基址。 若要深入了解何时且为何要使用此选项，请参阅 [Larry Osterman 的网络日志](/archive/blogs/larryosterman/why-should-i-even-bother-to-use-dlls-in-my-system)。

```xml
<BaseAddress>address</BaseAddress>
```

其中 `address` 是 DLL 的基址。 可将此地址指定为十进制数、十六进制数或八进制数。 DLL 的默认基址由 .NET 公共语言运行时设置。 此地址中的低序字将被舍入取整。 例如，如果指定 `0x11110001`，它将被舍入为 `0x11110000`。 要完成 DLL 的签名过程，请使用具有 -R 选项的 SN.EXE。

## <a name="checksumalgorithm"></a>ChecksumAlgorithm

此选项控制用于对 PDB 中的源文件进行编码的校验和算法。

```xml
<ChecksumAlgorithm>algorithm</ChecksumAlgorithm>
```

`algorithm` 必须是 `SHA1`（默认）或 `SHA256`。

## <a name="codepage"></a>CodePage

如果所需页不是系统的当前默认代码页，则此选项指定编译期间要使用的代码页。
  
```xml
<CodePage>id</CodePage>
```

其中 `id` 是要用于编译中所有源代码文件的代码页 ID。 编译器将首先尝试将所有源文件解释为 UTF-8。 如果源代码文件使用除 UTF-8 以外的编码并使用除 7 位 ASCII 字符以外的字符，请使用 CodePage 选项指定应使用的代码页。 CodePage 适用于编译中的所有源代码文件。 有关如何查找系统上支持哪些代码页的信息，请参阅 [GetCPInfo](/windows/desktop/api/winnls/nf-winnls-getcpinfo)。

## <a name="utf8output"></a>Utf8Output

Utf8Output 选项使用 UTF-8 编码显示编译器输出。

```xml
<Utf8Output>true</Utf8Output>
```

在某些国际配置中，编译器输出无法在控制台上正确显示。 使用 Utf8Output 并将编译器输出重定向到文件。

## <a name="filealignment"></a>FileAlignment

FileAlignment 选项用于指定输出文件中各节的大小。 有效值为 512、1024、2048、4096 和 8192。 这些值以字节为单位。

```xml
<FileAlignment>number</FileAlignment>
```

在 Visual Studio 中，可以从项目的“生成”属性的“高级”页中设置 FileAlignment 选项。 每一节都将在是 FileAlignment 值的倍数的边界上对齐。 没有固定的默认值。 如果未指定 FileAlignment，公共语言运行时在编译时会选取一个默认值。 通过指定节的大小，可以影响输出文件的大小。 修改节的大小可能对将在较小设备上运行的程序有用。 使用 [DUMPBIN](/cpp/build/reference/dumpbin-options) 可查看有关输出文件中各节的信息。

## <a name="errorendlocation"></a>ErrorEndLocation

指示编译器输出每个错误结尾位置的行和列。

```xml
<ErrorEndLocation>filename</ErrorEndLocation>
```

默认情况下，编译器会为所有错误和警告在源代码中写入起始位置。 当此选项设置为 true 时，编译器会为每个错误和警告写入起始和结尾位置。

## <a name="nostandardlib"></a>NoStandardLib

NoStandardLib 可防止导入 mscorlib.dll，后者定义了整个 System 命名空间。

```xml
<NoStandardLib>true</NoStandardLib>
```

如果你想要定义或创建自己的 System 命名空间和对象，请使用此选项。 如果不指定 NoStandardLib，则 mscorlib.dll 将被导入到程序中（与指定 `<NoStandardLib>false</NoStandardLib>` 相同）。

## <a name="subsystemversion"></a>SubsystemVersion

指定可执行文件可以运行的子系统的最低版本。 大多数情况下，此选项确保该可执行文件可以使用早期 Windows 版本中未提供的安全功能。

> [!NOTE]
> 若要指定子系统本身，请使用 [TargetType](./output.md#targettype) 编译器选项。

```xml
<SubsystemVersion>major.minor</SubsystemVersion>
```

`major.minor` 指定所需的子系统最低版本，以主版本和次要版本之间使用点标记的方式表示。 例如，你可以指定应用程序不能在 Windows 7 之前的操作系统上运行。 将此选项的值设置为 6.01，如本文后面的表中所述。 将 `major` 和 `minor` 的值指定为整数。 `minor` 版本中的前导零不会更改版本，但尾随零会。 例如，6.1 和 6.01 表示相同的版本，但 6.10 表示另一个版本。 建议次要版本用两位数表示，以免混淆。

下表列出了常见的 Windows 子系统版本。

|Windows 版本|子系统版本|
|---------------------|-----------------------|
|Windows Server 2003|5.02|
|Windows Vista|6.00|
|Windows 7|6.01|
|Windows Server 2008|6.01|
|Windows 8|6.02|

SubsystemVersion 编译器选项的默认值取决于以下列表中的条件：

- 只要设置了以下列表中的任意编译器选项，则默认值为 6.02：
  - [/target:appcontainerexe](output.md)
  - [/target:winmdobj](output.md)
  - [-platform:arm](output.md)
- 如果使用 MSBuild，面向 .NET Framework 4.5，并且未设置先前在此列表中指定的任何编译器选项，则默认值为 6.00。
- 如果前面的条件均不符合，则默认值为 4.00。

## <a name="moduleassemblyname"></a>ModuleAssemblyName

指定 .netmodule 可以访问其非公共类型的程序集的名称。

```xml
<ModuleAssemblyName>assembly_name</ModuleAssemblyName>
```

生成 .netmodule 时，应使用 ModuleAssemblyName 并满足以下条件：

- .netmodule 需要具有访问现有程序集中非公共类型的权限。
- 知道生成后的 .netmodule 所在程序集的名称。
- 现有程序集已经获得友元程序集访问权限，可访问将在其中生成 .netmodule 的程序集。

有关生成 .netmodule 的详细信息，请参阅模块的 [TargetType](output.md#targettype) 选项。 有关友元程序集的详细信息，请参阅[友元程序集](../../../standard/assembly/friend.md)。
