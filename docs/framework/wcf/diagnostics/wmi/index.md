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
# <a name="using-windows-management-instrumentation-for-diagnostics"></a>使用 Windows Management Instrumentation 进行诊断

Windows Communication Foundation (WCF) 会通过 WCF Windows Management Instrumentation (WMI) 提供程序，在运行时公开服务的检测数据。  
  
## <a name="enabling-wmi"></a>启用 WMI  

 WMI 是 Microsoft 基于 Web 的企业管理 (WBEM) 标准的实现。 有关 WMI SDK 的详细信息，请参阅 [Windows Management Instrumentation](/windows/desktop/WmiSdk/wmi-start-page)。 WBEM 是有关应用程序如何向外部管理工具公开管理规范的行业标准。  
  
 WMI 提供程序是一个在运行时通过 WBEM 兼容接口公开规范的组件。 它由一组包含属性/值对的 WMI 对象组成。 这些对可以是多个简单类型。 管理工具可以在运行时通过接口连接至服务。 WCF 公开服务的属性，如地址、绑定、行为和侦听器。  
  
 您可以在应用程序的配置文件中激活内置 WMI 提供程序。 此操作通过 `wmiProviderEnabled` 部分中的属性完成 [\<diagnostics>](../../../configure-apps/file-schema/wcf/diagnostics.md) [\<system.serviceModel>](../../../configure-apps/file-schema/wcf/system-servicemodel.md) ，如下面的示例配置中所示。  
  
```xml  
<system.serviceModel>  
    …  
    <diagnostics wmiProviderEnabled="true" />  
    …  
</system.serviceModel>  
```  
  
 此配置项公开 WMI 接口。 现在，您可以通过此接口连接管理应用程序并访问应用程序的管理规范。  
  
## <a name="accessing-wmi-data"></a>访问 WMI 数据  

 可以采用多种不同方式访问 WMI 数据。 Microsoft 为脚本、Visual Basic 应用程序、c + + 应用程序和 .NET Framework 提供 WMI Api。 有关详细信息，请参阅 [使用 WMI](/windows/win32/wmisdk/using-wmi)。  
  
> [!CAUTION]
> 如果您使用 .NET Framework 提供的方法以编程方式访问 WMI 数据，您应注意此类方法可能会在建立连接时引发异常。 连接并不是在构建 <xref:System.Management.ManagementObject> 实例的过程中建立的，而是在涉及实际数据交换的第一次请求时建立的。 因此，您应该使用 `try..catch` 块来捕捉可能的异常。  
  
 您可以在 WMI 中更改 `System.ServiceModel` 跟踪源的跟踪和消息日志记录级别以及消息日志记录选项。 为此，可以访问 [AppDomainInfo](appdomaininfo.md) 实例，该实例将公开下列布尔属性： `LogMessagesAtServiceLevel` 、 `LogMessagesAtTransportLevel` 、 `LogMalformedMessages` 和 `TraceLevel` 。 因此，如果为消息日志记录配置了跟踪侦听器，但是在配置中将这些选项设置为 `false`，那么可以在以后运行应用程序时将它们更改为 `true`。 这将在运行时有效地启用消息日志记录。 同样，如果在配置文件中启用了消息日志记录，可以在运行时使用 WMI 将其禁用。  
  
 您应该注意如果没有用于消息日志记录的消息日志记录跟踪侦听器，或者配置文件中没有指定用于跟踪的 `System.ServiceModel` 跟踪侦听器，您所执行的任何更改都将无效，即使已被 WMI 接受的更改也不例外。 有关正确设置各个侦听器的详细信息，请参阅 [配置消息日志记录](../configuring-message-logging.md) 和 [配置跟踪](../tracing/configuring-tracing.md)。 应用程序启动时，由配置指定的所有其他跟踪源的跟踪级别均有效，而且不能更改。  
  
 WCF 公开 `GetOperationCounterInstanceName` 用于脚本编写的方法。 如果您为其提供了一个操作名称，此方法将返回一个性能计数器实例名称。 但是，它不会验证您的输入。 因此，如果您提供了一个不正确的操作名称，将返回不正确的计数器名称。  
  
 `OutgoingChannel` `Service` 如果未在方法中创建目标服务的 WCF 客户端，则实例的属性不会对服务打开的用于连接到其他服务的通道计数 `Service` 。  
  
 **警告** WMI 仅支持 <xref:System.TimeSpan> 最多3个小数位数的值。 例如，如果您的服务将其中一个属性设置为 <xref:System.TimeSpan.MaxValue>，当通过 WMI 查看时，它的值将保留 3 位小数。  
  
## <a name="security"></a>安全性  

 因为 WCF WMI 提供程序允许发现环境中的服务，所以，您应该非常小心地授予对该服务的访问权限。 如果您放宽了默认的仅管理员访问，就可能允许不完全受信任方访问您的环境中的敏感数据。 特别是，如果您放宽了对远程 WMI 访问的权限，可能会发生洪泛攻击。 如果某个进程被大量 WMI 请求溢满，其性能将有所下降。  
  
 此外，如果您放宽了对 MOF 文件的访问权限，不完全受信任方可以控制 WMI 的行为并更改加载到 WMI 架构中的对象。 例如，不完全受信任方可以移除字段，这样，管理员将无法看到关键数据，或者未填充或导致异常的字段将添加到文件中。  
  
 默认情况下，WCF WMI 提供程序为管理员授予 "执行方法"、"提供程序写入" 和 "启用帐户" 权限，并为 ASP.NET、本地服务和网络服务授予 "启用帐户" 权限。 特别是在非 Windows Vista 平台上，ASP.NET 帐户对 WMI 空间命名空间具有读取访问权限。 如果您不希望对特定用户组授予这些特权，就应该停用 WMI 提供程序（默认状态为禁用），或者是禁用对特定用户组的访问。  
  
 此外，当您尝试通过配置启用 WMI 时，WMI 可能会因用户特权不足而无法启用。 但是，并不会将任何事件写入事件日志用于记录此次失败。  
  
 若要修改用户特权级别，请使用下列步骤。  
  
