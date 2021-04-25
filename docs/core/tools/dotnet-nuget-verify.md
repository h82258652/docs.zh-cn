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
# <a name="dotnet-nuget-verify"></a>dotnet nuget verify

**本文适用于：** ✔️ .NET 5.0.100-rc.2.x SDK 及更高版本

## <a name="name"></a>名称

`dotnet nuget verify` -验证已签名的 NuGet 包。

## <a name="synopsis"></a>摘要

```dotnetcli
dotnet nuget verify [<package-path(s)>]
    [--all]
    [--certificate-fingerprint <FINGERPRINT>]
    [-v|--verbosity <LEVEL>]

dotnet nuget verify -h|--help
```

## <a name="description"></a>描述

`dotnet nuget verify` 命令验证已签名的 NuGet 包。

## <a name="arguments"></a>参数

- **`package-path(s)`**

  指定要验证的包的文件路径。 可以传入多个位置参数以验证多个包。

## <a name="options"></a>选项

- **`--all`**

  指定应对包执行的所有可能的验证。 默认情况下，只验证 `signatures`。

> [!NOTE]
> 此命令当前仅支持 `signature` 验证。

- **`--certificate-fingerprint <FINGERPRINT>`**

  验证签名者证书是否与指定的其中一个 `SHA256` 指纹匹配。 可以多次提供此选项以提供多个指纹。

* **`-v|--verbosity <LEVEL>`**

  设置 [MSBuild 详细级别](/visualstudio/msbuild/obtaining-build-logs-with-msbuild#verbosity-settings)。 允许使用的值为 `q[uiet]`、`m[inimal]`、`n[ormal]`、`d[etailed]` 和 `diag[nostic]`。 默认值为 `minimal`。

    下表显示了针对每种详细级别显示的内容。

                                      | `q[uiet]` | `m[inimal]` | `n[ormal]` | `d[etailed]` | `diag[nostic]`
    ----------------------------------| --------- | ----------- | ---------- | -----------| --------------
    `Certificate chain Information`   | ❌       | ❌          | ❌         | ✔️         | ✔️
    `Path to package being verified`  | ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Hashing algorithm used for signature`        | ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> SHA1 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Issued By`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Issued By`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> SHA-256 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Validity period`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> SHA1 hash`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Timestamp Certificate -> Subject name`| ❌       | ❌          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Subject name`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> SHA-256 hash`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Validity period`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Author/Repository Certificate -> Service index URL (If applicable)`| ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Package name being verified`                    | ❌       | ✔️          | ✔️         | ✔️         | ✔️
    `Type of signature (author or repository)`| ❌       | ✔️          | ✔️         | ✔️         | ✔️

    ❌ 表示未显示的详细信息。 ✔️ 表示已显示的详细信息。

* **`-h|--help`**

  打印出有关命令的简短帮助。

## <a name="examples"></a>示例

- 验证 foo.nupkg：

  ```dotnetcli
  dotnet nuget verify foo.nupkg
  ```

- 验证多个 NuGet 包 - foo.nupkg 和指定目录中的所有 .nupkg 文件 ：

  ```dotnetcli
  dotnet nuget verify foo.nupkg c:\mydir\*.nupkg
  ```

- 验证 foo.nupkg 签名是否与指定的证书指纹匹配：

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039
  ```

- 验证 foo.nupkg 签名是否与指定的其中一个证书指纹匹配：

  ```dotnetcli
  dotnet nuget verify foo.nupkg --certificate-fingerprint CE40881FF5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E039 --certificate-fingerprint EC10992GG5F0AD3E58965DA20A9F571EF1651A56933748E1BF1C99E537C4E027
  ```
