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
# <a name="tutorial-automated-visual-inspection-using-transfer-learning-with-the-mlnet-image-classification-api"></a>教程：通过 ML.NET 图像分类 API 使用迁移学习自动进行肉眼检查

了解如何使用迁移学习、预先训练的 TensorFlow 模型和 ML.NET 图像分类 API 将混凝土表面的图像分类为有裂缝或无裂缝，以训练自定义深度学习模型。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 了解问题
> - 了解 ML.NET 图像分类 API
> - 了解预先训练的模型
> - 使用迁移学习训练自定义的 TensorFlow 图像分类模型
> - 使用自定义模型对图像进行分类

## <a name="prerequisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。

## <a name="image-classification-transfer-learning-sample-overview"></a>图像分类迁移学习示例概述

此示例是一个 C# .NET Core 控制台应用程序，它使用预先训练的深度学习 TensorFlow 模型对图像进行分类。 可在[示例浏览器](/samples/dotnet/machinelearning-samples/mlnet-image-classification-transfer-learning/)上找到此示例的代码。

## <a name="understand-the-problem"></a>了解问题

图像分类是一种计算机视觉问题。 图像分类使用图像作为输入，并将其分类为规定的类。 图像分类模型通常使用深度学习和神经网络进行训练。 有关详细信息，请参阅[深度学习与机器学习](/azure/machine-learning/concept-deep-learning-vs-machine-learning)。

图像分类很有用的一些情况包括：

- 面部识别
- 情感检测
- 医疗诊断
- 路标检测

本教程训练自定义图像分类模型，以便对桥面自动执行肉眼检查，以识别由裂缝损坏的结构。

## <a name="mlnet-image-classification-api"></a>ML.NET 图像分类 API

ML.NET 提供了各种执行图像分类的方式。 本教程使用图像分类 API 应用迁移学习。 图像分类 API 使用 [TensorFlow.NET](https://github.com/SciSharp/TensorFlow.NET)，即一个为 TensorFlow C++ API 提供 C# 绑定的低级别库。

## <a name="what-is-transfer-learning"></a>什么是迁移学习？

迁移学习应用从解决一个问题到解决其他相关问题所获得的知识。

从头开始训练深度学习模型需要设置多个参数、大量已标记的训练数据和海量计算资源（数百个 GPU 小时）。 结合使用预先训练的模型与迁移学习可让你快速完成训练过程。

## <a name="training-process"></a>训练过程

图像分类 API 通过加载预先训练的 TensorFlow 模型启动训练过程。 该训练过程由两个以下步骤组成：

1. 瓶颈阶段
2. 训练阶段

![训练步骤](./media/image-classification-api-transfer-learning/training.png)

### <a name="bottleneck-phase"></a>瓶颈阶段

在瓶颈阶段，会加载训练图像集，并将像素值用作预先训练模型冻结层的输入或功能。 冻结层包含神经网络的所有层，最高可达倒数第二层，通常也称为瓶颈层。 这些层被称为冻结层，因为这些层中不会出现任何训练并且操作是直通的。 帮助模型区分不同类的低级别模式就是在这些冻结层上进行计算的。 层数越大，此步骤的计算就越密集。 幸运的是，由于这是一种一次性计算，因此在使用不同参数进行试验时，可以缓存结果并将其用于后续运行。

### <a name="training-phase"></a>训练阶段

计算瓶颈阶段的输出值后，这些值将用作输入以重新训练模型的最后一层。 此过程是一种迭代过程，并且会运行模型参数指定的次数。 在每次运行过程中，都将评估损失和准确度。 然后，对模型进行适当调整，以最大程度地降低损失并最大程度地提高准确度。 训练完成后，将输出两种模型格式。 其中之一是模型的 `.pb` 版本，另一种则是模型的 `.zip` ML.NET 序列化版本。 如果在 ML.NET 支持的环境中操作，建议使用模型的 `.zip` 版本。 但如果在不支持 ML.NET 的环境中操作，则可以选择使用 `.pb` 版本。

## <a name="understand-the-pretrained-model"></a>了解预先训练的模型

本教程中使用的预先训练模型是残差网络 (ResNet) v2 模型的 101 层变体。 原始模型经过训练，可以将图像分为一千个类别。 此模型将大小为 224 x 224 的图像作为输入，并输出其训练的每个类的类概率。 此模型的一部分用于使用自定义图像在两个类之间进行预测来训练新模型。

## <a name="create-console-application"></a>创建控制台应用程序

在大致了解了迁移学习和图像分类 API 后，现在可以构建应用程序。

1. 创建名为“DeepLearning_ImageClassification_Binary”的 C# .NET Core 控制台应用程序。
1. 安装“Microsoft.ML”NuGet 包：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    1. 在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”。
    1. 选择“nuget.org”作为“包源”。
    1. 选择“浏览”选项卡。
    1. 选中“包括预发行版”复选框。
    1. 搜索 Microsoft.ML。
    1. 选择“安装”按钮  。
    1. 选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。
    1. 为 Microsoft.ML.Vision、SciSharp.TensorFlow.Redist 和 Microsoft.ML.ImageAnalytics NuGet 包重复上述步骤。  

### <a name="prepare-and-understand-the-data"></a>准备和了解数据

> [!NOTE]
> 本教程的数据集来自由 Maguire, Marc、Dorafshan, Sattar 和 Thomas, Robert J. 共同撰写的“SDNET2018：机器学习应用程序的混凝土裂缝图像数据集”(2018)。 浏览所有数据集。 论文 48。 <https://digitalcommons.usu.edu/all_datasets/48>

SDNET2018 是一个图像数据集，其中包含有裂缝和无裂缝混凝土结构（桥面、墙壁和人行道）的注释。

![SDNET2018 数据集桥面示例](./media/image-classification-api-transfer-learning/sdnet2018decksamples.png)

这些数据组织为三个子目录：

- D 包含桥面图像
- P 包含人行道图像
- W 包含墙壁图像

这些子目录中的每个子目录都包含两个额外的带前缀的子目录：

- C 是用于有裂缝的表面的前缀
- U 是用于无裂缝的表面的前缀

本教程仅使用桥面图像。

1. 下载[数据集](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/assets.zip)并解压缩。
1. 在项目中创建名为“资产”的目录，用于保存数据集文件。
1. 将 CD 与 UD 子目录从最近解压缩的目录复制到“资产”目录  。

### <a name="create-input-and-output-classes"></a>创建输入和输出类

1. 打开 Program.cs 文件，并将文件顶部的现有 `using` 语句替换为以下内容：

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L1-L7)]

