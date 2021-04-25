---
title: 教程：使用 ONNX 深度学习模型检测对象
description: 本教程演示如何在 ML.NET 中使用预训练的 ONNX 深度学习模型来检测图像中的对象。
author: luisquintanilla
ms.author: luquinta
ms.date: 04/13/2021
ms.topic: tutorial
ms.custom: mvc
ms.openlocfilehash: 2146772d8db28568884e485b95c4cfe8e2ebba2d
ms.sourcegitcommit: 5ddbd1f65d0369b4cc8c8ff91c72f1b524c90221
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/15/2021
ms.locfileid: "107514484"
---
# <a name="tutorial-detect-objects-using-onnx-in-mlnet"></a>教程：在 ML.NET 中使用 ONNX 检测对象

了解如何在 ML.NET 中使用预训练的 ONNX 模型来检测图像中的对象。

从头开始训练对象检测模型需要设置数百万个参数、大量已标记的训练数据和海量计算资源（数百个 GPU 小时）。 使用预训练的模型可让你快速完成训练过程。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 了解问题
> - 了解什么是 ONNX 以及它如何与 ML.NET 配合使用
> - 了解模型
> - 重用预训练的模型
> - 使用已加载模型检测对象

## <a name="pre-requisites"></a>先决条件

- 安装了“.NET Core 跨平台开发”工作负载的 [Visual Studio 2019](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=inline+link&utm_content=download+vs2019) 或更高版本或 Visual Studio 2017 版本 15.6 或更高版本。
- [Microsoft.ML Nuget 包](https://www.nuget.org/packages/Microsoft.ML/)
- [Microsoft.ML.ImageAnalytics NuGet 包](https://www.nuget.org/packages/Microsoft.ML.ImageAnalytics/)
- [Microsoft.ML.OnnxTransformer NuGet 包](https://www.nuget.org/packages/Microsoft.ML.OnnxTransformer/)
- [Tiny YOLOv2 预训练的模型](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2)
- [Netron](https://github.com/lutzroeder/netron)（可选）

## <a name="onnx-object-detection-sample-overview"></a>ONNX 对象检测示例概述

此示例创建一个 .NET 核心控制台应用程序，该应用程序使用预训练的深度学习 ONNX 模型检测图像中的对象。 此示例的代码可以在 GitHub 上的 [dotnet/machinelearning-samples 存储库](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx)找到。

## <a name="what-is-object-detection"></a>什么是对象检测？

对象检测是计算机视觉问题。 虽然与图像分类密切相关，但是对象检测以更精细的比例执行图像分类。 对象检测用于定位图像中的实体并对其进行分类。 物体检测模型通常使用深度学习和神经网络进行训练。 有关详细信息，请参阅[深度学习与机器学习](/azure/machine-learning/concept-deep-learning-vs-machine-learning)。

如果图像包含多个不同类型的对象，请使用对象检测。

![显示图像分类与对象分类的屏幕截图。](./media/object-detection-onnx/img-classification-obj-detection.png)

对象检测的一些用例包括：

- 自动驾驶汽车
- 机器人
- 人脸检测
- 工作区安全性
- 对象计数
- 活动识别

## <a name="select-a-deep-learning-model"></a>选择深度学习模型

深度学习是机器学习的一部分。 若要训练深度学习模型，则需要大量的数据。 数据中的模式用一系列层表示。 数据中的关系被编码为包含权重的层之间的连接。 权重越大，关系越强。 总的来说，这一系列的层和连接被称为人工神经网络。 网络中的层越多，它就越“深”，使其成为一个深层的神经网络。

神经网络有多种类型，最常见的是多层感知器 (MLP)、卷积神经网络 (CNN) 和循环神经网络 (RNN)。 最基本的是 MLP，可将一组输入映射到一组输出。 如果数据没有空间或时间组件，建议使用这种神经网络。 CNN 利用卷积层来处理数据中包含的空间信息。 图像处理就是 CNN 的一个很好的用例，它检测图像区域中是否存在特征（例如，图像中心是否有鼻子？） 最后，RNN 允许将状态或内存的持久性用作输入。 RNN 用于时间序列分析，其中事件的顺序排序和上下文很重要。

### <a name="understand-the-model"></a>了解模型

对象检测是图像处理任务。 因此，训练解决该问题的大多数深度学习模型都是 CNN。 本教程中使用的模型是 Tiny YOLOv2 模型，这是该文件中描述的 YOLOv2 模型的一个更紧凑版本：[“YOLO9000：更好、更快、更强”，作者：Redmon 和 Farhadi](https://arxiv.org/pdf/1612.08242.pdf)。 Tiny YOLOv2 在 Pascal VOC 数据集上进行训练，共包含 15 层，可预测 20 种不同类别的对象。 由于 Tiny YOLOv2 是原始 YOLOv2 模型的精简版本，因此需要在速度和精度之间进行权衡。 构成模型的不同层可以使用 Neutron 等工具进行可视化。 检查模型将在构成神经网络的所有层之间生成连接映射，其中每个层都将包含层名称以及各自输入/输出的维度。 用于描述模型输入和输出的数据结构称为张量。 可以将张量视为以 N 维存储数据的容器。 对于 Tiny YOLOv2，输入层名称为 `image`，它需要一个维度为 `3 x 416 x 416` 的张量。 输出层名称为 `grid`，且生成维度为 `125 x 13 x 13` 的输出张量。

![将输入层拆分为隐藏层，然后拆分输出层](./media/object-detection-onnx/netron-model-map-layers.png)

YOLO 模型采用图像 `3(RGB) x 416px x 416px`。 模型接受此输入，并将其传递到不同的层以生成输出。 输出将输入图像划分为一个 `13 x 13` 网格，网格中的每个单元格由 `125` 值组成。

### <a name="what-is-an-onnx-model"></a>什么是 ONNX 模型？

开放神经网络交换 (ONNX) 是 AI 模型的开放源代码格式。 ONNX 支持框架之间的互操作性。 这意味着，你可以在许多常见的机器学习框架（如 pytorch）中训练模型，将其转换为 ONNX 格式，并在其他框架（如 ML.NET）中使用 ONNX 模型。 有关详细信息，请参阅 [ONNX 网站](https://onnx.ai/)。

![所使用的 ONNX 支持格式的关系图。](./media/object-detection-onnx/onnx-supported-formats.png)

预训练的 Tiny YOLOv2 模型以 ONNX 格式存储，这是层的序列化表示形式，也是这些层的已知模式。 在 ML.NET 中，使用 [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) 和 [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) NuGet 包实现了与 ONNX 的互操作性。 [`ImageAnalytics`](xref:Microsoft.ML.Transforms.Image) 包包含一系列转换，这些转换采用图像并将其编码为可用作预测或训练管道输入的数值。 [`OnnxTransformer`](xref:Microsoft.ML.Transforms.Onnx.OnnxTransformer) 包利用 ONNX 运行时加载 ONNX 模型并使用它根据提供的输入进行预测。

![ONNX 文件到 ONNX 运行时的数据流。](./media/object-detection-onnx/onnx-ml-net-integration.png)

## <a name="set-up-the-net-core-project"></a>设置 .NET Core 项目

对 ONNX 的涵义以及 Tiny YOLOv2 的工作原理有了大致了解之后，接下来了解如何生成应用程序。

### <a name="create-a-console-application"></a>创建控制台应用程序

1. 创建名为 ObjectDetection 的 .NET Core 控制台应用程序。

1. 安装“Microsoft.ML NuGet 包”  ：

    [!INCLUDE [mlnet-current-nuget-version](../../../includes/mlnet-current-nuget-version.md)]

    - 在“解决方案资源管理器”中，右键单击项目，然后选择“管理 NuGet 包”  。
    - 选择“nuget.org”作为“包源”，选择“浏览”选项卡，再搜索“Microsoft.ML”。
    - 选择“安装”按钮  。
    - 选择“预览更改”对话框上的“确定”按钮，如果你同意所列包的许可条款，则选择“接受许可”对话框上的“我接受”按钮。
    - 对 Microsoft.ML.ImageAnalytics、Microsoft.ML.OnnxTransformer 和 Microsoft.ML.OnnxRuntime 重复这些步骤  。

### <a name="prepare-your-data-and-pre-trained-model"></a>准备你的数据和预训练的模型

1. 下载并解压缩[项目资产目录 zip 文件](https://github.com/dotnet/machinelearning-samples/raw/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/assets.zip)。

1. 将 `assets` 目录复制到 ObjectDetection 项目目录中。 此目录及其子目录包含本教程所需的图像文件（Tiny YOLOv2 模型除外，将在下一步中下载并添加此模型）。

1. 从 [ONNX Model Zoo](https://github.com/onnx/models/tree/master/vision/object_detection_segmentation/tiny-yolov2) 下载并解压缩 [Tiny YOLOv2 模型](https://onnxzoo.blob.core.windows.net/models/opset_8/tiny_yolov2/tiny_yolov2.tar.gz)。

    打开命令提示符并输入以下命令：

    ```shell
    tar -xvzf tiny_yolov2.tar.gz
    ```

1. 将提取的 `model.onnx` 文件从刚刚解压缩的目录复制到 ObjectDetection 项目的 `assets\Model` 目录中，并将其重命名为 `TinyYolo2_model.onnx`。 此目录包含本教程所需的模型。

1. 在“解决方案资源管理器”中，右键单击资产目录和子目录中的每个文件，再选择“属性”。 在“高级”下，将“复制到输出目录”的值更改为“如果较新则复制”    。

### <a name="create-classes-and-define-paths"></a>创建类和定义路径

打开 Program.cs 文件，并将以下附加的 `using` 语句添加到该文件顶部：

[!code-csharp [ProgramUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L1-L7)]

接下来，定义各种资产的路径。

1. 首先，在 `Program` 类的 `Main` 方法下面添加 `GetAbsolutePath` 方法。

    [!code-csharp [GetAbsolutePath](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L66-L74)]

1. 然后，在 `Main` 方法内，创建字段以存储资产的位置。

    [!code-csharp [AssetDefinition](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L17-L21)]

向项目添加新目录以存储输入数据和预测类。

在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”  。 当新文件夹出现在解决方案资源管理器中时，将其命名为“DataStructures”。

在新创建的“DataStructures”目录中创建输入数据类。

1. 在“解决方案资源管理器”中，右键单击“DataStructures”目录，然后选择“添加” > “新项” 。
1. 在“添加新项”对话框中，选择“类”，并将“名称”字段更改为“ImageNetData.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 ImageNetData.cs 文件。 将下面的 `using` 语句添加到 ImageNetData.cs 顶部：

    [!code-csharp [ImageNetDataUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L1-L4)]

    删除现有类定义，并将 `ImageNetData` 类的以下代码添加到 ImageNetData.cs 文件中：

    [!code-csharp [ImageNetDataClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetData.cs#L8-L23)]

    `ImageNetData` 是输入图像数据类，包含以下 <xref:System.String> 字段：

    - `ImagePath` 包含存储图像的路径。
    - `Label` 包含文件的名称。

    此外，`ImageNetData` 包含方法 `ReadFromFile`，该方法加载存储在指定的 `imageFolder` 路径中的多个图像文件，并将它们作为 `ImageNetData` 对象的集合返回。

在“DataStructures”目录中创建预测类。

1. 在“解决方案资源管理器”中，右键单击“DataStructures”目录，然后选择“添加” > “新项” 。
1. 在“添加新项”对话框中，选择“类”，并将“名称”字段更改为“ImageNetPrediction.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 ImageNetPrediction.cs 文件。 将下面的 `using` 语句添加到 ImageNetPrediction.cs 顶部：

    [!code-csharp [ImageNetPredictionUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L1)]

    删除现有类定义，并将 `ImageNetPrediction` 类的以下代码添加到 ImageNetPrediction.cs 文件中：

    [!code-csharp [ImageNetPredictionClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/DataStructures/ImageNetPrediction.cs#L5-L9)]

    `ImageNetPrediction` 是预测数据类，包含以下 `float[]` 字段：

    - `PredictedLabel` 包含图像中检测到的每个边界框的尺寸、对象分数和类概率。

### <a name="initialize-variables-in-main"></a>在 Main 中初始化变量

执行所有 ML.NET 操作都是从 [MLContext 类](xref:Microsoft.ML.MLContext)开始，初始化 `mlContext` 可创建一个新的 ML.NET 环境，可在模型创建工作流对象之间共享该环境。 从概念上讲，它与实体框架中的 `DBContext` 类似。

通过将以下行添加到 `outputFolder` 字段下 Program.cs 的 `Main` 方法，使用新的 `MLContext` 实例初始化 `mlContext` 变量。

[!code-csharp [InitMLContext](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L24)]

## <a name="create-a-parser-to-post-process-model-outputs"></a>创建分析器来处理模型输出

该模型将图像分割为 `13 x 13` 网格，其中每个网格单元格为 `32px x 32px`。 每个网格单元格包含 5 个潜在的对象边界框。 边界框有 25 个元素：

![左侧是网格示例，右侧是边界框示例](./media/object-detection-onnx/model-output-description.png)

- `x`：边界框中心相对于与其关联的网格单元格的 x 位置。
- `y`：边界框中心相对于与其关联的网格单元格格的 y 位置。
- `w`：边界框的宽度。
- `h`：边界框的高度。
- `o`：对象存在于边界框内的置信度值，也称为对象得分。
- 模型预测的 20 个类中每个类的 `p1-p20` 类概率。

总之，描述 5 个边界框中每个框的 25 个元素构成了每个网格单元格中包含的 125 个元素。

预训练的 ONNX 模型生成的输出是长度为 `21125` 的浮点数组，表示维度为 `125 x 13 x 13` 的张量元素。 为了将模型生成的预测转换为张量，需要进行一些后处理工作。 为此，请创建一组类以帮助分析输出。

向项目中添加一个新目录以组织分析器类集。

1. 在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新文件夹”  。 当新文件夹出现在解决方案资源管理器中时，将其命名为“YoloParser”。

### <a name="create-bounding-boxes-and-dimensions"></a>创建边界框和维度

模型输出的数据包含图像中对象边界框的坐标和维度。 创建维度的基类。

1. 在“解决方案资源管理器”中，右键单击“YoloParser”目录，然后选择“添加” > “新项” 。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“DimensionsBase.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 DimensionsBase.cs 文件。 删除所有 `using` 语句和现有类定义。

    将 `DimensionsBase` 类的以下代码添加到 DimensionsBase.cs 文件中：

    [!code-csharp [DimensionsBaseClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/DimensionsBase.cs#L3-L9)]

    `DimensionsBase` 具有以下 `float` 属性：

    - `X`：包含对象沿 x 轴的位置。
    - `Y`：包含对象沿 y 轴的位置。
    - `Height`：包含对象的高度。
    - `Width`：包含对象的宽度。

接下来，为边界框创建一个类。

1. 在“解决方案资源管理器”中，右键单击“YoloParser”目录，然后选择“添加” > “新项” 。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“YoloBoundingBox.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 YoloBoundingBox.cs 文件。 将下面的 `using` 语句添加到 YoloBoundingBox.cs 顶部：

    [!code-csharp [YoloBoundingBoxUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L1)]

    在现有类定义的正上方，添加一个名为 `BoundingBoxDimensions` 的新类定义，该定义继承自 `DimensionsBase` 类以包含相应边界框的维度。

    [!code-csharp [BoundingBoxDimClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L5)]

    删除现有 `YoloBoundingBox` 类定义，并将 `YoloBoundingBox` 类的以下代码添加到 YoloBoundingBox.cs 文件中：

    [!code-csharp [YoloBoundingBoxClass](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloBoundingBox.cs#L7-L21)]

    `YoloBoundingBox` 具有以下属性：

    - `Dimensions`：包含边界框的维度。
    - `Label`：包含在边界框内检测到的对象类。
    - `Confidence`：包含类的置信度。
    - `Rect`：包含边界框维度的矩形表示形式。
    - `BoxColor`：包含与用于在图像上绘制的相应类关联的颜色。

### <a name="create-the-parser"></a>创建分析器

创建维度和边界框的类之后，接下来创建分析器。

1. 在“解决方案资源管理器”中，右键单击“YoloParser”目录，然后选择“添加” > “新项” 。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“YoloOutputParser.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 YoloOutputParser.cs 文件。 将下面的 `using` 语句添加到 YoloOutputParser.cs 顶部：

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L1-L4)]

    在现有 `YoloOutputParser` 类定义中，添加一个嵌套类，其中包含图像中每个单元格的尺寸。 为 `CellDimensions` 类添加以下代码，该类继承自 `YoloOutputParser` 类定义顶部的 `DimensionsBase` 类。

    [!code-csharp [YoloParserUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L10)]

1. 在 `YoloOutputParser` 类定义中，添加以下常量和字段。

    [!code-csharp [ParserVarDefinitions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L12-L21)]

    - `ROW_COUNT` 是分割图像的网格中的行数。
    - `COL_COUNT` 是分割图像的网格中的列数。
    - `CHANNEL_COUNT` 是其中一个网格单元格中包含的值的总数。
    - `BOXES_PER_CELL` 是单元格中边界框的数量，
    - `BOX_INFO_FEATURE_COUNT` 是框中包含的特征数（x、y、高度、宽度、置信度）。
    - `CLASS_COUNT` 是每个边界框中包含的类预测数。
    - `CELL_WIDTH` 是图像网格中一个单元格的宽度。
    - `CELL_HEIGHT` 是图像网格中一个单元格的高度。
    - `channelStride` 是网格中当前单元格的起始位置。

    当模型进行预测（也称为评分）时，它会将 `416px x 416px` 输入图像划分为 `13 x 13` 大小的单元格网格。 每个单元格都包含 `32px x 32px`。 在每个单元格内，有 5 个边界框，每个边框包含 5 个特征（x、y、宽度、高度、置信度）。 此外，每个边界框包含每个类的概率，在这种情况下为 20。 因此，每个单元包含 125 条信息（5 个特征 + 20 个类概率）。

为所有 5 个边界框在 `channelStride` 下创建的定位点列表：

[!code-csharp [ParserAnchors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L23-L26)]

定位点是边界框的预定义的高度和宽度比例。 模型检测到的大多数对象或类都具有相似的比例。 这在创建边界框时非常有用。 不是预测边界框，而是计算预定义维度的偏移量，因此减少了预测边界框所需的计算量。 通常，这些定位点比例是基于所使用的数据集计算的。 在这种情况下，由于数据集是已知的且值已预先计算，因此可以硬编码定位点。

接下来，定义模型将预测的标签或类。 该模型预测了 20 个类，它们是原始 YOLOv2 模型预测的类总数的子集。

在 `anchors` 下面添加标签列表。

[!code-csharp [ParserLabels](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L28-L34)]

每个类都有相关联的颜色。 在 `labels` 下分配类的颜色：

[!code-csharp [ParserColors](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L36-L59)]

### <a name="create-helper-functions"></a>创建帮助程序函数

后处理阶段涉及一系列步骤。 为此，可以使用几种帮助程序方法。

分析器使用的帮助程序方法是：

- `Sigmoid` 应用 sigmoid 函数，该函数输出介于 0 和 1 之间的数字。
- `Softmax` 将输入向量规范化为概率分布。
- `GetOffset` 将一维模型输出中的元素映射到 `125 x 13 x 13` 张量中的对应位置。
- `ExtractBoundingBoxes` 使用模型输出中的 `GetOffset` 方法提取边界框维度。
- `GetConfidence` 提取置信度值，该值表示模型是否检测到对象，并使用 `Sigmoid` 函数将其转换为百分比。
- `MapBoundingBoxToCell` 使用边界框维度并将它们映射到图像中的相应单元格。
- `ExtractClasses` 使用 `GetOffset` 方法从模型输出中提取边界框的类预测，并使用 `Softmax` 方法将它们转换为概率分布。
- `GetTopResult` 从具有最高概率的预测类列表中选择类。
- `IntersectionOverUnion` 筛选具有较低概率的重叠边界框。

在 `classColors` 列表下面添加所有帮助程序方法的代码。

[!code-csharp [ParserHelperMethods](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L61-L151)]

定义完所有帮助程序方法之后，即可使用它们来处理模型输出。

在 `IntersectionOverUnion` 方法下面，创建 `ParseOutputs` 方法以处理模型生成的输出。

```csharp
public IList<YoloBoundingBox> ParseOutputs(float[] yoloModelOutputs, float threshold = .3F)
{

}
```

创建一个列表来存储边界框并在 `ParseOutputs` 方法中定义变量。

[!code-csharp [BBoxList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L155)]

每个图像都被分成一个 `13 x 13` 单元格的网格。 每个单元格包含五个边界框。 在 `boxes` 变量下方，添加代码以处理每个单元格中的所有框。

```csharp
for (int row = 0; row < ROW_COUNT; row++)
{
    for (int column = 0; column < COL_COUNT; column++)
    {
        for (int box = 0; box < BOXES_PER_CELL; box++)
        {

        }
    }
}
```

在最内层循环内，计算一维模型输出中当前框的起始位置。

[!code-csharp [ChannelDef](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L163)]

在其下方，使用 `ExtractBoundingBoxDimensions` 方法获取当前边界框的维度。

[!code-csharp [GetBBoxDims](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L165)]

然后，使用 `GetConfidence` 方法获取当前边界框的置信度。

[!code-csharp [GetConfidence](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L167)]

之后，使用 `MapBoundingBoxToCell` 方法将当前边界框映射到正在处理的当前单元格。

[!code-csharp [MapBoundingBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L169)]

在进行任何进一步的处理之前，请检查置信值是否大于提供的阈值。 如果没有，请处理下一个边界框。

[!code-csharp [CheckThreshold1](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L171-L172)]

否则，继续处理输出。 下一步是使用 `ExtractClasses` 方法获取当前边界框的预测类的概率分布。

[!code-csharp [ExtractClasses](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L174)]

然后，使用 `GetTopResult` 方法获取当前框概率最高的类的值和索引，并计算其得分。

[!code-csharp [GetTopResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L176-L177)]

再次使用 `topScore` 只保留那些超过指定阈值的边界框。

[!code-csharp [CheckThreshold2](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L179-L180)]

最后，如果当前边界框超过阈值，请创建一个新的 `BoundingBox` 对象并将其添加到 `boxes` 列表中。

[!code-csharp [AddBBoxToList](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L182-L194)]

处理完图像中的所有单元格后，返回 `boxes` 列表。 在 `ParseOutputs` 方法的最外层 for 循环下面添加以下 return 语句。

[!code-csharp [ReturnBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L198)]

### <a name="filter-overlapping-boxes"></a>筛选重叠框

从模型输出中提取所有高置信度的边界框之后，接下来需要进行额外的筛选以移除重叠的图像。 在 `ParseOutputs` 方法下添加一个名为 `FilterBoundingBoxes` 的方法：

```csharp
public IList<YoloBoundingBox> FilterBoundingBoxes(IList<YoloBoundingBox> boxes, int limit, float threshold)
{

}
```

在 `FilterBoundingBoxes` 方法中，首先创建一个等于检测到的框大小的数组，并将所有插槽标记为“活动”或“待处理”。

[!code-csharp [InitActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L203-L207)]

然后，根据置信度按降序对包含边界框的列表进行排序。

[!code-csharp [SortBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L209-L211)]

之后，创建一个列表来保存筛选的结果。

[!code-csharp [InitFilterResult](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L213)]

通过遍历每个边界框，开始处理每个边界框。

```csharp
for (int i = 0; i < boxes.Count; i++)
{

}
```

在此 for 循环内部，检查是否可以处理当前边界框。

```csharp
if (isActiveBoxes[i])
{

}
```

如果可以，请将边界框添加到结果列表中。 如果结果超出了要提取的框的指定限制，则中断循环。 在 if 语句中添加以下代码。

[!code-csharp [AddFirstBBoxToResults](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L219-L223)]

否则，请查看相邻的边界框。 在框限制检查下面添加以下代码。

```csharp
for (var j = i + 1; j < boxes.Count; j++)
{

}
```

与第一个框一样，如果相邻框处于活动或待处理状态，请使用 `IntersectionOverUnion` 方法检查第一个框和第二个框是否超出指定的阈值。 将以下代码添加到最内层的 for 循环中。

[!code-csharp [IOUBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L227-L239)]

在检查相邻边界框的最内部 for 循环之外，查看是否有任何剩余的边界框要处理。 如果没有，则中断外部 for 循环。

[!code-csharp [CheckActiveSlots](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L242-L243)]

最后，在 `FilterBoundingBoxes` 方法的初始 for 循环之外，返回结果：

[!code-csharp [ReturnFilteredBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/YoloParser/YoloOutputParser.cs#L246)]

很棒！ 现在可以将此代码与模型配合使用，以进行评分。

## <a name="use-the-model-for-scoring"></a>使用该模型进行评分

就像后处理一样，评分步骤也有几个步骤。 为此，请向项目添加包含评分逻辑的类。

1. 在“解决方案资源管理器”中，右键单击项目，然后选择“添加” > “新项”。
1. 在“添加新项”对话框中，选择“类”并将“名称”字段更改为“OnnxModelScorer.cs”  。 然后，选择“添加”  按钮。

    此时，将在代码编辑器中打开 OnnxModelScorer.cs 文件。 将下面的 `using` 语句添加到 OnnxModelScorer.cs 顶部：

    [!code-csharp [ScorerUsings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L1-L7)]

    在 `OnnxModelScorer` 类定义中，添加以下变量。

    [!code-csharp [InitScorerVars](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L13-L17)]

    在其下方，为 `OnnxModelScorer` 类创建一个构造函数，用于初始化先前定义的变量。

    [!code-csharp [ScorerCtor](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L19-L24)]

    创建构造函数之后，定义两个结构，其中包含与图像和模型设置相关的变量。 创建一个名为 `ImageNetSettings` 的结构，以包含预期作为模型输入的高度和宽度。

    [!code-csharp [ImageNetSettingStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L26-L30)]

    然后，创建另一个名为 `TinyYoloModelSettings` 的结构，其中包含模型的输入层和输出层名称。 若要可视化模型的输入层和输出层名称，可以使用 [Netron](https://github.com/lutzroeder/netron) 之类的工具。

    [!code-csharp [YoloSettingsStruct](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L32-L43)]

    接下来，创建用于评分的第一组方法。 在 `OnnxModelScorer` 类中创建 `LoadModel` 方法。

    ```csharp
    private ITransformer LoadModel(string modelLocation)
    {

    }
    ```

    在 `LoadModel` 方法中，添加以下代码以进行日志记录。

    [!code-csharp [LoadModelLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L47-L49)]

    ML.NET 管道需要知道在调用 [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit%2A) 方法时要操作的数据架构。 在这种情况下，将使用类似于训练的过程。 但是，由于没有进行实际训练，因此可以使用空的 [`IDataView`](xref:Microsoft.ML.IDataView)。 使用空列表为管道创建新的 [`IDataView`](xref:Microsoft.ML.IDataView)。

    [!code-csharp [LoadEmptyIDV](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L52)]

    在此之后，定义管道。 管道将包含四个转换。

    - [`LoadImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.LoadImages%2A) 将图片作为位图加载。
    - [`ResizeImages`](xref:Microsoft.ML.ImageEstimatorsCatalog.ResizeImages%2A) 将图片重新调整为指定的大小（在本例中为 `416 x 416`）。
    - [`ExtractPixels`](xref:Microsoft.ML.ImageEstimatorsCatalog.ExtractPixels%2A) 将图像的像素表示形式从位图更改为数字向量。
    - [`ApplyOnnxModel`](xref:Microsoft.ML.OnnxCatalog.ApplyOnnxModel%2A) 加载 ONNX 模型并使用它对提供的数据进行评分。

    在 `data` 变量下面的 `LoadModel` 方法中定义管道。

    [!code-csharp [ScoringPipeline](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L55-L58)]

    现在，可以实例化模型以进行评分。 调用管道上的 [`Fit`](xref:Microsoft.ML.IEstimator%601.Fit%2A) 方法并将其返回以进行进一步处理。

    [!code-csharp [FitReturnModel](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L61-L63)]

加载模型后，可将其用来进行预测。 若要简化该过程，请在 `LoadModel` 方法下创建一个名为 `PredictDataUsingModel` 的方法。

```csharp
private IEnumerable<float[]> PredictDataUsingModel(IDataView testData, ITransformer model)
{

}
```

在 `PredictDataUsingModel` 中，添加以下代码以进行日志记录。

[!code-csharp [PredictDataLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L68-L71)]

然后，使用 [`Transform`](xref:Microsoft.ML.ITransformer.Transform%2A) 方法对数据进行评分。

[!code-csharp [ScoreImages](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L73)]

提取预测的概率并将其返回以进行其他处理。

[!code-csharp [ReturnModelOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L75-L77)]

现在已经设置了两个步骤，将它们合并为一个方法。 在 `PredictDataUsingModel` 方法下面，添加一个名为 `Score` 的新方法。

[!code-csharp [ScoreMethod](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/OnnxModelScorer.cs#L80-L85)]

马上就大功告成了！ 现在可以将其全部投入使用。

## <a name="detect-objects"></a>检测对象

现在所有设置都已完成，可以检测一些对象。 首先在 Program.cs 类中添加对评分器和分析器的引用。

[!code-csharp [ReferenceScorerParser](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L8-L9)]

### <a name="score-and-parse-model-outputs"></a>对模型输出进行评分和分析

在 Program.cs 类的 `Main` 方法内，添加 try-catch 语句。

```csharp
try
{

}
catch (Exception ex)
{
    Console.WriteLine(ex.ToString());
}
```

在 `try` 块内部，开始实现对象检测逻辑。 首先，将数据加载到 [`IDataView`](xref:Microsoft.ML.IDataView) 中。

[!code-csharp [LoadData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L29-L30)]

然后，创建 `OnnxModelScorer` 的实例，并使用它对加载的数据进行评分。

[!code-csharp [ScoreData](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L33-L36)]

现在执行后处理步骤。 创建 `YoloOutputParser` 的实例并使用它来处理模型输出。

[!code-csharp [ParsePredictions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L39-L44)]

处理完模型输出后，便可以在图像上绘制边界框。

### <a name="visualize-predictions"></a>将预测结果可视化

在模型对图像进行评分并处理好输出后，必须在图像上绘制边界框。 为此，请在 Program.cs 内的 `GetAbsolutePath` 方法下添加名为 `DrawBoundingBox` 的方法。

```csharp
private static void DrawBoundingBox(string inputImageLocation, string outputImageLocation, string imageName, IList<YoloBoundingBox> filteredBoundingBoxes)
{

}
```

首先，加载图像并使用 `DrawBoundingBox` 方法获取高度和宽度尺寸。

[!code-csharp [LoadImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L78-L81)]

然后，创建 for-each 循环以遍历模型检测到的每个边界框。

```csharp
foreach (var box in filteredBoundingBoxes)
{

}
```

在 for each 循环的内部，获取边界框的维度。

[!code-csharp [GetBBoxDimensions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L86-L89)]

由于边界框的维度对应于 `416 x 416` 的模型输入，因此缩放边界框维度以匹配图像的实际尺寸。

[!code-csharp [ScaleImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L92-L95)]

然后，为将出现在每个边界框上方的文本定义模板。 文本将包含相应边界框内的对象类以及置信度。

[!code-csharp [DefineBBoxText](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L98)]

若要在图像上绘制，请将其转换为 [`Graphics`](xref:System.Drawing.Graphics) 对象。

```csharp
using (Graphics thumbnailGraphic = Graphics.FromImage(image))
{

}
```

在 `using` 代码块内，调整图形的 [`Graphics`](xref:System.Drawing.Graphics) 对象设置。

[!code-csharp [TuneGraphicSettings](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L102-L104)]

在下面，设置文本和边界框的字体和颜色选项。

[!code-csharp [SetColorOptions](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L106-L114)]

使用 [`FillRectangle`](xref:System.Drawing.Graphics.FillRectangle%2A) 方法创建并填充边界框上方的矩形以包含文本。 这将有助于对比文本，提高可读性。

[!code-csharp [DrawTextBackground](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L117)]

然后，使用 [`DrawString`](xref:System.Drawing.Graphics.DrawString%2A) 和 [`DrawRectangle`](xref:System.Drawing.Graphics.DrawRectangle%2A) 方法在图像上绘制文本和边界框。

[!code-csharp [DrawClassAndBBox](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L118-L121)]

在 for-each 循环之外，添加代码以保存 `outputDirectory` 中的图像。

[!code-csharp [SaveImage](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L125-L130)]

要获得应用程序在运行时按预期进行预测的其他反馈，请在 Program.cs 文件中的 `DrawBoundingBox` 方法下添加名为 `LogDetectedObjects` 的方法，以将检测到的对象输出到控制台。

[!code-csharp [LogOutputs](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L133-L143)]

你已经有了帮助器方法，现在可以根据预测创建视觉反馈，并可添加 for 循环来循环访问每个已评分的图像。

```csharp
for (var i = 0; i < images.Count(); i++)
{

}
```

在 for 循环内部，获取图像文件的名称以及与之关联的边界框。

[!code-csharp [GetImageFileName](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L49-L50)]

在此之后，使用 `DrawBoundingBox` 方法在图像上绘制边界框。

[!code-csharp [DrawBBoxes](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L52)]

最后，使用 `LogDetectedObjects` 方法将预测输出到控制台。

[!code-csharp [LogPredictionsOutput](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L54)]

在 try-catch 语句之后，添加其他逻辑以指示进程已完成运行。

[!code-csharp [EndProcessLog](~/machinelearning-samples/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx/ObjectDetectionConsoleApp/Program.cs#L62-L63)]

就这么简单！

## <a name="results"></a>结果

按照上述步骤操作后，运行控制台应用 (Ctrl + F5)。 结果应如以下输出所示。 你可能会看到警告或处理消息，为清楚起见，这些消息已从以下结果中删除。

```console
=====Identify the objects in the images=====

.....The objects in the image image1.jpg are detected as below....
car and its Confidence score: 0.9697262
car and its Confidence score: 0.6674225
person and its Confidence score: 0.5226039
car and its Confidence score: 0.5224892
car and its Confidence score: 0.4675332

.....The objects in the image image2.jpg are detected as below....
cat and its Confidence score: 0.6461141
cat and its Confidence score: 0.6400049

.....The objects in the image image3.jpg are detected as below....
chair and its Confidence score: 0.840578
chair and its Confidence score: 0.796363
diningtable and its Confidence score: 0.6056048
diningtable and its Confidence score: 0.3737402

.....The objects in the image image4.jpg are detected as below....
dog and its Confidence score: 0.7608147
person and its Confidence score: 0.6321323
dog and its Confidence score: 0.5967442
person and its Confidence score: 0.5730394
person and its Confidence score: 0.5551759

========= End of Process..Hit any Key ========
```

若要查看带有边界框的图像，请导航到 `assets/images/output/` 目录。 以下是其中一个已处理的图像示例。

![已处理的餐厅图像示例](./media/object-detection-onnx/dinning-room-table-chairs.png)

祝贺你！ 现已通过重用 ML.NET 中的预训练 `ONNX` 模型，成功生成了对象检测机器学习模型。

可以在 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx) 存储库中找到本教程的源代码。

在本教程中，你将了解：
> [!div class="checklist"]
>
> - 了解问题
> - 了解什么是 ONNX 以及它如何与 ML.NET 配合使用
> - 了解模型
> - 重用预训练的模型
> - 使用已加载模型检测对象

请查看机器学习示例 GitHub 存储库，以探索扩展的对象检测示例。
> [!div class="nextstepaction"]
> [dotnet/machinelearning-samples GitHub 存储库](https://github.com/dotnet/machinelearning-samples/tree/main/samples/csharp/getting-started/DeepLearning_ObjectDetection_Onnx)
