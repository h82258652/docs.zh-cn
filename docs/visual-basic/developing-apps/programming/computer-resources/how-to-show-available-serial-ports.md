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
# <a name="how-to-show-available-serial-ports-in-visual-basic"></a><span data-ttu-id="90008-103">如何：在 Visual Basic 中显示可用的串行端口</span><span class="sxs-lookup"><span data-stu-id="90008-103">How to: Show Available Serial Ports in Visual Basic</span></span>

<span data-ttu-id="90008-104">本主题介绍在 Visual Basic 中如何使用 `My.Computer.Ports` 显示计算机的可用串行端口。</span><span class="sxs-lookup"><span data-stu-id="90008-104">This topic describes how to use `My.Computer.Ports` to show the available serial ports of the computer in Visual Basic.</span></span>  
  
 <span data-ttu-id="90008-105">若要使用户可以选择要使用的端口，请将串行端口的名称置于 <xref:System.Windows.Forms.ListBox> 控件中。</span><span class="sxs-lookup"><span data-stu-id="90008-105">To allow a user to select which port to use, the names of the serial ports are placed in a <xref:System.Windows.Forms.ListBox> control.</span></span>  
  
## <a name="example"></a><span data-ttu-id="90008-106">示例</span><span class="sxs-lookup"><span data-stu-id="90008-106">Example</span></span>  

 <span data-ttu-id="90008-107">此示例循环访问 `My.Computer.Ports.SerialPortNames` 属性返回的所有字符串。</span><span class="sxs-lookup"><span data-stu-id="90008-107">This example loops over all the strings that the `My.Computer.Ports.SerialPortNames` property returns.</span></span> <span data-ttu-id="90008-108">这些字符串是计算机上的可用串行端口的名称。</span><span class="sxs-lookup"><span data-stu-id="90008-108">These strings are the names of the available serial ports on the computer.</span></span>  
  
 <span data-ttu-id="90008-109">通常，用户从可用端口列表中选择应用程序应使用的串行端口。</span><span class="sxs-lookup"><span data-stu-id="90008-109">Typically, a user selects which serial port the application should use from the list of available ports.</span></span> <span data-ttu-id="90008-110">在此示例中，串行端口名称存储在 <xref:System.Windows.Forms.ListBox> 控件中。</span><span class="sxs-lookup"><span data-stu-id="90008-110">In this example, the serial port names are stored in a <xref:System.Windows.Forms.ListBox> control.</span></span> <span data-ttu-id="90008-111">有关详细信息，请参阅 [ListBox 控件](/dotnet/desktop/winforms/controls/listbox-control-windows-forms)。</span><span class="sxs-lookup"><span data-stu-id="90008-111">For more information, see [ListBox Control](/dotnet/desktop/winforms/controls/listbox-control-windows-forms).</span></span>  
  
 [!code-vb[VbVbalrMyComputer#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyComputer/VB/Class2.vb#45)]  
  
 <span data-ttu-id="90008-112">此代码示例也可作为 IntelliSense 代码片段。</span><span class="sxs-lookup"><span data-stu-id="90008-112">This code example is also available as an IntelliSense code snippet.</span></span> <span data-ttu-id="90008-113">它位于代码片段选取器的“连接和网络”中。</span><span class="sxs-lookup"><span data-stu-id="90008-113">In the code snippet picker, it is located in **Connectivity and Networking**.</span></span> <span data-ttu-id="90008-114">有关详细信息，请参阅[代码片段](/visualstudio/ide/code-snippets)。</span><span class="sxs-lookup"><span data-stu-id="90008-114">For more information, see [Code Snippets](/visualstudio/ide/code-snippets).</span></span>  
  
## <a name="compiling-the-code"></a><span data-ttu-id="90008-115">编译代码</span><span class="sxs-lookup"><span data-stu-id="90008-115">Compiling the Code</span></span>  

 <span data-ttu-id="90008-116">此示例需要：</span><span class="sxs-lookup"><span data-stu-id="90008-116">This example requires:</span></span>  
  
- <span data-ttu-id="90008-117">对 System.Windows.Forms.dll 的项目引用。</span><span class="sxs-lookup"><span data-stu-id="90008-117">A project reference to System.Windows.Forms.dll.</span></span>  
  
- <span data-ttu-id="90008-118">对 <xref:System.Windows.Forms> 命名空间成员的访问权限。</span><span class="sxs-lookup"><span data-stu-id="90008-118">Access to the members of the <xref:System.Windows.Forms> namespace.</span></span> <span data-ttu-id="90008-119">如果未在代码中完全限定成员名称，则添加 `Imports` 语句。</span><span class="sxs-lookup"><span data-stu-id="90008-119">Add an `Imports` statement if you are not fully qualifying member names in your code.</span></span> <span data-ttu-id="90008-120">有关详细信息，请参阅 [Imports 语句（.NET 命名空间和类型）](../../../language-reference/statements/imports-statement-net-namespace-and-type.md)。</span><span class="sxs-lookup"><span data-stu-id="90008-120">For more information, see [Imports Statement (.NET Namespace and Type)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md).</span></span>  
  
- <span data-ttu-id="90008-121">窗体具有名为 `ListBox1` 的 <xref:System.Windows.Forms.ListBox> 控件。</span><span class="sxs-lookup"><span data-stu-id="90008-121">That your form have a <xref:System.Windows.Forms.ListBox> control named `ListBox1`.</span></span>  
  
## <a name="robust-programming"></a><span data-ttu-id="90008-122">可靠编程</span><span class="sxs-lookup"><span data-stu-id="90008-122">Robust Programming</span></span>  

 <span data-ttu-id="90008-123">不必使用 <xref:System.Windows.Forms.ListBox> 控件显示可用串行端口名称。</span><span class="sxs-lookup"><span data-stu-id="90008-123">You do not have to use the <xref:System.Windows.Forms.ListBox> control to display the available serial port names.</span></span> <span data-ttu-id="90008-124">相反，可以使用 <xref:System.Windows.Forms.ComboBox> 或其他控件。</span><span class="sxs-lookup"><span data-stu-id="90008-124">Instead, you can use a <xref:System.Windows.Forms.ComboBox> or other control.</span></span> <span data-ttu-id="90008-125">如果应用程序不需要来自用户的响应，则可以使用 <xref:System.Windows.Forms.TextBox> 控件显示信息。</span><span class="sxs-lookup"><span data-stu-id="90008-125">If the application does not need a response from the user, you can use a <xref:System.Windows.Forms.TextBox> control to display the information.</span></span>
  
## <a name="see-also"></a><span data-ttu-id="90008-126">另请参阅</span><span class="sxs-lookup"><span data-stu-id="90008-126">See also</span></span>

- <xref:Microsoft.VisualBasic.Devices.Ports>
- [<span data-ttu-id="90008-127">如何：使用连接到串行端口的调制解调器拨号</span><span class="sxs-lookup"><span data-stu-id="90008-127">How to: Dial Modems Attached to Serial Ports</span></span>](how-to-dial-modems-attached-to-serial-ports.md)
- [<span data-ttu-id="90008-128">如何：将字符串发送到串行端口</span><span class="sxs-lookup"><span data-stu-id="90008-128">How to: Send Strings to Serial Ports</span></span>](how-to-send-strings-to-serial-ports.md)
- [<span data-ttu-id="90008-129">如何：从串行端口接收字符串</span><span class="sxs-lookup"><span data-stu-id="90008-129">How to: Receive Strings From Serial Ports</span></span>](how-to-receive-strings-from-serial-ports.md)
