---
title: 教程：使用迁移学习自动进行肉眼检查
description: 本教程演示如何使用图像检测 API 将混凝土表面的图像分类为有裂缝或无裂缝，以使用迁移学习在 ML.NET 中训练 TensorFlow 深度学习模型。
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 92c768c429a2eae22071556cc13c7457a7cf26df
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514510"
---
# <a name="tutorial-automated-visual-inspection-using-transfer-learning-with-the-mlnet-image-classification-api"></a><span data-ttu-id="0fe94-103">教程：通过 ML.NET 图像分类 API 使用迁移学习自动进行肉眼检查</span><span class="sxs-lookup"><span data-stu-id="0fe94-103">Tutorial: Automated visual inspection using transfer learning with the ML.NET Image Classification API</span></span>

<span data-ttu-id="0fe94-104">了解如何使用迁移学习、预先训练的 TensorFlow 模型和 ML.NET 图像分类 API 将混凝土表面的图像分类为有裂缝或无裂缝，以训练自定义深度学习模型。</span><span class="sxs-lookup"><span data-stu-id="0fe94-104">Learn how to train a custom deep learning model using transfer learning, a pretrained TensorFlow model and the ML.NET Image Classification API to classify images of concrete surfaces as cracked or uncracked.</span></span>

<span data-ttu-id="0fe94-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="0fe94-105">In this tutorial, you learn how to:</span></span>
> [!div class="checklist"]
>
> - <span data-ttu-id="0fe94-106">了解问题</span><span class="sxs-lookup"><span data-stu-id="0fe94-106">Understand the problem</span></span>
> - <span data-ttu-id="0fe94-107">了解 ML.NET 图像分类 API</span><span class="sxs-lookup"><span data-stu-id="0fe94-107">Learn about ML.NET Image Classification API</span></span>
> - <span data-ttu-id="0fe94-108">了解预先训练的模型</span><span class="sxs-lookup"><span data-stu-id="0fe94-108">Understand the pretrained model</span></span>
> - <span data-ttu-id="0fe94-109">使用迁移学习训练自定义的 TensorFlow 图像分类模型</span><span class="sxs-lookup"><span data-stu-id="0fe94-109">Use transfer learning to train a custom TensorFlow image classification model</span></span>
> - <span data-ttu-id="0fe94-110">使用自定义模型对图像进行分类</span><span class="sxs-lookup"><span data-stu-id="0fe94-110">Classify images with the custom model</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0fe94-111">先决条件</span><span class="sxs-lookup"><span data-stu-id="0fe94-111">Prerequisites</span></span>

- <span data-ttu-id="0fe94-112">安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-112">[Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) or later or Visual Studio 2017 version 15.6 or later with the ".NET Core cross-platform development" workload installed.</span></span>

## <a name="image-classification-transfer-learning-sample-overview"></a><span data-ttu-id="0fe94-113">图像分类迁移学习示例概述</span><span class="sxs-lookup"><span data-stu-id="0fe94-113">Image classification transfer learning sample overview</span></span>

<span data-ttu-id="0fe94-114">此示例是一个 C# .NET Core 控制台应用程序，它使用预先训练的深度学习 TensorFlow 模型对图像进行分类。</span><span class="sxs-lookup"><span data-stu-id="0fe94-114">This sample is a C# .NET Core console application that classifies images using a pretrained deep learning TensorFlow model.</span></span> <span data-ttu-id="0fe94-115">可在[示例浏览器](/samples/dotnet/machinelearning-samples/mlnet-image-classification-transfer-learning/)上找到此示例的代码。</span><span class="sxs-lookup"><span data-stu-id="0fe94-115">The code for this sample can be found on the [samples browser](/samples/dotnet/machinelearning-samples/mlnet-image-classification-transfer-learning/).</span></span>

## <a name="understand-the-problem"></a><span data-ttu-id="0fe94-116">了解问题</span><span class="sxs-lookup"><span data-stu-id="0fe94-116">Understand the problem</span></span>

<span data-ttu-id="0fe94-117">图像分类是一种计算机视觉问题。</span><span class="sxs-lookup"><span data-stu-id="0fe94-117">Image classification is a computer vision problem.</span></span> <span data-ttu-id="0fe94-118">图像分类使用图像作为输入，并将其分类为规定的类。</span><span class="sxs-lookup"><span data-stu-id="0fe94-118">Image classification takes an image as input and categorizes it into a prescribed class.</span></span> <span data-ttu-id="0fe94-119">图像分类模型通常使用深度学习和神经网络进行训练。</span><span class="sxs-lookup"><span data-stu-id="0fe94-119">Image classification models are commonly trained using deep learning and neural networks.</span></span> <span data-ttu-id="0fe94-120">有关详细信息，请参阅[深度学习与机器学习](/azure/machine-learning/concept-deep-learning-vs-machine-learning)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-120">See [Deep learning vs. machine learning](/azure/machine-learning/concept-deep-learning-vs-machine-learning) for more information.</span></span>

<span data-ttu-id="0fe94-121">图像分类很有用的一些情况包括：</span><span class="sxs-lookup"><span data-stu-id="0fe94-121">Some scenarios where image classification is useful include:</span></span>

- <span data-ttu-id="0fe94-122">面部识别</span><span class="sxs-lookup"><span data-stu-id="0fe94-122">Facial recognition</span></span>
- <span data-ttu-id="0fe94-123">情感检测</span><span class="sxs-lookup"><span data-stu-id="0fe94-123">Emotion detection</span></span>
- <span data-ttu-id="0fe94-124">医疗诊断</span><span class="sxs-lookup"><span data-stu-id="0fe94-124">Medical diagnosis</span></span>
- <span data-ttu-id="0fe94-125">路标检测</span><span class="sxs-lookup"><span data-stu-id="0fe94-125">Landmark detection</span></span>

<span data-ttu-id="0fe94-126">本教程训练自定义图像分类模型，以便对桥面自动执行肉眼检查，以识别由裂缝损坏的结构。</span><span class="sxs-lookup"><span data-stu-id="0fe94-126">This tutorial trains a custom image classification model to perform automated visual inspection of bridge decks to identify structures that are damaged by cracks.</span></span>

## <a name="mlnet-image-classification-api"></a><span data-ttu-id="0fe94-127">ML.NET 图像分类 API</span><span class="sxs-lookup"><span data-stu-id="0fe94-127">ML.NET Image Classification API</span></span>

<span data-ttu-id="0fe94-128">ML.NET 提供了各种执行图像分类的方式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-128">ML.NET provides various ways of performing image classification.</span></span> <span data-ttu-id="0fe94-129">本教程使用图像分类 API 应用迁移学习。</span><span class="sxs-lookup"><span data-stu-id="0fe94-129">This tutorial applies transfer learning using the Image Classification API.</span></span> <span data-ttu-id="0fe94-130">图像分类 API 使用 [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET)，即一个为 TensorFlow C++ API 提供 C# 绑定的低级别库。</span><span class="sxs-lookup"><span data-stu-id="0fe94-130">The Image Classification API makes use of [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET), a low-level library that provides C# bindings for the TensorFlow C++ API.</span></span>

## <a name="what-is-transfer-learning"></a><span data-ttu-id="0fe94-131">什么是迁移学习？</span><span class="sxs-lookup"><span data-stu-id="0fe94-131">What is transfer learning?</span></span>

<span data-ttu-id="0fe94-132">迁移学习应用从解决一个问题到解决其他相关问题所获得的知识。</span><span class="sxs-lookup"><span data-stu-id="0fe94-132">Transfer learning applies knowledge gained from solving one problem to another related problem.</span></span>

