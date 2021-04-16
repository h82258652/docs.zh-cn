---
title: 代码分析规则的配置文件
description: 了解用于配置代码分析规则的不同配置文件。
ms.date: 09/24/2020
ms.topic: conceptual
no-loc:
- EditorConfig
ms.openlocfilehash: b98fdd48f2373bd23fcd3273834860a60c682969
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "99216377"
---
# <a name="configuration-files-for-code-analysis-rules"></a><span data-ttu-id="174fa-103">代码分析规则的配置文件</span><span class="sxs-lookup"><span data-stu-id="174fa-103">Configuration files for code analysis rules</span></span>

<span data-ttu-id="174fa-104">代码分析规则具有多种[配置选项](configuration-options.md)。</span><span class="sxs-lookup"><span data-stu-id="174fa-104">Code analysis rules have various [configuration options](configuration-options.md).</span></span> <span data-ttu-id="174fa-105">可以在下列任一分析器配置文件中将这些选项指定为键值对：</span><span class="sxs-lookup"><span data-stu-id="174fa-105">You specify these options as key-value pairs in one of the following analyzer configuration files:</span></span>

- <span data-ttu-id="174fa-106">[EditorConfig](#editorconfig) 文件：基于文件或基于文件夹的配置选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-106">[EditorConfig](#editorconfig) file: File-based or folder-based configuration options.</span></span>
- <span data-ttu-id="174fa-107">[全局 AnalyzerConfig](#global-analyzerconfig) 文件：项目级别配置选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-107">[Global AnalyzerConfig](#global-analyzerconfig) file: Project-level configuration options.</span></span> <span data-ttu-id="174fa-108">当某些项目文件位于项目文件夹外时，它非常有用。</span><span class="sxs-lookup"><span data-stu-id="174fa-108">Useful when some project files reside outside the project folder.</span></span>

## EditorConfig

<span data-ttu-id="174fa-109">[EditorConfig](/visualstudio/ide/create-portable-custom-editor-options) 文件用于提供适用于特定资源文件或文件夹的选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-109">[EditorConfig](/visualstudio/ide/create-portable-custom-editor-options) files are used to provide **options that apply to specific source files or folders**.</span></span> <span data-ttu-id="174fa-110">选项位于节标头下，用于标识适用的文件和文件夹。</span><span class="sxs-lookup"><span data-stu-id="174fa-110">Options are placed under section headers to identify the applicable files and folders.</span></span> <span data-ttu-id="174fa-111">为要配置的每个规则添加一个条目，并将其放置在相应的文件扩展名节下，例如 `[*.cs]`。</span><span class="sxs-lookup"><span data-stu-id="174fa-111">Add an entry for each rule you want to configure, and place it under the corresponding file extension section, for example, `[*.cs]`.</span></span>

```ini
[*.cs]
<option_name> = <option_value>
```

<span data-ttu-id="174fa-112">在上面的示例中，`[*.cs]` 是一个 editorconfig 节标头，用于选择当前文件夹（包括子文件夹）中带有 `.cs` 文件扩展名的所有 C# 文件。</span><span class="sxs-lookup"><span data-stu-id="174fa-112">In the above example, `[*.cs]` is an editorconfig section header to select all C# files with `.cs` file extension within the current folder, including subfolders.</span></span> <span data-ttu-id="174fa-113">接下来 `<option_name> = <option_value>` 这一条目是一个分析器选项，将应用于所有 C# 文件。</span><span class="sxs-lookup"><span data-stu-id="174fa-113">The subsequent entry, `<option_name> = <option_value>`, is an analyzer option that will be applied to all the C# files.</span></span>

<span data-ttu-id="174fa-114">可将文件放在相应的目录中，将 EditorConfig 文件约定应用于文件夹、项目或整个存储库。</span><span class="sxs-lookup"><span data-stu-id="174fa-114">You can apply EditorConfig file conventions to a folder, a project, or an entire repo by placing the file in the corresponding directory.</span></span> <span data-ttu-id="174fa-115">可在生成时执行分析时以及在 Visual Studio 中编辑代码时应用这些选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-115">These options are applied when executing the analysis at build time and while you edit code in Visual Studio.</span></span>

<span data-ttu-id="174fa-116">如果有一个现有的 .editorconfig 文件可用于编辑器设置（如缩进大小或是否剪裁尾随空格），可将代码分析配置选项放在同一文件中。</span><span class="sxs-lookup"><span data-stu-id="174fa-116">If you have an existing *.editorconfig* file for editor settings such as indent size or whether to trim trailing whitespace, you can place your code analysis configuration options in the same file.</span></span>

> [!TIP]
> <span data-ttu-id="174fa-117">Visual Studio 提供 .editorconfig 项模板，通过该模板可轻松地将其中一个文件添加到项目中。</span><span class="sxs-lookup"><span data-stu-id="174fa-117">Visual Studio provides an *.editorconfig* item template that makes is easy to add one of these files to your project.</span></span> <span data-ttu-id="174fa-118">有关详细信息，请参阅[将 EditorConfig 文件添加到项目](/visualstudio/ide/create-portable-custom-editor-options#add-an-editorconfig-file-to-a-project)。</span><span class="sxs-lookup"><span data-stu-id="174fa-118">For more information, see [Add an EditorConfig file to a project](/visualstudio/ide/create-portable-custom-editor-options#add-an-editorconfig-file-to-a-project).</span></span>

### <a name="example"></a><span data-ttu-id="174fa-119">示例</span><span class="sxs-lookup"><span data-stu-id="174fa-119">Example</span></span>

<span data-ttu-id="174fa-120">下面是一个示例 EditorConfig 文件，用于配置选项和规则严重性：</span><span class="sxs-lookup"><span data-stu-id="174fa-120">Following is an example EditorConfig file to configure options and rule severity:</span></span>

```ini
# Remove the line below if you want to inherit .editorconfig settings from higher directories
root = true

# C# files
[*.cs]

#### Core EditorConfig Options ####

# Indentation and spacing
indent_size = 4
indent_style = space
tab_width = 4

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="global-analyzerconfig"></a><span data-ttu-id="174fa-121">全局 AnalyzerConfig</span><span class="sxs-lookup"><span data-stu-id="174fa-121">Global AnalyzerConfig</span></span>

<span data-ttu-id="174fa-122">从 .NET 5 SDK（在 Visual Studio 2019 版本 16.8 和更高版本中受支持）开始，还可配置包含全局 AnalyzerConfig 文件的分析器选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-122">Starting with the .NET 5 SDK (which is supported in Visual Studio 2019 version 16.8 and later), you can also configure analyzer options with global _AnalyzerConfig_ files.</span></span> <span data-ttu-id="174fa-123">这些文件用于提供适用于项目中所有源文件的选项，不考虑其文件名和文件路径。</span><span class="sxs-lookup"><span data-stu-id="174fa-123">These files are used to provide **options that apply to all the source files in a project**, regardless of their file names or file paths.</span></span>

<span data-ttu-id="174fa-124">与 [EditorConfig](#editorconfig) 文件不同，全局配置文件不能用于为 IDE 配置编辑器样式设置，如缩进大小或是否剪裁尾随空格。</span><span class="sxs-lookup"><span data-stu-id="174fa-124">Unlike [EditorConfig](#editorconfig) files, global config files can't be used to configure editor style settings for IDEs, such as indent size or whether to trim trailing whitespace.</span></span> <span data-ttu-id="174fa-125">而是专用于指定项目级别分析器配置选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-125">Instead, they are designed purely for specifying project-level analyzer configuration options.</span></span>

### <a name="format"></a><span data-ttu-id="174fa-126">格式</span><span class="sxs-lookup"><span data-stu-id="174fa-126">Format</span></span>

<span data-ttu-id="174fa-127">EditorConfig 文件必须包含节标头（如 `[*.cs]`），以标识适用的文件和文件夹，但全局 AnalyzerConfig 文件没有节标头。</span><span class="sxs-lookup"><span data-stu-id="174fa-127">Unlike EditorConfig files, which must have section headers, such as `[*.cs]`, to identify the applicable files and folders, global AnalyzerConfig files don't have section headers.</span></span> <span data-ttu-id="174fa-128">相反，它们需要 `is_global = true` 格式的顶级条目，以便与常规 EditorConfig 文件区分开来。</span><span class="sxs-lookup"><span data-stu-id="174fa-128">Instead, they require a top level entry of the form `is_global = true` to differentiate them from regular EditorConfig files.</span></span> <span data-ttu-id="174fa-129">这表示文件中的所有选项都适用于整个项目。</span><span class="sxs-lookup"><span data-stu-id="174fa-129">This indicates that all the options in the file apply to the entire project.</span></span> <span data-ttu-id="174fa-130">例如：</span><span class="sxs-lookup"><span data-stu-id="174fa-130">For example:</span></span>

```ini
is_global = true
<option_name> = <option_value>
```

### <a name="naming"></a><span data-ttu-id="174fa-131">命名</span><span class="sxs-lookup"><span data-stu-id="174fa-131">Naming</span></span>

<span data-ttu-id="174fa-132">EditorConfig 文件必须命名为 `.editorconfig`，而全局配置文件不需要有特定的名称或文件扩展名。</span><span class="sxs-lookup"><span data-stu-id="174fa-132">Unlike EditorConfig files, which must be named `.editorconfig`, global config files do not need to have a specific name or extension.</span></span> <span data-ttu-id="174fa-133">但是，如果将这些文件命名为 `.globalconfig`，它们将隐式应用于当前文件夹（包括子文件夹）中的所有 C# 和 Visual Basic 项目。</span><span class="sxs-lookup"><span data-stu-id="174fa-133">However, if you name these files as `.globalconfig` then they will be implicitly applied to all the C# and Visual Basic projects within the current folder, including subfolders.</span></span> <span data-ttu-id="174fa-134">否则，必须将 `GlobalAnalyzerConfigFiles` 项显式添加到 MSBuild 项目文件中：</span><span class="sxs-lookup"><span data-stu-id="174fa-134">Otherwise, you must explicitly add the `GlobalAnalyzerConfigFiles` item to your MSBuild project file:</span></span>

```xml
<ItemGroup>
  <GlobalAnalyzerConfigFiles Include="<path_to_global_analyzer_config>" />
</ItemGroup>
```

> [!NOTE]
> <span data-ttu-id="174fa-135">即使文件命名为 `.globalconfig`，也需要顶级条目 `is_global = true`。</span><span class="sxs-lookup"><span data-stu-id="174fa-135">The top-level entry `is_global = true` is required even when the file is named `.globalconfig`.</span></span>

### <a name="example"></a><span data-ttu-id="174fa-136">示例</span><span class="sxs-lookup"><span data-stu-id="174fa-136">Example</span></span>

<span data-ttu-id="174fa-137">下面是一个示例全局 AnalyzerConfig 文件，用于在项目级别配置选项和规则严重性：</span><span class="sxs-lookup"><span data-stu-id="174fa-137">Following is an example global AnalyzerConfig file to configure options and rule severity at the project level:</span></span>

```ini
# Top level entry required to mark this as a global AnalyzerConfig file
is_global = true

# NOTE: No section headers for configuration entries

#### .NET Coding Conventions ####

# this. and Me. preferences
dotnet_style_qualification_for_method = true:warning

#### Diagnostic configuration ####

# CA1000: Do not declare static members on generic types
dotnet_diagnostic.CA1000.severity = warning
```

## <a name="precedence"></a><span data-ttu-id="174fa-138">优先级</span><span class="sxs-lookup"><span data-stu-id="174fa-138">Precedence</span></span>

<span data-ttu-id="174fa-139">EditorConfig 文件和全局 AnalyzerConfig 文件都为每个选项指定键值对。</span><span class="sxs-lookup"><span data-stu-id="174fa-139">Both EditorConfig files and global AnalyzerConfig files specify a key-value pair for each option.</span></span> <span data-ttu-id="174fa-140">如果有多个条目具有相同键但值不同，则会发生冲突。</span><span class="sxs-lookup"><span data-stu-id="174fa-140">Conflicts arise when there are multiple entries with the same key but different values.</span></span>

### <a name="general-options"></a><span data-ttu-id="174fa-141">常规选项</span><span class="sxs-lookup"><span data-stu-id="174fa-141">General options</span></span>

<span data-ttu-id="174fa-142">选项之间发生冲突时，可使用以下优先级规则来解决冲突：</span><span class="sxs-lookup"><span data-stu-id="174fa-142">When conflicts arise between options, the following precedence rules are used to resolve the conflicts:</span></span>

- <span data-ttu-id="174fa-143">相同配置文件中出现冲突条目：先选在文件中靠后的条目。</span><span class="sxs-lookup"><span data-stu-id="174fa-143">Conflicting entries in the same configuration file: The entry that appears later in the file wins.</span></span> <span data-ttu-id="174fa-144">这适用于在单个 EditorConfig 文件中和单个全局 AnalyzerConfig 文件中的冲突条目。</span><span class="sxs-lookup"><span data-stu-id="174fa-144">This is true for conflicting entries within a single EditorConfig file and also within a single global AnalyzerConfig file.</span></span>

- <span data-ttu-id="174fa-145">两个 EditorConfig 文件中出现冲突条目：先选 EditorConfig 文件位于文件系统更深层的条目（因此文件路径较长）。</span><span class="sxs-lookup"><span data-stu-id="174fa-145">Conflicting entries in two EditorConfig files: The entry in the EditorConfig file that's deeper in the file system, and hence has a longer file path, wins.</span></span>

- <span data-ttu-id="174fa-146">两个全局 AnalyzerConfig 文件中出现冲突条目：报告编译器警告，忽略这两个条目。</span><span class="sxs-lookup"><span data-stu-id="174fa-146">Conflicting entries in two global AnalyzerConfig files: A compiler warning is reported and both entries are ignored.</span></span>

- <span data-ttu-id="174fa-147">EditorConfig 文件和全局 AnalyzerConfig 文件中出现冲突条目：先选 EditorConfig 文件中的条目。</span><span class="sxs-lookup"><span data-stu-id="174fa-147">Conflicting entries in an EditorConfig file and a Global AnalyzerConfig file: The entry in the EditorConfig file wins.</span></span>

### <a name="severity-options"></a><span data-ttu-id="174fa-148">严重性选项</span><span class="sxs-lookup"><span data-stu-id="174fa-148">Severity options</span></span>

<span data-ttu-id="174fa-149">上述[常规优先规则](#general-options)适用于配置文件中指定的所有选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-149">The previous [general precedence rules](#general-options) apply for all options specified in configuration files.</span></span> <span data-ttu-id="174fa-150">[严重性配置](configuration-options.md#severity-level)选项适用于下列其他优先级规则：</span><span class="sxs-lookup"><span data-stu-id="174fa-150">For [severity configuration](configuration-options.md#severity-level) options, the following additional precedence rules apply:</span></span>

- <span data-ttu-id="174fa-151">在命令行上作为编译器选项（`/nowarn` 或 `/warnaserror`）指定的严重性选项始终会重写 EditorConfig 和全局 AnalyzerConfig 文件中指定的[严重性配置](configuration-options.md#severity-level)选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-151">Severity options specified at the command line as compiler options (`/nowarn` or `/warnaserror`) always override [severity configuration](configuration-options.md#severity-level) options specified in EditorConfig and global AnalyzerConfig files.</span></span>

- <span data-ttu-id="174fa-152">还可使用[规则集](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules)文件指定严重性选项。</span><span class="sxs-lookup"><span data-stu-id="174fa-152">Severity options can also be specified with a [Ruleset](/visualstudio/code-quality/using-rule-sets-to-group-code-analysis-rules) file.</span></span> <span data-ttu-id="174fa-153">但是，规则集文件已弃用，改用 EditorConfig 和全局 AnalyzerConfig 文件。</span><span class="sxs-lookup"><span data-stu-id="174fa-153">However, rulesets files are deprecated in favor of EditorConfig and global AnalyzerConfig files.</span></span> <span data-ttu-id="174fa-154">建议[将规则集文件转换为等效的 EditorConfig 文件](/visualstudio/code-quality/use-roslyn-analyzers#convert-an-existing-ruleset-file-to-editorconfig-file)。</span><span class="sxs-lookup"><span data-stu-id="174fa-154">It's recommended that you [convert ruleset files to an equivalent EditorConfig file](/visualstudio/code-quality/use-roslyn-analyzers#convert-an-existing-ruleset-file-to-editorconfig-file).</span></span> <span data-ttu-id="174fa-155">规则集文件和 EditorConfig 或全局 AnalyzerConfig 文件中的严重性冲突条目的优先级未定义。</span><span class="sxs-lookup"><span data-stu-id="174fa-155">Precedence for conflicting severity entries from a ruleset file and EditorConfig or global AnalyzerConfig files is _undefined_.</span></span>

- <span data-ttu-id="174fa-156">有关具有不同键的相关严重性选项的优先级规则的信息（例如，为单个规则和为规则所属的类别指定不同的严重性），请参阅[代码分析的配置选项](configuration-options.md#precedence)。</span><span class="sxs-lookup"><span data-stu-id="174fa-156">For information about precedence rules for related severity options with different keys, for example, when different severities are specified for a single rule and for the category that rule falls under, see [Configuration options for code analysis](configuration-options.md#precedence).</span></span>

## <a name="see-also"></a><span data-ttu-id="174fa-157">另请参阅</span><span class="sxs-lookup"><span data-stu-id="174fa-157">See also</span></span>

- [<span data-ttu-id="174fa-158">EditorConfig 与全局 AnalyzerConfig 设计问题</span><span class="sxs-lookup"><span data-stu-id="174fa-158">EditorConfig vs global AnalyzerConfig design issue</span></span>](https://github.com/dotnet/roslyn/issues/47707)
