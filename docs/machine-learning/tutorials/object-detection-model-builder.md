---
title: 教程：使用 Model Builder 检测图像中的对象
description: 本教程演示了如何使用 ML.NET Model Builder 和 Azure ML 生成对象检测模型，以检测图像中的停车标志。
author: briacht
ms.author: brachtma
ms.date: 03/12/2021
ms.topic: tutorial
ms.custom: mlnet-tooling
ms.openlocfilehash: 3a773f7364e3fd6c8ad0073eac091fa38716afa6
ms.sourcegitcommit: 44af69720863bd09bd7a4509bf1ec119466ba6e8
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/02/2021
ms.locfileid: "106231266"
---
# <a name="tutorial-detect-stop-signs-in-images-with-model-builder"></a><span data-ttu-id="e204b-103">教程：使用 Model Builder 检测图像中的停车标志</span><span class="sxs-lookup"><span data-stu-id="e204b-103">Tutorial: Detect stop signs in images with Model Builder</span></span>

<span data-ttu-id="e204b-104">了解如何使用 ML.NET Model Builder 和 Azure ML 生成对象检测模型，以检测和定位图像中的停车标志。</span><span class="sxs-lookup"><span data-stu-id="e204b-104">Learn how to build an object detection model using ML.NET Model Builder and Azure ML to detect and locate stop signs in images.</span></span>

<span data-ttu-id="e204b-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="e204b-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="e204b-106">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="e204b-106">Prepare and understand the data</span></span>
> - <span data-ttu-id="e204b-107">选择方案</span><span class="sxs-lookup"><span data-stu-id="e204b-107">Choose the scenario</span></span>
> - <span data-ttu-id="e204b-108">选择训练环境</span><span class="sxs-lookup"><span data-stu-id="e204b-108">Choose the training environment</span></span>
> - <span data-ttu-id="e204b-109">加载数据</span><span class="sxs-lookup"><span data-stu-id="e204b-109">Load the data</span></span>
> - <span data-ttu-id="e204b-110">定型模型</span><span class="sxs-lookup"><span data-stu-id="e204b-110">Train the model</span></span>
> - <span data-ttu-id="e204b-111">评估模型</span><span class="sxs-lookup"><span data-stu-id="e204b-111">Evaluate the model</span></span>
> - <span data-ttu-id="e204b-112">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="e204b-112">Use the model for predictions</span></span>

> [!NOTE]
> <span data-ttu-id="e204b-113">模型生成器当前为预览版。</span><span class="sxs-lookup"><span data-stu-id="e204b-113">Model Builder is currently in Preview.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e204b-114">先决条件</span><span class="sxs-lookup"><span data-stu-id="e204b-114">Prerequisites</span></span>

<span data-ttu-id="e204b-115">有关先决条件和安装说明列表，请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)。</span><span class="sxs-lookup"><span data-stu-id="e204b-115">For a list of prerequisites and installation instructions, visit the [Model Builder installation guide](../how-to-guides/install-model-builder.md).</span></span>

## <a name="model-builder-object-detection-overview"></a><span data-ttu-id="e204b-116">Model Builder 对象检测概述</span><span class="sxs-lookup"><span data-stu-id="e204b-116">Model Builder object detection overview</span></span>

<span data-ttu-id="e204b-117">对象检测是计算机视觉问题。</span><span class="sxs-lookup"><span data-stu-id="e204b-117">Object detection is a computer vision problem.</span></span> <span data-ttu-id="e204b-118">虽然与图像分类密切相关，但是对象检测以更精细的比例执行图像分类。</span><span class="sxs-lookup"><span data-stu-id="e204b-118">While closely related to image classification, object detection performs image classification at a more granular scale.</span></span> <span data-ttu-id="e204b-119">对象检测用于定位图像中的实体并对其进行分类。</span><span class="sxs-lookup"><span data-stu-id="e204b-119">Object detection both locates _and_ categorizes entities within images.</span></span> <span data-ttu-id="e204b-120">如果图像包含多个不同类型的对象，请使用对象检测。</span><span class="sxs-lookup"><span data-stu-id="e204b-120">Use object detection when images contain multiple objects of different types.</span></span>

![显示图像分类与对象分类的屏幕截图。](./media/object-detection-onnx/img-classification-obj-detection.PNG)

<span data-ttu-id="e204b-122">对象检测的一些用例包括：</span><span class="sxs-lookup"><span data-stu-id="e204b-122">Some use cases for object detection include:</span></span>

- <span data-ttu-id="e204b-123">自动驾驶汽车</span><span class="sxs-lookup"><span data-stu-id="e204b-123">Self-Driving Cars</span></span>
- <span data-ttu-id="e204b-124">机器人</span><span class="sxs-lookup"><span data-stu-id="e204b-124">Robotics</span></span>
- <span data-ttu-id="e204b-125">人脸检测</span><span class="sxs-lookup"><span data-stu-id="e204b-125">Face Detection</span></span>
- <span data-ttu-id="e204b-126">工作区安全性</span><span class="sxs-lookup"><span data-stu-id="e204b-126">Workplace Safety</span></span>
- <span data-ttu-id="e204b-127">对象计数</span><span class="sxs-lookup"><span data-stu-id="e204b-127">Object Counting</span></span>
- <span data-ttu-id="e204b-128">活动识别</span><span class="sxs-lookup"><span data-stu-id="e204b-128">Activity Recognition</span></span>

