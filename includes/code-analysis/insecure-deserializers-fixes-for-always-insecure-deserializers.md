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
- 如果可能，请改用安全的序列化程序，并且不允许攻击者指定要反序列化的任意类型。 一些更安全的序列化程序包括：

  - <xref:System.Runtime.Serialization.DataContractSerializer?displayProperty=nameWithType>
  - <xref:System.Runtime.Serialization.Json.DataContractJsonSerializer?displayProperty=nameWithType>
  - <xref:System.Web.Script.Serialization.JavaScriptSerializer?displayProperty=nameWithType> - 请勿使用 <xref:System.Web.Script.Serialization.SimpleTypeResolver?displayProperty=nameWithType>。 如果必须使用类型解析程序，请将反序列化的类型限制为预期列表。
  - <xref:System.Xml.Serialization.XmlSerializer?displayProperty=nameWithType>
  - Newtonsoft Json.NET - 使用 TypeNameHandling.None。 如果必须为 TypeNameHandling 使用其他值，请将反序列化的类型限制为具有自定义 ISerializationBinder 的预期列表。
  - 协议缓冲区

- 使序列化的数据免被篡改。 序列化后，对序列化的数据进行加密签名。 在反序列化之前，验证加密签名。 保护加密密钥不被泄露，并针对密钥轮换进行设计。
