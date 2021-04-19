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
# <a name="advanced-c-compiler-options"></a><span data-ttu-id="98f05-104">高级 C# 编译器选项</span><span class="sxs-lookup"><span data-stu-id="98f05-104">Advanced C# compiler options</span></span>

<span data-ttu-id="98f05-105">以下选项支持高级方案。</span><span class="sxs-lookup"><span data-stu-id="98f05-105">The following options support advanced scenarios.</span></span> <span data-ttu-id="98f05-106">新的 MSBuild 语法以粗体显示。</span><span class="sxs-lookup"><span data-stu-id="98f05-106">The new MSBuild syntax is shown in **Bold**.</span></span> <span data-ttu-id="98f05-107">旧的 `csc.exe` 语法以 `code style` 显示。</span><span class="sxs-lookup"><span data-stu-id="98f05-107">The older `csc.exe` syntax is shown in `code style`.</span></span>

- <span data-ttu-id="98f05-108">**MainEntryPoint**、**StartupObject** / `-main`：指定包含入口点的类型。</span><span class="sxs-lookup"><span data-stu-id="98f05-108">**MainEntryPoint**, **StartupObject** / `-main`: Specify the type that contains the entry point.</span></span>
- <span data-ttu-id="98f05-109">**PdbFile** / `-pdb`：指定调试信息文件名。</span><span class="sxs-lookup"><span data-stu-id="98f05-109">**PdbFile** / `-pdb`: Specify debug information file name.</span></span>
- <span data-ttu-id="98f05-110">**PathMap** / `-pathmap`：指定编译器输出的源路径名的映射。</span><span class="sxs-lookup"><span data-stu-id="98f05-110">**PathMap** / `-pathmap`: Specify a mapping for source path names output by the compiler.</span></span>
- <span data-ttu-id="98f05-111">**ApplicationConfiguration** / `-appconfig`：指定包含程序集绑定设置的应用程序配置文件。</span><span class="sxs-lookup"><span data-stu-id="98f05-111">**ApplicationConfiguration** / `-appconfig`: Specify an application configuration file containing assembly binding settings.</span></span>
- <span data-ttu-id="98f05-112">**AdditionalLibPaths** / `-lib`：指定要在其中搜索引用的其他目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-112">**AdditionalLibPaths** / `-lib`: Specify additional directories to search in for references.</span></span>
- <span data-ttu-id="98f05-113">**GenerateFullPaths** / `-fullpath`：编译器生成完全限定的路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-113">**GenerateFullPaths** / `-fullpath`: Compiler generates fully qualified paths.</span></span>
- <span data-ttu-id="98f05-114">**PreferredUILang** / `-preferreduilang`：指定首选输出语言名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-114">**PreferredUILang** / `-preferreduilang`: Specify the preferred output language name.</span></span>
- <span data-ttu-id="98f05-115">**BaseAddress** / `-baseaddress`：指定要生成的库的基址。</span><span class="sxs-lookup"><span data-stu-id="98f05-115">**BaseAddress** / `-baseaddress`: Specify the base address for the library to be built.</span></span>
- <span data-ttu-id="98f05-116">**ChecksumAlgorithm** / `-checksumalgorithm`：指定用于计算 PDB 中存储的源文件校验和的算法。</span><span class="sxs-lookup"><span data-stu-id="98f05-116">**ChecksumAlgorithm** / `-checksumalgorithm` : Specify algorithm for calculating source file checksum stored in PDB.</span></span>
- <span data-ttu-id="98f05-117">**CodePage** / `-codepage`：指定在打开源文件时使用的代码页。</span><span class="sxs-lookup"><span data-stu-id="98f05-117">**CodePage** / `-codepage`: Specify the codepage to use when opening source files.</span></span>
- <span data-ttu-id="98f05-118">**Utf8Output** / `-utf8output`：以 UTF-8 编码输出编译器消息。</span><span class="sxs-lookup"><span data-stu-id="98f05-118">**Utf8Output** / `-utf8output`: Output compiler messages in UTF-8 encoding.</span></span>
- <span data-ttu-id="98f05-119">**FileAlignment** / `-filealign`：指定用于输出文件节的对齐方式。</span><span class="sxs-lookup"><span data-stu-id="98f05-119">**FileAlignment** / `-filealign`: Specify the alignment used for output file sections.</span></span>
- <span data-ttu-id="98f05-120">**ErrorEndLocation** / `-errorendlocation`：输出每个错误结尾位置的行和列。</span><span class="sxs-lookup"><span data-stu-id="98f05-120">**ErrorEndLocation** / `-errorendlocation`: Output line and column of the end location of each error.</span></span>
- <span data-ttu-id="98f05-121">**NoStandardLib** / `-nostdlib`：不引用标准库 mscorlib.dll。</span><span class="sxs-lookup"><span data-stu-id="98f05-121">**NoStandardLib** / `-nostdlib`: Don't reference standard library *mscorlib.dll*.</span></span>
- <span data-ttu-id="98f05-122">**SubsystemVersion** / `-subsystemversion`：指定此程序集的子系统版本。</span><span class="sxs-lookup"><span data-stu-id="98f05-122">**SubsystemVersion** / `-subsystemversion`: Specify subsystem version of this assembly.</span></span>
- <span data-ttu-id="98f05-123">**ModuleAssemblyName** / `-moduleassemblyname`：此模块所属程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-123">**ModuleAssemblyName** / `-moduleassemblyname`: Name of the assembly that this module will be a part of.</span></span>

