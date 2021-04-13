---
title: dotnet nuget push 命令
description: dotnet nuget push 命令可将包推送到服务器并发布。
author: karann-msft
ms.date: 02/14/2020
ms.openlocfilehash: 71bc14fa729945b3d1e1508d014287cc5355f8cc
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/27/2021
ms.locfileid: "105637554"
---
# <a name="dotnet-nuget-push"></a><span data-ttu-id="f95e2-103">dotnet nuget push</span><span class="sxs-lookup"><span data-stu-id="f95e2-103">dotnet nuget push</span></span>

<span data-ttu-id="f95e2-104">**本文适用于：** ✔️ .NET Core 2.x SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="f95e2-104">**This article applies to:** ✔️ .NET Core 2.x SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="f95e2-105">“属性”</span><span class="sxs-lookup"><span data-stu-id="f95e2-105">Name</span></span>

<span data-ttu-id="f95e2-106">`dotnet nuget push` - 将包推送到服务器，并将其发布。</span><span class="sxs-lookup"><span data-stu-id="f95e2-106">`dotnet nuget push` - Pushes a package to the server and publishes it.</span></span>

## <a name="synopsis"></a><span data-ttu-id="f95e2-107">摘要</span><span class="sxs-lookup"><span data-stu-id="f95e2-107">Synopsis</span></span>

```dotnetcli
dotnet nuget push [<ROOT>] [-d|--disable-buffering] [--force-english-output]
    [--interactive] [-k|--api-key <API_KEY>] [-n|--no-symbols]
    [--no-service-endpoint] [-s|--source <SOURCE>] [--skip-duplicate]
    [-sk|--symbol-api-key <API_KEY>] [-ss|--symbol-source <SOURCE>]
    [-t|--timeout <TIMEOUT>]

dotnet nuget push -h|--help
```

## <a name="description"></a><span data-ttu-id="f95e2-108">描述</span><span class="sxs-lookup"><span data-stu-id="f95e2-108">Description</span></span>

<span data-ttu-id="f95e2-109">`dotnet nuget push` 将包推送到服务器，并将其发布。</span><span class="sxs-lookup"><span data-stu-id="f95e2-109">The `dotnet nuget push` command pushes a package to the server and publishes it.</span></span> <span data-ttu-id="f95e2-110">push 命令使用在系统的 NuGet 配置文件或配置文件链中找到的服务器和凭据详细信息。</span><span class="sxs-lookup"><span data-stu-id="f95e2-110">The push command uses server and credential details found in the system's NuGet config file or chain of config files.</span></span> <span data-ttu-id="f95e2-111">有关配置文件的详细信息，请参阅 [Configuring NuGet Behavior](/nuget/consume-packages/configuring-nuget-behavior)（配置 NuGet 行为）。</span><span class="sxs-lookup"><span data-stu-id="f95e2-111">For more information on config files, see [Configuring NuGet Behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span> <span data-ttu-id="f95e2-112">通过加载 *%AppData%\NuGet\NuGet.config* (Windows) 或 *$HOME/.local/share* (Linux/macOS) 获得 NuGet 的默认配置，然后加载任意 *nuget.config* 或 *.nuget\nuget.config*，从驱动器的根目录开始，并在当前目录中结束。</span><span class="sxs-lookup"><span data-stu-id="f95e2-112">NuGet's default configuration is obtained by loading *%AppData%\NuGet\NuGet.config* (Windows) or *$HOME/.local/share* (Linux/macOS), then loading any *nuget.config* or *.nuget\nuget.config* starting from the root of drive and ending in the current directory.</span></span>

<span data-ttu-id="f95e2-113">命令推送现有包。</span><span class="sxs-lookup"><span data-stu-id="f95e2-113">The command pushes an existing package.</span></span> <span data-ttu-id="f95e2-114">它不会创建包。</span><span class="sxs-lookup"><span data-stu-id="f95e2-114">It doesn't create a package.</span></span> <span data-ttu-id="f95e2-115">若要创建包，请使用 [`dotnet pack`](dotnet-pack.md)。</span><span class="sxs-lookup"><span data-stu-id="f95e2-115">To create a package, use [`dotnet pack`](dotnet-pack.md).</span></span>

