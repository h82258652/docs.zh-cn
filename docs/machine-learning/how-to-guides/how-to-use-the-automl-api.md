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
# <a name="how-to-use-the-mlnet-automated-machine-learning-api"></a><span data-ttu-id="cbb40-104">如何使用 ML.NET 自动化机器学习 API</span><span class="sxs-lookup"><span data-stu-id="cbb40-104">How to use the ML.NET automated machine learning API</span></span>

<span data-ttu-id="cbb40-105">自动化机器学习 (AutoML) 可自动化将机器学习应用于数据的过程。</span><span class="sxs-lookup"><span data-stu-id="cbb40-105">Automated machine learning (AutoML) automates the process of applying machine learning to data.</span></span> <span data-ttu-id="cbb40-106">在给定数据集的情况下，可以运行 AutoML **试验** 来迭代不同的数据特征、机器学习算法和超参数，从而选择最佳模型。</span><span class="sxs-lookup"><span data-stu-id="cbb40-106">Given a dataset, you can run an AutoML **experiment** to iterate over different data featurizations, machine learning algorithms, and hyperparameters to select the best model.</span></span>

> [!NOTE]
> <span data-ttu-id="cbb40-107">本主题涉及 ML.NET 目前处于预览状态的自动化机器学习 API。</span><span class="sxs-lookup"><span data-stu-id="cbb40-107">This topic refers to the automated machine learning API for ML.NET, which is currently in preview.</span></span> <span data-ttu-id="cbb40-108">材料可能会有所变化。</span><span class="sxs-lookup"><span data-stu-id="cbb40-108">Material may be subject to change.</span></span>

## <a name="load-data"></a><span data-ttu-id="cbb40-109">加载数据</span><span class="sxs-lookup"><span data-stu-id="cbb40-109">Load data</span></span>

<span data-ttu-id="cbb40-110">自动化机器学习支持将数据集加载到 [IDataView](xref:Microsoft.ML.IDataView) 中。</span><span class="sxs-lookup"><span data-stu-id="cbb40-110">Automated machine learning supports loading a dataset into an [IDataView](xref:Microsoft.ML.IDataView).</span></span> <span data-ttu-id="cbb40-111">数据可以采用制表符分隔值 (TSV) 文件和逗号分隔值 (CSV) 文件的形式。</span><span class="sxs-lookup"><span data-stu-id="cbb40-111">Data can be in the form of tab-separated value (TSV) files and comma separated value (CSV) files.</span></span>

<span data-ttu-id="cbb40-112">示例：</span><span class="sxs-lookup"><span data-stu-id="cbb40-112">Example:</span></span>

```csharp
using Microsoft.ML;
using Microsoft.ML.AutoML;
    // ...
    MLContext mlContext = new MLContext();
    IDataView trainDataView = mlContext.Data.LoadFromTextFile<SentimentIssue>("my-data-file.csv", hasHeader: true);
```

## <a name="select-the-machine-learning-task-type"></a><span data-ttu-id="cbb40-113">选择机器学习任务类型</span><span class="sxs-lookup"><span data-stu-id="cbb40-113">Select the machine learning task type</span></span>

<span data-ttu-id="cbb40-114">在创建试验之前，请确定想要解决的机器学习问题的类型。</span><span class="sxs-lookup"><span data-stu-id="cbb40-114">Before creating an experiment, determine the kind of machine learning problem you want to solve.</span></span> <span data-ttu-id="cbb40-115">自动化机器学习支持以下 ML 任务：</span><span class="sxs-lookup"><span data-stu-id="cbb40-115">Automated machine learning supports the following ML tasks:</span></span>

* <span data-ttu-id="cbb40-116">二元分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-116">Binary Classification</span></span>
* <span data-ttu-id="cbb40-117">多类分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-117">Multiclass Classification</span></span>
* <span data-ttu-id="cbb40-118">回归测试</span><span class="sxs-lookup"><span data-stu-id="cbb40-118">Regression</span></span>
* <span data-ttu-id="cbb40-119">建议</span><span class="sxs-lookup"><span data-stu-id="cbb40-119">Recommendation</span></span>

