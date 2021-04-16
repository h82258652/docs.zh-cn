---
author: dotpaul
ms.author: paulming
ms.date: 04/05/2019
ms.topic: include
ms.openlocfilehash: 9235f1bcda7529b71aef542d3a49da08a9d4b59e
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590079"
---
反序列化不受信任的数据时，不安全的反序列化程序易受攻击。 攻击者可能会修改序列化数据，使其包含非预期类型，进而注入具有不良副作用的对象。 例如，针对不安全反序列化程序的攻击可以在基础操作系统上执行命令，通过网络进行通信，或删除文件。
