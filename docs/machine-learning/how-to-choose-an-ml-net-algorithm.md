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
# <a name="how-to-choose-an-mlnet-algorithm"></a><span data-ttu-id="c22dd-103">如何选择 ML.NET 算法</span><span class="sxs-lookup"><span data-stu-id="c22dd-103">How to choose an ML.NET algorithm</span></span>

<span data-ttu-id="c22dd-104">对于每个 [ML.NET 任务](resources/tasks.md)，有多种训练算法可供选择。</span><span class="sxs-lookup"><span data-stu-id="c22dd-104">For each [ML.NET task](resources/tasks.md), there are multiple training algorithms to choose from.</span></span> <span data-ttu-id="c22dd-105">选择哪个算法取决于尝试解决的问题、数据的特征以及可用的计算和存储资源。</span><span class="sxs-lookup"><span data-stu-id="c22dd-105">Which one to choose depends on the problem you are trying to solve, the characteristics of your data, and the compute and storage resources you have available.</span></span> <span data-ttu-id="c22dd-106">值得注意的是，训练机器学习模型是一个迭代过程。</span><span class="sxs-lookup"><span data-stu-id="c22dd-106">It is important to note that training a machine learning model is an iterative process.</span></span> <span data-ttu-id="c22dd-107">可能需要尝试多种算法才能找到效果最好的算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-107">You might need to try multiple algorithms to find the one that works best.</span></span>

<span data-ttu-id="c22dd-108">算法在 **特征** 上运行。</span><span class="sxs-lookup"><span data-stu-id="c22dd-108">Algorithms operate on **features**.</span></span> <span data-ttu-id="c22dd-109">特征是根据输入数据进行计算的数字值。</span><span class="sxs-lookup"><span data-stu-id="c22dd-109">Features are numerical values computed from your input data.</span></span> <span data-ttu-id="c22dd-110">它们是机器学习算法的最佳输入。</span><span class="sxs-lookup"><span data-stu-id="c22dd-110">They are optimal inputs for machine learning algorithms.</span></span> <span data-ttu-id="c22dd-111">可以使用一个或多个[数据转换](resources/transforms.md)将原始输入数据转换为特征。</span><span class="sxs-lookup"><span data-stu-id="c22dd-111">You transform your raw input data into features using one or more [data transforms](resources/transforms.md).</span></span> <span data-ttu-id="c22dd-112">例如，文本数据被转换为一组字词计数和字词组合计数。</span><span class="sxs-lookup"><span data-stu-id="c22dd-112">For example, text data is transformed into a set of word counts and word combination counts.</span></span> <span data-ttu-id="c22dd-113">使用数据转换从原始数据类型中提取特征后，它们被称为 **特征化**。</span><span class="sxs-lookup"><span data-stu-id="c22dd-113">Once the features have been extracted from a raw data type using data transforms, they are referred to as **featurized**.</span></span> <span data-ttu-id="c22dd-114">例如，特征化文本或特征化图像数据。</span><span class="sxs-lookup"><span data-stu-id="c22dd-114">For example, featurized text, or featurized image data.</span></span>

## <a name="trainer--algorithm--task"></a><span data-ttu-id="c22dd-115">训练程序 = 算法 + 任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-115">Trainer = Algorithm + Task</span></span>

<span data-ttu-id="c22dd-116">算法是执行后可生成 **模型** 的数学运算。</span><span class="sxs-lookup"><span data-stu-id="c22dd-116">An algorithm is the math that executes to produce a **model**.</span></span> <span data-ttu-id="c22dd-117">不同的算法生成具有不同特征的模型。</span><span class="sxs-lookup"><span data-stu-id="c22dd-117">Different algorithms produce models with different characteristics.</span></span>

<span data-ttu-id="c22dd-118">借助 ML.NET，同一算法可以应用于不同的任务。</span><span class="sxs-lookup"><span data-stu-id="c22dd-118">With ML.NET, the same algorithm can be applied to different tasks.</span></span> <span data-ttu-id="c22dd-119">例如，随机双坐标上升可用于二元分类、多类分类和回归。</span><span class="sxs-lookup"><span data-stu-id="c22dd-119">For example, Stochastic Dual Coordinate Ascent can be used for Binary Classification, Multiclass Classification, and Regression.</span></span> <span data-ttu-id="c22dd-120">区别在于如何解释算法的输出来匹配任务。</span><span class="sxs-lookup"><span data-stu-id="c22dd-120">The difference is in how the output of the algorithm is interpreted to match the task.</span></span>

