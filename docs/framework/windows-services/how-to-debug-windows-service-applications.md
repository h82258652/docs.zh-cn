---
title: 如何：调试 Windows 服务应用程序
description: 了解如何调试 Windows 服务应用程序，这种调试并不像调试其他 Visual Studio 应用程序类型那样简单。
ms.date: 03/30/2017
helpviewer_keywords:
- debugging Windows Service applications
- debugging [Visual Studio], Windows services
- Windows services, debugging
- Windows Service applications, debugging
- services, debugging
ms.assetid: 63ab0800-0f05-4f1e-88e6-94c73fd920a2
ms.openlocfilehash: 29ab09e0de024ed4256dc5be6bd8e54ff28b101b
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494527"
---
# <a name="how-to-debug-windows-service-applications"></a>如何：调试 Windows 服务应用程序

必须从服务控制管理器的上下文中而不是 Visual Studio 中运行服务。 因此，调试服务不像调试其他 Visual Studio 应用程序类型一样简单。 要调试服务，必须启动该服务，然后将调试器附加到该服务正在其中运行的进程中。 然后你可以使用所有 Visual Studio 的标准调试功能来调试你的应用程序。  
  
> [!CAUTION]
> 除非你知道该进程是什么进程并了解进行附加和可能终止该进程的后果，否则你不应该附加到进程。 例如，如果你附加到 WinLogon 进程然后停止调试，系统将停止，因为系统没有 WinLogon 无法运行。  
  
 你仅可将调试器附加到正在运行的服务。 附加过程会中断服务的当前运行；它实际不会停止或暂停服务的处理。 也就是说，如果当你开始调试时你的服务正在运行，在你调试它时它在技术上仍处于启动状态，但是它的处理已经挂起。  
  
 附加到进程后，你可以设置断点并使用它们来调试你的代码。 退出你用于附加到进程的对话框后，你实际上处于调试模式。 你可以使用服务控制管理器来启动、停止、暂停和继续你的服务，从而命中你设置的断点。 调试成功后，你可稍后删除该虚拟服务。  
  
 本文介绍了调试在本地计算机上运行的服务，但你也可以调试在远程计算机上运行的 Windows 服务。 请参阅[远程调试](/visualstudio/debugger/debug-installed-app-package)。  
  
> [!NOTE]
> 调试 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法会比较困难，因为服务控制管理器对所有启动服务的尝试施加了 30 秒的限制。 有关详细信息，请参阅[疑难解答：调试 Windows 服务](troubleshooting-debugging-windows-services.md)。  
  
> [!WARNING]
> 若要获取有意义的调试信息，Visual Studio 调试器需要找到正在调试的二进制文件的符号文件。 如果你正在调试你在 Visual Studio 中生成的服务，符号文件（.pdb 文件）就在与可执行文件或库相同的文件夹中，调试器会自动加载它们。 如果你正在调试你没有生成过的服务，你应该首先查找该服务的符号文件，确保调试器可找到它们。 请参阅[在 Visual Studio 调试器中指定符号 (.pdb) 和源文件](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger)。 如果你正在调试系统进程，或想要你的服务中的系统调用符号，你应该添加 Microsoft 符号服务器。 请参阅[调试符号](/windows/desktop/DxTechArts/debugging-with-symbols)。  
  
### <a name="to-debug-a-service"></a>调试服务  
  
1. 在调试配置中生成你的服务。  
  
2. 安装你的服务。 有关详细信息，请参阅[如何：安装和卸载服务](how-to-install-and-uninstall-services.md)。  
  
3. 从   “服务控制管理器”、“服务器资源管理器”或代码启动服务。 有关详细信息，请参阅[如何：启动服务](how-to-start-services.md)。  
  
4. 使用管理凭据启动 Visual Studio,那么你就能附加到系统进程。  
  
5. （可选）在 Visual Studio 菜单栏上，选择“工具”、“选项”   。 在“选项”对话框中，选择“调试”、“符号”，选择“Microsoft 符号服务器”复选框，然后选择“确定”按钮      。  
  
6. 在菜单栏上，从“调试”  或“工具”  菜单选择“附加到进程”  。 （键盘：Ctrl+Alt+P）  
  
     这将显示“进程”  对话框。  
  
7. 选择“显示所有用户的进程”复选框  。  
  
8. 在“可用进程”  部分,为服务选择进程，然后选择“附加”  。  
  
    > [!TIP]
    > 该进程的名称将与你的服务的可执行文件相同。  
  
     出现 **“附加到进程”** 对话框。  
  
9. 选择相应的选项，然后选择“确定”  以关闭对话框。  
  
    > [!NOTE]
    > 你现在处于调试模式。  
  
10. 设置任意你想要在代码中使用的断点。  
  
11. 访问服务控制管理器并操作你的服务，发送停止、暂停和继续命令以命中你的断点。 有关运行服务控制管理器的详细信息，请参阅[如何：启动服务](how-to-start-services.md)。 请参见[疑难解答：调试 Windows 服务](troubleshooting-debugging-windows-services.md)。  
  
## <a name="debugging-tips-for-windows-services"></a>Windows 服务的调试提示  

 附加到服务的进程使你可调试该服务的大多数代码，但不是全部。 例如，由于服务已经启动，你无法调试服务的 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法中的代码或 `Main` 方法中用于在此处加载服务的代码。 避免这种限制的一种方法是在你的仅用于辅助调试的服务应用程序中创建一个临时辅助服务。 你可以安装两个服务，然后启动该虚拟服务来加载服务进程。 临时服务启动进程后，可以使用 Visual Studio 中的  “调试”菜单来附加到服务进程。  
  
 尝试添加对 <xref:System.Threading.Thread.Sleep%2A> 方法的调用延迟操作，直到你能够附加到该进程。  
  
 尝试更改常规控制台应用程序的程序。 为此，请按如下所示重写 `Main` 方法，这样它可以作为 Windows 服务和控制台应用程序运行，具体取决于启动方式。  
  
#### <a name="how-to-run-a-windows-service-as-a-console-application"></a>如何：将 Windows 服务作为控制台应用运行  
  
1. 向你运行 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 方法的服务添加一个方法：  
  
    ```csharp  
    internal void TestStartupAndStop(string[] args)  
    {  
        this.OnStart(args);  
        Console.ReadLine();  
        this.OnStop();  
    }  
    ```  
  
2. 按如下所示重写 `Main` 方法：  
  
    ```csharp  
    static void Main(string[] args)  
    {  
        if (Environment.UserInteractive)  
        {  
            MyNewService service1 = new MyNewService(args);  
            service1.TestStartupAndStop(args);  
        }  
        else  
        {  
            // Put the body of your old Main method here.  
        }  
    }
    ```  
  
3. 在项目属性的“应用程序”  选项卡中，将“输出类型”  设置为“控制台应用程序”  。  
  
4. 选择“启动调试”  (F5)。  
  
5. 若要将该程序再次作为 Windows 服务运行，请安装它并像通常启动 Windows 服务一样启动它。 不必恢复这些更改。  
  
 在某些情况下，你必须使用 Windows 调试器，比如当你想要调试仅在系统启动时发生的问题时。 [下载 Windows 驱动程序工具包 (WDK)](/windows-hardware/drivers/download-the-wdk)，并查看[如何调试 Windows 服务](https://support.microsoft.com/kb/824344)。  
  
## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：安装和卸载服务](how-to-install-and-uninstall-services.md)
- [如何：启动服务](how-to-start-services.md)
- [调试服务](/windows/desktop/Services/debugging-a-service)
