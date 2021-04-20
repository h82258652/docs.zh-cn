---
title: 对数据加密
description: 了解如何在 .NET 中使用对称算法或非对称算法加密数据。
ms.date: 04/16/2021
dev_langs:
- csharp
- vb
helpviewer_keywords:
- encryption [.NET], symmetric
- symmetric encryption
- cryptography [.NET], asymmetric
- asymmetric encryption
ms.assetid: 7ecce51f-db5f-4bd4-9321-cceb6fcb2a77
ms.openlocfilehash: e4b006db86eda1a2a62dfd31073be9aabb7a3bb8
ms.sourcegitcommit: 8f71a6c655a9c39d5223401aed76c02ba00e03ee
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/20/2021
ms.locfileid: "107740327"
---
# <a name="encrypting-data"></a>对数据加密

对称加密和非对称加密是使用不同的进程执行的。 对称加密是对流执行的，因此适用于加密大量数据。 非对称加密是对少数字节执行的，因此仅适用于加密少量数据。

## <a name="symmetric-encryption"></a>对称加密

托管对称加密类与名为 <xref:System.Security.Cryptography.CryptoStream> 的特殊流类一起使用，后者用于加密读入流中的数据。 使用托管流类（ **CryptoStream** 类）初始化一个类，该类实现 <xref:System.Security.Cryptography.ICryptoTransform> 接口 (从实现加密算法的类创建) ，以及 <xref:System.Security.Cryptography.CryptoStreamMode> 描述允许 **CryptoStream** 的访问类型的枚举。 可以使用从类派生的任何类 <xref:System.IO.Stream> （包括 <xref:System.IO.FileStream> 、和）初始化 CryptoStream 类 <xref:System.IO.MemoryStream> <xref:System.Net.Sockets.NetworkStream> 。 使用这些类，可以对多个流对象执行对称加密。

下面的示例演示如何创建算法的默认实现类的新实例 <xref:System.Security.Cryptography.Aes> 。 实例用于对 **CryptoStream** 类执行加密。 在此示例中，使用名为 **的流对象初始化** CryptoStream `fileStream` ，该流对象可以是任何类型的托管流。 **Aes** 类中的 **CreateEncryptor** 方法传递用于加密的密钥和 IV。 在此例中，使用了由 `aes` 生成的默认密钥和 IV。

```vb
Dim aes As Aes = Aes.Create()
Dim cryptStream As New CryptoStream(
    fileStream, aes.CreateEncryptor(key, iv), CryptoStreamMode.Write)
```

```csharp
Aes aes = Aes.Create();
CryptoStream cryptStream = new CryptoStream(
    fileStream, aes.CreateEncryptor(key, iv), CryptoStreamMode.Write);
```

执行此代码后，将使用 AES 算法对写入到 **CryptoStream** 对象的任何数据进行加密。

下面的示例演示创建流、加密流、写入流和关闭流的整个过程。 此示例创建使用 **CryptoStream** 类和 **Aes** 类加密的文件流。 生成的 IV 将写入到的开头 <xref:System.IO.FileStream> ，因此它可以读取并用于解密。 然后，使用类将消息写入到加密流 <xref:System.IO.StreamWriter> 。 尽管可以多次使用同一密钥来加密和解密数据，但建议每次生成新的随机 IV。 这样，即使在纯文本相同的情况下，加密的数据也总是不同。

:::code language="csharp" source="snippets/encrypting-data/csharp/aes-encrypt.cs":::
:::code language="vb" source="snippets/encrypting-data/vb/aes-encrypt.vb":::

此代码使用 AES 对称算法加密流，并编写 IV，然后加密 "Hello World！" 写入流。 如果代码成功，它将创建一个名为 *TestData.txt* 的加密文件，并在控制台中显示以下文本：

```console
The text was encrypted.
```

您可以使用对称解密示例对 [数据](decrypting-data.md)进行解密。 该示例和此示例指定了相同的密钥。

但是，如果引发异常，则代码会在控制台中显示以下文本：

```console
The encryption failed.
```

## <a name="asymmetric-encryption"></a>非对称加密

非对称算法通常用于加密少量数据，例如加密对称密钥和 IV。 通常情况下，单独执行的非对称加密使用另一方生成的公共密钥。 <xref:System.Security.Cryptography.RSA>该类由 .net 提供，用于实现此目的。

