---
description: 用于控制代码生成的 C# 编译器选项。 这些选项会影响编译器为给定编译生成的代码。
title: 'C # 编译器选项 - 代码生成选项'
ms.date: 03/12/2021
f1_keywords:
- cs.build.options
helpviewer_keywords:
- DebugType compiler option [C#]
- Optimize compiler option [C#]
- Deterministic compiler option [C#]
- ProduceOnlyReferenceAssembly compiler option [C#]
ms.openlocfilehash: 02610c9d0142643bdb553f8b8177d1a4a2237717
ms.sourcegitcommit: 652f62fc8f3ab6a264681b6eb5211ac7539bd115
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "105964788"
---
# <a name="c-compiler-options-that-control-code-generation"></a>控制代码生成的 C# 编译器选项

下面的选项控制编译器生成的代码。 新的 MSBuild 语法以粗体显示。 旧的 csc.exe 语法以 `code style` 显示。

- **DebugType** / `-debug`：发出（或不发出）调试信息。
- **Optimize** / `-optimize`：启用优化。
- **Deterministic** / `-deterministic`：从相同的输入源生成每字节对等的输出。
- **ProduceOnlyReferenceAssembly** / `-refonly`：生成引用程序集作为主输出，而非生成完整程序集。

## <a name="debugtype"></a>DebugType

DebugType 选项将使编译器生成调试信息，并将此信息放置在一个或多个输出文件中。 默认情况下，将为调试生成配置添加调试信息。 默认情况下，为发布生成配置禁用调试信息。

```xml
<DebugType>pdbonly</DebugType>
```

自 C# 6.0 起，对于所有编译器版本而言，pdbonly 与 full 之间没有任何区别。 请选择 pdbonly。 若要更改 .pdb 文件的位置，请参阅 [PdbFile](./advanced.md#pdbfile)。

以下为有效值：

| Value      | 含义                                                                                                 |
|------------|---------------------------------------------------------------------------------------------------------|
| `full`     | 使用当前平台的默认格式向 _.pdb_ 文件发出调试信息：<br>Windows：Windows pdb 文件。 <br>Linux/macOS：[可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 文件。 |
| `pdbonly`  | 与 `full` 相同。 有关详细信息，请参阅下面的注释。 |
| `portable` | 使用跨平台[可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 格式向 .pdb 文件发出调试信息。 |
| `embedded` | 使用 [可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 格式向 _.dll/.exe_ 自身（未生成 _.pdb_ 文件）发出调试信息。 |

> [!IMPORTANT]
> 以下信息仅适用于 C# 6.0 以前的编译器。
> 此元素的值可以是 `full` 或 `pdbonly`。 full 参数（在不指定 pdbonly 时生效）允许将调试器附加到正在运行的程序。 指定 pdbonly 后，可以在调试器中启动程序时进行源代码调试，但仅在正在运行的程序附加到调试器时才显示汇编程序。 使用此选项创建调试版本。 如果使用 Full，请注意，对经过优化的 JIT 代码的速度和大小会存在一定影响，使用 full 时对代码质量的影响较小 。 建议使用 pdbonly 或不使用 PDB 生成发布代码。 pdbonly 和 full 之间的一个区别在于，使用 full，编译器将发出 <xref:System.Diagnostics.DebuggableAttribute>，用于告知 JIT 编译器有可用调试信息  。 因此，在使用 full 时，如果代码包含设置为 false 的 <xref:System.Diagnostics.DebuggableAttribute>，将出现错误。 有关如何配置应用程序的调试性能的详细信息，请参阅[令映像更易于调试](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)。

## <a name="optimize"></a>优化

Optimize 选项启用或禁用编译器执行的优化，使输出文件更小、更快、更有效。 默认情况下，为发布生成配置启用 Optimize 选项。 默认情况下，为调试生成配置禁用此选项。

```xml
<Optimize>true</Optimize>
```

在 Visual Studio 中，可以从项目的“生成”属性页中设置 Optimize 选项。

Optimize 还指示公共语言运行时在运行时优化代码。 默认情况下，禁用优化。 指定 Optimize+ 可启用优化。 生成程序集使用的模块时，请使用与程序集所使用的相同 Optimize 设置。 可以将 Optimize 和 [Debug](#debugtype) 选项组合使用。

## <a name="deterministic"></a>具有确定性

如果输入相同，则会导致编译器生成的程序集其逐字节输出在整个编译期间中相同。

```xml
<Deterministic>true</Deterministic>
```

默认情况下，一组给定输入的编译器输出是唯一的，因为编译器会添加时间戳和随意数字生成的 MVID。 使用 `<Deterministic>` 选项生成确定性的程序集，只要输入保持不变，该程序集的二进制内容在整个编译中都是相同的  。 在此类生成中，时间戳和 MVID 字段会被替换为从所有编译输入的哈希派生的值。 编译器会考虑影响确定性的以下输入：

- 命令行参数序列。
- 编译器 .rsp 响应文件的内容。
- 所用编译器的精确版本及其引用的程序集。
- 当前目录路径。
- 直接或间接地显式传递到编译器的所有文件的二进制内容，包括：
  - 源文件
  - 引用的程序集
  - 引用的模块
  - 资源
  - 强名称密钥文件
  - @ 响应文件
  - 分析器
  - 规则集
  - 分析器可能使用的其他文件
- 当前区域性（针对生成诊断和异常消息的语言）。
- 在未指定编码情况下使用的默认编码（或当前代码页）。
- 编译器搜索路径（例如，由`-lib` 或 `-recurse` 指定）上文件是否存在及其内容。
- 运行编译器的公共语言运行时 (CLR) 平台。
- `%LIBPATH%` 的值，该值会影响分析器的依赖项加载。

可使用确定性编译来确定是否从可信源编译二进制内容。 当源公开可用时，确定性输出会很有用。 它还可以确定生成步骤是否依赖于生成过程中使用的二进制文件的更改。

## <a name="produceonlyreferenceassembly"></a>ProduceOnlyReferenceAssembly

ProduceOnlyReferenceAssembly 选项表示应输出引用程序集（而不是实现程序集）作为主输出。 ProduceOnlyReferenceAssembly 参数以无提示方式禁用输出 PDB，因为无法执行引用程序集。

```xml
<ProduceOnlyReferenceAssembly>true</ProduceOnlyReferenceAssembly>
```

引用程序集是一种特殊类型的程序集。 引用程序集只包含表示库的公共 API 外围应用所需的最少元数据量。 它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。 有关详细信息，请参阅[引用程序集](../../../standard/assembly/reference-assemblies.md)。

ProduceOnlyReferenceAssembly 和 [ProduceReferenceAssembly](output.md#producereferenceassembly) 选项是互斥的。
