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
# <a name="how-to-debug-windows-service-applications"></a><span data-ttu-id="f8e03-103">如何：调试 Windows 服务应用程序</span><span class="sxs-lookup"><span data-stu-id="f8e03-103">How to: Debug Windows Service Applications</span></span>

<span data-ttu-id="f8e03-104">必须从服务控制管理器的上下文中而不是 Visual Studio 中运行服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-104">A service must be run from within the context of the Services Control Manager rather than from within Visual Studio.</span></span> <span data-ttu-id="f8e03-105">因此，调试服务不像调试其他 Visual Studio 应用程序类型一样简单。</span><span class="sxs-lookup"><span data-stu-id="f8e03-105">For this reason, debugging a service is not as straightforward as debugging other Visual Studio application types.</span></span> <span data-ttu-id="f8e03-106">要调试服务，必须启动该服务，然后将调试器附加到该服务正在其中运行的进程中。</span><span class="sxs-lookup"><span data-stu-id="f8e03-106">To debug a service, you must start the service and then attach a debugger to the process in which it is running.</span></span> <span data-ttu-id="f8e03-107">然后你可以使用所有 Visual Studio 的标准调试功能来调试你的应用程序。</span><span class="sxs-lookup"><span data-stu-id="f8e03-107">You can then debug your application by using all of the standard debugging functionality of Visual Studio.</span></span>  
  
> [!CAUTION]
> <span data-ttu-id="f8e03-108">除非你知道该进程是什么进程并了解进行附加和可能终止该进程的后果，否则你不应该附加到进程。</span><span class="sxs-lookup"><span data-stu-id="f8e03-108">You should not attach to a process unless you know what the process is and understand the consequences of attaching to and possibly killing that process.</span></span> <span data-ttu-id="f8e03-109">例如，如果你附加到 WinLogon 进程然后停止调试，系统将停止，因为系统没有 WinLogon 无法运行。</span><span class="sxs-lookup"><span data-stu-id="f8e03-109">For example, if you attach to the WinLogon process and then stop debugging, the system will halt because it can’t operate without WinLogon.</span></span>  
  
 <span data-ttu-id="f8e03-110">你仅可将调试器附加到正在运行的服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-110">You can attach the debugger only to a running service.</span></span> <span data-ttu-id="f8e03-111">附加过程会中断服务的当前运行；它实际不会停止或暂停服务的处理。</span><span class="sxs-lookup"><span data-stu-id="f8e03-111">The attachment process interrupts the current functioning of your service; it doesn’t actually stop or pause the service's processing.</span></span> <span data-ttu-id="f8e03-112">也就是说，如果当你开始调试时你的服务正在运行，在你调试它时它在技术上仍处于启动状态，但是它的处理已经挂起。</span><span class="sxs-lookup"><span data-stu-id="f8e03-112">That is, if your service is running when you begin debugging, it is still technically in the Started state as you debug it, but its processing has been suspended.</span></span>  
  
 <span data-ttu-id="f8e03-113">附加到进程后，你可以设置断点并使用它们来调试你的代码。</span><span class="sxs-lookup"><span data-stu-id="f8e03-113">After attaching to the process, you can set breakpoints and use these to debug your code.</span></span> <span data-ttu-id="f8e03-114">退出你用于附加到进程的对话框后，你实际上处于调试模式。</span><span class="sxs-lookup"><span data-stu-id="f8e03-114">Once you exit the dialog box you use to attach to the process, you are effectively in debug mode.</span></span> <span data-ttu-id="f8e03-115">你可以使用服务控制管理器来启动、停止、暂停和继续你的服务，从而命中你设置的断点。</span><span class="sxs-lookup"><span data-stu-id="f8e03-115">You can use the Services Control Manager to start, stop, pause and continue your service, thus hitting the breakpoints you've set.</span></span> <span data-ttu-id="f8e03-116">调试成功后，你可稍后删除该虚拟服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-116">You can later remove this dummy service after debugging is successful.</span></span>  
  
 <span data-ttu-id="f8e03-117">本文介绍了调试在本地计算机上运行的服务，但你也可以调试在远程计算机上运行的 Windows 服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-117">This article covers debugging a service that's running on the local computer, but you can also debug Windows Services that are running on a remote computer.</span></span> <span data-ttu-id="f8e03-118">请参阅[远程调试](/visualstudio/debugger/debug-installed-app-package)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-118">See [Remote Debugging](/visualstudio/debugger/debug-installed-app-package).</span></span>  
  