<span data-ttu-id="0fe94-133">从头开始训练深度学习模型需要设置多个参数、大量已标记的训练数据和海量计算资源（数百个 GPU 小时）。</span><span class="sxs-lookup"><span data-stu-id="0fe94-133">Training a deep learning model from scratch requires setting several parameters, a large amount of labeled training data, and a vast amount of compute resources (hundreds of GPU hours).</span></span> <span data-ttu-id="0fe94-134">结合使用预先训练的模型与迁移学习可让你快速完成训练过程。</span><span class="sxs-lookup"><span data-stu-id="0fe94-134">Using a pretrained model along with transfer learning allows you to shortcut the training process.</span></span>

## <a name="training-process"></a><span data-ttu-id="0fe94-135">训练过程</span><span class="sxs-lookup"><span data-stu-id="0fe94-135">Training process</span></span>

<span data-ttu-id="0fe94-136">图像分类 API 通过加载预先训练的 TensorFlow 模型启动训练过程。</span><span class="sxs-lookup"><span data-stu-id="0fe94-136">The Image Classification API starts the training process by loading a pretrained TensorFlow model.</span></span> <span data-ttu-id="0fe94-137">该训练过程由两个以下步骤组成：</span><span class="sxs-lookup"><span data-stu-id="0fe94-137">The training process consists of two steps:</span></span>

1. <span data-ttu-id="0fe94-138">瓶颈阶段</span><span class="sxs-lookup"><span data-stu-id="0fe94-138">Bottleneck phase</span></span>
2. <span data-ttu-id="0fe94-139">训练阶段</span><span class="sxs-lookup"><span data-stu-id="0fe94-139">Training phase</span></span>

![训练步骤](./media/image-classification-api-transfer-learning/training.png)

### <a name="bottleneck-phase"></a><span data-ttu-id="0fe94-141">瓶颈阶段</span><span class="sxs-lookup"><span data-stu-id="0fe94-141">Bottleneck phase</span></span>

<span data-ttu-id="0fe94-142">在瓶颈阶段，会加载训练图像集，并将像素值用作预先训练模型冻结层的输入或功能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-142">During the bottleneck phase, the set of training images is loaded and the pixel values are used as input, or features, for the frozen layers of the pretrained model.</span></span> <span data-ttu-id="0fe94-143">冻结层包含神经网络的所有层，最高可达倒数第二层，通常也称为瓶颈层。</span><span class="sxs-lookup"><span data-stu-id="0fe94-143">The frozen layers include all of the layers in the neural network up to the penultimate layer, informally known as the bottleneck layer.</span></span> <span data-ttu-id="0fe94-144">这些层被称为冻结层，因为这些层中不会出现任何训练并且操作是直通的。</span><span class="sxs-lookup"><span data-stu-id="0fe94-144">These layers are referred to as frozen because no training will occur on these layers and operations are pass-through.</span></span> <span data-ttu-id="0fe94-145">帮助模型区分不同类的低级别模式就是在这些冻结层上进行计算的。</span><span class="sxs-lookup"><span data-stu-id="0fe94-145">It's at these frozen layers where the lower-level patterns that help a model differentiate between the different classes are computed.</span></span> <span data-ttu-id="0fe94-146">层数越大，此步骤的计算就越密集。</span><span class="sxs-lookup"><span data-stu-id="0fe94-146">The larger the number of layers, the more computationally intensive this step is.</span></span> <span data-ttu-id="0fe94-147">幸运的是，由于这是一种一次性计算，因此在使用不同参数进行试验时，可以缓存结果并将其用于后续运行。</span><span class="sxs-lookup"><span data-stu-id="0fe94-147">Fortunately, since this is a one-time calculation, the results can be cached and used in later runs when experimenting with different parameters.</span></span>

### <a name="training-phase"></a><span data-ttu-id="0fe94-148">训练阶段</span><span class="sxs-lookup"><span data-stu-id="0fe94-148">Training phase</span></span>

<span data-ttu-id="0fe94-149">计算瓶颈阶段的输出值后，这些值将用作输入以重新训练模型的最后一层。</span><span class="sxs-lookup"><span data-stu-id="0fe94-149">Once the output values from the bottleneck phase are computed, they are used as input to retrain the final layer of the model.</span></span> <span data-ttu-id="0fe94-150">此过程是一种迭代过程，并且会运行模型参数指定的次数。</span><span class="sxs-lookup"><span data-stu-id="0fe94-150">This process is iterative and runs for the number of times specified by model parameters.</span></span> <span data-ttu-id="0fe94-151">在每次运行过程中，都将评估损失和准确度。</span><span class="sxs-lookup"><span data-stu-id="0fe94-151">During each run, the loss and accuracy are evaluated.</span></span> <span data-ttu-id="0fe94-152">然后，对模型进行适当调整，以最大程度地降低损失并最大程度地提高准确度。</span><span class="sxs-lookup"><span data-stu-id="0fe94-152">Then, the appropriate adjustments are made to improve the model with the goal of minimizing the loss and maximizing the accuracy.</span></span> <span data-ttu-id="0fe94-153">训练完成后，将输出两种模型格式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-153">Once training is finished, two model formats are output.</span></span> <span data-ttu-id="0fe94-154">其中之一是模型的 `.pb` 版本，另一种则是模型的 `.zip` ML.NET 序列化版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-154">One of them is the `.pb` version of the model and the other is the `.zip` ML.NET serialized version of the model.</span></span> <span data-ttu-id="0fe94-155">如果在 ML.NET 支持的环境中操作，建议使用模型的 `.zip` 版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-155">When working in environments supported by ML.NET, it is recommended to use the `.zip` version of the model.</span></span> <span data-ttu-id="0fe94-156">但如果在不支持 ML.NET 的环境中操作，则可以选择使用 `.pb` 版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-156">However, in environments where ML.NET is not supported, you have the option of using the `.pb` version.</span></span>

## <a name="understand-the-pretrained-model"></a><span data-ttu-id="0fe94-157">了解预先训练的模型</span><span class="sxs-lookup"><span data-stu-id="0fe94-157">Understand the pretrained model</span></span>

<span data-ttu-id="0fe94-158">本教程中使用的预先训练模型是残差网络 (ResNet) v2 模型的 101 层变体。</span><span class="sxs-lookup"><span data-stu-id="0fe94-158">The pretrained model used in this tutorial is the 101-layer variant of the Residual Network (ResNet) v2 model.</span></span> <span data-ttu-id="0fe94-159">原始模型经过训练，可以将图像分为一千个类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-159">The original model is trained to classify images into a thousand categories.</span></span> <span data-ttu-id="0fe94-160">此模型将大小为 224 x 224 的图像作为输入，并输出其训练的每个类的类概率。</span><span class="sxs-lookup"><span data-stu-id="0fe94-160">The model takes as input an image of size 224 x 224 and outputs the class probabilities for each of the classes it's trained on.</span></span> <span data-ttu-id="0fe94-161">此模型的一部分用于使用自定义图像在两个类之间进行预测来训练新模型。</span><span class="sxs-lookup"><span data-stu-id="0fe94-161">Part of this model is used to train a new model using custom images to make predictions between two classes.</span></span>

## <a name="create-console-application"></a><span data-ttu-id="0fe94-162">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="0fe94-162">Create console application</span></span>

<span data-ttu-id="0fe94-163">在大致了解了迁移学习和图像分类 API 后，现在可以构建应用程序。</span><span class="sxs-lookup"><span data-stu-id="0fe94-163">Now that you have a general understanding of transfer learning and the Image Classification API, it's time to build the application.</span></span>