1. 在 Program.cs 的 `Program` 类下，创建一个名为 `ImageData` 的类。 此类用于表示最初加载的数据。

    [!code-csharp [ImageDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L137-L142)]

    `ImageData` 包含以下属性：

    - `ImagePath` 是存储图像的完全限定的路径。
    - `Label` 是图像所属的类别。 这是要预测的值。

1. 为输入和输出数据创建类

    1. 在 `ImageData` 类下，在名为 `ModelInput` 的新类中定义输入数据的架构。

        [!code-csharp [ModelInputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L144-L153)]

        `ModelInput` 包含以下属性：

        - `Image` 是图像的 `byte[]` 表示形式。 模型需要此类型的图像数据以供训练。
        - `LabelAsKey` 是 `Label` 的数值表示形式。
        - `ImagePath` 是存储图像的完全限定的路径。
        - `Label` 是图像所属的类别。 这是要预测的值。

        仅 `Image` 和 `LabelAsKey` 用于训练模型和进行预测。 已保留 `ImagePath` 和 `Label` 属性，以方便访问原始图像文件名称和类别。

    1. 然后，在 `ModelInput` 类下，在名为 `ModelOutput` 的新类中定义输出数据的架构。

        [!code-csharp [ModelOutputClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L155-L162)]

        `ModelOutput` 包含以下属性：

        - `ImagePath` 是存储图像的完全限定的路径。
        - `Label` 是图像所属的原始类别。 这是要预测的值。
        - `PredictedLabel` 是模型预测的值。

        与 `ModelInput` 类似，只需 `PredictedLabel` 即可进行预测，因为它包含由模型作出的预测。 已保留 `ImagePath` 和 `Label` 属性，以方便访问原始图像文件名称和类别。

### <a name="create-workspace-directory"></a>创建工作区目录

当训练和验证数据不经常更改时，最佳做法是缓存计算的瓶颈值以便进行进一步的运行。

1. 在你的项目中，创建名为“工作区”的新目录，以存储计算的瓶颈值和模型的 `.pb` 版本。

