---
ms.openlocfilehash: 60e326af0d956ceb63b32e5d3ec2436fe09a7005
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96591049"
---
[使用 SFTP 客户端](https://www.raspberrypi.org/documentation/remote-access/ssh/sftp.md)，将文件从开发计算机上的发布位置复制到 Raspberry Pi 上的新文件夹中。

例如，若要使用 `scp` 命令将文件从开发计算机复制到 Raspberry Pi，请打开命令提示符并执行以下命令：

```console
scp -r /publish-location/* pi@raspberrypi:/home/pi/deployment-location/
```

其中：

- `-r` 选项指示 `scp` 以递归方式复制文件。
- /publish-location/ 是在上一步中发布的文件夹。
- `pi@raspberypi` 是格式为 `<username>@<hostname>` 的用户和主机名。
- /home/pi/deployment-location/ 是 Raspberry pi 上的新文件夹。

> [!TIP]
> 最新版本的 Windows 具有 OpenSSH，其中包括预安装的 `scp`。
