---
description: 了解详细信息：使用诊断 Windows Management Instrumentation
title: 使用 Windows Management Instrumentation 进行诊断
ms.date: 03/30/2017
ms.assetid: fe48738d-e31b-454d-b5ec-24c85c6bf79a
ms.openlocfilehash: 2c3d79133b24c180f69d8ad015c1b08d9a9d188f
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494813"
---
# <a name="using-windows-management-instrumentation-for-diagnostics"></a><span data-ttu-id="dc45d-103">使用 Windows Management Instrumentation 进行诊断</span><span class="sxs-lookup"><span data-stu-id="dc45d-103">Using Windows Management Instrumentation for Diagnostics</span></span>

<span data-ttu-id="dc45d-104">Windows Communication Foundation (WCF) 会通过 WCF Windows Management Instrumentation (WMI) 提供程序，在运行时公开服务的检测数据。</span><span class="sxs-lookup"><span data-stu-id="dc45d-104">Windows Communication Foundation (WCF) exposes inspection data of a service at runtime through a WCF Windows Management Instrumentation (WMI) provider.</span></span>  
  
## <a name="enabling-wmi"></a><span data-ttu-id="dc45d-105">启用 WMI</span><span class="sxs-lookup"><span data-stu-id="dc45d-105">Enabling WMI</span></span>  

 <span data-ttu-id="dc45d-106">WMI 是 Microsoft 基于 Web 的企业管理 (WBEM) 标准的实现。</span><span class="sxs-lookup"><span data-stu-id="dc45d-106">WMI is Microsoft's implementation of the Web-Based Enterprise Management (WBEM) standard.</span></span> <span data-ttu-id="dc45d-107">有关 WMI SDK 的详细信息，请参阅 [Windows Management Instrumentation](/windows/desktop/WmiSdk/wmi-start-page)。</span><span class="sxs-lookup"><span data-stu-id="dc45d-107">For more information about the WMI SDK, see [Windows Management Instrumentation](/windows/desktop/WmiSdk/wmi-start-page).</span></span> <span data-ttu-id="dc45d-108">WBEM 是有关应用程序如何向外部管理工具公开管理规范的行业标准。</span><span class="sxs-lookup"><span data-stu-id="dc45d-108">WBEM is an industry standard for how applications expose management instrumentation to external management tools.</span></span>  
  
 <span data-ttu-id="dc45d-109">WMI 提供程序是一个在运行时通过 WBEM 兼容接口公开规范的组件。</span><span class="sxs-lookup"><span data-stu-id="dc45d-109">A WMI provider is a component that exposes instrumentation at runtime through a WBEM-compatible interface.</span></span> <span data-ttu-id="dc45d-110">它由一组包含属性/值对的 WMI 对象组成。</span><span class="sxs-lookup"><span data-stu-id="dc45d-110">It consists of a set of WMI objects that have attribute/value pairs.</span></span> <span data-ttu-id="dc45d-111">这些对可以是多个简单类型。</span><span class="sxs-lookup"><span data-stu-id="dc45d-111">Pairs can be of a number of simple types.</span></span> <span data-ttu-id="dc45d-112">管理工具可以在运行时通过接口连接至服务。</span><span class="sxs-lookup"><span data-stu-id="dc45d-112">Management tools can connect to the services through the interface at runtime.</span></span> <span data-ttu-id="dc45d-113">WCF 公开服务的属性，如地址、绑定、行为和侦听器。</span><span class="sxs-lookup"><span data-stu-id="dc45d-113">WCF exposes attributes of services such as addresses, bindings, behaviors, and listeners.</span></span>  
  
 <span data-ttu-id="dc45d-114">您可以在应用程序的配置文件中激活内置 WMI 提供程序。</span><span class="sxs-lookup"><span data-stu-id="dc45d-114">The built-in WMI provider can be activated in the configuration file of the application.</span></span> <span data-ttu-id="dc45d-115">此操作通过 `wmiProviderEnabled` 部分中的属性完成 [\<diagnostics>](../../../configure-apps/file-schema/wcf/diagnostics.md) [\<system.serviceModel>](../../../configure-apps/file-schema/wcf/system-servicemodel.md) ，如下面的示例配置中所示。</span><span class="sxs-lookup"><span data-stu-id="dc45d-115">This is done through the `wmiProviderEnabled` attribute of the [\<diagnostics>](../../../configure-apps/file-schema/wcf/diagnostics.md) in the [\<system.serviceModel>](../../../configure-apps/file-schema/wcf/system-servicemodel.md) section, as shown in the following sample configuration.</span></span>  
  
