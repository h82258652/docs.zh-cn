---
ms.openlocfilehash: 94d2f0acd34532c9c9960fee87b6ef657f04cd72
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494307"
---
### <a name="path-apis-dont-throw-an-exception-for-invalid-characters"></a>路径 API 对无效字符不引发异常

如果找到无效的字符，涉及文件路径的 API 将不再验证路径字符或引发 `M:System.ArgumentException>。

#### <a name="change-description"></a>更改说明

在 .NET Framework 和 .NET Core 1.0-2.0 中，如果路径参数包含无效路径字符，[受影响的 API](#affected-apis) 部分中列出的方法便会引发 <xref:System.ArgumentException>。 从 .NET Core 2.1 开始，如果找到无效字符，这些方法就不再检查[无效路径字符](xref:System.IO.Path.GetInvalidPathChars%2A)或引发异常。

#### <a name="reason-for-change"></a>更改原因

主动验证路径字符会对某些跨平台场景形成阻碍。 引入此更改是为了使 .NET 不尝试复制或预测操作系统 API 调用的结果。 有关详细信息，请参阅博客文章 [.Net Core 2.1 中的 System.IO 速览](/archive/blogs/jeremykuhne/system-io-in-net-core-2-1-sneak-peek)。

#### <a name="version-introduced"></a>引入的版本

.NET Core 2.1

#### <a name="recommended-action"></a>建议的操作

如果代码依赖这些 API 来检查无效字符，则可以添加对 <xref:System.IO.Path.GetInvalidPathChars%2A?displayProperty=nameWithType> 的调用。

#### <a name="affected-apis"></a>受影响的 API

- <xref:System.IO.Directory.CreateDirectory%2A?displayProperty=fullName>
- <xref:System.IO.Directory.Delete%2A?displayProperty=fullName>
- <xref:System.IO.Directory.EnumerateDirectories%2A?displayProperty=fullName>
- <xref:System.IO.Directory.EnumerateFiles%2A?displayProperty=fullName>
- <xref:System.IO.Directory.EnumerateFileSystemEntries%2A?displayProperty=fullName>
- <xref:System.IO.Directory.GetCreationTime(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetCreationTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetDirectories%2A?displayProperty=fullName>
- <xref:System.IO.Directory.GetDirectoryRoot(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetFiles%2A?displayProperty=fullName>
- <xref:System.IO.Directory.GetFileSystemEntries%2A?displayProperty=fullName>
- <xref:System.IO.Directory.GetLastAccessTime(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetLastAccessTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetLastWriteTime(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetLastWriteTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.GetParent(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.Move(System.String,System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.SetCreationTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.Directory.SetCreationTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.Directory.SetCurrentDirectory(System.String)?displayProperty=fullName>
- <xref:System.IO.Directory.SetLastAccessTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.Directory.SetLastAccessTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.Directory.SetLastWriteTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.Directory.SetLastWriteTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- [System.IO.DirectoryInfo ctor](xref:System.IO.DirectoryInfo.%23ctor(System.String))
- <xref:System.IO.Directory.GetDirectories%2A?displayProperty=fullName>
- <xref:System.IO.Directory.GetFiles%2A?displayProperty=fullName>
- <xref:System.IO.DirectoryInfo.GetFileSystemInfos%2A?displayProperty=fullName>
- <xref:System.IO.File.AppendAllText%2A?displayProperty=fullName>
- <xref:System.IO.File.AppendAllTextAsync%2A?displayProperty=fullName>
- <xref:System.IO.File.Copy%2A?displayProperty=fullName>
- <xref:System.IO.File.Create%2A?displayProperty=fullName>
- <xref:System.IO.File.CreateText%2A?displayProperty=fullName>
- <xref:System.IO.File.Decrypt%2A?displayProperty=fullName>
- <xref:System.IO.File.Delete%2A?displayProperty=fullName>
- <xref:System.IO.File.Encrypt%2A?displayProperty=fullName>
- <xref:System.IO.File.GetAttributes(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetCreationTime(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetCreationTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetLastAccessTime(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetLastAccessTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetLastWriteTime(System.String)?displayProperty=fullName>
- <xref:System.IO.File.GetLastWriteTimeUtc(System.String)?displayProperty=fullName>
- <xref:System.IO.File.Move%2A?displayProperty=fullName>
- <xref:System.IO.File.Open%2A?displayProperty=fullName>
- <xref:System.IO.File.OpenRead(System.String)?displayProperty=fullName>
- <xref:System.IO.File.OpenText(System.String)?displayProperty=fullName>
- <xref:System.IO.File.OpenWrite(System.String)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllBytes(System.String)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllBytesAsync(System.String,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllLines(System.String)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllLinesAsync(System.String,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllText(System.String)?displayProperty=fullName>
- <xref:System.IO.File.ReadAllTextAsync(System.String,System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.File.SetAttributes(System.String,System.IO.FileAttributes)?displayProperty=fullName>
- <xref:System.IO.File.SetCreationTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.SetCreationTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.SetLastAccessTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.SetLastAccessTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.SetLastWriteTime(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.SetLastWriteTimeUtc(System.String,System.DateTime)?displayProperty=fullName>
- <xref:System.IO.File.WriteAllBytes(System.String,System.Byte[])?displayProperty=fullName>
- <xref:System.IO.File.WriteAllBytesAsync(System.String,System.Byte[],System.Threading.CancellationToken)?displayProperty=fullName>
- <xref:System.IO.File.WriteAllLines%2A?displayProperty=fullName>
- <xref:System.IO.File.WriteAllLinesAsync%2A?displayProperty=fullName>
- <xref:System.IO.File.WriteAllText%2A?displayProperty=fullName>
- [System.IO.FileInfo ctor](xref:System.IO.FileInfo.%23ctor(System.String))
- <xref:System.IO.FileInfo.CopyTo%2A?displayProperty=fullName>
- <xref:System.IO.FileInfo.MoveTo%2A?displayProperty=fullName>
- [System.IO.FileStream ctor](xref:System.IO.FileStream.%23ctor%2A)
- <xref:System.IO.Path.GetFullPath(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.IsPathRooted(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.GetPathRoot(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.ChangeExtension(System.String,System.String)?displayProperty=fullName>
- <xref:System.IO.Path.GetDirectoryName(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.GetExtension(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.HasExtension(System.String)?displayProperty=fullName>
- <xref:System.IO.Path.Combine%2A?displayProperty=fullName>

#### <a name="see-also"></a>另请参阅

- [.NET Core 2.1 中的 System.IO 速览](/archive/blogs/jeremykuhne/system-io-in-net-core-2-1-sneak-peek)

<!--

### Category

Core .NET libraries

### Affected APIs

- `Overload:System.IO.Directory.CreateDirectory`
- `Overload:System.IO.Directory.Delete`
- `Overload:System.IO.Directory.EnumerateDirectories`
- `Overload:System.IO.Directory.EnumerateFiles`
- `Overload:System.IO.Directory.EnumerateFileSystemEntries`
- `M:System.IO.Directory.GetCreationTime(System.String)`
- `M:System.IO.Directory.GetCreationTimeUtc(System.String)`
- `Overload:System.IO.Directory.GetDirectories`
- `M:System.IO.Directory.GetDirectoryRoot(System.String)`
- `Overload:System.IO.Directory.GetFiles`
- `Overload:System.IO.Directory.GetFileSystemEntries`
- `M:System.IO.Directory.GetLastAccessTime(System.String)`
- `M:System.IO.Directory.GetLastAccessTimeUtc(System.String)`
- `M:System.IO.Directory.GetLastWriteTime(System.String)`
- `M:System.IO.Directory.GetLastWriteTimeUtc(System.String)`
- `M:System.IO.Directory.GetParent(System.String)`
- `M:System.IO.Directory.Move(System.String,System.String)`
- `M:System.IO.Directory.SetCreationTime(System.String)`
- `M:System.IO.Directory.SetCreationTimeUtc(System.String)`
- `M:System.IO.Directory.SetCurrentDirectory(System.String)`
- `M:System.IO.Directory.SetLastAccessTime(System.String)`
- `M:System.IO.Directory.SetLastAccessTimeUtc(System.String)`
- `M:System.IO.Directory.SetLastWriteTime(System.String)`
- `M:System.IO.Directory.SetLastWriteTimeUtc(System.String)`
- `M:System.IO.DirectoryInfo.%23ctor(System.String)>
- `Overload:System.IO.Directory.GetDirectories`
- `Overload:System.IO.Directory.GetFiles`
- `Overload:System.IO.DirectoryInfo.GetFileSystemInfos`
- `Overload:System.IO.File.AppendAllText`
- `Overload:System.IO.File.AppendAllTextAsync`
- `Overload:System.IO.File.Copy`
- `Overload:System.IO.File.Create`
- `Overload:System.IO.File.CreateText`
- `Overload:System.IO.File.Decrypt`
- `Overload:System.IO.File.Delete`
- `Overload:System.IO.File.Encrypt`
- `M:System.IO.File.GetAttributes(System.String)`
- `M:System.IO.File.GetCreationTime(System.String)`
- `M:System.IO.File.GetCreationTimeUtc(System.String)`
- `M:System.IO.File.GetLastAccessTime(System.String)`
- `M:System.IO.File.GetLastAccessTimeUtc(System.String)`
- `M:System.IO.File.GetLastWriteTime(System.String)`
- `M:System.IO.File.GetLastWriteTimeUtc(System.String)`
- `Overload:System.IO.File.Move`
- `Overload:System.IO.File.Open`
- `M:System.IO.File.OpenRead(System.String)`
- `M:System.IO.File.OpenText(System.String)`
- `M:System.IO.File.OpenWrite(System.String)`
- `M:System.IO.File.ReadAllBytes(System.String)`
- `M:System.IO.File.ReadAllBytesAsync(System.String,System.Threading.CancellationToken)`
- `M:System.IO.File.ReadAllLines(System.String)`
- `M:System.IO.File.ReadAllLinesAsync(System.String,System.Threading.CancellationToken)`
- `M:System.IO.File.ReadAllText(System.String)`
- `M:System.IO.File.ReadAllTextAsync(System.String,System.Threading.CancellationToken)`
- `M:System.IO.File.SetAttributes(System.String)`
- `M:System.IO.File.SetCreationTime(System.String)`
- `M:System.IO.File.SetCreationTimeUtc(System.String)`
- `M:System.IO.File.SetLastAccessTime(System.String)`
- `M:System.IO.File.SetLastAccessTimeUtc(System.String)`
- `M:System.IO.File.SetLastWriteTime(System.String)`
- `M:System.IO.File.SetLastWriteTimeUtc(System.String)`
- `M:System.IO.File.WriteAllBytes(System.String)`
- `M:System.IO.File.WriteAllBytesAsync(System.String,System.Threading.CancellationToken)`
- `Overload:System.IO.File.WriteAllLines`
- `Overload:System.IO.File.WriteAllLinesAsync`
- `Overload:System.IO.File.WriteAllText`
- `M:System.IO.FileInfo.#ctor(System.String)`
- `Overload:System.IO.FileInfo.CopyTo`
- `Overload:System.IO.FileInfo.MoveTo`
- `Overload:System.IO.FileStream.#ctor`
- `M:System.IO.Path.GetFullPath(System.String)`
- `M:System.IO.Path.IsPathRooted(System.String)`
- `M:System.IO.Path.GetPathRoot(System.String)`
- `M:System.IO.Path.ChangeExtension(System.String,System.String)`
- `M:System.IO.Path.GetDirectoryName(System.String)`
- `M:System.IO.Path.GetExtension(System.String)`
- `M:System.IO.Path.HasExtension(System.String)`
- `Overload:System.IO.Path.Combine`

-->
