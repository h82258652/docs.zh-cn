---
description: 详细了解：-subsystemversion (Visual Basic)
title: -subsystemversion
ms.date: 03/13/2018
helpviewer_keywords:
- /subsystemversion compiler option [Visual Basic]
- -subsystemversion compiler option [Visual Basic]
- subsystemversion compiler option [Visual Basic]
ms.assetid: 08be22b2-f447-4cd3-8203-120b1b920b54
ms.openlocfilehash: 5cb4c79b3f837e2427f1b76e7f49ed0bcec596dd
ms.sourcegitcommit: aab60b21144bf04b3057b5d59aa7c58edaef32d1
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/14/2021
ms.locfileid: "107494059"
---
# <a name="-subsystemversion-visual-basic"></a><span data-ttu-id="7d3c1-103">-subsystemversion (Visual Basic)</span><span class="sxs-lookup"><span data-stu-id="7d3c1-103">-subsystemversion (Visual Basic)</span></span>

<span data-ttu-id="7d3c1-104">指定可以运行生成的可执行文件的子系统的最低版本，以此确定可以运行该可执行文件的 Windows 版本。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-104">Specifies the minimum version of the subsystem on which the generated executable file can run, thereby determining the versions of Windows on which the executable file can run.</span></span> <span data-ttu-id="7d3c1-105">大多数情况下，此选项确保该可执行文件可以利用早期 Windows 版本中未提供的特定安全功能。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-105">Most commonly, this option ensures that the executable file can leverage particular security features that aren't available with older versions of Windows.</span></span>

> [!NOTE]
> <span data-ttu-id="7d3c1-106">若要指定子系统本身，请使用 [-target](target.md) 编译器选项。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-106">To specify the subsystem itself, use the [-target](target.md) compiler option.</span></span>

## <a name="syntax"></a><span data-ttu-id="7d3c1-107">语法</span><span class="sxs-lookup"><span data-stu-id="7d3c1-107">Syntax</span></span>

```vb
-subsystemversion:major.minor
```

## <a name="parameters"></a><span data-ttu-id="7d3c1-108">参数</span><span class="sxs-lookup"><span data-stu-id="7d3c1-108">Parameters</span></span>

`major.minor`

<span data-ttu-id="7d3c1-109">所需的子系统最低版本，以主版本和次要版本之间使用点标记的方式表示。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-109">The minimum required version of the subsystem, as expressed in a dot notation for major and minor versions.</span></span> <span data-ttu-id="7d3c1-110">例如，如果将此选项的值设置为 6.01，则可以指定应用程序不能在低于 Windows 7 的操作系统上运行（如本主题后面的表中所述）。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-110">For example, you can specify that an application can't run on an operating system that's older than Windows 7 if you set the value of this option to 6.01, as the table later in this topic describes.</span></span> <span data-ttu-id="7d3c1-111">必须将 `major` 和 `minor` 的值指定为整数。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-111">You must specify the values for `major` and `minor` as integers.</span></span>

<span data-ttu-id="7d3c1-112">`minor` 版本中的前导零不会更改版本，但尾随零会。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-112">Leading zeroes in the `minor` version don't change the version, but trailing zeroes do.</span></span> <span data-ttu-id="7d3c1-113">例如，6.1 和 6.01 表示相同的版本，但 6.10 表示另一个版本。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-113">For example, 6.1 and 6.01 refer to the same version, but 6.10 refers to a different version.</span></span> <span data-ttu-id="7d3c1-114">建议次要版本用两位数表示，以免混淆。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-114">We recommend expressing the minor version as two digits to avoid confusion.</span></span>

## <a name="remarks"></a><span data-ttu-id="7d3c1-115">备注</span><span class="sxs-lookup"><span data-stu-id="7d3c1-115">Remarks</span></span>

<span data-ttu-id="7d3c1-116">下表列出了常见的 Windows 子系统版本。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-116">The following table lists common subsystem versions of Windows.</span></span>