> [!NOTE]
> <span data-ttu-id="f8e03-119">调试 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法会比较困难，因为服务控制管理器对所有启动服务的尝试施加了 30 秒的限制。</span><span class="sxs-lookup"><span data-stu-id="f8e03-119">Debugging the <xref:System.ServiceProcess.ServiceBase.OnStart%2A> method can be difficult because the Services Control Manager imposes a 30-second limit on all attempts to start a service.</span></span> <span data-ttu-id="f8e03-120">有关详细信息，请参阅[疑难解答：调试 Windows 服务](troubleshooting-debugging-windows-services.md)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-120">For more information, see [Troubleshooting: Debugging Windows Services](troubleshooting-debugging-windows-services.md).</span></span>  
  
> [!WARNING]
> <span data-ttu-id="f8e03-121">若要获取有意义的调试信息，Visual Studio 调试器需要找到正在调试的二进制文件的符号文件。</span><span class="sxs-lookup"><span data-stu-id="f8e03-121">To get meaningful information for debugging, the Visual Studio debugger needs to find symbol files for the binaries that are being debugged.</span></span> <span data-ttu-id="f8e03-122">如果你正在调试你在 Visual Studio 中生成的服务，符号文件（.pdb 文件）就在与可执行文件或库相同的文件夹中，调试器会自动加载它们。</span><span class="sxs-lookup"><span data-stu-id="f8e03-122">If you are debugging a service that you built in Visual Studio, the symbol files (.pdb files) are in the same folder as the executable or library, and the debugger loads them automatically.</span></span> <span data-ttu-id="f8e03-123">如果你正在调试你没有生成过的服务，你应该首先查找该服务的符号文件，确保调试器可找到它们。</span><span class="sxs-lookup"><span data-stu-id="f8e03-123">If you are debugging a service that you didn't build, you should first find symbols for the service and make sure they can be found by the debugger.</span></span> <span data-ttu-id="f8e03-124">请参阅[在 Visual Studio 调试器中指定符号 (.pdb) 和源文件](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-124">See [Specify Symbol (.pdb) and Source Files in the Visual Studio Debugger](/visualstudio/debugger/specify-symbol-dot-pdb-and-source-files-in-the-visual-studio-debugger).</span></span> <span data-ttu-id="f8e03-125">如果你正在调试系统进程，或想要你的服务中的系统调用符号，你应该添加 Microsoft 符号服务器。</span><span class="sxs-lookup"><span data-stu-id="f8e03-125">If you're debugging a system process or want to have symbols for system calls in your services, you should add the Microsoft Symbol Servers.</span></span> <span data-ttu-id="f8e03-126">请参阅[调试符号](/windows/desktop/DxTechArts/debugging-with-symbols)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-126">See [Debugging Symbols](/windows/desktop/DxTechArts/debugging-with-symbols).</span></span>  
  
### <a name="to-debug-a-service"></a><span data-ttu-id="f8e03-127">调试服务</span><span class="sxs-lookup"><span data-stu-id="f8e03-127">To debug a service</span></span>  
  
1. <span data-ttu-id="f8e03-128">在调试配置中生成你的服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-128">Build your service in the Debug configuration.</span></span>  
  
2. <span data-ttu-id="f8e03-129">安装你的服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-129">Install your service.</span></span> <span data-ttu-id="f8e03-130">有关详细信息，请参阅[如何：安装和卸载服务](how-to-install-and-uninstall-services.md)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-130">For more information, see [How to: Install and Uninstall Services](how-to-install-and-uninstall-services.md).</span></span>  
  
