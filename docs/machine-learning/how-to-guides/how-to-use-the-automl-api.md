---
title: 如何使用 ML.NET 自动化 ML API
description: ML.NET 自动化 ML API 可自动化模型生成过程并生成可供部署的模型。 了解可用于配置自动化机器学习任务的选项。
ms.date: 12/18/2019
ms.custom: mvc,how-to
ms.openlocfilehash: 31204610d471f13aca177f0d599c1eba5cd7a624
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876327"
---
# <a name="how-to-use-the-mlnet-automated-machine-learning-api"></a>如何使用 ML.NET 自动化机器学习 API

自动化机器学习 (AutoML) 可自动化将机器学习应用于数据的过程。 在给定数据集的情况下，可以运行 AutoML **试验** 来迭代不同的数据特征、机器学习算法和超参数，从而选择最佳模型。

> [!NOTE]
> 本主题涉及 ML.NET 目前处于预览状态的自动化机器学习 API。 材料可能会有所变化。

## <a name="load-data"></a>加载数据

自动化机器学习支持将数据集加载到 [IDataView](xref:Microsoft.ML.IDataView) 中。 数据可以采用制表符分隔值 (TSV) 文件和逗号分隔值 (CSV) 文件的形式。

示例：

```csharp
using Microsoft.ML;
using Microsoft.ML.AutoML;
    // ...
    MLContext mlContext = new MLContext();
    IDataView trainDataView = mlContext.Data.LoadFromTextFile<SentimentIssue>("my-data-file.csv", hasHeader: true);
```

## <a name="select-the-machine-learning-task-type"></a>选择机器学习任务类型

在创建试验之前，请确定想要解决的机器学习问题的类型。 自动化机器学习支持以下 ML 任务：

* 二元分类
* 多类分类
* 回归测试
* 建议

## <a name="create-experiment-settings"></a>创建试验设置

为确定的 ML 任务类型创建试验设置：

* 二元分类

  ```csharp
  var experimentSettings = new BinaryExperimentSettings();
  ```

* 多类分类

  ```csharp
  var experimentSettings = new MulticlassExperimentSettings();
  ```

* 回归测试

  ```csharp
  var experimentSettings = new RegressionExperimentSettings();
  ```

* 建议

  ```csharp
  var experimentSettings = new RecommendationExperimentSettings();
  ```

## <a name="configure-experiment-settings"></a>配置试验设置

试验具有高度可配置性。 有关配置设置的完整列表，请参阅 [AutoML API 文档](/dotnet/api/microsoft.ml.automl?view=ml-dotnet-preview)。

示例包括：

1. 指定试验的最长允许运行时间。

    ```csharp
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    ```

1. 在试验的计划完成时间之前，使用取消令牌取消试验。

    ```csharp
    experimentSettings.CancellationToken = cts.Token;

    // Cancel experiment after the user presses any key
    CancelExperimentAfterAnyKeyPress(cts);
    ```

1. 指定其他优化指标。

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.OptimizingMetric = RegressionMetric.MeanSquaredError;
    ```

1. `CacheDirectory` 设置是指向目录的指针，执行 AutoML 任务期间训练的所有模型都将保存在该目录中。 如果 `CacheDirectory` 设置为 null，则模型将保留在内存中，而不是写入磁盘。

    ```csharp
    experimentSettings.CacheDirectory = null;
    ```

1. 指示自动化 ML 不要使用特定训练程序。

    将根据每个任务探索待优化训练程序的默认列表。 此列表可以针对每个试验进行修改。 例如，可以从列表中删除在数据集上运行缓慢的训练程序。 若要优化一个特定的训练程序，请调用 `experimentSettings.Trainers.Clear()`，然后添加想要使用的训练程序。

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.Trainers.Remove(RegressionTrainer.LbfgsPoissonRegression);
    experimentSettings.Trainers.Remove(RegressionTrainer.OnlineGradientDescent);
    ```

每个 ML 任务支持的训练程序的列表可在以下相应链接中找到：

* [支持的二元分类算法](xref:Microsoft.ML.AutoML.BinaryClassificationTrainer)
* [支持的多类分类算法](xref:Microsoft.ML.AutoML.MulticlassClassificationTrainer)
* [支持的回归算法](xref:Microsoft.ML.AutoML.RegressionTrainer)
* [支持的推荐算法](xref:Microsoft.ML.AutoML.RecommendationTrainer)

## <a name="optimizing-metric"></a>优化指标

如以上示例所示，优化指标可确定在模型训练期间要优化的指标。 可以选择的优化指标由选择的任务类型决定。 以下是可用指标的列表。