## <a name="mainentrypoint-or-startupobject"></a><span data-ttu-id="98f05-124">MainEntryPoint 或 StartupObject</span><span class="sxs-lookup"><span data-stu-id="98f05-124">MainEntryPoint or StartupObject</span></span>

<span data-ttu-id="98f05-125">如果多个类包含 `Main` 方法，此选项将指定包含程序入口点的类。</span><span class="sxs-lookup"><span data-stu-id="98f05-125">This option specifies the class that contains the entry point to the program, if more than one class contains a `Main` method.</span></span>

```xml
<StartupObject>MyNamespace.Program</StartupObject>
```

<span data-ttu-id="98f05-126">或</span><span class="sxs-lookup"><span data-stu-id="98f05-126">or</span></span>

```xml
<MainEntryPoint>MyNamespace.Program</MainEntryPoint>
```

<span data-ttu-id="98f05-127">其中 `Program` 是包含 `Main` 方法的类型。</span><span class="sxs-lookup"><span data-stu-id="98f05-127">Where `Program` is the type that contains the `Main` method.</span></span> <span data-ttu-id="98f05-128">提供的类名必须是完全限定类名；它必须包括完整命名空间（包含类），后跟类名。</span><span class="sxs-lookup"><span data-stu-id="98f05-128">The provided class name must be fully qualified; it must include the full namespace containing the class, followed by the class name.</span></span> <span data-ttu-id="98f05-129">例如，当 `Main` 方法位于 `MyApplication.Core` 命名空间中的 `Program` 类中时，编译器选项必须为 `-main:MyApplication.Core.Program`。</span><span class="sxs-lookup"><span data-stu-id="98f05-129">For example, when the `Main` method is located inside the `Program` class in the `MyApplication.Core` namespace, the compiler option has to be `-main:MyApplication.Core.Program`.</span></span> <span data-ttu-id="98f05-130">如果编译包含具有 [`Main`](../../programming-guide/main-and-command-args/index.md) 方法的多个类型，则可以指定哪个类型包含 `Main` 方法。</span><span class="sxs-lookup"><span data-stu-id="98f05-130">If your compilation includes more than one type with a [`Main`](../../programming-guide/main-and-command-args/index.md) method, you can specify which type contains the `Main` method.</span></span>

> [!NOTE]
> <span data-ttu-id="98f05-131">此选项不能用于包含[顶级语句](../../programming-guide/main-and-command-args/top-level-statements.md)的项目，即使该项目包含一个或多个 `Main` 方法也是如此。</span><span class="sxs-lookup"><span data-stu-id="98f05-131">This option can't be used for a project that includes [top-level statements](../../programming-guide/main-and-command-args/top-level-statements.md), even if that project contains one or more `Main` methods.</span></span>

## <a name="pdbfile"></a><span data-ttu-id="98f05-132">PdbFile</span><span class="sxs-lookup"><span data-stu-id="98f05-132">PdbFile</span></span>

<span data-ttu-id="98f05-133">PdbFile 编译器选项指定调试符号文件的名称和位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-133">The **PdbFile** compiler option specifies the name and location of the debug symbols file.</span></span>  <span data-ttu-id="98f05-134">`filename` 值指定调试符号文件的名称和位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-134">The `filename` value specifies the name and location of the debug symbols file.</span></span>  

```xml
<PdbFile>filename</PdbFile>
```

