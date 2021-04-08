---
title: 如何选择 ML.NET 算法
description: 了解如何为机器学习模型选择 ML.NET 算法
ms.topic: overview
ms.date: 03/31/2021
ms.openlocfilehash: c1a35f2b5b2ece2a846469f855e91b49887f0c90
ms.sourcegitcommit: b5d2290673e1c91260c9205202dd8b95fbab1a0b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/01/2021
ms.locfileid: "106122620"
---
# <a name="how-to-choose-an-mlnet-algorithm"></a>如何选择 ML.NET 算法

对于每个 [ML.NET 任务](resources/tasks.md)，有多种训练算法可供选择。 选择哪个算法取决于尝试解决的问题、数据的特征以及可用的计算和存储资源。 值得注意的是，训练机器学习模型是一个迭代过程。 可能需要尝试多种算法才能找到效果最好的算法。

算法在 **特征** 上运行。 特征是根据输入数据进行计算的数字值。 它们是机器学习算法的最佳输入。 可以使用一个或多个[数据转换](resources/transforms.md)将原始输入数据转换为特征。 例如，文本数据被转换为一组字词计数和字词组合计数。 使用数据转换从原始数据类型中提取特征后，它们被称为 **特征化**。 例如，特征化文本或特征化图像数据。

## <a name="trainer--algorithm--task"></a>训练程序 = 算法 + 任务

算法是执行后可生成 **模型** 的数学运算。 不同的算法生成具有不同特征的模型。

借助 ML.NET，同一算法可以应用于不同的任务。 例如，随机双坐标上升可用于二元分类、多类分类和回归。 区别在于如何解释算法的输出来匹配任务。

对于每种算法/任务组合，ML.NET 都提供用于执行训练算法并进行解释的组件。 这些组件称为训练程序。 例如，<xref:Microsoft.ML.Trainers.SdcaRegressionTrainer> 使用应用于 **回归** 任务的 **StochasticDualCoordinatedAscent** 算法。

## <a name="linear-algorithms"></a>线性算法

线性算法生成一个模型，该模型根据输入数据和一组 **权重** 的线性组合计算 **分数**。 权重是训练期间估算的模型参数。