3. <span data-ttu-id="f8e03-131">从   “服务控制管理器”、“服务器资源管理器”或代码启动服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-131">Start your service, either from **Services Control Manager**, **Server Explorer**, or from code.</span></span> <span data-ttu-id="f8e03-132">有关详细信息，请参阅[如何：启动服务](how-to-start-services.md)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-132">For more information, see [How to: Start Services](how-to-start-services.md).</span></span>  
  
4. <span data-ttu-id="f8e03-133">使用管理凭据启动 Visual Studio,那么你就能附加到系统进程。</span><span class="sxs-lookup"><span data-stu-id="f8e03-133">Start Visual Studio with administrative credentials so you can attach to system processes.</span></span>  
  
5. <span data-ttu-id="f8e03-134">（可选）在 Visual Studio 菜单栏上，选择“工具”、“选项”   。</span><span class="sxs-lookup"><span data-stu-id="f8e03-134">(Optional) On the Visual Studio menu bar, choose **Tools**, **Options**.</span></span> <span data-ttu-id="f8e03-135">在“选项”对话框中，选择“调试”、“符号”，选择“Microsoft 符号服务器”复选框，然后选择“确定”按钮      。</span><span class="sxs-lookup"><span data-stu-id="f8e03-135">In the **Options** dialog box, choose **Debugging**, **Symbols**, select the **Microsoft Symbol Servers** check box, and then choose the **OK** button.</span></span>  
  
6. <span data-ttu-id="f8e03-136">在菜单栏上，从“调试”  或“工具”  菜单选择“附加到进程”  。</span><span class="sxs-lookup"><span data-stu-id="f8e03-136">On the menu bar, choose **Attach to Process** from the **Debug** or **Tools** menu.</span></span> <span data-ttu-id="f8e03-137">（键盘：Ctrl+Alt+P）</span><span class="sxs-lookup"><span data-stu-id="f8e03-137">(Keyboard: Ctrl+Alt+P)</span></span>  
  
     <span data-ttu-id="f8e03-138">这将显示“进程”  对话框。</span><span class="sxs-lookup"><span data-stu-id="f8e03-138">The **Processes** dialog box appears.</span></span>  
  
7. <span data-ttu-id="f8e03-139">选择“显示所有用户的进程”复选框  。</span><span class="sxs-lookup"><span data-stu-id="f8e03-139">Select the **Show processes from all users** check box.</span></span>  
  
8. <span data-ttu-id="f8e03-140">在“可用进程”  部分,为服务选择进程，然后选择“附加”  。</span><span class="sxs-lookup"><span data-stu-id="f8e03-140">In the **Available Processes** section, choose the process for your service, and then choose **Attach**.</span></span>  
  
    > [!TIP]
    > <span data-ttu-id="f8e03-141">该进程的名称将与你的服务的可执行文件相同。</span><span class="sxs-lookup"><span data-stu-id="f8e03-141">The process will have the same name as the executable file for your service.</span></span>  
  
     <span data-ttu-id="f8e03-142">出现 **“附加到进程”** 对话框。</span><span class="sxs-lookup"><span data-stu-id="f8e03-142">The **Attach to Process** dialog box appears.</span></span>  
  
9. <span data-ttu-id="f8e03-143">选择相应的选项，然后选择“确定”  以关闭对话框。</span><span class="sxs-lookup"><span data-stu-id="f8e03-143">Choose the appropriate options, and then choose **OK** to close the dialog box.</span></span>  
  
    > [!NOTE]
    > <span data-ttu-id="f8e03-144">你现在处于调试模式。</span><span class="sxs-lookup"><span data-stu-id="f8e03-144">You are now in debug mode.</span></span>  
  
10. <span data-ttu-id="f8e03-145">设置任意你想要在代码中使用的断点。</span><span class="sxs-lookup"><span data-stu-id="f8e03-145">Set any breakpoints you want to use in your code.</span></span>  
  