## <a name="create-experiment-settings"></a><span data-ttu-id="cbb40-120">创建试验设置</span><span class="sxs-lookup"><span data-stu-id="cbb40-120">Create experiment settings</span></span>

<span data-ttu-id="cbb40-121">为确定的 ML 任务类型创建试验设置：</span><span class="sxs-lookup"><span data-stu-id="cbb40-121">Create experiment settings for the determined ML task type:</span></span>

* <span data-ttu-id="cbb40-122">二元分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-122">Binary Classification</span></span>

  ```csharp
  var experimentSettings = new BinaryExperimentSettings();
  ```

* <span data-ttu-id="cbb40-123">多类分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-123">Multiclass Classification</span></span>

  ```csharp
  var experimentSettings = new MulticlassExperimentSettings();
  ```

* <span data-ttu-id="cbb40-124">回归测试</span><span class="sxs-lookup"><span data-stu-id="cbb40-124">Regression</span></span>

  ```csharp
  var experimentSettings = new RegressionExperimentSettings();
  ```

* <span data-ttu-id="cbb40-125">建议</span><span class="sxs-lookup"><span data-stu-id="cbb40-125">Recommendation</span></span>

  ```csharp
  var experimentSettings = new RecommendationExperimentSettings();
  ```

## <a name="configure-experiment-settings"></a><span data-ttu-id="cbb40-126">配置试验设置</span><span class="sxs-lookup"><span data-stu-id="cbb40-126">Configure experiment settings</span></span>

<span data-ttu-id="cbb40-127">试验具有高度可配置性。</span><span class="sxs-lookup"><span data-stu-id="cbb40-127">Experiments are highly configurable.</span></span> <span data-ttu-id="cbb40-128">有关配置设置的完整列表，请参阅 [AutoML API 文档](/dotnet/api/microsoft.ml.automl?view=ml-dotnet-preview)。</span><span class="sxs-lookup"><span data-stu-id="cbb40-128">See the [AutoML API docs](/dotnet/api/microsoft.ml.automl?view=ml-dotnet-preview) for a full list of configuration settings.</span></span>

<span data-ttu-id="cbb40-129">示例包括：</span><span class="sxs-lookup"><span data-stu-id="cbb40-129">Some examples include:</span></span>

1. <span data-ttu-id="cbb40-130">指定试验的最长允许运行时间。</span><span class="sxs-lookup"><span data-stu-id="cbb40-130">Specify the maximum time that the experiment is allowed to run.</span></span>

    ```csharp
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    ```

1. <span data-ttu-id="cbb40-131">在试验的计划完成时间之前，使用取消令牌取消试验。</span><span class="sxs-lookup"><span data-stu-id="cbb40-131">Use a cancellation token to cancel the experiment before it is scheduled to finish.</span></span>

    ```csharp
    experimentSettings.CancellationToken = cts.Token;

    // Cancel experiment after the user presses any key
    CancelExperimentAfterAnyKeyPress(cts);
    ```