<span data-ttu-id="c22dd-121">对于每种算法/任务组合，ML.NET 都提供用于执行训练算法并进行解释的组件。</span><span class="sxs-lookup"><span data-stu-id="c22dd-121">For each algorithm/task combination, ML.NET provides a component that executes the training algorithm and makes the interpretation.</span></span> <span data-ttu-id="c22dd-122">这些组件称为训练程序。</span><span class="sxs-lookup"><span data-stu-id="c22dd-122">These components are called trainers.</span></span> <span data-ttu-id="c22dd-123">例如，<xref:Microsoft.ML.Trainers.SdcaRegressionTrainer> 使用应用于 **回归** 任务的 **StochasticDualCoordinatedAscent** 算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-123">For example, the <xref:Microsoft.ML.Trainers.SdcaRegressionTrainer> uses the **StochasticDualCoordinatedAscent** algorithm applied to the **Regression** task.</span></span>

## <a name="linear-algorithms"></a><span data-ttu-id="c22dd-124">线性算法</span><span class="sxs-lookup"><span data-stu-id="c22dd-124">Linear algorithms</span></span>

<span data-ttu-id="c22dd-125">线性算法生成一个模型，该模型根据输入数据和一组 **权重** 的线性组合计算 **分数**。</span><span class="sxs-lookup"><span data-stu-id="c22dd-125">Linear algorithms produce a model that calculates **scores** from a linear combination of the input data and a set of **weights**.</span></span> <span data-ttu-id="c22dd-126">权重是训练期间估算的模型参数。</span><span class="sxs-lookup"><span data-stu-id="c22dd-126">The weights are parameters of the model estimated during training.</span></span>