### <a name="define-paths-and-initialize-variables"></a>定义路径并初始化变量

1. 在 `Main` 方法中，定义资产的位置、计算的瓶颈值和模型的 `.pb` 版本。

    [!code-csharp [DefinePaths](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L15-L17)]

1. 使用 [MLContext](xref:Microsoft.ML.MLContext) 的新实例初始化 `mlContext` 变量。

    [!code-csharp [MLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L19)]

    执行所有 ML.NET 操作都是从 [MLContext](xref:Microsoft.ML.MLContext) 类开始，初始化 mlContext 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DbContext` 类似。

## <a name="load-the-data"></a>加载数据

### <a name="create-data-loading-utility-method"></a>创建数据加载实用工具方法

图像存储在两个子目录中。 在加载数据之前，需要将数据的格式设置为 `ImageData` 对象的列表。 为此，在 `Main` 方法下创建 `LoadImagesFromDirectory` 方法。

```csharp
public static IEnumerable<ImageData> LoadImagesFromDirectory(string folder, bool useFolderNameAsLabel = true)
{

}
```

1. 在 `LoadImagesFromDirectory` 中添加以下代码，以便从子目录获取所有文件路径：

    [!code-csharp [GetFiles](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L104-L105)]

1. 然后，使用 `foreach` 语句循环访问每个文件。

    ```csharp
    foreach (var file in files)
    {

    }
    ```

1. 在 `foreach` 语句中，检查文件扩展名是否受支持。 图像分类 API 支持 JPEG 和 PNG 格式。

    [!code-csharp [CheckExtension](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L109-L111)]

1. 然后，获取文件的标签。 如果 `useFolderNameAsLabel` 参数设置为 `true`，则保存文件的父级目录用作标签。 否则，标签应为文件名的前缀或文件名本身。

    [!code-csharp [GetLabel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L112-L126)]

1. 最后，创建 `ModelInput` 的新实例。

    [!code-csharp [CreateImageData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L128-L132)]

### <a name="prepare-the-data"></a>准备数据

1. 返回 `Main` 方法，使用 `LoadImagesFromDirectory` 实用工具方法获取用于训练的图像列表。

    [!code-csharp [LoadImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L22)]

1. 然后，使用 [`LoadFromEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.LoadFromEnumerable%2A) 方法将图像加载到 [`IDataView`](xref:Microsoft.ML.IDataView)。

    [!code-csharp [CreateIDataView](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L24)]

1. 按从目录中读取数据的顺序加载数据。 使用 [`ShuffleRows`](xref:Microsoft.ML.DataOperationsCatalog.ShuffleRows%2A) 方法无序播放数据，以便平衡这些数据。

    [!code-csharp [ShuffleRows](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L26)]

1. 机器学习模型要求输入采用数值格式。 因此，在训练之前需要对数据进行一些预处理。 创建一个由 [`MapValueToKey`](xref:Microsoft.ML.ConversionsExtensionsCatalog.MapValueToKey%2A) 和 `LoadRawImageBytes` 转换组成的 [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601)。 `MapValueToKey` 转换采用 `Label` 列中的分类值，将其转换为数值 `KeyType` 值，并将其存储在名为 `LabelAsKey` 的新列中。 `LoadImages` 采用 `ImagePath` 列中的值和 `imageFolder` 参数，以加载用于训练的图像。

    [!code-csharp [PreprocessingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L28-L34)]

1. 使用 [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) 方法将数据应用到 `preprocessingPipeline` [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601)，然后使用 [`Transform`](xref:Microsoft.ML.Data.TransformerChain%601.Transform%2A) 方法，该方法返回包含预处理数据的 [`IDataView`](xref:Microsoft.ML.IDataView)。

    [!code-csharp [PreprocessData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L36-L38)]

