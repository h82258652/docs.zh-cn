---
title: 教程：用于对图像进行分类的 ML.NET 分类模型
description: 了解如何通过用于处理图像的预先训练的 TensorFlow 模型来训练分类模型，以便对图像进行分类。
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mvc, title-hack-0612
ms.openlocfilehash: f510b6a21807a2ab1e2d7c878c285038bea33d42
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514471"
---
# <a name="tutorial-train-an-mlnet-classification-model-to-categorize-images"></a>教程：训练 ML.NET 分类模型以对图像进行分类

了解如何通过用于处理图像的预先训练的 TensorFlow 模型来训练分类模型，以便对图像进行分类。

TensorFlow 模型经过训练，可以将图像分为一千个类别。 由于 TensorFlow 模型知道如何识别图像中的模式，因此 ML.NET 模型可以在其管道中使用前者的一部分，以将原始图像转换为特征或输入来训练分类模型。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 了解问题
> * 将经过预先训练的 TensorFlow 模型合并到 ML.NET 管道中
> * 训练和评估 ML.NET 模型
> * 对测试图像进行分类

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) 存储库中找到本教程的源代码。 请注意在本教程中，.NET 项目配置默认面向 .NET core 2.2。

## <a name="prerequisites"></a>先决条件

* 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。
* [教程资产目录 .ZIP 文件](https://github.com/dotnet/samples/blob/main/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)
* [InceptionV1 机器学习模型](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)

## <a name="select-the-right-machine-learning-task"></a>选择正确的机器学习任务

### <a name="deep-learning"></a>深度学习

[深度学习](https://en.wikipedia.org/wiki/Deep_learning)是机器学习的一部分，颠覆了计算机视觉和语音识别等领域。

深度学习模型是使用包含多个学习层的大量[已标记的数据](https://en.wikipedia.org/wiki/Labeled_data)和[神经网络](https://en.wikipedia.org/wiki/Artificial_neural_network)来定型。 深度学习：

* 执行计算机视觉等一些任务的效果更好。
* 需要大量训练数据。

图像分类是一项特定分类任务，可用于将图像自动分类为多个类别，例如：

* 检测图像中是否有人脸。
* 检测是猫还是狗。

 或者在以下图像中，确定图像是食物、玩具还是家用电器：

![披萨图像](./media/image-classification/220px-Pepperoni_pizza.jpg)
![泰迪熊图像](./media/image-classification/119px-Nalle_-_a_small_brown_teddy_bear.jpg)
![烤面包机图像](./media/image-classification/193px-Broodrooster.jpg)

>[!Note]
> 上面的图像属于维基共享资源，并按如下方式进行属性化：
>
> * “220px-Pepperoni_pizza.jpg”公有领域，[https://commons.wikimedia.org/w/index.php?curid=79505](<https://commons.wikimedia.org/w/index.php?curid=79505>)
> * “119px-Nalle_-_a_small_brown_teddy_bear.jpg”作者为 [Jonik](https://commons.wikimedia.org/wiki/User:Jonik) - 独自拍摄，CC BY-SA 2.0，[https://commons.wikimedia.org/w/index.php?curid=48166](<https://commons.wikimedia.org/w/index.php?curid=48166>)。
> * “193px-Broodrooster.jpg”作者为 [M.Minderhoud](https://nl.wikipedia.org/wiki/Gebruiker:Michiel1972) - 自有作品，CC BY-SA 3.0，[https://commons.wikimedia.org/w/index.php?curid=27403](<https://commons.wikimedia.org/w/index.php?curid=27403>)

从头开始训练[图像分类](https://en.wikipedia.org/wiki/Outline_of_object_recognition)模型需要设置数百万个参数、大量已标记的训练数据和海量计算资源（数百个 GPU 小时）。 虽然不如从头开始定型自定义模型有效，但借助预先训练的模型，可以通过处理数千张图像（与数百万张已标记的图像相比）来简化此过程，并非常快速地生成自定义模型（在没有 GPU 的计算机上一小时内完成）。 本教程进一步缩小了该过程，只使用十二个训练图像。

`Inception model` 经过训练，可将图像分为一千种类别，但对于本教程，你需要将图像分类为较小的类别集，并且仅对这些类别进行分类。可以使用 `Inception model` 的功能将图像识别和分类到自定义图像分类器的新限制类别。

* 食物
* 玩具
* 家用电器

本教程使用 TensorFlow [Inception](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip) 深入学习模型，该模型是在 `ImageNet` 数据集上经过训练的常用图像识别模型。 TensorFlow 模型将整个图像分为一千个类别，例如“伞”、“运动衫”和“洗碗机”。

由于 `Inception model` 已预先在数千个不同图像上进行过训练，因此内部包含图像识别所需的[图像特征](https://en.wikipedia.org/wiki/Feature_(computer_vision))。 可以在模型中使用这些内部图像特征，用更少的类训练一个新模型。

如下图中所示，在 .NET Core 或 .NET Framework 应用中添加对 ML.NET NuGet 包的引用。 实际上，ML.NET 包括并引用本机 `TensorFlow` 库，可用于编写代码来加载已训练的现有 `TensorFlow` 模型文件。

![TensorFlow 转换 ML.NET 体系结构图](./media/image-classification/tensorflow-mlnet.png)

### <a name="multiclass-classification"></a>多类分类

使用 TensorFlow Inception 模型提取适合作为经典机器学习算法输入的特征后，我们添加了一个 ML.NET [多类分类器](../resources/tasks.md#multiclass-classification)。

此例中使用的特定训练者是[多项式逻辑回归算法](https://en.wikipedia.org/wiki/Multinomial_logistic_regression)。

此训练者实现的算法可以很好地处理大量特征存在的问题，这是深度学习模型对图像数据进行操作时遇到的情况。

有关详细信息，请参阅[深度学习与机器学习](/azure/machine-learning/concept-deep-learning-vs-machine-learning)。

### <a name="data"></a>数据

数据源有两个：`.tsv` 文件和图像文件。  `tags.tsv` 文件包含两个列：第一列定义为 `ImagePath`，第二列是与图像对应的 `Label`。 下面的示例文件没有标题行，如下所示：

<!-- markdownlint-disable MD010 -->
```tsv
broccoli.jpg    food
pizza.jpg   food
pizza2.jpg  food
teddy2.jpg  toy
teddy3.jpg  toy
teddy4.jpg  toy
toaster.jpg appliance
toaster2.png    appliance
```
<!-- markdownlint-enable MD010 -->

定型图像和测试图像位于下载 zip 文件的资产文件夹中。 这些图像属于维基共享资源。
> [维基共享资源](https://commons.wikimedia.org/w/index.php?title=Main_Page&oldid=313158208)，免费媒体存储库。 检索时间：2018 年 10 月 17 日 10:48 检索位置：<https://commons.wikimedia.org/wiki/Pizza>
> <https://commons.wikimedia.org/wiki/Toaster>
> <https://commons.wikimedia.org/wiki/Teddy_bear>

## <a name="setup"></a>安装

### <a name="create-a-project"></a>创建项目

1. 创建名为“TransferLearningTF”的 **.NET Core 控制台应用程序**。

1. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    * 在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。
    * 选择“nuget.org”作为“包源”，选择“浏览”选项卡，再搜索“Microsoft.ML”。
    * 选择“安装”按钮。
    * 选择“预览更改”对话框中的“确定”按钮 。
    * 如果同意所列包的许可条款，请选择“接受许可”对话框中的“我接受”按钮。
    * 对 Microsoft.ML.ImageAnalytics、SciSharp.TensorFlow.Redist 和 Microsoft.ML.TensorFlow 重复上述步骤  。

### <a name="download-assets"></a>下载资产

1. 下载并解压缩[项目资产目录 zip 文件](https://github.com/dotnet/samples/blob/main/machine-learning/tutorials/TransferLearningTF/image-classifier-assets.zip)。

1. 将“`assets`”目录复制到“TransferLearningTF”项目目录中。 此目录及其子目录包含本教程所需的数据和支持文件（Inception 模型除外，将在下一步中下载并添加此模型）。

1. 下载并解压缩 [Inception 模型](https://storage.googleapis.com/download.tensorflow.org/models/inception5h.zip)。

1. 将刚刚解压缩的“`inception5h`”目录的内容复制到 TransferLearningTF 项目的“`assets/inception`”目录中。 此目录包含本教程所需的模型和其他支持文件，如下图所示：

   ![Inception 目录内容](./media/image-classification/inception-files.png)

1. 在“解决方案资源管理器”中，右键单击资产目录和子目录中的每个文件，再选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

### <a name="create-classes-and-define-paths"></a>创建类和定义路径

1. 将以下附加的 `using` 语句添加到“Program.cs”文件顶部：

    [!code-csharp[AddUsings](./snippets/image-classification/csharp/Program.cs#AddUsings)]

1. 将以下代码添加到 `Main` 方法正上方的行中，以指定资产路径：

    [!code-csharp[DeclareGlobalVariables](./snippets/image-classification/csharp/Program.cs#DeclareGlobalVariables)]

1. 为输入数据和预测结果创建类。

    [!code-csharp[DeclareImageData](./snippets/image-classification/csharp/Program.cs#DeclareImageData)]

    `ImageData` 是输入图像数据类，包含以下 <xref:System.String> 字段：

    * `ImagePath` 包含图像文件名。
    * `Label` 包含图像标签值。

1. 向项目添加 `ImagePrediction` 的新类：

    [!code-csharp[DeclareImagePrediction](./snippets/image-classification/csharp/Program.cs#DeclareImagePrediction)]

    `ImagePrediction` 是图像预测类，包含以下字段：

    * `Score` 包含给定图像分类的置信度百分比。
    * `PredictedLabelValue` 包含预测出的图像分类标签的值。

    `ImagePrediction` 是在定型模型后用于预测的类。 它包含图像路径的 `string` (`ImagePath`)。 `Label` 用于重用和训练模型。 `PredictedLabelValue` 在预测和评估过程中使用。 对于计算，将使用带定型数据的输入、预测值和模型。

### <a name="initialize-variables-in-main"></a>在 Main 中初始化变量

1. 使用 `MLContext` 的新实例初始化 `mlContext` 变量。  用下面 `Main` 方法中的代码替换 `Console.WriteLine("Hello World!")` 行：

    [!code-csharp[CreateMLContext](./snippets/image-classification/csharp/Program.cs#CreateMLContext)]

    执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

### <a name="create-a-struct-for-inception-model-parameters"></a>为 Inception 模型参数创建结构

1. Inception 模型具有多个需要传入的参数。 紧跟在 `Main()` 方法后面，使用以下代码创建结构，以将参数值映射到易记名称：

    [!code-csharp[InceptionSettings](./snippets/image-classification/csharp/Program.cs#InceptionSettings)]

### <a name="create-a-display-utility-method"></a>创建显示实用工具方法

由于将多次显示图像数据和相关预测，请创建显示实用工具方法，用于处理图像和预测结果的显示。

1. 紧跟在 `InceptionSettings` 结构后面，使用下面的代码创建 `DisplayResults()` 方法：

    ```csharp
    private static void DisplayResults(IEnumerable<ImagePrediction> imagePredictionData)
    {

    }
    ```

1. 填充 `DisplayResults` 方法的主体：

    [!code-csharp[DisplayPredictions](./snippets/image-classification/csharp/Program.cs#DisplayPredictions)]

### <a name="create-a-tsv-file-utility-method"></a>创建 .tsv 文件实用工具方法

1. 使用下面的代码紧随 `DisplayResults()` 方法后创建 `ReadFromTsv()` 方法：

    ```csharp
    public static IEnumerable<ImageData> ReadFromTsv(string file, string folder)
    {

    }
    ```

1. 填充 `ReadFromTsv` 方法的主体：

    [!code-csharp[ReadFromTsv](./snippets/image-classification/csharp/Program.cs#ReadFromTsv)]

    代码分析整个 `tags.tsv` 文件，以将文件路径添加到 `ImagePath` 属性的图像文件名中，并将它和 `Label` 加载到 `ImageData` 对象中。

### <a name="create-a-method-to-make-a-prediction"></a>创建用于进行预测的方法

1. 使用下面的代码在 `DisplayResults()` 方法前创建 `ClassifySingleImage()` 方法：

    ```csharp
    public static void ClassifySingleImage(MLContext mlContext, ITransformer model)
    {

    }
    ```

1. 创建 `ImageData` 对象，其中包含单个 `ImagePath` 的完全限定路径和图像文件名。 将以下代码添加为 `ClassifySingleImage()` 方法的接下来几行：

    [!code-csharp[LoadImageData](./snippets/image-classification/csharp/Program.cs#LoadImageData)]

1. 通过添加以下代码作为 `ClassifySingleImage` 方法中的下一行，进行单一预测：

    [!code-csharp[PredictSingle](./snippets/image-classification/csharp/Program.cs#PredictSingle)]

    若要获得预测，请使用 [Predict()](xref:Microsoft.ML.PredictionEngine%602.Predict%2A) 方法。 [PredictionEngine](xref:Microsoft.ML.PredictionEngine%602) 是一个简便 API，可使用它对单个数据实例执行预测。 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 不是线程安全型。 可以在单线程环境或原型环境中使用。 为了在生产环境中提高性能和线程安全，请使用 `PredictionEnginePool` 服务，这将创建一个在整个应用程序中使用的 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 对象的 [`ObjectPool`](xref:Microsoft.Extensions.ObjectPool.ObjectPool%601)。 请参阅本指南，了解如何[在 ASP.NET Core Web API 中使用 `PredictionEnginePool`](../how-to-guides/serve-model-web-api-ml-net.md#register-predictionenginepool-for-use-in-the-application)。

    > [!NOTE]
    > `PredictionEnginePool` 服务扩展目前处于预览状态。

1. `ClassifySingleImage()` 方法的下一行代码用于显示预测结果：

   [!code-csharp[DisplayPrediction](./snippets/image-classification/csharp/Program.cs#DisplayPrediction)]

## <a name="construct-the-mlnet-model-pipeline"></a>构造 ML.NET 模型管道

ML.NET 模型管道是一个估算器链。 请注意，管道构造过程中不会发生任何执行。 估算器对象已创建，但不会执行。

1. 添加生成模型的方法

    此方法是教程的核心。 它为模型创建管道，并训练管道以生成 ML.NET 模型。 它还针对某些以前看不到的测试数据评估模型。

    紧跟在 `InceptionSettings` 结构后面且恰好在 `DisplayResults()` 方法前面，使用以下代码创建 `GenerateModel()` 方法：

    ```csharp
    public static ITransformer GenerateModel(MLContext mlContext)
    {

    }
    ```

1. 添加估算器以从图像数据加载、调整和提取像素：

    [!code-csharp[ImageTransforms](./snippets/image-classification/csharp/Program.cs#ImageTransforms)]

    图像数据需要处理成 TensorFlow 模型所需的格式。 在本例中，图像将加载到内存中，将其调整为一致大小，并将像素提取成一个数值矢量。

1. 添加估算器以加载 TensorFlow 模型并进行评分：

    [!code-csharp[ScoreTensorFlowModel](./snippets/image-classification/csharp/Program.cs#ScoreTensorFlowModel)]

    管道中的此阶段将 TensorFlow 模型加载到内存中，然后通过 TensorFlow 模型网络处理像素值的矢量。 将输入应用于深度学习模型并使用该模型生成输出的过程称为 **评分**。 当作为一个整体使用模型时，评分将做出推理或预测。

    在本例中，将使用除最后一层（这是进行推理的层）之外的全部 TensorFlow 模型。 倒数第二层的输出标有 `softmax_2_preactivation`。 此层的输出实际上是特征矢量，用于描述原始输入图像的特征。

    TensorFlow 模型生成的此特征矢量将用作 ML.NET 训练算法的输入。

1. 添加估算器以将训练数据中的字符串标签映射到整数键值：

    [!code-csharp[MapValueToKey](./snippets/image-classification/csharp/Program.cs#MapValueToKey)]

    接下来追加的 ML.NET 训练者要求其标签采用 `key` 格式，而不是任意字符串。 键是一个数字，一对一映射到字符串值。

1. 添加 ML.NET 训练算法：

    [!code-csharp[AddTrainer](./snippets/image-classification/csharp/Program.cs#AddTrainer)]

1. 添加估算器以将预测的键值映射回字符串：

    [!code-csharp[MapKeyToValue](./snippets/image-classification/csharp/Program.cs#MapKeyToValue)]

## <a name="train-the-model"></a>定型模型

1. 使用 [LoadFromTextFile](xref:Microsoft.ML.TextLoaderSaverCatalog.LoadFromTextFile(Microsoft.ML.DataOperationsCatalog,System.String,Microsoft.ML.Data.TextLoader.Options)) 包装器加载训练数据。 将以下代码作为下一行添加到 `GenerateModel()` 方法中：

    [!code-csharp[LoadData](./snippets/image-classification/csharp/Program.cs#LoadData "Load the data")]

    ML.NET 中的数据表示为 [IDataView 类](xref:Microsoft.ML.IDataView)。 `IDataView` 是用于描述表格数据（数字和文本）的一种灵活且有效的方法。 可从文本文件或实时（例如，SQL 数据库或日志文件）将数据加载到 `IDataView` 对象。

1. 用上面加载的数据训练模型：

    [!code-csharp[TrainModel](./snippets/image-classification/csharp/Program.cs#TrainModel)]

    `Fit()` 方法通过将训练数据集应用于管道来训练模型。

## <a name="evaluate-the-accuracy-of-the-model"></a>评估模型的准确性

1. 通过将以下代码添加到 `GenerateModel` 方法的下一行，加载并转换测试数据：

    [!code-csharp[LoadAndTransformTestData](./snippets/image-classification/csharp/Program.cs#LoadAndTransformTestData "Load and transform test data")]

    可以使用几个示例图像来评估模型。 与训练数据类似，需要将这些数据加载到 `IDataView` 中，以便模型可以对其进行转换。

1. 若要评估模型，请将以下代码添加到 `GenerateModel()` 方法：

    [!code-csharp[Evaluate](./snippets/image-classification/csharp/Program.cs#Evaluate)]

    在你设置预测后，[Evaluate()](xref:Microsoft.ML.RecommendationCatalog.Evaluate%2A) 方法便能：

    * 评估模型（将预测的值与测试数据集 `labels` 进行比较）。
    * 返回模型性能指标。

1. 显示模型准确性指标

    使用下面的代码显示指标、共享结果，然后处理它们：

    [!code-csharp[DisplayMetrics](./snippets/image-classification/csharp/Program.cs#DisplayMetrics)]

    下面是图像分类评估指标：

    * `Log-loss` - 请参阅[对数损失](../resources/glossary.md#log-loss)。 通常会希望对数损失尽可能接近 0。
    * `Per class Log-loss`. 建议每类别的对数损失尽可能接近 0。

1. 添加以下代码，将经过训练的模型作为下一行代码返回：

    [!code-csharp[SaveModel](./snippets/image-classification/csharp/Program.cs#ReturnModel)]

## <a name="run-the-application"></a>运行应用程序！

1. 创建 MLContext 类后，添加对 `Main` 方法中 `GenerateModel` 的调用：

    [!code-csharp[CallGenerateModel](./snippets/image-classification/csharp/Program.cs#CallGenerateModel)]

1. 将对 `ClassifySingleImage()` 方法的调用添加为 `Main` 方法的下一行代码：

    [!code-csharp[CallClassifySingleImage](./snippets/image-classification/csharp/Program.cs#CallClassifySingleImage)]

1. 运行控制台应用 (Ctrl + F5)。 结果应如以下输出所示。  你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。

    ```console
    =============== Training classification model ===============
    Image: broccoli2.jpg predicted as: food with score: 0.8955513
    Image: pizza3.jpg predicted as: food with score: 0.9667718
    Image: teddy6.jpg predicted as: toy with score: 0.9797683
    =============== Classification metrics ===============
    LogLoss is: 0.0653774699265059
    PerClassLogLoss is: 0.110315812569315 , 0.0204391272836966 , 0
    =============== Making single image classification ===============
    Image: toaster3.jpg predicted as: appliance with score: 0.9646884
    ```

祝贺你！ 通过用于处理图像的预先训练的 TensorFlow，你现已 成功在 ML.NET 中生成分类模型，以便对图像进行分类。

可以在 [dotnet/samples](https://github.com/dotnet/samples/tree/main/machine-learning/tutorials/TransferLearningTF) 存储库中找到本教程的源代码。

在本教程中，你将了解：
> [!div class="checklist"]
>
> * 了解问题
> * 将经过预先训练的 TensorFlow 模型合并到 ML.NET 管道中
> * 训练和评估 ML.NET 模型
> * 对测试图像进行分类

请查看机器学习示例 GitHub 存储库，以探索扩展的图像分类示例。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub 存储库](https://github.com/dotnet/machinelearning-samples/)
