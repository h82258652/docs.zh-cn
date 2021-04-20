---
title: 解密数据
description: 了解如何使用对称算法或非对称算法解密 .NET 中的数据。
ms.date: 04/19/2021
dev_langs:
- csharp
- vb
helpviewer_keywords:
- data [.NET], decryption
- symmetric decryption
- asymmetric decryption
- decryption
ms.assetid: 9b266b6c-a9b2-4d20-afd8-b3a0d8fd48a0
ms.openlocfilehash: 1b12e4250f3b2345afd05dbe3c257eb3e7277634
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740339"
---
# <a name="decrypting-data"></a>解密数据

解密是加密的反向操作。 对于私钥加密，必须知道用于加密数据的密钥和 IV。 对于公钥加密，必须知道公钥（如果使用了私钥来加密数据）或私钥（如果使用了公钥来加密数据）。

## <a name="symmetric-decryption"></a>对称解密

解密用对称算法加密的数据类似于用对称算法加密数据的过程。 <xref:System.Security.Cryptography.CryptoStream>类与 .net 提供的对称加密类一起使用，对从任何托管流对象中读取的数据进行解密。

下面的示例演示如何创建算法的默认实现类的新实例 <xref:System.Security.Cryptography.Aes> 。 实例用于对对象执行解密 <xref:System.Security.Cryptography.CryptoStream> 。 此示例首先创建实现类的新实例 <xref:System.Security.Cryptography.Aes> 。 它从托管流变量中读取初始化向量 (IV) 值 `fileStream` 。 接下来，它将实例化 <xref:System.Security.Cryptography.CryptoStream> 对象并将其初始化为 `fileStream` 实例的值。 将向 <xref:System.Security.Cryptography.SymmetricAlgorithm.CreateDecryptor%2A?displayProperty=nameWithType> 实例中的方法 <xref:System.Security.Cryptography.Aes> 传递 IV 值和用于加密的相同密钥。

```vb
Dim aes As Aes = Aes.Create()
Dim cryptStream As New CryptoStream(
    fileStream, aes.CreateDecryptor(key, iv), CryptoStreamMode.Read)
```

```csharp
Aes aes = Aes.Create();
CryptoStream cryptStream = new CryptoStream(
    fileStream, aes.CreateDecryptor(key, iv), CryptoStreamMode.Read);
```

下面的示例显示创建流、解密流、从流中读取和关闭流的整个过程。 将创建一个文件流对象，该对象读取名为 *TestData.txt* 的文件。 然后，使用 **CryptoStream** 类和 **Aes** 类对文件流进行解密。 此示例指定在对称加密示例中用于 [加密数据](encrypting-data.md)的键值。 它不会显示加密和传输这些值所需的代码。

:::code language="csharp" source="snippets/decrypting-data/csharp/aes-decrypt.cs":::
:::code language="vb" source="snippets/decrypting-data/vb/aes-decrypt.vb":::

前面的示例使用相同的密钥和对称加密示例中用于 [加密数据](encrypting-data.md)的算法。 它对该示例创建的 *TestData.txt* 文件进行解密，并在控制台上显示原始文本。

## <a name="asymmetric-decryption"></a>非对称解密

通常，一方（A 方）同时生成公钥和私钥，并将其存储在内存或加密密钥容器中。 然后 A 方将公钥发送到另一方（B 方）。 使用公钥时，参与方 B 会对数据进行加密，然后将数据发送回 A 方。接收到数据后，A 方使用对应的私钥将其解密。 A 方只有使用与 B 方用于加密数据的公钥相对应的私钥，解密才能成功。

有关如何将非对称密钥存储在安全加密密钥容器中以及随后如何获取非对称密钥的信息，请参阅 [How to: Store Asymmetric Keys in a Key Container](how-to-store-asymmetric-keys-in-a-key-container.md)。

下面的示例阐释如何对表示一个对称密钥和 IV 的两个字节数组进行解密。 有关如何以可方便地发送到第三方的格式从 <xref:System.Security.Cryptography.RSA> 对象提取非对称公钥的信息，请参阅 [Encrypting Data](encrypting-data.md)的托管流的值。

```vb
'Create a new instance of the RSA class.
Dim rsa As RSA = RSA.Create()

' Export the public key information and send it to a third party.
' Wait for the third party to encrypt some data and send it back.

'Decrypt the symmetric key and IV.
symmetricKey = rsa.Decrypt(encryptedSymmetricKey, RSAEncryptionPadding.Pkcs1)
symmetricIV = rsa.Decrypt(encryptedSymmetricIV, RSAEncryptionPadding.Pkcs1)
```

```csharp
//Create a new instance of the RSA class.
RSA rsa = RSA.Create();

// Export the public key information and send it to a third party.
// Wait for the third party to encrypt some data and send it back.

//Decrypt the symmetric key and IV.
symmetricKey = rsa.Decrypt(encryptedSymmetricKey, RSAEncryptionPadding.Pkcs1);
symmetricIV = rsa.Decrypt(encryptedSymmetricIV , RSAEncryptionPadding.Pkcs1);
```

## <a name="see-also"></a>请参阅

- [生成加密和解密的密钥](generating-keys-for-encryption-and-decryption.md)
- [加密数据](encrypting-data.md)
- [加密服务](cryptographic-services.md)
- [加密模型](cryptography-model.md)
- [跨平台加密](cross-platform-cryptography.md)
- [使用填充对 CBC 模式对称解密的漏洞进行计时](vulnerabilities-cbc-mode.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