```xml  
<system.serviceModel>  
    …  
    <diagnostics wmiProviderEnabled="true" />  
    …  
</system.serviceModel>  
```  
  
 <span data-ttu-id="dc45d-116">此配置项公开 WMI 接口。</span><span class="sxs-lookup"><span data-stu-id="dc45d-116">This configuration entry exposes a WMI interface.</span></span> <span data-ttu-id="dc45d-117">现在，您可以通过此接口连接管理应用程序并访问应用程序的管理规范。</span><span class="sxs-lookup"><span data-stu-id="dc45d-117">Management applications can now connect through this interface and access the management instrumentation of the application.</span></span>  
  
## <a name="accessing-wmi-data"></a><span data-ttu-id="dc45d-118">访问 WMI 数据</span><span class="sxs-lookup"><span data-stu-id="dc45d-118">Accessing WMI Data</span></span>  

 <span data-ttu-id="dc45d-119">可以采用多种不同方式访问 WMI 数据。</span><span class="sxs-lookup"><span data-stu-id="dc45d-119">WMI data can be accessed in many different ways.</span></span> <span data-ttu-id="dc45d-120">Microsoft 为脚本、Visual Basic 应用程序、c + + 应用程序和 .NET Framework 提供 WMI Api。</span><span class="sxs-lookup"><span data-stu-id="dc45d-120">Microsoft provides WMI APIs for scripts, Visual Basic applications, C++ applications, and the .NET Framework.</span></span> <span data-ttu-id="dc45d-121">有关详细信息，请参阅 [使用 WMI](/windows/win32/wmisdk/using-wmi)。</span><span class="sxs-lookup"><span data-stu-id="dc45d-121">For more information, see [Using WMI](/windows/win32/wmisdk/using-wmi).</span></span>  
  