1. <span data-ttu-id="0fe94-164">创建名为“DeepLearning_ImageClassification_Binary”的 C# .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="0fe94-164">Create a **C# .NET Core Console Application** called "DeepLearning_ImageClassification_Binary".</span></span>
1. <span data-ttu-id="0fe94-165">安装“Microsoft.ML”NuGet 包：</span><span class="sxs-lookup"><span data-stu-id="0fe94-165">Install the **Microsoft.ML** NuGet Package:</span></span>

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. <span data-ttu-id="0fe94-166">在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”。</span><span class="sxs-lookup"><span data-stu-id="0fe94-166">In Solution Explorer, right-click on your project and select **Manage NuGet Packages**.</span></span>
    1. <span data-ttu-id="0fe94-167">选择“nuget.org”作为“包源”。</span><span class="sxs-lookup"><span data-stu-id="0fe94-167">Choose "nuget.org" as the Package source.</span></span>
    1. <span data-ttu-id="0fe94-168">选择“浏览”选项卡。</span><span class="sxs-lookup"><span data-stu-id="0fe94-168">Select the **Browse** tab.</span></span>
    1. <span data-ttu-id="0fe94-169">选中“包括预发行版”复选框。</span><span class="sxs-lookup"><span data-stu-id="0fe94-169">Check the **Include prerelease** checkbox.</span></span>
    1. <span data-ttu-id="0fe94-170">搜索 Microsoft.ML。</span><span class="sxs-lookup"><span data-stu-id="0fe94-170">Search for **Microsoft.ML**.</span></span>
    1. <span data-ttu-id="0fe94-171">选择“安装”按钮  。</span><span class="sxs-lookup"><span data-stu-id="0fe94-171">Select the **Install** button.</span></span>
    1. <span data-ttu-id="0fe94-172">选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。</span><span class="sxs-lookup"><span data-stu-id="0fe94-172">Select the **OK** button on the **Preview Changes** dialog and then select the **I Accept** button on the **License Acceptance** dialog if you agree with the license terms for the packages listed.</span></span>
    1. <span data-ttu-id="0fe94-173">为 Microsoft.ML.Vision、SciSharp.TensorFlow.Redist 和 Microsoft.ML.ImageAnalytics NuGet 包重复上述步骤。  </span><span class="sxs-lookup"><span data-stu-id="0fe94-173">Repeat these steps for the **Microsoft.ML.Vision**, **SciSharp.TensorFlow.Redist**, and **Microsoft.ML.ImageAnalytics** NuGet packages.</span></span>

### <a name="prepare-and-understand-the-data"></a><span data-ttu-id="0fe94-174">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="0fe94-174">Prepare and understand the data</span></span>

> [!NOTE]
> <span data-ttu-id="0fe94-175">本教程的数据集来自由 Maguire, Marc、Dorafshan, Sattar 和 Thomas, Robert J. 共同撰写的“SDNET2018：机器学习应用程序的混凝土裂缝图像数据集”(2018)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-175">The datasets for this tutorial are from Maguire, Marc; Dorafshan, Sattar; and Thomas, Robert J., "SDNET2018: A concrete crack image dataset for machine learning applications" (2018).</span></span> <span data-ttu-id="0fe94-176">浏览所有数据集。</span><span class="sxs-lookup"><span data-stu-id="0fe94-176">Browse all Datasets.</span></span> <span data-ttu-id="0fe94-177">论文 48。</span><span class="sxs-lookup"><span data-stu-id="0fe94-177">Paper 48.</span></span> <https://digitalcommons.usu.edu/all_datasets/48>

<span data-ttu-id="0fe94-178">SDNET2018 是一个图像数据集，其中包含有裂缝和无裂缝混凝土结构（桥面、墙壁和人行道）的注释。</span><span class="sxs-lookup"><span data-stu-id="0fe94-178">SDNET2018 is an image dataset that contains annotations for cracked and non-cracked concrete structures (bridge decks, walls, and pavement).</span></span>

![SDNET2018 数据集桥面示例](./media/image-classification-api-transfer-learning/sdnet2018decksamples.png)

<span data-ttu-id="0fe94-180">这些数据组织为三个子目录：</span><span class="sxs-lookup"><span data-stu-id="0fe94-180">The data is organized in three subdirectories:</span></span>

- <span data-ttu-id="0fe94-181">D 包含桥面图像</span><span class="sxs-lookup"><span data-stu-id="0fe94-181">D contains bridge deck images</span></span>
- <span data-ttu-id="0fe94-182">P 包含人行道图像</span><span class="sxs-lookup"><span data-stu-id="0fe94-182">P contains pavement images</span></span>
- <span data-ttu-id="0fe94-183">W 包含墙壁图像</span><span class="sxs-lookup"><span data-stu-id="0fe94-183">W contains wall images</span></span>

<span data-ttu-id="0fe94-184">这些子目录中的每个子目录都包含两个额外的带前缀的子目录：</span><span class="sxs-lookup"><span data-stu-id="0fe94-184">Each of these subdirectories contains two additional prefixed subdirectories:</span></span>

- <span data-ttu-id="0fe94-185">C 是用于有裂缝的表面的前缀</span><span class="sxs-lookup"><span data-stu-id="0fe94-185">C is the prefix used for cracked surfaces</span></span>
- <span data-ttu-id="0fe94-186">U 是用于无裂缝的表面的前缀</span><span class="sxs-lookup"><span data-stu-id="0fe94-186">U is the prefix used for uncracked surfaces</span></span>

<span data-ttu-id="0fe94-187">本教程仅使用桥面图像。</span><span class="sxs-lookup"><span data-stu-id="0fe94-187">In this tutorial, only bridge deck images are used.</span></span>

1. <span data-ttu-id="0fe94-188">下载[数据集](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/assets.zip)并解压缩。</span><span class="sxs-lookup"><span data-stu-id="0fe94-188">Download the [dataset](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/assets.zip) and unzip.</span></span>
1. <span data-ttu-id="0fe94-189">在项目中创建名为“资产”的目录，用于保存数据集文件。</span><span class="sxs-lookup"><span data-stu-id="0fe94-189">Create a directory named "assets" in your project to save your dataset files.</span></span>
1. <span data-ttu-id="0fe94-190">将 CD 与 UD 子目录从最近解压缩的目录复制到“资产”目录  。</span><span class="sxs-lookup"><span data-stu-id="0fe94-190">Copy the *CD* and *UD* subdirectories from the recently unzipped directory to the *assets* directory.</span></span>

### <a name="create-input-and-output-classes"></a><span data-ttu-id="0fe94-191">创建输入和输出类</span><span class="sxs-lookup"><span data-stu-id="0fe94-191">Create input and output classes</span></span>

1. <span data-ttu-id="0fe94-192">打开 Program.cs 文件，并将文件顶部的现有 `using` 语句替换为以下内容：</span><span class="sxs-lookup"><span data-stu-id="0fe94-192">Open the *Program.cs* file and replace the existing `using` statements at the top of the file with the following:</span></span>

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L1-L7)]

