---
title: 通过 Docker 使应用程序容器化的教程
description: 在本教程中，你将了解如何使用 Docker 容器化 .NET Core 应用。
ms.date: 03/22/2021
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 0a64743046d31badb10b5240a172b6e47c76d3cc
ms.sourcegitcommit: 26721a2260deabb3318cc98af8619306711153cd
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/24/2021
ms.locfileid: "105027879"
---
# <a name="tutorial-containerize-a-net-core-app"></a>教程：使 .NET Core 应用程序容器化

在本教程中，你将了解如何使用 Docker 容器化 .NET Core 应用。 容器具有很多特性和优点，如具有不可变的基础结构、提供可移植的体系结构和实现可伸缩性。 此映像可用于为本地开发环境、私有云或公有云创建容器。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 创建并发布简单的 .NET Core 应用
> - 创建并配置用于 .NET Core 的 Dockerfile
> - 生成 Docker 映像
> - 创建并运行 Docker 容器

你将了解用于 .NET Core 应用的 Docker 容器生成和部署任务。 Docker 平台  使用 Docker 引擎  快速生成应用，并将应用打包为 Docker 映像  。 这些映像采用 Dockerfile  格式编写，以供在分层容器中部署和运行。

> [!NOTE]
> 本教程不适用于 ASP.NET Core 应用  。 如果使用的是 ASP.NET Core，请参阅教程[了解如何容器化 ASP.NET Core 应用程序](/aspnet/core/host-and-deploy/docker/building-net-docker-images)。

## <a name="prerequisites"></a>先决条件

安装以下必备组件：

