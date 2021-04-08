---
title: dotnet pack 命令
description: dotnet pack 命令可为 .NET 项目创建 NuGet 包。
ms.date: 04/28/2020
ms.openlocfilehash: 52790b61e8b2d59fa6a8fc68bad6a1e0dc13a97b
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122634"
---
# <a name="dotnet-pack"></a><span data-ttu-id="530e2-103">dotnet pack</span><span class="sxs-lookup"><span data-stu-id="530e2-103">dotnet pack</span></span>

<span data-ttu-id="530e2-104">**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="530e2-104">**This article applies to:** ✔️ .NET Core 2.x SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="530e2-105">“属性”</span><span class="sxs-lookup"><span data-stu-id="530e2-105">Name</span></span>

<span data-ttu-id="530e2-106">`dotnet pack` - 将代码打包到 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="530e2-106">`dotnet pack` - Packs the code into a NuGet package.</span></span>

## <a name="synopsis"></a><span data-ttu-id="530e2-107">摘要</span><span class="sxs-lookup"><span data-stu-id="530e2-107">Synopsis</span></span>

```dotnetcli
dotnet pack [<PROJECT>|<SOLUTION>] [-c|--configuration <CONFIGURATION>]
    [--force] [--include-source] [--include-symbols] [--interactive]
    [--no-build] [--no-dependencies] [--no-restore] [--nologo]
    [-o|--output <OUTPUT_DIRECTORY>] [--runtime <RUNTIME_IDENTIFIER>]
    [-s|--serviceable] [-v|--verbosity <LEVEL>]
    [--version-suffix <VERSION_SUFFIX>]

dotnet pack -h|--help
```

## <a name="description"></a><span data-ttu-id="530e2-108">描述</span><span class="sxs-lookup"><span data-stu-id="530e2-108">Description</span></span>

<span data-ttu-id="530e2-109">`dotnet pack` 命令生成项目并创建 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="530e2-109">The `dotnet pack` command builds the project and creates NuGet packages.</span></span> <span data-ttu-id="530e2-110">该命令的结果是一个 NuGet 包，也就是一个 .nupkg 文件。</span><span class="sxs-lookup"><span data-stu-id="530e2-110">The result of this command is a NuGet package (that is, a *.nupkg* file).</span></span>

<span data-ttu-id="530e2-111">如果要生成包含调试符号的包，可以使用以下两个选项：</span><span class="sxs-lookup"><span data-stu-id="530e2-111">If you want to generate a package that contains the debug symbols, you have two options available:</span></span>

- <span data-ttu-id="530e2-112">`--include-symbols`：该选项用于创建符号包。</span><span class="sxs-lookup"><span data-stu-id="530e2-112">`--include-symbols` - it creates the symbols package.</span></span>
- <span data-ttu-id="530e2-113">`--include-source`：该选项用于创建带有 `src` 文件夹的符号包，该文件夹包含源文件。</span><span class="sxs-lookup"><span data-stu-id="530e2-113">`--include-source` - it creates the symbols package with a `src` folder inside containing the source files.</span></span>

<span data-ttu-id="530e2-114">将被打包项目的 NuGet 依赖项添加到 *.nuspec* 文件，以便在安装包时可以进行正确解析。</span><span class="sxs-lookup"><span data-stu-id="530e2-114">NuGet dependencies of the packed project are added to the *.nuspec* file, so they're properly resolved when the package is installed.</span></span> <span data-ttu-id="530e2-115">如果打包的项目具有对其他项目的引用，则不会将其他项目包含在包中。</span><span class="sxs-lookup"><span data-stu-id="530e2-115">If the packed project has references to other projects, the other projects are not included in the package.</span></span> <span data-ttu-id="530e2-116">目前，如果具有项目到项目的依赖项，则每个项目均必须包含一个包。</span><span class="sxs-lookup"><span data-stu-id="530e2-116">Currently, you must have a package per project if you have project-to-project dependencies.</span></span>

