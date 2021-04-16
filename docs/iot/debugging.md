---
title: 在 Raspberry Pi 上调试 .NET 应用
description: 了解如何在 Raspberry Pi 和类似设备上调试 .NET 应用。
author: camsoper
ms.author: casoper
ms.date: 11/13/2020
ms.topic: how-to
ms.prod: dotnet
zone_pivot_groups: ide-set-one
ms.openlocfilehash: 58858384c49a296e0b33d663f3ef930caf9cace6
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "102258060"
---
# <a name="debug-net-apps-on-raspberry-pi"></a><span data-ttu-id="a83bf-103">在 Raspberry Pi 上调试 .NET 应用</span><span class="sxs-lookup"><span data-stu-id="a83bf-103">Debug .NET apps on Raspberry Pi</span></span>

<span data-ttu-id="a83bf-104">调试在基于 ARM 的 IoT 设备（如 Raspberry Pi）上运行的 .NET 应用带来了独特的挑战。</span><span class="sxs-lookup"><span data-stu-id="a83bf-104">Debugging .NET apps running on ARM-based IoT devices like Raspberry Pi presents a unique challenge.</span></span> <span data-ttu-id="a83bf-105">可以在 ARM 设备上开发 .NET 应用。</span><span class="sxs-lookup"><span data-stu-id="a83bf-105">It is possible to develop .NET apps on ARM devices.</span></span> <span data-ttu-id="a83bf-106">但是，在 Visual Studio 外调试 .NET 应用所需的 OmniSharp 与 ARM 设备不兼容。</span><span class="sxs-lookup"><span data-stu-id="a83bf-106">However, OmniSharp, which is required for debugging .NET apps outside of Visual Studio, is not compatible with ARM devices.</span></span> <span data-ttu-id="a83bf-107">因此，必须从兼容平台远程调试应用。</span><span class="sxs-lookup"><span data-stu-id="a83bf-107">As a result, apps must be debugged remotely from a compatible platform.</span></span>

::: zone pivot="vscode"

## <a name="debug-from-visual-studio-code-cross-platform"></a><span data-ttu-id="a83bf-108">从 Visual Studio Code（跨平台）调试</span><span class="sxs-lookup"><span data-stu-id="a83bf-108">Debug from Visual Studio Code (cross-platform)</span></span>

<span data-ttu-id="a83bf-109">从 Visual Studio Code 在 Raspberry Pi 上调试 .NET，需要在 Raspberry Pi 上以及项目的 launch.json 文件中执行配置步骤。</span><span class="sxs-lookup"><span data-stu-id="a83bf-109">Debugging .NET on Raspberry Pi from Visual Studio Code requires configuration steps on the Raspberry Pi and in the project's *launch.json* file.</span></span>

### <a name="enable-ssh-on-the-raspberry-pi"></a><span data-ttu-id="a83bf-110">在 Raspberry Pi 上启用 SSH</span><span class="sxs-lookup"><span data-stu-id="a83bf-110">Enable SSH on the Raspberry Pi</span></span>

