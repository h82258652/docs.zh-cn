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
# <a name="storeadmexe-isolated-storage-tool"></a><span data-ttu-id="9e75f-104">Storeadm.exe（独立存储工具）</span><span class="sxs-lookup"><span data-stu-id="9e75f-104">Storeadm.exe (Isolated Storage Tool)</span></span>

<span data-ttu-id="9e75f-105">独立存储工具列出或移除当前用户的所有现有存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-105">The Isolated Storage tool lists or removes all existing stores for the current user.</span></span>  
  
 <span data-ttu-id="9e75f-106">此工具会自动随 Visual Studio 一起安装。</span><span class="sxs-lookup"><span data-stu-id="9e75f-106">This tool is automatically installed with Visual Studio.</span></span> <span data-ttu-id="9e75f-107">若要运行该工具，请使用 [Visual Studio 开发人员命令提示或 Visual Studio 开发人员 PowerShell](/visualstudio/ide/reference/command-prompt-powershell)。</span><span class="sxs-lookup"><span data-stu-id="9e75f-107">To run the tool, use [Visual Studio Developer Command Prompt or Visual Studio Developer PowerShell](/visualstudio/ide/reference/command-prompt-powershell).</span></span>
  
 <span data-ttu-id="9e75f-108">在命令提示符处，键入以下内容：</span><span class="sxs-lookup"><span data-stu-id="9e75f-108">At the command prompt, type the following:</span></span>  
  
## <a name="syntax"></a><span data-ttu-id="9e75f-109">语法</span><span class="sxs-lookup"><span data-stu-id="9e75f-109">Syntax</span></span>  
  
```console  
storeadm [/list][/machine][/remove][/roaming][/quiet]  
```  
  
## <a name="parameters"></a><span data-ttu-id="9e75f-110">参数</span><span class="sxs-lookup"><span data-stu-id="9e75f-110">Parameters</span></span>  
  
|<span data-ttu-id="9e75f-111">选项</span><span class="sxs-lookup"><span data-stu-id="9e75f-111">Option</span></span>|<span data-ttu-id="9e75f-112">说明</span><span class="sxs-lookup"><span data-stu-id="9e75f-112">Description</span></span>|  
|------------|-----------------|  
|<span data-ttu-id="9e75f-113">**/h**[**elp**]</span><span class="sxs-lookup"><span data-stu-id="9e75f-113">**/h**[**elp**]</span></span>|<span data-ttu-id="9e75f-114">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="9e75f-114">Displays command syntax and options for the tool.</span></span>|  
|<span data-ttu-id="9e75f-115">/list</span><span class="sxs-lookup"><span data-stu-id="9e75f-115">**/list**</span></span>|<span data-ttu-id="9e75f-116">显示当前用户的所有现有存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-116">Displays all existing stores for the current user.</span></span> <span data-ttu-id="9e75f-117">这包括该用户执行的所有应用程序或程序集的存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-117">This includes the stores for all applications or assemblies executed by this user.</span></span>|  
|<span data-ttu-id="9e75f-118">/machine</span><span class="sxs-lookup"><span data-stu-id="9e75f-118">**/machine**</span></span>|<span data-ttu-id="9e75f-119">选择计算机存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-119">Selects the machine store.</span></span> <span data-ttu-id="9e75f-120">将此选项与 /list 或 /remove 选项一起使用可指定操作将应用于计算机存储 。</span><span class="sxs-lookup"><span data-stu-id="9e75f-120">Use this option with the **/list** or **/remove** option to specify that the action should apply to the machine store.</span></span><br /><br /> <span data-ttu-id="9e75f-121">.NET Framework 2.0 的新增功能</span><span class="sxs-lookup"><span data-stu-id="9e75f-121">New in the .NET Framework 2.0</span></span>|  
|<span data-ttu-id="9e75f-122">**/quiet**</span><span class="sxs-lookup"><span data-stu-id="9e75f-122">**/quiet**</span></span>|<span data-ttu-id="9e75f-123">指定安静模式；取消信息性输出以便只显示错误消息。</span><span class="sxs-lookup"><span data-stu-id="9e75f-123">Specifies quiet mode; suppresses informational output so that only error messages appear.</span></span>|  
|<span data-ttu-id="9e75f-124">/remove</span><span class="sxs-lookup"><span data-stu-id="9e75f-124">**/remove**</span></span>|<span data-ttu-id="9e75f-125">永久性移除当前用户的所有现有存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-125">Permanently removes all existing stores for the current user.</span></span>|  
|<span data-ttu-id="9e75f-126">/roaming</span><span class="sxs-lookup"><span data-stu-id="9e75f-126">**/roaming**</span></span>|<span data-ttu-id="9e75f-127">选择漫游存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-127">Selects the roaming store.</span></span> <span data-ttu-id="9e75f-128">将此选项与 /list 或 /remove 选项一起使可指定操作将应用于漫游存储 。</span><span class="sxs-lookup"><span data-stu-id="9e75f-128">Use this option with the **/list** or **/remove** options to specify that the action should apply to the roaming store.</span></span>|  
|<span data-ttu-id="9e75f-129">**/?**</span><span class="sxs-lookup"><span data-stu-id="9e75f-129">**/?**</span></span>|<span data-ttu-id="9e75f-130">显示该工具的命令语法和选项。</span><span class="sxs-lookup"><span data-stu-id="9e75f-130">Displays command syntax and options for the tool.</span></span>|  
  