<span data-ttu-id="530e2-117">默认情况下，`dotnet pack` 先构建项目。</span><span class="sxs-lookup"><span data-stu-id="530e2-117">By default, `dotnet pack` builds the project first.</span></span> <span data-ttu-id="530e2-118">如果希望避免此行为，则传递 `--no-build` 选项。</span><span class="sxs-lookup"><span data-stu-id="530e2-118">If you wish to avoid this behavior, pass the `--no-build` option.</span></span> <span data-ttu-id="530e2-119">此选项在持续集成 (CI) 生成方案中通常非常有用，你可以知道代码是之前生成的。</span><span class="sxs-lookup"><span data-stu-id="530e2-119">This option is often useful in Continuous Integration (CI) build scenarios where you know the code was previously built.</span></span>

> [!NOTE]
> <span data-ttu-id="530e2-120">在某些情况下，无法执行隐式生成。</span><span class="sxs-lookup"><span data-stu-id="530e2-120">In some cases, the implicit build cannot be performed.</span></span> <span data-ttu-id="530e2-121">设置 `GeneratePackageOnBuild` 以避免生成目标和包目标之间的循环依赖关系时可能会发生这种情况。</span><span class="sxs-lookup"><span data-stu-id="530e2-121">This can occur when `GeneratePackageOnBuild` is set, to avoid a cyclic dependency between build and pack targets.</span></span> <span data-ttu-id="530e2-122">如果存在锁定文件或其他问题，生成也可能失败。</span><span class="sxs-lookup"><span data-stu-id="530e2-122">The build can also fail if there is a locked file or other issue.</span></span>

