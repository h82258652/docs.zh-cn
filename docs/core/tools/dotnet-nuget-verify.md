---
title: dotnet nuget verify 命令
description: dotnet nuget verify 命令可验证已签名的包。
author: kartheekp-ms
ms.date: 10/08/2020
ms.openlocfilehash: 1c300e5a09b4049a9895b9b3f6c742f701dc2200
ms.sourcegitcommit: 985c603cb21a085f8a8105f34ff5b87a44b76ab4
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/16/2021
ms.locfileid: "107564839"
---
# <a name="dotnet-nuget-verify"></a><span data-ttu-id="5d834-103">dotnet nuget verify</span><span class="sxs-lookup"><span data-stu-id="5d834-103">dotnet nuget verify</span></span>

<span data-ttu-id="5d834-104">**本文适用于：** ✔️ .NET 5.0.100-rc.2.x SDK 及更高版本</span><span class="sxs-lookup"><span data-stu-id="5d834-104">**This article applies to:** ✔️ .NET 5.0.100-rc.2.x SDK and later versions</span></span>

## <a name="name"></a><span data-ttu-id="5d834-105">名称</span><span class="sxs-lookup"><span data-stu-id="5d834-105">Name</span></span>

<span data-ttu-id="5d834-106">`dotnet nuget verify` -验证已签名的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="5d834-106">`dotnet nuget verify` - Verifies a signed NuGet package.</span></span>

## <a name="synopsis"></a><span data-ttu-id="5d834-107">摘要</span><span class="sxs-lookup"><span data-stu-id="5d834-107">Synopsis</span></span>

```dotnetcli
dotnet nuget verify [<package-path(s)>]
    [--all]
    [--certificate-fingerprint <FINGERPRINT>]
    [-v|--verbosity <LEVEL>]

dotnet nuget verify -h|--help
```

## <a name="description"></a><span data-ttu-id="5d834-108">描述</span><span class="sxs-lookup"><span data-stu-id="5d834-108">Description</span></span>

<span data-ttu-id="5d834-109">`dotnet nuget verify` 命令验证已签名的 NuGet 包。</span><span class="sxs-lookup"><span data-stu-id="5d834-109">The `dotnet nuget verify` command verifies a signed NuGet package.</span></span>

## <a name="arguments"></a><span data-ttu-id="5d834-110">参数</span><span class="sxs-lookup"><span data-stu-id="5d834-110">Arguments</span></span>

- **`package-path(s)`**

  <span data-ttu-id="5d834-111">指定要验证的包的文件路径。</span><span class="sxs-lookup"><span data-stu-id="5d834-111">Specifies the file path to the package(s) to be verified.</span></span> <span data-ttu-id="5d834-112">可以传入多个位置参数以验证多个包。</span><span class="sxs-lookup"><span data-stu-id="5d834-112">Multiple position arguments can be passed in to verify multiple packages.</span></span>

## <a name="options"></a><span data-ttu-id="5d834-113">选项</span><span class="sxs-lookup"><span data-stu-id="5d834-113">Options</span></span>

- **`--all`**

  <span data-ttu-id="5d834-114">指定应对包执行的所有可能的验证。</span><span class="sxs-lookup"><span data-stu-id="5d834-114">Specifies that all verifications possible should be performed on the package(s).</span></span> <span data-ttu-id="5d834-115">默认情况下，只验证 `signatures`。</span><span class="sxs-lookup"><span data-stu-id="5d834-115">By default, only `signatures` are verified.</span></span>

> [!NOTE]
> <span data-ttu-id="5d834-116">此命令当前仅支持 `signature` 验证。</span><span class="sxs-lookup"><span data-stu-id="5d834-116">This command currently supports only `signature` verification.</span></span>

- **`--certificate-fingerprint <FINGERPRINT>`**

  <span data-ttu-id="5d834-117">验证签名者证书是否与指定的其中一个 `SHA256` 指纹匹配。</span><span class="sxs-lookup"><span data-stu-id="5d834-117">Verify that the signer certificate matches with one of the specified `SHA256` fingerprints.</span></span> <span data-ttu-id="5d834-118">可以多次提供此选项以提供多个指纹。</span><span class="sxs-lookup"><span data-stu-id="5d834-118">This option can be supplied multiple times to provide multiple fingerprints.</span></span>