1. 要训练模型，具有训练数据集和验证数据集至关重要。 模型在训练集中进行训练。 它对不可见数据的预测能力取决于针对验证集的性能。 根据该性能的结果，模型会调整其所了解的内容，以进行改进。 验证集可以来自拆分原始数据集，也可以来自为此目的而保留的其他源。 在本例中，预先处理的数据集被拆分为训练集、验证集和测试集。

    [!code-csharp [CreateDataSplits](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L40-L41)]

    上面的代码示例执行两种拆分。 首先，拆分预先处理的数据，并将 70% 用于训练，而将剩余的 30% 用于验证。 然后，将此 30% 的验证集进一步拆分为验证集和测试集，其中，90% 用于验证，10% 用于测试。

    考虑这些数据分区用途的一种方法是进行测试。 在为测试而学习时，可以查看笔记、书籍或其他资源来掌握测试中的概念。 这便是训练集的作用。 然后，可以进行模拟测试来验证你的知识。 这时验证集便派上了用场。 你需要在参加实际测试之前，检查是否牢固掌握了概念。 根据这些结果，你可以记下做错的内容或无法充分理解的内容，并在复习以应对实际测试时纳入更改。 最后，进行测试。 这便是测试集的作用。 你从未见过测试题目，现在使用你从训练和验证中学到的内容将你的知识应用到手头上的任务。

1. 为训练、验证和测试数据的分区分配各自的值。

    [!code-csharp [CreateDatasets](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L43-L45)]

## <a name="define-the-training-pipeline"></a>定义训练管道

模型训练包含以下几个步骤。 首先，使用图像分类 API 来训练模型。 然后，使用 `MapKeyToValue` 转换将 `PredictedLabel` 列中的编码标签转换回其原始分类值。

1. 创建新变量以存储 `ImageClassificationTrainer` 的一组必需参数和可选参数。

    [!code-csharp [ClassifierOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L47-L57)]

    `ImageClassificationTrainer` 使用几个可选参数：

    - `FeatureColumnName` 是用作模型的输入的列。
    - `LabelColumnName` 是要预测的值的列。
    - `ValidationSet` 是包含验证数据的 [`IDataView`](xref:Microsoft.ML.IDataView)。
    - `Arch` 定义要使用的预先训练的模型体系结构。 本教程使用 ResNetv2 模型的 101 层变体。
    - `MetricsCallback` 绑定函数，以便在训练期间跟踪进度。
    - `TestOnTrainSet` 告知模型在不存在验证集时根据训练集度量性能。
    - `ReuseTrainSetBottleneckCachedValues` 告知模型是否在后续运行中使用瓶颈阶段的缓存值。 瓶颈阶段是在第一次执行时需要大量计算的一次性直通计算。 如果训练数据未发生更改，并且你想要使用不同数量的学习周期或批大小进行试验，则使用缓存的值可以显著减少训练模型所需的时间量。
    - `ReuseValidationSetBottleneckCachedValues` 与 `ReuseTrainSetBottleneckCachedValues` 类似，只是在本例中，它适用于验证数据集。
    - `WorkspacePath` 定义目录，在该目录中存储计算的瓶颈值和模型的 `.pb` 版本。

1. 定义包含 `mapLabelEstimator` 和 `ImageClassificationTrainer` 的 [`EstimatorChain`](xref:Microsoft.ML.Data.EstimatorChain%601) 训练管道。

    [!code-csharp [TrainingPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L59-L60)]

1. 使用 [`Fit`](xref:Microsoft.ML.Data.EstimatorChain%601.Fit%2A) 方法训练模型。

    [!code-csharp [TrainModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L62)]

## <a name="use-the-model"></a>使用模型

训练模型后，现在可以使用它来对图像进行分类。

在 `Main` 方法下，创建一个名为 `OutputPrediction` 的新实用工具方法，以便在控制台中显示预测信息。

[!code-csharp [OuputPredictionMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L96-L100)]

### <a name="classify-a-single-image"></a>对单个图像进行分类

1. 将名为 `ClassifySingleImage` 的新方法添加到 `Main` 方法下，以进行并输出单个图像预测。

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. 在 `ClassifySingleImage` 方法中创建一个 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 是一种方便的 API，它允许传入并对单个数据实例执行预测。

    [!code-csharp [CreatePredictionEngine](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L73)]

1. 使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法将 `data` [`IDataView`](xref:Microsoft.ML.IDataView) 转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，以访问单个 `ModelInput` 实例，然后获取第一个观察值。

    [!code-csharp [GetTestInputData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L75)]