## <a name="arguments"></a><span data-ttu-id="f95e2-116">自变量</span><span class="sxs-lookup"><span data-stu-id="f95e2-116">Arguments</span></span>

- **`ROOT`**

  <span data-ttu-id="f95e2-117">指定要推送的包的文件路径。</span><span class="sxs-lookup"><span data-stu-id="f95e2-117">Specifies the file path to the package to be pushed.</span></span>

## <a name="options"></a><span data-ttu-id="f95e2-118">选项</span><span class="sxs-lookup"><span data-stu-id="f95e2-118">Options</span></span>

- **`-d|--disable-buffering`**

  <span data-ttu-id="f95e2-119">当推送到 HTTP(S) 服务器以减少内存使用率时，禁用缓冲。</span><span class="sxs-lookup"><span data-stu-id="f95e2-119">Disables buffering when pushing to an HTTP(S) server to reduce memory usage.</span></span>

- **`--force-english-output`**

  <span data-ttu-id="f95e2-120">使用固定的、基于英语的区域性强制运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="f95e2-120">Forces the application to run using an invariant, English-based culture.</span></span>

- **`-h|--help`**

  <span data-ttu-id="f95e2-121">打印出有关命令的简短帮助。</span><span class="sxs-lookup"><span data-stu-id="f95e2-121">Prints out a short help for the command.</span></span>

- **`--interactive`**

  <span data-ttu-id="f95e2-122">对于身份验证等操作，允许命令阻止并要求手动操作。</span><span class="sxs-lookup"><span data-stu-id="f95e2-122">Allows the command to block and requires manual action for operations like authentication.</span></span> <span data-ttu-id="f95e2-123">自 .NET Core 2.2 SDK 起可用的选项。</span><span class="sxs-lookup"><span data-stu-id="f95e2-123">Option available since .NET Core 2.2 SDK.</span></span>

- **`-k|--api-key <API_KEY>`**

  <span data-ttu-id="f95e2-124">服务器的 API 密钥。</span><span class="sxs-lookup"><span data-stu-id="f95e2-124">The API key for the server.</span></span>

- **`-n|--no-symbols`**

  <span data-ttu-id="f95e2-125">不推送符号（即使存在）。</span><span class="sxs-lookup"><span data-stu-id="f95e2-125">Doesn't push symbols (even if present).</span></span>

- **`--no-service-endpoint`**

  <span data-ttu-id="f95e2-126">不将“api/v2/package”追加至源 URL。</span><span class="sxs-lookup"><span data-stu-id="f95e2-126">Doesn't append "api/v2/package" to the source URL.</span></span> <span data-ttu-id="f95e2-127">自 .NET Core 2.1 SDK 起可用的选项。</span><span class="sxs-lookup"><span data-stu-id="f95e2-127">Option available since .NET Core 2.1 SDK.</span></span>

- **`-s|--source <SOURCE>`**

  <span data-ttu-id="f95e2-128">指定服务器 URL。</span><span class="sxs-lookup"><span data-stu-id="f95e2-128">Specifies the server URL.</span></span> <span data-ttu-id="f95e2-129">NuGet 标识 UNC 或本地文件夹源，只在其中复制文件，而不会使用 HTTP 进行推送。</span><span class="sxs-lookup"><span data-stu-id="f95e2-129">NuGet identifies a UNC or local folder source and simply copies the file there instead of pushing it using HTTP.</span></span>
  > [!IMPORTANT]
  > <span data-ttu-id="f95e2-130">从 NuGet 3.4.2 开始，此参数为必需，除非 NuGet 配置文件指定了 `DefaultPushSource` 值。</span><span class="sxs-lookup"><span data-stu-id="f95e2-130">Starting with NuGet 3.4.2, this is a mandatory parameter unless the NuGet config file specifies a `DefaultPushSource` value.</span></span> <span data-ttu-id="f95e2-131">有关详细信息，请参阅[配置 NuGet 行为](/nuget/consume-packages/configuring-nuget-behavior)。</span><span class="sxs-lookup"><span data-stu-id="f95e2-131">For more information, see [Configuring NuGet behavior](/nuget/consume-packages/configuring-nuget-behavior).</span></span>

