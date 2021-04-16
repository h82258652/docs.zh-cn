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
# <a name="debug-net-apps-on-raspberry-pi"></a>在 Raspberry Pi 上调试 .NET 应用

调试在基于 ARM 的 IoT 设备（如 Raspberry Pi）上运行的 .NET 应用带来了独特的挑战。 可以在 ARM 设备上开发 .NET 应用。 但是，在 Visual Studio 外调试 .NET 应用所需的 OmniSharp 与 ARM 设备不兼容。 因此，必须从兼容平台远程调试应用。

::: zone pivot="vscode"

## <a name="debug-from-visual-studio-code-cross-platform"></a>从 Visual Studio Code（跨平台）调试

从 Visual Studio Code 在 Raspberry Pi 上调试 .NET，需要在 Raspberry Pi 上以及项目的 launch.json 文件中执行配置步骤。

### <a name="enable-ssh-on-the-raspberry-pi"></a>在 Raspberry Pi 上启用 SSH

远程调试需要 SSH。 若要启用 SSH，请[参阅 Raspberry Pi 文档中的“启用 SSH”](https://www.raspberrypi.org/documentation/remote-access/ssh/)。

### <a name="install-the-visual-studio-remote-debugger-on-the-raspberry-pi"></a>在 Raspberry Pi 上安装 Visual Studio 远程调试器

在 Raspberry Pi 上的 Bash 控制台中（从本地或通过 SSH）完成以下步骤：

1. 执行以下命令以在 Raspberry Pi 上下载并安装 Visual Studio 远程调试器：

    ```bash
    curl -sSL https://aka.ms/getvsdbgsh | /bin/sh /dev/stdin -v latest -l ~/vsdbg
    ```

1. 调试器需要作为 `root` 运行。 默认情况下，`root` 没有 Raspberry Pi 上的密码。 通过执行以下命令并按照提示设置 `root` 的密码。

    ```bash
    sudo passwd root
    ```

1. Visual Studio Code 使用 SSH 协议进行远程调试。 出于安全考虑，默认不允许 `root` 通过 SSH 登录。 若要使 `root` 通过 SSH 登录，请完成以下步骤：

    1. 执行以下命令以在 [nano](https://www.nano-editor.org/docs.php) 中打开 /etc/ssh/sshd_config。

        ```bash
        sudo nano /etc/ssh/sshd_config
        ```

    1. 按 <kbd>Ctrl+W</kbd> 并搜索 PermitRootLogin。
    1. 如果需要，取消 PermitRootLogin 行的评论。
    1. 按如下所示更改行：

        ```console
        PermitRootLogin yes
        ```

        > [!NOTE]
        > 如果在 /etc/ssh/sshd_config 中没有 PermitRootLogin 行，请使用如上所示的值添加新行。

    1. 按 <kbd>Ctrl+X</kbd> 退出并保存更改。 当系统提示“是否保存已修改的缓冲区?”时，请按 <kbd>Y</kbd> 确认。 按 <kbd>Enter</kbd> 确认覆盖原始文件。
    1. 通过执行以下命令重新启动 Raspberry Pi：

        ```bash
        sudo reboot
        ```

### <a name="setup-launchjson-in-visual-studio-code"></a>在 Visual Studio Code 中安装 launch.json 文件

将启动配置添加到项目的 launch.js 文件中。 如果项目没有 launch.json 文件，请添加一个文件，方法是切换到“运行”选项卡，选择“创建 launch.json 文件”，然后在对话框中选择“.NET”或“.NET Core”   。

launch.json 中的新配置应如下所示：

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

请注意以下内容：

- `program` 是 Pi 上的 .NET 运行时的路径。
- `args` 是要在 Pi 上调试的程序集的路径。
- `cwd` 是在 Pi 上启动应用时使用的工作目录。
- `pipeProgram` 是本地计算机上 SSH 客户端的路径。
- `pipeArgs` 是要传递到 SSH 客户端的参数。 请确保指定 password 参数，以及格式为 `<user>@<hostname>` 的 `root` 用户。

> [!IMPORTANT]
> 上面的示例使用 plink（[PuTTY](https://www.ssh.com/ssh/putty/)<span class="docon docon-navigate-external x-hidden-focus"></span> SSH 客户端的一个组件）。 可改为使用 [OpenSSH](https://www.openssh.com/)<span class="docon docon-navigate-external x-hidden-focus"></span>（包含在 Windows 和 Linux 的较新版本中） 但是，OpenSSH 不支持密码作为命令行参数发送。 若要使用 OpenSSH，请[为 Raspberry Pi 配置无密码 SSH 访问](https://www.raspberrypi.org/documentation/remote-access/ssh/passwordless.md)。

### <a name="deploy-the-app"></a>部署应用

按照[将 .Net 应用部署到 Raspberry Pi](deployment.md) 中的说明来部署应用。 确保部署路径与 launch.js 配置内 `cwd` 参数中指定的路径相同。

### <a name="launch-the-debugger"></a>启动调试程序

在“运行”选项卡，选择“.NET Core Launch (远程控制台)”配置，然后选择“开始调试”  。 此应用在 Raspberry Pi 上启动。 调试器可用于设置断点、检查局部变量等。

## <a name="references"></a>参考

[Remote debugging with VS Code on Windows to a Raspberry Pi using .NET Core on ARM](https://www.hanselman.com/blog/remote-debugging-with-vs-code-on-windows-to-a-raspberry-pi-using-net-core-on-arm)（在 Windows 上使用 VS Code 与 Raspberry Pi 在 ARM 上使用 .NET Core 进行远程调试的比较）

::: zone-end

::: zone pivot="visualstudio"

## <a name="debug-from-visual-studio-on-windows"></a>在 Windows 上从 Visual Studio 进行调试

Visual Studio 可通过 SSH 在远程设备上调试 .NET 应用。 设备上无需进行专用配置。 若要详细了解如何使用 Visual Studio 远程调试 .NET，请参阅[在 Linux 上使用 SSH 远程调试 .Net](/visualstudio/debugger/remote-debugging-dotnet-core-linux-with-ssh?view=vs-2019)。

::: zone-end