11. <span data-ttu-id="f8e03-146">访问服务控制管理器并操作你的服务，发送停止、暂停和继续命令以命中你的断点。</span><span class="sxs-lookup"><span data-stu-id="f8e03-146">Access the Services Control Manager and manipulate your service, sending stop, pause, and continue commands to hit your breakpoints.</span></span> <span data-ttu-id="f8e03-147">有关运行服务控制管理器的详细信息，请参阅[如何：启动服务](how-to-start-services.md)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-147">For more information about running the Services Control Manager, see [How to: Start Services](how-to-start-services.md).</span></span> <span data-ttu-id="f8e03-148">请参见[疑难解答：调试 Windows 服务](troubleshooting-debugging-windows-services.md)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-148">Also, see [Troubleshooting: Debugging Windows Services](troubleshooting-debugging-windows-services.md).</span></span>  
  
## <a name="debugging-tips-for-windows-services"></a><span data-ttu-id="f8e03-149">Windows 服务的调试提示</span><span class="sxs-lookup"><span data-stu-id="f8e03-149">Debugging Tips for Windows Services</span></span>  

 <span data-ttu-id="f8e03-150">附加到服务的进程使你可调试该服务的大多数代码，但不是全部。</span><span class="sxs-lookup"><span data-stu-id="f8e03-150">Attaching to the service's process allows you to debug most, but not all, the code for that service.</span></span> <span data-ttu-id="f8e03-151">例如，由于服务已经启动，你无法调试服务的 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法中的代码或 `Main` 方法中用于在此处加载服务的代码。</span><span class="sxs-lookup"><span data-stu-id="f8e03-151">For example, because the service has already been started, you cannot debug the code in the service's <xref:System.ServiceProcess.ServiceBase.OnStart%2A> method or the code in the `Main` method that is used to load the service this way.</span></span> <span data-ttu-id="f8e03-152">避免这种限制的一种方法是在你的仅用于辅助调试的服务应用程序中创建一个临时辅助服务。</span><span class="sxs-lookup"><span data-stu-id="f8e03-152">One way to work around this limitation is to create a temporary second service in your service application that exists only to aid in debugging.</span></span> <span data-ttu-id="f8e03-153">你可以安装两个服务，然后启动该虚拟服务来加载服务进程。</span><span class="sxs-lookup"><span data-stu-id="f8e03-153">You can install both services, and then start this dummy service to load the service process.</span></span> <span data-ttu-id="f8e03-154">临时服务启动进程后，可以使用 Visual Studio 中的  “调试”菜单来附加到服务进程。</span><span class="sxs-lookup"><span data-stu-id="f8e03-154">Once the temporary service has started the process, you can use the **Debug** menu in Visual Studio to attach to the service process.</span></span>  
  
 <span data-ttu-id="f8e03-155">尝试添加对 <xref:System.Threading.Thread.Sleep%2A> 方法的调用延迟操作，直到你能够附加到该进程。</span><span class="sxs-lookup"><span data-stu-id="f8e03-155">Try adding calls to the <xref:System.Threading.Thread.Sleep%2A> method to delay action until you’re able to attach to the process.</span></span>  
  
 <span data-ttu-id="f8e03-156">尝试更改常规控制台应用程序的程序。</span><span class="sxs-lookup"><span data-stu-id="f8e03-156">Try changing the program to a regular console application.</span></span> <span data-ttu-id="f8e03-157">为此，请按如下所示重写 `Main` 方法，这样它可以作为 Windows 服务和控制台应用程序运行，具体取决于启动方式。</span><span class="sxs-lookup"><span data-stu-id="f8e03-157">To do this, rewrite the `Main` method as follows so it can run both as a Windows Service and as a console application, depending on how it's started.</span></span>  
  
