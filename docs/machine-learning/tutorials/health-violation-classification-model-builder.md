---
title: 教程：使用模型生成器对卫生违规行为进行分类
description: 此教程说明如何使用 ML.NET 模型生成器生成一个多级分类模型，以对旧金山的餐馆卫生违规严重性进行分类。
author: luisquintanilla
ms.author: luquinta
ms.date: 11/21/2019
ms.topic: tutorial
ms.custom: mvc,mlnet-tooling
ms.openlocfilehash: 468d09510cb55971d25877e40b7c54aae479ac84
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104875611"
---
# <a name="tutorial-classify-the-severity-of-restaurant-health-violations-with-model-builder"></a><span data-ttu-id="894e2-103">教程：使用模型生成器对餐馆卫生违规行为的严重程度进行分类</span><span class="sxs-lookup"><span data-stu-id="894e2-103">Tutorial: Classify the severity of restaurant health violations with Model Builder</span></span>

<span data-ttu-id="894e2-104">了解如何使用模型生成器生成一个多级分类模型，以对在卫生检查期间发现的餐馆违规行为的风险级别进行分类。</span><span class="sxs-lookup"><span data-stu-id="894e2-104">Learn how to build a multiclass classification model using Model Builder to categorize the risk level of restaurant violations found during health inspections.</span></span>

<span data-ttu-id="894e2-105">在本教程中，你将了解：</span><span class="sxs-lookup"><span data-stu-id="894e2-105">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
>
> - <span data-ttu-id="894e2-106">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="894e2-106">Prepare and understand the data</span></span>
> - <span data-ttu-id="894e2-107">选择方案</span><span class="sxs-lookup"><span data-stu-id="894e2-107">Choose a scenario</span></span>
> - <span data-ttu-id="894e2-108">从数据库加载数据</span><span class="sxs-lookup"><span data-stu-id="894e2-108">Load data from a database</span></span>
> - <span data-ttu-id="894e2-109">定型模型</span><span class="sxs-lookup"><span data-stu-id="894e2-109">Train the model</span></span>
> - <span data-ttu-id="894e2-110">评估模型</span><span class="sxs-lookup"><span data-stu-id="894e2-110">Evaluate the model</span></span>
> - <span data-ttu-id="894e2-111">使用预测模型</span><span class="sxs-lookup"><span data-stu-id="894e2-111">Use the model for predictions</span></span>

> [!NOTE]
> <span data-ttu-id="894e2-112">模型生成器当前为预览版。</span><span class="sxs-lookup"><span data-stu-id="894e2-112">Model Builder is currently in Preview.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="894e2-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="894e2-113">Prerequisites</span></span>

<span data-ttu-id="894e2-114">有关先决条件和安装说明列表，请访问[模型生成器安装指南](../how-to-guides/install-model-builder.md)。</span><span class="sxs-lookup"><span data-stu-id="894e2-114">For a list of prerequisites and installation instructions, visit the [Model Builder installation guide](../how-to-guides/install-model-builder.md).</span></span>

## <a name="model-builder-multiclass-classification-overview"></a><span data-ttu-id="894e2-115">模型生成器多级分类概述</span><span class="sxs-lookup"><span data-stu-id="894e2-115">Model Builder multiclass classification overview</span></span>

<span data-ttu-id="894e2-116">此示例会创建一个 C# .NET Core 控制台应用程序，用于通过模型生成器生成的机器学习模型对卫生违规风险进行分类。</span><span class="sxs-lookup"><span data-stu-id="894e2-116">This sample creates a C# .NET Core console application that categorizes the risk of health violations using a machine learning model built with Model Builder.</span></span> <span data-ttu-id="894e2-117">有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub 存储库中找到。</span><span class="sxs-lookup"><span data-stu-id="894e2-117">You can find the source code for this tutorial at the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub repository.</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="894e2-118">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="894e2-118">Create a console application</span></span>

