---
title: NTLM 和 Kerberos 身份验证
description: 了解 .NET Framework 应用程序的默认 NTLM 身份验证和 Kerberos 身份验证的工作原理，并了解非默认的 NTLM 身份验证。
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- authentication [.NET Framework], NTLM
- authentication [.NET Framework], Kerberos
- user authentication, Kerberos
- user authentication, NTLM
- Kerberos authentication
- receiving data, authentication
- NTLM authentication
- Internet, authentication
- client authentication, Kerberos
- sending data, authentication
- network resources, authentication
- classes [.NET Framework], authentication
- client authentication, NTLM
ms.assetid: 9ef65560-f596-4469-bcce-f4d5407b55cd
ms.openlocfilehash: 92d9175f96fa54ccc3514ed479755f1318452bbc
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494072"
---
# <a name="ntlm-and-kerberos-authentication"></a><span data-ttu-id="83a9f-103">NTLM 和 Kerberos 身份验证</span><span class="sxs-lookup"><span data-stu-id="83a9f-103">NTLM and Kerberos Authentication</span></span>

<span data-ttu-id="83a9f-104">默认 NTLM 身份验证和 Kerberos 身份验证使用与调用应用程序关联的 Microsoft Windows 用户凭据来尝试通过服务器进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="83a9f-104">Default NTLM authentication and Kerberos authentication use the Microsoft Windows user credentials associated with the calling application to attempt authentication with the server.</span></span> <span data-ttu-id="83a9f-105">使用非默认 NTLM 身份验证时，应用程序会将认证类型设置为 NTLM，并使用 <xref:System.Net.NetworkCredential> 对象将用户名、密码和域传递给主机，如下例所示。</span><span class="sxs-lookup"><span data-stu-id="83a9f-105">When using non-default NTLM authentication, the application sets the authentication type to NTLM and uses a <xref:System.Net.NetworkCredential> object to pass the user name, password, and domain to the host, as shown in the following example.</span></span>  
  
```vb  
Dim MyURI As String = "http://www.contoso.com/"  
Dim WReq As WebRequest = WebRequest.Create(MyURI)  
WReq.Credentials = _  
    New NetworkCredential(UserName, SecurelyStoredPassword, Domain)  
```  
  
```csharp  
String MyURI = "http://www.contoso.com/";  
WebRequest WReq = WebRequest.Create (MyURI);  
WReq.Credentials =
    new NetworkCredential(UserName, SecurelyStoredPassword, Domain);  
```  
  
 <span data-ttu-id="83a9f-106">需要使用应用程序用户凭据连接到 Internet 服务的应用程序可以使用用户默认凭据进行此操作，如以下示例所示。</span><span class="sxs-lookup"><span data-stu-id="83a9f-106">Applications that need to connect to Internet services using the credentials of the application user can do so with the user's default credentials, as shown in the following example.</span></span>  
  
```vb  
Dim MyURI As String = "http://www.contoso.com/"  
Dim WReq As WebRequest = WebRequest.Create(MyURI)  
WReq.Credentials = CredentialCache.DefaultCredentials  
```  
  
```csharp  
String MyURI = "http://www.contoso.com/";  
WebRequest WReq = WebRequest.Create (MyURI);  
WReq.Credentials = CredentialCache.DefaultCredentials;  
```  
  
 <span data-ttu-id="83a9f-107">协商身份验证模块确定远程服务器使用 NTLM 身份验证还是 Kerberos 身份验证，并发送相应响应。</span><span class="sxs-lookup"><span data-stu-id="83a9f-107">The negotiate authentication module determines whether the remote server is using NTLM or Kerberos authentication, and sends the appropriate response.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="83a9f-108">无法通过代理服务器进行 NTLM 身份验证。</span><span class="sxs-lookup"><span data-stu-id="83a9f-108">NTLM authentication does not work through a proxy server.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="83a9f-109">请参阅</span><span class="sxs-lookup"><span data-stu-id="83a9f-109">See also</span></span>

- [<span data-ttu-id="83a9f-110">基本和摘要式身份验证</span><span class="sxs-lookup"><span data-stu-id="83a9f-110">Basic and Digest Authentication</span></span>](basic-and-digest-authentication.md)
- [<span data-ttu-id="83a9f-111">Internet 身份验证</span><span class="sxs-lookup"><span data-stu-id="83a9f-111">Internet Authentication</span></span>](internet-authentication.md)