1. <span data-ttu-id="0fe94-193">在 Program.cs 的 `Program` 类下，创建一个名为 `ImageData` 的类。</span><span class="sxs-lookup"><span data-stu-id="0fe94-193">Below the `Program` class in *Program.cs*, create a class called `ImageData`.</span></span> <span data-ttu-id="0fe94-194">此类用于表示最初加载的数据。</span><span class="sxs-lookup"><span data-stu-id="0fe94-194">This class is used to represent the initially loaded data.</span></span>

    [!code-csharp [ImageDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L137-L142)]

    <span data-ttu-id="0fe94-195">`ImageData` 包含以下属性：</span><span class="sxs-lookup"><span data-stu-id="0fe94-195">`ImageData` contains the following properties:</span></span>

    - <span data-ttu-id="0fe94-196">`ImagePath` 是存储图像的完全限定的路径。</span><span class="sxs-lookup"><span data-stu-id="0fe94-196">`ImagePath` is the fully qualified path where the image is stored.</span></span>
    - <span data-ttu-id="0fe94-197">`Label` 是图像所属的类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-197">`Label` is the category the image belongs to.</span></span> <span data-ttu-id="0fe94-198">这是要预测的值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-198">This is the value to predict.</span></span>

1. <span data-ttu-id="0fe94-199">为输入和输出数据创建类</span><span class="sxs-lookup"><span data-stu-id="0fe94-199">Create classes for your input and output data</span></span>

    1. <span data-ttu-id="0fe94-200">在 `ImageData` 类下，在名为 `ModelInput` 的新类中定义输入数据的架构。</span><span class="sxs-lookup"><span data-stu-id="0fe94-200">Below the `ImageData` class, define the schema of your input data in a new class called `ModelInput`.</span></span>

        [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L144-L153)]

        <span data-ttu-id="0fe94-201">`ModelInput` 包含以下属性：</span><span class="sxs-lookup"><span data-stu-id="0fe94-201">`ModelInput` contains the following properties:</span></span>

        - <span data-ttu-id="0fe94-202">`Image` 是图像的 `byte[]` 表示形式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-202">`Image` is the `byte[]` representation of the image.</span></span> <span data-ttu-id="0fe94-203">模型需要此类型的图像数据以供训练。</span><span class="sxs-lookup"><span data-stu-id="0fe94-203">The model expects image data to be of this type for training.</span></span>
        - <span data-ttu-id="0fe94-204">`LabelAsKey` 是 `Label` 的数值表示形式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-204">`LabelAsKey` is the numerical representation of the `Label`.</span></span>
        - <span data-ttu-id="0fe94-205">`ImagePath` 是存储图像的完全限定的路径。</span><span class="sxs-lookup"><span data-stu-id="0fe94-205">`ImagePath` is the fully qualified path where the image is stored.</span></span>
        - <span data-ttu-id="0fe94-206">`Label` 是图像所属的类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-206">`Label` is the category the image belongs to.</span></span> <span data-ttu-id="0fe94-207">这是要预测的值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-207">This is the value to predict.</span></span>

        <span data-ttu-id="0fe94-208">仅 `Image` 和 `LabelAsKey` 用于训练模型和进行预测。</span><span class="sxs-lookup"><span data-stu-id="0fe94-208">Only `Image` and `LabelAsKey` are used to train the model and make predictions.</span></span> <span data-ttu-id="0fe94-209">已保留 `ImagePath` 和 `Label` 属性，以方便访问原始图像文件名称和类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-209">The `ImagePath` and `Label` properties are kept for convenience to access the original image file name and category.</span></span>

    1. <span data-ttu-id="0fe94-210">然后，在 `ModelInput` 类下，在名为 `ModelOutput` 的新类中定义输出数据的架构。</span><span class="sxs-lookup"><span data-stu-id="0fe94-210">Then, below the `ModelInput` class, define the schema of your output data in a new class called `ModelOutput`.</span></span>

        [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L155-L162)]

        <span data-ttu-id="0fe94-211">`ModelOutput` 包含以下属性：</span><span class="sxs-lookup"><span data-stu-id="0fe94-211">`ModelOutput` contains the following properties:</span></span>

        - <span data-ttu-id="0fe94-212">`ImagePath` 是存储图像的完全限定的路径。</span><span class="sxs-lookup"><span data-stu-id="0fe94-212">`ImagePath` is the fully qualified path where the image is stored.</span></span>
        - <span data-ttu-id="0fe94-213">`Label` 是图像所属的原始类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-213">`Label` is the original category the image belongs to.</span></span> <span data-ttu-id="0fe94-214">这是要预测的值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-214">This is the value to predict.</span></span>
        - <span data-ttu-id="0fe94-215">`PredictedLabel` 是模型预测的值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-215">`PredictedLabel` is the value predicted by the model.</span></span>

        <span data-ttu-id="0fe94-216">与 `ModelInput` 类似，只需 `PredictedLabel` 即可进行预测，因为它包含由模型作出的预测。</span><span class="sxs-lookup"><span data-stu-id="0fe94-216">Similar to `ModelInput`, only the `PredictedLabel` is required to make predictions since it contains the prediction made by the model.</span></span> <span data-ttu-id="0fe94-217">已保留 `ImagePath` 和 `Label` 属性，以方便访问原始图像文件名称和类别。</span><span class="sxs-lookup"><span data-stu-id="0fe94-217">The `ImagePath` and `Label` properties are retained for convenience to access the original image file name and category.</span></span>

### <a name="create-workspace-directory"></a><span data-ttu-id="0fe94-218">创建工作区目录</span><span class="sxs-lookup"><span data-stu-id="0fe94-218">Create workspace directory</span></span>

<span data-ttu-id="0fe94-219">当训练和验证数据不经常更改时，最佳做法是缓存计算的瓶颈值以便进行进一步的运行。</span><span class="sxs-lookup"><span data-stu-id="0fe94-219">When training and validation data do not change often, it is good practice to cache the computed bottleneck values for further runs.</span></span>

1. <span data-ttu-id="0fe94-220">在你的项目中，创建名为“工作区”的新目录，以存储计算的瓶颈值和模型的 `.pb` 版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-220">In your project, create a new directory called *workspace* to store the computed bottleneck values and `.pb` version of the model.</span></span>

### <a name="define-paths-and-initialize-variables"></a><span data-ttu-id="0fe94-221">定义路径并初始化变量</span><span class="sxs-lookup"><span data-stu-id="0fe94-221">Define paths and initialize variables</span></span>

1. <span data-ttu-id="0fe94-222">在 `Main` 方法中，定义资产的位置、计算的瓶颈值和模型的 `.pb` 版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-222">Inside the `Main` method, define the location of your assets, computed bottleneck values and `.pb` version of the model.</span></span>

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L15-L17)]

1. <span data-ttu-id="0fe94-223">使用 [MLContext](xref:Microsoft.ML.MLContext) 的新实例初始化 `mlContext` 变量。</span><span class="sxs-lookup"><span data-stu-id="0fe94-223">Initialize the `mlContext` variable with a new instance of [MLContext](xref:Microsoft.ML.MLContext).</span></span>

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L19)]

    <span data-ttu-id="0fe94-224">执行所有 ML.NET 操作都是从 [MLContext](xref:Microsoft.ML.MLContext) 类开始，初始化 mlContext 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。</span><span class="sxs-lookup"><span data-stu-id="0fe94-224">The [MLContext](xref:Microsoft.ML.MLContext) class is a starting point for all ML.NET operations, and initializing mlContext creates a new ML.NET environment that can be shared across the model creation workflow objects.</span></span> <span data-ttu-id="0fe94-225">从概念上讲，它与实体框架中的 `DbContext` 类似。</span><span class="sxs-lookup"><span data-stu-id="0fe94-225">It's similar, conceptually, to `DbContext` in Entity Framework.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="0fe94-226">加载数据</span><span class="sxs-lookup"><span data-stu-id="0fe94-226">Load the data</span></span>

### <a name="create-data-loading-utility-method"></a><span data-ttu-id="0fe94-227">创建数据加载实用工具方法</span><span class="sxs-lookup"><span data-stu-id="0fe94-227">Create data loading utility method</span></span>

