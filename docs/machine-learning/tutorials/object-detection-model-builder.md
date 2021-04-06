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
# <a name="tutorial-detect-stop-signs-in-images-with-model-builder"></a>教程：使用 Model Builder 检测图像中的停车标志

了解如何使用 ML.NET Model Builder 和 Azure ML 生成对象检测模型，以检测和定位图像中的停车标志。

在本教程中，你将了解：

> [!div class="checklist"]
>
> - 准备和了解数据
> - 选择方案
> - 选择训练环境
> - 加载数据
> - 定型模型
> - 评估模型
> - 使用预测模型

> [!NOTE]
> 模型生成器当前为预览版。

## <a name="prerequisites"></a>先决条件

有关先决条件和安装说明列表，请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)。

## <a name="model-builder-object-detection-overview"></a>Model Builder 对象检测概述

对象检测是计算机视觉问题。 虽然与图像分类密切相关，但是对象检测以更精细的比例执行图像分类。 对象检测用于定位图像中的实体并对其进行分类。 如果图像包含多个不同类型的对象，请使用对象检测。

![显示图像分类与对象分类的屏幕截图。](./media/object-detection-onnx/img-classification-obj-detection.PNG)

对象检测的一些用例包括：

- 自动驾驶汽车
- 机器人
- 人脸检测
- 工作区安全性
- 对象计数
- 活动识别

此示例将创建一个 C# .NET Core 控制台应用程序，该应用程序使用通过 Model Builder 生成的机器学习模型来检测图像中的停车标志。 有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub 存储库中找到。

## <a name="prepare-and-understand-the-data"></a>准备和了解数据