1. <span data-ttu-id="cbb40-132">指定其他优化指标。</span><span class="sxs-lookup"><span data-stu-id="cbb40-132">Specify a different optimizing metric.</span></span>

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.OptimizingMetric = RegressionMetric.MeanSquaredError;
    ```

1. <span data-ttu-id="cbb40-133">`CacheDirectory` 设置是指向目录的指针，执行 AutoML 任务期间训练的所有模型都将保存在该目录中。</span><span class="sxs-lookup"><span data-stu-id="cbb40-133">The `CacheDirectory` setting is a pointer to a directory where all models trained during the AutoML task will be saved.</span></span> <span data-ttu-id="cbb40-134">如果 `CacheDirectory` 设置为 null，则模型将保留在内存中，而不是写入磁盘。</span><span class="sxs-lookup"><span data-stu-id="cbb40-134">If `CacheDirectory` is set to null, models will be kept in memory instead of written to disk.</span></span>

    ```csharp
    experimentSettings.CacheDirectory = null;
    ```

1. <span data-ttu-id="cbb40-135">指示自动化 ML 不要使用特定训练程序。</span><span class="sxs-lookup"><span data-stu-id="cbb40-135">Instruct automated ML not to use certain trainers.</span></span>

    <span data-ttu-id="cbb40-136">将根据每个任务探索待优化训练程序的默认列表。</span><span class="sxs-lookup"><span data-stu-id="cbb40-136">A default list of trainers to optimize are explored per task.</span></span> <span data-ttu-id="cbb40-137">此列表可以针对每个试验进行修改。</span><span class="sxs-lookup"><span data-stu-id="cbb40-137">This list can be modified for each experiment.</span></span> <span data-ttu-id="cbb40-138">例如，可以从列表中删除在数据集上运行缓慢的训练程序。</span><span class="sxs-lookup"><span data-stu-id="cbb40-138">For instance, trainers that run slowly on your dataset can be removed from the list.</span></span> <span data-ttu-id="cbb40-139">若要优化一个特定的训练程序，请调用 `experimentSettings.Trainers.Clear()`，然后添加想要使用的训练程序。</span><span class="sxs-lookup"><span data-stu-id="cbb40-139">To optimize on one specific trainer call `experimentSettings.Trainers.Clear()`, then add the trainer that you want to use.</span></span>

    ```csharp
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.Trainers.Remove(RegressionTrainer.LbfgsPoissonRegression);
    experimentSettings.Trainers.Remove(RegressionTrainer.OnlineGradientDescent);
    ```

<span data-ttu-id="cbb40-140">每个 ML 任务支持的训练程序的列表可在以下相应链接中找到：</span><span class="sxs-lookup"><span data-stu-id="cbb40-140">The list of supported trainers per ML task can be found at the corresponding link below:</span></span>

* [<span data-ttu-id="cbb40-141">支持的二元分类算法</span><span class="sxs-lookup"><span data-stu-id="cbb40-141">Supported Binary Classification Algorithms</span></span>](xref:Microsoft.ML.AutoML.BinaryClassificationTrainer)
* [<span data-ttu-id="cbb40-142">支持的多类分类算法</span><span class="sxs-lookup"><span data-stu-id="cbb40-142">Supported Multiclass Classification Algorithms</span></span>](xref:Microsoft.ML.AutoML.MulticlassClassificationTrainer)
* [<span data-ttu-id="cbb40-143">支持的回归算法</span><span class="sxs-lookup"><span data-stu-id="cbb40-143">Supported Regression Algorithms</span></span>](xref:Microsoft.ML.AutoML.RegressionTrainer)
* [<span data-ttu-id="cbb40-144">支持的推荐算法</span><span class="sxs-lookup"><span data-stu-id="cbb40-144">Supported Recommendation Algorithms</span></span>](xref:Microsoft.ML.AutoML.RecommendationTrainer)

## <a name="optimizing-metric"></a><span data-ttu-id="cbb40-145">优化指标</span><span class="sxs-lookup"><span data-stu-id="cbb40-145">Optimizing metric</span></span>

<span data-ttu-id="cbb40-146">如以上示例所示，优化指标可确定在模型训练期间要优化的指标。</span><span class="sxs-lookup"><span data-stu-id="cbb40-146">The optimizing metric, as shown in the example above, determines the metric to be optimized during model training.</span></span> <span data-ttu-id="cbb40-147">可以选择的优化指标由选择的任务类型决定。</span><span class="sxs-lookup"><span data-stu-id="cbb40-147">The optimizing metric you can select is determined by the task type you choose.</span></span> <span data-ttu-id="cbb40-148">以下是可用指标的列表。</span><span class="sxs-lookup"><span data-stu-id="cbb40-148">Below is a list of available metrics.</span></span>

|[<span data-ttu-id="cbb40-149">二元分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-149">Binary Classification</span></span>](xref:Microsoft.ML.AutoML.BinaryClassificationMetric) | [<span data-ttu-id="cbb40-150">多类分类</span><span class="sxs-lookup"><span data-stu-id="cbb40-150">Multiclass Classification</span></span>](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric) |[<span data-ttu-id="cbb40-151">回归和建议</span><span class="sxs-lookup"><span data-stu-id="cbb40-151">Regression & Recommendation</span></span>](xref:Microsoft.ML.AutoML.RegressionMetric)
|-- |-- |--
|<span data-ttu-id="cbb40-152">精确度</span><span class="sxs-lookup"><span data-stu-id="cbb40-152">Accuracy</span></span>| <span data-ttu-id="cbb40-153">LogLoss</span><span class="sxs-lookup"><span data-stu-id="cbb40-153">LogLoss</span></span> | <span data-ttu-id="cbb40-154">RSquared</span><span class="sxs-lookup"><span data-stu-id="cbb40-154">RSquared</span></span>
|<span data-ttu-id="cbb40-155">AreaUnderPrecisionRecallCurve</span><span class="sxs-lookup"><span data-stu-id="cbb40-155">AreaUnderPrecisionRecallCurve</span></span> | <span data-ttu-id="cbb40-156">LogLossReduction</span><span class="sxs-lookup"><span data-stu-id="cbb40-156">LogLossReduction</span></span> | <span data-ttu-id="cbb40-157">MeanAbsoluteError</span><span class="sxs-lookup"><span data-stu-id="cbb40-157">MeanAbsoluteError</span></span>
|<span data-ttu-id="cbb40-158">AreaUnderRocCurve</span><span class="sxs-lookup"><span data-stu-id="cbb40-158">AreaUnderRocCurve</span></span> | <span data-ttu-id="cbb40-159">MacroAccuracy</span><span class="sxs-lookup"><span data-stu-id="cbb40-159">MacroAccuracy</span></span> | <span data-ttu-id="cbb40-160">MeanSquaredError</span><span class="sxs-lookup"><span data-stu-id="cbb40-160">MeanSquaredError</span></span>
|<span data-ttu-id="cbb40-161">F1Score</span><span class="sxs-lookup"><span data-stu-id="cbb40-161">F1Score</span></span> | <span data-ttu-id="cbb40-162">MicroAccuracy</span><span class="sxs-lookup"><span data-stu-id="cbb40-162">MicroAccuracy</span></span> | <span data-ttu-id="cbb40-163">RootMeanSquaredError</span><span class="sxs-lookup"><span data-stu-id="cbb40-163">RootMeanSquaredError</span></span>
|<span data-ttu-id="cbb40-164">NegativePrecision</span><span class="sxs-lookup"><span data-stu-id="cbb40-164">NegativePrecision</span></span> | <span data-ttu-id="cbb40-165">TopKAccuracy</span><span class="sxs-lookup"><span data-stu-id="cbb40-165">TopKAccuracy</span></span>
|<span data-ttu-id="cbb40-166">NegativeRecall</span><span class="sxs-lookup"><span data-stu-id="cbb40-166">NegativeRecall</span></span> |
|<span data-ttu-id="cbb40-167">PositivePrecision</span><span class="sxs-lookup"><span data-stu-id="cbb40-167">PositivePrecision</span></span>
|<span data-ttu-id="cbb40-168">PositiveRecall</span><span class="sxs-lookup"><span data-stu-id="cbb40-168">PositiveRecall</span></span>

## <a name="data-pre-processing-and-featurization"></a><span data-ttu-id="cbb40-169">数据预处理和特征化</span><span class="sxs-lookup"><span data-stu-id="cbb40-169">Data pre-processing and featurization</span></span>

> [!NOTE]
> <span data-ttu-id="cbb40-170">特征列仅支持 <xref:System.Boolean>、<xref:System.Single> 和 <xref:System.String> 类型。</span><span class="sxs-lookup"><span data-stu-id="cbb40-170">The feature column only supported types of <xref:System.Boolean>, <xref:System.Single>, and <xref:System.String>.</span></span>

<span data-ttu-id="cbb40-171">默认情况下会进行数据预处理，并会自动执行以下步骤：</span><span class="sxs-lookup"><span data-stu-id="cbb40-171">Data pre-processing happens by default and the following steps are performed automatically for you:</span></span>

1. <span data-ttu-id="cbb40-172">删除没有有用信息的特征</span><span class="sxs-lookup"><span data-stu-id="cbb40-172">Drop features with no useful information</span></span>

    <span data-ttu-id="cbb40-173">从训练和验证集中删除不包含任何有用信息的特征。</span><span class="sxs-lookup"><span data-stu-id="cbb40-173">Drop features with no useful information from training and validation sets.</span></span> <span data-ttu-id="cbb40-174">这包括缺少所有值的特征、在所有行中使用相同值的特征，或者包含极高基数（例如哈希、ID 或 GUID）的特征。</span><span class="sxs-lookup"><span data-stu-id="cbb40-174">These include features with all values missing, same value across all rows or with extremely high cardinality (e.g., hashes, IDs or GUIDs).</span></span>

1. <span data-ttu-id="cbb40-175">缺少值指示和插补</span><span class="sxs-lookup"><span data-stu-id="cbb40-175">Missing value indication and imputation</span></span>

    <span data-ttu-id="cbb40-176">使用数据类型的默认值填充缺少值的单元格。</span><span class="sxs-lookup"><span data-stu-id="cbb40-176">Fill missing value cells with the default value for the datatype.</span></span> <span data-ttu-id="cbb40-177">使用与输入列数量相同的插槽数追加指示特征。</span><span class="sxs-lookup"><span data-stu-id="cbb40-177">Append indicator features with the same number of slots as the input column.</span></span> <span data-ttu-id="cbb40-178">如果输入列中缺少值，则追加的指示特征中的值为 `1`，否则为 `0`。</span><span class="sxs-lookup"><span data-stu-id="cbb40-178">The value in the appended indicator features is `1` if the value in the input column is missing and `0` otherwise.</span></span>

1. <span data-ttu-id="cbb40-179">生成其他特征</span><span class="sxs-lookup"><span data-stu-id="cbb40-179">Generate additional features</span></span>

    <span data-ttu-id="cbb40-180">对于文本特征：生成使用一元语法和三元语法的词袋特征。</span><span class="sxs-lookup"><span data-stu-id="cbb40-180">For text features: Bag-of-word features using unigrams and tri-character-grams.</span></span>

    <span data-ttu-id="cbb40-181">对于分类特征：生成用于低基数特征的单热编码，以及用于高基数分类特征的单热哈希编码。</span><span class="sxs-lookup"><span data-stu-id="cbb40-181">For categorical features: One-hot encoding for low cardinality features, and one-hot-hash encoding for high cardinality categorical features.</span></span>

1. <span data-ttu-id="cbb40-182">转换和编码</span><span class="sxs-lookup"><span data-stu-id="cbb40-182">Transformations and encodings</span></span>

    <span data-ttu-id="cbb40-183">具有极少唯一值的文本特征转换为分类特征。</span><span class="sxs-lookup"><span data-stu-id="cbb40-183">Text features with very few unique values transformed into categorical features.</span></span> <span data-ttu-id="cbb40-184">根据分类特征的基数，执行单热编码或单热哈希编码。</span><span class="sxs-lookup"><span data-stu-id="cbb40-184">Depending on cardinality of categorical features, perform one-hot encoding or one-hot hash encoding.</span></span>

## <a name="exit-criteria"></a><span data-ttu-id="cbb40-185">退出条件</span><span class="sxs-lookup"><span data-stu-id="cbb40-185">Exit criteria</span></span>

<span data-ttu-id="cbb40-186">定义完成任务的条件：</span><span class="sxs-lookup"><span data-stu-id="cbb40-186">Define the criteria to complete your task:</span></span>

1. <span data-ttu-id="cbb40-187">一段时间后退出 - 通过在试验设置中使用 `MaxExperimentTimeInSeconds`，可以定义任务应持续运行的时长（以秒为单位）。</span><span class="sxs-lookup"><span data-stu-id="cbb40-187">Exit after a length of time - Using `MaxExperimentTimeInSeconds` in your experiment settings you can define how long in seconds that an task should continue to run.</span></span>

1. <span data-ttu-id="cbb40-188">收到取消令牌时退出 - 可以使用取消令牌，以便在任务的计划完成时间之前取消该任务。</span><span class="sxs-lookup"><span data-stu-id="cbb40-188">Exit on a cancellation token -  You can use a cancellation token that lets you cancel the task before it is scheduled to finish.</span></span>

    ```csharp
    var cts = new CancellationTokenSource();
    var experimentSettings = new RegressionExperimentSettings();
    experimentSettings.MaxExperimentTimeInSeconds = 3600;
    experimentSettings.CancellationToken = cts.Token;
    ```

## <a name="create-an-experiment"></a><span data-ttu-id="cbb40-189">创建试验</span><span class="sxs-lookup"><span data-stu-id="cbb40-189">Create an experiment</span></span>

<span data-ttu-id="cbb40-190">配置试验设置后，即可创建试验。</span><span class="sxs-lookup"><span data-stu-id="cbb40-190">Once you have configured the experiment settings, you are ready to create the experiment.</span></span>

```csharp
RegressionExperiment experiment = mlContext.Auto().CreateRegressionExperiment(experimentSettings);
```

## <a name="run-the-experiment"></a><span data-ttu-id="cbb40-191">运行试验</span><span class="sxs-lookup"><span data-stu-id="cbb40-191">Run the experiment</span></span>

<span data-ttu-id="cbb40-192">运行试验会触发数据预处理、学习算法选择和超参数调整。</span><span class="sxs-lookup"><span data-stu-id="cbb40-192">Running the experiment triggers data pre-processing, learning algorithm selection, and hyperparameter tuning.</span></span> <span data-ttu-id="cbb40-193">AutoML 将继续生成特征化、学习算法和超参数的组合，直到达到 `MaxExperimentTimeInSeconds` 或试验终止。</span><span class="sxs-lookup"><span data-stu-id="cbb40-193">AutoML will continue to generate combinations of featurization, learning algorithms, and hyperparameters until the `MaxExperimentTimeInSeconds` is reached or the experiment is terminated.</span></span>

```csharp
ExperimentResult<RegressionMetrics> experimentResult = experiment
    .Execute(trainingDataView, LabelColumnName, progressHandler: progressHandler);