<span data-ttu-id="0fe94-228">图像存储在两个子目录中。</span><span class="sxs-lookup"><span data-stu-id="0fe94-228">The images are stored in two subdirectories.</span></span> <span data-ttu-id="0fe94-229">在加载数据之前，需要将数据的格式设置为 `ImageData` 对象的列表。</span><span class="sxs-lookup"><span data-stu-id="0fe94-229">Before loading the data, it needs to be formatted into a list of `ImageData` objects.</span></span> <span data-ttu-id="0fe94-230">为此，在 `Main` 方法下创建 `LoadImagesFromDirectory` 方法。</span><span class="sxs-lookup"><span data-stu-id="0fe94-230">To do so, create the `LoadImagesFromDirectory` method below the `Main` method.</span></span>

```csharp
public static IEnumerable<ImageData> LoadImagesFromDirectory(string folder, bool useFolderNameAsLabel = true)
{

}
```

1. <span data-ttu-id="0fe94-231">在 `LoadImagesFromDirectory` 中添加以下代码，以便从子目录获取所有文件路径：</span><span class="sxs-lookup"><span data-stu-id="0fe94-231">Inside the `LoadImagesFromDirectory`, add the following code to get all of the file paths from the subdirectories:</span></span>

    [!code-csharp [GetFiles](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L104-L105)]

1. <span data-ttu-id="0fe94-232">然后，使用 `foreach` 语句循环访问每个文件。</span><span class="sxs-lookup"><span data-stu-id="0fe94-232">Then, iterate through each of the files using a `foreach` statement.</span></span>

    ```csharp
    foreach (var file in files)
    {

    }
    ```

1. <span data-ttu-id="0fe94-233">在 `foreach` 语句中，检查文件扩展名是否受支持。</span><span class="sxs-lookup"><span data-stu-id="0fe94-233">Inside the `foreach` statement, check that the file extensions are supported.</span></span> <span data-ttu-id="0fe94-234">图像分类 API 支持 JPEG 和 PNG 格式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-234">The Image Classification API supports JPEG and PNG formats.</span></span>

    [!code-csharp [CheckExtension](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L109-L111)]

1. <span data-ttu-id="0fe94-235">然后，获取文件的标签。</span><span class="sxs-lookup"><span data-stu-id="0fe94-235">Then, get the label for the file.</span></span> <span data-ttu-id="0fe94-236">如果 `useFolderNameAsLabel` 参数设置为 `true`，则保存文件的父级目录用作标签。</span><span class="sxs-lookup"><span data-stu-id="0fe94-236">If the `useFolderNameAsLabel` parameter is set to `true`, then the parent directory where the file is saved is used as the label.</span></span> <span data-ttu-id="0fe94-237">否则，标签应为文件名的前缀或文件名本身。</span><span class="sxs-lookup"><span data-stu-id="0fe94-237">Otherwise, it expects the label to be a prefix of the file name or the file name itself.</span></span>

    [!code-csharp [GetLabel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L112-L126)]

1. <span data-ttu-id="0fe94-238">最后，创建 `ModelInput` 的新实例。</span><span class="sxs-lookup"><span data-stu-id="0fe94-238">Finally, create a new instance of `ModelInput`.</span></span>

    [!code-csharp [CreateImageData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L128-L132)]

### <a name="prepare-the-data"></a><span data-ttu-id="0fe94-239">准备数据</span><span class="sxs-lookup"><span data-stu-id="0fe94-239">Prepare the data</span></span>

1. <span data-ttu-id="0fe94-240">返回 `Main` 方法，使用 `LoadImagesFromDirectory` 实用工具方法获取用于训练的图像列表。</span><span class="sxs-lookup"><span data-stu-id="0fe94-240">Back in the `Main` method, use the `LoadImagesFromDirectory` utility method to get the list of images used for training.</span></span>

    [!code-csharp [LoadImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L22)]

