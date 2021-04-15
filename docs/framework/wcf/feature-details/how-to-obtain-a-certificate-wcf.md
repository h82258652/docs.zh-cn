---
description: '了解详细信息：如何：获取 (WCF 的证书) '
title: 如何：获取证书 (WCF)
ms.date: 03/30/2017
helpviewer_keywords:
- certificates [WCF], obtaining
ms.assetid: d53762fd-15ea-42dc-b0ea-6a6597aa23f7
ms.openlocfilehash: dad44776819f9d026445689e072e4aa5059d54fb
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494800"
---
# <a name="how-to-obtain-a-certificate-wcf"></a><span data-ttu-id="4b942-103">如何：获取证书 (WCF)</span><span class="sxs-lookup"><span data-stu-id="4b942-103">How to: Obtain a Certificate (WCF)</span></span>

<span data-ttu-id="4b942-104">若要使用 Windows Communication Foundation 的任何 (WCF) 功能，只需获取证书即可。</span><span class="sxs-lookup"><span data-stu-id="4b942-104">To use any of the Windows Communication Foundation (WCF) features of that use X.509 certificates, you just first obtain certificates.</span></span>  
  
## <a name="obtain-an-x509-certificate"></a><span data-ttu-id="4b942-105">获取 x.509 证书</span><span class="sxs-lookup"><span data-stu-id="4b942-105">Obtain an X.509 certificate</span></span>  
  
<span data-ttu-id="4b942-106">选择以下选项之一：</span><span class="sxs-lookup"><span data-stu-id="4b942-106">Choose one of the following:</span></span>  
  
- <span data-ttu-id="4b942-107">从证书颁发机构（如 VeriSign Inc）购买证书。</span><span class="sxs-lookup"><span data-stu-id="4b942-107">Purchase a certificate from a certification authority, such as VeriSign, Inc.</span></span>  
  
- <span data-ttu-id="4b942-108">设置自己的证书服务，并让证书颁发机构为证书签名。</span><span class="sxs-lookup"><span data-stu-id="4b942-108">Set up your own certificate service and have a certification authority sign the certificates.</span></span> <span data-ttu-id="4b942-109">Windows Server 2003、Windows 2000 Server、Windows 2000 Server Datacenter 和 Windows 2000 Datacenter 服务器都包含支持公钥基础结构 (PKI) 的证书服务。</span><span class="sxs-lookup"><span data-stu-id="4b942-109">Windows Server 2003, Windows 2000 Server, Windows 2000 Server Datacenter, and Windows 2000 Datacenter Server all include certificate services that support public key infrastructure (PKI).</span></span> <span data-ttu-id="4b942-110">在 Windows Server 2008 中，使用 [Active Directory 证书服务](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731564(v=ws.10)) 角色来管理证书颁发机构。</span><span class="sxs-lookup"><span data-stu-id="4b942-110">In Windows Server 2008, use the [Active Directory Certificate Services](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc731564(v=ws.10)) role to manage a certification authority.</span></span>  
  
- <span data-ttu-id="4b942-111">设置自己的证书服务，但不对证书进行签名。</span><span class="sxs-lookup"><span data-stu-id="4b942-111">Set up your own certificate service and do not have the certificates signed.</span></span>  
  
> [!NOTE]
> <span data-ttu-id="4b942-112">无论采取哪种方法，包含 X.509 证书的 SOAP 请求的接收方都必须信任 X.509 证书。</span><span class="sxs-lookup"><span data-stu-id="4b942-112">Whichever approach you take, the recipient of the SOAP request that contains the X.509 certificate must trust the X.509 certificate.</span></span> <span data-ttu-id="4b942-113">这意味着证书链中的 X.509 证书或颁发者位于“受信任的人”证书存储区中，并且 X.509 证书不在“不受信任的证书”存储区中。</span><span class="sxs-lookup"><span data-stu-id="4b942-113">This means that the X.509 certificate or an issuer in the certificate chain is in the Trusted People certificate store and that the X.509 certificate is not in the Untrusted Certificates store.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="4b942-114">另请参阅</span><span class="sxs-lookup"><span data-stu-id="4b942-114">See also</span></span>

- [<span data-ttu-id="4b942-115">使用证书</span><span class="sxs-lookup"><span data-stu-id="4b942-115">Working with Certificates</span></span>](working-with-certificates.md)
- [<span data-ttu-id="4b942-116">如何：创建开发期间使用的临时证书</span><span class="sxs-lookup"><span data-stu-id="4b942-116">How to: Create Temporary Certificates for Use During Development</span></span>](how-to-create-temporary-certificates-for-use-during-development.md)