#### <a name="how-to-run-a-windows-service-as-a-console-application"></a><span data-ttu-id="f8e03-158">如何：将 Windows 服务作为控制台应用运行</span><span class="sxs-lookup"><span data-stu-id="f8e03-158">How to: Run a Windows Service as a console application</span></span>  
  
1. <span data-ttu-id="f8e03-159">向你运行 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 和 <xref:System.ServiceProcess.ServiceBase.OnStop%2A> 方法的服务添加一个方法：</span><span class="sxs-lookup"><span data-stu-id="f8e03-159">Add a method to your service that runs the <xref:System.ServiceProcess.ServiceBase.OnStart%2A> and <xref:System.ServiceProcess.ServiceBase.OnStop%2A> methods:</span></span>  
  
    ```csharp  
    internal void TestStartupAndStop(string[] args)  
    {  
        this.OnStart(args);  
        Console.ReadLine();  
        this.OnStop();  
    }  
    ```  
  
2. <span data-ttu-id="f8e03-160">按如下所示重写 `Main` 方法：</span><span class="sxs-lookup"><span data-stu-id="f8e03-160">Rewrite the `Main` method as follows:</span></span>  
  
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
  
3. <span data-ttu-id="f8e03-161">在项目属性的“应用程序”  选项卡中，将“输出类型”  设置为“控制台应用程序”  。</span><span class="sxs-lookup"><span data-stu-id="f8e03-161">In the **Application** tab of the project's properties, set the **Output type** to **Console Application**.</span></span>  
  
4. <span data-ttu-id="f8e03-162">选择“启动调试”  (F5)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-162">Choose **Start Debugging** (F5).</span></span>  
  
5. <span data-ttu-id="f8e03-163">若要将该程序再次作为 Windows 服务运行，请安装它并像通常启动 Windows 服务一样启动它。</span><span class="sxs-lookup"><span data-stu-id="f8e03-163">To run the program as a Windows Service again, install it and start it as usual for a Windows Service.</span></span> <span data-ttu-id="f8e03-164">不必恢复这些更改。</span><span class="sxs-lookup"><span data-stu-id="f8e03-164">It's not necessary to reverse these changes.</span></span>  
  
 <span data-ttu-id="f8e03-165">在某些情况下，你必须使用 Windows 调试器，比如当你想要调试仅在系统启动时发生的问题时。</span><span class="sxs-lookup"><span data-stu-id="f8e03-165">In some cases, such as when you want to debug an issue that occurs only on system startup, you have to use the Windows debugger.</span></span> <span data-ttu-id="f8e03-166">[下载 Windows 驱动程序工具包 (WDK)](/windows-hardware/drivers/download-the-wdk)，并查看[如何调试 Windows 服务](https://support.microsoft.com/kb/824344)。</span><span class="sxs-lookup"><span data-stu-id="f8e03-166">[Download the Windows Driver Kit (WDK)](/windows-hardware/drivers/download-the-wdk) and see [How to debug Windows Services](https://support.microsoft.com/kb/824344).</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="f8e03-167">请参阅</span><span class="sxs-lookup"><span data-stu-id="f8e03-167">See also</span></span>

- [<span data-ttu-id="f8e03-168">Windows 服务应用程序介绍</span><span class="sxs-lookup"><span data-stu-id="f8e03-168">Introduction to Windows Service Applications</span></span>](introduction-to-windows-service-applications.md)
- [<span data-ttu-id="f8e03-169">如何：安装和卸载服务</span><span class="sxs-lookup"><span data-stu-id="f8e03-169">How to: Install and Uninstall Services</span></span>](how-to-install-and-uninstall-services.md)
- [<span data-ttu-id="f8e03-170">如何：启动服务</span><span class="sxs-lookup"><span data-stu-id="f8e03-170">How to: Start Services</span></span>](how-to-start-services.md)
- [<span data-ttu-id="f8e03-171">调试服务</span><span class="sxs-lookup"><span data-stu-id="f8e03-171">Debugging a Service</span></span>](/windows/desktop/Services/debugging-a-service)