<span data-ttu-id="98f05-135">指定 [DebugType](code-generation.md#debugtype) 后，编译器将在创建输出文件（.exe 或 .dll）的相同目录中创建 .pdb 文件。</span><span class="sxs-lookup"><span data-stu-id="98f05-135">When you specify [**DebugType**](code-generation.md#debugtype), the compiler will create a *.pdb* file in the same directory where the compiler will create the output file (.exe or .dll).</span></span> <span data-ttu-id="98f05-136">.pdb 文件与输出文件的名称具有相同的基文件名。</span><span class="sxs-lookup"><span data-stu-id="98f05-136">The *.pdb* file has the same base file name as the name of the output file.</span></span> <span data-ttu-id="98f05-137">PdbFile 允许为 .pdb 文件指定非默认的文件名和位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-137">**PdbFile** allows you to specify a non-default file name and location for the .pdb file.</span></span> <span data-ttu-id="98f05-138">不能在 Visual Studio 开发环境中设置此编译器选项，也不能以编程方式对其进行更改。</span><span class="sxs-lookup"><span data-stu-id="98f05-138">This compiler option cannot be set in the Visual Studio development environment, nor can it be changed programmatically.</span></span>  

## <a name="pathmap"></a><span data-ttu-id="98f05-139">PathMap</span><span class="sxs-lookup"><span data-stu-id="98f05-139">PathMap</span></span>

<span data-ttu-id="98f05-140">PathMap 编译器选项指定如何将物理路径映射到编译器输出的源路径名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-140">The **PathMap** compiler option specifies how to map physical paths to source path names output by the compiler.</span></span> <span data-ttu-id="98f05-141">此选项将编译器在其上运行的计算机上的每个物理路径映射到应写入输出文件的相应路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-141">This option maps each physical path on the machine where the compiler runs to a corresponding path that should be written in the output files.</span></span> <span data-ttu-id="98f05-142">在下面的示例中，`path1` 是当前环境中源文件的完整路径，`sourcePath1` 是任何输出文件中替代 `path1` 的源路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-142">In the following example, `path1` is the full path to the source files in the current environment, and `sourcePath1` is the source path substituted for `path1` in any output files.</span></span> <span data-ttu-id="98f05-143">若要指定多个映射的源路径，请用分号分隔每个路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-143">To specify multiple mapped source paths, separate each with a semicolon.</span></span>

```xml
<PathMap>path1=sourcePath1;path2=sourcePath2</PathMap>
```

<span data-ttu-id="98f05-144">编译器将源路径写入其输出，原因如下：</span><span class="sxs-lookup"><span data-stu-id="98f05-144">The compiler writes the source path into its output for the following reasons:</span></span>

1. <span data-ttu-id="98f05-145">将 <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> 应用于可选参数时，会将源路径替换为参数。</span><span class="sxs-lookup"><span data-stu-id="98f05-145">The source path is substituted for an argument when the <xref:System.Runtime.CompilerServices.CallerFilePathAttribute> is applied to an optional parameter.</span></span>
1. <span data-ttu-id="98f05-146">PDB 文件中嵌入的源路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-146">The source path is embedded in a PDB file.</span></span>
1. <span data-ttu-id="98f05-147">PDB 文件的路径嵌入到 PE（可移植的可执行文件）文件中。</span><span class="sxs-lookup"><span data-stu-id="98f05-147">The path of the PDB file is embedded into a PE (portable executable) file.</span></span>

## <a name="applicationconfiguration"></a><span data-ttu-id="98f05-148">ApplicationConfiguration</span><span class="sxs-lookup"><span data-stu-id="98f05-148">ApplicationConfiguration</span></span>

<span data-ttu-id="98f05-149">ApplicationConfiguration 编译器选项使 C# 应用程序能够在程序集绑定时将程序集的应用程序配置 (app.config) 文件的位置指定为公共语言运行时 (CLR)。</span><span class="sxs-lookup"><span data-stu-id="98f05-149">The **ApplicationConfiguration** compiler option enables a C# application to specify the location of an assembly's application configuration (app.config) file to the common language runtime (CLR) at assembly binding time.</span></span>

```xml
<ApplicationConfiguration>file</ApplicationConfiguration>
```

<span data-ttu-id="98f05-150">其中 `file` 是包含程序集绑定设置的应用程序配置文件。</span><span class="sxs-lookup"><span data-stu-id="98f05-150">Where `file` is the application configuration file that contains assembly binding settings.</span></span> <span data-ttu-id="98f05-151">ApplicationConfiguration 的一种用途是处理高级方案，在这种方案中，程序集必须同时引用特定引用程序集的 .NET Framework 版本和 .NET Framework for Silverlight 版本。</span><span class="sxs-lookup"><span data-stu-id="98f05-151">One use of **ApplicationConfiguration** is advanced scenarios in which an assembly has to reference both the .NET Framework version and the .NET Framework for Silverlight version of a particular reference assembly at the same time.</span></span> <span data-ttu-id="98f05-152">例如，在 Windows Presentation Foundation (WPF) 中编写的 XAML 设计器可能需要为设计器用户界面引用 WPF 桌面以及随附于 Silverlight 的 WPF 子集。</span><span class="sxs-lookup"><span data-stu-id="98f05-152">For example, a XAML designer written in Windows Presentation Foundation (WPF) might have to reference both the WPF Desktop, for the designer's user interface, and the subset of WPF that is included with Silverlight.</span></span> <span data-ttu-id="98f05-153">同一设计器程序集必须访问这两个程序集。</span><span class="sxs-lookup"><span data-stu-id="98f05-153">The same designer assembly has to access both assemblies.</span></span> <span data-ttu-id="98f05-154">默认情况下，单独引用会导致编译器错误，因为程序集绑定将这两个程序集视为等效。</span><span class="sxs-lookup"><span data-stu-id="98f05-154">By default, the separate references cause a compiler error, because assembly binding sees the two assemblies as equivalent.</span></span> <span data-ttu-id="98f05-155">借助 ApplicationConfiguration 编译器选项，可通过使用 `<supportPortability>` 标记指定禁用默认行为的 app.config 文件的位置，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="98f05-155">The **ApplicationConfiguration** compiler option enables you to specify the location of an app.config file that disables the default behavior by using a `<supportPortability>` tag, as shown in the following example.</span></span>

```xml
<supportPortability PKT="7cec85d7bea7798e" enable="false"/>
```

<span data-ttu-id="98f05-156">编译器将文件的位置传递给 CLR 的程序集绑定逻辑。</span><span class="sxs-lookup"><span data-stu-id="98f05-156">The compiler passes the location of the file to the CLR's assembly-binding logic.</span></span>

> [!NOTE]
> <span data-ttu-id="98f05-157">若要使用已在项目中设置的 app.config 文件，请将属性标记 `<UseAppConfigForCompiler>` 添加到 .csproj 文件，并将其值设置为 `true`。</span><span class="sxs-lookup"><span data-stu-id="98f05-157">To use the app.config file that is already set in the project, add property tag `<UseAppConfigForCompiler>` to the *.csproj* file and set its value to `true`.</span></span> <span data-ttu-id="98f05-158">若要指定不同的 app.config 文件，请添加属性标记 `<AppConfigForCompiler>` 并将其值设置为该文件的位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-158">To specify a different app.config file, add property tag `<AppConfigForCompiler>` and set its value to the location of the file.</span></span>

<span data-ttu-id="98f05-159">以下示例展示一个 app.config 文件，通过使用该文件，应用程序能够同时引用任何 .NET Framework 程序集（同时存在于后述两个实现中）的 .NET Framework 实现和 .NET Framework for Silverlight 实现。</span><span class="sxs-lookup"><span data-stu-id="98f05-159">The following example shows an app.config file that enables an application to have references to both the .NET Framework implementation and the .NET Framework for Silverlight implementation of any .NET Framework assembly that exists in both implementations.</span></span> <span data-ttu-id="98f05-160">ApplicationConfiguration 编译器选项指定此 app.config 文件的位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-160">The **ApplicationConfiguration** compiler option specifies the location of this app.config file.</span></span>

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

## <a name="additionallibpaths"></a><span data-ttu-id="98f05-161">AdditionalLibPaths</span><span class="sxs-lookup"><span data-stu-id="98f05-161">AdditionalLibPaths</span></span>

<span data-ttu-id="98f05-162">AdditionalLibPaths 选项指定通过 [References](inputs.md#references) 选项引用的程序集的位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-162">The **AdditionalLibPaths** option specifies the location of assemblies referenced with the [**References**](inputs.md#references) option.</span></span>

```xml
<AdditionalLibPaths>dir1[,dir2]</AdditionalLibPaths>
```

<span data-ttu-id="98f05-163">其中 `dir1` 是在当前工作目录（从中调用编译器的目录）或公共语言运行时的系统目录中未找到引用程序集时，编译器将在其中进行查找的目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-163">Where `dir1` is a directory for the compiler to look in if a referenced assembly isn't found in the current working directory (the directory from which you're invoking the compiler) or in the common language runtime's system directory.</span></span> <span data-ttu-id="98f05-164">`dir2` 是要在其中搜索程序集引用的一个或多个其他目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-164">`dir2` is one or more additional directories to search in for assembly references.</span></span> <span data-ttu-id="98f05-165">用逗号分隔每个目录名称，且中间没有空格。</span><span class="sxs-lookup"><span data-stu-id="98f05-165">Separate directory names with a comma, and without white space between them.</span></span> <span data-ttu-id="98f05-166">编译器按以下顺序搜索未完全限定的程序集引用：</span><span class="sxs-lookup"><span data-stu-id="98f05-166">The compiler searches for assembly references that aren't fully qualified in the following order:</span></span>  

1. <span data-ttu-id="98f05-167">当前工作目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-167">Current working directory.</span></span>
1. <span data-ttu-id="98f05-168">公共语言运行时系统目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-168">The common language runtime system directory.</span></span>
1. <span data-ttu-id="98f05-169">由 AdditionalLibPaths 指定的目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-169">Directories specified by **AdditionalLibPaths**.</span></span>
1. <span data-ttu-id="98f05-170">由 LIB 环境变量指定的目录。</span><span class="sxs-lookup"><span data-stu-id="98f05-170">Directories specified by the LIB environment variable.</span></span>

<span data-ttu-id="98f05-171">使用 Reference 指定程序集引用。</span><span class="sxs-lookup"><span data-stu-id="98f05-171">Use **Reference** to specify an assembly reference.</span></span> <span data-ttu-id="98f05-172">AdditionalLibPaths 是累加的。</span><span class="sxs-lookup"><span data-stu-id="98f05-172">**AdditionalLibPaths** is additive.</span></span> <span data-ttu-id="98f05-173">每一次指定的值都追加到以前的值中。</span><span class="sxs-lookup"><span data-stu-id="98f05-173">Specifying it more than once appends to any prior values.</span></span> <span data-ttu-id="98f05-174">由于程序集清单中未指定依赖程序集的路径，因此应用程序将查找并使用全局程序集缓存中的程序集。</span><span class="sxs-lookup"><span data-stu-id="98f05-174">Since the path to the dependent assembly isn't specified in the assembly manifest, the application will find and use the assembly in the global assembly cache.</span></span> <span data-ttu-id="98f05-175">编译器可以引用程序集并不表示公共语言运行时可以在运行时找到并加载程序集。</span><span class="sxs-lookup"><span data-stu-id="98f05-175">The compiler referencing the assembly doesn't imply the common language runtime can find and load the assembly at runtime.</span></span> <span data-ttu-id="98f05-176">有关运行时如何搜索引用的程序集的详细信息，请参阅[运行时如何定位程序集](../../../framework/deployment/how-the-runtime-locates-assemblies.md)。</span><span class="sxs-lookup"><span data-stu-id="98f05-176">See [How the Runtime Locates Assemblies](../../../framework/deployment/how-the-runtime-locates-assemblies.md) for details on how the runtime searches for referenced assemblies.</span></span>  

## <a name="generatefullpaths"></a><span data-ttu-id="98f05-177">GenerateFullPaths</span><span class="sxs-lookup"><span data-stu-id="98f05-177">GenerateFullPaths</span></span>

<span data-ttu-id="98f05-178">GenerateFullPaths 选项导致编译器在列出编译错误和警告时指定文件的完整路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-178">The **GenerateFullPaths** option causes the compiler to specify the full path to the file when listing compilation errors and warnings.</span></span>
  
```Xml
<GenerateFullPaths>true</GenerateFullPaths>
```

<span data-ttu-id="98f05-179">默认情况下，由编译所产生的错误和警告指定其中发现错误的文件的名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-179">By default, errors and warnings that result from compilation specify the name of the file in which an error was found.</span></span> <span data-ttu-id="98f05-180">GenerateFullPaths 选项导致编译器指定文件的完整路径。</span><span class="sxs-lookup"><span data-stu-id="98f05-180">The **GenerateFullPaths** option causes the compiler to specify the full path to the file.</span></span> <span data-ttu-id="98f05-181">此编译器选项在 Visual Studio 中不可用，并且无法以编程方式更改。</span><span class="sxs-lookup"><span data-stu-id="98f05-181">This compiler option is unavailable in Visual Studio and cannot be changed programmatically.</span></span>

## <a name="preferreduilang"></a><span data-ttu-id="98f05-182">PreferredUILang</span><span class="sxs-lookup"><span data-stu-id="98f05-182">PreferredUILang</span></span>

<span data-ttu-id="98f05-183">通过使用 PreferredUILang 编译器选项，可指定 C# 编译器用于显示输出（如错误消息）的语言。</span><span class="sxs-lookup"><span data-stu-id="98f05-183">By using the **PreferredUILang** compiler option, you can specify the language in which the C# compiler displays output, such as error messages.</span></span>

```xml
<PreferredUILang>language</PreferredUILang>
```

<span data-ttu-id="98f05-184">其中 `language` 是用于编译器输出的语言的[语言名称](/windows/desktop/Intl/language-names)。</span><span class="sxs-lookup"><span data-stu-id="98f05-184">Where `language` is the [language name](/windows/desktop/Intl/language-names) of the language to use for compiler output.</span></span> <span data-ttu-id="98f05-185">可以使用 PreferredUILang 编译器选项指定 C# 编译器用于错误消息和其他命令行输出的语言。</span><span class="sxs-lookup"><span data-stu-id="98f05-185">You can use the **PreferredUILang** compiler option to specify the language that you want the C# compiler to use for error messages and other command-line output.</span></span> <span data-ttu-id="98f05-186">如果未安装针对该语言的语言包，将改用操作系统的语言设置。</span><span class="sxs-lookup"><span data-stu-id="98f05-186">If the language pack for the language isn't installed, the language setting of the operating system is used instead.</span></span>

## <a name="baseaddress"></a><span data-ttu-id="98f05-187">BaseAddress</span><span class="sxs-lookup"><span data-stu-id="98f05-187">BaseAddress</span></span>

<span data-ttu-id="98f05-188">通过 BaseAddress 选项，可指定加载 DLL 的首选基址。</span><span class="sxs-lookup"><span data-stu-id="98f05-188">The **BaseAddress** option lets you specify the preferred base address at which to load a DLL.</span></span> <span data-ttu-id="98f05-189">若要深入了解何时且为何要使用此选项，请参阅 [Larry Osterman 的网络日志](/archive/blogs/larryosterman/why-should-i-even-bother-to-use-dlls-in-my-system)。</span><span class="sxs-lookup"><span data-stu-id="98f05-189">For more information about when and why to use this option, see [Larry Osterman's WebLog](/archive/blogs/larryosterman/why-should-i-even-bother-to-use-dlls-in-my-system).</span></span>

```xml
<BaseAddress>address</BaseAddress>
```

<span data-ttu-id="98f05-190">其中 `address` 是 DLL 的基址。</span><span class="sxs-lookup"><span data-stu-id="98f05-190">Where `address` is the base address for the DLL.</span></span> <span data-ttu-id="98f05-191">可将此地址指定为十进制数、十六进制数或八进制数。</span><span class="sxs-lookup"><span data-stu-id="98f05-191">This address can be specified as a decimal, hexadecimal, or octal number.</span></span> <span data-ttu-id="98f05-192">DLL 的默认基址由 .NET 公共语言运行时设置。</span><span class="sxs-lookup"><span data-stu-id="98f05-192">The default base address for a DLL is set by the .NET common language runtime.</span></span> <span data-ttu-id="98f05-193">此地址中的低序字将被舍入取整。</span><span class="sxs-lookup"><span data-stu-id="98f05-193">The lower-order word in this address will be rounded.</span></span> <span data-ttu-id="98f05-194">例如，如果指定 `0x11110001`，它将被舍入为 `0x11110000`。</span><span class="sxs-lookup"><span data-stu-id="98f05-194">For example, if you specify `0x11110001`, it will be rounded to `0x11110000`.</span></span> <span data-ttu-id="98f05-195">要完成 DLL 的签名过程，请使用具有 -R 选项的 SN.EXE。</span><span class="sxs-lookup"><span data-stu-id="98f05-195">To complete the signing process for a DLL, use SN.EXE with the -R option.</span></span>

## <a name="checksumalgorithm"></a><span data-ttu-id="98f05-196">ChecksumAlgorithm</span><span class="sxs-lookup"><span data-stu-id="98f05-196">ChecksumAlgorithm</span></span>

<span data-ttu-id="98f05-197">此选项控制用于对 PDB 中的源文件进行编码的校验和算法。</span><span class="sxs-lookup"><span data-stu-id="98f05-197">This option controls the checksum algorithm we use to encode source files in the PDB.</span></span>

```xml
<ChecksumAlgorithm>algorithm</ChecksumAlgorithm>
```

<span data-ttu-id="98f05-198">`algorithm` 必须是 `SHA1`（默认）或 `SHA256`。</span><span class="sxs-lookup"><span data-stu-id="98f05-198">The `algorithm` must be either `SHA1` (default) or `SHA256`.</span></span>

## <a name="codepage"></a><span data-ttu-id="98f05-199">CodePage</span><span class="sxs-lookup"><span data-stu-id="98f05-199">CodePage</span></span>

<span data-ttu-id="98f05-200">如果所需页不是系统的当前默认代码页，则此选项指定编译期间要使用的代码页。</span><span class="sxs-lookup"><span data-stu-id="98f05-200">This option specifies which codepage to use during compilation if the required page isn't the current default codepage for the system.</span></span>
  
```xml
<CodePage>id</CodePage>
```

<span data-ttu-id="98f05-201">其中 `id` 是要用于编译中所有源代码文件的代码页 ID。</span><span class="sxs-lookup"><span data-stu-id="98f05-201">Where `id` is the id of the code page to use for all source code files in the compilation.</span></span> <span data-ttu-id="98f05-202">编译器将首先尝试将所有源文件解释为 UTF-8。</span><span class="sxs-lookup"><span data-stu-id="98f05-202">The compiler will first attempt to interpret all source files as UTF-8.</span></span> <span data-ttu-id="98f05-203">如果源代码文件使用除 UTF-8 以外的编码并使用除 7 位 ASCII 字符以外的字符，请使用 CodePage 选项指定应使用的代码页。</span><span class="sxs-lookup"><span data-stu-id="98f05-203">If your source code files are in an encoding other than UTF-8 and use characters other than 7-bit ASCII characters, use the **CodePage** option to specify which code page should be used.</span></span> <span data-ttu-id="98f05-204">CodePage 适用于编译中的所有源代码文件。</span><span class="sxs-lookup"><span data-stu-id="98f05-204">**CodePage** applies to all source code files in your compilation.</span></span> <span data-ttu-id="98f05-205">有关如何查找系统上支持哪些代码页的信息，请参阅 [GetCPInfo](/windows/desktop/api/winnls/nf-winnls-getcpinfo)。</span><span class="sxs-lookup"><span data-stu-id="98f05-205">See [GetCPInfo](/windows/desktop/api/winnls/nf-winnls-getcpinfo) for information on how to find which code pages are supported on your system.</span></span>

## <a name="utf8output"></a><span data-ttu-id="98f05-206">Utf8Output</span><span class="sxs-lookup"><span data-stu-id="98f05-206">Utf8Output</span></span>

<span data-ttu-id="98f05-207">Utf8Output 选项使用 UTF-8 编码显示编译器输出。</span><span class="sxs-lookup"><span data-stu-id="98f05-207">The **Utf8Output** option displays compiler output using UTF-8 encoding.</span></span>

```xml
<Utf8Output>true</Utf8Output>
```

<span data-ttu-id="98f05-208">在某些国际配置中，编译器输出无法在控制台上正确显示。</span><span class="sxs-lookup"><span data-stu-id="98f05-208">In some international configurations, compiler output cannot correctly be displayed in the console.</span></span> <span data-ttu-id="98f05-209">使用 Utf8Output 并将编译器输出重定向到文件。</span><span class="sxs-lookup"><span data-stu-id="98f05-209">Use **Utf8Output** and redirect compiler output to a file.</span></span>

## <a name="filealignment"></a><span data-ttu-id="98f05-210">FileAlignment</span><span class="sxs-lookup"><span data-stu-id="98f05-210">FileAlignment</span></span>

<span data-ttu-id="98f05-211">FileAlignment 选项用于指定输出文件中各节的大小。</span><span class="sxs-lookup"><span data-stu-id="98f05-211">The **FileAlignment** option lets you specify the size of sections in your output file.</span></span> <span data-ttu-id="98f05-212">有效值为 512、1024、2048、4096 和 8192。</span><span class="sxs-lookup"><span data-stu-id="98f05-212">Valid values are 512, 1024, 2048, 4096, and 8192.</span></span> <span data-ttu-id="98f05-213">这些值以字节为单位。</span><span class="sxs-lookup"><span data-stu-id="98f05-213">These values are in bytes.</span></span>

```xml
<FileAlignment>number</FileAlignment>
```

<span data-ttu-id="98f05-214">在 Visual Studio 中，可以从项目的“生成”属性的“高级”页中设置 FileAlignment 选项。</span><span class="sxs-lookup"><span data-stu-id="98f05-214">You set the **FileAlignment** option from the **Advanced** page of the **Build** properties for your project in Visual Studio.</span></span> <span data-ttu-id="98f05-215">每一节都将在是 FileAlignment 值的倍数的边界上对齐。</span><span class="sxs-lookup"><span data-stu-id="98f05-215">Each section will be aligned on a boundary that is a multiple of the **FileAlignment** value.</span></span> <span data-ttu-id="98f05-216">没有固定的默认值。</span><span class="sxs-lookup"><span data-stu-id="98f05-216">There's no fixed default.</span></span> <span data-ttu-id="98f05-217">如果未指定 FileAlignment，公共语言运行时在编译时会选取一个默认值。</span><span class="sxs-lookup"><span data-stu-id="98f05-217">If **FileAlignment** isn't specified, the common language runtime picks a default at compile time.</span></span> <span data-ttu-id="98f05-218">通过指定节的大小，可以影响输出文件的大小。</span><span class="sxs-lookup"><span data-stu-id="98f05-218">By specifying the section size, you affect the size of the output file.</span></span> <span data-ttu-id="98f05-219">修改节的大小可能对将在较小设备上运行的程序有用。</span><span class="sxs-lookup"><span data-stu-id="98f05-219">Modifying section size may be useful for programs that will run on smaller devices.</span></span> <span data-ttu-id="98f05-220">使用 [DUMPBIN](/cpp/build/reference/dumpbin-options) 可查看有关输出文件中各节的信息。</span><span class="sxs-lookup"><span data-stu-id="98f05-220">Use [DUMPBIN](/cpp/build/reference/dumpbin-options) to see information about sections in your output file.</span></span>

## <a name="errorendlocation"></a><span data-ttu-id="98f05-221">ErrorEndLocation</span><span class="sxs-lookup"><span data-stu-id="98f05-221">ErrorEndLocation</span></span>

<span data-ttu-id="98f05-222">指示编译器输出每个错误结尾位置的行和列。</span><span class="sxs-lookup"><span data-stu-id="98f05-222">Instructs the compiler to output line and column of the end location of each error.</span></span>

```xml
<ErrorEndLocation>filename</ErrorEndLocation>
```

<span data-ttu-id="98f05-223">默认情况下，编译器会为所有错误和警告在源代码中写入起始位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-223">By default, the compiler writes the starting location in source for all errors and warnings.</span></span> <span data-ttu-id="98f05-224">当此选项设置为 true 时，编译器会为每个错误和警告写入起始和结尾位置。</span><span class="sxs-lookup"><span data-stu-id="98f05-224">When this option is set to true, the compiler writes both the starting and end location for each error and warning.</span></span>

## <a name="nostandardlib"></a><span data-ttu-id="98f05-225">NoStandardLib</span><span class="sxs-lookup"><span data-stu-id="98f05-225">NoStandardLib</span></span>

<span data-ttu-id="98f05-226">NoStandardLib 可防止导入 mscorlib.dll，后者定义了整个 System 命名空间。</span><span class="sxs-lookup"><span data-stu-id="98f05-226">**NoStandardLib** prevents the import of mscorlib.dll, which defines the entire System namespace.</span></span>

```xml
<NoStandardLib>true</NoStandardLib>
```

<span data-ttu-id="98f05-227">如果你想要定义或创建自己的 System 命名空间和对象，请使用此选项。</span><span class="sxs-lookup"><span data-stu-id="98f05-227">Use this option if you want to define or create your own System namespace and objects.</span></span> <span data-ttu-id="98f05-228">如果不指定 NoStandardLib，则 mscorlib.dll 将被导入到程序中（与指定 `<NoStandardLib>false</NoStandardLib>` 相同）。</span><span class="sxs-lookup"><span data-stu-id="98f05-228">If you don't specify **NoStandardLib**, mscorlib.dll is imported into your program (same as specifying `<NoStandardLib>false</NoStandardLib>`).</span></span>

## <a name="subsystemversion"></a><span data-ttu-id="98f05-229">SubsystemVersion</span><span class="sxs-lookup"><span data-stu-id="98f05-229">SubsystemVersion</span></span>

<span data-ttu-id="98f05-230">指定可执行文件可以运行的子系统的最低版本。</span><span class="sxs-lookup"><span data-stu-id="98f05-230">Specifies the minimum version of the subsystem on which the executable file runs.</span></span> <span data-ttu-id="98f05-231">大多数情况下，此选项确保该可执行文件可以使用早期 Windows 版本中未提供的安全功能。</span><span class="sxs-lookup"><span data-stu-id="98f05-231">Most commonly, this option ensures that the executable file can use security features that aren’t available with older versions of Windows.</span></span>

> [!NOTE]
> <span data-ttu-id="98f05-232">若要指定子系统本身，请使用 [TargetType](./output.md#targettype) 编译器选项。</span><span class="sxs-lookup"><span data-stu-id="98f05-232">To specify the subsystem itself, use the [**TargetType**](./output.md#targettype) compiler option.</span></span>

```xml
<SubsystemVersion>major.minor</SubsystemVersion>
```

<span data-ttu-id="98f05-233">`major.minor` 指定所需的子系统最低版本，以主版本和次要版本之间使用点标记的方式表示。</span><span class="sxs-lookup"><span data-stu-id="98f05-233">The `major.minor` specify the minimum required version of the subsystem, as expressed in a dot notation for major and minor versions.</span></span> <span data-ttu-id="98f05-234">例如，你可以指定应用程序不能在 Windows 7 之前的操作系统上运行。</span><span class="sxs-lookup"><span data-stu-id="98f05-234">For example, you can specify that an application can't run on an operating system that's older than Windows 7.</span></span> <span data-ttu-id="98f05-235">将此选项的值设置为 6.01，如本文后面的表中所述。</span><span class="sxs-lookup"><span data-stu-id="98f05-235">Set the value of this option to 6.01, as the table later in this article describes.</span></span> <span data-ttu-id="98f05-236">将 `major` 和 `minor` 的值指定为整数。</span><span class="sxs-lookup"><span data-stu-id="98f05-236">You specify the values for `major` and `minor` as integers.</span></span> <span data-ttu-id="98f05-237">`minor` 版本中的前导零不会更改版本，但尾随零会。</span><span class="sxs-lookup"><span data-stu-id="98f05-237">Leading zeroes in the `minor` version don't change the version, but trailing zeroes do.</span></span> <span data-ttu-id="98f05-238">例如，6.1 和 6.01 表示相同的版本，但 6.10 表示另一个版本。</span><span class="sxs-lookup"><span data-stu-id="98f05-238">For example, 6.1 and 6.01 refer to the same version, but 6.10 refers to a different version.</span></span> <span data-ttu-id="98f05-239">建议次要版本用两位数表示，以免混淆。</span><span class="sxs-lookup"><span data-stu-id="98f05-239">We recommend expressing the minor version as two digits to avoid confusion.</span></span>

<span data-ttu-id="98f05-240">下表列出了常见的 Windows 子系统版本。</span><span class="sxs-lookup"><span data-stu-id="98f05-240">The following table lists common subsystem versions of Windows.</span></span>

|<span data-ttu-id="98f05-241">Windows 版本</span><span class="sxs-lookup"><span data-stu-id="98f05-241">Windows version</span></span>|<span data-ttu-id="98f05-242">子系统版本</span><span class="sxs-lookup"><span data-stu-id="98f05-242">Subsystem version</span></span>|
|---------------------|-----------------------|
|<span data-ttu-id="98f05-243">Windows Server 2003</span><span class="sxs-lookup"><span data-stu-id="98f05-243">Windows Server 2003</span></span>|<span data-ttu-id="98f05-244">5.02</span><span class="sxs-lookup"><span data-stu-id="98f05-244">5.02</span></span>|
|<span data-ttu-id="98f05-245">Windows Vista</span><span class="sxs-lookup"><span data-stu-id="98f05-245">Windows Vista</span></span>|<span data-ttu-id="98f05-246">6.00</span><span class="sxs-lookup"><span data-stu-id="98f05-246">6.00</span></span>|
|<span data-ttu-id="98f05-247">Windows 7</span><span class="sxs-lookup"><span data-stu-id="98f05-247">Windows 7</span></span>|<span data-ttu-id="98f05-248">6.01</span><span class="sxs-lookup"><span data-stu-id="98f05-248">6.01</span></span>|
|<span data-ttu-id="98f05-249">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="98f05-249">Windows Server 2008</span></span>|<span data-ttu-id="98f05-250">6.01</span><span class="sxs-lookup"><span data-stu-id="98f05-250">6.01</span></span>|
|<span data-ttu-id="98f05-251">Windows 8</span><span class="sxs-lookup"><span data-stu-id="98f05-251">Windows 8</span></span>|<span data-ttu-id="98f05-252">6.02</span><span class="sxs-lookup"><span data-stu-id="98f05-252">6.02</span></span>|

<span data-ttu-id="98f05-253">SubsystemVersion 编译器选项的默认值取决于以下列表中的条件：</span><span class="sxs-lookup"><span data-stu-id="98f05-253">The default value of the **SubsystemVersion** compiler option depends on the conditions in the following list:</span></span>

- <span data-ttu-id="98f05-254">只要设置了以下列表中的任意编译器选项，则默认值为 6.02：</span><span class="sxs-lookup"><span data-stu-id="98f05-254">The default value is 6.02 if any compiler option in the following list is set:</span></span>
  - [<span data-ttu-id="98f05-255">/target:appcontainerexe</span><span class="sxs-lookup"><span data-stu-id="98f05-255">-target:appcontainerexe</span></span>](output.md)
  - [<span data-ttu-id="98f05-256">/target:winmdobj</span><span class="sxs-lookup"><span data-stu-id="98f05-256">-target:winmdobj</span></span>](output.md)
  - [<span data-ttu-id="98f05-257">-platform:arm</span><span class="sxs-lookup"><span data-stu-id="98f05-257">-platform:arm</span></span>](output.md)
- <span data-ttu-id="98f05-258">如果使用 MSBuild，面向 .NET Framework 4.5，并且未设置先前在此列表中指定的任何编译器选项，则默认值为 6.00。</span><span class="sxs-lookup"><span data-stu-id="98f05-258">The default value is 6.00 if you're using MSBuild, you're targeting .NET Framework 4.5, and you haven't set any of the compiler options that were specified earlier in this list.</span></span>
- <span data-ttu-id="98f05-259">如果前面的条件均不符合，则默认值为 4.00。</span><span class="sxs-lookup"><span data-stu-id="98f05-259">The default value is 4.00 if none of the previous conditions are true.</span></span>

## <a name="moduleassemblyname"></a><span data-ttu-id="98f05-260">ModuleAssemblyName</span><span class="sxs-lookup"><span data-stu-id="98f05-260">ModuleAssemblyName</span></span>

<span data-ttu-id="98f05-261">指定 .netmodule 可以访问其非公共类型的程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-261">Specifies the name of an assembly whose non-public types a *.netmodule* can access.</span></span>

```xml
<ModuleAssemblyName>assembly_name</ModuleAssemblyName>
```

<span data-ttu-id="98f05-262">生成 .netmodule 时，应使用 ModuleAssemblyName 并满足以下条件：</span><span class="sxs-lookup"><span data-stu-id="98f05-262">**ModuleAssemblyName** should be used when building a *.netmodule*, and where the following conditions are true:</span></span>

- <span data-ttu-id="98f05-263">.netmodule 需要具有访问现有程序集中非公共类型的权限。</span><span class="sxs-lookup"><span data-stu-id="98f05-263">The *.netmodule* needs access to non-public types in an existing assembly.</span></span>
- <span data-ttu-id="98f05-264">知道生成后的 .netmodule 所在程序集的名称。</span><span class="sxs-lookup"><span data-stu-id="98f05-264">You know the name of the assembly into which the .netmodule will be built.</span></span>
- <span data-ttu-id="98f05-265">现有程序集已经获得友元程序集访问权限，可访问将在其中生成 .netmodule 的程序集。</span><span class="sxs-lookup"><span data-stu-id="98f05-265">The existing assembly has granted friend assembly access to the assembly into which the .*netmodule* will be built.</span></span>

<span data-ttu-id="98f05-266">有关生成 .netmodule 的详细信息，请参阅模块的 [TargetType](output.md#targettype) 选项。</span><span class="sxs-lookup"><span data-stu-id="98f05-266">For more information on building a .netmodule, see [**TargetType**](output.md#targettype) option of **module**.</span></span> <span data-ttu-id="98f05-267">有关友元程序集的详细信息，请参阅[友元程序集](../../../standard/assembly/friend.md)。</span><span class="sxs-lookup"><span data-stu-id="98f05-267">For more information on friend assemblies, see [Friend Assemblies](../../../standard/assembly/friend.md).</span></span>