- **`--skip-duplicate`**

  <span data-ttu-id="f95e2-132">将多个包推送到 HTTP(S) 服务器时，将任何 409 冲突响应视为警告，以便可以继续推送。</span><span class="sxs-lookup"><span data-stu-id="f95e2-132">When pushing multiple packages to an HTTP(S) server, treats any 409 Conflict response as a warning so that the push can continue.</span></span> <span data-ttu-id="f95e2-133">自 .NET Core 3.1 SDK 起可用。</span><span class="sxs-lookup"><span data-stu-id="f95e2-133">Available since .NET Core 3.1 SDK.</span></span>

- **`-sk|--symbol-api-key <API_KEY>`**

  <span data-ttu-id="f95e2-134">符号服务器的 API 密钥。</span><span class="sxs-lookup"><span data-stu-id="f95e2-134">The API key for the symbol server.</span></span>

- **`-ss|--symbol-source <SOURCE>`**

  <span data-ttu-id="f95e2-135">指定符号服务器 URL。</span><span class="sxs-lookup"><span data-stu-id="f95e2-135">Specifies the symbol server URL.</span></span>

- **`-t|--timeout <TIMEOUT>`**

  <span data-ttu-id="f95e2-136">指定推送到服务器的超时（秒）。</span><span class="sxs-lookup"><span data-stu-id="f95e2-136">Specifies the timeout for pushing to a server in seconds.</span></span> <span data-ttu-id="f95e2-137">默认值为 300 秒（5 分钟）。</span><span class="sxs-lookup"><span data-stu-id="f95e2-137">Defaults to 300 seconds (5 minutes).</span></span> <span data-ttu-id="f95e2-138">指定 0 时应用默认值。</span><span class="sxs-lookup"><span data-stu-id="f95e2-138">Specifying 0 applies the default value.</span></span>

## <a name="examples"></a><span data-ttu-id="f95e2-139">示例</span><span class="sxs-lookup"><span data-stu-id="f95e2-139">Examples</span></span>

- <span data-ttu-id="f95e2-140">将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源（使用 API 密钥）：</span><span class="sxs-lookup"><span data-stu-id="f95e2-140">Push *foo.nupkg* to the default push source specified in the NuGet config file, using an API key:</span></span>

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a
  ```

- <span data-ttu-id="f95e2-141">将 foo.nupkg  推送到官方 NuGet 服务器，以指定 API 密钥：</span><span class="sxs-lookup"><span data-stu-id="f95e2-141">Push *foo.nupkg* to the official NuGet server, specifying an API key:</span></span>

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://api.nuget.org/v3/index.json
  ```
  
  * <span data-ttu-id="f95e2-142">将 foo.nupkg 推送到自定义推送源 `https://customsource`（指定 API 密钥）  ：</span><span class="sxs-lookup"><span data-stu-id="f95e2-142">Push *foo.nupkg* to the custom push source `https://customsource`, specifying an API key:</span></span>

  ```dotnetcli
  dotnet nuget push foo.nupkg -k 4003d786-cc37-4004-bfdf-c4f3e8ef9b3a -s https://customsource/
  ```

- <span data-ttu-id="f95e2-143">将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源：</span><span class="sxs-lookup"><span data-stu-id="f95e2-143">Push *foo.nupkg* to the default push source specified in the NuGet config file:</span></span>

  ```dotnetcli
  dotnet nuget push foo.nupkg
  ```