<span data-ttu-id="a83bf-111">远程调试需要 SSH。</span><span class="sxs-lookup"><span data-stu-id="a83bf-111">SSH is required for remote debugging.</span></span> <span data-ttu-id="a83bf-112">若要启用 SSH，请[参阅 Raspberry Pi 文档中的“启用 SSH”](https://www.raspberrypi.org/documentation/remote-access/ssh/)。</span><span class="sxs-lookup"><span data-stu-id="a83bf-112">To enable SSH, [refer to *Enable SSH* in the Raspberry Pi documentation](https://www.raspberrypi.org/documentation/remote-access/ssh/).</span></span>

### <a name="install-the-visual-studio-remote-debugger-on-the-raspberry-pi"></a><span data-ttu-id="a83bf-113">在 Raspberry Pi 上安装 Visual Studio 远程调试器</span><span class="sxs-lookup"><span data-stu-id="a83bf-113">Install the Visual Studio Remote Debugger on the Raspberry Pi</span></span>

<span data-ttu-id="a83bf-114">在 Raspberry Pi 上的 Bash 控制台中（从本地或通过 SSH）完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a83bf-114">Within a Bash console on the Raspberry Pi (either locally or via SSH), complete the following steps:</span></span>

1. <span data-ttu-id="a83bf-115">执行以下命令以在 Raspberry Pi 上下载并安装 Visual Studio 远程调试器：</span><span class="sxs-lookup"><span data-stu-id="a83bf-115">Execute the following command to download and install the Visual Studio Remote Debugger on the Raspberry Pi:</span></span>

    ```bash
    curl -sSL https://aka.ms/getvsdbgsh | /bin/sh /dev/stdin -v latest -l ~/vsdbg
    ```

1. <span data-ttu-id="a83bf-116">调试器需要作为 `root` 运行。</span><span class="sxs-lookup"><span data-stu-id="a83bf-116">The debugger requires running as `root`.</span></span> <span data-ttu-id="a83bf-117">默认情况下，`root` 没有 Raspberry Pi 上的密码。</span><span class="sxs-lookup"><span data-stu-id="a83bf-117">By default, `root` has no password on Raspberry Pi.</span></span> <span data-ttu-id="a83bf-118">通过执行以下命令并按照提示设置 `root` 的密码。</span><span class="sxs-lookup"><span data-stu-id="a83bf-118">Set a password for `root` by executing the following command and following the prompts.</span></span>

    ```bash
    sudo passwd root
    ```

1. <span data-ttu-id="a83bf-119">Visual Studio Code 使用 SSH 协议进行远程调试。</span><span class="sxs-lookup"><span data-stu-id="a83bf-119">Visual Studio Code uses the SSH protocol to debug remotely.</span></span> <span data-ttu-id="a83bf-120">出于安全考虑，默认不允许 `root` 通过 SSH 登录。</span><span class="sxs-lookup"><span data-stu-id="a83bf-120">For security purposes, `root` is not permitted to log on via SSH by default.</span></span> <span data-ttu-id="a83bf-121">若要使 `root` 通过 SSH 登录，请完成以下步骤：</span><span class="sxs-lookup"><span data-stu-id="a83bf-121">To enable `root` to log on via SSH, complete the following steps:</span></span>

    1. <span data-ttu-id="a83bf-122">执行以下命令以在 [nano](https://www.nano-editor.org/docs.php) 中打开 /etc/ssh/sshd_config。</span><span class="sxs-lookup"><span data-stu-id="a83bf-122">Execute the following command to open */etc/ssh/sshd_config* in [nano](https://www.nano-editor.org/docs.php).</span></span>

        ```bash
        sudo nano /etc/ssh/sshd_config
        ```

    1. <span data-ttu-id="a83bf-123">按 <kbd>Ctrl+W</kbd> 并搜索 PermitRootLogin。</span><span class="sxs-lookup"><span data-stu-id="a83bf-123">Press <kbd>Ctrl+W</kbd> and search for **PermitRootLogin**.</span></span>
    1. <span data-ttu-id="a83bf-124">如果需要，取消 PermitRootLogin 行的评论。</span><span class="sxs-lookup"><span data-stu-id="a83bf-124">Uncomment the line with **PermitRootLogin** if needed.</span></span>
    1. <span data-ttu-id="a83bf-125">按如下所示更改行：</span><span class="sxs-lookup"><span data-stu-id="a83bf-125">Change the line to read as follows:</span></span>

        ```console
        PermitRootLogin yes
        ```

        > [!NOTE]
        > <span data-ttu-id="a83bf-126">如果在 /etc/ssh/sshd_config 中没有 PermitRootLogin 行，请使用如上所示的值添加新行。</span><span class="sxs-lookup"><span data-stu-id="a83bf-126">If there is no **PermitRootLogin** line in */etc/ssh/sshd_config*, add a new line with the value shown above.</span></span>

    1. <span data-ttu-id="a83bf-127">按 <kbd>Ctrl+X</kbd> 退出并保存更改。</span><span class="sxs-lookup"><span data-stu-id="a83bf-127">Press <kbd>Ctrl+X</kbd> to exit and save your chances.</span></span> <span data-ttu-id="a83bf-128">当系统提示“是否保存已修改的缓冲区?”时，请按 <kbd>Y</kbd> 确认。</span><span class="sxs-lookup"><span data-stu-id="a83bf-128">When prompted **Save modified buffer?**, press <kbd>Y</kbd> to confirm.</span></span> <span data-ttu-id="a83bf-129">按 <kbd>Enter</kbd> 确认覆盖原始文件。</span><span class="sxs-lookup"><span data-stu-id="a83bf-129">Press <kbd>Enter</kbd> to confirm overwriting the original file.</span></span>
    1. <span data-ttu-id="a83bf-130">通过执行以下命令重新启动 Raspberry Pi：</span><span class="sxs-lookup"><span data-stu-id="a83bf-130">Reboot the Raspberry Pi by executing the following command:</span></span>

        ```bash
        sudo reboot
        ```

### <a name="setup-launchjson-in-visual-studio-code"></a><span data-ttu-id="a83bf-131">在 Visual Studio Code 中安装 launch.json 文件</span><span class="sxs-lookup"><span data-stu-id="a83bf-131">Setup launch.json in Visual Studio Code</span></span>

<span data-ttu-id="a83bf-132">将启动配置添加到项目的 launch.js 文件中。</span><span class="sxs-lookup"><span data-stu-id="a83bf-132">Add a launch configuration to the project's *launch.json*.</span></span> <span data-ttu-id="a83bf-133">如果项目没有 launch.json 文件，请添加一个文件，方法是切换到“运行”选项卡，选择“创建 launch.json 文件”，然后在对话框中选择“.NET”或“.NET Core”   。</span><span class="sxs-lookup"><span data-stu-id="a83bf-133">If the project doesn't have a *launch.json* file, add one by switching to the **Run** tab, selecting **create a launch.json file**, and selecting **.NET** or **.NET Core** in the dialog.</span></span>

<span data-ttu-id="a83bf-134">launch.json 中的新配置应如下所示：</span><span class="sxs-lookup"><span data-stu-id="a83bf-134">The new configuration in *launch.json* should look similar to this:</span></span>

```json
"configurations": [
    {
        "name": ".NET Core Launch (remote console)",
        "type": "coreclr",
        "request": "launch",
        "preLaunchTask": "build",
        "program": "/home/pi/.dotnet/dotnet",
        "args": ["/home/pi/sample/Sample.dll"],
        "cwd": "/home/pi/sample",
        "stopAtEntry": false,
        "console": "internalConsole",
        "pipeTransport": {
            "pipeCwd": "${workspaceFolder}",
            "pipeProgram": "C:\\Program Files\\PuTTY\\PLINK.EXE",
            "pipeArgs": [
                "-pw",
                "P@ssw0rd",
                "root@raspberrypi"
            ],
            "debuggerPath": "/home/pi/vsdbg/vsdbg"
        }
    },
```

<span data-ttu-id="a83bf-135">请注意以下内容：</span><span class="sxs-lookup"><span data-stu-id="a83bf-135">Notice the following:</span></span>

- <span data-ttu-id="a83bf-136">`program` 是 Pi 上的 .NET 运行时的路径。</span><span class="sxs-lookup"><span data-stu-id="a83bf-136">`program` is the path to the .NET runtime on the Pi.</span></span>
- <span data-ttu-id="a83bf-137">`args` 是要在 Pi 上调试的程序集的路径。</span><span class="sxs-lookup"><span data-stu-id="a83bf-137">`args` is the path to the assembly to debug on the Pi.</span></span>
- <span data-ttu-id="a83bf-138">`cwd` 是在 Pi 上启动应用时使用的工作目录。</span><span class="sxs-lookup"><span data-stu-id="a83bf-138">`cwd` is the working directory to use when launching the app on the Pi.</span></span>
- <span data-ttu-id="a83bf-139">`pipeProgram` 是本地计算机上 SSH 客户端的路径。</span><span class="sxs-lookup"><span data-stu-id="a83bf-139">`pipeProgram` is the path to an SSH client on the local machine.</span></span>
- <span data-ttu-id="a83bf-140">`pipeArgs` 是要传递到 SSH 客户端的参数。</span><span class="sxs-lookup"><span data-stu-id="a83bf-140">`pipeArgs` are the parameters to be passed to the SSH client.</span></span> <span data-ttu-id="a83bf-141">请确保指定 password 参数，以及格式为 `<user>@<hostname>` 的 `root` 用户。</span><span class="sxs-lookup"><span data-stu-id="a83bf-141">Be sure to specify the password parameter, as well as the `root` user in the format `<user>@<hostname>`.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="a83bf-142">上面的示例使用 plink（[PuTTY](https://www.ssh.com/ssh/putty/)<span class="docon docon-navigate-external x-hidden-focus"></span> SSH 客户端的一个组件）。</span><span class="sxs-lookup"><span data-stu-id="a83bf-142">The above example uses *plink*, a component of the [PuTTY](https://www.ssh.com/ssh/putty/)<span class="docon docon-navigate-external x-hidden-focus"></span> SSH client.</span></span> <span data-ttu-id="a83bf-143">可改为使用 [OpenSSH](https://www.openssh.com/)<span class="docon docon-navigate-external x-hidden-focus"></span>（包含在 Windows 和 Linux 的较新版本中）</span><span class="sxs-lookup"><span data-stu-id="a83bf-143">[OpenSSH](https://www.openssh.com/)<span class="docon docon-navigate-external x-hidden-focus"></span>, which is included in recent versions of Windows and Linux, may be used instead.</span></span> <span data-ttu-id="a83bf-144">但是，OpenSSH 不支持密码作为命令行参数发送。</span><span class="sxs-lookup"><span data-stu-id="a83bf-144">However, OpenSSH doesn't support sending passwords as a command-line parameter.</span></span> <span data-ttu-id="a83bf-145">若要使用 OpenSSH，请[为 Raspberry Pi 配置无密码 SSH 访问](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)。</span><span class="sxs-lookup"><span data-stu-id="a83bf-145">To use OpenSSH, [configure your Raspberry Pi for passwordless SSH access](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md).</span></span>

### <a name="deploy-the-app"></a><span data-ttu-id="a83bf-146">部署应用</span><span class="sxs-lookup"><span data-stu-id="a83bf-146">Deploy the app</span></span>

<span data-ttu-id="a83bf-147">按照[将 .Net 应用部署到 Raspberry Pi](deployment.md) 中的说明来部署应用。</span><span class="sxs-lookup"><span data-stu-id="a83bf-147">Deploy the app as described in [Deploy .NET apps to Raspberry Pi](deployment.md).</span></span> <span data-ttu-id="a83bf-148">确保部署路径与 launch.js 配置内 `cwd` 参数中指定的路径相同。</span><span class="sxs-lookup"><span data-stu-id="a83bf-148">Ensure the deployment path is the same path specified in the `cwd` parameter in the *launch.json* configuration.</span></span>

### <a name="launch-the-debugger"></a><span data-ttu-id="a83bf-149">启动调试程序</span><span class="sxs-lookup"><span data-stu-id="a83bf-149">Launch the debugger</span></span>

<span data-ttu-id="a83bf-150">在“运行”选项卡，选择“.NET Core Launch (远程控制台)”配置，然后选择“开始调试”  。</span><span class="sxs-lookup"><span data-stu-id="a83bf-150">On the **Run** tab, select the **.NET Core Launch (remote console)** configuration and select **Start Debugging**.</span></span> <span data-ttu-id="a83bf-151">此应用在 Raspberry Pi 上启动。</span><span class="sxs-lookup"><span data-stu-id="a83bf-151">The app launches on the Raspberry Pi.</span></span> <span data-ttu-id="a83bf-152">调试器可用于设置断点、检查局部变量等。</span><span class="sxs-lookup"><span data-stu-id="a83bf-152">The debugger may be used to set breakpoints, inspect locals, and more.</span></span>

## <a name="references"></a><span data-ttu-id="a83bf-153">参考</span><span class="sxs-lookup"><span data-stu-id="a83bf-153">References</span></span>

<span data-ttu-id="a83bf-154">[Remote debugging with VS Code on Windows to a Raspberry Pi using .NET Core on ARM](https://www.hanselman.com/blog/remote-debugging-with-vs-code-on-windows-to-a-raspberry-pi-using-net-core-on-arm)（在 Windows 上使用 VS Code 与 Raspberry Pi 在 ARM 上使用 .NET Core 进行远程调试的比较）</span><span class="sxs-lookup"><span data-stu-id="a83bf-154">[Remote debugging with VS Code on Windows to a Raspberry Pi using .NET Core on ARM](https://www.hanselman.com/blog/remote-debugging-with-vs-code-on-windows-to-a-raspberry-pi-using-net-core-on-arm)</span></span>

::: zone-end

::: zone pivot="visualstudio"

## <a name="debug-from-visual-studio-on-windows"></a><span data-ttu-id="a83bf-155">在 Windows 上从 Visual Studio 进行调试</span><span class="sxs-lookup"><span data-stu-id="a83bf-155">Debug from Visual Studio on Windows</span></span>

<span data-ttu-id="a83bf-156">Visual Studio 可通过 SSH 在远程设备上调试 .NET 应用。</span><span class="sxs-lookup"><span data-stu-id="a83bf-156">Visual Studio can debug .NET apps on remote devices via SSH.</span></span> <span data-ttu-id="a83bf-157">设备上无需进行专用配置。</span><span class="sxs-lookup"><span data-stu-id="a83bf-157">No specialized configuration is required on the device.</span></span> <span data-ttu-id="a83bf-158">若要详细了解如何使用 Visual Studio 远程调试 .NET，请参阅[在 Linux 上使用 SSH 远程调试 .Net](/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?view=vs-2019)。</span><span class="sxs-lookup"><span data-stu-id="a83bf-158">For details on using Visual Studio to debug .NET remotely, see [Remote debug .NET on Linux using SSH](/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?view=vs-2019).</span></span>

::: zone-end