下面的示例使用公钥信息加密对称密钥和 IV。 初始化了代表第三方公钥的两个字节数组。 <xref:System.Security.Cryptography.RSAParameters> 对象初始化为这些值。 接下来，使用方法将 **RSAParameters** 对象与它) 表示的公钥 (一起导入到 **RSA** 实例中 <xref:System.Security.Cryptography.RSA.ImportParameters%2A?displayProperty=nameWithType> 。 最后，通过类创建私钥和 IV <xref:System.Security.Cryptography.Aes> 进行加密。 此示例要求系统安装 128 位加密。

```vb
Imports System
Imports System.Security.Cryptography

Module Module1

    Sub Main()
        'Initialize the byte arrays to the public key information.
        Dim modulus As Byte() = {214, 46, 220, 83, 160, 73, 40, 39, 201, 155, 19, 202, 3, 11, 191, 178, 56, 74, 90, 36, 248, 103, 18, 144, 170, 163, 145, 87, 54, 61, 34, 220, 222, 207, 137, 149, 173, 14, 92, 120, 206, 222, 158, 28, 40, 24, 30, 16, 175, 108, 128, 35, 230, 118, 40, 121, 113, 125, 216, 130, 11, 24, 90, 48, 194, 240, 105, 44, 76, 34, 57, 249, 228, 125, 80, 38, 9, 136, 29, 117, 207, 139, 168, 181, 85, 137, 126, 10, 126, 242, 120, 247, 121, 8, 100, 12, 201, 171, 38, 226, 193, 180, 190, 117, 177, 87, 143, 242, 213, 11, 44, 180, 113, 93, 106, 99, 179, 68, 175, 211, 164, 116, 64, 148, 226, 254, 172, 147}

        Dim exponent As Byte() = {1, 0, 1}

        'Create values to store encrypted symmetric keys.
        Dim encryptedSymmetricKey() As Byte
        Dim encryptedSymmetricIV() As Byte

        'Create a new instance of the default RSA implementation class.
        Dim rsa As RSA = RSA.Create()

        'Create a new instance of the RSAParameters structure.
        Dim rsaKeyInfo As New RSAParameters()

        'Set rsaKeyInfo to the public key values.
        rsaKeyInfo.Modulus = modulus
        rsaKeyInfo.Exponent = exponent

        'Import key parameters into rsa
        rsa.ImportParameters(rsaKeyInfo)

        'Create a new instance of the default Aes implementation class.
        Dim aes As Aes = Aes.Create()

        'Encrypt the symmetric key and IV.
        encryptedSymmetricKey = rsa.Encrypt(aes.Key, RSAEncryptionPadding.Pkcs1)
        encryptedSymmetricIV = rsa.Encrypt(aes.IV, RSAEncryptionPadding.Pkcs1)
    End Sub

End Module
```

```csharp
using System;
using System.Security.Cryptography;

class Class1
{
    static void Main()
    {
        //Initialize the byte arrays to the public key information.
        byte[] modulus =
        {
            214,46,220,83,160,73,40,39,201,155,19,202,3,11,191,178,56,
            74,90,36,248,103,18,144,170,163,145,87,54,61,34,220,222,
            207,137,149,173,14,92,120,206,222,158,28,40,24,30,16,175,
            108,128,35,230,118,40,121,113,125,216,130,11,24,90,48,194,
            240,105,44,76,34,57,249,228,125,80,38,9,136,29,117,207,139,
            168,181,85,137,126,10,126,242,120,247,121,8,100,12,201,171,
            38,226,193,180,190,117,177,87,143,242,213,11,44,180,113,93,
            106,99,179,68,175,211,164,116,64,148,226,254,172,147
        };

        byte[] exponent = { 1, 0, 1 };

        //Create values to store encrypted symmetric keys.
        byte[] encryptedSymmetricKey;
        byte[] encryptedSymmetricIV;

        //Create a new instance of the RSA class.
        RSA rsa = RSA.Create();

        //Create a new instance of the RSAParameters structure.
        RSAParameters rsaKeyInfo = new RSAParameters();

        //Set rsaKeyInfo to the public key values.
        rsaKeyInfo.Modulus = modulus;
        rsaKeyInfo.Exponent = exponent;

        //Import key parameters into rsa.
        rsa.ImportParameters(rsaKeyInfo);

        //Create a new instance of the default Aes implementation class.
        Aes aes = Aes.Create();

        //Encrypt the symmetric key and IV.
        encryptedSymmetricKey = rsa.Encrypt(aes.Key, RSAEncryptionPadding.Pkcs1);
        encryptedSymmetricIV = rsa.Encrypt(aes.IV, RSAEncryptionPadding.Pkcs1);
    }
}
```

## <a name="see-also"></a>请参阅

- [生成加密和解密的密钥](generating-keys-for-encryption-and-decryption.md)
- [解密数据](decrypting-data.md)
- [加密服务](cryptographic-services.md)
- [跨平台加密](cross-platform-cryptography.md)
- [使用填充对 CBC 模式对称解密的漏洞进行计时](vulnerabilities-cbc-mode.md)
- [ASP.NET Core 数据保护](/aspnet/core/security/data-protection/introduction)
