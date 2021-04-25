---
title: 如何：启动服务
description: 了解启动服务的多种方式。 安装服务后，必须启动它。 开始调用服务类上的 OnStart 方法。
ms.date: 03/30/2017
helpviewer_keywords:
- Windows Service applications, starting
- services, starting
ms.assetid: 9ea77955-2d96-4c3d-913c-14db7604cdad
ms.openlocfilehash: bf1a4f676c3c7789036cc3650169e04892c2a191
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494540"
---
# <a name="how-to-start-services"></a>如何：启动服务

安装服务后，必须启动它。 开始调用服务类上的 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法。 <xref:System.ServiceProcess.ServiceBase.OnStart%2A> 方法通常定义服务将执行的有用工作。 服务启动后，在手动暂停或停止它前，该服务将保持活动状态。

服务可以设置为自动或手动启动。 自动启动的服务将在安装它的计算机重启或首次打开时启动。 用户必须启动手动启动的服务。

> [!NOTE]
> 默认情况下，使用 Visual Studio 创建的服务设置为手动启动。

可以通过多种方式手动启动服务 — 从“服务器资源管理器”  、“服务控制管理器”  ，或从使用名为 <xref:System.ServiceProcess.ServiceController> 的组件的代码均可启动。

可以在 <xref:System.ServiceProcess.ServiceInstaller> 类中设置 <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> 属性，以确定是应该手动还是自动启动服务。

## <a name="specify-how-a-service-should-start"></a>指定服务的启动方式

1. 在创建服务后为其添加必要的安装程序。 有关详细信息，请参阅[如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)。

2. 在设计器中，单击正在使用的服务的服务安装程序。

3. 在“属性”  窗口中，将 <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> 属性设置为以下之一：

    |安装服务|设置此值|
    |----------------------------------|--------------------|
    |计算机重启时|**自动**|
    |显式用户动作启动服务时|手动|

    > [!TIP]
    > 为防止服务完全启动，可以将 <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> 属性设置为“禁用”  。 如果要多次重启服务器并希望通过阻止在服务器启动时通常会启动的服务来节省时间，则可以执行此操作。

    > [!NOTE]
    > 安装服务后，可以更改这些属性和其他属性。

    可以通过多种方式启动将其 <xref:System.ServiceProcess.ServiceInstaller.StartType%2A> 进程设置为“手动”  的服务 — 从“服务器资源管理器”、   “Windows 服务控制管理器”，或从代码均可启动。 请务必注意，实际上，并非所有这些方法都在服务控制管理器  的上下文中启动服务；  服务器资源管理器和启动服务的编程方法实际上会操纵控制器。

## <a name="start-a-service-from-server-explorer"></a>从服务器资源管理器启动服务

1. 在“服务器资源管理器”  中，添加所需的服务器（如果尚未列出）。 有关详细信息，请参阅“操作说明：访问和初始化服务器资源管理器/数据库资源管理器。

2. 展开“服务”  节点，然后找到要启动的服务。

3. 右键单击该服务的名称，并选择“启动”。

### <a name="start-a-service-from-services"></a>从“服务”启动服务

1. 打开“服务”应用。

2. 从列表中选择你的服务，右键单击该服务，然后选择“启动”。

## <a name="start-a-service-from-code"></a>从代码启动服务

1. 创建 <xref:System.ServiceProcess.ServiceController> 类的实例，并将其配置为与要管理的服务进行交互。

2. 调用 <xref:System.ServiceProcess.ServiceController.Start%2A> 方法以启动该服务。

## <a name="see-also"></a>请参阅

- [Windows 服务应用程序介绍](introduction-to-windows-service-applications.md)
- [如何：创建 Windows 服务](how-to-create-windows-services.md)
- [如何：将安装程序添加到服务应用程序](how-to-add-installers-to-your-service-application.md)
