---
author: dotpaul
ms.author: paulming
ms.date: 04/17/2019
ms.topic: include
ms.openlocfilehash: 2b60e0e713745b1887ff148c5b3ef151584eb21d
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96590078"
---
在以下情况下，禁止显示此规则的警告是安全的：

- 已知输入受到信任。 考虑到应用程序的信任边界和数据流可能会随时间发生变化。
- 已采取了[如何修复冲突](#how-to-fix-violations)的某项预防措施。
