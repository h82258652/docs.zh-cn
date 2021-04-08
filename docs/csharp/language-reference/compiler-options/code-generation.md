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
# <a name="c-compiler-options-that-control-code-generation"></a><span data-ttu-id="db34a-104">控制代码生成的 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="db34a-104">C# Compiler Options that control code generation</span></span>

<span data-ttu-id="db34a-105">下面的选项控制编译器生成的代码。</span><span class="sxs-lookup"><span data-stu-id="db34a-105">The following options control code generation by the compiler.</span></span> <span data-ttu-id="db34a-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="db34a-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="db34a-107">旧的 csc.exe 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="db34a-107">The older *csc.exe* syntax is shown in `code style`.</span></span>

- <span data-ttu-id="db34a-108">**DebugType** / `-debug`：发出（或不发出）调试信息。</span><span class="sxs-lookup"><span data-stu-id="db34a-108">**DebugType** / `-debug`: Emit (or don't emit) debugging information.</span></span>
- <span data-ttu-id="db34a-109">**Optimize** / `-optimize`：启用优化。</span><span class="sxs-lookup"><span data-stu-id="db34a-109">**Optimize** / `-optimize`: Enable optimizations.</span></span>
- <span data-ttu-id="db34a-110">**Deterministic** / `-deterministic`：从相同的输入源生成每字节对等的输出。</span><span class="sxs-lookup"><span data-stu-id="db34a-110">**Deterministic** / `-deterministic`: Produce byte-for-byte equivalent output from the same input source.</span></span>
- <span data-ttu-id="db34a-111">**ProduceOnlyReferenceAssembly** / `-refonly`：生成引用程序集作为主输出，而非生成完整程序集。</span><span class="sxs-lookup"><span data-stu-id="db34a-111">**ProduceOnlyReferenceAssembly** / `-refonly`: Produce a reference assembly, instead of a full assembly, as the primary output.</span></span>

## <a name="debugtype"></a><span data-ttu-id="db34a-112">DebugType</span><span class="sxs-lookup"><span data-stu-id="db34a-112">DebugType</span></span>

<span data-ttu-id="db34a-113">DebugType 选项将使编译器生成调试信息，并将此信息放置在一个或多个输出文件中。</span><span class="sxs-lookup"><span data-stu-id="db34a-113">The **DebugType** option causes the compiler to generate debugging information and place it in the output file or files.</span></span> <span data-ttu-id="db34a-114">默认情况下，将为调试生成配置添加调试信息。</span><span class="sxs-lookup"><span data-stu-id="db34a-114">Debugging information is added by default for the *Debug* build configuration.</span></span> <span data-ttu-id="db34a-115">默认情况下，为发布生成配置禁用调试信息。</span><span class="sxs-lookup"><span data-stu-id="db34a-115">It is off by default for the *Release* build configuration.</span></span>

```xml
<DebugType>pdbonly</DebugType>
```

<span data-ttu-id="db34a-116">自 C# 6.0 起，对于所有编译器版本而言，pdbonly 与 full 之间没有任何区别。</span><span class="sxs-lookup"><span data-stu-id="db34a-116">For all compiler versions starting with C# 6.0, there is no difference between *pdbonly* and *full*.</span></span> <span data-ttu-id="db34a-117">请选择 pdbonly。</span><span class="sxs-lookup"><span data-stu-id="db34a-117">Choose *pdbonly*.</span></span> <span data-ttu-id="db34a-118">若要更改 .pdb 文件的位置，请参阅 [PdbFile](./advanced.md#pdbfile)。</span><span class="sxs-lookup"><span data-stu-id="db34a-118">To change the location of the *.pdb* file, see [**PdbFile**](./advanced.md#pdbfile).</span></span>

<span data-ttu-id="db34a-119">以下为有效值：</span><span class="sxs-lookup"><span data-stu-id="db34a-119">The following values are valid:</span></span>

| <span data-ttu-id="db34a-120">Value</span><span class="sxs-lookup"><span data-stu-id="db34a-120">Value</span></span>      | <span data-ttu-id="db34a-121">含义</span><span class="sxs-lookup"><span data-stu-id="db34a-121">Meaning</span></span>                                                                                                 |
|------------|---------------------------------------------------------------------------------------------------------|
| `full`     | <span data-ttu-id="db34a-122">使用当前平台的默认格式向 _.pdb_ 文件发出调试信息：</span><span class="sxs-lookup"><span data-stu-id="db34a-122">Emit debugging information to _.pdb_ file using default format for the current platform:</span></span><br><span data-ttu-id="db34a-123">Windows：Windows pdb 文件。</span><span class="sxs-lookup"><span data-stu-id="db34a-123">**Windows**: A Windows pdb file.</span></span> <br><span data-ttu-id="db34a-124">Linux/macOS：[可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 文件。</span><span class="sxs-lookup"><span data-stu-id="db34a-124">**Linux/macOS**: A [Portable PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) file.</span></span> |
| `pdbonly`  | <span data-ttu-id="db34a-125">与 `full` 相同。</span><span class="sxs-lookup"><span data-stu-id="db34a-125">Same as `full`.</span></span> <span data-ttu-id="db34a-126">有关详细信息，请参阅下面的注释。</span><span class="sxs-lookup"><span data-stu-id="db34a-126">See the note below for more information.</span></span> |
| `portable` | <span data-ttu-id="db34a-127">使用跨平台[可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 格式向 .pdb 文件发出调试信息。</span><span class="sxs-lookup"><span data-stu-id="db34a-127">Emit debugging information to to .pdb file using cross-platform [Portable PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) format.</span></span> |
| `embedded` | <span data-ttu-id="db34a-128">使用 [可移植 PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) 格式向 _.dll/.exe_ 自身（未生成 _.pdb_ 文件）发出调试信息。</span><span class="sxs-lookup"><span data-stu-id="db34a-128">Emit debugging information into the _.dll/.exe_ itself (_.pdb_ file is not produced) using [Portable PDB](https://github.com/dotnet/core/blob/main/Documentation/diagnostics/portable_pdb.md) format.</span></span> |

> [!IMPORTANT]
> <span data-ttu-id="db34a-129">以下信息仅适用于 C# 6.0 以前的编译器。</span><span class="sxs-lookup"><span data-stu-id="db34a-129">The following information applies only to compilers older than C# 6.0.</span></span>
> <span data-ttu-id="db34a-130">此元素的值可以是 `full` 或 `pdbonly`。</span><span class="sxs-lookup"><span data-stu-id="db34a-130">The value of this element can be either `full` or `pdbonly`.</span></span> <span data-ttu-id="db34a-131">full 参数（在不指定 pdbonly 时生效）允许将调试器附加到正在运行的程序。</span><span class="sxs-lookup"><span data-stu-id="db34a-131">The *full* argument, which is in effect if you don't specify *pdbonly*, enables attaching a debugger to the running program.</span></span> <span data-ttu-id="db34a-132">指定 pdbonly 后，可以在调试器中启动程序时进行源代码调试，但仅在正在运行的程序附加到调试器时才显示汇编程序。</span><span class="sxs-lookup"><span data-stu-id="db34a-132">Specifying *pdbonly* allows source code debugging when the program is started in the debugger but will only display assembler when the running program is attached to the debugger.</span></span> <span data-ttu-id="db34a-133">使用此选项创建调试版本。</span><span class="sxs-lookup"><span data-stu-id="db34a-133">Use this option to create debug builds.</span></span> <span data-ttu-id="db34a-134">如果使用 Full，请注意，对经过优化的 JIT 代码的速度和大小会存在一定影响，使用 full 时对代码质量的影响较小 。</span><span class="sxs-lookup"><span data-stu-id="db34a-134">If you use *Full*, be aware that there's some impact on the speed and size of JIT optimized code and a small impact on code quality with *full*.</span></span> <span data-ttu-id="db34a-135">建议使用 pdbonly 或不使用 PDB 生成发布代码。</span><span class="sxs-lookup"><span data-stu-id="db34a-135">We recommend *pdbonly* or no PDB for generating release code.</span></span> <span data-ttu-id="db34a-136">pdbonly 和 full 之间的一个区别在于，使用 full，编译器将发出 <xref:System.Diagnostics.DebuggableAttribute>，用于告知 JIT 编译器有可用调试信息  。</span><span class="sxs-lookup"><span data-stu-id="db34a-136">One difference between *pdbonly* and *full* is that with *full* the compiler emits a <xref:System.Diagnostics.DebuggableAttribute>, which is used to tell the JIT compiler that debug information is available.</span></span> <span data-ttu-id="db34a-137">因此，在使用 full 时，如果代码包含设置为 false 的 <xref:System.Diagnostics.DebuggableAttribute>，将出现错误。</span><span class="sxs-lookup"><span data-stu-id="db34a-137">Therefore, you will get an error if your code contains the <xref:System.Diagnostics.DebuggableAttribute> set to false if you use *full*.</span></span> <span data-ttu-id="db34a-138">有关如何配置应用程序的调试性能的详细信息，请参阅[令映像更易于调试](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md)。</span><span class="sxs-lookup"><span data-stu-id="db34a-138">For more information on how to configure the debug performance of an application, see [Making an Image Easier to Debug](../../../framework/debug-trace-profile/making-an-image-easier-to-debug.md).</span></span>

## <a name="optimize"></a><span data-ttu-id="db34a-139">优化</span><span class="sxs-lookup"><span data-stu-id="db34a-139">Optimize</span></span>

<span data-ttu-id="db34a-140">Optimize 选项启用或禁用编译器执行的优化，使输出文件更小、更快、更有效。</span><span class="sxs-lookup"><span data-stu-id="db34a-140">The **Optimize** option enables or disables optimizations performed by the compiler to make your output file smaller, faster, and more efficient.</span></span> <span data-ttu-id="db34a-141">默认情况下，为发布生成配置启用 Optimize 选项。</span><span class="sxs-lookup"><span data-stu-id="db34a-141">The *Optimize* option is enabled by default for a *Release* build configuration.</span></span> <span data-ttu-id="db34a-142">默认情况下，为调试生成配置禁用此选项。</span><span class="sxs-lookup"><span data-stu-id="db34a-142">It is off by default for a *Debug* build configuration.</span></span>

```xml
<Optimize>true</Optimize>
```

<span data-ttu-id="db34a-143">在 Visual Studio 中，可以从项目的“生成”属性页中设置 Optimize 选项。</span><span class="sxs-lookup"><span data-stu-id="db34a-143">You set the **Optimize** option from **Build** properties page for your project in Visual Studio.</span></span>

<span data-ttu-id="db34a-144">Optimize 还指示公共语言运行时在运行时优化代码。</span><span class="sxs-lookup"><span data-stu-id="db34a-144">**Optimize** also tells the common language runtime to optimize code at runtime.</span></span> <span data-ttu-id="db34a-145">默认情况下，禁用优化。</span><span class="sxs-lookup"><span data-stu-id="db34a-145">By default, optimizations are disabled.</span></span> <span data-ttu-id="db34a-146">指定 Optimize+ 可启用优化。</span><span class="sxs-lookup"><span data-stu-id="db34a-146">Specify **Optimize+** to enable optimizations.</span></span> <span data-ttu-id="db34a-147">生成程序集使用的模块时，请使用与程序集所使用的相同 Optimize 设置。</span><span class="sxs-lookup"><span data-stu-id="db34a-147">When building a module to be used by an assembly, use the same **Optimize** settings as used by the assembly.</span></span> <span data-ttu-id="db34a-148">可以将 Optimize 和 [Debug](#debugtype) 选项组合使用。</span><span class="sxs-lookup"><span data-stu-id="db34a-148">It's possible to combine the **Optimize** and [**Debug**](#debugtype) options.</span></span>

## <a name="deterministic"></a><span data-ttu-id="db34a-149">具有确定性</span><span class="sxs-lookup"><span data-stu-id="db34a-149">Deterministic</span></span>

<span data-ttu-id="db34a-150">如果输入相同，则会导致编译器生成的程序集其逐字节输出在整个编译期间中相同。</span><span class="sxs-lookup"><span data-stu-id="db34a-150">Causes the compiler to produce an assembly whose byte-for-byte output is identical across compilations for identical inputs.</span></span>

```xml
<Deterministic>true</Deterministic>
```

<span data-ttu-id="db34a-151">默认情况下，一组给定输入的编译器输出是唯一的，因为编译器会添加时间戳和随意数字生成的 MVID。</span><span class="sxs-lookup"><span data-stu-id="db34a-151">By default, compiler output from a given set of inputs is unique, since the compiler adds a timestamp and an MVID that is generated from random numbers.</span></span> <span data-ttu-id="db34a-152">使用 `<Deterministic>` 选项生成确定性的程序集，只要输入保持不变，该程序集的二进制内容在整个编译中都是相同的  。</span><span class="sxs-lookup"><span data-stu-id="db34a-152">You use the `<Deterministic>` option to produce a *deterministic assembly*, one whose binary content is identical across compilations as long as the input remains the same.</span></span> <span data-ttu-id="db34a-153">在此类生成中，时间戳和 MVID 字段会被替换为从所有编译输入的哈希派生的值。</span><span class="sxs-lookup"><span data-stu-id="db34a-153">In such a build, the timestamp and MVID fields will be replaced with values derived from a hash of all the compilation inputs.</span></span> <span data-ttu-id="db34a-154">编译器会考虑影响确定性的以下输入：</span><span class="sxs-lookup"><span data-stu-id="db34a-154">The compiler considers the following inputs that affect determinism:</span></span>

- <span data-ttu-id="db34a-155">命令行参数序列。</span><span class="sxs-lookup"><span data-stu-id="db34a-155">The sequence of command-line parameters.</span></span>
- <span data-ttu-id="db34a-156">编译器 .rsp 响应文件的内容。</span><span class="sxs-lookup"><span data-stu-id="db34a-156">The contents of the compiler's .rsp response file.</span></span>
- <span data-ttu-id="db34a-157">所用编译器的精确版本及其引用的程序集。</span><span class="sxs-lookup"><span data-stu-id="db34a-157">The precise version of the compiler used, and its referenced assemblies.</span></span>
- <span data-ttu-id="db34a-158">当前目录路径。</span><span class="sxs-lookup"><span data-stu-id="db34a-158">The current directory path.</span></span>
- <span data-ttu-id="db34a-159">直接或间接地显式传递到编译器的所有文件的二进制内容，包括：</span><span class="sxs-lookup"><span data-stu-id="db34a-159">The binary contents of all files explicitly passed to the compiler either directly or indirectly, including:</span></span>
  - <span data-ttu-id="db34a-160">源文件</span><span class="sxs-lookup"><span data-stu-id="db34a-160">Source files</span></span>
  - <span data-ttu-id="db34a-161">引用的程序集</span><span class="sxs-lookup"><span data-stu-id="db34a-161">Referenced assemblies</span></span>
  - <span data-ttu-id="db34a-162">引用的模块</span><span class="sxs-lookup"><span data-stu-id="db34a-162">Referenced modules</span></span>
  - <span data-ttu-id="db34a-163">资源</span><span class="sxs-lookup"><span data-stu-id="db34a-163">Resources</span></span>
  - <span data-ttu-id="db34a-164">强名称密钥文件</span><span class="sxs-lookup"><span data-stu-id="db34a-164">The strong name key file</span></span>
  - <span data-ttu-id="db34a-165">@ 响应文件</span><span class="sxs-lookup"><span data-stu-id="db34a-165">@ response files</span></span>
  - <span data-ttu-id="db34a-166">分析器</span><span class="sxs-lookup"><span data-stu-id="db34a-166">Analyzers</span></span>
  - <span data-ttu-id="db34a-167">规则集</span><span class="sxs-lookup"><span data-stu-id="db34a-167">Rulesets</span></span>
  - <span data-ttu-id="db34a-168">分析器可能使用的其他文件</span><span class="sxs-lookup"><span data-stu-id="db34a-168">Other files that may be used by analyzers</span></span>
- <span data-ttu-id="db34a-169">当前区域性（针对生成诊断和异常消息的语言）。</span><span class="sxs-lookup"><span data-stu-id="db34a-169">The current culture (for the language in which diagnostics and exception messages are produced).</span></span>
- <span data-ttu-id="db34a-170">在未指定编码情况下使用的默认编码（或当前代码页）。</span><span class="sxs-lookup"><span data-stu-id="db34a-170">The default encoding (or the current code page) if the encoding isn't specified.</span></span>
- <span data-ttu-id="db34a-171">编译器搜索路径（例如，由`-lib` 或 `-recurse` 指定）上文件是否存在及其内容。</span><span class="sxs-lookup"><span data-stu-id="db34a-171">The existence, non-existence, and contents of files on the compiler's search paths (specified, for example, by `-lib` or `-recurse`).</span></span>
- <span data-ttu-id="db34a-172">运行编译器的公共语言运行时 (CLR) 平台。</span><span class="sxs-lookup"><span data-stu-id="db34a-172">The Common Language Runtime (CLR) platform on which the compiler is run.</span></span>
- <span data-ttu-id="db34a-173">`%LIBPATH%` 的值，该值会影响分析器的依赖项加载。</span><span class="sxs-lookup"><span data-stu-id="db34a-173">The value of `%LIBPATH%`, which can affect analyzer dependency loading.</span></span>

<span data-ttu-id="db34a-174">可使用确定性编译来确定是否从可信源编译二进制内容。</span><span class="sxs-lookup"><span data-stu-id="db34a-174">Deterministic compilation can be used for establishing whether a binary is compiled from a trusted source.</span></span> <span data-ttu-id="db34a-175">当源公开可用时，确定性输出会很有用。</span><span class="sxs-lookup"><span data-stu-id="db34a-175">Deterministic output can be useful when the source is publicly available.</span></span> <span data-ttu-id="db34a-176">它还可以确定生成步骤是否依赖于生成过程中使用的二进制文件的更改。</span><span class="sxs-lookup"><span data-stu-id="db34a-176">It can also determine whether build steps that are dependent on changes to binary used in the build process.</span></span>

## <a name="produceonlyreferenceassembly"></a><span data-ttu-id="db34a-177">ProduceOnlyReferenceAssembly</span><span class="sxs-lookup"><span data-stu-id="db34a-177">ProduceOnlyReferenceAssembly</span></span>

<span data-ttu-id="db34a-178">ProduceOnlyReferenceAssembly 选项表示应输出引用程序集（而不是实现程序集）作为主输出。</span><span class="sxs-lookup"><span data-stu-id="db34a-178">The **ProduceOnlyReferenceAssembly** option indicates that a reference assembly should be output instead of an implementation assembly, as the primary output.</span></span> <span data-ttu-id="db34a-179">ProduceOnlyReferenceAssembly 参数以无提示方式禁用输出 PDB，因为无法执行引用程序集。</span><span class="sxs-lookup"><span data-stu-id="db34a-179">The **ProduceOnlyReferenceAssembly** parameter silently disables outputting PDBs, as reference assemblies cannot be executed.</span></span>

```xml
<ProduceOnlyReferenceAssembly>true</ProduceOnlyReferenceAssembly>
```

<span data-ttu-id="db34a-180">引用程序集是一种特殊类型的程序集。</span><span class="sxs-lookup"><span data-stu-id="db34a-180">Reference assemblies are a special type of assembly.</span></span> <span data-ttu-id="db34a-181">引用程序集只包含表示库的公共 API 外围应用所需的最少元数据量。</span><span class="sxs-lookup"><span data-stu-id="db34a-181">Reference assemblies contain only the minimum amount of metadata required to represent the library's public API surface.</span></span> <span data-ttu-id="db34a-182">它们包括在生成工具中引用程序集时所需的所有成员的声明，但不包括所有成员实现以及对其 API 协定没有明显影响的私有成员的声明。</span><span class="sxs-lookup"><span data-stu-id="db34a-182">They include declarations for all members that are significant when referencing an assembly in build tools, but exclude all member implementations and declarations of private members that have no observable impact on their API contract.</span></span> <span data-ttu-id="db34a-183">有关详细信息，请参阅[引用程序集](../../../standard/assembly/reference-assemblies.md)。</span><span class="sxs-lookup"><span data-stu-id="db34a-183">For more information, see [Reference assemblies](../../../standard/assembly/reference-assemblies.md).</span></span>

<span data-ttu-id="db34a-184">ProduceOnlyReferenceAssembly 和 [ProduceReferenceAssembly](output.md#producereferenceassembly) 选项是互斥的。</span><span class="sxs-lookup"><span data-stu-id="db34a-184">The **ProduceOnlyReferenceAssembly** and [**ProduceReferenceAssembly**](output.md#producereferenceassembly) options are mutually exclusive.</span></span>
