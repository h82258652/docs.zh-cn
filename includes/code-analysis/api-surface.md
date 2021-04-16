---
ms.openlocfilehash: bf7ea8a36b9c36d82b4c00ada890829bf4c2c024
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97700850"
---
### <a name="include-specific-api-surfaces"></a>包含特定的 API 图面

你可以根据代码库的可访问性，配置要针对其运行此规则的部分。 例如，若要指定规则应仅针对非公共 API 图面运行，请将以下键值对添加到项目中的 .editorconfig 文件：

```ini
dotnet_code_quality.CAXXXX.api_surface = private, internal
```