<span data-ttu-id="530e2-123">可向 `dotnet pack` 命令提供 MSBuild 属性，用于打包进程。</span><span class="sxs-lookup"><span data-stu-id="530e2-123">You can provide MSBuild properties to the `dotnet pack` command for the packing process.</span></span> <span data-ttu-id="530e2-124">有关详细信息，请参阅 [NuGet 包目标属性](/nuget/reference/msbuild-targets#pack-target)和 [MSBuild 命令行引用](/visualstudio/msbuild/msbuild-command-line-reference)。</span><span class="sxs-lookup"><span data-stu-id="530e2-124">For more information, see [NuGet pack target properties](/nuget/reference/msbuild-targets#pack-target) and the [MSBuild Command-Line Reference](/visualstudio/msbuild/msbuild-command-line-reference).</span></span> <span data-ttu-id="530e2-125">[示例](#examples)部分介绍了如何在不同的情况下使用 MSBuild `-p` 开关。</span><span class="sxs-lookup"><span data-stu-id="530e2-125">The [Examples](#examples) section shows how to use the MSBuild `-p` switch for a couple of different scenarios.</span></span>

<span data-ttu-id="530e2-126">默认情况下，Web 项目不可打包。</span><span class="sxs-lookup"><span data-stu-id="530e2-126">Web projects aren't packable by default.</span></span> <span data-ttu-id="530e2-127">若要覆盖默认行为，请将以下属性添加到 .csproj 文件中：</span><span class="sxs-lookup"><span data-stu-id="530e2-127">To override the default behavior, add the following property to your *.csproj* file:</span></span>

```xml
<PropertyGroup>
   <IsPackable>true</IsPackable>
</PropertyGroup>
```

### <a name="implicit-restore"></a><span data-ttu-id="530e2-128">隐式还原</span><span class="sxs-lookup"><span data-stu-id="530e2-128">Implicit restore</span></span>

[!INCLUDE[dotnet restore note + options](~/includes/dotnet-restore-note-options.md)]

## <a name="arguments"></a><span data-ttu-id="530e2-129">自变量</span><span class="sxs-lookup"><span data-stu-id="530e2-129">Arguments</span></span>

`PROJECT | SOLUTION`

  <span data-ttu-id="530e2-130">要打包的项目或解决方案。</span><span class="sxs-lookup"><span data-stu-id="530e2-130">The project or solution to pack.</span></span> <span data-ttu-id="530e2-131">它可能是 csproj 文件、vbproj 文件、fsproj 文件、解决方案文件或目录的路径。</span><span class="sxs-lookup"><span data-stu-id="530e2-131">It's either a path to a csproj, vbproj, or fsproj file, or to a solution file or directory.</span></span> <span data-ttu-id="530e2-132">如果未指定，此命令会搜索当前目录，以获取项目文件或解决方案文件。</span><span class="sxs-lookup"><span data-stu-id="530e2-132">If not specified, the command searches the current directory for a project or solution file.</span></span>

## <a name="options"></a><span data-ttu-id="530e2-133">选项</span><span class="sxs-lookup"><span data-stu-id="530e2-133">Options</span></span>

- **`-c|--configuration <CONFIGURATION>`**

  <span data-ttu-id="530e2-134">定义生成配置。</span><span class="sxs-lookup"><span data-stu-id="530e2-134">Defines the build configuration.</span></span> <span data-ttu-id="530e2-135">大多数项目的默认配置为 `Debug`，但你可以覆盖项目中的生成配置设置。</span><span class="sxs-lookup"><span data-stu-id="530e2-135">The default for most projects is `Debug`, but you can override the build configuration settings in your project.</span></span>

- **`--force`**

  <span data-ttu-id="530e2-136">强制解析所有依赖项，即使上次还原已成功，也不例外。</span><span class="sxs-lookup"><span data-stu-id="530e2-136">Forces all dependencies to be resolved even if the last restore was successful.</span></span> <span data-ttu-id="530e2-137">指定此标记等同于删除 project.assets.json 文件。</span><span class="sxs-lookup"><span data-stu-id="530e2-137">Specifying this flag is the same as deleting the *project.assets.json* file.</span></span>

- **`-h|--help`**

  <span data-ttu-id="530e2-138">打印出有关命令的简短帮助。</span><span class="sxs-lookup"><span data-stu-id="530e2-138">Prints out a short help for the command.</span></span>

- **`--include-source`**

  <span data-ttu-id="530e2-139">除输出目录中的常规 NuGet 包外，还包括调试符号 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="530e2-139">Includes the debug symbols NuGet packages in addition to the regular NuGet packages in the output directory.</span></span> <span data-ttu-id="530e2-140">源文件包括在符号包内的 `src` 文件夹中。</span><span class="sxs-lookup"><span data-stu-id="530e2-140">The sources files are included in the `src` folder within the symbols package.</span></span>

- **`--include-symbols`**

  <span data-ttu-id="530e2-141">除输出目录中的常规 NuGet 包外，还包括调试符号 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="530e2-141">Includes the debug symbols NuGet packages in addition to the regular NuGet packages in the output directory.</span></span>

- **`--interactive`**

  <span data-ttu-id="530e2-142">允许命令停止并等待用户输入或操作（例如，完成身份验证）。</span><span class="sxs-lookup"><span data-stu-id="530e2-142">Allows the command to stop and wait for user input or action (for example, to complete authentication).</span></span> <span data-ttu-id="530e2-143">自 .NET Core 3.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="530e2-143">Available since .NET Core 3.0 SDK.</span></span>

- **`--no-build`**

  <span data-ttu-id="530e2-144">打包前不生成项目。</span><span class="sxs-lookup"><span data-stu-id="530e2-144">Doesn't build the project before packing.</span></span> <span data-ttu-id="530e2-145">还将隐式设置 `--no-restore` 标记。</span><span class="sxs-lookup"><span data-stu-id="530e2-145">It also implicitly sets the `--no-restore` flag.</span></span>

- **`--no-dependencies`**

  <span data-ttu-id="530e2-146">忽略项目间引用，仅还原根项目。</span><span class="sxs-lookup"><span data-stu-id="530e2-146">Ignores project-to-project references and only restores the root project.</span></span>

- **`--no-restore`**

  <span data-ttu-id="530e2-147">运行此命令时不执行隐式还原。</span><span class="sxs-lookup"><span data-stu-id="530e2-147">Doesn't execute an implicit restore when running the command.</span></span>

- **`--nologo`**

  <span data-ttu-id="530e2-148">不显示启动版权标志或版权消息。</span><span class="sxs-lookup"><span data-stu-id="530e2-148">Doesn't display the startup banner or the copyright message.</span></span> <span data-ttu-id="530e2-149">自 .NET Core 3.0 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="530e2-149">Available since .NET Core 3.0 SDK.</span></span>

- **`-o|--output <OUTPUT_DIRECTORY>`**

  <span data-ttu-id="530e2-150">将生成的包放置在指定目录。</span><span class="sxs-lookup"><span data-stu-id="530e2-150">Places the built packages in the directory specified.</span></span>

- **`--runtime <RUNTIME_IDENTIFIER>`**

  <span data-ttu-id="530e2-151">指定要为其还原包的目标运行时。</span><span class="sxs-lookup"><span data-stu-id="530e2-151">Specifies the target runtime to restore packages for.</span></span> <span data-ttu-id="530e2-152">有关运行时标识符 (RID) 的列表，请参阅 [RID 目录](../rid-catalog.md)。</span><span class="sxs-lookup"><span data-stu-id="530e2-152">For a list of Runtime Identifiers (RIDs), see the [RID catalog](../rid-catalog.md).</span></span>

- **`-s|--serviceable`**

  <span data-ttu-id="530e2-153">设置包中可用的标志。</span><span class="sxs-lookup"><span data-stu-id="530e2-153">Sets the serviceable flag in the package.</span></span> <span data-ttu-id="530e2-154">有关详细信息，请参阅 [.NET 博客：.NET Framework 4.5.1 支持 .NET NuGet 库的 Microsoft 安全更新](https://aka.ms/nupkgservicing)。</span><span class="sxs-lookup"><span data-stu-id="530e2-154">For more information, see [.NET Blog: .NET Framework 4.5.1 Supports Microsoft Security Updates for .NET NuGet Libraries](https://aka.ms/nupkgservicing).</span></span>

- **`--version-suffix <VERSION_SUFFIX>`**

  <span data-ttu-id="530e2-155">定义项目中 `$(VersionSuffix)` MSBuild 属性的值。</span><span class="sxs-lookup"><span data-stu-id="530e2-155">Defines the value for the `$(VersionSuffix)` MSBuild property in the project.</span></span>

- **`-v|--verbosity <LEVEL>`**

  <span data-ttu-id="530e2-156">设置命令的详细级别。</span><span class="sxs-lookup"><span data-stu-id="530e2-156">Sets the verbosity level of the command.</span></span> <span data-ttu-id="530e2-157">允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。</span><span class="sxs-lookup"><span data-stu-id="530e2-157">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span>

## <a name="examples"></a><span data-ttu-id="530e2-158">示例</span><span class="sxs-lookup"><span data-stu-id="530e2-158">Examples</span></span>

- <span data-ttu-id="530e2-159">打包当前目录中的项目：</span><span class="sxs-lookup"><span data-stu-id="530e2-159">Pack the project in the current directory:</span></span>

  ```dotnetcli
  dotnet pack
  ```

- <span data-ttu-id="530e2-160">打包 `app1` 项目：</span><span class="sxs-lookup"><span data-stu-id="530e2-160">Pack the `app1` project:</span></span>

  ```dotnetcli
  dotnet pack ~/projects/app1/project.csproj
  ```

- <span data-ttu-id="530e2-161">打包当前目录中的项目并将生成的包放置到 `nupkgs` 文件夹：</span><span class="sxs-lookup"><span data-stu-id="530e2-161">Pack the project in the current directory and place the resulting packages into the `nupkgs` folder:</span></span>

  ```dotnetcli
  dotnet pack --output nupkgs
  ```

- <span data-ttu-id="530e2-162">将当前目录中的项目打包到 `nupkgs` 文件夹并跳过生成步骤：</span><span class="sxs-lookup"><span data-stu-id="530e2-162">Pack the project in the current directory into the `nupkgs` folder and skip the build step:</span></span>

  ```dotnetcli
  dotnet pack --no-build --output nupkgs
  ```

- <span data-ttu-id="530e2-163">将项目的版本后缀配置为 *.csproj* 文件中的 `<VersionSuffix>$(VersionSuffix)</VersionSuffix>`，使用给定的后缀打包当前项目，并更新生成的程序包版本：</span><span class="sxs-lookup"><span data-stu-id="530e2-163">With the project's version suffix configured as `<VersionSuffix>$(VersionSuffix)</VersionSuffix>` in the *.csproj* file, pack the current project and update the resulting package version with the given suffix:</span></span>

  ```dotnetcli
  dotnet pack --version-suffix "ci-1234"
  ```

- <span data-ttu-id="530e2-164">使用 `PackageVersion` MSBuild 属性将包版本设置为 `2.1.0`：</span><span class="sxs-lookup"><span data-stu-id="530e2-164">Set the package version to `2.1.0` with the `PackageVersion` MSBuild property:</span></span>

  ```dotnetcli
  dotnet pack -p:PackageVersion=2.1.0
  ```

- <span data-ttu-id="530e2-165">打包特定[目标框架](../../standard/frameworks.md)的项目：</span><span class="sxs-lookup"><span data-stu-id="530e2-165">Pack the project for a specific [target framework](../../standard/frameworks.md):</span></span>

  ```dotnetcli
  dotnet pack -p:TargetFrameworks=net45
  ```

- <span data-ttu-id="530e2-166">打包项目，并使用特定运行时 (Windows 10) 进行还原操作：</span><span class="sxs-lookup"><span data-stu-id="530e2-166">Pack the project and use a specific runtime (Windows 10) for the restore operation:</span></span>

  ```dotnetcli
  dotnet pack --runtime win10-x64
  ```

- <span data-ttu-id="530e2-167">使用 .nuspec 文件打包项目：</span><span class="sxs-lookup"><span data-stu-id="530e2-167">Pack the project using a *.nuspec* file:</span></span>

  ```dotnetcli
  dotnet pack ~/projects/app1/project.csproj -p:NuspecFile=~/projects/app1/project.nuspec -p:NuspecBasePath=~/projects/app1/nuget
  ```

  <span data-ttu-id="530e2-168">要了解如何使用 `NuspecFile`、`NuspecBasePath` 和 `NuspecProperties`，请参阅以下资源：</span><span class="sxs-lookup"><span data-stu-id="530e2-168">For information about how to use `NuspecFile`, `NuspecBasePath`, and `NuspecProperties`, see the following resources:</span></span>

  - [<span data-ttu-id="530e2-169">使用 .nuspec 打包</span><span class="sxs-lookup"><span data-stu-id="530e2-169">Packing using a .nuspec</span></span>](/nuget/reference/msbuild-targets#packing-using-a-nuspec)
  - [<span data-ttu-id="530e2-170">用于创建自定义包的高级扩展点</span><span class="sxs-lookup"><span data-stu-id="530e2-170">Advanced extension points to create customized package</span></span>](/nuget/reference/msbuild-targets#advanced-extension-points-to-create-customized-package)
  - [<span data-ttu-id="530e2-171">全局属性</span><span class="sxs-lookup"><span data-stu-id="530e2-171">Global properties</span></span>](/visualstudio/msbuild/msbuild-properties#global-properties)