- [.NET Core 5.0 SDK](https://dotnet.microsoft.com/download)\
如果已安装 .NET Core，请使用 `dotnet --info` 命令来确定使用的是哪个 SDK。
- [Docker 社区版](https://www.docker.com/products/docker-desktop)
- Dockerfile 和 .NET Core 示例应用的临时工作文件夹  。 在本教程中，docker-working  用作工作文件夹的名称。

## <a name="create-net-core-app"></a>创建 .Net Core 应用

需要有可供 Docker 容器运行的 .NET Core 应用。 打开终端、创建工作文件夹（如果尚没有），然后进入该文件夹。 在工作文件夹中，运行下面的命令，在名为“app”的子目录中新建一个项目  ：

```dotnetcli
dotnet new console -o App -n NetCore.Docker
```

文件夹树将如下所示：

```
docker-working
    └──App
        ├──NetCore.Docker.csproj
        ├──Program.cs
        └──obj
            ├──NetCore.Docker.csproj.nuget.dgspec.json
            ├──NetCore.Docker.csproj.nuget.g.props
            ├──NetCore.Docker.csproj.nuget.g.targets
            ├──project.assets.json
            └──project.nuget.cache
```

`dotnet new` 命令会新建一个名为“应用”的文件夹，并生成一个“Hello World”控制台应用程序  。 从终端会话更改目录并导航到“App”文件夹  。 使用 `dotnet run` 命令启动应用。 应用程序将运行，并在命令下方打印 `Hello World!`：

```dotnetcli
dotnet run
Hello World!
```

默认模板创建应用，此应用先打印输出到终端，然后立即终止。 本教程将使用无限循环的应用。 在文本编辑器中，打开“Program.cs”  文件。

> [!TIP]
> 如果使用 Visual Studio Code，则从上一个终端会话中键入以下命令：
>
> ```console
> code .
> ```
>
> 这会在 Visual Studio Code 中打开包含该项目的“App”文件夹  。

Program.cs 应如下面的 C# 代码所示  ：

```csharp
using System;

namespace NetCore.Docker
{
    class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello World!");
        }
    }
}
```

将此文件替换为以下每秒计数一次的代码：

```csharp
using System;
using System.Threading.Tasks;

namespace NetCore.Docker
{
    class Program
    {
        static async Task Main(string[] args)
        {
            var counter = 0;
            var max = args.Length != 0 ? Convert.ToInt32(args[0]) : -1;
            while (max == -1 || counter < max)
            {
                Console.WriteLine($"Counter: {++counter}");
                await Task.Delay(1000);
            }
        }
    }
}
```

保存此文件，并使用 `dotnet run` 再次测试程序。 请注意，此应用无限期运行。 使用取消命令 <kbd>Ctrl+C</kbd> 可以停止运行。 下面是一个示例输出：

```dotnetcli
dotnet run
Counter: 1
Counter: 2
Counter: 3
Counter: 4
^C
```

如果你在命令行中向应用传递一个数字，它就只会计数到这个数字，然后退出。 试一试用 `dotnet run -- 5` 计数到 5。

> [!IMPORTANT]
> `--` 之后的参数都不传递到 `dotnet run` 命令，而是传递到你的应用程序。

## <a name="publish-net-core-app"></a>发布 .Net Core 应用

在将 .NET Core 应用添加到 Docker 映像之前，必须先发布该应用。 最好让容器运行应用的已发布版本。 若要发布应用，请运行以下命令：

```dotnetcli
dotnet publish -c Release
```

此命令将应用编译到“发布”文件夹中  。 从工作文件夹到“发布”文件夹的路径应为 `.\App\bin\Release\net5.0\publish\`

#### <a name="windows"></a>[Windows](#tab/windows)

在“应用”文件夹中获取“发布”文件夹的目录列表，以验证 NetCore.Docker.dll 文件是否已创建   。

```powershell
dir .\bin\Release\net5.0\publish\

    Directory: C:\Users\dapine\App\bin\Release\net5.0\publish

Mode                LastWriteTime         Length Name
----                -------------         ------ ----
-a----        4/27/2020   8:27 AM            434 NetCore.Docker.deps.json
-a----        4/27/2020   8:27 AM           6144 NetCore.Docker.dll
-a----        4/27/2020   8:27 AM         171520 NetCore.Docker.exe
-a----        4/27/2020   8:27 AM            860 NetCore.Docker.pdb
-a----        4/27/2020   8:27 AM            154 NetCore.Docker.runtimeconfig.json
```

#### <a name="linux"></a>[Linux](#tab/linux)

使用 `ls` 命令获取目录列表，并验证 NetCore.Docker.dll 文件是否已创建  。

```bash
me@DESKTOP:/docker-working/app$ ls bin/Release/net5.0/publish
NetCore.Docker.deps.json  NetCore.Docker.dll  NetCore.Docker.pdb  NetCore.Docker.runtimeconfig.json
```

---

## <a name="create-the-dockerfile"></a>创建 Dockerfile

`docker build` 命令使用 Dockerfile  文件来创建容器映像。 此文件是名为“Dockerfile”  的文本文件，它没有扩展名。

在包含 .csproj 的目录中创建名为“Dockerfile”的文件，并在文本编辑器中将其打开   。 本教程将使用 ASP.NET Core 运行时映像（包含 .NET Core 运行时映像），并与 .NET Core 控制台应用程序相对应。

```dockerfile
FROM mcr.microsoft.com/dotnet/aspnet:5.0
```

> [!NOTE]
> 尽管可能已使用 `mcr.microsoft.com/dotnet/runtime:5.0` 映像，但此处有意使用 ASP.NET Core 运行时映像。

`FROM` 关键字需要完全限定的 Docker 容器映像名称。 Microsoft 容器注册表（MCR，mcr.microsoft.com）是 Docker Hub 的联合，可托管可公开访问的容器。 `dotnet/core` 段是容器存储库，其中 `aspnet` 段是容器映像名称。 该映像使用 `5.0` 进行标记，它用于版本控制。 因此，`mcr.microsoft.com/dotnet/aspnet:5.0` 是 .NET Core 5.0 运行时。 请确保拉取的运行时版本与 SDK 面向的运行时一致。 例如，在上一节中创建的应用使用了 .NET Core 5.0 SDK，在 Dockerfile 中引用的基础映像被标记为 5.0。

保存 Dockerfile 文件  。 工作文件夹的目录结果应如下所示。 为节省本文的空间，省略了一些更深级别的文件和文件夹：

```
docker-working
    └──App
        ├──Dockerfile
        ├──NetCore.Docker.csproj
        ├──Program.cs
        ├──bin
        │   └──Release
        │       └──net5.0
        │           └──publish
        │               ├──NetCore.Docker.deps.json
        │               ├──NetCore.Docker.exe
        │               ├──NetCore.Docker.dll
        │               ├──NetCore.Docker.pdb
        │               └──NetCore.Docker.runtimeconfig.json
        └──obj
            └──...
```

在终端中运行以下命令：

```console
docker build -t counter-image -f Dockerfile .
```

Docker 会处理 Dockerfile  中的每一行。 `docker build` 命令中的 `.` 指示 Docker 在当前文件夹中查找 Dockerfile  。 此命令生成映像，并创建指向相应映像的本地存储库“counter-image”  。 在此命令完成后，运行 `docker images` 以列出已安装的映像：

```console
docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
counter-image                           latest              e6780479db63        4 days ago          190MB
mcr.microsoft.com/dotnet/aspnet         5.0                 e6780479db63        4 days ago          190MB
```

请注意，两个映像共用相同的“IMAGE ID”  值。 两个映像使用的 ID 值相同是因为，Dockerfile  中的唯一命令是在现有映像的基础之上生成新映像。 接下来，在 Dockerfile 中添加三个命令  。 两个命令都新建映像层，最后一个命令表示 counter-image 存储库条目指向的映像  。

```dockerfile
COPY bin/Release/net5.0/publish/ App/
WORKDIR /App
ENTRYPOINT ["dotnet", "NetCore.Docker.dll"]
```

`COPY` 命令指示 Docker 将计算机上的指定文件夹复制到容器中的文件夹。 在此示例中，“publish”文件夹被复制到容器中的“App”文件夹   。

`WORKDIR` 命令将容器内的当前目录更改为“App”   。

下一个命令 `ENTRYPOINT` 指示 Docker 将容器配置为可执行文件运行。 在容器启动时，`ENTRYPOINT` 命令运行。 当此命令结束时，容器也会自动停止。

> [!TIP]
> 为了提高安全性，可以选择退出诊断管道。 选择退出后，容器将以只读方式运行。 为此，请将 `COMPlus_EnableDiagnostics` 环境变量指定为 `0`（就在 `ENTRYPOINT` 步骤之前）：
>
> ```dockerfile
> ENV COMPlus_EnableDiagnostics=0
> ```

在终端中，运行 `docker build -t counter-image -f Dockerfile .`；在此命令完成后，运行 `docker images`。

```console
docker build -t counter-image -f Dockerfile .
Sending build context to Docker daemon  1.117MB
Step 1/4 : FROM mcr.microsoft.com/dotnet/aspnet:5.0
 ---> e6780479db63
Step 2/4 : COPY bin/Release/net5.0/publish/ App/
 ---> d1732740eed2
Step 3/4 : WORKDIR /App
 ---> Running in b1701a42f3ff
Removing intermediate container b1701a42f3ff
 ---> 919aab5b95e3
Step 4/4 : ENTRYPOINT ["dotnet", "NetCore.Docker.dll"]
 ---> Running in c12aebd26ced
Removing intermediate container c12aebd26ced
 ---> cd11c3df9b19
Successfully built cd11c3df9b19
Successfully tagged counter-image:latest

docker images
REPOSITORY                              TAG                 IMAGE ID            CREATED             SIZE
counter-image                           latest              cd11c3df9b19        41 seconds ago      190MB
mcr.microsoft.com/dotnet/aspnet         5.0                 e6780479db63        4 days ago          190MB
```

Dockerfile  中的每个命令生成了一个层，并创建了“IMAGE ID”  。 最终“IMAGE ID”是“cd11c3df9b19”（你的 ID 会有所不同），接下来在此映像的基础之上创建容器   。

## <a name="create-a-container"></a>创建容器

至此，已有包含应用的映像，可以创建容器了。 可以通过两种方式来创建容器。 首先，新建已停止的容器。

```console
docker create --name core-counter counter-image
0f281cb3af994fba5d962cc7d482828484ea14ead6bfe386a35e5088c0058851
```

上面的 `docker create` 命令在 counter-image 映像的基础之上创建容器  。 此命令的输出显示已创建容器的“CONTAINER ID”  （你的 ID 会有所不同）。 若要查看所有  容器的列表，请使用 `docker ps -a` 命令：

```console
docker ps -a
CONTAINER ID    IMAGE            COMMAND                   CREATED           STATUS     PORTS    NAMES
0f281cb3af99    counter-image    "dotnet NetCore.Dock…"    40 seconds ago    Created             core-counter
```

### <a name="manage-the-container"></a>管理容器

容器是使用特定名称 `core-counter` 创建的，此名称用于管理容器。 下面的示例使用 `docker start` 命令来启动容器，然后使用 `docker ps` 命令仅显示正在运行的容器：

```console
docker start core-counter
core-counter

docker ps
CONTAINER ID    IMAGE            COMMAND                   CREATED          STATUS          PORTS    NAMES
2f6424a7ddce    counter-image    "dotnet NetCore.Dock…"    2 minutes ago    Up 11 seconds            core-counter
```

同样，`docker stop` 命令会停止容器。 下面的示例使用 `docker stop` 命令来停止容器，然后使用 `docker ps` 命令来显示未在运行的容器：

```console
docker stop core-counter
core-counter

docker ps
CONTAINER ID    IMAGE    COMMAND    CREATED    STATUS    PORTS    NAMES
```

### <a name="connect-to-a-container"></a>连接到容器

在容器运行后，可以连接到它来查看输出。 使用 `docker start` 和 `docker attach` 命令，启动容器并查看输出流。 在此示例中，<kbd>Ctrl+C</kbd> 击键用于从正在运行的容器中分离出来。 除非另行指定，否则此击键将结束容器中的进程，这会停止容器。 `--sig-proxy=false` 参数可确保 <kbd>Ctrl+C</kbd> 不会停止容器中的进程。

从容器中分离出来后重新连接，以验证它是否仍在运行和计数。

```console
docker start core-counter
core-counter

docker attach --sig-proxy=false core-counter
Counter: 7
Counter: 8
Counter: 9
^C

docker attach --sig-proxy=false core-counter
Counter: 17
Counter: 18
Counter: 19
^C
```

### <a name="delete-a-container"></a>删除容器

就本文而言，你不希望存在不执行任何操作的容器。 删除前面创建的容器。 如果容器正在运行，停止容器。

```console
docker stop core-counter
```

下面的示例列出了所有容器。 然后，它使用 `docker rm` 命令来删除容器，并再次检查是否有任何正在运行的容器。

```console
docker ps -a
CONTAINER ID    IMAGE            COMMAND                   CREATED          STATUS                        PORTS    NAMES
2f6424a7ddce    counter-image    "dotnet NetCore.Dock…"    7 minutes ago    Exited (143) 20 seconds ago            core-counter

docker rm core-counter
core-counter

docker ps -a
CONTAINER ID    IMAGE    COMMAND    CREATED    STATUS    PORTS    NAMES
```

### <a name="single-run"></a>单次运行

Docker 提供了 `docker run` 命令，用于将容器作为单一命令进行创建和运行。 使用此命令，无需依次运行 `docker create` 和 `docker start`。 另外，还可以将此命令设置为，在容器停止时自动删除容器。 例如，使用 `docker run -it --rm` 可以执行两项操作，先自动使用当前终端连接到容器，再在容器完成时删除容器：

```console
docker run -it --rm counter-image
Counter: 1
Counter: 2
Counter: 3
Counter: 4
Counter: 5
^C
```

容器还会将参数传递给 .NET Core 应用的执行。 指示 .NET Core 应用仅计数为 3 个传入 3 个。

```console
docker run -it --rm counter-image 3
Counter: 1
Counter: 2
Counter: 3
```

使用 `docker run -it`，<kbd>Ctrl+C</kbd> 命令会停止在容器中运行的进程，进而停止容器。 由于提供了 `--rm` 参数，因此在进程停止时自动删除容器。 验证它是否不存在：

```console
docker ps -a
CONTAINER ID    IMAGE    COMMAND    CREATED    STATUS    PORTS    NAMES
```

### <a name="change-the-entrypoint"></a>更改 ENTRYPOINT

使用 `docker run` 命令，还可以修改 Dockerfile  中的 `ENTRYPOINT` 命令，并运行其他内容，但只能针对相应容器。 例如，使用以下命令来运行 `bash` 或 `cmd.exe`。 根据需要，编辑此命令。

#### <a name="windows"></a>[Windows](#tab/windows)

在本例中，`ENTRYPOINT` 更改为 `cmd.exe`。 通过按下 <kbd>Ctrl+C</kbd> 来结束进程并停止容器。

```console
docker run -it --rm --entrypoint "cmd.exe" counter-image

Microsoft Windows [Version 10.0.17763.379]
(c) 2018 Microsoft Corporation. All rights reserved.

C:\>dir
 Volume in drive C has no label.
 Volume Serial Number is 3005-1E84

 Directory of C:\

04/09/2019  08:46 AM    <DIR>          app
03/07/2019  10:25 AM             5,510 License.txt
04/02/2019  01:35 PM    <DIR>          Program Files
04/09/2019  01:06 PM    <DIR>          Users
04/02/2019  01:35 PM    <DIR>          Windows
               1 File(s)          5,510 bytes
               4 Dir(s)  21,246,517,248 bytes free

C:\>^C
```

#### <a name="linux"></a>[Linux](#tab/linux)

在本例中，`ENTRYPOINT` 更改为 `bash`。 通过运行 `exit` 命令来结束进程并停止容器。

```bash
docker run -it --rm --entrypoint "bash" counter-image
root@b735b9799abf:/App# ls
NetCore.Docker.deps.json  NetCore.Docker.dll  NetCore.Docker.exe  NetCore.Docker.pdb  NetCore.Docker.runtimeconfig.json
root@b735b9799abf:/App# dotnet NetCore.Docker.dll 3
Counter: 1
Counter: 2
Counter: 3
root@b735b9799abf:/App# exit
exit
```

---

## <a name="essential-commands"></a>重要命令

Docker 包含许多不同的命令，可用于创建、管理以及与容器和映像进行交互。 下面这些 Docker 命令对于管理容器来说至关重要：

- [docker build](https://docs.docker.com/engine/reference/commandline/build/)
- [docker run](https://docs.docker.com/engine/reference/commandline/run/)
- [docker ps](https://docs.docker.com/engine/reference/commandline/ps/)
- [docker stop](https://docs.docker.com/engine/reference/commandline/stop/)
- [docker rm](https://docs.docker.com/engine/reference/commandline/rm/)
- [docker rmi](https://docs.docker.com/engine/reference/commandline/rmi/)
- [docker image](https://docs.docker.com/engine/reference/commandline/image/)

## <a name="clean-up-resources"></a>清理资源

在本教程中，你创建了容器和映像。 如果需要，请删除这些资源。 以下命令可用于

01. 列出所有容器

    ```console
    docker ps -a
    ```

02. 按名称停止正在运行的容器。

    ```console
    docker stop counter-image
    ```

03. 删除容器

    ```console
    docker rm counter-image
    ```

接下来，删除计算机上不再需要的任何映像。 依次删除 Dockerfile  创建的映像，以及 Dockerfile  所依据的 .NET Core 映像。 可以使用 IMAGE ID  或 REPOSITORY:TAG  格式字符串。

```console
docker rmi counter-image:latest
docker rmi mcr.microsoft.com/dotnet/aspnet:5.0
```

使用 `docker images` 命令来列出已安装的映像。

> [!TIP]
> 映像文件可能很大。 通常情况下，需要删除在测试和开发应用期间创建的临时容器。 如果计划在相应运行时的基础之上生成其他映像，通常会将基础映像与运行时一同安装。

## <a name="next-steps"></a>后续步骤

- [了解如何容器化 ASP.NET Core 应用程序。](/aspnet/core/host-and-deploy/docker/building-net-docker-images)
- [试学“ASP.NET Core 微服务”教程。](https://dotnet.microsoft.com/learn/web/aspnet-microservice-tutorial/intro)
- [查看支持容器的 Azure 服务。](https://azure.microsoft.com/overview/containers/)
- [了解 Dockerfile 命令。](https://docs.docker.com/engine/reference/builder/)
- [了解用于 Visual Studio 的容器工具](/visualstudio/containers/overview)