线性算法适用于[线性可分](https://en.wikipedia.org/wiki/Linear_separability)的特征。

在使用线性算法进行训练之前，应对特征进行规范化。 这样可防止某个特征对结果产生比其他特征更多的影响。

一般而言，线性算法可缩放且速度快，训练和预测费用也很低。 它们按特征数量进行缩放，并按训练数据集的大小粗略进行缩放。

线性算法对训练数据进行多次传递。 如果数据集适用于内存，则在追加训练程序之前向 ML.NET 管道添加[缓存检查点](xref:Microsoft.ML.LearningPipelineExtensions.AppendCacheCheckpoint%2A)将使训练运行速度加快。

### <a name="averaged-perceptron"></a>平均感知器

最适合用于文本分类。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>|二元分类|是|

### <a name="stochastic-dual-coordinated-ascent"></a>随机双坐标上升

默认性能良好，不需要调整。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>|多类分类|是|
|<xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>|多类分类|是|
|<xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>|回归|是|

### <a name="l-bfgs"></a>L-BFGS

在有大量特征时使用。 生成逻辑回归训练统计数据，但缩放性能不如 AveragedPerceptronTrainer。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>|多类分类|是|
|<xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>|回归|是|

### <a name="symbolic-stochastic-gradient-descent"></a>符号随机梯度下降

最快速、最准确的线性二元分类训练程序。 可随处理器数量很好地缩放。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>|二元分类|是|

### <a name="online-gradient-descent"></a>在线梯度下降

实现标准（非批处理）随机梯度下降，其中可以选择损失函数，还可以选择使用一段时间内的可视向量平均值来更新权重向量。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>|回归|是|

## <a name="decision-tree-algorithms"></a>决策树算法

决策树算法创建包含一系列决策的模型：实际上是包含数据值的流程图。

特征不需要线性可分即可使用此类算法。 特征无需规范化，因为特征向量中的各个值在决策过程中独立使用。

决策树算法通常非常准确。

除广义加性模型 (GAM) 之外，在存在大量特征的情况下，树模型可能缺少可解释性。

决策树算法需要更多资源，并且缩放性能不如线性算法。 它们在适用于内存的数据集中拥有良好性能。

提升决策树是一组小型决策树，其中每个决策树对输入数据进行评分并将分数传递到下一个决策树来生成更好的分数，以此类推，其中每个决策树都会在之前决策树的基础上有所改进。

### <a name="light-gradient-boosted-machine"></a>轻型梯度增强机

最快速、最准确的二元分类树训练程序。 高度可优化。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>|多类分类|是|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>|回归|是|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>|排名|否|

### <a name="fast-tree"></a>快速决策树

用于特征化图像数据。 在非均衡数据方面具有弹性。 高度可优化。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>|回归|是|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>|回归|是|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>|排名|否|

### <a name="fast-forest"></a>快速林

适用于干扰性数据。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>|二元分类|是|
|<xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>|回归|是|

### <a name="generalized-additive-model-gam"></a>广义加性模型 (GAM)

最适合用于使用决策树算法时表现良好但可解释性为优先事项的问题。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>|二元分类|否|
|<xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>|回归|否|

## <a name="matrix-factorization"></a>矩阵分解

### <a name="matrix-factorization"></a>矩阵分解

用于建议中的[协作筛选](https://en.wikipedia.org/wiki/Collaborative_filtering)。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>|建议|否|

### <a name="field-aware-factorization-machine"></a>场感知分解机

 最适合用于具有大型数据集的稀疏分类数据。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>|二元分类|否|

## <a name="meta-algorithms"></a>元算法

这些训练程序根据二元训练程序创建多类训练程序。 与 <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>、<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer> 配合使用。

### <a name="one-versus-all"></a>一对多

此多类分类器为每个类训练一个二元分类器，这可将该类与所有其他类区分开来。 规模因要分类的类的数量而受到限制。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OneVersusAllTrainer>|多类分类|是|

### <a name="pairwise-coupling"></a>成对耦合

此多类分类器在每对类上训练二元分类算法。 规模因类的数量而受到限制，因为必须训练每个两个类的组合。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>|多类分类|否|

## <a name="k-means"></a>K-Means

用于聚类分析。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.KMeansTrainer>|群集|是|

## <a name="principal-component-analysis"></a>主体组件分析

用于异常情况检测。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>|异常检测|否|

## <a name="naive-bayes"></a>Naive Bayes

当特征独立且训练数据集很小时，请使用此多类分类算法。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>|多类分类|是|

## <a name="prior-trainer"></a>前期训练程序

使用此二元分类算法来确定其他训练程序的性能基线。 其他训练程序的指标应优于前期训练程序才能成为有效指标。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.PriorTrainer>|二元分类|是|

## <a name="support-vector-machines"></a>支持向量机

支持向量机 (SVM) 是极其流行的、经过广泛研究的监管学习模型种类，可在线性和非线性分类任务中使用。

最近的研究重点探究了如何优化这些模型，以便将其有效扩展为更大型的定型集。

### <a name="linear-svm"></a>线性 SVM

使用通过布尔标记的数据训练的线性二元分类模型预测目标。 交替使用随机梯度下降步骤和投影步骤。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LinearSvmTrainer>|二元分类|是|

### <a name="local-deep-svm"></a>本地深度 SVM

使用非线性二元分类模型预测目标。 缩减预测时间成本；预测成本随训练集的大小呈对数而非线性增长，可接受在分类准确性方面出现损失。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LdSvmTrainer>|二元分类|是|

## <a name="ordinary-least-squares"></a>普通最小二乘法

“普通最小二乘法 (OLS)”是线性回归中最常用的方法之一。

普通最小二乘法是指损失函数，该函数将误差计算为实际值与预测线之间差值的平方和，通过最小化平方误差来拟合模型。 此方法假设输入与因变量之间存在较强的线性关系。

|培训师|任务|ONNX 可导出|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OlsTrainer>|回归|是|