1. 使用 [`Predict`](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 方法对图像进行分类。

    [!code-csharp [MakeSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L77)]

1. 使用 `OutputPrediction` 方法将预测输出到控制台。

    [!code-csharp [OuputSinglePrediction](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L79-L80)]

1. 在 `Main` 方法中，使用图像的测试集调用 `ClassifySingleImage`。

    [!code-csharp [ClassifySingleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L64)]

### <a name="classify-multiple-images"></a>对多个图像进行分类

1. 将名为 `ClassifyImages` 的新方法添加到 `ClassifySingleImage` 方法下，以进行并输出多个图像预测。

    ```csharp
    public static void ClassifyImages(MLContext mlContext, IDataView data, ITransformer trainedModel)
    {

    }
    ```

1. 使用 [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) 方法创建包含预测的 [`IDataView`](xref:Microsoft.ML.IDataView)。 将以下代码添加到 `ClassifyImages` 方法中。

    [!code-csharp [MakeMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L85)]

1. 使用 [`CreateEnumerable`](xref:Microsoft.ML.DataOperationsCatalog.CreateEnumerable%2A) 方法将 `predictionData` [`IDataView`](xref:Microsoft.ML.IDataView) 转换为 [`IEnumerable`](xref:System.Collections.Generic.IEnumerable%601)，以循环访问预测，然后获取前 10 个观察值。

    [!code-csharp [IEnumerablePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L87)]

1. 循环访问并输出预测的原始标签和预测标签。

    [!code-csharp [OutputMultiplePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L89-L93)]

1. 最后，在 `Main` 方法中，使用图像的测试集调用 `ClassifyImages`。

    [!code-csharp [ClassifyImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ImageClassification_Binary/DeepLearning_ImageClassification/Program.cs#L66)]

## <a name="run-the-application"></a>运行此应用程序

运行控制台应用。 输出应如下所示。 你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。 为简洁起见，输出已进行压缩。

**瓶颈阶段**

不会为图像名称打印任何值，因为图像已作为 `byte[]` 加载，因此没有要显示的图像名称。

```test
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 279
Phase: Bottleneck Computation, Dataset used:      Train, Image Index: 280
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   1
Phase: Bottleneck Computation, Dataset used: Validation, Image Index:   2
```

**训练阶段**

```text
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  21, Accuracy:  0.6797619
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  22, Accuracy:  0.7642857
Phase: Training, Dataset used: Validation, Batch Processed Count:   6, Epoch:  23, Accuracy:  0.7916667
```

**对图像输出进行分类**

```text
Classifying single image
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD

Classifying multiple images
Image: 7001-220.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-163.jpg | Actual Value: UD | Predicted Value: UD
Image: 7001-210.jpg | Actual Value: UD | Predicted Value: UD
```

检查 7001-220.jpg 图像时，你可以看到它实际上并无裂缝。

![用于预测的 SDNET2018 数据集图像](./media/image-classification-api-transfer-learning/predictedimage.jpg)

祝贺你！ 现已成功构建了用于对图像进行分类的深度学习模型。

### <a name="improve-the-model"></a>改进模型

如果你对模型的结果不满意，则可以尝试使用以下方法来改进其性能：

- **更多数据**：模型学习的示例越多，其性能就越好。 下载完整的 [SDNET2018 数据集](https://digitalcommons.usu.edu/cgi/viewcontent.cgi?filename=2&article=1047&context=all_datasets&type=additional)并将其用于训练。
- **增加数据**：向数据添加多样性的一种常见方法是通过拍摄图像并应用不同转换（旋转、翻转、移动、裁剪）来增加数据。 这为模型添加了更多不同的示例以供学习。
- **训练更长时间**：训练的时间越长，模型的调整效果就越好。 增加学习周期数可以提高模型的性能。
- **试验超参数**：除了在本教程中使用的参数之外，还可以对其他参数进行调整，以便潜在地提高性能。 更改学习率（确定在每个学习周期后对模型所做的更新量）可以提高性能。
- **使用其他模型体系结构**：根据数据的外观，可以最好地了解其功能的模型可能会有所不同。 如果你对模型的性能不满意，请尝试更改体系结构。

## <a name="next-steps"></a>后续步骤

在本教程中，你已了解如何使用迁移学习、预先训练的图像分类 TensorFlow 模型和 ML.NET 图像分类 API 构建自定义深度学习模型，以将混凝土表面的图像分类为有裂缝或无裂缝。

进入下一教程了解详细信息。

> [!div class="nextstepaction"]
> [对象检测](object-detection-onnx.md)
