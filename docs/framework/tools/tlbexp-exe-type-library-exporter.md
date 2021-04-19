---
title: Tlbexp.exe（类型库导出程序）
description: 查看 Tlbexp.exe（类型库导出程序）。 此工具生成一个类型库，其中描述在一个公共语言运行时 (CLR) 程序集中定义的类型。
ms.date: 03/30/2017
helpviewer_keywords:
- exporting type library [.NET Framework]
- exporter tool [.NET Framework]
- Tlbexp.exe
- Type Library Exporter
- type libraries [.NET Framework], exporting
ms.assetid: a487d61b-d166-467b-a7ca-d8b52fbff42d
ms.openlocfilehash: 49b776781a7a61db7e90b0c1f0abec6d63f50ba0
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494644"
---
# <a name="tlbexpexe-type-library-exporter"></a>Tlbexp.exe（类型库导出程序）

类型库导出程序生成一个类型库，该类型库描述公共语言运行时程序集中定义的类型。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
tlbexp assemblyName [options]  
```  
  
## <a name="parameters"></a>参数  
  
|参数|描述|  
|--------------|-----------------|  
|*assemblyName*|为其导出类型库的程序集。|  
  
|选项|描述|  
|------------|-----------------|  
|/asmpath: directory|指定要在其中搜索程序集的位置。 如果使用此选项，则必须显式指定要在其中搜索所引用的程序集的位置（包括当前目录）。<br /><br /> 当使用 asmpath 选项时，类型库导出程序不会在全局程序集缓存 (GAC) 中查找程序集。|  
|**/help**|显示该工具的命令语法和选项。|  
|/names: filename|指定类型库中名称的大小写。 filename 参数是一个文本文件。 文件中的每一行均指定类型库中一个名称的大小写。|  
|**/nologo**|取消显示 Microsoft 启动版权标志。|  
|/oldnames|强制 Tlbexp.exe 导出修饰类型名（如果存在类型名冲突）。 请注意，这是 .NET Framework 2.0 版之前的版本中的默认行为。|  
|/out: file|指定要生成的类型库文件的名称。 如果省略该选项，则 Tlbexp.exe 将生成一个与程序集的名称（实际的程序集名称，不一定与包含程序集的文件同名）相同且具有 .tlb 扩展名的类型库。|  
|/silence: `warningnumber`|禁止显示指定的警告。 此选项不能与 **/silent** 一起使用。|  
|**/Silent**|取消显示成功消息。 此选项不能与 **/silence** 一起使用。|  
|/tlbreference: typelibraryname|强制 Tlbexp.exe 在不参考注册表的情况下显式解析类型库引用。 例如，如果程序集 B 引用程序集 A，则可使用此选项来提供显式类型库引用而不依赖于注册表中指定的类型库。 Tlbexp.exe 将执行版本检查以确保类型库版本与程序集版本相匹配；否则将生成错误。<br /><br /> 请注意，在将 <xref:System.Runtime.InteropServices.ComImportAttribute> 特性应用于一个接口，然后该接口由另一类型实现的情况下，tlbreference 选项仍咨询注册表。|  
|/tlbrefpath: path|所引用的类型库的完全限定路径。|  
|/win32|在 64 位计算机上编译时，此选项指定 Tlbexp.exe 生成一个 32 位类型库。|  
|/win64|在 32 位计算机上编译时，此选项指定 Tlbexp.exe 生成一个 64 位类型库。|  
|**/verbose**|指定详细模式；显示需要为其生成类型库的任何引用程序集的列表。|  
|**/?**|显示该工具的命令语法和选项。|  
  
> [!NOTE]
> Tlbexp.exe 的命令行选项不区分大小写，并可以按任何顺序提供。 只需指定足够的选项来唯一标识它。 例如，/n 等效于 /nologo，/o: outfile.tlb 等效于 /out: outfile.tlb  。  
  
## <a name="remarks"></a>备注  

 Tlbexp.exe 生成一个类型库，该类型库包含程序集中定义的类型的定义。 应用程序（如 Visual Basic 6.0）可以使用生成的类型库来绑定到程序集中定义的 .NET 类型。  
  
> [!IMPORTANT]
> 不能使用 Tlbexp.exe 导出 Windows 元数据 (.winmd) 文件。 不支持导出 Windows 运行时程序集。  
  
 立即转换整个程序集。 不能使用 Tlbexp.exe 生成程序集中定义的类型子集的类型信息。  
  
 不能使用 Tlbexp.exe 从使用[类型库导入程序 (Tlbimp.exe)](tlbimp-exe-type-library-importer.md) 导入的程序集生成类型库。 相反，应参考用 Tlbimp.exe 导入的原始类型库。 你可以从引用程序集（使用 Tlbimp.exe 导入）的程序集导出类型库。 请参见下面的示例部分。  
  
 Tlbexp.exe 将生成的类型库置于当前工作目录中或为输出文件指定的目录中。 一个程序集可能会导致生成多个类型库。  
  
 Tlbexp.exe 生成类型库，但不注册它。 这与[程序集注册工具 (Regasm.exe)](regasm-exe-assembly-registration-tool.md) 不同，后者生成并注册类型库。 若要使用 COM 生成和注册类型库，请使用 Regasm.exe。  
  
 如果未指定 `/win32` 或 `/win64` 选项，则 Tlbexp.exe 将生成一个 32 位或 64 位类型库，该类型库与执行编译所用的计算机的类型（32 位或 64 位计算机）相对应。 为了交叉编译，可以在 32 位计算机上使用 `/win64` 选项来生成 64 位类型库，也可以在 64 位计算机上使用 `/win32` 选项来生成 32 位类型库。 在 32 位类型库中，将 <xref:System.Runtime.InteropServices.ComTypes.SYSKIND> 值设置为 <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN32>。 在 64 位类型库中，将 <xref:System.Runtime.InteropServices.ComTypes.SYSKIND> 值设置为 <xref:System.Runtime.InteropServices.ComTypes.SYSKIND.SYS_WIN64>。 相应地转换所有数据类型转换（例如，指针大小的数据类型，如 `IntPtr` 和 `UIntPtr`）。  
  
 如果使用 <xref:System.Runtime.InteropServices.MarshalAsAttribute> 特性指定 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArraySubType> 或 `VT_UNKOWN` 的 `VT_DISPATCH` 值，则 Tlbexp.exe 将忽略随后使用的任何 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType> 字段。 例如，对于以下签名：  
  
```csharp
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_UNKNOWN, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructUnkSafe(){return null;}  
[return:MarshalAs(UnmanagedType.SafeArray, SafeArraySubType=VarEnum.VT_DISPATCH, SafeArrayUserDefinedSubType=typeof(ConsoleKeyInfo))] public Array StructDispSafe(){return null;}  
```  
  
 生成以下类型库：  
  
```cpp
[id(0x60020004)]  
HRESULT StructUnkSafe([out, retval] SAFEARRAY(IUnknown*)* pRetVal);  
[id(0x60020005)]  
HRESULT StructDispSafe([out, retval] SAFEARRAY(IDispatch*)* pRetVal);  
```  
  
 请注意，Tlbexp.exe 忽略 <xref:System.Runtime.InteropServices.MarshalAsAttribute.SafeArrayUserDefinedSubType> 字段。  
  
 由于类型库无法容纳在程序集中找到的所有信息，因此在导出过程中，Tlbexp.exe 可能会放弃一些数据。 有关对转换过程的说明和发出到类型库中的每条信息的源的标识，请参见[有关从程序集转换到类型库的摘要](/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))。  
  
 请注意，类型库导出程序导出具有 <xref:System.TypedReference> 类型的 `VARIANT` 参数的方法，尽管该 <xref:System.TypedReference> 对象在非托管代码中没有意义。 在导出具有 <xref:System.TypedReference> 参数的方法时，类型库导出程序不会生成警告或错误，但使用结果类型库的非托管代码将无法正常运行。
  
## <a name="examples"></a>示例  

 下面的命令生成一个与 `myTest.dll` 中找到的程序集同名的类型库。  
  
```console  
tlbexp myTest.dll  
```  
  
 下面的命令生成一个名为 `clipper.tlb` 的类型库。  
  
```console  
tlbexp myTest.dll /out:clipper.tlb  
```  
  
 下面的示例阐释使用 Tlbexp.exe 从一个程序集导出类型库，该类型库引用使用 Tlbimp.exe 导入的程序集。  
  
 首先使用 Tlbimp.exe 导入类型库 `myLib.tlb`，然后将其另存为 `myLib.dll`。  
  
```console  
tlbimp myLib.tlb /out:myLib.dll  
```  
  
 下面的命令使用 C# 编译器编译 `Sample.dll,`，后者引用前面的示例中创建的 `myLib.dll`。  
  
```console  
CSC Sample.cs /reference:myLib.dll /out:Sample.dll  
```  
  
 下面的命令为引用 `Sample.dll` 的 `myLib.dll` 生成类型库。  
  
```console  
tlbexp Sample.dll  
```  
  
## <a name="see-also"></a>请参阅

- <xref:System.Runtime.InteropServices.TypeLibExporterFlags>
- [工具](index.md)
- [Regasm.exe（程序集注册工具）](regasm-exe-assembly-registration-tool.md)
- [有关从程序集转换到类型库的摘要](/previous-versions/dotnet/netframework-4.0/xk1120c3(v=vs.100))
- [Tlbimp.exe（类型库导入程序）](tlbimp-exe-type-library-importer.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
