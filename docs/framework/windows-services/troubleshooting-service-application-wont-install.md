---
title: 疑难解答：服务应用程序无法安装
description: 如果无法安装服务应用程序，请进行故障排除。 请确保已正确设置服务类的 ServiceName 属性。
ms.date: 03/30/2017
helpviewer_keywords:
- troubleshooting service applications
- services, troubleshooting
- services, debugging
- Windows services, troubleshooting
- troubleshooting NT services
- Windows Service applications, troubleshooting
ms.assetid: 45c48e2e-b97d-44bc-8896-14f328e0ce33
ms.openlocfilehash: 057fe52d43daad90df862c3acaec41a427333cba
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494423"
---
# <a name="troubleshooting-service-application-wont-install"></a><span data-ttu-id="6c2a0-104">疑难解答：服务应用程序无法安装</span><span class="sxs-lookup"><span data-stu-id="6c2a0-104">Troubleshooting: Service Application Won't Install</span></span>

<span data-ttu-id="6c2a0-105">如果服务应用程序无法正确安装，请检查以确保服务类的 <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> 属性设置为与该服务安装程序中显示的值相同的值。</span><span class="sxs-lookup"><span data-stu-id="6c2a0-105">If your service application will not install correctly, check to make sure that the <xref:System.ServiceProcess.ServiceBase.ServiceName%2A> property for the service class is set to the same value as is shown in the installer for that service.</span></span> <span data-ttu-id="6c2a0-106">两个实例中的值必须相同才能正确安装服务。</span><span class="sxs-lookup"><span data-stu-id="6c2a0-106">The value must be the same in both instances in order for your service to install correctly.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="6c2a0-107">还可以查看安装日志以获取有关安装进程的反馈。</span><span class="sxs-lookup"><span data-stu-id="6c2a0-107">You can also look at the installation logs to get feedback on the installation process.</span></span>  
  
 <span data-ttu-id="6c2a0-108">还应该检查以确定是否已安装具有相同名称的其他服务。</span><span class="sxs-lookup"><span data-stu-id="6c2a0-108">You should also check to determine whether you have another service with the same name already installed.</span></span> <span data-ttu-id="6c2a0-109">服务名称必须唯一，安装才能成功。</span><span class="sxs-lookup"><span data-stu-id="6c2a0-109">Service names must be unique for installation to succeed.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="6c2a0-110">请参阅</span><span class="sxs-lookup"><span data-stu-id="6c2a0-110">See also</span></span>

- [<span data-ttu-id="6c2a0-111">Windows 服务应用程序介绍</span><span class="sxs-lookup"><span data-stu-id="6c2a0-111">Introduction to Windows Service Applications</span></span>](introduction-to-windows-service-applications.md)