|[二元分类](xref:Microsoft.ML.AutoML.BinaryClassificationMetric) | [多类分类](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric) |[回归和建议](xref:Microsoft.ML.AutoML.RegressionMetric)
|-- |-- |--
|精确度| LogLoss | RSquared
|AreaUnderPrecisionRecallCurve | LogLossReduction | MeanAbsoluteError
|AreaUnderRocCurve | MacroAccuracy | MeanSquaredError
|F1Score | MicroAccuracy | RootMeanSquaredError
|NegativePrecision | TopKAccuracy
|NegativeRecall |
|PositivePrecision
|PositiveRecall

## <a name="data-pre-processing-and-featurization"></a>数据预处理和特征化

> [!NOTE]
> 特征列仅支持 <xref:System.Boolean>、<xref:System.Single> 和 <xref:System.String> 类型。

默认情况下会进行数据预处理，并会自动执行以下步骤：

1. 删除没有有用信息的特征

    从训练和验证集中删除不包含任何有用信息的特征。 这包括缺少所有值的特征、在所有行中使用相同值的特征，或者包含极高基数（例如哈希、ID 或 GUID）的特征。

1. 缺少值指示和插补

    使用数据类型的默认值填充缺少值的单元格。 使用与输入列数量相同的插槽数追加指示特征。 如果输入列中缺少值，则追加的指示特征中的值为 `1`，否则为 `0`。

1. 生成其他特征

    对于文本特征：生成使用一元语法和三元语法的词袋特征。

    对于分类特征：生成用于低基数特征的单热编码，以及用于高基数分类特征的单热哈希编码。

1. 转换和编码

    具有极少唯一值的文本特征转换为分类特征。 根据分类特征的基数，执行单热编码或单热哈希编码。

## <a name="exit-criteria"></a>退出条件

定义完成任务的条件：

1. 一段时间后退出 - 通过在试验设置中使用 `MaxExperimentTimeInSeconds`，可以定义任务应持续运行的时长（以秒为单位）。

1. 收到取消令牌时退出 - 可以使用取消令牌，以便在任务的计划完成时间之前取消该任务。

    ```csharp
    var cts = new CancellationTokenSource();
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    experimentSettings.CancellationToken = cts.Token;
    ```

## <a name="create-an-experiment"></a>创建试验

配置试验设置后，即可创建试验。

```csharp
RegressionExperiment experiment = mlContext.Auto().CreateRegressionExperiment(experimentSettings);
```

## <a name="run-the-experiment"></a>运行试验

运行试验会触发数据预处理、学习算法选择和超参数调整。 AutoML 将继续生成特征化、学习算法和超参数的组合，直到达到 `MaxExperimentTimeInSeconds` 或试验终止。

```csharp
ExperimentResult<RegressionMetrics> experimentResult = experiment
    .Execute(trainingDataView, LabelColumnName, progressHandler: progressHandler);
```

如果想要传入验证数据、指示列目的的列信息或特征预提取器，请探索 `Execute()` 的其他重载。

## <a name="training-modes"></a>训练模式

### <a name="training-dataset"></a>训练数据集

AutoML 提供重载试验执行方法，该方法允许提供训练数据。 在内部，自动化 ML 将数据划分为训练-验证拆分。

```csharp
experiment.Execute(trainDataView);
```

### <a name="custom-validation-dataset"></a>自定义验证数据集

如果随机拆分不可接受（时序数据通常如此），则使用自定义验证数据集。 可以指定自己的验证数据集。 将根据指定的验证数据集而不是一个或多个随机数据集评估模型。

```csharp
experiment.Execute(trainDataView, validationDataView);
```

## <a name="explore-model-metrics"></a>探索模型指标

在 ML 试验的每次迭代后，系统会存储与该任务相关的指标。

例如，我们可以从最佳运行中访问验证指标：

```csharp
RegressionMetrics metrics = experimentResult.BestRun.ValidationMetrics;
Console.WriteLine($"R-Squared: {metrics.RSquared:0.##}");
Console.WriteLine($"Root Mean Squared Error: {metrics.RootMeanSquaredError:0.##}");
```

以下是每个 ML 任务的所有可用指标：

* [二元分类指标](xref:Microsoft.ML.AutoML.BinaryClassificationMetric)
* [多类分类指标](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric)
* [回归和建议指标](xref:Microsoft.ML.AutoML.RegressionMetric)

## <a name="see-also"></a>另请参阅

有关完整的代码示例等，请访问 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main#automate-mlnet-models-generation-preview-state) GitHub 存储库。
