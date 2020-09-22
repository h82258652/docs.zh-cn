---
title: dotnet-symbol - .NET Core
description: 安装和使用 dotnet-symbol 命令行工具。
ms.date: 08/26/2020
ms.openlocfilehash: 5a96306fc96525b00e57eda089a45b730a7e3e8c
ms.sourcegitcommit: aa6d8a90a4f5d8fe0f6e967980b8c98433f05a44
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 09/16/2020
ms.locfileid: "90679183"
---
# <a name="symbol-downloader-dotnet-symbol"></a><span data-ttu-id="7e590-103">符号下载器 (dotnet-symbol)</span><span class="sxs-lookup"><span data-stu-id="7e590-103">Symbol downloader (dotnet-symbol)</span></span>

<span data-ttu-id="7e590-104">本文适用于： ✔️ .NET Core 2.1 SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="7e590-104">**This article applies to:** ✔️ .NET Core 2.1 SDK and later versions</span></span>

## <a name="install-dotnet-symbol"></a><span data-ttu-id="7e590-105">安装 dotnet-symbol</span><span class="sxs-lookup"><span data-stu-id="7e590-105">Install dotnet-symbol</span></span>

<span data-ttu-id="7e590-106">若要安装最新版 `dotnet-symbol` [NuGet 包](https://www.nuget.org/packages/dotnet-symbol)，请使用 [dotnet tool install](../tools/dotnet-tool-install.md) 命令：</span><span class="sxs-lookup"><span data-stu-id="7e590-106">To install the latest release version of the `dotnet-symbol` [NuGet package](https://www.nuget.org/packages/dotnet-symbol), use the [dotnet tool install](../tools/dotnet-tool-install.md) command:</span></span>

```dotnetcli
dotnet tool install -g dotnet-symbol
```

## <a name="synopsis"></a><span data-ttu-id="7e590-107">摘要</span><span class="sxs-lookup"><span data-stu-id="7e590-107">Synopsis</span></span>

```console
dotnet-symbol [-h|--help] [options] <FILES>
```

## <a name="description"></a><span data-ttu-id="7e590-108">描述</span><span class="sxs-lookup"><span data-stu-id="7e590-108">Description</span></span>

<span data-ttu-id="7e590-109">`dotnet-symbol` 全局工具下载调试核心转储和小型转储所需的文件（符号、DAC、模块等）。</span><span class="sxs-lookup"><span data-stu-id="7e590-109">The `dotnet-symbol` global tool downloads files (symbols, DAC, modules, etc.) needed for debugging core dumps and minidumps.</span></span> <span data-ttu-id="7e590-110">当调试其他计算机上捕获的转储时，这很有用。</span><span class="sxs-lookup"><span data-stu-id="7e590-110">This can be useful when debugging dumps captured on another machine.</span></span> <span data-ttu-id="7e590-111">`dotnet-symbol` 可用于下载分析转储所需的模块和符号。</span><span class="sxs-lookup"><span data-stu-id="7e590-111">`dotnet-symbol` can download modules and symbols needed to analyze the dump.</span></span>

## <a name="options"></a><span data-ttu-id="7e590-112">选项</span><span class="sxs-lookup"><span data-stu-id="7e590-112">Options</span></span>

- **`--microsoft-symbol-server`**

  <span data-ttu-id="7e590-113">添加“http://msdl.microsoft.com/download/symbols”符号服务器路径（默认）。</span><span class="sxs-lookup"><span data-stu-id="7e590-113">Add 'http://msdl.microsoft.com/download/symbols' symbol server path (default).</span></span>

- **`--server-path <symbol server path>`**

  <span data-ttu-id="7e590-114">将符号服务器添加到服务器路径。</span><span class="sxs-lookup"><span data-stu-id="7e590-114">Add a symbol server to the server path.</span></span>

- **`authenticated-server-path <pat> <server path>`**

  <span data-ttu-id="7e590-115">使用个人访问令牌 (PAT) 将经过身份验证的符号服务器添加到服务器路径。</span><span class="sxs-lookup"><span data-stu-id="7e590-115">Add an authenticated symbol server to the server path using a personal access token (PAT).</span></span>

- **`--cache-directory <file cache directory>`**

  <span data-ttu-id="7e590-116">添加缓存目录。</span><span class="sxs-lookup"><span data-stu-id="7e590-116">Adds a cache directory.</span></span>

- **`--recurse-subdirectories`**

  <span data-ttu-id="7e590-117">处理所有子目录中的输入文件。</span><span class="sxs-lookup"><span data-stu-id="7e590-117">Process input files in all subdirectories.</span></span>

- **`--host-only`**

  <span data-ttu-id="7e590-118">仅下载 lldb 加载核心转储所需的主机程序（即 dotnet）。</span><span class="sxs-lookup"><span data-stu-id="7e590-118">Download only the host program (i.e. dotnet) that lldb needs for loading core dumps.</span></span>

- **`--symbols`**

  <span data-ttu-id="7e590-119">下载符号文件（.pdb、.dbg 和 .dwarf）。</span><span class="sxs-lookup"><span data-stu-id="7e590-119">Download symbol files (.pdb, .dbg, .dwarf).</span></span>

- **`--modules`**

  <span data-ttu-id="7e590-120">下载模块文件（.dll、.so 和 .dylib）。</span><span class="sxs-lookup"><span data-stu-id="7e590-120">Download the module files (.dll, .so, .dylib).</span></span>

- **`--debugging`**

  <span data-ttu-id="7e590-121">下载特殊的调试模块（DAC、DBI 和 SOS）。</span><span class="sxs-lookup"><span data-stu-id="7e590-121">Download the special debugging modules (DAC, DBI, SOS).</span></span>

- **`--windows-pdbs`**

  <span data-ttu-id="7e590-122">当可移植的 PDB 也可用时，会强制下载 Windows PDB。</span><span class="sxs-lookup"><span data-stu-id="7e590-122">Force the downloading of the Windows PDBs when Portable PDBs are also available.</span></span>

- **`-o, --output <output directory>`**

  <span data-ttu-id="7e590-123">设置输出目录。</span><span class="sxs-lookup"><span data-stu-id="7e590-123">Set the output directory.</span></span> <span data-ttu-id="7e590-124">否则，请在输入文件旁边写入（默认）。</span><span class="sxs-lookup"><span data-stu-id="7e590-124">Otherwise, write next to the input file (default).</span></span>

- **`-d, --diagnostics`**

  <span data-ttu-id="7e590-125">启用诊断输出。</span><span class="sxs-lookup"><span data-stu-id="7e590-125">Enable diagnostic output.</span></span>

- **`-h|--help`**

  <span data-ttu-id="7e590-126">显示命令行帮助。</span><span class="sxs-lookup"><span data-stu-id="7e590-126">Shows command-line help.</span></span>

## <a name="download-symbols"></a><span data-ttu-id="7e590-127">下载符号</span><span class="sxs-lookup"><span data-stu-id="7e590-127">Download symbols</span></span>

<span data-ttu-id="7e590-128">默认情况下，针对转储文件运行 `dotnet-symbol` 将下载调试转储所需的所有模块、符号和 DAC/DBI 文件，包括托管程序集。</span><span class="sxs-lookup"><span data-stu-id="7e590-128">Running `dotnet-symbol` against a dump file will, by default, download all the modules, symbols, and DAC/DBI files needed to debug the dump including the managed assemblies.</span></span> <span data-ttu-id="7e590-129">由于 SOS 现在可以按需下载符号，因此可以使用仅带主机 (dotnet) 和调试模块的 lldb 分析大多数 Linux 核心转储。</span><span class="sxs-lookup"><span data-stu-id="7e590-129">Because SOS can now download symbols when needed, most Linux core dumps can be analyzed using lldb with only the host (dotnet) and debugging modules.</span></span> <span data-ttu-id="7e590-130">若要获取使用 lldb 诊断核心转储所需的这些文件，请运行以下内容：</span><span class="sxs-lookup"><span data-stu-id="7e590-130">To get these files necessary for diagnosing a core dump with lldb run:</span></span>

```console
dotnet-symbol --host-only --debugging <dump file path>
```

## <a name="troubleshoot"></a><span data-ttu-id="7e590-131">故障排除</span><span class="sxs-lookup"><span data-stu-id="7e590-131">Troubleshoot</span></span>

- <span data-ttu-id="7e590-132">下载符号时出现“404 未找到”。</span><span class="sxs-lookup"><span data-stu-id="7e590-132">404 Not Found while downloading symbols.</span></span>

   <span data-ttu-id="7e590-133">只有通过官方渠道（例如[官方网站](https://dotnet.microsoft.com/download/dotnet-core)和 [dotnet 安装脚本中的默认源](../tools/dotnet-install-script.md)）获得的官方 .NET Core 运行时版本才支持符号下载。</span><span class="sxs-lookup"><span data-stu-id="7e590-133">Symbol download is only supported for official .NET Core runtime versions acquired through official channels such as [the official web site](https://dotnet.microsoft.com/download/dotnet-core) and the [default sources in the dotnet installation scripts](../tools/dotnet-install-script.md).</span></span> <span data-ttu-id="7e590-134">下载调试文件时出现 404 错误，这可能表示转储是使用来自其他源的 .NET Core 运行时创建的，例如，从本地源、特定 Linux 发行版或从社区站点（例如 archlinux）构建的转储。</span><span class="sxs-lookup"><span data-stu-id="7e590-134">A 404 error while downloading debugging files may indicate that the dump was created with a .NET Core runtime from another source, such as one built from source locally or for a particular Linux distro, or from community sites like archlinux.</span></span> <span data-ttu-id="7e590-135">在此类情况下，应从这些源或创建转储文件的环境复制调试所需的文件（dotnet、libcoreclr.so 和 libmscordaccore.so）。</span><span class="sxs-lookup"><span data-stu-id="7e590-135">In such cases, file necessary for debugging (dotnet, libcoreclr.so, and libmscordaccore.so) should be copied from those sources or from the environment the dump file was created in.</span></span>