1. 单击 "开始"，然后单击 "运行"，然后键入 **compmgmt.msc**。  
  
2. 右键单击 " **服务和应用程序/WMI 控件** " 以选择 " **属性**"。  
  
3. 选择 " **安全** " 选项卡，然后导航到 **根/** 空间命名空间。 单击 " **安全** " 按钮。  
  
4. 选择要控制其访问权限的特定组或用户，并使用 " **允许** " 或 " **拒绝** " 复选框配置权限。  
  
## <a name="granting-wcf-wmi-registration-permissions-to-additional-users"></a>向其他用户授予 WCF WMI 注册权限  

 WCF 将管理数据公开到 WMI。 它通过承载进程内 WMI 提供程序（有时称为 "分离的提供程序"）来实现此目的。 对于要公开的管理数据，注册此提供程序的帐户必须具有适当的权限。 在 Windows 中，默认情况下只有一小组特权帐户可以注册分离的提供程序。 这是一个问题，因为用户通常要从不在默认集中的帐户下运行的 WCF 服务公开 WMI 数据。  
  
 若要提供此访问权，管理员必须按以下顺序向其他帐户授予以下权限：  
  
1. 用于访问 WCF WMI 命名空间的权限。  
  
2. 用于注册 WCF 分离 WMI 提供程序的权限。  
  
#### <a name="to-grant-wmi-namespace-access-permission"></a>授予 WMI 命名空间访问权限  
  
1. 运行下面的 PowerShell 脚本。  
  
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
  
     此 PowerShell 脚本使用安全描述符定义语言 (SDDL) 授予 Built-In 用户组访问 "root/用户" WMI 命名空间的权限。 它指定了以下 ACL：  
  
    - 内置管理员 (BA) - 已经具有访问权。  
  
    - 网络服务 (NS) - 已经具有访问权。  
  
    - 本地系统 (LS) - 已经具有访问权。  
  
    - Built-In Users - 向其授予访问权的组。  
  
#### <a name="to-grant-provider-registration-access"></a>向提供程序授予注册访问权  
  
1. 运行下面的 PowerShell 脚本。  
  
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
  
### <a name="granting-access-to-arbitrary-users-or-groups"></a>向任意用户或组授予访问权  

 本节中的示例向所有本地用户授予 WMI 提供程序注册特权。 如果要向不是内置的用户或组授予访问权限，则必须获取该用户或组的安全标识符 (SID) 。 获取任意用户的 SID 没有简单方法。 一个方法是以所需的用户身份登录，然后发出以下 shell 命令。  
  
```console
Whoami /user  
```  
  
有关详细信息，请参阅众所周知的 [sid](/troubleshoot/windows-server/identity/security-identifiers-in-windows)。  
  
## <a name="accessing-remote-wmi-object-instances"></a>访问远程 WMI 对象实例  

 如果需要访问远程计算机上的 WCF WMI 实例，则必须在用于访问的工具上启用数据包隐私。 以下部分描述如何通过使用 WMI CIM Studio、Windows Management Instrumentation 测试器以及 .NET SDK 2.0 实现这些目标。  
  
### <a name="wmi-cim-studio"></a>WMI CIM Studio

如果已安装 WMI 管理工具，则可以使用 WMI CIM Studio 访问 WMI 实例。 这些工具位于以下文件夹中：
  
*%windir%\Program Files\wmi tools\ 工具\\*
  
1. 在 " **连接到命名空间：** " 窗口中，键入 **root\ServiceModel** ，然后单击 **"确定"。**  
  
2. 在 " **WMI CIM Studio 登录** " 窗口中，单击 " **选项" >>** "按钮展开该窗口。 为 "**身份验证级别**" 选择 "**数据包隐私**"，然后单击 **"确定"**。  
  
### <a name="windows-management-instrumentation-tester"></a>Windows Management Instrumentation 测试器  

 此工具由 Windows 安装。 若要运行它，请通过在 "**启动/运行**" 对话框中键入 **cmd.exe** 来启动命令控制台，然后单击 **"确定"**。 然后，在命令窗口中键入 **wbemtest.exe** 。 Windows Management Instrumentation 测试器工具随即启动。  
  
1. 单击窗口右上角的 " **连接** " 按钮。  
  
2. 在新窗口中，为 "**命名空间**" 字段输入 **root\ServiceModel** ，并为 "**身份验证级别**" 选择 "**数据包隐私**"。 单击“连接”  。  
  
### <a name="using-managed-code"></a>使用托管代码  

 您还可以通过使用由 <xref:System.Management> 命名空间提供的类以编程方式访问远程 WMI 实例。 下面的代码示例演示如何实现此操作。  
  
```csharp
String wcfNamespace = $@"\\{this.serviceMachineName}\Root\ServiceModel");
  
ConnectionOptions connection = new ConnectionOptions();  
connection.Authentication = AuthenticationLevel.PacketPrivacy;  
ManagementScope scope = new ManagementScope(this.wcfNamespace, connection);  
```