<span data-ttu-id="e204b-129">此示例将创建一个 C# .NET Core 控制台应用程序，该应用程序使用通过 Model Builder 生成的机器学习模型来检测图像中的停车标志。</span><span class="sxs-lookup"><span data-stu-id="e204b-129">This sample creates a C# .NET Core console application that detects stop signs in images using a machine learning model built with Model Builder.</span></span> <span data-ttu-id="e204b-130">有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub 存储库中找到。</span><span class="sxs-lookup"><span data-stu-id="e204b-130">You can find the source code for this tutorial at the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub repository.</span></span>

## <a name="prepare-and-understand-the-data"></a><span data-ttu-id="e204b-131">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="e204b-131">Prepare and understand the data</span></span>

<span data-ttu-id="e204b-132">停车标志数据集包含从 [Unsplash](https://unsplash.com/) 下载的 50 个图像，每个图像都至少包含一个停车标志。</span><span class="sxs-lookup"><span data-stu-id="e204b-132">The Stop Sign dataset consists of 50 images downloaded from [Unsplash](https://unsplash.com/), each of which contain at least one stop sign.</span></span>

<span data-ttu-id="e204b-133">需要做的第一件事就是为图像添加批注，或围绕每个图像中的停车标志绘制边界框。</span><span class="sxs-lookup"><span data-stu-id="e204b-133">The first thing you need to do is annotate your images, or draw bounding boxes around the stop signs in each image.</span></span> <span data-ttu-id="e204b-134">在本教程中，你将使用一个名为 [VoTT](https://github.com/microsoft/VoTT) 的工具来为图像添加批注。</span><span class="sxs-lookup"><span data-stu-id="e204b-134">In this tutorial, you will annotate your images with a tool called [VoTT](https://github.com/microsoft/VoTT).</span></span>

> <span data-ttu-id="e204b-135">如果想要跳过下面的数据标签标注步骤，可以[下载此数据集版本](https://aka.ms/mlnet-object-detection-tutorial-assets)，然后跳到[创建控制台应用程序](object-detection-model-builder.md#create-a-console-application)。</span><span class="sxs-lookup"><span data-stu-id="e204b-135">If you want to skip the data labeling steps below, you can [download this version of the dataset](https://aka.ms/mlnet-object-detection-tutorial-assets) and skip to [Create a console application](object-detection-model-builder.md#create-a-console-application).</span></span>

### <a name="create-a-new-vott-project"></a><span data-ttu-id="e204b-136">新建一个 VoTT 项目</span><span class="sxs-lookup"><span data-stu-id="e204b-136">Create a new VoTT project</span></span>

1. <span data-ttu-id="e204b-137">[下载数据集](https://aka.ms/mlnet-object-detection-tutorial-dataset)（其中包含 50 个停车标志图像），然后进行解压缩。</span><span class="sxs-lookup"><span data-stu-id="e204b-137">[Download the dataset](https://aka.ms/mlnet-object-detection-tutorial-dataset) of 50 stop sign images and unzip.</span></span>
1. <span data-ttu-id="e204b-138">[下载 VoTT](https://github.com/Microsoft/VoTT/releases)（视觉对象标记工具）。</span><span class="sxs-lookup"><span data-stu-id="e204b-138">[Download VoTT](https://github.com/Microsoft/VoTT/releases) (Visual Object Tagging Tool).</span></span>
1. <span data-ttu-id="e204b-139">打开 VoTT，选择“新建项目”。</span><span class="sxs-lookup"><span data-stu-id="e204b-139">Open VoTT and select **New Project**.</span></span>

    ![VoTT 主屏幕](./media/object-detection-model-builder/vott.png)

1. <span data-ttu-id="e204b-141">在“项目设置”中，将“显示名称”更改为“StopSignObjDetection”。</span><span class="sxs-lookup"><span data-stu-id="e204b-141">In **Project Settings**, change the **Display Name** to "StopSignObjDetection".</span></span>
1. <span data-ttu-id="e204b-142">更改安全令牌，以生成新的安全令牌。</span><span class="sxs-lookup"><span data-stu-id="e204b-142">Change the **Security Token** to *Generate New Security Token*.</span></span>
1. <span data-ttu-id="e204b-143">在“源连接”旁边，选择“添加连接”。</span><span class="sxs-lookup"><span data-stu-id="e204b-143">Next to **Source Connection**, select **Add Connection**.</span></span>
1. <span data-ttu-id="e204b-144">在“连接设置”中，将源连接的显示名称更改为“StopSignImages”，并选择“本地文件系统”作为提供程序。</span><span class="sxs-lookup"><span data-stu-id="e204b-144">In **Connection Settings**, change the **Display Name** for the source connection to "StopSignImages", and select *Local File System* as the **Provider**.</span></span> <span data-ttu-id="e204b-145">对于“文件夹路径”，请选择包含 50 个训练图像的 Stop-Signs 文件夹，然后选择“保存连接”。</span><span class="sxs-lookup"><span data-stu-id="e204b-145">For the **Folder Path**, select the *Stop-Signs* folder which contains the 50 training images, and then select **Save Connection**.</span></span>

    ![VoTT“新建连接”对话框](./media/object-detection-model-builder/vott-new-connection.png)

1. <span data-ttu-id="e204b-147">在“项目设置”中，将“源连接”更改为 StopSignImages（刚创建的连接）。</span><span class="sxs-lookup"><span data-stu-id="e204b-147">In **Project Settings**, change the **Source Connection** to *StopSignImages* (the connection you just created).</span></span>
1. <span data-ttu-id="e204b-148">将“目标连接”也更改为 StopSignImages。</span><span class="sxs-lookup"><span data-stu-id="e204b-148">Change the **Target Connection** to *StopSignImages* as well.</span></span> <span data-ttu-id="e204b-149">“项目设置”现在应类似于以下屏幕截图：</span><span class="sxs-lookup"><span data-stu-id="e204b-149">Your **Project Settings** should now look similar to this screenshot:</span></span>

    ![VoTT“项目设置”对话框](./media/object-detection-model-builder/vott-new-project.png)

1. <span data-ttu-id="e204b-151">选择“保存项目”。</span><span class="sxs-lookup"><span data-stu-id="e204b-151">Select **Save Project**.</span></span>

### <a name="add-tag-and-label-images"></a><span data-ttu-id="e204b-152">添加标记并为图像添加标签</span><span class="sxs-lookup"><span data-stu-id="e204b-152">Add tag and label images</span></span>

<span data-ttu-id="e204b-153">现在，你应该会看到一个窗口，其中左侧是所有训练图像的预览图像，中间是所选定图像的预览图像，右侧是标记列。</span><span class="sxs-lookup"><span data-stu-id="e204b-153">You should now see a window with preview images of all the training images on the left, a preview of the selected image in the middle, and a **Tags** column on the right.</span></span> <span data-ttu-id="e204b-154">此屏幕是标记编辑器。</span><span class="sxs-lookup"><span data-stu-id="e204b-154">This screen is the **Tags editor**.</span></span>

1. <span data-ttu-id="e204b-155">选择“标记”工具栏中的第一个（加号形状）图标，以添加新标记。</span><span class="sxs-lookup"><span data-stu-id="e204b-155">Select the first (plus-shaped) icon in the **Tags** toolbar to add a new tag.</span></span>

    ![VoTT“新建标记”图标](./media/object-detection-model-builder/vott-new-tag-icon.png)

1. <span data-ttu-id="e204b-157">将标记命名为“Stop-Sign”，然后在键盘上按 Enter 键<kbd></kbd>。</span><span class="sxs-lookup"><span data-stu-id="e204b-157">Name the tag "Stop-Sign" and hit <kbd>Enter</kbd> on your keyboard.</span></span>

    ![VoTT 新标记](./media/object-detection-model-builder/vott-new-tag.png)

1. <span data-ttu-id="e204b-159">单击并拖动鼠标，围绕图像中的每个停车标志绘制一个矩形。</span><span class="sxs-lookup"><span data-stu-id="e204b-159">Click and drag to draw a rectangle around each stop sign in the image.</span></span> <span data-ttu-id="e204b-160">如果无法使用光标绘制矩形，请尝试从顶部的工具栏中选择“绘制矩形”工具，或使用键盘快捷键 R<kbd></kbd>。</span><span class="sxs-lookup"><span data-stu-id="e204b-160">If the cursor does not let you draw a rectangle, try selecting the **Draw Rectangle** tool from the toolbar on the top, or use the keyboard shortcut <kbd>R</kbd>.</span></span>
1. <span data-ttu-id="e204b-161">绘制矩形后，选择在前面的步骤中创建的 Stop-Sign 标记，将此标记添加到边界框。</span><span class="sxs-lookup"><span data-stu-id="e204b-161">After drawing your rectangle, select the **Stop-Sign** tag that you created in the previous steps to add the tag to the bounding box.</span></span>
1. <span data-ttu-id="e204b-162">单击数据集中下一个图像的预览图像，重复此过程。</span><span class="sxs-lookup"><span data-stu-id="e204b-162">Click on the preview image for the next image in the dataset and repeat this process.</span></span>
1. <span data-ttu-id="e204b-163">继续对每个图像中的每一个停车标志执行步骤 3-4。</span><span class="sxs-lookup"><span data-stu-id="e204b-163">Continue steps 3-4 for every stop sign in every image.</span></span>

    ![VoTT 为图像添加批注](./media/object-detection-model-builder/vott-annotating.gif)

### <a name="export-your-vott-json"></a><span data-ttu-id="e204b-165">导出 VoTT JSON</span><span class="sxs-lookup"><span data-stu-id="e204b-165">Export your VoTT JSON</span></span>

<span data-ttu-id="e204b-166">为所有训练图像添加标签后，可以导出文件，以便 Model Builder 使用它进行训练。</span><span class="sxs-lookup"><span data-stu-id="e204b-166">Once you have labeled all of your training images, you can export the file that will be used by Model Builder for training.</span></span>

1. <span data-ttu-id="e204b-167">选择左侧工具栏中的第四个图标（框中包含斜箭头的图标），以转到“导出设置”。</span><span class="sxs-lookup"><span data-stu-id="e204b-167">Select the fourth icon in the left toolbar (the one with the diagonal arrow in a box) to go to the **Export Settings**.</span></span>
1. <span data-ttu-id="e204b-168">将“提供程序”保留为 VoTT JSON。</span><span class="sxs-lookup"><span data-stu-id="e204b-168">Leave the **Provider** as *VoTT JSON*.</span></span>
1. <span data-ttu-id="e204b-169">将“资产状态”更改为“仅带标记的资产”。</span><span class="sxs-lookup"><span data-stu-id="e204b-169">Change the **Asset State** to *Only tagged Assets*.</span></span>
1. <span data-ttu-id="e204b-170">取消选中“包括图像”。</span><span class="sxs-lookup"><span data-stu-id="e204b-170">Uncheck **Include Images**.</span></span> <span data-ttu-id="e204b-171">如果包括图像，则训练图像将复制到生成的导出文件夹，此操作并不必要。</span><span class="sxs-lookup"><span data-stu-id="e204b-171">If you include the images, then the training images will be copied to the export folder that is generated, which is not necessary.</span></span>
1. <span data-ttu-id="e204b-172">选择“保存导出设置”。</span><span class="sxs-lookup"><span data-stu-id="e204b-172">Select **Save Export Settings**.</span></span>

    ![VoTT“导出设置”](./media/object-detection-model-builder/vott-export.png)

1. <span data-ttu-id="e204b-174">返回到“标记编辑器”（形状类似功能区的左侧工具栏中的第二个图标）。</span><span class="sxs-lookup"><span data-stu-id="e204b-174">Go back to the **Tags editor** (the second icon in the left toolbar shaped like a ribbon).</span></span> <span data-ttu-id="e204b-175">在顶部工具栏中，选择“导出项目”图标（最后一个图标，形状类似包含箭头的框），或使用键盘快捷键 Ctrl<kbd></kbd>+E<kbd></kbd>。</span><span class="sxs-lookup"><span data-stu-id="e204b-175">In the top toolbar, select the **Export Project** icon (the last icon shaped like an arrow in a box), or use the keyboard shortcut <kbd>Ctrl</kbd>+<kbd>E</kbd>.</span></span>

    ![VoTT“导出”按钮](./media/object-detection-model-builder/vott-export-button.png)

<span data-ttu-id="e204b-177">此导出操作将在 Stop-Sign-Images 文件夹中新建一个名为 vott-json-export 的文件夹，并将在此新文件夹中生成名为 StopSignObjDetection-export 的 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="e204b-177">This export will create a new folder called *vott-json-export* in your *Stop-Sign-Images* folder and will generate a JSON file named *StopSignObjDetection-export* in that new folder.</span></span> <span data-ttu-id="e204b-178">在后续步骤中使用 Model Builder 训练对象检测模型时，你将使用此 JSON 文件。</span><span class="sxs-lookup"><span data-stu-id="e204b-178">You will use this JSON file in the next steps for training an object detection model in Model Builder.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="e204b-179">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="e204b-179">Create a console application</span></span>

1. <span data-ttu-id="e204b-180">在 Visual Studio 中，创建一个名为 StopSignDetection 的 C# .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="e204b-180">In Visual Studio, create a **C# .NET Core console application** called *StopSignDetection*.</span></span>

## <a name="choose-a-scenario"></a><span data-ttu-id="e204b-181">选择方案</span><span class="sxs-lookup"><span data-stu-id="e204b-181">Choose a scenario</span></span>

1. <span data-ttu-id="e204b-182">在“解决方案资源管理器”中，右键单击 StopSignDetection 项目，然后选择“添加” > “机器学习”，以打开 Model Builder UI。</span><span class="sxs-lookup"><span data-stu-id="e204b-182">In **Solution Explorer**, right-click the *StopSignDetection* project, and select **Add** > **Machine Learning** to open the Model Builder UI.</span></span>
1. <span data-ttu-id="e204b-183">对于此示例，方案为对象检测。</span><span class="sxs-lookup"><span data-stu-id="e204b-183">For this sample, the scenario is object detection.</span></span> <span data-ttu-id="e204b-184">在 Model Builder 的“方案”步骤中，选择“对象检测”方案。 </span><span class="sxs-lookup"><span data-stu-id="e204b-184">In the **Scenario** step of Model Builder, select the **Object Detection** scenario.</span></span>

![Visual Studio 中的“模型生成器”向导](./media/object-detection-model-builder/obj-det-scenario.png)

> <span data-ttu-id="e204b-186">如果在方案列表中看不到“对象检测”，则可能需要[更新 Model Builder 的版本](https://marketplace.visualstudio.com/items?itemName=MLNET.07)。</span><span class="sxs-lookup"><span data-stu-id="e204b-186">If you don't see *Object Detection* in the list of scenarios, you may need to [update your version of Model Builder](https://marketplace.visualstudio.com/items?itemName=MLNET.07).</span></span>

## <a name="choose-the-training-environment"></a><span data-ttu-id="e204b-187">选择训练环境</span><span class="sxs-lookup"><span data-stu-id="e204b-187">Choose the training environment</span></span>

<span data-ttu-id="e204b-188">目前，Model Builder 仅支持使用 Azure 机器学习来训练对象检测模型，因此默认情况下会选择 Azure 训练环境。</span><span class="sxs-lookup"><span data-stu-id="e204b-188">Currently, Model Builder supports training object detection models with Azure Machine Learning only, so the Azure training environment is selected by default.</span></span>

![Azure 训练环境选择](./media/object-detection-model-builder/obj-det-environment.png)

<span data-ttu-id="e204b-190">若要使用 Azure ML 训练模型，必须从 Model Builder 创建 Azure ML 试验。</span><span class="sxs-lookup"><span data-stu-id="e204b-190">To train a model using Azure ML, you must create an Azure ML experiment from Model Builder.</span></span>

<span data-ttu-id="e204b-191">Azure ML 试验是一种封装一次或多次机器学习训练运行的配置和结果的资源。</span><span class="sxs-lookup"><span data-stu-id="e204b-191">An **Azure ML experiment** is a resource that encapsulates the configuration and results for one or more machine learning training runs.</span></span>

<span data-ttu-id="e204b-192">若要创建 Azure ML 试验，首先需要在 Azure 中配置环境。</span><span class="sxs-lookup"><span data-stu-id="e204b-192">To create an Azure ML experiment, you first need to configure your environment in Azure.</span></span> <span data-ttu-id="e204b-193">若要运行试验，需要具备以下条件：</span><span class="sxs-lookup"><span data-stu-id="e204b-193">An experiment needs the following to run:</span></span>

- <span data-ttu-id="e204b-194">Azure 订阅</span><span class="sxs-lookup"><span data-stu-id="e204b-194">An Azure subscription</span></span>
- <span data-ttu-id="e204b-195">**工作区**：一种 Azure ML 资源，可集中放置训练运行过程中创建的所有 Azure ML 资源和项目。</span><span class="sxs-lookup"><span data-stu-id="e204b-195">A **workspace**: an Azure ML resource that provides a central place for all Azure ML resources and artifacts created as part of a training run.</span></span>
- <span data-ttu-id="e204b-196">**计算**：Azure 机器学习计算是基于云的 Linux VM，用于训练。</span><span class="sxs-lookup"><span data-stu-id="e204b-196">A **compute**: an Azure Machine Learning compute is a cloud-based Linux VM used for training.</span></span> <span data-ttu-id="e204b-197">详细了解 [Model Builder 支持的计算类型](../resources/azure-training-concepts-model-builder.md#what-is-an-azure-machine-learning-compute)。</span><span class="sxs-lookup"><span data-stu-id="e204b-197">Learn more about [compute types supported by Model Builder](../resources/azure-training-concepts-model-builder.md#what-is-an-azure-machine-learning-compute).</span></span>

### <a name="set-up-an-azure-ml-workspace"></a><span data-ttu-id="e204b-198">设置 Azure ML 工作区</span><span class="sxs-lookup"><span data-stu-id="e204b-198">Set up an Azure ML workspace</span></span>

<span data-ttu-id="e204b-199">配置环境：</span><span class="sxs-lookup"><span data-stu-id="e204b-199">To configure your environment:</span></span>

1. <span data-ttu-id="e204b-200">选择“设置工作区”按钮。</span><span class="sxs-lookup"><span data-stu-id="e204b-200">Select the **Set up workspace** button.</span></span>
1. <span data-ttu-id="e204b-201">在“新建试验”对话框中，选择你的 Azure 订阅。</span><span class="sxs-lookup"><span data-stu-id="e204b-201">In the **Create new experiment** dialog, select your Azure subscription.</span></span>
1. <span data-ttu-id="e204b-202">选择现有的工作区或新建一个 Azure ML 工作区。</span><span class="sxs-lookup"><span data-stu-id="e204b-202">Select an existing workspace or create a new Azure ML workspace.</span></span>

    <span data-ttu-id="e204b-203">新建工作区时，将预配以下资源：</span><span class="sxs-lookup"><span data-stu-id="e204b-203">When you create a new workspace, the following resources are provisioned:</span></span>

    - <span data-ttu-id="e204b-204">Azure 机器学习工作区</span><span class="sxs-lookup"><span data-stu-id="e204b-204">Azure Machine Learning workspace</span></span>
    - <span data-ttu-id="e204b-205">Azure 存储</span><span class="sxs-lookup"><span data-stu-id="e204b-205">Azure Storage</span></span>
    - <span data-ttu-id="e204b-206">Azure Application Insights</span><span class="sxs-lookup"><span data-stu-id="e204b-206">Azure Application Insights</span></span>
    - <span data-ttu-id="e204b-207">Azure 容器注册表</span><span class="sxs-lookup"><span data-stu-id="e204b-207">Azure Container Registry</span></span>
    - <span data-ttu-id="e204b-208">Azure Key Vault</span><span class="sxs-lookup"><span data-stu-id="e204b-208">Azure Key Vault</span></span>

    <span data-ttu-id="e204b-209">因此，此过程可能需要几分钟的时间。</span><span class="sxs-lookup"><span data-stu-id="e204b-209">As a result, this process may take a few minutes.</span></span>

1. <span data-ttu-id="e204b-210">选择现有计算，或创建新的 Azure ML 计算。</span><span class="sxs-lookup"><span data-stu-id="e204b-210">Select an existing compute or create a new Azure ML compute.</span></span> <span data-ttu-id="e204b-211">此过程可能需要几分钟时间。</span><span class="sxs-lookup"><span data-stu-id="e204b-211">This process may take a few minutes.</span></span>
1. <span data-ttu-id="e204b-212">保留默认试验名称，选择“创建”。</span><span class="sxs-lookup"><span data-stu-id="e204b-212">Leave the default experiment name and select **Create**.</span></span>

    ![Azure 工作区“设置”对话框](./media/object-detection-model-builder/azure-dialog.png)

<span data-ttu-id="e204b-214">你已创建第一个试验，并已在工作区中注册该试验名称。</span><span class="sxs-lookup"><span data-stu-id="e204b-214">The first experiment is created, and the experiment name is registered in the workspace.</span></span> <span data-ttu-id="e204b-215">如果任何后续运行使用相同的试验名称，则该运行将记录为同一试验的一部分。</span><span class="sxs-lookup"><span data-stu-id="e204b-215">Any subsequent runs (if the same experiment name is used ) are logged as part of the same experiment.</span></span> <span data-ttu-id="e204b-216">否则会创建新试验。</span><span class="sxs-lookup"><span data-stu-id="e204b-216">Otherwise, a new experiment is created.</span></span>

<span data-ttu-id="e204b-217">对配置满意后，选择 Model Builder 中的“下一步”按钮，以转到“数据”步骤。</span><span class="sxs-lookup"><span data-stu-id="e204b-217">If you’re satisfied with your configuration, select the **Next step** button in Model Builder to move to the **Data** step.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="e204b-218">加载数据</span><span class="sxs-lookup"><span data-stu-id="e204b-218">Load the data</span></span>

<span data-ttu-id="e204b-219">在 Model Builder 的“数据”步骤中，选择你的训练数据集。</span><span class="sxs-lookup"><span data-stu-id="e204b-219">In the **Data** step of Model Builder, you will select your training dataset.</span></span>

><span data-ttu-id="e204b-220">Model Builder 目前只接受 VoTT 生成的 JSON 格式，但该团队计划在将来添加对更多格式的支持。</span><span class="sxs-lookup"><span data-stu-id="e204b-220">Model Builder currently only accepts the format of JSON generated by VoTT, but the team plans to add support for more formats in the future.</span></span> <span data-ttu-id="e204b-221">如果希望 Model Builder 中支持某种对象检测数据集格式，请在 [GitHub](https://github.com/dotnet/machinelearning-modelbuilder/issues/new?assignees=&labels=&template=data_suggestion.md&title=) 上提交反馈。</span><span class="sxs-lookup"><span data-stu-id="e204b-221">If there is a dataset format for object detection that you’d like to see supported in Model Builder, leave your feedback on [GitHub](https://github.com/dotnet/machinelearning-modelbuilder/issues/new?assignees=&labels=&template=data_suggestion.md&title=).</span></span>

1. <span data-ttu-id="e204b-222">选择“选择文件夹”文本框旁边的按钮，并使用文件资源管理器查找 `StopSignObjDetection-export.json`，该内容应位于 Stop-Signs/vott-json-export 目录中。</span><span class="sxs-lookup"><span data-stu-id="e204b-222">Select the button next to the **Select a folder** text box and use the File Explorer to find the `StopSignObjDetection-export.json` which should be located in the *Stop-Signs/vott-json-export* directory.</span></span>

    ![Model Builder“数据”步骤](./media/object-detection-model-builder/obj-det-data.png)

1. <span data-ttu-id="e204b-224">数据在“数据预览”中正确显示时，选择“下一步”，以转到“训练”步骤。</span><span class="sxs-lookup"><span data-stu-id="e204b-224">If your data looks correct in the **Data Preview**, select **Next step** to move on to the **Train** step.</span></span>

## <a name="train-the-model"></a><span data-ttu-id="e204b-225">定型模型</span><span class="sxs-lookup"><span data-stu-id="e204b-225">Train the model</span></span>

<span data-ttu-id="e204b-226">下一步是训练模型。</span><span class="sxs-lookup"><span data-stu-id="e204b-226">The next step is to train your model.</span></span>

<span data-ttu-id="e204b-227">在 Model Builder“训练”屏幕中，选择“开始训练”按钮。</span><span class="sxs-lookup"><span data-stu-id="e204b-227">In the Model Builder **Train** screen, select the **Start training** button.</span></span>

<span data-ttu-id="e204b-228">此时，数据将上传到 Azure 存储，训练过程在 Azure ML 中开始。</span><span class="sxs-lookup"><span data-stu-id="e204b-228">At this point, your data is uploaded to Azure Storage and the training process begins in Azure ML.</span></span>

><span data-ttu-id="e204b-229">训练过程需要一定的时间，并且根据所选计算的大小和数据量，所需时间可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="e204b-229">The training process takes some time, and the amount of time may vary depending on the size of compute selected as well as the amount of data.</span></span> <span data-ttu-id="e204b-230">第一次在 Azure 中对模型进行训练时，由于必须预配资源，训练时间可能稍长。</span><span class="sxs-lookup"><span data-stu-id="e204b-230">The first time a model is trained in Azure, you can expect a slightly longer training time because resources have to be provisioned.</span></span> <span data-ttu-id="e204b-231">对于这个包含 50 个图像的示例而言，训练大约需要 16 分钟。</span><span class="sxs-lookup"><span data-stu-id="e204b-231">For this sample of 50 images, training took about 16 minutes.</span></span>

<span data-ttu-id="e204b-232">可以在 Visual Studio 中选择“监视 Azure 门户中的当前运行”链接，以在 Azure 机器学习门户中跟踪运行进度。</span><span class="sxs-lookup"><span data-stu-id="e204b-232">You can track the progress of your runs in the Azure Machine Learning portal by selecting the **Monitor current run in Azure portal** link in Visual Studio.</span></span>

<span data-ttu-id="e204b-233">训练完成后，选择“下一步”按钮，转到“评估”步骤。</span><span class="sxs-lookup"><span data-stu-id="e204b-233">Once training is complete, select the **Next step** button to move on to the **Evaluate** step.</span></span>

## <a name="evaluate-the-model"></a><span data-ttu-id="e204b-234">评估模型</span><span class="sxs-lookup"><span data-stu-id="e204b-234">Evaluate the model</span></span>

<span data-ttu-id="e204b-235">在“评估”屏幕中，可以大致了解训练过程生成的结果，包括模型准确率。</span><span class="sxs-lookup"><span data-stu-id="e204b-235">In the Evaluate screen, you get an overview of the results from the training process, including the model accuracy.</span></span>

![Model Builder“评估”步骤](./media/object-detection-model-builder/obj-det-evaluate.png)

<span data-ttu-id="e204b-237">在此示例中，准确率显示 100%，这意味着，数据集中的图像太少，很可能会导致模型过度拟合。</span><span class="sxs-lookup"><span data-stu-id="e204b-237">In this case, the accuracy says 100%, which means that the model is more than likely *overfit* due to too few images in the dataset.</span></span>

<span data-ttu-id="e204b-238">可以使用“试用模型”体验，快速检查模型是否按预期工作。</span><span class="sxs-lookup"><span data-stu-id="e204b-238">You can use the **Try your model** experience to quickly check whether your model is performing as expected.</span></span>

<span data-ttu-id="e204b-239">选择“浏览图像”，并提供测试图像，最好是模型未在训练过程中使用的图像。</span><span class="sxs-lookup"><span data-stu-id="e204b-239">Select **Browse an image** and provide a test image, preferably one that the model did not use as part of training.</span></span>

![Model Builder“试用模型”](./media/object-detection-model-builder/obj-det-try-model.png)

<span data-ttu-id="e204b-241">检测到的每个边界框上显示的分数表示检测到的对象的置信度。</span><span class="sxs-lookup"><span data-stu-id="e204b-241">The score shown on each detected bounding box indicates the **confidence** of the detected object.</span></span> <span data-ttu-id="e204b-242">例如，在上面的屏幕截图中，围绕停车标志的边界框上的分数表示，该模型对于检测到的对象是停车标志的确定程度为 99%。</span><span class="sxs-lookup"><span data-stu-id="e204b-242">For instance, in the screenshot above, the score on the bounding box around the stop sign indicates that the model is 99% sure that the detected object is a stop sign.</span></span>

<span data-ttu-id="e204b-243">“分数阈值”可以通过阈值滑块增加或降低，该阈值将根据分数添加和删除检测到的对象。</span><span class="sxs-lookup"><span data-stu-id="e204b-243">The **Score threshold**, which can be increased or decreased with the threshold slider, will add and remove detected objects based on their scores.</span></span> <span data-ttu-id="e204b-244">例如，如果阈值为 .51，则模型将仅显示置信度分数为 .51 或更高的对象。</span><span class="sxs-lookup"><span data-stu-id="e204b-244">For instance, if the threshold is .51, then the model will only show objects that have a confidence score of .51 or above.</span></span> <span data-ttu-id="e204b-245">增加阈值时，你会看到检测到的对象变少，而降低阈值时，你会看到检测到的对象变多。</span><span class="sxs-lookup"><span data-stu-id="e204b-245">As you increase the threshold, you will see less detected objects, and as you decrease the threshold, you will see more detected objects.</span></span>

<span data-ttu-id="e204b-246">如果对准确率指标不满意，尝试提高准确率的一个简单方法是使用更多的数据。</span><span class="sxs-lookup"><span data-stu-id="e204b-246">If you're not satisfied with your accuracy metrics, one easy way to try to improve model accuracy is to use more data.</span></span> <span data-ttu-id="e204b-247">如果满意，则选择“下一步”链接，以转到 Model Builder 中的“代码”步骤。</span><span class="sxs-lookup"><span data-stu-id="e204b-247">Otherwise, select the **Next step** link to move on to the **Code** step in Model Builder.</span></span>

## <a name="add-the-code-to-make-predictions"></a><span data-ttu-id="e204b-248">添加代码进行预测</span><span class="sxs-lookup"><span data-stu-id="e204b-248">Add the code to make predictions</span></span>

<span data-ttu-id="e204b-249">培训期间会创建两个项目。</span><span class="sxs-lookup"><span data-stu-id="e204b-249">Two projects are created as a result of the training process.</span></span>

- <span data-ttu-id="e204b-250">*StopSignDetectionML.ConsoleApp*：一个 C# .NET Core 控制台应用程序，其中包含模型训练代码和示例使用代码。</span><span class="sxs-lookup"><span data-stu-id="e204b-250">*StopSignDetectionML.ConsoleApp*: A C# .NET Core Console application that contains the model training code and sample consumption code.</span></span>
- <span data-ttu-id="e204b-251">*StopSignDetectionML.Model*：一个 .NET Standard 类库，包含定义输入和输出模型数据架构的数据模型、训练期间性能最佳的模型的保存版本，以及用于执行预测的帮助程序类（称为 `ConsumeModel`）。</span><span class="sxs-lookup"><span data-stu-id="e204b-251">*StopSignDetectionML.Model*: A .NET Standard class library containing the data models that define the schema of input and output model data, the saved version of the best performing model during training, and a helper class called `ConsumeModel` to make predictions.</span></span>

1. <span data-ttu-id="e204b-252">在模型生成器的“代码”步骤中，选择“添加项目”，以将自动生成的项目添加到解决方案。</span><span class="sxs-lookup"><span data-stu-id="e204b-252">In the code step of Model Builder, select **Add Projects** to add the auto-generated projects to the solution.</span></span>
1. <span data-ttu-id="e204b-253">在 StopSignDetection 项目中打开 Program.cs 文件，并在该文件的顶部添加以下 using 语句，以引用 StopSignDetectionML.Model 项目：</span><span class="sxs-lookup"><span data-stu-id="e204b-253">Open the *Program.cs* file in the *StopSignDetection* project, and add the following using statement at the top of the file to reference the *StopSignDetectionML.Model* project:</span></span>

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L2)]

1. <span data-ttu-id="e204b-254">下载以下[测试图像](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/test-image1.jpeg)。</span><span class="sxs-lookup"><span data-stu-id="e204b-254">Download the following [test image](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/test-image1.jpeg).</span></span>
1. <span data-ttu-id="e204b-255">在应用程序的 `Main` 方法中将 `ImageSource` 属性设置为测试图像的文件路径，然后创建 `ModelInput` 类的新实例。</span><span class="sxs-lookup"><span data-stu-id="e204b-255">Create a new instance of the `ModelInput` class with the `ImageSource` property set to the filepath of your test image inside the `Main` method of your application.</span></span> <span data-ttu-id="e204b-256">用以下代码替换“Hello World”语句：</span><span class="sxs-lookup"><span data-stu-id="e204b-256">Replace the "Hello World" statement with the following code:</span></span>

    [!code-csharp [InputData](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L10-L15)]

1. <span data-ttu-id="e204b-257">使用 `Predict` 类中的 `ConsumeModel` 方法加载训练的模型，为模型创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)，并对新数据进行预测。</span><span class="sxs-lookup"><span data-stu-id="e204b-257">Use the `Predict` method from the `ConsumeModel` class to load the trained model, create a [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) for the model, and make predictions on new data.</span></span> <span data-ttu-id="e204b-258">在 `ModelInput` 语句下面添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="e204b-258">Add the following code below the `ModelInput` statement:</span></span>

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L15)]

1. <span data-ttu-id="e204b-259">通过添加以下代码，打印出预测的输出，其中包括标签、坐标和分数：</span><span class="sxs-lookup"><span data-stu-id="e204b-259">Print out the Prediction's output, including the label, coordinates, and score by adding the following code:</span></span>

    [!code-csharp [PrintResults](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L17-L18)]

1. <span data-ttu-id="e204b-260">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="e204b-260">Run the application.</span></span>

    <span data-ttu-id="e204b-261">该程序生成的输出应类似于下面的代码段：</span><span class="sxs-lookup"><span data-stu-id="e204b-261">The output generated by the program should look similar to the snippet below:</span></span>

    ```bash
    Predicted Boxes:

    Top: 89.453415, Left: 481.95343, Right: 724.8073, Bottom: 388.32385, Label: Stop-Sign, Score: 0.99539465
    ```

<span data-ttu-id="e204b-262">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="e204b-262">Congratulations!</span></span> <span data-ttu-id="e204b-263">你已成功使用 Model Builder 生成用来检测图像中停车标志的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="e204b-263">You've successfully built a machine learning model to detect stop signs in images using Model Builder.</span></span> <span data-ttu-id="e204b-264">有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub 存储库中找到。</span><span class="sxs-lookup"><span data-stu-id="e204b-264">You can find the source code for this tutorial at the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub repository.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e204b-265">其他资源</span><span class="sxs-lookup"><span data-stu-id="e204b-265">Additional resources</span></span>

<span data-ttu-id="e204b-266">若要详细了解本教程中所述的主题，请访问以下资源:</span><span class="sxs-lookup"><span data-stu-id="e204b-266">To learn more about topics mentioned in this tutorial, visit the following resources:</span></span>

- [<span data-ttu-id="e204b-267">模型生成器应用场景</span><span class="sxs-lookup"><span data-stu-id="e204b-267">Model Builder Scenarios</span></span>](../automate-training-with-model-builder.md#scenario)
- [<span data-ttu-id="e204b-268">在 ML.NET 中使用 ONNX 检测对象</span><span class="sxs-lookup"><span data-stu-id="e204b-268">Object Detection using ONNX in ML.NET</span></span>](object-detection-onnx.md)
