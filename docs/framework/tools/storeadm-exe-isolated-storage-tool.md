---
title: Storeadm.exe（独立存储工具）
description: 了解 Storeadm.exe（独立存储工具）。 此工具列出或移除当前用户的所有现有存储。
ms.date: 03/30/2017
helpviewer_keywords:
- Storeadm.exe
- listing stores for current user
- Isolated Storage tool
- stores, current user
- removing stores
ms.assetid: b81202b8-d91d-4b23-9c53-4a112f74a44a
ms.openlocfilehash: 89eb2b35dc640f21f3d6d6ca7f477841f1a020c7
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494033"
---
# <a name="storeadmexe-isolated-storage-tool"></a>Storeadm.exe（独立存储工具）

独立存储工具列出或移除当前用户的所有现有存储。  
  
 此工具会自动随 Visual Studio 一起安装。 若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。
  
 在命令提示符处，键入以下内容：  
  
## <a name="syntax"></a>语法  
  
```console  
storeadm [/list][/machine][/remove][/roaming][/quiet]  
```  
  
## <a name="parameters"></a>参数  
  
|选项|说明|  
|------------|-----------------|  
|**/h**[**elp**]|显示该工具的命令语法和选项。|  
|/list|显示当前用户的所有现有存储。 这包括该用户执行的所有应用程序或程序集的存储。|  
|/machine|选择计算机存储。 将此选项与 /list 或 /remove 选项一起使用可指定操作将应用于计算机存储 。<br /><br /> .NET Framework 2.0 的新增功能|  
|**/quiet**|指定安静模式；取消信息性输出以便只显示错误消息。|  
|/remove|永久性移除当前用户的所有现有存储。|  
|/roaming|选择漫游存储。 将此选项与 /list 或 /remove 选项一起使可指定操作将应用于漫游存储 。|  
|**/?**|显示该工具的命令语法和选项。|  
  
## <a name="remarks"></a>备注  

 如果从命令行运行 Storeadm.exe 但不指定任何选项，则将显示此工具的语法和选项。  
  
 /list 和 /remove 选项通常一次使用一个；但如果指定了两个或两个以上的选项，则它们将按照在命令行上出现的顺序执行 。  
  
 应用程序可以选择保存到两个用户存储之一或保存到计算机存储：  
  
- 本地存储位于保证不会漫游的位置，即使为用户启用了用户数据漫游也是如此。  
  
- 漫游存储位于能够漫游的位置，但只有通过 Windows 管理为用户启用了漫游后才可做到这一点。  
  
- 计算机存储对于计算机上的所有用户是公共的，并且它存储在该计算机上的公共目录下。
  
实际上，是否为用户启用漫游并不会影响 Storeadm.exe 的管理。 在不使用任何选项的情况下运行此工具会向本地存储应用所有操作。 在使用 /roaming 选项的情况下运行此工具会将所有操作应用于可漫游的存储。 在使用 /machine 选项的情况下运行此工具会将所有操作应用于计算机存储。  
  
## <a name="see-also"></a>请参阅

- [工具](index.md)
- [独立存储](../../standard/io/isolated-storage.md)
- [开发人员命令行 shell](/visualstudio/ide/reference/command-prompt-powershell)