- <span data-ttu-id="f95e2-144">将 foo.symbols.nupkg 推送到默认符号源  ：</span><span class="sxs-lookup"><span data-stu-id="f95e2-144">Push *foo.symbols.nupkg* to the default symbols source:</span></span>

  ```dotnetcli
  dotnet nuget push foo.symbols.nupkg
  ```

- <span data-ttu-id="f95e2-145">将 foo.nupkg 推送到 NuGet 配置文件中指定的默认推送源（超时 360 秒）：</span><span class="sxs-lookup"><span data-stu-id="f95e2-145">Push *foo.nupkg* to the default push source specified in the NuGet config file, with a 360-second timeout:</span></span>

  ```dotnetcli
  dotnet nuget push foo.nupkg --timeout 360
  ```

- <span data-ttu-id="f95e2-146">将当前目录中的所有 .nupkg 文件推送到 NuGet 配置文件中指定的默认推送源：</span><span class="sxs-lookup"><span data-stu-id="f95e2-146">Push all *.nupkg* files in the current directory to the default push source specified in the NuGet config file:</span></span>

  ```dotnetcli
  dotnet nuget push "*.nupkg"
  ```

  > [!NOTE]
  > <span data-ttu-id="f95e2-147">如果此命令不起作用，则可能是较旧版本的 SDK（.NET Core 2.1 SDK 及更早版本）中的 bug 导致的。</span><span class="sxs-lookup"><span data-stu-id="f95e2-147">If this command doesn't work, it might be due to a bug that existed in older versions of the SDK (.NET Core 2.1 SDK and earlier versions).</span></span>
  > <span data-ttu-id="f95e2-148">要解决此问题，请升级 SDK 版本或改为运行以下命令：`dotnet nuget push "**/*.nupkg"`</span><span class="sxs-lookup"><span data-stu-id="f95e2-148">To fix this, upgrade your SDK version or run the following command instead: `dotnet nuget push "**/*.nupkg"`</span></span>
  
  > [!NOTE]
  > <span data-ttu-id="f95e2-149">用于执行文件组合的 bash 等 shell 需要用引号括起来。</span><span class="sxs-lookup"><span data-stu-id="f95e2-149">The enclosing quotes are required for shells such as bash that perform file globbing.</span></span> <span data-ttu-id="f95e2-150">有关详细信息，请参阅 [NuGet/Home#4393](https://github.com/NuGet/Home/issues/4393#issuecomment-667618120)。</span><span class="sxs-lookup"><span data-stu-id="f95e2-150">For more information, see [NuGet/Home#4393](https://github.com/NuGet/Home/issues/4393#issuecomment-667618120).</span></span>

- <span data-ttu-id="f95e2-151">将所有 .nupkg 文件推送到 NuGet 配置文件中指定的默认推送源，即使 HTTP(S) 服务器返回“409 Conflict”响应也是如此：</span><span class="sxs-lookup"><span data-stu-id="f95e2-151">Push all *.nupkg* files to the default push source specified in the NuGet config file, even if a 409 Conflict response is returned by an HTTP(S) server:</span></span>

  ```dotnetcli
  dotnet nuget push "*.nupkg" --skip-duplicate
  ```

- <span data-ttu-id="f95e2-152">将当前目录中的所有 .nupkg 文件推送到本地源目录  ：</span><span class="sxs-lookup"><span data-stu-id="f95e2-152">Push all *.nupkg* files in the current directory to a local feed directory:</span></span>

  ```dotnetcli
  dotnet nuget push "*.nupkg" -s c:\mydir
  ```

  <span data-ttu-id="f95e2-153">此命令不会将包存储在分层文件夹结构中，因此建议优化性能。</span><span class="sxs-lookup"><span data-stu-id="f95e2-153">This command doesn't store packages in a hierarchical folder structure, which is recommended to optimize performance.</span></span> <span data-ttu-id="f95e2-154">有关详细信息，请参阅[本地源](/nuget/hosting-packages/local-feeds)。</span><span class="sxs-lookup"><span data-stu-id="f95e2-154">For more information, see [Local feeds](/nuget/hosting-packages/local-feeds).</span></span>  