1. <span data-ttu-id="0fe94-241">然后，使用 [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable%2A) 方法将图像加载到 [`IDataView`](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-241">Then, load the images into an [`IDataView`](xref:Microsoft.ML.IDataView) using the [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable%2A) method.</span></span>

    [!code-csharp [CreateIDataView](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L24)]

1. <span data-ttu-id="0fe94-242">按从目录中读取数据的顺序加载数据。</span><span class="sxs-lookup"><span data-stu-id="0fe94-242">The data is loaded in the order it was read from the directories.</span></span> <span data-ttu-id="0fe94-243">使用 [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows%2A) 方法无序播放数据，以便平衡这些数据。</span><span class="sxs-lookup"><span data-stu-id="0fe94-243">To balance the data, shuffle it using the [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows%2A) method.</span></span>

    [!code-csharp [ShuffleRows](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L26)]

1. <span data-ttu-id="0fe94-244">机器学习模型要求输入采用数值格式。</span><span class="sxs-lookup"><span data-stu-id="0fe94-244">Machine learning models expect input to be in numerical format.</span></span> <span data-ttu-id="0fe94-245">因此，在训练之前需要对数据进行一些预处理。</span><span class="sxs-lookup"><span data-stu-id="0fe94-245">Therefore, some preprocessing needs to be done on the data prior to training.</span></span> <span data-ttu-id="0fe94-246">创建一个由 [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 和 `LoadRawImageBytes` 转换组成的 [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-246">Create an [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) made up of the [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) and `LoadRawImageBytes` transforms.</span></span> <span data-ttu-id="0fe94-247">`MapValueToKey` 转换采用 `Label` 列中的分类值，将其转换为数值 `KeyType` 值，并将其存储在名为 `LabelAsKey` 的新列中。</span><span class="sxs-lookup"><span data-stu-id="0fe94-247">The `MapValueToKey` transform takes the categorical value in the `Label` column, converts it to a numerical `KeyType` value and stores it in a new column called `LabelAsKey`.</span></span> <span data-ttu-id="0fe94-248">`LoadImages` 采用 `ImagePath` 列中的值和 `imageFolder` 参数，以加载用于训练的图像。</span><span class="sxs-lookup"><span data-stu-id="0fe94-248">The `LoadImages` takes the values from the `ImagePath` column along with the `imageFolder` parameter to load images for training.</span></span>

    [!code-csharp [PreprocessingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L28-L34)]

1. <span data-ttu-id="0fe94-249">使用 [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) 方法将数据应用到 `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601)，然后使用 [`Transform`](xref:Microsoft.ML.Data.TransformerChain%601.Transform%2A) 方法，该方法返回包含预处理数据的 [`IDataView`](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-249">Use the [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) method to apply the data to the `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) followed by the [`Transform`](xref:Microsoft.ML.Data.TransformerChain%601.Transform%2A) method, which returns an [`IDataView`](xref:Microsoft.ML.IDataView) containing the pre-processed data.</span></span>

    [!code-csharp [PreprocessData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L36-L38)]

1. <span data-ttu-id="0fe94-250">要训练模型，具有训练数据集和验证数据集至关重要。</span><span class="sxs-lookup"><span data-stu-id="0fe94-250">To train a model, it's important to have a training dataset as well as a validation dataset.</span></span> <span data-ttu-id="0fe94-251">模型在训练集中进行训练。</span><span class="sxs-lookup"><span data-stu-id="0fe94-251">The model is trained on the training set.</span></span> <span data-ttu-id="0fe94-252">它对不可见数据的预测能力取决于针对验证集的性能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-252">How well it makes predictions on unseen data is measured by the performance against the validation set.</span></span> <span data-ttu-id="0fe94-253">根据该性能的结果，模型会调整其所了解的内容，以进行改进。</span><span class="sxs-lookup"><span data-stu-id="0fe94-253">Based on the results of that performance, the model makes adjustments to what it has learned in an effort to improve.</span></span> <span data-ttu-id="0fe94-254">验证集可以来自拆分原始数据集，也可以来自为此目的而保留的其他源。</span><span class="sxs-lookup"><span data-stu-id="0fe94-254">The validation set can come from either splitting your original dataset or from another source that has already been set aside for this purpose.</span></span> <span data-ttu-id="0fe94-255">在本例中，预先处理的数据集被拆分为训练集、验证集和测试集。</span><span class="sxs-lookup"><span data-stu-id="0fe94-255">In this case, the pre-processed dataset is split into training, validation and test sets.</span></span>

    [!code-csharp [CreateDataSplits](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L40-L41)]

    <span data-ttu-id="0fe94-256">上面的代码示例执行两种拆分。</span><span class="sxs-lookup"><span data-stu-id="0fe94-256">The code sample above performs two splits.</span></span> <span data-ttu-id="0fe94-257">首先，拆分预先处理的数据，并将 70% 用于训练，而将剩余的 30% 用于验证。</span><span class="sxs-lookup"><span data-stu-id="0fe94-257">First, the pre-processed data is split and 70% is used for training while the remaining 30% is used for validation.</span></span> <span data-ttu-id="0fe94-258">然后，将此 30% 的验证集进一步拆分为验证集和测试集，其中，90% 用于验证，10% 用于测试。</span><span class="sxs-lookup"><span data-stu-id="0fe94-258">Then, the 30% validation set is further split into validation and test sets where 90% is used for validation and 10% is used for testing.</span></span>

    <span data-ttu-id="0fe94-259">考虑这些数据分区用途的一种方法是进行测试。</span><span class="sxs-lookup"><span data-stu-id="0fe94-259">A way to think about the purpose of these data partitions is taking an exam.</span></span> <span data-ttu-id="0fe94-260">在为测试而学习时，可以查看笔记、书籍或其他资源来掌握测试中的概念。</span><span class="sxs-lookup"><span data-stu-id="0fe94-260">When studying for an exam, you review your notes, books, or other resources to get a grasp on the concepts that are on the exam.</span></span> <span data-ttu-id="0fe94-261">这便是训练集的作用。</span><span class="sxs-lookup"><span data-stu-id="0fe94-261">This is what the train set is for.</span></span> <span data-ttu-id="0fe94-262">然后，可以进行模拟测试来验证你的知识。</span><span class="sxs-lookup"><span data-stu-id="0fe94-262">Then, you might take a mock exam to validate your knowledge.</span></span> <span data-ttu-id="0fe94-263">这时验证集便派上了用场。</span><span class="sxs-lookup"><span data-stu-id="0fe94-263">This is where the validation set comes in handy.</span></span> <span data-ttu-id="0fe94-264">你需要在参加实际测试之前，检查是否牢固掌握了概念。</span><span class="sxs-lookup"><span data-stu-id="0fe94-264">You want to check whether you have a good grasp of the concepts before taking the actual exam.</span></span> <span data-ttu-id="0fe94-265">根据这些结果，你可以记下做错的内容或无法充分理解的内容，并在复习以应对实际测试时纳入更改。</span><span class="sxs-lookup"><span data-stu-id="0fe94-265">Based on those results, you take note of what you got wrong or didn't understand well and incorporate your changes as you review for the real exam.</span></span> <span data-ttu-id="0fe94-266">最后，进行测试。</span><span class="sxs-lookup"><span data-stu-id="0fe94-266">Finally, you take the exam.</span></span> <span data-ttu-id="0fe94-267">这便是测试集的作用。</span><span class="sxs-lookup"><span data-stu-id="0fe94-267">This is what the test set is used for.</span></span> <span data-ttu-id="0fe94-268">你从未见过测试题目，现在使用你从训练和验证中学到的内容将你的知识应用到手头上的任务。</span><span class="sxs-lookup"><span data-stu-id="0fe94-268">You've never seen the questions that are on the exam and now use what you learned from training and validation to apply your knowledge to the task at hand.</span></span>

1. <span data-ttu-id="0fe94-269">为训练、验证和测试数据的分区分配各自的值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-269">Assign the partitions their respective values for the train, validation and test data.</span></span>

    [!code-csharp [CreateDatasets](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L43-L45)]

## <a name="define-the-training-pipeline"></a><span data-ttu-id="0fe94-270">定义训练管道</span><span class="sxs-lookup"><span data-stu-id="0fe94-270">Define the training pipeline</span></span>

<span data-ttu-id="0fe94-271">模型训练包含以下几个步骤。</span><span class="sxs-lookup"><span data-stu-id="0fe94-271">Model training consists of a couple of steps.</span></span> <span data-ttu-id="0fe94-272">首先，使用图像分类 API 来训练模型。</span><span class="sxs-lookup"><span data-stu-id="0fe94-272">First, Image Classification API is used to train the model.</span></span> <span data-ttu-id="0fe94-273">然后，使用 `MapKeyToValue` 转换将 `PredictedLabel` 列中的编码标签转换回其原始分类值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-273">Then, the encoded labels in the `PredictedLabel` column are converted back to their original categorical value using the `MapKeyToValue` transform.</span></span>

1. <span data-ttu-id="0fe94-274">创建新变量以存储 `ImageClassificationTrainer` 的一组必需参数和可选参数。</span><span class="sxs-lookup"><span data-stu-id="0fe94-274">Create a new variable to store a set of required and optional parameters for an `ImageClassificationTrainer`.</span></span>

    [!code-csharp [ClassifierOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L47-L57)]

    <span data-ttu-id="0fe94-275">`ImageClassificationTrainer` 使用几个可选参数：</span><span class="sxs-lookup"><span data-stu-id="0fe94-275">An `ImageClassificationTrainer` takes several optional parameters:</span></span>

    - <span data-ttu-id="0fe94-276">`FeatureColumnName` 是用作模型的输入的列。</span><span class="sxs-lookup"><span data-stu-id="0fe94-276">`FeatureColumnName` is the column that is used as input for the model.</span></span>
    - <span data-ttu-id="0fe94-277">`LabelColumnName` 是要预测的值的列。</span><span class="sxs-lookup"><span data-stu-id="0fe94-277">`LabelColumnName` is the column for the value to predict.</span></span>
    - <span data-ttu-id="0fe94-278">`ValidationSet` 是包含验证数据的 [`IDataView`](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-278">`ValidationSet` is the [`IDataView`](xref:Microsoft.ML.IDataView) containing the validation data.</span></span>
    - <span data-ttu-id="0fe94-279">`Arch` 定义要使用的预先训练的模型体系结构。</span><span class="sxs-lookup"><span data-stu-id="0fe94-279">`Arch` defines which of the pretrained model architectures to use.</span></span> <span data-ttu-id="0fe94-280">本教程使用 ResNetv2 模型的 101 层变体。</span><span class="sxs-lookup"><span data-stu-id="0fe94-280">This tutorial uses the 101-layer variant of the ResNetv2 model.</span></span>
    - <span data-ttu-id="0fe94-281">`MetricsCallback` 绑定函数，以便在训练期间跟踪进度。</span><span class="sxs-lookup"><span data-stu-id="0fe94-281">`MetricsCallback` binds a function to track the progress during training.</span></span>
    - <span data-ttu-id="0fe94-282">`TestOnTrainSet` 告知模型在不存在验证集时根据训练集度量性能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-282">`TestOnTrainSet` tells the model to measure performance against the training set when no validation set is present.</span></span>
    - <span data-ttu-id="0fe94-283">`ReuseTrainSetBottleneckCachedValues` 告知模型是否在后续运行中使用瓶颈阶段的缓存值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-283">`ReuseTrainSetBottleneckCachedValues` tells the model whether to use the cached values from the bottleneck phase in subsequent runs.</span></span> <span data-ttu-id="0fe94-284">瓶颈阶段是在第一次执行时需要大量计算的一次性直通计算。</span><span class="sxs-lookup"><span data-stu-id="0fe94-284">The bottleneck phase is a one-time pass-through computation that is computationally intensive the first time it is performed.</span></span> <span data-ttu-id="0fe94-285">如果训练数据未发生更改，并且你想要使用不同数量的学习周期或批大小进行试验，则使用缓存的值可以显著减少训练模型所需的时间量。</span><span class="sxs-lookup"><span data-stu-id="0fe94-285">If the training data does not change and you want to experiment using a different number of epochs or batch size, using the cached values significantly reduces the amount of time required to train a model.</span></span>
    - <span data-ttu-id="0fe94-286">`ReuseValidationSetBottleneckCachedValues` 与 `ReuseTrainSetBottleneckCachedValues` 类似，只是在本例中，它适用于验证数据集。</span><span class="sxs-lookup"><span data-stu-id="0fe94-286">`ReuseValidationSetBottleneckCachedValues` is similar to `ReuseTrainSetBottleneckCachedValues` only that in this case it's for the validation dataset.</span></span>
    - <span data-ttu-id="0fe94-287">`WorkspacePath` 定义目录，在该目录中存储计算的瓶颈值和模型的 `.pb` 版本。</span><span class="sxs-lookup"><span data-stu-id="0fe94-287">`WorkspacePath` defines the directory where to store the computed bottleneck values and `.pb` version of the model.</span></span>

1. <span data-ttu-id="0fe94-288">定义包含 `mapLabelEstimator` 和 `ImageClassificationTrainer` 的 [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) 训练管道。</span><span class="sxs-lookup"><span data-stu-id="0fe94-288">Define the [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) training pipeline that consists of both the `mapLabelEstimator` and the `ImageClassificationTrainer`.</span></span>

    [!code-csharp [TrainingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L59-L60)]

1. <span data-ttu-id="0fe94-289">使用 [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) 方法训练模型。</span><span class="sxs-lookup"><span data-stu-id="0fe94-289">Use the [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) method to train your model.</span></span>

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L62)]

## <a name="use-the-model"></a><span data-ttu-id="0fe94-290">使用模型</span><span class="sxs-lookup"><span data-stu-id="0fe94-290">Use the model</span></span>

<span data-ttu-id="0fe94-291">训练模型后，现在可以使用它来对图像进行分类。</span><span class="sxs-lookup"><span data-stu-id="0fe94-291">Now that you have trained your model, it's time to use it to classify images.</span></span>

<span data-ttu-id="0fe94-292">在 `Main` 方法下，创建一个名为 `OutputPrediction` 的新实用工具方法，以便在控制台中显示预测信息。</span><span class="sxs-lookup"><span data-stu-id="0fe94-292">Below the `Main` method, create a new utility method called `OutputPrediction` to display prediction information in the console.</span></span>

[!code-csharp [OuputPredictionMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L96-L100)]

### <a name="classify-a-single-image"></a><span data-ttu-id="0fe94-293">对单个图像进行分类</span><span class="sxs-lookup"><span data-stu-id="0fe94-293">Classify a single image</span></span>

1. <span data-ttu-id="0fe94-294">将名为 `ClassifySingleImage` 的新方法添加到 `Main` 方法下，以进行并输出单个图像预测。</span><span class="sxs-lookup"><span data-stu-id="0fe94-294">Add a new method called `ClassifySingleImage` below the `Main` method to make and output a single image prediction.</span></span>

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. <span data-ttu-id="0fe94-295">在 `ClassifySingleImage` 方法中创建一个 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-295">Create a [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) inside the `ClassifySingleImage` method.</span></span> <span data-ttu-id="0fe94-296">[`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 是一种方便的 API，它允许传入并对单个数据实例执行预测。</span><span class="sxs-lookup"><span data-stu-id="0fe94-296">The [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) is a convenience API, which allows you to pass in and then perform a prediction on a single instance of data.</span></span>

    [!code-csharp [CreatePredictionEngine](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L73)]

1. <span data-ttu-id="0fe94-297">使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法将 `data` [`IDataView`](xref:Microsoft.ML.IDataView) 转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，以访问单个 `ModelInput` 实例，然后获取第一个观察值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-297">To access a single `ModelInput` instance, convert the `data` [`IDataView`](xref:Microsoft.ML.IDataView) into an [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method and then get the first observation.</span></span>

    [!code-csharp [GetTestInputData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L75)]

1. <span data-ttu-id="0fe94-298">使用 [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 方法对图像进行分类。</span><span class="sxs-lookup"><span data-stu-id="0fe94-298">Use the [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) method to classify the image.</span></span>

    [!code-csharp [MakeSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L77)]

1. <span data-ttu-id="0fe94-299">使用 `OutputPrediction` 方法将预测输出到控制台。</span><span class="sxs-lookup"><span data-stu-id="0fe94-299">Output the prediction to the console with the `OutputPrediction` method.</span></span>

    [!code-csharp [OuputSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L79-L80)]

1. <span data-ttu-id="0fe94-300">在 `Main` 方法中，使用图像的测试集调用 `ClassifySingleImage`。</span><span class="sxs-lookup"><span data-stu-id="0fe94-300">Inside the `Main` method, call `ClassifySingleImage` using the test set of images.</span></span>

    [!code-csharp [ClassifySingleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L64)]

### <a name="classify-multiple-images"></a><span data-ttu-id="0fe94-301">对多个图像进行分类</span><span class="sxs-lookup"><span data-stu-id="0fe94-301">Classify multiple images</span></span>

1. <span data-ttu-id="0fe94-302">将名为 `ClassifyImages` 的新方法添加到 `ClassifySingleImage` 方法下，以进行并输出多个图像预测。</span><span class="sxs-lookup"><span data-stu-id="0fe94-302">Add a new method called `ClassifyImages` below the `ClassifySingleImage` method to make and output multiple image predictions.</span></span>

    ```csharp
    public static void ClassifyImages(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. <span data-ttu-id="0fe94-303">使用 [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) 方法创建包含预测的 [`IDataView`](xref:Microsoft.ML.IDataView)。</span><span class="sxs-lookup"><span data-stu-id="0fe94-303">Create an [`IDataView`](xref:Microsoft.ML.IDataView) containing the predictions by using the [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) method.</span></span> <span data-ttu-id="0fe94-304">将以下代码添加到 `ClassifyImages` 方法中。</span><span class="sxs-lookup"><span data-stu-id="0fe94-304">Add the following code inside the `ClassifyImages` method.</span></span>

    [!code-csharp [MakeMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L85)]

1. <span data-ttu-id="0fe94-305">使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法将 `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) 转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，以循环访问预测，然后获取前 10 个观察值。</span><span class="sxs-lookup"><span data-stu-id="0fe94-305">In order to iterate over the predictions, convert the `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) into an [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601) using the [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) method and then get the first 10 observations.</span></span>

    [!code-csharp [IEnumerablePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L87)]

1. <span data-ttu-id="0fe94-306">循环访问并输出预测的原始标签和预测标签。</span><span class="sxs-lookup"><span data-stu-id="0fe94-306">Iterate and output the original and predicted labels for the predictions.</span></span>

    [!code-csharp [OutputMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L89-L93)]

1. <span data-ttu-id="0fe94-307">最后，在 `Main` 方法中，使用图像的测试集调用 `ClassifyImages`。</span><span class="sxs-lookup"><span data-stu-id="0fe94-307">Finally, inside the `Main` method, call `ClassifyImages` using the test set of images.</span></span>

    [!code-csharp [ClassifyImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L66)]

## <a name="run-the-application"></a><span data-ttu-id="0fe94-308">运行此应用程序</span><span class="sxs-lookup"><span data-stu-id="0fe94-308">Run the application</span></span>

<span data-ttu-id="0fe94-309">运行控制台应用。</span><span class="sxs-lookup"><span data-stu-id="0fe94-309">Run your console app.</span></span> <span data-ttu-id="0fe94-310">输出应如下所示。</span><span class="sxs-lookup"><span data-stu-id="0fe94-310">The output should be similar to that below.</span></span> <span data-ttu-id="0fe94-311">你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。</span><span class="sxs-lookup"><span data-stu-id="0fe94-311">You may see warnings or processing messages, but these messages have been removed from the following results for clarity.</span></span> <span data-ttu-id="0fe94-312">为简洁起见，输出已进行压缩。</span><span class="sxs-lookup"><span data-stu-id="0fe94-312">For brevity, the output has been condensed.</span></span>

<span data-ttu-id="0fe94-313">**瓶颈阶段**</span><span class="sxs-lookup"><span data-stu-id="0fe94-313">**Bottleneck phase**</span></span>

<span data-ttu-id="0fe94-314">不会为图像名称打印任何值，因为图像已作为 `byte[]` 加载，因此没有要显示的图像名称。</span><span class="sxs-lookup"><span data-stu-id="0fe94-314">No value is printed for the image name because the images are loaded as a `byte[]` therefore there is no image name to display.</span></span>

```test
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 279
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 280
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   1
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   2
```

<span data-ttu-id="0fe94-315">**训练阶段**</span><span class="sxs-lookup"><span data-stu-id="0fe94-315">**Training phase**</span></span>

```text
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  21, Accuracy:  0.6797619
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  22, Accuracy:  0.7642857
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  23, Accuracy:  0.7916667
```

<span data-ttu-id="0fe94-316">**对图像输出进行分类**</span><span class="sxs-lookup"><span data-stu-id="0fe94-316">**Classify images output**</span></span>

```text
Classifying single image
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD

Classifying multiple images
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-163.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-210.jpg | Actual Value: UD | Predicted Value: UD
```

<span data-ttu-id="0fe94-317">检查 7001-220.jpg 图像时，你可以看到它实际上并无裂缝。</span><span class="sxs-lookup"><span data-stu-id="0fe94-317">Upon inspection of the *7001-220.jpg* image, you can see that it in fact is not cracked.</span></span>

![用于预测的 SDNET2018 数据集图像](./media/image-classification-api-transfer-learning/predictedimage.jpg)

<span data-ttu-id="0fe94-319">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="0fe94-319">Congratulations!</span></span> <span data-ttu-id="0fe94-320">现已成功构建了用于对图像进行分类的深度学习模型。</span><span class="sxs-lookup"><span data-stu-id="0fe94-320">You've now successfully built a deep learning model for classifying images.</span></span>

### <a name="improve-the-model"></a><span data-ttu-id="0fe94-321">改进模型</span><span class="sxs-lookup"><span data-stu-id="0fe94-321">Improve the model</span></span>

<span data-ttu-id="0fe94-322">如果你对模型的结果不满意，则可以尝试使用以下方法来改进其性能：</span><span class="sxs-lookup"><span data-stu-id="0fe94-322">If you're not satisfied with the results of your model, you can try to improve its performance by trying some of the following approaches:</span></span>

- <span data-ttu-id="0fe94-323">**更多数据**：模型学习的示例越多，其性能就越好。</span><span class="sxs-lookup"><span data-stu-id="0fe94-323">**More Data**: The more examples a model learns from, the better it performs.</span></span> <span data-ttu-id="0fe94-324">下载完整的 [SDNET2018 数据集](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional)并将其用于训练。</span><span class="sxs-lookup"><span data-stu-id="0fe94-324">Download the full [SDNET2018 dataset](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional) and use it to train.</span></span>
- <span data-ttu-id="0fe94-325">**增加数据**：向数据添加多样性的一种常见方法是通过拍摄图像并应用不同转换（旋转、翻转、移动、裁剪）来增加数据。</span><span class="sxs-lookup"><span data-stu-id="0fe94-325">**Augment the data**: A common technique to add variety to the data is to augment the data by taking an image and applying different transforms (rotate, flip, shift, crop).</span></span> <span data-ttu-id="0fe94-326">这为模型添加了更多不同的示例以供学习。</span><span class="sxs-lookup"><span data-stu-id="0fe94-326">This adds more varied examples for the model to learn from.</span></span>
- <span data-ttu-id="0fe94-327">**训练更长时间**：训练的时间越长，模型的调整效果就越好。</span><span class="sxs-lookup"><span data-stu-id="0fe94-327">**Train for a longer time**: The longer you train, the more tuned the model will be.</span></span> <span data-ttu-id="0fe94-328">增加学习周期数可以提高模型的性能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-328">Increasing the number of epochs may improve the performance of your model.</span></span>
- <span data-ttu-id="0fe94-329">**试验超参数**：除了在本教程中使用的参数之外，还可以对其他参数进行调整，以便潜在地提高性能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-329">**Experiment with the hyper-parameters**: In addition to the parameters used in this tutorial, other parameters can be tuned to potentially improve performance.</span></span> <span data-ttu-id="0fe94-330">更改学习率（确定在每个学习周期后对模型所做的更新量）可以提高性能。</span><span class="sxs-lookup"><span data-stu-id="0fe94-330">Changing the learning rate, which determines the magnitude of updates made to the model after each epoch may improve performance.</span></span>
- <span data-ttu-id="0fe94-331">**使用其他模型体系结构**：根据数据的外观，可以最好地了解其功能的模型可能会有所不同。</span><span class="sxs-lookup"><span data-stu-id="0fe94-331">**Use a different model architecture**: Depending on what your data looks like, the model that can best learn its features may differ.</span></span> <span data-ttu-id="0fe94-332">如果你对模型的性能不满意，请尝试更改体系结构。</span><span class="sxs-lookup"><span data-stu-id="0fe94-332">If you're not satisfied with the performance of your model, try changing the architecture.</span></span>

## <a name="next-steps"></a><span data-ttu-id="0fe94-333">后续步骤</span><span class="sxs-lookup"><span data-stu-id="0fe94-333">Next steps</span></span>

<span data-ttu-id="0fe94-334">在本教程中，你已了解如何使用迁移学习、预先训练的图像分类 TensorFlow 模型和 ML.NET 图像分类 API 构建自定义深度学习模型，以将混凝土表面的图像分类为有裂缝或无裂缝。</span><span class="sxs-lookup"><span data-stu-id="0fe94-334">In this tutorial, you learned how to build a custom deep learning model using transfer learning, a pretrained image classification TensorFlow model and the ML.NET Image Classification API to classify images of concrete surfaces as cracked or uncracked.</span></span>

<span data-ttu-id="0fe94-335">进入下一教程了解详细信息。</span><span class="sxs-lookup"><span data-stu-id="0fe94-335">Advance to the next tutorial to learn more.</span></span>

> [!div class="nextstepaction"]
> [<span data-ttu-id="0fe94-336">对象检测</span><span class="sxs-lookup"><span data-stu-id="0fe94-336">Object Detection</span></span>](object-detection-onnx.md)