## <a name="remarks"></a><span data-ttu-id="9e75f-131">备注</span><span class="sxs-lookup"><span data-stu-id="9e75f-131">Remarks</span></span>  

 <span data-ttu-id="9e75f-132">如果从命令行运行 Storeadm.exe 但不指定任何选项，则将显示此工具的语法和选项。</span><span class="sxs-lookup"><span data-stu-id="9e75f-132">Running Storeadm.exe from the command line without specifying any options displays the syntax and options for the tool.</span></span>  
  
 <span data-ttu-id="9e75f-133">/list 和 /remove 选项通常一次使用一个；但如果指定了两个或两个以上的选项，则它们将按照在命令行上出现的顺序执行 。</span><span class="sxs-lookup"><span data-stu-id="9e75f-133">The **/list** and **/remove** options are typically used one at a time; however, if two or more options are specified they will be performed in the order in which they appear on the command line.</span></span>  
  
 <span data-ttu-id="9e75f-134">应用程序可以选择保存到两个用户存储之一或保存到计算机存储：</span><span class="sxs-lookup"><span data-stu-id="9e75f-134">Applications have a choice of saving to one of two stores for a user or to the machine store:</span></span>  
  
- <span data-ttu-id="9e75f-135">本地存储位于保证不会漫游的位置，即使为用户启用了用户数据漫游也是如此。</span><span class="sxs-lookup"><span data-stu-id="9e75f-135">The local store exists in a location that is guaranteed not to roam, even if user data roaming is enabled for the user.</span></span>  
  
- <span data-ttu-id="9e75f-136">漫游存储位于能够漫游的位置，但只有通过 Windows 管理为用户启用了漫游后才可做到这一点。</span><span class="sxs-lookup"><span data-stu-id="9e75f-136">The roaming store exists in a location that is able to roam, but can only do so if roaming is enabled for the user via Windows administration.</span></span>  
  
- <span data-ttu-id="9e75f-137">计算机存储对于计算机上的所有用户是公共的，并且它存储在该计算机上的公共目录下。</span><span class="sxs-lookup"><span data-stu-id="9e75f-137">The machine store is common to all users on a machine and is stored under a common directory on that machine.</span></span>
  
<span data-ttu-id="9e75f-138">实际上，是否为用户启用漫游并不会影响 Storeadm.exe 的管理。</span><span class="sxs-lookup"><span data-stu-id="9e75f-138">Whether roaming is actually enabled for the user does not affect the administration of Storeadm.exe.</span></span> <span data-ttu-id="9e75f-139">在不使用任何选项的情况下运行此工具会向本地存储应用所有操作。</span><span class="sxs-lookup"><span data-stu-id="9e75f-139">Running the tool without any options applies all actions to the local store.</span></span> <span data-ttu-id="9e75f-140">在使用 /roaming 选项的情况下运行此工具会将所有操作应用于可漫游的存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-140">Running the tool with the **/roaming** option applies all actions to the store that is able to roam.</span></span> <span data-ttu-id="9e75f-141">在使用 /machine 选项的情况下运行此工具会将所有操作应用于计算机存储。</span><span class="sxs-lookup"><span data-stu-id="9e75f-141">Running the tool with the **/machine** option applies all actions to the machine store.</span></span>  
  
## <a name="see-also"></a><span data-ttu-id="9e75f-142">请参阅</span><span class="sxs-lookup"><span data-stu-id="9e75f-142">See also</span></span>

- [<span data-ttu-id="9e75f-143">工具</span><span class="sxs-lookup"><span data-stu-id="9e75f-143">Tools</span></span>](index.md)
- [<span data-ttu-id="9e75f-144">独立存储</span><span class="sxs-lookup"><span data-stu-id="9e75f-144">Isolated Storage</span></span>](../../standard/io/isolated-storage.md)
- [<span data-ttu-id="9e75f-145">开发人员命令行 shell</span><span class="sxs-lookup"><span data-stu-id="9e75f-145">Developer command-line shells</span></span>](/visualstudio/ide/reference/command-prompt-powershell)