<span data-ttu-id="c22dd-127">线性算法适用于[线性可分](https://en.wikipedia.org/wiki/Linear_separability)的特征。</span><span class="sxs-lookup"><span data-stu-id="c22dd-127">Linear algorithms work well for features that are [linearly separable](https://en.wikipedia.org/wiki/Linear_separability).</span></span>

<span data-ttu-id="c22dd-128">在使用线性算法进行训练之前，应对特征进行规范化。</span><span class="sxs-lookup"><span data-stu-id="c22dd-128">Before training with a linear algorithm, the features should be normalized.</span></span> <span data-ttu-id="c22dd-129">这样可防止某个特征对结果产生比其他特征更多的影响。</span><span class="sxs-lookup"><span data-stu-id="c22dd-129">This prevents one feature from having more influence over the result than others.</span></span>

<span data-ttu-id="c22dd-130">一般而言，线性算法可缩放且速度快，训练和预测费用也很低。</span><span class="sxs-lookup"><span data-stu-id="c22dd-130">In general, linear algorithms are scalable, fast, cheap to train, and cheap to predict.</span></span> <span data-ttu-id="c22dd-131">它们按特征数量进行缩放，并按训练数据集的大小粗略进行缩放。</span><span class="sxs-lookup"><span data-stu-id="c22dd-131">They scale by the number of features and approximately by the size of the training data set.</span></span>

<span data-ttu-id="c22dd-132">线性算法对训练数据进行多次传递。</span><span class="sxs-lookup"><span data-stu-id="c22dd-132">Linear algorithms make multiple passes over the training data.</span></span> <span data-ttu-id="c22dd-133">如果数据集适用于内存，则在追加训练程序之前向 ML.NET 管道添加[缓存检查点](xref:Microsoft.ML.LearningPipelineExtensions.AppendCacheCheckpoint%2A)将使训练运行速度加快。</span><span class="sxs-lookup"><span data-stu-id="c22dd-133">If your dataset fits into memory, then adding a [cache checkpoint](xref:Microsoft.ML.LearningPipelineExtensions.AppendCacheCheckpoint%2A) to your ML.NET pipeline before appending the trainer will make the training run faster.</span></span>

### <a name="averaged-perceptron"></a><span data-ttu-id="c22dd-134">平均感知器</span><span class="sxs-lookup"><span data-stu-id="c22dd-134">Averaged perceptron</span></span>

<span data-ttu-id="c22dd-135">最适合用于文本分类。</span><span class="sxs-lookup"><span data-stu-id="c22dd-135">Best for text classification.</span></span>

|<span data-ttu-id="c22dd-136">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-136">Trainer</span></span>|<span data-ttu-id="c22dd-137">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-137">Task</span></span>|<span data-ttu-id="c22dd-138">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-138">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>|<span data-ttu-id="c22dd-139">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-139">Binary classification</span></span>|<span data-ttu-id="c22dd-140">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-140">Yes</span></span>|

### <a name="stochastic-dual-coordinated-ascent"></a><span data-ttu-id="c22dd-141">随机双坐标上升</span><span class="sxs-lookup"><span data-stu-id="c22dd-141">Stochastic dual coordinated ascent</span></span>

<span data-ttu-id="c22dd-142">默认性能良好，不需要调整。</span><span class="sxs-lookup"><span data-stu-id="c22dd-142">Tuning not needed for good default performance.</span></span>

|<span data-ttu-id="c22dd-143">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-143">Trainer</span></span>|<span data-ttu-id="c22dd-144">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-144">Task</span></span>|<span data-ttu-id="c22dd-145">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-145">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.SdcaLogisticRegressionBinaryTrainer>|<span data-ttu-id="c22dd-146">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-146">Binary classification</span></span>|<span data-ttu-id="c22dd-147">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-147">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.SdcaNonCalibratedBinaryTrainer>|<span data-ttu-id="c22dd-148">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-148">Binary classification</span></span>|<span data-ttu-id="c22dd-149">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-149">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.SdcaMaximumEntropyMulticlassTrainer>|<span data-ttu-id="c22dd-150">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-150">Multiclass classification</span></span>|<span data-ttu-id="c22dd-151">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-151">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.SdcaNonCalibratedMulticlassTrainer>|<span data-ttu-id="c22dd-152">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-152">Multiclass classification</span></span>|<span data-ttu-id="c22dd-153">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-153">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.SdcaRegressionTrainer>|<span data-ttu-id="c22dd-154">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-154">Regression</span></span>|<span data-ttu-id="c22dd-155">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-155">Yes</span></span>|

### <a name="l-bfgs"></a><span data-ttu-id="c22dd-156">L-BFGS</span><span class="sxs-lookup"><span data-stu-id="c22dd-156">L-BFGS</span></span>

<span data-ttu-id="c22dd-157">在有大量特征时使用。</span><span class="sxs-lookup"><span data-stu-id="c22dd-157">Use when number of features is large.</span></span> <span data-ttu-id="c22dd-158">生成逻辑回归训练统计数据，但缩放性能不如 AveragedPerceptronTrainer。</span><span class="sxs-lookup"><span data-stu-id="c22dd-158">Produces logistic regression training statistics, but doesn't scale as well as the AveragedPerceptronTrainer.</span></span>

|<span data-ttu-id="c22dd-159">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-159">Trainer</span></span>|<span data-ttu-id="c22dd-160">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-160">Task</span></span>|<span data-ttu-id="c22dd-161">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-161">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>|<span data-ttu-id="c22dd-162">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-162">Binary classification</span></span>|<span data-ttu-id="c22dd-163">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-163">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.LbfgsMaximumEntropyMulticlassTrainer>|<span data-ttu-id="c22dd-164">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-164">Multiclass classification</span></span>|<span data-ttu-id="c22dd-165">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-165">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.LbfgsPoissonRegressionTrainer>|<span data-ttu-id="c22dd-166">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-166">Regression</span></span>|<span data-ttu-id="c22dd-167">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-167">Yes</span></span>|

### <a name="symbolic-stochastic-gradient-descent"></a><span data-ttu-id="c22dd-168">符号随机梯度下降</span><span class="sxs-lookup"><span data-stu-id="c22dd-168">Symbolic stochastic gradient descent</span></span>

<span data-ttu-id="c22dd-169">最快速、最准确的线性二元分类训练程序。</span><span class="sxs-lookup"><span data-stu-id="c22dd-169">Fastest and most accurate linear binary classification trainer.</span></span> <span data-ttu-id="c22dd-170">可随处理器数量很好地缩放。</span><span class="sxs-lookup"><span data-stu-id="c22dd-170">Scales well with number of processors.</span></span>

|<span data-ttu-id="c22dd-171">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-171">Trainer</span></span>|<span data-ttu-id="c22dd-172">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-172">Task</span></span>|<span data-ttu-id="c22dd-173">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-173">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>|<span data-ttu-id="c22dd-174">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-174">Binary classification</span></span>|<span data-ttu-id="c22dd-175">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-175">Yes</span></span>|

### <a name="online-gradient-descent"></a><span data-ttu-id="c22dd-176">在线梯度下降</span><span class="sxs-lookup"><span data-stu-id="c22dd-176">Online gradient descent</span></span>

<span data-ttu-id="c22dd-177">实现标准（非批处理）随机梯度下降，其中可以选择损失函数，还可以选择使用一段时间内的可视向量平均值来更新权重向量。</span><span class="sxs-lookup"><span data-stu-id="c22dd-177">Implements the standard (non-batch) stochastic gradient descent, with a choice of loss functions, and an option to update the weight vector using the average of the vectors seen over time.</span></span>

|<span data-ttu-id="c22dd-178">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-178">Trainer</span></span>|<span data-ttu-id="c22dd-179">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-179">Task</span></span>|<span data-ttu-id="c22dd-180">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-180">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OnlineGradientDescentTrainer>|<span data-ttu-id="c22dd-181">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-181">Regression</span></span>|<span data-ttu-id="c22dd-182">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-182">Yes</span></span>|

## <a name="decision-tree-algorithms"></a><span data-ttu-id="c22dd-183">决策树算法</span><span class="sxs-lookup"><span data-stu-id="c22dd-183">Decision tree algorithms</span></span>

<span data-ttu-id="c22dd-184">决策树算法创建包含一系列决策的模型：实际上是包含数据值的流程图。</span><span class="sxs-lookup"><span data-stu-id="c22dd-184">Decision tree algorithms create a model that contains a series of decisions: effectively a flow chart through the data values.</span></span>

<span data-ttu-id="c22dd-185">特征不需要线性可分即可使用此类算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-185">Features do not need to be linearly separable to use this type of algorithm.</span></span> <span data-ttu-id="c22dd-186">特征无需规范化，因为特征向量中的各个值在决策过程中独立使用。</span><span class="sxs-lookup"><span data-stu-id="c22dd-186">And features do not need to be normalized, because the individual values in the feature vector are used independently in the decision process.</span></span>

<span data-ttu-id="c22dd-187">决策树算法通常非常准确。</span><span class="sxs-lookup"><span data-stu-id="c22dd-187">Decision tree algorithms are generally very accurate.</span></span>

<span data-ttu-id="c22dd-188">除广义加性模型 (GAM) 之外，在存在大量特征的情况下，树模型可能缺少可解释性。</span><span class="sxs-lookup"><span data-stu-id="c22dd-188">Except for Generalized Additive Models (GAMs), tree models can lack explainability when the number of features is large.</span></span>

<span data-ttu-id="c22dd-189">决策树算法需要更多资源，并且缩放性能不如线性算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-189">Decision tree algorithms take more resources and do not scale as well as linear ones do.</span></span> <span data-ttu-id="c22dd-190">它们在适用于内存的数据集中拥有良好性能。</span><span class="sxs-lookup"><span data-stu-id="c22dd-190">They do perform well on datasets that can fit into memory.</span></span>

<span data-ttu-id="c22dd-191">提升决策树是一组小型决策树，其中每个决策树对输入数据进行评分并将分数传递到下一个决策树来生成更好的分数，以此类推，其中每个决策树都会在之前决策树的基础上有所改进。</span><span class="sxs-lookup"><span data-stu-id="c22dd-191">Boosted decision trees are an ensemble of small trees where each tree scores the input data and passes the score onto the next tree to produce a better score, and so on, where each tree in the ensemble improves on the previous.</span></span>

### <a name="light-gradient-boosted-machine"></a><span data-ttu-id="c22dd-192">轻型梯度增强机</span><span class="sxs-lookup"><span data-stu-id="c22dd-192">Light gradient boosted machine</span></span>

<span data-ttu-id="c22dd-193">最快速、最准确的二元分类树训练程序。</span><span class="sxs-lookup"><span data-stu-id="c22dd-193">Fastest and most accurate of the binary classification tree trainers.</span></span> <span data-ttu-id="c22dd-194">高度可优化。</span><span class="sxs-lookup"><span data-stu-id="c22dd-194">Highly tunable.</span></span>

|<span data-ttu-id="c22dd-195">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-195">Trainer</span></span>|<span data-ttu-id="c22dd-196">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-196">Task</span></span>|<span data-ttu-id="c22dd-197">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-197">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>|<span data-ttu-id="c22dd-198">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-198">Binary classification</span></span>|<span data-ttu-id="c22dd-199">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-199">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmMulticlassTrainer>|<span data-ttu-id="c22dd-200">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-200">Multiclass classification</span></span>|<span data-ttu-id="c22dd-201">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-201">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmRegressionTrainer>|<span data-ttu-id="c22dd-202">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-202">Regression</span></span>|<span data-ttu-id="c22dd-203">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-203">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.LightGbm.LightGbmRankingTrainer>|<span data-ttu-id="c22dd-204">排名</span><span class="sxs-lookup"><span data-stu-id="c22dd-204">Ranking</span></span>|<span data-ttu-id="c22dd-205">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-205">No</span></span>|

### <a name="fast-tree"></a><span data-ttu-id="c22dd-206">快速决策树</span><span class="sxs-lookup"><span data-stu-id="c22dd-206">Fast tree</span></span>

<span data-ttu-id="c22dd-207">用于特征化图像数据。</span><span class="sxs-lookup"><span data-stu-id="c22dd-207">Use for featurized image data.</span></span> <span data-ttu-id="c22dd-208">在非均衡数据方面具有弹性。</span><span class="sxs-lookup"><span data-stu-id="c22dd-208">Resilient to unbalanced data.</span></span> <span data-ttu-id="c22dd-209">高度可优化。</span><span class="sxs-lookup"><span data-stu-id="c22dd-209">Highly tunable.</span></span>

|<span data-ttu-id="c22dd-210">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-210">Trainer</span></span>|<span data-ttu-id="c22dd-211">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-211">Task</span></span>|<span data-ttu-id="c22dd-212">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-212">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>|<span data-ttu-id="c22dd-213">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-213">Binary classification</span></span>|<span data-ttu-id="c22dd-214">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-214">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeRegressionTrainer>|<span data-ttu-id="c22dd-215">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-215">Regression</span></span>|<span data-ttu-id="c22dd-216">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-216">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeTweedieTrainer>|<span data-ttu-id="c22dd-217">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-217">Regression</span></span>|<span data-ttu-id="c22dd-218">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-218">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.FastTree.FastTreeRankingTrainer>|<span data-ttu-id="c22dd-219">排名</span><span class="sxs-lookup"><span data-stu-id="c22dd-219">Ranking</span></span>|<span data-ttu-id="c22dd-220">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-220">No</span></span>|

### <a name="fast-forest"></a><span data-ttu-id="c22dd-221">快速林</span><span class="sxs-lookup"><span data-stu-id="c22dd-221">Fast forest</span></span>

<span data-ttu-id="c22dd-222">适用于干扰性数据。</span><span class="sxs-lookup"><span data-stu-id="c22dd-222">Works well with noisy data.</span></span>

|<span data-ttu-id="c22dd-223">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-223">Trainer</span></span>|<span data-ttu-id="c22dd-224">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-224">Task</span></span>|<span data-ttu-id="c22dd-225">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-225">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>|<span data-ttu-id="c22dd-226">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-226">Binary classification</span></span>|<span data-ttu-id="c22dd-227">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-227">Yes</span></span>|
|<xref:Microsoft.ML.Trainers.FastTree.FastForestRegressionTrainer>|<span data-ttu-id="c22dd-228">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-228">Regression</span></span>|<span data-ttu-id="c22dd-229">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-229">Yes</span></span>|

### <a name="generalized-additive-model-gam"></a><span data-ttu-id="c22dd-230">广义加性模型 (GAM)</span><span class="sxs-lookup"><span data-stu-id="c22dd-230">Generalized additive model (GAM)</span></span>

<span data-ttu-id="c22dd-231">最适合用于使用决策树算法时表现良好但可解释性为优先事项的问题。</span><span class="sxs-lookup"><span data-stu-id="c22dd-231">Best for problems that perform well with tree algorithms but where explainability is a priority.</span></span>

|<span data-ttu-id="c22dd-232">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-232">Trainer</span></span>|<span data-ttu-id="c22dd-233">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-233">Task</span></span>|<span data-ttu-id="c22dd-234">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-234">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>|<span data-ttu-id="c22dd-235">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-235">Binary classification</span></span>|<span data-ttu-id="c22dd-236">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-236">No</span></span>|
|<xref:Microsoft.ML.Trainers.FastTree.GamRegressionTrainer>|<span data-ttu-id="c22dd-237">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-237">Regression</span></span>|<span data-ttu-id="c22dd-238">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-238">No</span></span>|

## <a name="matrix-factorization"></a><span data-ttu-id="c22dd-239">矩阵分解</span><span class="sxs-lookup"><span data-stu-id="c22dd-239">Matrix factorization</span></span>

### <a name="matrix-factorization"></a><span data-ttu-id="c22dd-240">矩阵分解</span><span class="sxs-lookup"><span data-stu-id="c22dd-240">Matrix Factorization</span></span>

<span data-ttu-id="c22dd-241">用于建议中的[协作筛选](https://en.wikipedia.org/wiki/Collaborative_filtering)。</span><span class="sxs-lookup"><span data-stu-id="c22dd-241">Used for [collaborative filtering](https://en.wikipedia.org/wiki/Collaborative_filtering) in recommendation.</span></span>

|<span data-ttu-id="c22dd-242">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-242">Trainer</span></span>|<span data-ttu-id="c22dd-243">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-243">Task</span></span>|<span data-ttu-id="c22dd-244">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-244">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.MatrixFactorizationTrainer>|<span data-ttu-id="c22dd-245">建议</span><span class="sxs-lookup"><span data-stu-id="c22dd-245">Recommendation</span></span>|<span data-ttu-id="c22dd-246">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-246">No</span></span>|

### <a name="field-aware-factorization-machine"></a><span data-ttu-id="c22dd-247">场感知分解机</span><span class="sxs-lookup"><span data-stu-id="c22dd-247">Field Aware Factorization Machine</span></span>

 <span data-ttu-id="c22dd-248">最适合用于具有大型数据集的稀疏分类数据。</span><span class="sxs-lookup"><span data-stu-id="c22dd-248">Best for sparse categorical data, with large datasets.</span></span>

|<span data-ttu-id="c22dd-249">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-249">Trainer</span></span>|<span data-ttu-id="c22dd-250">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-250">Task</span></span>|<span data-ttu-id="c22dd-251">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-251">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.FieldAwareFactorizationMachineTrainer>|<span data-ttu-id="c22dd-252">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-252">Binary classification</span></span>|<span data-ttu-id="c22dd-253">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-253">No</span></span>|

## <a name="meta-algorithms"></a><span data-ttu-id="c22dd-254">元算法</span><span class="sxs-lookup"><span data-stu-id="c22dd-254">Meta algorithms</span></span>

<span data-ttu-id="c22dd-255">这些训练程序根据二元训练程序创建多类训练程序。</span><span class="sxs-lookup"><span data-stu-id="c22dd-255">These trainers create a multiclass trainer from a binary trainer.</span></span> <span data-ttu-id="c22dd-256">与 <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>、<xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>、<xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>、<xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer> 配合使用。</span><span class="sxs-lookup"><span data-stu-id="c22dd-256">Use with <xref:Microsoft.ML.Trainers.AveragedPerceptronTrainer>, <xref:Microsoft.ML.Trainers.LbfgsLogisticRegressionBinaryTrainer>, <xref:Microsoft.ML.Trainers.SymbolicSgdLogisticRegressionBinaryTrainer>, <xref:Microsoft.ML.Trainers.LightGbm.LightGbmBinaryTrainer>, <xref:Microsoft.ML.Trainers.FastTree.FastTreeBinaryTrainer>, <xref:Microsoft.ML.Trainers.FastTree.FastForestBinaryTrainer>, <xref:Microsoft.ML.Trainers.FastTree.GamBinaryTrainer>.</span></span>

### <a name="one-versus-all"></a><span data-ttu-id="c22dd-257">一对多</span><span class="sxs-lookup"><span data-stu-id="c22dd-257">One versus all</span></span>

<span data-ttu-id="c22dd-258">此多类分类器为每个类训练一个二元分类器，这可将该类与所有其他类区分开来。</span><span class="sxs-lookup"><span data-stu-id="c22dd-258">This multiclass classifier trains one binary classifier for each class, which distinguishes that class from all other classes.</span></span> <span data-ttu-id="c22dd-259">规模因要分类的类的数量而受到限制。</span><span class="sxs-lookup"><span data-stu-id="c22dd-259">Is limited in scale by the number of classes to categorize.</span></span>

|<span data-ttu-id="c22dd-260">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-260">Trainer</span></span>|<span data-ttu-id="c22dd-261">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-261">Task</span></span>|<span data-ttu-id="c22dd-262">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-262">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OneVersusAllTrainer>|<span data-ttu-id="c22dd-263">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-263">Multiclass classification</span></span>|<span data-ttu-id="c22dd-264">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-264">Yes</span></span>|

### <a name="pairwise-coupling"></a><span data-ttu-id="c22dd-265">成对耦合</span><span class="sxs-lookup"><span data-stu-id="c22dd-265">Pairwise coupling</span></span>

<span data-ttu-id="c22dd-266">此多类分类器在每对类上训练二元分类算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-266">This multiclass classifier trains a binary classification algorithm on each pair of classes.</span></span> <span data-ttu-id="c22dd-267">规模因类的数量而受到限制，因为必须训练每个两个类的组合。</span><span class="sxs-lookup"><span data-stu-id="c22dd-267">Is limited in scale by the number of classes, as each combination of two classes must be trained.</span></span>

|<span data-ttu-id="c22dd-268">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-268">Trainer</span></span>|<span data-ttu-id="c22dd-269">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-269">Task</span></span>|<span data-ttu-id="c22dd-270">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-270">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.PairwiseCouplingTrainer>|<span data-ttu-id="c22dd-271">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-271">Multiclass classification</span></span>|<span data-ttu-id="c22dd-272">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-272">No</span></span>|

## <a name="k-means"></a><span data-ttu-id="c22dd-273">K-Means</span><span class="sxs-lookup"><span data-stu-id="c22dd-273">K-Means</span></span>

<span data-ttu-id="c22dd-274">用于聚类分析。</span><span class="sxs-lookup"><span data-stu-id="c22dd-274">Used for clustering.</span></span>

|<span data-ttu-id="c22dd-275">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-275">Trainer</span></span>|<span data-ttu-id="c22dd-276">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-276">Task</span></span>|<span data-ttu-id="c22dd-277">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-277">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.KMeansTrainer>|<span data-ttu-id="c22dd-278">群集</span><span class="sxs-lookup"><span data-stu-id="c22dd-278">Clustering</span></span>|<span data-ttu-id="c22dd-279">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-279">Yes</span></span>|

## <a name="principal-component-analysis"></a><span data-ttu-id="c22dd-280">主体组件分析</span><span class="sxs-lookup"><span data-stu-id="c22dd-280">Principal component analysis</span></span>

<span data-ttu-id="c22dd-281">用于异常情况检测。</span><span class="sxs-lookup"><span data-stu-id="c22dd-281">Used for anomaly detection.</span></span>

|<span data-ttu-id="c22dd-282">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-282">Trainer</span></span>|<span data-ttu-id="c22dd-283">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-283">Task</span></span>|<span data-ttu-id="c22dd-284">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-284">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.RandomizedPcaTrainer>|<span data-ttu-id="c22dd-285">异常检测</span><span class="sxs-lookup"><span data-stu-id="c22dd-285">Anomaly detection</span></span>|<span data-ttu-id="c22dd-286">否</span><span class="sxs-lookup"><span data-stu-id="c22dd-286">No</span></span>|

## <a name="naive-bayes"></a><span data-ttu-id="c22dd-287">Naive Bayes</span><span class="sxs-lookup"><span data-stu-id="c22dd-287">Naive Bayes</span></span>

<span data-ttu-id="c22dd-288">当特征独立且训练数据集很小时，请使用此多类分类算法。</span><span class="sxs-lookup"><span data-stu-id="c22dd-288">Use this multi-class classification algorithm when the features are independent, and the training dataset is small.</span></span>

|<span data-ttu-id="c22dd-289">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-289">Trainer</span></span>|<span data-ttu-id="c22dd-290">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-290">Task</span></span>|<span data-ttu-id="c22dd-291">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-291">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.NaiveBayesMulticlassTrainer>|<span data-ttu-id="c22dd-292">多类分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-292">Multiclass classification</span></span>|<span data-ttu-id="c22dd-293">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-293">Yes</span></span>|

## <a name="prior-trainer"></a><span data-ttu-id="c22dd-294">前期训练程序</span><span class="sxs-lookup"><span data-stu-id="c22dd-294">Prior Trainer</span></span>

<span data-ttu-id="c22dd-295">使用此二元分类算法来确定其他训练程序的性能基线。</span><span class="sxs-lookup"><span data-stu-id="c22dd-295">Use this binary classification algorithm to baseline the performance of other trainers.</span></span> <span data-ttu-id="c22dd-296">其他训练程序的指标应优于前期训练程序才能成为有效指标。</span><span class="sxs-lookup"><span data-stu-id="c22dd-296">To be effective, the metrics of the other trainers should be better than the prior trainer.</span></span>

|<span data-ttu-id="c22dd-297">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-297">Trainer</span></span>|<span data-ttu-id="c22dd-298">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-298">Task</span></span>|<span data-ttu-id="c22dd-299">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-299">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.PriorTrainer>|<span data-ttu-id="c22dd-300">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-300">Binary classification</span></span>|<span data-ttu-id="c22dd-301">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-301">Yes</span></span>|

## <a name="support-vector-machines"></a><span data-ttu-id="c22dd-302">支持向量机</span><span class="sxs-lookup"><span data-stu-id="c22dd-302">Support vector machines</span></span>

<span data-ttu-id="c22dd-303">支持向量机 (SVM) 是极其流行的、经过广泛研究的监管学习模型种类，可在线性和非线性分类任务中使用。</span><span class="sxs-lookup"><span data-stu-id="c22dd-303">Support vector machines (SVMs) are an extremely popular and well-researched class of supervised learning models, which can be used in linear and non-linear classification tasks.</span></span>

<span data-ttu-id="c22dd-304">最近的研究重点探究了如何优化这些模型，以便将其有效扩展为更大型的定型集。</span><span class="sxs-lookup"><span data-stu-id="c22dd-304">Recent research has focused on ways to optimize these models to efficiently scale to larger training sets.</span></span>

### <a name="linear-svm"></a><span data-ttu-id="c22dd-305">线性 SVM</span><span class="sxs-lookup"><span data-stu-id="c22dd-305">Linear SVM</span></span>

<span data-ttu-id="c22dd-306">使用通过布尔标记的数据训练的线性二元分类模型预测目标。</span><span class="sxs-lookup"><span data-stu-id="c22dd-306">Predicts a target using a linear binary classification model trained over boolean labeled data.</span></span> <span data-ttu-id="c22dd-307">交替使用随机梯度下降步骤和投影步骤。</span><span class="sxs-lookup"><span data-stu-id="c22dd-307">Alternates between stochastic gradient descent steps and projection steps.</span></span>

|<span data-ttu-id="c22dd-308">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-308">Trainer</span></span>|<span data-ttu-id="c22dd-309">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-309">Task</span></span>|<span data-ttu-id="c22dd-310">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-310">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LinearSvmTrainer>|<span data-ttu-id="c22dd-311">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-311">Binary classification</span></span>|<span data-ttu-id="c22dd-312">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-312">Yes</span></span>|

### <a name="local-deep-svm"></a><span data-ttu-id="c22dd-313">本地深度 SVM</span><span class="sxs-lookup"><span data-stu-id="c22dd-313">Local Deep SVM</span></span>

<span data-ttu-id="c22dd-314">使用非线性二元分类模型预测目标。</span><span class="sxs-lookup"><span data-stu-id="c22dd-314">Predicts a target using a non-linear binary classification model.</span></span> <span data-ttu-id="c22dd-315">缩减预测时间成本；预测成本随训练集的大小呈对数而非线性增长，可接受在分类准确性方面出现损失。</span><span class="sxs-lookup"><span data-stu-id="c22dd-315">Reduces the prediction time cost; the prediction cost grows logarithmically with the size of the training set, rather than linearly, with a tolerable loss in classification accuracy.</span></span>

|<span data-ttu-id="c22dd-316">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-316">Trainer</span></span>|<span data-ttu-id="c22dd-317">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-317">Task</span></span>|<span data-ttu-id="c22dd-318">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-318">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.LdSvmTrainer>|<span data-ttu-id="c22dd-319">二元分类</span><span class="sxs-lookup"><span data-stu-id="c22dd-319">Binary classification</span></span>|<span data-ttu-id="c22dd-320">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-320">Yes</span></span>|

## <a name="ordinary-least-squares"></a><span data-ttu-id="c22dd-321">普通最小二乘法</span><span class="sxs-lookup"><span data-stu-id="c22dd-321">Ordinary least squares</span></span>

<span data-ttu-id="c22dd-322">“普通最小二乘法 (OLS)”是线性回归中最常用的方法之一。</span><span class="sxs-lookup"><span data-stu-id="c22dd-322">Ordinary least squares (OLS) is one of the most commonly used techniques in linear regression.</span></span>

<span data-ttu-id="c22dd-323">普通最小二乘法是指损失函数，该函数将误差计算为实际值与预测线之间差值的平方和，通过最小化平方误差来拟合模型。</span><span class="sxs-lookup"><span data-stu-id="c22dd-323">Ordinary least squares refers to the loss function, which computes error as the sum of the square of distance from the actual value to the predicted line, and fits the model by minimizing the squared error.</span></span> <span data-ttu-id="c22dd-324">此方法假设输入与因变量之间存在较强的线性关系。</span><span class="sxs-lookup"><span data-stu-id="c22dd-324">This method assumes a strong linear relationship between the inputs and the dependent variable.</span></span>

|<span data-ttu-id="c22dd-325">培训师</span><span class="sxs-lookup"><span data-stu-id="c22dd-325">Trainer</span></span>|<span data-ttu-id="c22dd-326">任务</span><span class="sxs-lookup"><span data-stu-id="c22dd-326">Task</span></span>|<span data-ttu-id="c22dd-327">ONNX 可导出</span><span class="sxs-lookup"><span data-stu-id="c22dd-327">ONNX Exportable</span></span>|
|---------|----------|----------|
|<xref:Microsoft.ML.Trainers.OlsTrainer>|<span data-ttu-id="c22dd-328">回归</span><span class="sxs-lookup"><span data-stu-id="c22dd-328">Regression</span></span>|<span data-ttu-id="c22dd-329">是</span><span class="sxs-lookup"><span data-stu-id="c22dd-329">Yes</span></span>|