> [!CAUTION]
> <span data-ttu-id="dc45d-122">如果您使用 .NET Framework 提供的方法以编程方式访问 WMI 数据，您应注意此类方法可能会在建立连接时引发异常。</span><span class="sxs-lookup"><span data-stu-id="dc45d-122">If you use the .NET Framework provided methods to programmatically access WMI data, you should be aware that such methods may throw exceptions when the connection is established.</span></span> <span data-ttu-id="dc45d-123">连接并不是在构建 <xref:System.Management.ManagementObject> 实例的过程中建立的，而是在涉及实际数据交换的第一次请求时建立的。</span><span class="sxs-lookup"><span data-stu-id="dc45d-123">The connection is not established during the construction of the <xref:System.Management.ManagementObject> instance, but on the first request involving actual data exchange.</span></span> <span data-ttu-id="dc45d-124">因此，您应该使用 `try..catch` 块来捕捉可能的异常。</span><span class="sxs-lookup"><span data-stu-id="dc45d-124">Therefore, you should use a `try..catch` block to catch the possible exceptions.</span></span>  
  
 <span data-ttu-id="dc45d-125">您可以在 WMI 中更改 `System.ServiceModel` 跟踪源的跟踪和消息日志记录级别以及消息日志记录选项。</span><span class="sxs-lookup"><span data-stu-id="dc45d-125">You can change the trace and message logging level, as well as message logging options for the `System.ServiceModel` trace source in WMI.</span></span> <span data-ttu-id="dc45d-126">为此，可以访问 [AppDomainInfo](appdomaininfo.md) 实例，该实例将公开下列布尔属性： `LogMessagesAtServiceLevel` 、 `LogMessagesAtTransportLevel` 、 `LogMalformedMessages` 和 `TraceLevel` 。</span><span class="sxs-lookup"><span data-stu-id="dc45d-126">This can be done by accessing the [AppDomainInfo](appdomaininfo.md) instance, which exposes these Boolean properties: `LogMessagesAtServiceLevel`, `LogMessagesAtTransportLevel`, `LogMalformedMessages`, and `TraceLevel`.</span></span> <span data-ttu-id="dc45d-127">因此，如果为消息日志记录配置了跟踪侦听器，但是在配置中将这些选项设置为 `false`，那么可以在以后运行应用程序时将它们更改为 `true`。</span><span class="sxs-lookup"><span data-stu-id="dc45d-127">Therefore, if you configure a trace listener for message logging, but set these options to `false` in configuration, you can later change them to `true` when the application is running.</span></span> <span data-ttu-id="dc45d-128">这将在运行时有效地启用消息日志记录。</span><span class="sxs-lookup"><span data-stu-id="dc45d-128">This will effectively enable message logging at runtime.</span></span> <span data-ttu-id="dc45d-129">同样，如果在配置文件中启用了消息日志记录，可以在运行时使用 WMI 将其禁用。</span><span class="sxs-lookup"><span data-stu-id="dc45d-129">Similarly, if you enable message logging in your configuration file, you can disable it at runtime using WMI.</span></span>  
  
 <span data-ttu-id="dc45d-130">您应该注意如果没有用于消息日志记录的消息日志记录跟踪侦听器，或者配置文件中没有指定用于跟踪的 `System.ServiceModel` 跟踪侦听器，您所执行的任何更改都将无效，即使已被 WMI 接受的更改也不例外。</span><span class="sxs-lookup"><span data-stu-id="dc45d-130">You should be aware that if no message logging trace listeners for message logging, or no `System.ServiceModel` trace listeners for tracing are specified in the configuration file, none of your changes are taken into effect, even though the changes are accepted by WMI.</span></span> <span data-ttu-id="dc45d-131">有关正确设置各个侦听器的详细信息，请参阅 [配置消息日志记录](../configuring-message-logging.md) 和 [配置跟踪](../tracing/configuring-tracing.md)。</span><span class="sxs-lookup"><span data-stu-id="dc45d-131">For more information on properly setting up the respective listeners, see [Configuring Message Logging](../configuring-message-logging.md) and [Configuring Tracing](../tracing/configuring-tracing.md).</span></span> <span data-ttu-id="dc45d-132">应用程序启动时，由配置指定的所有其他跟踪源的跟踪级别均有效，而且不能更改。</span><span class="sxs-lookup"><span data-stu-id="dc45d-132">The trace level of all other trace sources specified by configuration is effective when the application starts, and cannot be changed.</span></span>  
  
 <span data-ttu-id="dc45d-133">WCF 公开 `GetOperationCounterInstanceName` 用于脚本编写的方法。</span><span class="sxs-lookup"><span data-stu-id="dc45d-133">WCF exposes a `GetOperationCounterInstanceName` method for scripting.</span></span> <span data-ttu-id="dc45d-134">如果您为其提供了一个操作名称，此方法将返回一个性能计数器实例名称。</span><span class="sxs-lookup"><span data-stu-id="dc45d-134">This method returns a performance counter instance name if you provide it with an operation name.</span></span> <span data-ttu-id="dc45d-135">但是，它不会验证您的输入。</span><span class="sxs-lookup"><span data-stu-id="dc45d-135">However, it does not validate your input.</span></span> <span data-ttu-id="dc45d-136">因此，如果您提供了一个不正确的操作名称，将返回不正确的计数器名称。</span><span class="sxs-lookup"><span data-stu-id="dc45d-136">Therefore, if you provide an incorrect operation name, an incorrect counter name is returned.</span></span>  
  
 <span data-ttu-id="dc45d-137">`OutgoingChannel` `Service` 如果未在方法中创建目标服务的 WCF 客户端，则实例的属性不会对服务打开的用于连接到其他服务的通道计数 `Service` 。</span><span class="sxs-lookup"><span data-stu-id="dc45d-137">The `OutgoingChannel` property of the `Service` instance does not count channels opened by a service to connect to another service, if the WCF client to the destination service is not created within the `Service` method.</span></span>  
  
 <span data-ttu-id="dc45d-138">**警告** WMI 仅支持 <xref:System.TimeSpan> 最多3个小数位数的值。</span><span class="sxs-lookup"><span data-stu-id="dc45d-138">**Caution** WMI only supports a <xref:System.TimeSpan> value up to 3 decimal points.</span></span> <span data-ttu-id="dc45d-139">例如，如果您的服务将其中一个属性设置为 <xref:System.TimeSpan.MaxValue>，当通过 WMI 查看时，它的值将保留 3 位小数。</span><span class="sxs-lookup"><span data-stu-id="dc45d-139">For example, if your service sets one of its properties to <xref:System.TimeSpan.MaxValue>, its value is truncated after 3 decimal points when viewed through WMI.</span></span>  
  