停车标志数据集包含从 [Unsplash](https://unsplash.com/) 下载的 50 个图像，每个图像都至少包含一个停车标志。

需要做的第一件事就是为图像添加批注，或围绕每个图像中的停车标志绘制边界框。 在本教程中，你将使用一个名为 [VoTT](https://github.com/microsoft/VoTT) 的工具来为图像添加批注。

> 如果想要跳过下面的数据标签标注步骤，可以[下载此数据集版本](https://aka.ms/mlnet-object-detection-tutorial-assets)，然后跳到[创建控制台应用程序](object-detection-model-builder.md#create-a-console-application)。

### <a name="create-a-new-vott-project"></a>新建一个 VoTT 项目

1. [下载数据集](https://aka.ms/mlnet-object-detection-tutorial-dataset)（其中包含 50 个停车标志图像），然后进行解压缩。
1. [下载 VoTT](https://github.com/Microsoft/VoTT/releases)（视觉对象标记工具）。
1. 打开 VoTT，选择“新建项目”。

    ![VoTT 主屏幕](./media/object-detection-model-builder/vott.png)

1. 在“项目设置”中，将“显示名称”更改为“StopSignObjDetection”。
1. 更改安全令牌，以生成新的安全令牌。
1. 在“源连接”旁边，选择“添加连接”。
1. 在“连接设置”中，将源连接的显示名称更改为“StopSignImages”，并选择“本地文件系统”作为提供程序。 对于“文件夹路径”，请选择包含 50 个训练图像的 Stop-Signs 文件夹，然后选择“保存连接”。

    ![VoTT“新建连接”对话框](./media/object-detection-model-builder/vott-new-connection.png)

1. 在“项目设置”中，将“源连接”更改为 StopSignImages（刚创建的连接）。
1. 将“目标连接”也更改为 StopSignImages。 “项目设置”现在应类似于以下屏幕截图：

    ![VoTT“项目设置”对话框](./media/object-detection-model-builder/vott-new-project.png)

1. 选择“保存项目”。

### <a name="add-tag-and-label-images"></a>添加标记并为图像添加标签

现在，你应该会看到一个窗口，其中左侧是所有训练图像的预览图像，中间是所选定图像的预览图像，右侧是标记列。 此屏幕是标记编辑器。

1. 选择“标记”工具栏中的第一个（加号形状）图标，以添加新标记。

    ![VoTT“新建标记”图标](./media/object-detection-model-builder/vott-new-tag-icon.png)

1. 将标记命名为“Stop-Sign”，然后在键盘上按 Enter 键<kbd></kbd>。

    ![VoTT 新标记](./media/object-detection-model-builder/vott-new-tag.png)

1. 单击并拖动鼠标，围绕图像中的每个停车标志绘制一个矩形。 如果无法使用光标绘制矩形，请尝试从顶部的工具栏中选择“绘制矩形”工具，或使用键盘快捷键 R<kbd></kbd>。
1. 绘制矩形后，选择在前面的步骤中创建的 Stop-Sign 标记，将此标记添加到边界框。
1. 单击数据集中下一个图像的预览图像，重复此过程。
1. 继续对每个图像中的每一个停车标志执行步骤 3-4。

    ![VoTT 为图像添加批注](./media/object-detection-model-builder/vott-annotating.gif)

### <a name="export-your-vott-json"></a>导出 VoTT JSON

为所有训练图像添加标签后，可以导出文件，以便 Model Builder 使用它进行训练。

1. 选择左侧工具栏中的第四个图标（框中包含斜箭头的图标），以转到“导出设置”。
1. 将“提供程序”保留为 VoTT JSON。
1. 将“资产状态”更改为“仅带标记的资产”。
1. 取消选中“包括图像”。 如果包括图像，则训练图像将复制到生成的导出文件夹，此操作并不必要。
1. 选择“保存导出设置”。

    ![VoTT“导出设置”](./media/object-detection-model-builder/vott-export.png)

1. 返回到“标记编辑器”（形状类似功能区的左侧工具栏中的第二个图标）。 在顶部工具栏中，选择“导出项目”图标（最后一个图标，形状类似包含箭头的框），或使用键盘快捷键 Ctrl<kbd></kbd>+E<kbd></kbd>。

    ![VoTT“导出”按钮](./media/object-detection-model-builder/vott-export-button.png)

此导出操作将在 Stop-Sign-Images 文件夹中新建一个名为 vott-json-export 的文件夹，并将在此新文件夹中生成名为 StopSignObjDetection-export 的 JSON 文件。 在后续步骤中使用 Model Builder 训练对象检测模型时，你将使用此 JSON 文件。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 在 Visual Studio 中，创建一个名为 StopSignDetection 的 C# .NET Core 控制台应用程序。

## <a name="choose-a-scenario"></a>选择方案

1. 在“解决方案资源管理器”中，右键单击 StopSignDetection 项目，然后选择“添加” > “机器学习”，以打开 Model Builder UI。
1. 对于此示例，方案为对象检测。 在 Model Builder 的“方案”步骤中，选择“对象检测”方案。 

![Visual Studio 中的“模型生成器”向导](./media/object-detection-model-builder/obj-det-scenario.png)

> 如果在方案列表中看不到“对象检测”，则可能需要[更新 Model Builder 的版本](https://marketplace.visualstudio.com/items?itemName=MLNET.07)。

## <a name="choose-the-training-environment"></a>选择训练环境

目前，Model Builder 仅支持使用 Azure 机器学习来训练对象检测模型，因此默认情况下会选择 Azure 训练环境。

![Azure 训练环境选择](./media/object-detection-model-builder/obj-det-environment.png)

若要使用 Azure ML 训练模型，必须从 Model Builder 创建 Azure ML 试验。

Azure ML 试验是一种封装一次或多次机器学习训练运行的配置和结果的资源。

若要创建 Azure ML 试验，首先需要在 Azure 中配置环境。 若要运行试验，需要具备以下条件：

- Azure 订阅
- **工作区**：一种 Azure ML 资源，可集中放置训练运行过程中创建的所有 Azure ML 资源和项目。
- **计算**：Azure 机器学习计算是基于云的 Linux VM，用于训练。 详细了解 [Model Builder 支持的计算类型](../resources/azure-training-concepts-model-builder.md#what-is-an-azure-machine-learning-compute)。

### <a name="set-up-an-azure-ml-workspace"></a>设置 Azure ML 工作区

配置环境：

1. 选择“设置工作区”按钮。
1. 在“新建试验”对话框中，选择你的 Azure 订阅。
1. 选择现有的工作区或新建一个 Azure ML 工作区。

    新建工作区时，将预配以下资源：

    - Azure 机器学习工作区
    - Azure 存储
    - Azure Application Insights
    - Azure 容器注册表
    - Azure Key Vault

    因此，此过程可能需要几分钟的时间。

1. 选择现有计算，或创建新的 Azure ML 计算。 此过程可能需要几分钟时间。
1. 保留默认试验名称，选择“创建”。

    ![Azure 工作区“设置”对话框](./media/object-detection-model-builder/azure-dialog.png)

你已创建第一个试验，并已在工作区中注册该试验名称。 如果任何后续运行使用相同的试验名称，则该运行将记录为同一试验的一部分。 否则会创建新试验。

对配置满意后，选择 Model Builder 中的“下一步”按钮，以转到“数据”步骤。

## <a name="load-the-data"></a>加载数据

在 Model Builder 的“数据”步骤中，选择你的训练数据集。

>Model Builder 目前只接受 VoTT 生成的 JSON 格式，但该团队计划在将来添加对更多格式的支持。 如果希望 Model Builder 中支持某种对象检测数据集格式，请在 [GitHub](https://github.com/dotnet/machinelearning-modelbuilder/issues/new?assignees=&labels=&template=data_suggestion.md&title=) 上提交反馈。

1. 选择“选择文件夹”文本框旁边的按钮，并使用文件资源管理器查找 `StopSignObjDetection-export.json`，该内容应位于 Stop-Signs/vott-json-export 目录中。

    ![Model Builder“数据”步骤](./media/object-detection-model-builder/obj-det-data.png)

1. 数据在“数据预览”中正确显示时，选择“下一步”，以转到“训练”步骤。

## <a name="train-the-model"></a>定型模型

下一步是训练模型。

在 Model Builder“训练”屏幕中，选择“开始训练”按钮。

此时，数据将上传到 Azure 存储，训练过程在 Azure ML 中开始。

>训练过程需要一定的时间，并且根据所选计算的大小和数据量，所需时间可能会有所不同。 第一次在 Azure 中对模型进行训练时，由于必须预配资源，训练时间可能稍长。 对于这个包含 50 个图像的示例而言，训练大约需要 16 分钟。

可以在 Visual Studio 中选择“监视 Azure 门户中的当前运行”链接，以在 Azure 机器学习门户中跟踪运行进度。

训练完成后，选择“下一步”按钮，转到“评估”步骤。

## <a name="evaluate-the-model"></a>评估模型

在“评估”屏幕中，可以大致了解训练过程生成的结果，包括模型准确率。

![Model Builder“评估”步骤](./media/object-detection-model-builder/obj-det-evaluate.png)

在此示例中，准确率显示 100%，这意味着，数据集中的图像太少，很可能会导致模型过度拟合。

可以使用“试用模型”体验，快速检查模型是否按预期工作。

选择“浏览图像”，并提供测试图像，最好是模型未在训练过程中使用的图像。

![Model Builder“试用模型”](./media/object-detection-model-builder/obj-det-try-model.png)

检测到的每个边界框上显示的分数表示检测到的对象的置信度。 例如，在上面的屏幕截图中，围绕停车标志的边界框上的分数表示，该模型对于检测到的对象是停车标志的确定程度为 99%。

“分数阈值”可以通过阈值滑块增加或降低，该阈值将根据分数添加和删除检测到的对象。 例如，如果阈值为 .51，则模型将仅显示置信度分数为 .51 或更高的对象。 增加阈值时，你会看到检测到的对象变少，而降低阈值时，你会看到检测到的对象变多。

如果对准确率指标不满意，尝试提高准确率的一个简单方法是使用更多的数据。 如果满意，则选择“下一步”链接，以转到 Model Builder 中的“代码”步骤。

## <a name="add-the-code-to-make-predictions"></a>添加代码进行预测

培训期间会创建两个项目。

- *StopSignDetectionML.ConsoleApp*：一个 C# .NET Core 控制台应用程序，其中包含模型训练代码和示例使用代码。
- *StopSignDetectionML.Model*：一个 .NET Standard 类库，包含定义输入和输出模型数据架构的数据模型、训练期间性能最佳的模型的保存版本，以及用于执行预测的帮助程序类（称为 `ConsumeModel`）。

1. 在模型生成器的“代码”步骤中，选择“添加项目”，以将自动生成的项目添加到解决方案。
1. 在 StopSignDetection 项目中打开 Program.cs 文件，并在该文件的顶部添加以下 using 语句，以引用 StopSignDetectionML.Model 项目：

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L2)]

1. 下载以下[测试图像](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/test-image1.jpeg)。
1. 在应用程序的 `Main` 方法中将 `ImageSource` 属性设置为测试图像的文件路径，然后创建 `ModelInput` 类的新实例。 用以下代码替换“Hello World”语句：

    [!code-csharp [InputData](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L10-L15)]

1. 使用 `Predict` 类中的 `ConsumeModel` 方法加载训练的模型，为模型创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602)，并对新数据进行预测。 在 `ModelInput` 语句下面添加以下代码：

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L15)]

1. 通过添加以下代码，打印出预测的输出，其中包括标签、坐标和分数：

    [!code-csharp [PrintResults](~/machinelearning-samples/samples/modelbuilder/ObjectDetection_StopSigns/StopSignDetection/Program.cs#L17-L18)]

1. 运行应用程序。

    该程序生成的输出应类似于下面的代码段：

    ```bash
    Predicted Boxes:

    Top: 89.453415, Left: 481.95343, Right: 724.8073, Bottom: 388.32385, Label: Stop-Sign, Score: 0.99539465
    ```

祝贺你！ 你已成功使用 Model Builder 生成用来检测图像中停车标志的机器学习模型。 有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/ObjectDetection_StopSigns) GitHub 存储库中找到。

## <a name="additional-resources"></a>其他资源

若要详细了解本教程中所述的主题，请访问以下资源:

- [模型生成器应用场景](../automate-training-with-model-builder.md#scenario)
- [在 ML.NET 中使用 ONNX 检测对象](object-detection-onnx.md)
