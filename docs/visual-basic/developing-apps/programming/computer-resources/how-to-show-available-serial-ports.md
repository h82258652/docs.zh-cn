---
description: 详细了解：操作说明：在 Visual Basic 中显示可用的串行端口
title: 如何：显示可用的串行端口
ms.date: 07/20/2015
helpviewer_keywords:
- serial ports, availability
- My.Computer.Ports.SerialPortNames property
- My.Computer.Ports object
- ports, serial port availability
ms.assetid: eaf2ee5a-8103-4e10-a205-ed1d4db120ba
ms.openlocfilehash: 7fdbd5577ca40d1a900bc9442cb4bfeedae82c64
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494566"
---
# <a name="how-to-show-available-serial-ports-in-visual-basic"></a>如何：在 Visual Basic 中显示可用的串行端口

本主题介绍在 Visual Basic 中如何使用 `My.Computer.Ports` 显示计算机的可用串行端口。  
  
 若要使用户可以选择要使用的端口，请将串行端口的名称置于 <xref:System.Windows.Forms.ListBox> 控件中。  
  
## <a name="example"></a>示例  

 此示例循环访问 `My.Computer.Ports.SerialPortNames` 属性返回的所有字符串。 这些字符串是计算机上的可用串行端口的名称。  
  
 通常，用户从可用端口列表中选择应用程序应使用的串行端口。 在此示例中，串行端口名称存储在 <xref:System.Windows.Forms.ListBox> 控件中。 有关详细信息，请参阅 [ListBox 控件](/dotnet/desktop/winforms/controls/listbox-control-windows-forms)。  
  
 [!code-vb[VbVbalrMyComputer#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#45)]  
  
 此代码示例也可作为 IntelliSense 代码片段。 它位于代码片段选取器的“连接和网络”中。 有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。  
  
## <a name="compiling-the-code"></a>编译代码  

 此示例需要：  
  
- 对 System.Windows.Forms.dll 的项目引用。  
  
- 对 <xref:System.Windows.Forms> 命名空间成员的访问权限。 如果未在代码中完全限定成员名称，则添加 `Imports` 语句。 有关详细信息，请参阅 [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)。  
  
- 窗体具有名为 `ListBox1` 的 <xref:System.Windows.Forms.ListBox> 控件。  
  
## <a name="robust-programming"></a>可靠编程  

 不必使用 <xref:System.Windows.Forms.ListBox> 控件显示可用串行端口名称。 相反，可以使用 <xref:System.Windows.Forms.ComboBox> 或其他控件。 如果应用程序不需要来自用户的响应，则可以使用 <xref:System.Windows.Forms.TextBox> 控件显示信息。
  
## <a name="see-also"></a>另请参阅

- <xref:Microsoft.VisualBasic.Devices.Ports>
- [如何：使用连接到串行端口的调制解调器拨号](how-to-dial-modems-attached-to-serial-ports.md)
- [如何：将字符串发送到串行端口](how-to-send-strings-to-serial-ports.md)
- [如何：从串行端口接收字符串](how-to-receive-strings-from-serial-ports.md)