## <a name="security"></a><span data-ttu-id="dc45d-140">安全性</span><span class="sxs-lookup"><span data-stu-id="dc45d-140">Security</span></span>  

 <span data-ttu-id="dc45d-141">因为 WCF WMI 提供程序允许发现环境中的服务，所以，您应该非常小心地授予对该服务的访问权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-141">Because the WCF WMI provider allows the discovery of services in an environment, you should exercise extreme caution for granting access to it.</span></span> <span data-ttu-id="dc45d-142">如果您放宽了默认的仅管理员访问，就可能允许不完全受信任方访问您的环境中的敏感数据。</span><span class="sxs-lookup"><span data-stu-id="dc45d-142">If you relax the default administrator-only access, you may allow less-trusted parties access to sensitive data in your environment.</span></span> <span data-ttu-id="dc45d-143">特别是，如果您放宽了对远程 WMI 访问的权限，可能会发生洪泛攻击。</span><span class="sxs-lookup"><span data-stu-id="dc45d-143">Specifically, if you loosen permissions on remote WMI access, flooding attacks can occur.</span></span> <span data-ttu-id="dc45d-144">如果某个进程被大量 WMI 请求溢满，其性能将有所下降。</span><span class="sxs-lookup"><span data-stu-id="dc45d-144">If a process is flooded by excessive WMI requests, its performance can be degraded.</span></span>  
  
 <span data-ttu-id="dc45d-145">此外，如果您放宽了对 MOF 文件的访问权限，不完全受信任方可以控制 WMI 的行为并更改加载到 WMI 架构中的对象。</span><span class="sxs-lookup"><span data-stu-id="dc45d-145">In addition, if you relax access permissions for the MOF file, less-trusted parties can manipulate the behavior of WMI and alter the objects that are loaded in the WMI schema.</span></span> <span data-ttu-id="dc45d-146">例如，不完全受信任方可以移除字段，这样，管理员将无法看到关键数据，或者未填充或导致异常的字段将添加到文件中。</span><span class="sxs-lookup"><span data-stu-id="dc45d-146">For example, fields can be removed such that critical data is concealed from the administrator or that fields that do not populate or cause exceptions are added to the file.</span></span>  
  
 <span data-ttu-id="dc45d-147">默认情况下，WCF WMI 提供程序为管理员授予 "执行方法"、"提供程序写入" 和 "启用帐户" 权限，并为 ASP.NET、本地服务和网络服务授予 "启用帐户" 权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-147">By default, the WCF WMI provider grants "execute method", "provider write", and "enable account" permission for Administrator, and "enable account" permission for ASP.NET, Local Service and Network Service.</span></span> <span data-ttu-id="dc45d-148">特别是在非 Windows Vista 平台上，ASP.NET 帐户对 WMI 空间命名空间具有读取访问权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-148">In particular, on non-Windows Vista platforms, the ASP.NET account has read access to the WMI ServiceModel namespace.</span></span> <span data-ttu-id="dc45d-149">如果您不希望对特定用户组授予这些特权，就应该停用 WMI 提供程序（默认状态为禁用），或者是禁用对特定用户组的访问。</span><span class="sxs-lookup"><span data-stu-id="dc45d-149">If you do not want to grant these privileges to a particular user group, you should either deactivate the WMI provider (it is disabled by default), or disable access for the specific user group.</span></span>  
  
 <span data-ttu-id="dc45d-150">此外，当您尝试通过配置启用 WMI 时，WMI 可能会因用户特权不足而无法启用。</span><span class="sxs-lookup"><span data-stu-id="dc45d-150">In addition, when you attempt to enable WMI through configuration, WMI may not be enabled due to insufficient user privilege.</span></span> <span data-ttu-id="dc45d-151">但是，并不会将任何事件写入事件日志用于记录此次失败。</span><span class="sxs-lookup"><span data-stu-id="dc45d-151">However, no event is written to the event log to record this failure.</span></span>  
  
 <span data-ttu-id="dc45d-152">若要修改用户特权级别，请使用下列步骤。</span><span class="sxs-lookup"><span data-stu-id="dc45d-152">To modify user privilege levels, use the following steps.</span></span>  
  