|<span data-ttu-id="7d3c1-117">Windows 版本</span><span class="sxs-lookup"><span data-stu-id="7d3c1-117">Windows version</span></span>|<span data-ttu-id="7d3c1-118">子系统版本</span><span class="sxs-lookup"><span data-stu-id="7d3c1-118">Subsystem version</span></span>|
|---------------------|-----------------------|
|<span data-ttu-id="7d3c1-119">Windows Server 2003</span><span class="sxs-lookup"><span data-stu-id="7d3c1-119">Windows Server 2003</span></span>|<span data-ttu-id="7d3c1-120">5.02</span><span class="sxs-lookup"><span data-stu-id="7d3c1-120">5.02</span></span>|
|<span data-ttu-id="7d3c1-121">Windows Vista</span><span class="sxs-lookup"><span data-stu-id="7d3c1-121">Windows Vista</span></span>|<span data-ttu-id="7d3c1-122">6.00</span><span class="sxs-lookup"><span data-stu-id="7d3c1-122">6.00</span></span>|
|<span data-ttu-id="7d3c1-123">Windows 7</span><span class="sxs-lookup"><span data-stu-id="7d3c1-123">Windows 7</span></span>|<span data-ttu-id="7d3c1-124">6.01</span><span class="sxs-lookup"><span data-stu-id="7d3c1-124">6.01</span></span>|
|<span data-ttu-id="7d3c1-125">Windows Server 2008</span><span class="sxs-lookup"><span data-stu-id="7d3c1-125">Windows Server 2008</span></span>|<span data-ttu-id="7d3c1-126">6.01</span><span class="sxs-lookup"><span data-stu-id="7d3c1-126">6.01</span></span>|
|<span data-ttu-id="7d3c1-127">Windows 8</span><span class="sxs-lookup"><span data-stu-id="7d3c1-127">Windows 8</span></span>|<span data-ttu-id="7d3c1-128">6.02</span><span class="sxs-lookup"><span data-stu-id="7d3c1-128">6.02</span></span>|

## <a name="default-values"></a><span data-ttu-id="7d3c1-129">默认值</span><span class="sxs-lookup"><span data-stu-id="7d3c1-129">Default values</span></span>

<span data-ttu-id="7d3c1-130">-subsystemversion 编译器选项的默认值取决于以下列表中的条件  ：</span><span class="sxs-lookup"><span data-stu-id="7d3c1-130">The default value of the **-subsystemversion** compiler option depends on the conditions in the following list:</span></span>

- <span data-ttu-id="7d3c1-131">只要设置了以下列表中的任意编译器选项，则默认值为 6.02：</span><span class="sxs-lookup"><span data-stu-id="7d3c1-131">The default value is 6.02 if any compiler option in the following list is set:</span></span>

  - [<span data-ttu-id="7d3c1-132">/target:appcontainerexe</span><span class="sxs-lookup"><span data-stu-id="7d3c1-132">-target:appcontainerexe</span></span>](target.md)

  - [<span data-ttu-id="7d3c1-133">/target:winmdobj</span><span class="sxs-lookup"><span data-stu-id="7d3c1-133">-target:winmdobj</span></span>](target.md)

  - [<span data-ttu-id="7d3c1-134">-platform:arm</span><span class="sxs-lookup"><span data-stu-id="7d3c1-134">-platform:arm</span></span>](platform.md)

- <span data-ttu-id="7d3c1-135">如果使用 MSBuild，面向 .NET Framework 4.5，并且未设置先前在此列表中指定的任何编译器选项，则默认值为 6.00。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-135">The default value is 6.00 if you're using MSBuild, you're targeting .NET Framework 4.5, and you haven't set any of the compiler options that were specified earlier in this list.</span></span>

- <span data-ttu-id="7d3c1-136">如果前面的条件均不符合，则默认值为 4.00。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-136">The default value is 4.00 if none of the previous conditions is true.</span></span>

## <a name="setting-this-option"></a><span data-ttu-id="7d3c1-137">设置此选项</span><span class="sxs-lookup"><span data-stu-id="7d3c1-137">Setting this option</span></span>

<span data-ttu-id="7d3c1-138">若要在 Visual Studio 中设置 -subsystemversion 编译器选项，必须打开 .vbproj 文件，并在 MSBuild XML 中为 `SubsystemVersion` 属性指定一个值  。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-138">To set the **-subsystemversion** compiler option in Visual Studio, you must open the .vbproj file and specify a value for the `SubsystemVersion` property in the MSBuild XML.</span></span> <span data-ttu-id="7d3c1-139">不能在 Visual Studio IDE 中设置此选项。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-139">You can't set this option in the Visual Studio IDE.</span></span> <span data-ttu-id="7d3c1-140">有关详细信息，请参阅本主题前面的“默认值”或[常用的 MSBuild 项目属性](/visualstudio/msbuild/common-msbuild-project-properties)。</span><span class="sxs-lookup"><span data-stu-id="7d3c1-140">For more information, see "Default values" earlier in this topic or [Common MSBuild Project Properties](/visualstudio/msbuild/common-msbuild-project-properties).</span></span>

## <a name="see-also"></a><span data-ttu-id="7d3c1-141">请参阅</span><span class="sxs-lookup"><span data-stu-id="7d3c1-141">See also</span></span>

- [<span data-ttu-id="7d3c1-142">Visual Basic 命令行编译器</span><span class="sxs-lookup"><span data-stu-id="7d3c1-142">Visual Basic Command-Line Compiler</span></span>](index.md)

- [<span data-ttu-id="7d3c1-143">MSBuild 属性</span><span class="sxs-lookup"><span data-stu-id="7d3c1-143">MSBuild Properties</span></span>](/visualstudio/msbuild/msbuild-properties)