1. <span data-ttu-id="894e2-119">创建一个名为“RestaurantViolations”的 C# .NET Core 控制台应用程序。</span><span class="sxs-lookup"><span data-stu-id="894e2-119">Create a **C# .NET Core console application** called "RestaurantViolations".</span></span> <span data-ttu-id="894e2-120">请确保未选中“将解决方案和项目放置在同一目录中”(VS 2019) 或已选中“创建解决方案的目录”(VS 2017)     。</span><span class="sxs-lookup"><span data-stu-id="894e2-120">Make sure **Place solution and project in the same directory** is **unchecked** (VS 2019), or **Create directory for solution** is **checked** (VS 2017).</span></span>

## <a name="prepare-and-understand-the-data"></a><span data-ttu-id="894e2-121">准备和了解数据</span><span class="sxs-lookup"><span data-stu-id="894e2-121">Prepare and understand the data</span></span>

> <span data-ttu-id="894e2-122">培训和评估机器学习模型所用的数据集源自[旧金山公共卫生部餐馆安全性评分](https://www.sfdph.org/dph/EH/Food/score/default.asp)。</span><span class="sxs-lookup"><span data-stu-id="894e2-122">The data set used to train and evaluate the machine learning model is originally from the [San Francisco Department of Public Health Restaurant Safety Scores](https://www.sfdph.org/dph/EH/Food/score/default.asp).</span></span> <span data-ttu-id="894e2-123">为了便于使用，已将此数据集精简为仅包含与模型培训和进行预测相关的列。</span><span class="sxs-lookup"><span data-stu-id="894e2-123">For convenience, the dataset has been condensed to only include the columns relevant to train the model and make predictions.</span></span> <span data-ttu-id="894e2-124">如需详细了解此[数据集](https://data.sfgov.org/Health-and-Social-Services/Restaurant-Scores-LIVES-Standard/pyih-qa8i?row_index=0)，请访问以下网站。</span><span class="sxs-lookup"><span data-stu-id="894e2-124">Visit the following website to learn more about the [dataset](https://data.sfgov.org/Health-and-Social-Services/Restaurant-Scores-LIVES-Standard/pyih-qa8i?row_index=0).</span></span>

<span data-ttu-id="894e2-125">[下载餐馆安全性评分数据集](https://github.com/luisquintanilla/machinelearning-samples/raw/AB1608219/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantScores.zip)，并将其解压缩。</span><span class="sxs-lookup"><span data-stu-id="894e2-125">[Download the Restaurant Safety Scores dataset](https://github.com/luisquintanilla/machinelearning-samples/raw/AB1608219/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantScores.zip) and unzip it.</span></span>

<span data-ttu-id="894e2-126">此数据集中的各个行包含卫生部进行检查时发现的违规行为的相关信息，以及这些违规行为对公共卫生和安全构成威胁的风险评估。</span><span class="sxs-lookup"><span data-stu-id="894e2-126">Each row in the dataset contains information regarding violations observed during an inspection from the Health Department and a risk assessment of the threat those violations present to public health and safety.</span></span>

| <span data-ttu-id="894e2-127">检查类型</span><span class="sxs-lookup"><span data-stu-id="894e2-127">InspectionType</span></span> | <span data-ttu-id="894e2-128">违规行为描述</span><span class="sxs-lookup"><span data-stu-id="894e2-128">ViolationDescription</span></span> | <span data-ttu-id="894e2-129">风险类别</span><span class="sxs-lookup"><span data-stu-id="894e2-129">RiskCategory</span></span> |
| --- | --- | --- |
| <span data-ttu-id="894e2-130">常规 - 不定期</span><span class="sxs-lookup"><span data-stu-id="894e2-130">Routine - Unscheduled</span></span> | <span data-ttu-id="894e2-131">未对食物接触面进行充分清洁或消毒</span><span class="sxs-lookup"><span data-stu-id="894e2-131">Inadequately cleaned or sanitized food contact surfaces</span></span> | <span data-ttu-id="894e2-132">中等风险</span><span class="sxs-lookup"><span data-stu-id="894e2-132">Moderate Risk</span></span> |
| <span data-ttu-id="894e2-133">新营业场所</span><span class="sxs-lookup"><span data-stu-id="894e2-133">New Ownership</span></span> | <span data-ttu-id="894e2-134">高危害虫成群出现</span><span class="sxs-lookup"><span data-stu-id="894e2-134">High risk vermin infestation</span></span> | <span data-ttu-id="894e2-135">高风险</span><span class="sxs-lookup"><span data-stu-id="894e2-135">High Risk</span></span> |
| <span data-ttu-id="894e2-136">常规 - 不定期</span><span class="sxs-lookup"><span data-stu-id="894e2-136">Routine - Unscheduled</span></span> | <span data-ttu-id="894e2-137">擦拭布不干净或存放不当或消毒液不足</span><span class="sxs-lookup"><span data-stu-id="894e2-137">Wiping cloths not clean or properly stored or inadequate sanitizer</span></span> | <span data-ttu-id="894e2-138">低风险</span><span class="sxs-lookup"><span data-stu-id="894e2-138">Low Risk</span></span> |

- <span data-ttu-id="894e2-139">检查类型：检查的类型。</span><span class="sxs-lookup"><span data-stu-id="894e2-139">**InspectionType**: the type of inspection.</span></span> <span data-ttu-id="894e2-140">它可以是对新场所的首次检查、常规检查、投诉检查，以及其他各种类型的检查。</span><span class="sxs-lookup"><span data-stu-id="894e2-140">This can either be a first-time inspection for a new establishment, a routine inspection, a complaint inspection, and many other types.</span></span>
- <span data-ttu-id="894e2-141">违规行为描述：对检查期间发现的违规行为的描述。</span><span class="sxs-lookup"><span data-stu-id="894e2-141">**ViolationDescription**: a description of the violation found during inspection.</span></span>
- <span data-ttu-id="894e2-142">风险类别：违规行为对公共健康和安全构成风险的严重性。</span><span class="sxs-lookup"><span data-stu-id="894e2-142">**RiskCategory**: the risk severity a violation poses to public health and safety.</span></span>

<span data-ttu-id="894e2-143">`label` 是要预测的列。</span><span class="sxs-lookup"><span data-stu-id="894e2-143">The `label` is the column you want to predict.</span></span> <span data-ttu-id="894e2-144">执行分类任务时，目标是分配一个类别（文本或数值）。</span><span class="sxs-lookup"><span data-stu-id="894e2-144">When performing a classification task, the goal is to assign a category (text or numerical).</span></span> <span data-ttu-id="894e2-145">在此分类方案中，对违规行为的严重性赋值为：低风险、中等风险或高风险。</span><span class="sxs-lookup"><span data-stu-id="894e2-145">In this classification scenario, the severity of the violation is assigned the value of low, moderate, or high risk.</span></span> <span data-ttu-id="894e2-146">因此，“风险类别”是标签。</span><span class="sxs-lookup"><span data-stu-id="894e2-146">Therefore, the **RiskCategory** is the label.</span></span> <span data-ttu-id="894e2-147">`features` 是你为模型提供的用来预测 `label` 的输入。</span><span class="sxs-lookup"><span data-stu-id="894e2-147">The `features` are the inputs you give the model to predict the `label`.</span></span> <span data-ttu-id="894e2-148">在此案例中，“检查类型”和“违规行为描述”用作预测“风险类别”的特性或输入。</span><span class="sxs-lookup"><span data-stu-id="894e2-148">In this case, the **InspectionType** and **ViolationDescription** are used as features or inputs to predict the **RiskCategory**.</span></span>

## <a name="choose-a-scenario"></a><span data-ttu-id="894e2-149">选择方案</span><span class="sxs-lookup"><span data-stu-id="894e2-149">Choose a scenario</span></span>

![Visual Studio 中的“模型生成器”向导](./media/sentiment-analysis-model-builder/model-builder-screen.png)

<span data-ttu-id="894e2-151">为了训练模型，请从模型生成器提供的可用机器学习方案列表中进行选择。</span><span class="sxs-lookup"><span data-stu-id="894e2-151">To train your model, select from the list of available machine learning scenarios provided by Model Builder.</span></span> <span data-ttu-id="894e2-152">在此案例中，方案为“问题分类”。</span><span class="sxs-lookup"><span data-stu-id="894e2-152">In this case, the scenario is *Issue Classification*.</span></span>

1. <span data-ttu-id="894e2-153">在“解决方案资源管理器”中，右键单击“餐馆违规行为”项目，然后选择“添加” > “机器学习”。</span><span class="sxs-lookup"><span data-stu-id="894e2-153">In **Solution Explorer**, right-click the *RestaurantViolations* project, and select **Add** > **Machine Learning**.</span></span>
1. <span data-ttu-id="894e2-154">在此示例中，方案为多级分类。</span><span class="sxs-lookup"><span data-stu-id="894e2-154">For this sample, the scenario is multiclass classification.</span></span> <span data-ttu-id="894e2-155">在模型生成器的“方案”步骤中，选择“问题分类”方案。</span><span class="sxs-lookup"><span data-stu-id="894e2-155">In the *Scenario* step of Model Builder, select the **Issue Classification** scenario.</span></span>

## <a name="load-the-data"></a><span data-ttu-id="894e2-156">加载数据</span><span class="sxs-lookup"><span data-stu-id="894e2-156">Load the data</span></span>

<span data-ttu-id="894e2-157">模型生成器可接受来自 SQL Server 数据库或者 `csv` 或 `tsv` 格式的本地文件中的数据。</span><span class="sxs-lookup"><span data-stu-id="894e2-157">Model Builder accepts data from a SQL Server database or a local file in `csv` or `tsv` format.</span></span>

1. <span data-ttu-id="894e2-158">在模型生成器工具的数据步骤中，从数据源下拉列表中选择“SQL Server”。</span><span class="sxs-lookup"><span data-stu-id="894e2-158">In the data step of the Model Builder tool, select **SQL Server** from the data source dropdown.</span></span>
1. <span data-ttu-id="894e2-159">选择“连接到 SQL Server 数据库”文本框旁的按钮。</span><span class="sxs-lookup"><span data-stu-id="894e2-159">Select the button next to the **Connect to SQL Server database** text box.</span></span>
    1. <span data-ttu-id="894e2-160">在“选择数据”对话框中，选择“Microsoft SQL Server 数据库文件”。</span><span class="sxs-lookup"><span data-stu-id="894e2-160">In the **Choose Data** dialog, select **Microsoft SQL Server Database File**.</span></span>
    1. <span data-ttu-id="894e2-161">取消选中“始终使用此选择”复选框，然后选择“继续”。 </span><span class="sxs-lookup"><span data-stu-id="894e2-161">Uncheck the **Always use this selection** checkbox and select **Continue**.</span></span>
    1. <span data-ttu-id="894e2-162">在“连接属性”对话框中，选择“浏览”，然后选择已下载的“RestaurantScores.mdf”文件。</span><span class="sxs-lookup"><span data-stu-id="894e2-162">In the **Connection Properties** dialog, select **Browse** and select the downloaded *RestaurantScores.mdf* file.</span></span>
    1. <span data-ttu-id="894e2-163">选择“确定”。</span><span class="sxs-lookup"><span data-stu-id="894e2-163">Select **OK**.</span></span>
1. <span data-ttu-id="894e2-164">从“表名称”下拉列表中选择“违规行为”。</span><span class="sxs-lookup"><span data-stu-id="894e2-164">Choose **Violations** from the **Table Name** dropdown.</span></span>
1. <span data-ttu-id="894e2-165">在“要预测的列(标签)”下拉列表中选择“风险类别”。</span><span class="sxs-lookup"><span data-stu-id="894e2-165">Choose **RiskCategory** in the **Column to Predict (Label)** dropdown.</span></span>
1. <span data-ttu-id="894e2-166">保留默认的列选择，即在“输入列(特性)”下拉列表中选择的“检查类型”和“违规行为描述”。</span><span class="sxs-lookup"><span data-stu-id="894e2-166">Leave the default column selections, **InspectionType** and **ViolationDescription**, checked in the **Input Columns (Features)** dropdown.</span></span>
1. <span data-ttu-id="894e2-167">选择“培训”链接，转到模型生成器中的下一步。</span><span class="sxs-lookup"><span data-stu-id="894e2-167">Select the **Train** link to move to the next step in Model Builder.</span></span>

## <a name="train-the-model"></a><span data-ttu-id="894e2-168">定型模型</span><span class="sxs-lookup"><span data-stu-id="894e2-168">Train the model</span></span>

<span data-ttu-id="894e2-169">在本教程中，用于培训问题分类模型的机器学习任务是多级分类。</span><span class="sxs-lookup"><span data-stu-id="894e2-169">The machine learning task used to train the issue classification model in this tutorial is multiclass classification.</span></span> <span data-ttu-id="894e2-170">在模型培训过程中，模型生成器使用不同的多级分类算法和设置来培训各个模型，以便为数据集找到性能最佳的模型。</span><span class="sxs-lookup"><span data-stu-id="894e2-170">During the model training process, Model Builder trains separate models using different multiclass classification algorithms and settings to find the best performing model for your dataset.</span></span>

<span data-ttu-id="894e2-171">模型培训所需的时间与数据量成正比。</span><span class="sxs-lookup"><span data-stu-id="894e2-171">The time required for the model to train is proportional to the amount of data.</span></span> <span data-ttu-id="894e2-172">模型生成器会根据数据源的大小自动选择“训练时间(秒)”的默认值  。</span><span class="sxs-lookup"><span data-stu-id="894e2-172">Model Builder automatically selects a default value for **Time to train (seconds)** based on the size of your data source.</span></span>

1. <span data-ttu-id="894e2-173">尽管模型生成器将“训练时间(秒)”  的值设置为 10 秒，但可以将其增加到 30 秒。</span><span class="sxs-lookup"><span data-stu-id="894e2-173">Although Model Builder sets the value of **Time to train (seconds)** to 10 seconds, increase it to 30 seconds.</span></span> <span data-ttu-id="894e2-174">通过较长时间段的训练，模型生成器可以在最佳模型的搜索中浏览更多的算法和参数组合。</span><span class="sxs-lookup"><span data-stu-id="894e2-174">Training for a longer period of time allows Model Builder to explore a larger number of algorithms and combination of parameters in search of the best model.</span></span>
1. <span data-ttu-id="894e2-175">选择“开始训练”  。</span><span class="sxs-lookup"><span data-stu-id="894e2-175">Select **Start Training**.</span></span>

    <span data-ttu-id="894e2-176">在训练过程中，进度数据显示在训练步骤中的 `Progress` 部分。</span><span class="sxs-lookup"><span data-stu-id="894e2-176">Throughout the training process, progress data is displayed in the `Progress` section of the train step.</span></span>

    - <span data-ttu-id="894e2-177">“状态”显示培训进程的完成状态。</span><span class="sxs-lookup"><span data-stu-id="894e2-177">**Status** displays the completion status of the training process.</span></span>
    - <span data-ttu-id="894e2-178">“最高准确性”显示截至目前由模型生成器找到的性能最佳的模型的准确性。 </span><span class="sxs-lookup"><span data-stu-id="894e2-178">**Best accuracy** displays the accuracy of the best performing model found by Model Builder so far.</span></span> <span data-ttu-id="894e2-179">准确性越高，意味着模型对测试数据的预测越准确。</span><span class="sxs-lookup"><span data-stu-id="894e2-179">Higher accuracy means the model predicted more correctly on test data.</span></span>
    - <span data-ttu-id="894e2-180">“最佳算法”显示截至目前由模型生成器找到的性能最佳的算法的名称。 </span><span class="sxs-lookup"><span data-stu-id="894e2-180">**Best algorithm** displays the name of the best-performing algorithm found by Model Builder so far.</span></span>
    - <span data-ttu-id="894e2-181">“最新算法”显示模型生成器为了培训模型采用的最新算法名称。 </span><span class="sxs-lookup"><span data-stu-id="894e2-181">**Last algorithm** displays the name of the algorithm most recently used by Model Builder to train the model.</span></span>

1. <span data-ttu-id="894e2-182">培训完成后，选择“评估”链接以转到下一步。</span><span class="sxs-lookup"><span data-stu-id="894e2-182">Once training is complete, select the **Evaluate** link to move to the next step.</span></span>

## <a name="evaluate-the-model"></a><span data-ttu-id="894e2-183">评估模型</span><span class="sxs-lookup"><span data-stu-id="894e2-183">Evaluate the model</span></span>

<span data-ttu-id="894e2-184">培训步骤的成果将是一个具备最佳性能的模型。</span><span class="sxs-lookup"><span data-stu-id="894e2-184">The result of the training step is the one model that had the best performance.</span></span> <span data-ttu-id="894e2-185">在模型生成器的评估步骤中，输出部分将包含“最佳模型”项中性能最佳模型使用的算法，并包含“最佳模型准确度”中的指标。</span><span class="sxs-lookup"><span data-stu-id="894e2-185">In the evaluate step of Model Builder, the output section contains the algorithm used by the best performing model in the **Best Model** entry along with metrics in **Best Model Accuracy**.</span></span> <span data-ttu-id="894e2-186">此外，还会显示一个摘要表，其中最多包含五个已研究的模型及其指标。</span><span class="sxs-lookup"><span data-stu-id="894e2-186">Additionally, a summary table containing up to five models that were explored and their metrics is displayed.</span></span>

<span data-ttu-id="894e2-187">如果你对自己的准确性指标不满意，则尝试提高模型准确性的简单方法是增加模型的训练时间或使用更多数据。</span><span class="sxs-lookup"><span data-stu-id="894e2-187">If you're not satisfied with your accuracy metrics, some easy ways to try to improve model accuracy are to increase the amount of time to train the model or use more data.</span></span> <span data-ttu-id="894e2-188">否则，选择“代码”链接，转到模型生成器中的最后一步。</span><span class="sxs-lookup"><span data-stu-id="894e2-188">Otherwise, select the **code** link to move to the final step in Model Builder.</span></span>

## <a name="add-the-code-to-make-predictions"></a><span data-ttu-id="894e2-189">添加代码进行预测</span><span class="sxs-lookup"><span data-stu-id="894e2-189">Add the code to make predictions</span></span>

<span data-ttu-id="894e2-190">培训期间会创建两个项目。</span><span class="sxs-lookup"><span data-stu-id="894e2-190">Two projects are created as a result of the training process.</span></span>

- <span data-ttu-id="894e2-191">RestaurantViolationsML.ConsoleApp：一个 C# .NET Core 控制台应用程序，其中包含模型训练和示例使用代码。</span><span class="sxs-lookup"><span data-stu-id="894e2-191">RestaurantViolationsML.ConsoleApp: A C# .NET Core Console application that contains the model training and sample consumption code.</span></span>
- <span data-ttu-id="894e2-192">RestaurantViolationsML.Model：一个 .NET Standard 类库，包含定义输入和输出模型数据架构的数据模型、训练期间性能最佳的模型的保存版本，以及用于执行预测的帮助程序类（称为 `ConsumeModel`）。</span><span class="sxs-lookup"><span data-stu-id="894e2-192">RestaurantViolationsML.Model: A .NET Standard class library containing the data models that define the schema of input and output model data, the saved version of the best performing model during training, and a helper class called `ConsumeModel` to make predictions.</span></span>

1. <span data-ttu-id="894e2-193">在模型生成器的“代码”步骤中，选择“添加项目”，以将自动生成的项目添加到解决方案。</span><span class="sxs-lookup"><span data-stu-id="894e2-193">In the code step of Model Builder, select **Add Projects** to add the autogenerated projects to the solution.</span></span>
1. <span data-ttu-id="894e2-194">打开“餐馆违规行为”项目中的“Program.cs”文件。</span><span class="sxs-lookup"><span data-stu-id="894e2-194">Open the *Program.cs* file in the *RestaurantViolations* project.</span></span>
1. <span data-ttu-id="894e2-195">添加以下 using 语句以引用 RestaurantViolationsML.Model 项目：</span><span class="sxs-lookup"><span data-stu-id="894e2-195">Add the following using statement to reference the *RestaurantViolationsML.Model* project:</span></span>

    [!code-csharp [ProgramUsings](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L2)]

1. <span data-ttu-id="894e2-196">要使用模型对新数据进行预测，请在应用程序的 `Main` 方法内创建 `ModelInput` 类的新实例。</span><span class="sxs-lookup"><span data-stu-id="894e2-196">To make a prediction on new data using the model, create a new instance of the `ModelInput` class inside the `Main` method of your application.</span></span> <span data-ttu-id="894e2-197">请注意，风险类别不是输入的一部分。</span><span class="sxs-lookup"><span data-stu-id="894e2-197">Notice that the risk category is not part of the input.</span></span> <span data-ttu-id="894e2-198">这是因为模型将为它生成预测。</span><span class="sxs-lookup"><span data-stu-id="894e2-198">This is because the model generates the prediction for it.</span></span>

    [!code-csharp [TestData](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L11-L15)]

1. <span data-ttu-id="894e2-199">使用 `ConsumeModel` 类中的 `Predict` 方法。</span><span class="sxs-lookup"><span data-stu-id="894e2-199">Use the `Predict` method from the `ConsumeModel` class.</span></span> <span data-ttu-id="894e2-200">`Predict` 方法将加载经过培训的模型，为模型创建 [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) 并使用它对新数据进行预测。</span><span class="sxs-lookup"><span data-stu-id="894e2-200">The `Predict` method loads the trained model, creates a [`PredictionEngine`](xref:Microsoft.ML.PredictionEngine%602) for the model, and uses it to make predictions on new data.</span></span>

    [!code-csharp [Prediction](~/machinelearning-samples/samples/modelbuilder/MulticlassClassification_RestaurantViolations/RestaurantViolations/Program.cs#L17-L24)]

1. <span data-ttu-id="894e2-201">运行应用程序。</span><span class="sxs-lookup"><span data-stu-id="894e2-201">Run the application.</span></span>

    <span data-ttu-id="894e2-202">该程序生成的输出应类似于下面的代码段：</span><span class="sxs-lookup"><span data-stu-id="894e2-202">The output generated by the program should look similar to the snippet below:</span></span>

    ```bash
    Inspection Type: Complaint
    Violation Description: Inadequate sewage or wastewater disposal
    Risk Category: Moderate Risk
    ```

<span data-ttu-id="894e2-203">如果稍后需要在另一个解决方案中引用生成的项目，可以在 `C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` 目录中找到它们。</span><span class="sxs-lookup"><span data-stu-id="894e2-203">If you need to reference the generated projects at a later time inside of another solution, you can find them inside the `C:\Users\%USERNAME%\AppData\Local\Temp\MLVSTools` directory.</span></span>

<span data-ttu-id="894e2-204">祝贺你！</span><span class="sxs-lookup"><span data-stu-id="894e2-204">Congratulations!</span></span> <span data-ttu-id="894e2-205">你已成功使用模型生成器生成用于对卫生违规行为风险进行分类的机器学习模型。</span><span class="sxs-lookup"><span data-stu-id="894e2-205">You've successfully built a machine learning model to categorize the risk of health violations using Model Builder.</span></span> <span data-ttu-id="894e2-206">有关本教程中的源代码，可以从 [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub 存储库中找到。</span><span class="sxs-lookup"><span data-stu-id="894e2-206">You can find the source code for this tutorial at the [dotnet/machinelearning-samples](https://github.com/dotnet/machinelearning-samples/tree/main/samples/modelbuilder/MulticlassClassification_RestaurantViolations) GitHub repository.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="894e2-207">其他资源</span><span class="sxs-lookup"><span data-stu-id="894e2-207">Additional resources</span></span>

<span data-ttu-id="894e2-208">若要详细了解本教程中所述的主题，请访问以下资源:</span><span class="sxs-lookup"><span data-stu-id="894e2-208">To learn more about topics mentioned in this tutorial, visit the following resources:</span></span>

- [<span data-ttu-id="894e2-209">模型生成器应用场景</span><span class="sxs-lookup"><span data-stu-id="894e2-209">Model Builder Scenarios</span></span>](../automate-training-with-model-builder.md#scenario)
- [<span data-ttu-id="894e2-210">多类分类</span><span class="sxs-lookup"><span data-stu-id="894e2-210">Multiclass Classification</span></span>](../resources/glossary.md#multiclass-classification)
- [<span data-ttu-id="894e2-211">多级分类模型指标</span><span class="sxs-lookup"><span data-stu-id="894e2-211">Multiclass Classification Model Metrics</span></span>](../resources/metrics.md#evaluation-metrics-for-multi-class-classification)
