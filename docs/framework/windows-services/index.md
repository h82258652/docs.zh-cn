---
title: 开发 Windows 服务应用程序
description: 请单击介绍如何使用 Visual Studio 或 .NET SDK 开发 Windows 服务应用的文章的链接。
ms.date: 03/30/2017
helpviewer_keywords:
- ServiceInstaller class, Windows Service applications
- Service class, Windows Service applications
- Windows Service applications
- Windows services
- ServiceProcessInstaller class, Windows Service applications
- services
- .NET applications, Windows applications
ms.assetid: ba72d648-9553-4849-b829-069ad5ea014b
ms.openlocfilehash: 7a19d48ee4e7b12bc6ed0ff7fa5e2c8899bffaca
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494475"
---
# <a name="develop-windows-service-apps"></a><span data-ttu-id="dee4c-103">开发 Windows 服务应用</span><span class="sxs-lookup"><span data-stu-id="dee4c-103">Develop Windows service apps</span></span>

<span data-ttu-id="dee4c-104">使用 Visual Studio 或 .NET Framework SDK，可以通过创建作为服务安装的应用程序来轻松创建服务。</span><span class="sxs-lookup"><span data-stu-id="dee4c-104">Using Visual Studio or the .NET Framework SDK, you can easily create services by creating an application that is installed as a service.</span></span> <span data-ttu-id="dee4c-105">这种类型的应用程序称为 Windows 服务。</span><span class="sxs-lookup"><span data-stu-id="dee4c-105">This type of application is called a Windows service.</span></span> <span data-ttu-id="dee4c-106">借助框架功能，可以创建、安装服务，启动、停止服务并及控制服务的行为。</span><span class="sxs-lookup"><span data-stu-id="dee4c-106">With framework features, you can create services, install them, and start, stop, and otherwise control their behavior.</span></span>

> [!NOTE]
> <span data-ttu-id="dee4c-107">在 Visual Studio 中，可以使用 Visual C# 或 Visual Basic 在托管代码中创建服务，如果需要，可以与现有的 C++ 代码进行互操作。</span><span class="sxs-lookup"><span data-stu-id="dee4c-107">In Visual Studio you can create a service in managed code in Visual C# or Visual Basic, which can interoperate with existing C++ code if required.</span></span> <span data-ttu-id="dee4c-108">或者，可以通过使用 [ATL 项目向导](/cpp/atl/reference/atl-project-wizard)在本机 C++ 中创建 Windows 服务。</span><span class="sxs-lookup"><span data-stu-id="dee4c-108">Or, you can create a Windows service in native C++ by using the [ATL Project Wizard](/cpp/atl/reference/atl-project-wizard).</span></span>

## <a name="in-this-section"></a><span data-ttu-id="dee4c-109">本节内容</span><span class="sxs-lookup"><span data-stu-id="dee4c-109">In this section</span></span>

[<span data-ttu-id="dee4c-110">Windows 服务应用程序介绍</span><span class="sxs-lookup"><span data-stu-id="dee4c-110">Introduction to Windows Service Applications</span></span>](introduction-to-windows-service-applications.md)

<span data-ttu-id="dee4c-111">提供 Windows 服务应用程序、服务生存期以及服务应用程序与其他常见项目类型的区别的概述。</span><span class="sxs-lookup"><span data-stu-id="dee4c-111">Provides an overview of Windows service applications, the lifetime of a service, and how service applications differ from other common project types.</span></span>

[<span data-ttu-id="dee4c-112">演练：在组件设计器中创建 Windows 服务应用程序</span><span class="sxs-lookup"><span data-stu-id="dee4c-112">Walkthrough: Creating a Windows Service Application in the Component Designer</span></span>](walkthrough-creating-a-windows-service-application-in-the-component-designer.md)

<span data-ttu-id="dee4c-113">提供在 Visual Basic 和 Visual C# 中创建服务的示例。</span><span class="sxs-lookup"><span data-stu-id="dee4c-113">Provides an example of creating a service in Visual Basic and Visual C#.</span></span>

[<span data-ttu-id="dee4c-114">服务应用程序编程体系结构</span><span class="sxs-lookup"><span data-stu-id="dee4c-114">Service Application Programming Architecture</span></span>](service-application-programming-architecture.md)

<span data-ttu-id="dee4c-115">介绍服务编程中使用的语言元素。</span><span class="sxs-lookup"><span data-stu-id="dee4c-115">Explains the language elements used in service programming.</span></span>

[<span data-ttu-id="dee4c-116">如何：创建 Windows 服务</span><span class="sxs-lookup"><span data-stu-id="dee4c-116">How to: Create Windows Services</span></span>](how-to-create-windows-services.md)

<span data-ttu-id="dee4c-117">介绍使用 Windows 服务项目模板创建和配置 Windows 服务的过程。</span><span class="sxs-lookup"><span data-stu-id="dee4c-117">Describes the process of creating and configuring Windows services using the Windows service project template.</span></span>

## <a name="related-sections"></a><span data-ttu-id="dee4c-118">相关章节</span><span class="sxs-lookup"><span data-stu-id="dee4c-118">Related sections</span></span>

<span data-ttu-id="dee4c-119"><xref:System.ServiceProcess.ServiceBase> - 介绍用于创建服务的 <xref:System.ServiceProcess.ServiceBase> 类的主要功能。</span><span class="sxs-lookup"><span data-stu-id="dee4c-119"><xref:System.ServiceProcess.ServiceBase> - Describes the major features of the <xref:System.ServiceProcess.ServiceBase> class, which is used to create services.</span></span>

<span data-ttu-id="dee4c-120"><xref:System.ServiceProcess.ServiceProcessInstaller> - 介绍 <xref:System.ServiceProcess.ServiceProcessInstaller> 类的功能，该类与 <xref:System.ServiceProcess.ServiceInstaller> 类一起用于安装和卸载服务。</span><span class="sxs-lookup"><span data-stu-id="dee4c-120"><xref:System.ServiceProcess.ServiceProcessInstaller> - Describes the features of the <xref:System.ServiceProcess.ServiceProcessInstaller> class, which is used along with the <xref:System.ServiceProcess.ServiceInstaller> class to install and uninstall your services.</span></span>

<span data-ttu-id="dee4c-121"><xref:System.ServiceProcess.ServiceInstaller> - 介绍 <xref:System.ServiceProcess.ServiceInstaller> 类的功能，该类与 <xref:System.ServiceProcess.ServiceProcessInstaller> 类一起用于安装和卸载服务。</span><span class="sxs-lookup"><span data-stu-id="dee4c-121"><xref:System.ServiceProcess.ServiceInstaller> - Describes the features of the <xref:System.ServiceProcess.ServiceInstaller> class, which is used along with the <xref:System.ServiceProcess.ServiceProcessInstaller> class to install and uninstall your service.</span></span>

<span data-ttu-id="dee4c-122">[从模板创建项目](/previous-versions/visualstudio/visual-studio-2013/0fyc0azh(v=vs.120)) - 介绍本章中使用的项目类型以及如何从中进行选择。</span><span class="sxs-lookup"><span data-stu-id="dee4c-122">[Create Projects from Templates](/previous-versions/visualstudio/visual-studio-2013/0fyc0azh(v=vs.120)) -  Describes the projects types used in this chapter and how to choose between them.</span></span>