* **`-v|--verbosity <LEVEL>`**

  <span data-ttu-id="5d834-119">设置 [MSBuild 详细级别](/visualstudio/msbuild/obtaining-build-logs-with-msbuild#verbosity-settings)。</span><span class="sxs-lookup"><span data-stu-id="5d834-119">Sets the [MSBuild verbosity level](/visualstudio/msbuild/obtaining-build-logs-with-msbuild#verbosity-settings).</span></span> <span data-ttu-id="5d834-120">允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。</span><span class="sxs-lookup"><span data-stu-id="5d834-120">Allowed values are `q[uiet]`, `m[inimal]`, `n[ormal]`, `d[etailed]`, and `diag[nostic]`.</span></span> <span data-ttu-id="5d834-121">默认值为 `minimal`。</span><span class="sxs-lookup"><span data-stu-id="5d834-121">The default is `minimal`.</span></span>

    <span data-ttu-id="5d834-122">下表显示了针对每种详细级别显示的内容。</span><span class="sxs-lookup"><span data-stu-id="5d834-122">The following table shows what is displayed for each verbosity level.</span></span>

                                      | `q[uiet]` | `m[inimal]` | `n[ormal]` | `d[etailed]` | `diag[nostic]`
    ----------------------------------| --------- | ----------- | ---------- | -----------| --------------
    `Certificate chain Information`   | ❌       | ❌          | ❌         | <span data-ttu-id="5d834-123">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-123">✔️</span></span>         | <span data-ttu-id="5d834-124">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-124">✔️</span></span>
    `Path to package being verified`  | ❌       | ❌          | <span data-ttu-id="5d834-125">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-125">✔️</span></span>         | <span data-ttu-id="5d834-126">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-126">✔️</span></span>         | <span data-ttu-id="5d834-127">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-127">✔️</span></span>
    `Hashing algorithm used for signature`        | ❌       | ❌          | <span data-ttu-id="5d834-128">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-128">✔️</span></span>         | <span data-ttu-id="5d834-129">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-129">✔️</span></span>         | <span data-ttu-id="5d834-130">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-130">✔️</span></span>
    `Author/Repository Certificate -> SHA1 hash`| ❌       | ❌          | <span data-ttu-id="5d834-131">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-131">✔️</span></span>         | <span data-ttu-id="5d834-132">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-132">✔️</span></span>         | <span data-ttu-id="5d834-133">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-133">✔️</span></span>
    `Author/Repository Certificate -> Issued By`| ❌       | ❌          | <span data-ttu-id="5d834-134">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-134">✔️</span></span>         | <span data-ttu-id="5d834-135">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-135">✔️</span></span>         | <span data-ttu-id="5d834-136">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-136">✔️</span></span>
    `Timestamp Certificate -> Issued By`| ❌       | ❌          | <span data-ttu-id="5d834-137">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-137">✔️</span></span>         | <span data-ttu-id="5d834-138">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-138">✔️</span></span>         | <span data-ttu-id="5d834-139">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-139">✔️</span></span>
    `Timestamp Certificate -> SHA-256 hash`| ❌       | ❌          | <span data-ttu-id="5d834-140">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-140">✔️</span></span>         | <span data-ttu-id="5d834-141">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-141">✔️</span></span>         | <span data-ttu-id="5d834-142">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-142">✔️</span></span>
    `Timestamp Certificate -> Validity period`| ❌       | ❌          | <span data-ttu-id="5d834-143">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-143">✔️</span></span>         | <span data-ttu-id="5d834-144">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-144">✔️</span></span>         | <span data-ttu-id="5d834-145">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-145">✔️</span></span>
    `Timestamp Certificate -> SHA1 hash`| ❌       | ❌          | <span data-ttu-id="5d834-146">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-146">✔️</span></span>         | <span data-ttu-id="5d834-147">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-147">✔️</span></span>         | <span data-ttu-id="5d834-148">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-148">✔️</span></span>
    `Timestamp Certificate -> Subject name`| ❌       | ❌          | <span data-ttu-id="5d834-149">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-149">✔️</span></span>         | <span data-ttu-id="5d834-150">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-150">✔️</span></span>         | <span data-ttu-id="5d834-151">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-151">✔️</span></span>
    `Author/Repository Certificate -> Subject name`| ❌       | <span data-ttu-id="5d834-152">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-152">✔️</span></span>          | <span data-ttu-id="5d834-153">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-153">✔️</span></span>         | <span data-ttu-id="5d834-154">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-154">✔️</span></span>         | <span data-ttu-id="5d834-155">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-155">✔️</span></span>
    `Author/Repository Certificate -> SHA-256 hash`| ❌       | <span data-ttu-id="5d834-156">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-156">✔️</span></span>          | <span data-ttu-id="5d834-157">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-157">✔️</span></span>         | <span data-ttu-id="5d834-158">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-158">✔️</span></span>         | <span data-ttu-id="5d834-159">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-159">✔️</span></span>
    `Author/Repository Certificate -> Validity period`| ❌       | <span data-ttu-id="5d834-160">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-160">✔️</span></span>          | <span data-ttu-id="5d834-161">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-161">✔️</span></span>         | <span data-ttu-id="5d834-162">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-162">✔️</span></span>         | <span data-ttu-id="5d834-163">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-163">✔️</span></span>
    `Author/Repository Certificate -> Service index URL (If applicable)`| ❌       | <span data-ttu-id="5d834-164">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-164">✔️</span></span>          | <span data-ttu-id="5d834-165">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-165">✔️</span></span>         | <span data-ttu-id="5d834-166">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-166">✔️</span></span>         | <span data-ttu-id="5d834-167">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-167">✔️</span></span>
    `Package name being verified`                    | ❌       | <span data-ttu-id="5d834-168">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-168">✔️</span></span>          | <span data-ttu-id="5d834-169">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-169">✔️</span></span>         | <span data-ttu-id="5d834-170">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-170">✔️</span></span>         | <span data-ttu-id="5d834-171">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-171">✔️</span></span>
    `Type of signature (author or repository)`| ❌       | <span data-ttu-id="5d834-172">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-172">✔️</span></span>          | <span data-ttu-id="5d834-173">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-173">✔️</span></span>         | <span data-ttu-id="5d834-174">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-174">✔️</span></span>         | <span data-ttu-id="5d834-175">✔️</span><span class="sxs-lookup"><span data-stu-id="5d834-175">✔️</span></span>

    <span data-ttu-id="5d834-176">❌ 表示未显示的详细信息。</span><span class="sxs-lookup"><span data-stu-id="5d834-176">❌ indicates details that are **not** displayed.</span></span> <span data-ttu-id="5d834-177">✔️ 表示已显示的详细信息。</span><span class="sxs-lookup"><span data-stu-id="5d834-177">✔️ indicates details that are displayed.</span></span>

* **`-h|--help`**

  <span data-ttu-id="5d834-178">打印出有关命令的简短帮助。</span><span class="sxs-lookup"><span data-stu-id="5d834-178">Prints out a short help for the command.</span></span>

## <a name="examples"></a><span data-ttu-id="5d834-179">示例</span><span class="sxs-lookup"><span data-stu-id="5d834-179">Examples</span></span>

- <span data-ttu-id="5d834-180">验证 foo.nupkg：</span><span class="sxs-lookup"><span data-stu-id="5d834-180">Verify *foo.nupkg*:</span></span>

  ```dotnetcli
  dotnet nuget verify foo.nupkg
  ```

- <span data-ttu-id="5d834-181">验证多个 NuGet 包 - foo.nupkg 和指定目录中的所有 .nupkg 文件 ：</span><span class="sxs-lookup"><span data-stu-id="5d834-181">Verify multiple NuGet packages - *foo.nupkg* and *all .nupkg files in the directory specified*:</span></span>

  ```dotnetcli
  dotnet nuget verify foo.nupkg c:\mydir\*.nupkg
  ```

- <span data-ttu-id="5d834-182">验证 foo.nupkg 签名是否与指定的证书指纹匹配：</span><span class="sxs-lookup"><span data-stu-id="5d834-182">Verify *foo.nupkg* signature matches with the specified certificate fingerprint:</span></span>

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039
  ```

- <span data-ttu-id="5d834-183">验证 foo.nupkg 签名是否与指定的其中一个证书指纹匹配：</span><span class="sxs-lookup"><span data-stu-id="5d834-183">Verify *foo.nupkg* signature matches with one of the specified certificate fingerprints:</span></span>

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 --certificate-fingerprint EC10992GG5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E027
  ```