1. <span data-ttu-id="dc45d-153">单击 "开始"，然后单击 "运行"，然后键入 **compmgmt.msc**。</span><span class="sxs-lookup"><span data-stu-id="dc45d-153">Click Start and then Run and type **compmgmt.msc**.</span></span>  
  
2. <span data-ttu-id="dc45d-154">右键单击 " **服务和应用程序/WMI 控件** " 以选择 " **属性**"。</span><span class="sxs-lookup"><span data-stu-id="dc45d-154">Right-click **Services and Application/WMI Controls** to select **Properties**.</span></span>  
  
3. <span data-ttu-id="dc45d-155">选择 " **安全** " 选项卡，然后导航到 **根/** 空间命名空间。</span><span class="sxs-lookup"><span data-stu-id="dc45d-155">Select the **Security** Tab, and navigate to the **Root/ServiceModel** namespace.</span></span> <span data-ttu-id="dc45d-156">单击 " **安全** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="dc45d-156">Click the **Security** button.</span></span>  
  
4. <span data-ttu-id="dc45d-157">选择要控制其访问权限的特定组或用户，并使用 " **允许** " 或 " **拒绝** " 复选框配置权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-157">Select the specific group or user that you want to control access and use the **Allow** or **Deny** checkbox to configure permissions.</span></span>  
  
## <a name="granting-wcf-wmi-registration-permissions-to-additional-users"></a><span data-ttu-id="dc45d-158">向其他用户授予 WCF WMI 注册权限</span><span class="sxs-lookup"><span data-stu-id="dc45d-158">Granting WCF WMI Registration Permissions to Additional Users</span></span>  

 <span data-ttu-id="dc45d-159">WCF 将管理数据公开到 WMI。</span><span class="sxs-lookup"><span data-stu-id="dc45d-159">WCF exposes management data to WMI.</span></span> <span data-ttu-id="dc45d-160">它通过承载进程内 WMI 提供程序（有时称为 "分离的提供程序"）来实现此目的。</span><span class="sxs-lookup"><span data-stu-id="dc45d-160">It does so by hosting an in-process WMI provider, sometimes called a "decoupled provider".</span></span> <span data-ttu-id="dc45d-161">对于要公开的管理数据，注册此提供程序的帐户必须具有适当的权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-161">For the management data to be exposed, the account that registers this provider must have the appropriate permissions.</span></span> <span data-ttu-id="dc45d-162">在 Windows 中，默认情况下只有一小组特权帐户可以注册分离的提供程序。</span><span class="sxs-lookup"><span data-stu-id="dc45d-162">In Windows, only a small set of privileged accounts can register decoupled providers by default.</span></span> <span data-ttu-id="dc45d-163">这是一个问题，因为用户通常要从不在默认集中的帐户下运行的 WCF 服务公开 WMI 数据。</span><span class="sxs-lookup"><span data-stu-id="dc45d-163">This is a problem because users commonly want to expose WMI data from a WCF service running under an account that is not in the default set.</span></span>  
  
 <span data-ttu-id="dc45d-164">若要提供此访问权，管理员必须按以下顺序向其他帐户授予以下权限：</span><span class="sxs-lookup"><span data-stu-id="dc45d-164">To provide this access, an administrator must grant the following permissions to the additional account in the following order:</span></span>  
  
1. <span data-ttu-id="dc45d-165">用于访问 WCF WMI 命名空间的权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-165">Permission to access to the WCF WMI Namespace.</span></span>  
  
2. <span data-ttu-id="dc45d-166">用于注册 WCF 分离 WMI 提供程序的权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-166">Permission to register the WCF Decoupled WMI Provider.</span></span>  
  
#### <a name="to-grant-wmi-namespace-access-permission"></a><span data-ttu-id="dc45d-167">授予 WMI 命名空间访问权限</span><span class="sxs-lookup"><span data-stu-id="dc45d-167">To grant WMI namespace access permission</span></span>  
  
1. <span data-ttu-id="dc45d-168">运行下面的 PowerShell 脚本。</span><span class="sxs-lookup"><span data-stu-id="dc45d-168">Run the following PowerShell script.</span></span>  
  
    ```powershell  
    write-host ""  
    write-host "Granting Access to root/servicemodel WMI namespace to built in users group"  
    write-host ""  
  
    # Create the binary representation of the permissions to grant in SDDL  
    $newPermissions = "O:BAG:BAD:P(A;CI;CCDCLCSWRPWPRCWD;;;BA)(A;CI;CC;;;NS)(A;CI;CC;;;LS)(A;CI;CC;;;BU)"  
    $converter = new-object system.management.ManagementClass Win32_SecurityDescriptorHelper  
    $binarySD = $converter.SDDLToBinarySD($newPermissions)  
    $convertedPermissions = ,$binarySD.BinarySD  
  
    # Get the object used to set the permissions  
    $security = gwmi -namespace root/servicemodel -class __SystemSecurity  
  
    # Get and output the current settings  
    $binarySD = @($null)  
    $result = $security.PsBase.InvokeMethod("GetSD",$binarySD)  
  
    $outsddl = $converter.BinarySDToSDDL($binarySD[0])  
    write-host "Previous ACL: "$outsddl.SDDL  
  
    # Change the Access Control List (ACL) using SDDL  
    $result = $security.PsBase.InvokeMethod("SetSD",$convertedPermissions)
  
    # Get and output the current settings  
    $binarySD = @($null)  
    $result = $security.PsBase.InvokeMethod("GetSD",$binarySD)  
  
    $outsddl = $converter.BinarySDToSDDL($binarySD[0])  
    write-host "New ACL:      "$outsddl.SDDL  
    write-host ""  
    ```  
  
     <span data-ttu-id="dc45d-169">此 PowerShell 脚本使用安全描述符定义语言 (SDDL) 授予 Built-In 用户组访问 "root/用户" WMI 命名空间的权限。</span><span class="sxs-lookup"><span data-stu-id="dc45d-169">This PowerShell script uses Security Descriptor Definition Language (SDDL) to grant the Built-In Users group access to the "root/servicemodel" WMI namespace.</span></span> <span data-ttu-id="dc45d-170">它指定了以下 ACL：</span><span class="sxs-lookup"><span data-stu-id="dc45d-170">It specifies the following ACLs:</span></span>  
  
    - <span data-ttu-id="dc45d-171">内置管理员 (BA) - 已经具有访问权。</span><span class="sxs-lookup"><span data-stu-id="dc45d-171">Built-In Administrator (BA) - Already Had Access.</span></span>  
  
    - <span data-ttu-id="dc45d-172">网络服务 (NS) - 已经具有访问权。</span><span class="sxs-lookup"><span data-stu-id="dc45d-172">Network Service (NS) - Already Had Access.</span></span>  
  
    - <span data-ttu-id="dc45d-173">本地系统 (LS) - 已经具有访问权。</span><span class="sxs-lookup"><span data-stu-id="dc45d-173">Local System (LS) - Already Had Access.</span></span>  
  
    - <span data-ttu-id="dc45d-174">Built-In Users - 向其授予访问权的组。</span><span class="sxs-lookup"><span data-stu-id="dc45d-174">Built-In Users - The group to grant access to.</span></span>  
  
#### <a name="to-grant-provider-registration-access"></a><span data-ttu-id="dc45d-175">向提供程序授予注册访问权</span><span class="sxs-lookup"><span data-stu-id="dc45d-175">To grant provider registration access</span></span>  
  
1. <span data-ttu-id="dc45d-176">运行下面的 PowerShell 脚本。</span><span class="sxs-lookup"><span data-stu-id="dc45d-176">Run the following PowerShell script.</span></span>  
  
    ```powershell  
    write-host ""  
    write-host "Granting WCF provider registration access to built in users group"  
    write-host ""  
    # Set security on ServiceModel provider  
    $provider = get-WmiObject -namespace "root\servicemodel" __Win32Provider  
  
    write-host "Previous ACL: "$provider.SecurityDescriptor  
    $result = $provider.SecurityDescriptor = "O:BUG:BUD:(A;;0x1;;;BA)(A;;0x1;;;NS)(A;;0x1;;;LS)(A;;0x1;;;BU)"  
  
    # Commit the changes and display it to the console  
    $result = $provider.Put()  
    write-host "New ACL:      "$provider.SecurityDescriptor  
    write-host ""  
    ```  
  
### <a name="granting-access-to-arbitrary-users-or-groups"></a><span data-ttu-id="dc45d-177">向任意用户或组授予访问权</span><span class="sxs-lookup"><span data-stu-id="dc45d-177">Granting Access to Arbitrary Users or Groups</span></span>  

 <span data-ttu-id="dc45d-178">本节中的示例向所有本地用户授予 WMI 提供程序注册特权。</span><span class="sxs-lookup"><span data-stu-id="dc45d-178">The example in this section grants WMI Provider registration privileges to all local users.</span></span> <span data-ttu-id="dc45d-179">如果要向不是内置的用户或组授予访问权限，则必须获取该用户或组的安全标识符 (SID) 。</span><span class="sxs-lookup"><span data-stu-id="dc45d-179">If you want to grant access to a user or group that is not built in, then you must obtain that user or group's Security Identifier (SID).</span></span> <span data-ttu-id="dc45d-180">获取任意用户的 SID 没有简单方法。</span><span class="sxs-lookup"><span data-stu-id="dc45d-180">There is no simple way to get the SID for an arbitrary user.</span></span> <span data-ttu-id="dc45d-181">一个方法是以所需的用户身份登录，然后发出以下 shell 命令。</span><span class="sxs-lookup"><span data-stu-id="dc45d-181">One method is to log on as the desired user and then issue the following shell command.</span></span>  
  
```console
Whoami /user  
```  
  
<span data-ttu-id="dc45d-182">有关详细信息，请参阅众所周知的 [sid](/troubleshoot/windows-server/identity/security-identifiers-in-windows)。</span><span class="sxs-lookup"><span data-stu-id="dc45d-182">For more information, see [Well Known SIDs](/troubleshoot/windows-server/identity/security-identifiers-in-windows).</span></span>  
  
## <a name="accessing-remote-wmi-object-instances"></a><span data-ttu-id="dc45d-183">访问远程 WMI 对象实例</span><span class="sxs-lookup"><span data-stu-id="dc45d-183">Accessing Remote WMI Object Instances</span></span>  

 <span data-ttu-id="dc45d-184">如果需要访问远程计算机上的 WCF WMI 实例，则必须在用于访问的工具上启用数据包隐私。</span><span class="sxs-lookup"><span data-stu-id="dc45d-184">If you need to access WCF WMI instances on a remote machine, you must enable packet privacy on the tools that you use for access.</span></span> <span data-ttu-id="dc45d-185">以下部分描述如何通过使用 WMI CIM Studio、Windows Management Instrumentation 测试器以及 .NET SDK 2.0 实现这些目标。</span><span class="sxs-lookup"><span data-stu-id="dc45d-185">The following section describes how to achieve these using the WMI CIM Studio, Windows Management Instrumentation Tester, as well as .NET SDK 2.0.</span></span>  
  
### <a name="wmi-cim-studio"></a><span data-ttu-id="dc45d-186">WMI CIM Studio</span><span class="sxs-lookup"><span data-stu-id="dc45d-186">WMI CIM Studio</span></span>

<span data-ttu-id="dc45d-187">如果已安装 WMI 管理工具，则可以使用 WMI CIM Studio 访问 WMI 实例。</span><span class="sxs-lookup"><span data-stu-id="dc45d-187">If you've installed WMI Administrative Tools, you can use the WMI CIM Studio to access WMI instances.</span></span> <span data-ttu-id="dc45d-188">这些工具位于以下文件夹中：</span><span class="sxs-lookup"><span data-stu-id="dc45d-188">The tools are in the following folder:</span></span>
  
<span data-ttu-id="dc45d-189">*%windir%\Program Files\wmi tools\ 工具\\*</span><span class="sxs-lookup"><span data-stu-id="dc45d-189">*%windir%\Program Files\WMI Tools\\*</span></span>
  
1. <span data-ttu-id="dc45d-190">在 " **连接到命名空间：** " 窗口中，键入 **root\ServiceModel** ，然后单击 **"确定"。**</span><span class="sxs-lookup"><span data-stu-id="dc45d-190">In the **Connect to namespace:** window, type **root\ServiceModel** and click **OK.**</span></span>  
  
2. <span data-ttu-id="dc45d-191">在 " **WMI CIM Studio 登录** " 窗口中，单击 " **选项" >>** "按钮展开该窗口。</span><span class="sxs-lookup"><span data-stu-id="dc45d-191">In the **WMI CIM Studio Login** window, click the **Options >>** button to expand the window.</span></span> <span data-ttu-id="dc45d-192">为 "**身份验证级别**" 选择 "**数据包隐私**"，然后单击 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="dc45d-192">Select **Packet privacy** for **Authentication level**, and click **OK**.</span></span>  
  
### <a name="windows-management-instrumentation-tester"></a><span data-ttu-id="dc45d-193">Windows Management Instrumentation 测试器</span><span class="sxs-lookup"><span data-stu-id="dc45d-193">Windows Management Instrumentation Tester</span></span>  

 <span data-ttu-id="dc45d-194">此工具由 Windows 安装。</span><span class="sxs-lookup"><span data-stu-id="dc45d-194">This tool is installed by Windows.</span></span> <span data-ttu-id="dc45d-195">若要运行它，请通过在 "**启动/运行**" 对话框中键入 **cmd.exe** 来启动命令控制台，然后单击 **"确定"**。</span><span class="sxs-lookup"><span data-stu-id="dc45d-195">To run it, launch a command console by typing **cmd.exe** in the **Start/Run** dialog box and click **OK**.</span></span> <span data-ttu-id="dc45d-196">然后，在命令窗口中键入 **wbemtest.exe** 。</span><span class="sxs-lookup"><span data-stu-id="dc45d-196">Then, type **wbemtest.exe** in the command window.</span></span> <span data-ttu-id="dc45d-197">Windows Management Instrumentation 测试器工具随即启动。</span><span class="sxs-lookup"><span data-stu-id="dc45d-197">The Windows Management Instrumentation Tester tool is then launched.</span></span>  
  
1. <span data-ttu-id="dc45d-198">单击窗口右上角的 " **连接** " 按钮。</span><span class="sxs-lookup"><span data-stu-id="dc45d-198">Click the **Connect** button on the top right corner of the window.</span></span>  
  
2. <span data-ttu-id="dc45d-199">在新窗口中，为 "**命名空间**" 字段输入 **root\ServiceModel** ，并为 "**身份验证级别**" 选择 "**数据包隐私**"。</span><span class="sxs-lookup"><span data-stu-id="dc45d-199">In the new window, enter **root\ServiceModel** for the **Namespace** field, and select **Packet privacy** for **Authentication level**.</span></span> <span data-ttu-id="dc45d-200">单击“连接”  。</span><span class="sxs-lookup"><span data-stu-id="dc45d-200">Click **Connect**.</span></span>  
  
### <a name="using-managed-code"></a><span data-ttu-id="dc45d-201">使用托管代码</span><span class="sxs-lookup"><span data-stu-id="dc45d-201">Using Managed Code</span></span>  

 <span data-ttu-id="dc45d-202">您还可以通过使用由 <xref:System.Management> 命名空间提供的类以编程方式访问远程 WMI 实例。</span><span class="sxs-lookup"><span data-stu-id="dc45d-202">You can also access remote WMI instances programmatically by using classes provided by the <xref:System.Management> namespace.</span></span> <span data-ttu-id="dc45d-203">下面的代码示例演示如何实现此操作。</span><span class="sxs-lookup"><span data-stu-id="dc45d-203">The following code sample demonstrates how to do this.</span></span>  
  
```csharp
String wcfNamespace = $@"\\{this.serviceMachineName}\Root\ServiceModel");
  
ConnectionOptions connection = new ConnectionOptions();  
connection.Authentication = AuthenticationLevel.PacketPrivacy;  
ManagementScope scope = new ManagementScope(this.wcfNamespace, connection);  
```
