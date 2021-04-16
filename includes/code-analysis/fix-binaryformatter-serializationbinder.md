---
ms.openlocfilehash: 557d811e2eeaf926cb958471d214fc23c50a25f2
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590080"
---
- 改为使用安全序列化程序，并且不允许攻击者指定要反序列化的任意类型。 有关详细信息，请参阅[首选替代方案](/dotnet/standard/serialization/binaryformatter-security-guide#preferred-alternatives)。
- 使序列化的数据免被篡改。 序列化后，对序列化的数据进行加密签名。 在反序列化之前，验证加密签名。 保护加密密钥不被泄露，并针对密钥轮换进行设计。
- 此选项使代码容易遭受拒绝服务攻击，以及将来可能会发生的远程代码执行攻击。 有关详细信息，请参阅 [BinaryFormatter 安全指南](/dotnet/standard/serialization/binaryformatter-security-guide)。 限制反序列化的类型。 实现自定义 <xref:System.Runtime.Serialization.SerializationBinder?displayProperty=nameWithType>。 在反序列化之前，请在所有代码路径中将 `Binder` 属性设置为自定义 <xref:System.Runtime.Serialization.SerializationBinder> 的实例。 在替代的 <xref:System.Runtime.Serialization.SerializationBinder.BindToType%2A> 方法中，如果类型不是预期类型，将引发异常以停止反序列化。
