---
author: dotpaul
ms.author: paulming
ms.date: 05/01/2019
ms.topic: include
ms.openlocfilehash: cf944ae9ca200d34a1f0f32e68bf3aee73a9500a
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590083"
---
- <span data-ttu-id="f5ea9-101">如果可能，请改用安全的序列化程序，并且不允许攻击者指定要反序列化的任意类型。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-101">If possible, use a secure serializer instead, and **don't allow an attacker to specify an arbitrary type to deserialize**.</span></span> <span data-ttu-id="f5ea9-102">一些更安全的序列化程序包括：</span><span class="sxs-lookup"><span data-stu-id="f5ea9-102">Some safer serializers include:</span></span>

  - <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>
  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>
  - <span data-ttu-id="f5ea9-103"><xref:System.Web.Script.Serialization.JavaScriptSerializer?displayProperty=nameWithType> - 请勿使用 <xref:System.Web.Script.Serialization.SimpleTypeResolver?displayProperty=nameWithType>。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-103"><xref:System.Web.Script.Serialization.JavaScriptSerializer?displayProperty=nameWithType> - Never use <xref:System.Web.Script.Serialization.SimpleTypeResolver?displayProperty=nameWithType>.</span></span> <span data-ttu-id="f5ea9-104">如果必须使用类型解析程序，请将反序列化的类型限制为预期列表。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-104">If you must use a type resolver, restrict deserialized types to an expected list.</span></span>
  - <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>
  - <span data-ttu-id="f5ea9-105">Newtonsoft Json.NET - 使用 TypeNameHandling.None。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-105">Newtonsoft Json.NET - Use TypeNameHandling.None.</span></span> <span data-ttu-id="f5ea9-106">如果必须为 TypeNameHandling 使用其他值，请将反序列化的类型限制为具有自定义 ISerializationBinder 的预期列表。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-106">If you must use another value for TypeNameHandling, restrict deserialized types to an expected list with a custom ISerializationBinder.</span></span>
  - <span data-ttu-id="f5ea9-107">协议缓冲区</span><span class="sxs-lookup"><span data-stu-id="f5ea9-107">Protocol Buffers</span></span>

- <span data-ttu-id="f5ea9-108">使序列化的数据免被篡改。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-108">Make the serialized data tamper-proof.</span></span> <span data-ttu-id="f5ea9-109">序列化后，对序列化的数据进行加密签名。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-109">After serialization, cryptographically sign the serialized data.</span></span> <span data-ttu-id="f5ea9-110">在反序列化之前，验证加密签名。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-110">Before deserialization, validate the cryptographic signature.</span></span> <span data-ttu-id="f5ea9-111">保护加密密钥不被泄露，并针对密钥轮换进行设计。</span><span class="sxs-lookup"><span data-stu-id="f5ea9-111">Protect the cryptographic key from being disclosed and design for key rotations.</span></span>
