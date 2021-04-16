---
title: 安全规则（代码分析）
description: 了解代码分析安全规则。
ms.date: 10/02/2019
ms.topic: reference
f1_keywords:
- vs.codeanalysis.securityrules
helpviewer_keywords:
- security [Visual Studio ALM], Enterprise Templates
- security rules
- managed code analysis rules, security rules
- rules, security
author: gewarren
ms.author: gewarren
ms.openlocfilehash: 861827662a771ec7cc1827cdd8125be6c05bf05c
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "99719716"
---
# <a name="security-rules"></a>安全规则

安全规则可实现更安全的库和应用程序。 这些规则有助于防止程序中出现安全漏洞。 如果禁用其中任何规则，你应该在代码中清除标记原因，并通知开发项目的指定安全负责人。

## <a name="in-this-section"></a>在本节中

|规则|描述|
|----------|-----------------|
|[CA2100:检查 SQL 查询是否存在安全漏洞](ca2100.md)|一个方法使用按该方法的字符串参数生成的字符串设置 System.Data.IDbCommand.CommandText 属性。 此规则假定字符串参数中包含用户输入。 基于用户输入生成的 SQL 命令字符串易于受到 SQL 注入式攻击。|
|[CA2109:检查可见的事件处理程序](ca2109.md)|检测到公共事件处理方法或受保护事件处理方法。 除非绝对必要，否则不应公开事件处理方法。|
|[CA2119:密封满足私有接口的方法](ca2119.md)|可继承的公共类型为 internal（在 Visual Basic 中为 Friend）接口提供可重写的方法实现。 若要修复与此规则的冲突，请禁止方法在程序集外重写。|
|[CA2153：避免处理损坏状态异常](ca2153.md)|[损坏状态异常 (CSE)](/archive/msdn-magazine/2009/february/clr-inside-out-handling-corrupted-state-exceptions) 指示进程中存在内存损坏。 如果攻击者可以将攻击放置到损坏的内存区域，则捕获它们（而非允许进程崩溃）可能导致安全漏洞。|
|[CA2300：请勿使用不安全的反序列化程序 BinaryFormatte](ca2300.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2301：在未先设置 BinaryFormatter.Binder 的情况下，请不要调用 BinaryFormatter.Deserialize](ca2301.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2302：在调用 BinaryFormatter.Deserialize 之前，确保设置 BinaryFormatter.Binder](ca2302.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2305：请勿使用不安全的反序列化程序 LosFormatter](ca2305.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2310：请勿使用不安全的反序列化程序 NetDataContractSerializer](ca2310.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2311：在未先设置 NetDataContractSerializer.Binder 的情况下，请不要反序列化](ca2311.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2312：确保在反序列化之前设置 NetDataContractSerializer.Binder](ca2312.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2315：请勿使用不安全的反序列化程序 ObjectStateFormatter](ca2315.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2321：请勿使用 SimpleTypeResolver 对 JavaScriptSerializer 进行反序列化](ca2321.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2322：确保在反序列化之前没有使用 SimpleTypeResolver 初始化 JavaScriptSerializer](ca2322.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2326：请勿使用 None 以外的 TypeNameHandling 值](ca2326.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2327：不要使用不安全的 JsonSerializerSettings](ca2327.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2328：确保 JsonSerializerSettings 是安全的](ca2328.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2329：不要使用不安全的配置反序列化 JsonSerializer](ca2329.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2330：在反序列化时确保 JsonSerializer 具有安全配置](ca2330.md)|反序列化不受信任的数据时，会对不安全的反序列化程序造成风险。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。|
|[CA2350:确保 DataTable.ReadXml() 的输入受信任](ca2350.md)|对包含不受信任的输入的 <xref:System.Data.DataTable> 执行反序列化时，攻击者可能通过创建恶意输入实施拒绝服务攻击。 有可能存在未知的远程代码执行漏洞。|
|[CA2351:确保 DataSet.ReadXml() 的输入受信任](ca2351.md)|对包含不受信任的输入的 <xref:System.Data.DataSet> 执行反序列化时，攻击者可能通过创建恶意输入实施拒绝服务攻击。 有可能存在未知的远程代码执行漏洞。|
|[CA2352:可序列化类型中的不安全 DataSet 或 DataTable 容易受到远程代码执行攻击](ca2352.md)|带有 <xref:System.SerializableAttribute> 标记的类或结构包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 字段或属性，但不具有 <xref:System.CodeDom.Compiler.GeneratedCodeAttribute>。|
|[CA2353:可序列化类型中的不安全 DataSet 或 DataTable](ca2353.md)|使用 XML 序列化特性或数据协定特性进行了标记的类或结构包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 字段或属性。|
|[CA2354:反序列化对象图中的不安全 DataSet 或 DataTable 可能容易受到远程代码执行攻击](ca2354.md)|当使用序列化的 <xref:System.Runtime.Serialization.IFormatter?displayProperty=nameWithType> 进行反序列化时，且强制转换的类型的对象图可能包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 时。|
|[CA2355:反序列化对象图中的不安全 DataSet 或 DataTable](ca2355.md)|当强制转换的或指定的类型的对象图可能包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 类时，进行反序列化。|
|[CA2356：Web 反序列化的对象图中不安全的 DataSet 或 DataTable](ca2356.md)|带有 <xref:System.Web.Services.WebMethodAttribute?displayProperty=nameWithType> 或 <xref:System.ServiceModel.OperationContractAttribute?displayProperty=nameWithType> 的方法具有可能引用 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 的参数。|
|[CA2361：请确保包含 DataSet.ReadXml() 的自动生成的类没有与不受信任的数据一起使用](ca2361.md)|对包含不受信任的输入的 <xref:System.Data.DataSet> 执行反序列化时，攻击者可能通过创建恶意输入实施拒绝服务攻击。 有可能存在未知的远程代码执行漏洞。|
|[CA2362：自动生成的可序列化类型中不安全的数据集或数据表易受远程代码执行攻击](ca2362.md)|当反序列化具有 <xref:System.Runtime.Serialization.Formatters.Binary.BinaryFormatter> 的不受信任的输入且反序列化的对象图包含 <xref:System.Data.DataSet> 或 <xref:System.Data.DataTable> 时，攻击者可能创建执行远程代码执行攻击的恶意有效负载。|
|[CA3001：查看 SQL 注入漏洞的代码](ca3001.md)|使用不受信任的输入和 SQL 命令时，请注意防范 SQL 注入攻击。 SQL 注入攻击可以执行恶意的 SQL 命令，从而降低应用程序的安全性和完整性。|
|[CA3002：查看 XSS 漏洞的代码](ca3002.md)|在处理来自 Web 请求的不受信任的输入时，请注意防范跨站脚本 (XSS) 攻击。 XSS 攻击会将不受信任的输入注入原始 HTML 输出，使攻击者可以执行恶意脚本或恶意修改网页中的内容。|
|[CA3003:查看文件路径注入漏洞的代码](ca3003.md)|在处理来自 Web 请求的不受信任的输入时，请谨慎使用用户控制的输入指定文件路径。|
|[CA3004：查看信息泄露漏洞的代码](ca3004.md)|泄漏异常信息可让攻击者深入了解应用程序的内部机制，从而帮助攻击者找到其他漏洞并利用这些漏洞。|
|[CA3006：查看进程命令注入漏洞的代码](ca3006.md)|处理不受信任的输入时，请注意防范命令注入攻击。 命令注入攻击可在基础操作系统上执行恶意命令，从而降低服务器的安全和完整性。|
|[CA3007：查看公开重定向漏洞的代码](ca3007.md)|处理不受信任的输入时，请注意防范开放重定向漏洞。 攻击者可以利用开放重定向漏洞，使用你的网站提供合法 URL 的外观，但将毫不知情的访客重定向到钓鱼网页或其他恶意网页。|
|[CA3008：查看 XPath 注入漏洞的代码](ca3008.md)|处理不受信任的输入时，请注意防范 XPath 注入攻击。 使用不受信任的输入构造 XPath 查询可能会允许攻击者恶意控制查询，使其返回一个意外的结果，并可能泄漏查询的 XML 的内容。|
|[CA3009：查看 XML 注入漏洞的代码](ca3009.md)|处理不受信任的输入时，请注意防范 XML 注入攻击。|
|[CA3010：查看 XAML 注入漏洞的代码](ca3010.md)|处理不受信任的输入时，请注意防范 XAML 注入攻击。 XAML 是一种直接表示对象实例化和执行的标记语言。 这意味着 XAML 中创建的元素可以与系统资源（例如，网络访问和文件系统 IO）交互。|
|[CA3011：查看 DLL 注入漏洞的代码](ca3011.md)|处理不受信任的输入时，请谨慎加载不受信任的代码。 如果你的 Web 应用加载不受信任的代码，攻击者可能能够将恶意 DLL 注入到你的进程中，并执行恶意代码。|
|[CA3012：查看正则表达式注入漏洞的代码](ca3012.md)|处理不受信任的输入时，请注意防范正则表达式注入攻击。 攻击者可以使用正则表达式注入恶意修改正则表达式，让正则表达式匹配非预期结果，或者让正则表达式占用过多 CPU，从而形成拒绝服务攻击。|
|[CA3061：请勿按 URL 添加架构](ca3061.md)|请勿使用不安全的“添加”方法重载，因为这可能会导致危险的外部引用。|
|[CA3075:不安全的 DTD 处理](ca3075.md)|如果使用不安全的 DTDProcessing 实例或引用外部实体源，分析器可能会接受不受信任的输入并将敏感信息泄露给攻击者。|
|[CA3076:不安全的 XSLT 脚本执行](ca3076.md)|如果在 .NET 应用程序中不安全地执行可扩展样式表语言转换 (XSLT)，处理器可能会解析不受信任的 URI 引用，这种引用会把敏感信息泄露给攻击者，从而导致拒绝服务和跨站点攻击。|
|[CA3077:API 设计、XML 文档和 XML 文本读取器中的不安全处理](ca3077.md)|当设计派生自 XMLDocument 和 XMLTextReader 的 API 时，请注意 DtdProcessing。 当引用或解析外部实体源或设置 XML 中的不安全值时，使用不安全的 DTDProcessing 实例可能会导致信息泄露。|
|[CA3147:使用 ValidateAntiForgeryToken 标记谓词处理程序](ca3147.md)|设计 ASP.NET MVC 控制器时，请注意防范跨网站请求伪造攻击。 跨网站请求伪造攻击可将来自经过身份验证的用户的恶意请求发送到 ASP.NET MVC 控制器。|
|[CA5350:请勿使用弱加密算法](ca5350.md)|出于多种原因，现今使用弱加密算法和哈希函数，但不应使用它们来保证保密性或它们所保护的数据的完整性。 当此规则在代码中找到 TripleDES、SHA1、或 RIPEMD160 算法时，此规则将触发。|
|[CA5351：请勿使用已损坏的加密算法](ca5351.md)|损坏的加密算法不安全，强烈建议不要使用。 当此规则在代码中找到 MD5 哈希算法，或者 DES 或 RC2 加密算法时，此规则将触发。|
|[CA5358：请勿使用不安全的密码模式](ca5358.md)|请勿使用不安全的密码模式|
|[CA5359:请勿禁用证书验证](ca5359.md)|证书有助于对服务器的身份进行验证。 客户端应验证服务器证书，确保将请求发送到目标服务器。 如果 ServerCertificateValidationCallback 始终返回 `true`，那么任何证书都将通过验证。|
|[CA5360:在反序列化中不要调用危险的方法](ca5360.md)|不安全的反序列化是一种漏洞。当使用不受信任的数据来损害应用程序的逻辑，造成拒绝服务 (DoS) 攻击，或甚至在反序列化时任意执行代码，就会出现该漏洞。 应用程序对受其控制的不受信任的数据进行反序列化时，恶意用户很可能会滥用这些反序列化功能。 具体来说，就是在反序列化过程中调用危险方法。 如果攻击者成功执行不安全的反序列化攻击，就能实施更多攻击，如 DoS 攻击、绕过身份验证和执行远程代码。|
|[CA5361：不禁用较强加密的 SChannel 使用](ca5361.md)|将 `Switch.System.Net.DontEnableSchUseStrongCrypto` 设置为 `true` 会减弱传出的传输层安全性连接中使用的加密性。 较弱的加密性会泄露应用程序与服务器之间通信的机密性，使攻击者更易于窃听敏感数据。|
|[CA5362:反序列化对象图中存在潜在引用循环](ca5362.md)|反序列化不受信任的数据时，处理反序列化对象图的任何代码都需要在处理引用循环时不进入无限循环。 这包括反序列化回叫中的一部分代码和在反序列化完成后处理对象图的代码。 否则攻击者可能会利用带有包含引用循环的恶意数据执行拒绝服务攻击。|
|[CA5363：请勿禁用请求验证](ca5363.md)|请求验证是 ASP.NET 中的一项功能，可检查 HTTP 请求并确定这些请求是否包含可能导致跨站点脚本编写等注入攻击的潜在危险内容。|
|[CA5364：不使用已弃用的安全协议](ca5364.md)|传输层安全性 (TLS) 通常使用超文本传输协议安全 (HTTPS) 保障计算机之间的通信安全。 早期版本的 TLS 协议不如 TLS 1.2 和 TLS 1.3 安全，且更容易出现新的漏洞。 避免使用旧版本的协议，以便最大程度降低风险。|
|[CA5365:请勿禁用 HTTP 头检查](ca5365.md)|通过 HTTP 标头检查，可对在响应头中找到的回车符和换行符（\r 和 \n）进行编码。 此编码有助于避免注入攻击，这些注入攻击会攻击对标头包含的不受信数据进行回显的应用程序。|
|[CA5366:将 XmlReader 用于数据集读取 XML](ca5366.md)|使用 <xref:System.Data.DataSet> 读取包含不受信数据的 XML，可能会加载危险的外部引用，应使用具有安全解析程序或禁用了 DTD 处理的 <xref:System.Xml.XmlReader> 来限制这种行为。|
|[CA5367:请勿序列化具有 Pointer 字段的类型](ca5367.md)|此规则检查是否存在带有指针字段或属性的可序列化类。 无法进行序列化的成员可能是指针，例如使用 <xref:System.NonSerializedAttribute> 进行标记的静态成员或字段。|
|[CA5368:针对派生自 Page 的类设置 ViewStateUserKey](ca5368.md)|设置 <xref:System.Web.UI.Page.ViewStateUserKey> 属性有助于防止对应用程序的攻击，方法是允许你为各个用户的视图状态变量分配标识符，这样攻击者就无法使用变量生成攻击。 否则会出现“跨网站请求伪造”漏洞。|
|[CA5369：将 XmlReader 用于反序列化](ca5369.md)|处理不受信任的 DTD 和 XML 架构时可能会加载危险的外部引用，应使用具有安全解析程序或禁用了 DTD 和 XML 内联架构处理的 XmlReader 来限制这种行为。|
|[CA5370：将 XmlReader 用于验证读取器](ca5370.md)|处理不受信任的 DTD 和 XML 架构时可能会加载危险的外部引用。 此危险的加载行为可使用具有安全解析程序或者禁用了 DTD 和 XML 内联架构处理的 XmlReader 来进行限制。|
|[CA5371：将 XmlReader 用于架构读取](ca5371.md)|处理不受信任的 DTD 和 XML 架构时可能会加载危险的外部引用。 请使用具有安全解析程序或者禁用了 DTD 和 XML 内联架构处理的 XmlReader 对其进行限制。|
|[CA5372：将 XmlReader 用于 XPathDocument](ca5372.md)|处理来自不受信任的数据的 XML 时可能会加载危险的外部引用，可使用具有安全解析程序或禁用了 DTD 处理的 XmlReader 对其进行限制。|
|[CA5373：请勿使用已过时的密钥派生功能](ca5373.md)|此规则会检测对弱密钥派生方法 <xref:System.Security.Cryptography.PasswordDeriveBytes?displayProperty=fullName> 和 `Rfc2898DeriveBytes.CryptDeriveKey` 的调用。 <xref:System.Security.Cryptography.PasswordDeriveBytes?displayProperty=fullName> 使用了弱算法 PBKDF1。|
|[CA5374:请勿使用 XslTransform](ca5374.md)|此规则检查 <xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType> 是否在代码中进行了实例化。 <xref:System.Xml.Xsl.XslTransform?displayProperty=nameWithType> 现已过时且不应使用。|
|[CA5375:请勿使用帐户共享访问签名](ca5375.md)|帐户 SAS 可以委派对 blob 容器、表、队列和文件共享执行读取、写入和删除操作的访问权限，而这是服务 SAS 所不允许的。 但是它不支持容器级别的策略，并且其灵活性和对授予的权限的控制力更低。 一旦恶意用户获取它后，存储帐户的信息很容易泄露。|
|[CA5376:使用 SharedAccessProtocol HttpsOnly](ca5376.md)|SAS 是无法在 HTTP 上以纯文本形式传输的敏感数据。|
|[CA5377:使用容器级别访问策略](ca5377.md)|容器级别的访问策略可以随时修改或撤销。 它具有更高的灵活性，对授予的权限的控制力更强。|
|[CA5378：不禁用 ServicePointManagerSecurityProtocols](ca5378.md)|将 `DisableUsingServicePointManagerSecurityProtocols` 设置为 `true` 会将 Windows Communication Framework (WCF) 的传输层安全性 (TLS) 连接限制为使用 TLS 1.0。 该版本的 TLS 将被弃用。|
|[CA5379：确保密钥派生功能算法足够强](ca5379.md)|<xref:System.Security.Cryptography.Rfc2898DeriveBytes> 类默认使用 <xref:System.Security.Cryptography.HashAlgorithmName.SHA1> 算法。 应指定在 <xref:System.Security.Cryptography.HashAlgorithmName.SHA256> 或更高版本的构造函数的某些重载中使用哈希算法。 请注意，<xref:System.Security.Cryptography.Rfc2898DeriveBytes.HashAlgorithm> 属性只具有 `get` 访问器，而没有 `overriden` 修饰符。|
|[CA5380：请勿将证书添加到根存储中](ca5380.md)|此规则会对将证书添加到“受信任的根证书颁发机构”证书存储的代码进行检测。 默认情况下，“受信任的根证书颁发机构”证书存储配置有一组符合 Microsoft 根证书计划要求的公共 CA。|
|[CA5381：请确保证书未添加到根存储中](ca5381.md)|此规则会对可能将证书添加到“受信任的根证书颁发机构”证书存储的代码进行检测。 默认情况下，“受信任的根证书颁发机构”证书存储配置有一组符合 Microsoft 根证书计划要求的公共证书颁发机构 (CA)。|
|[CA5382:在 ASP.NET Core 中使用安全 Cookie](ca5382.md)|HTTPS 上可用的应用程序必须使用安全 Cookie，这会向浏览器指示，Cookie 只能使用传输层安全性 (TLS) 进行传输。|
|[CA5383:确保在 ASP.NET Core 中使用安全 Cookie](ca5383.md)|HTTPS 上可用的应用程序必须使用安全 Cookie，这会向浏览器指示，Cookie 只能使用传输层安全性 (TLS) 进行传输。|
|[CA5384:不使用数字签名算法(DSA)](ca5384.md)|DSA 是一种弱非对称加密算法。|
|[CA5385:设置具有足够密钥大小的 Rivest–Shamir–Adleman (RSA)算法](ca5385.md)|小于 2048 位的 RSA 密钥更容易受到暴力攻击。|
|[CA5386：避免对 SecurityProtocolType 值进行硬编码](ca5386.md)|传输层安全性 (TLS) 通常使用安全超文本传输协议 (HTTPS) 保障计算机之间的通信安全。 协议版本 TLS 1.0 和 TLS 1.1 已弃用，目前使用 TLS 1.2 和 TLS 1.3。 TLS 1.2 和 TLS 1.3 将来可能也会弃用。 要确保应用程序的安全性，请避免对协议版本进行硬编码，并且至少以 .NET Framework v4.7.1 为目标。|
|[CA5387:请勿使用迭代计数不足的弱密钥派生功能](ca5387.md)|此规则检查加密密钥是否由迭代计数小于 100,000 的 <xref:System.Security.Cryptography.Rfc2898DeriveBytes> 生成。 迭代计数较高有助于缓解尝试猜测已生成的加密密钥的字典攻击。|
|[CA5388:使用弱密钥派生功能时，请确保迭代计数足够大](ca5388.md)|此规则检查加密密钥是否由迭代计数可能小于 100,000 的 <xref:System.Security.Cryptography.Rfc2898DeriveBytes> 生成。 迭代计数较高有助于缓解尝试猜测已生成的加密密钥的字典攻击。|
|[CA5389：请勿将存档项的路径添加到目标文件系统路径中](ca5389.md)|文件路径可以是相对的，并且可能导致文件系统访问预期文件系统目标路径以外的内容，从而导致攻击者通过“布局和等待”技术恶意更改配置和执行远程代码。|
|[CA5390:请勿编码加密密钥](ca5390.md)|要成功使用对称算法，密钥必须只有发送方和接收方知道。 如果密钥是硬编码的，就容易被发现。 即使使用编译的二进制文件，恶意用户也容易将其提取出来。 私钥泄露后，密码文本可直接被解密并且不再受保护。|
|[CA5391:在 ASP.NET Core MVC 控制器中使用防伪造令牌](ca5391.md)|处理 `POST`、`PUT`、`PATCH` 或 `DELETE` 请求而不验证防伪造令牌可能易受到跨网站请求伪造攻击。 跨网站请求伪造攻击可将经过身份验证的用户的恶意请求发送到 ASP.NET Core MVC 控制器。|
|[CA5392:对 P/Invoke 使用 DefaultDllImportSearchPaths 属性](ca5392.md)|默认情况下，使用 <xref:System.Runtime.InteropServices.DllImportAttribute> 的 P/Invoke 函数会探测大量目录，包括要加载的库的当前工作目录。 这对于某些应用程序来说是一个安全隐患，会导致 DLL 劫持。|
|[CA5393:请勿使用不安全的 DllImportSearchPath 值](ca5393.md)|默认的 DLL 搜索目录和程序集目录中可能存在恶意 DLL。 或者根据应用程序运行的位置，应用程序的目录中可能存在恶意 DLL。|
|[CA5394:请勿使用不安全的随机性](ca5394.md)|如果使用加密较弱的伪随机数生成器，攻击者可以预测将要生成的安全敏感值。|
|[CA5395:缺少操作方法的 HttpVerb 属性](ca5395.md)|创建、编辑或以其它方式修改数据等所有操作方法都需要使用防伪特性来保护，以避免受跨网站请求伪造攻击的影响。 执行 GET 操作应是没有副作用且不会修改持久数据的安全操作。|
|[CA5396:将 HttpCookie 的 HttpOnly 设置为 true](ca5396.md)|请确保将安全敏感的 HTTP Cookie 标记为 HttpOnly，这是一个深度防御措施。 这表明 Web 浏览器应禁止脚本访问 Cookie。 注入恶意脚本是常见的窃取 Cookie 的方式。|
|[CA5397：不使用已弃用的 SslProtocols 值](ca5397.md)|传输层安全性 (TLS) 通常使用安全超文本传输协议 (HTTPS) 保障计算机之间的通信安全。 早期版本的 TLS 协议不如 TLS 1.2 和 TLS 1.3 安全，且更容易出现新的漏洞。 避免使用旧版本的协议，以便最大程度降低风险。|
|[CA5398：避免硬编码的 SslProtocols 值](ca5398.md)|传输层安全性 (TLS) 通常使用安全超文本传输协议 (HTTPS) 保障计算机之间的通信安全。 协议版本 TLS 1.0 和 TLS 1.1 已弃用，目前使用 TLS 1.2 和 TLS 1.3。 将来可能也会弃用 TLS 1.2 和 TLS 1.3。 要确保应用程序的安全性，请避免对协议版本进行硬编码。|
|[CA5399:绝对禁用 HttpClient 证书吊销列表检查](ca5399.md)|撤销的证书不再受信任。 攻击者可能使用它来传递某些恶意数据或窃取 HTTPS 通信中的敏感数据。|
|[CA5400:确保未禁用 HttpClient 证书吊销列表检查](ca5400.md)|撤销的证书不再受信任。 攻击者可能使用它来传递某些恶意数据或窃取 HTTPS 通信中的敏感数据。|
|[CA5401:不要将 CreateEncryptor 与非默认 IV 结合使用](ca5401.md)|对称加密应始终使用非可重复的初始化向量，以防止字典攻击。|
|[CA5402:将 CreateEncryptor 与默认 IV 结合使用](ca5402.md)|对称加密应始终使用非可重复的初始化向量，以防止字典攻击。|
|[CA5403：请勿硬编码证书](ca5403.md)|<xref:System.Security.Cryptography.X509Certificates.X509Certificate> 或 <xref:System.Security.Cryptography.X509Certificates.X509Certificate2> 构造函数的 `data` 或 `rawData` 参数是硬编码的。|