```

<span data-ttu-id="cbb40-194">如果想要传入验证数据、指示列目的的列信息或特征预提取器，请探索 `Execute()` 的其他重载。</span><span class="sxs-lookup"><span data-stu-id="cbb40-194">Explore other overloads for `Execute()` if you want to pass in validation data, column information indicating the column purpose, or prefeaturizers.</span></span>

## <a name="training-modes"></a><span data-ttu-id="cbb40-195">训练模式</span><span class="sxs-lookup"><span data-stu-id="cbb40-195">Training modes</span></span>

### <a name="training-dataset"></a><span data-ttu-id="cbb40-196">训练数据集</span><span class="sxs-lookup"><span data-stu-id="cbb40-196">Training dataset</span></span>

<span data-ttu-id="cbb40-197">AutoML 提供重载试验执行方法，该方法允许提供训练数据。</span><span class="sxs-lookup"><span data-stu-id="cbb40-197">AutoML provides an overloaded experiment execute method which allows you to provide training data.</span></span> <span data-ttu-id="cbb40-198">在内部，自动化 ML 将数据划分为训练-验证拆分。</span><span class="sxs-lookup"><span data-stu-id="cbb40-198">Internally, automated ML divides the data into train-validate splits.</span></span>

```csharp
experiment.Execute(trainDataView);
```

### <a name="custom-validation-dataset"></a><span data-ttu-id="cbb40-199">自定义验证数据集</span><span class="sxs-lookup"><span data-stu-id="cbb40-199">Custom validation dataset</span></span>

<span data-ttu-id="cbb40-200">如果随机拆分不可接受（时序数据通常如此），则使用自定义验证数据集。</span><span class="sxs-lookup"><span data-stu-id="cbb40-200">Use custom validation dataset if random split is not acceptable, as is usually the case with time series data.</span></span> <span data-ttu-id="cbb40-201">可以指定自己的验证数据集。</span><span class="sxs-lookup"><span data-stu-id="cbb40-201">You can specify your own validation dataset.</span></span> <span data-ttu-id="cbb40-202">将根据指定的验证数据集而不是一个或多个随机数据集评估模型。</span><span class="sxs-lookup"><span data-stu-id="cbb40-202">The model will be evaluated against the validation dataset specified instead of one or more random datasets.</span></span>

```csharp
experiment.Execute(trainDataView, validationDataView);
```

## <a name="explore-model-metrics"></a><span data-ttu-id="cbb40-203">探索模型指标</span><span class="sxs-lookup"><span data-stu-id="cbb40-203">Explore model metrics</span></span>

<span data-ttu-id="cbb40-204">在 ML 试验的每次迭代后，系统会存储与该任务相关的指标。</span><span class="sxs-lookup"><span data-stu-id="cbb40-204">After each iteration of an ML experiment, metrics relating to that task are stored.</span></span>

<span data-ttu-id="cbb40-205">例如，我们可以从最佳运行中访问验证指标：</span><span class="sxs-lookup"><span data-stu-id="cbb40-205">For example, we can access validation metrics from the best run:</span></span>

```csharp
RegressionMetrics metrics = experimentResult.BestRun.ValidationMetrics;
Console.WriteLine($"R-Squared: {metrics.RSquared:0.##}");
Console.WriteLine($"Root Mean Squared Error: {metrics.RootMeanSquaredError:0.##}");
```

<span data-ttu-id="cbb40-206">以下是每个 ML 任务的所有可用指标：</span><span class="sxs-lookup"><span data-stu-id="cbb40-206">The following are all the available metrics per ML task:</span></span>

* [<span data-ttu-id="cbb40-207">二元分类指标</span><span class="sxs-lookup"><span data-stu-id="cbb40-207">Binary classification metrics</span></span>](xref:Microsoft.ML.AutoML.BinaryClassificationMetric)
* [<span data-ttu-id="cbb40-208">多类分类指标</span><span class="sxs-lookup"><span data-stu-id="cbb40-208">Multiclass classification metrics</span></span>](xref:Microsoft.ML.AutoML.MulticlassClassificationMetric)
* [<span data-ttu-id="cbb40-209">回归和建议指标</span><span class="sxs-lookup"><span data-stu-id="cbb40-209">Regression & recommendation metrics</span></span>](xref:Microsoft.ML.AutoML.RegressionMetric)

## <a name="see-also"></a><span data-ttu-id="cbb40-210">另请参阅</span><span class="sxs-lookup"><span data-stu-id="cbb40-210">See also</span></span>

<span data-ttu-id="cbb40-211">有关完整的代码示例等，请访问 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main#automate-mlnet-models-generation-preview-state) GitHub 存储库。</span><span class="sxs-lookup"><span data-stu-id="cbb40-211">For full code samples and more visit the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main#automate-mlnet-models-generation-preview-state) GitHub repository.</span></span>
