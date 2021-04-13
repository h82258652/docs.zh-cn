---
title: 使用 .NET for Apache Spark 进行批处理的教程
description: 了解如何使用 .NET for Apache Spark 进行批处理。
author: mamccrea
ms.author: mamccrea
ms.date: 10/09/2020
ms.topic: tutorial
ms.openlocfilehash: 28674c583466bb4873581c9d233b9a508019b045
ms.sourcegitcommit: c7f0beaa2bd66ebca86362ca17d673f7e8256ca6
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/23/2021
ms.locfileid: "104876937"
---
# <a name="tutorial-do-batch-processing-with-net-for-apache-spark"></a><span data-ttu-id="9eb46-103">教程：使用 .NET for Apache Spark 支持批处理操作</span><span class="sxs-lookup"><span data-stu-id="9eb46-103">Tutorial: Do batch processing with .NET for Apache Spark</span></span>

<span data-ttu-id="9eb46-104">在本教程中，了解如何使用 .NET for Apache Spark 进行批处理。</span><span class="sxs-lookup"><span data-stu-id="9eb46-104">In this tutorial, you learn how to do batch processing using .NET for Apache Spark.</span></span> <span data-ttu-id="9eb46-105">批处理是指静态数据的转换，这意味着源数据已经加载到数据存储中。</span><span class="sxs-lookup"><span data-stu-id="9eb46-105">Batch processing is the transformation of data at rest, meaning that the source data has already been loaded into data storage.</span></span>

<span data-ttu-id="9eb46-106">批处理通常在需要为进一步分析做好准备的大型、平面数据集上执行。</span><span class="sxs-lookup"><span data-stu-id="9eb46-106">Batch processing is generally performed over large, flat datasets that need to be prepared for further analysis.</span></span> <span data-ttu-id="9eb46-107">日志处理和数据仓库是常见的批处理方案。</span><span class="sxs-lookup"><span data-stu-id="9eb46-107">Log processing and data warehousing are common batch processing scenarios.</span></span> <span data-ttu-id="9eb46-108">在这种情况下，你将分析有关 GitHub 项目的信息，如不同项目的分叉次数或项目最近的更新情况。</span><span class="sxs-lookup"><span data-stu-id="9eb46-108">In this scenario, you analyze information about GitHub projects, such as the number of time different projects have been forked or how recently projects have been updated.</span></span>

<span data-ttu-id="9eb46-109">本教程介绍如何执行下列操作：</span><span class="sxs-lookup"><span data-stu-id="9eb46-109">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
>
> * <span data-ttu-id="9eb46-110">创建和运行 .NET for Apache Spark 应用程序</span><span class="sxs-lookup"><span data-stu-id="9eb46-110">Create and run a .NET for Apache Spark application</span></span>
> * <span data-ttu-id="9eb46-111">将数据读入 DataFrame 并准备进行分析</span><span class="sxs-lookup"><span data-stu-id="9eb46-111">Read data into a DataFrame and prepare it for analysis</span></span>
> * <span data-ttu-id="9eb46-112">使用 Spark SQL 处理数据</span><span class="sxs-lookup"><span data-stu-id="9eb46-112">Process the data using Spark SQL</span></span>

## <a name="prerequisites"></a><span data-ttu-id="9eb46-113">先决条件</span><span class="sxs-lookup"><span data-stu-id="9eb46-113">Prerequisites</span></span>

<span data-ttu-id="9eb46-114">如果这是你第一次使用 .NET for Apache Spark，请参阅 [.NET for Apache Spark 入门](get-started.md)教程，了解如何准备环境并运行第一个 .NET for Apache Spark 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9eb46-114">If this is your first time using .NET for Apache Spark, check out the [Get started with .NET for Apache Spark](get-started.md) tutorial to learn how to prepare your environment and run your first .NET for Apache Spark application.</span></span>

## <a name="download-the-sample-data"></a><span data-ttu-id="9eb46-115">下载示例数据</span><span class="sxs-lookup"><span data-stu-id="9eb46-115">Download the sample data</span></span>

<span data-ttu-id="9eb46-116">[GHTorrent](http://ghtorrent.org/) 监视所有公共 GitHub 事件，如有关项目、提交和观察程序的信息，并将这些事件及其结构存储在数据库中。</span><span class="sxs-lookup"><span data-stu-id="9eb46-116">[GHTorrent](http://ghtorrent.org/) monitors all public GitHub events, such as info about projects, commits, and watchers, and stores the events and their structure in databases.</span></span> <span data-ttu-id="9eb46-117">在不同时间段收集的数据可作为可下载存档。</span><span class="sxs-lookup"><span data-stu-id="9eb46-117">Data collected over different time periods is available as downloadable archives.</span></span> <span data-ttu-id="9eb46-118">因为转储文件非常大，所以本指南使用可从 GitHub 下载的[转储文件的截断版本](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/projects_smaller.csv)。</span><span class="sxs-lookup"><span data-stu-id="9eb46-118">Because the dump files are very large, this guide uses a [truncated version of the dump file](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/projects_smaller.csv) that can be downloaded from GitHub.</span></span>

> [!NOTE]
> <span data-ttu-id="9eb46-119">GHTorrent 数据集按照双重许可方案 ([Creative Commons +](https://wiki.creativecommons.org/wiki/CCPlus)) 进行分发。</span><span class="sxs-lookup"><span data-stu-id="9eb46-119">The GHTorrent dataset is distributed under a dual licensing scheme ([Creative Commons +](https://wiki.creativecommons.org/wiki/CCPlus)).</span></span> <span data-ttu-id="9eb46-120">对于非商业用途（包括但不限于教育、研究或个人用途），数据集按照 [CC-BY-SA 许可](https://creativecommons.org/licenses/by-sa/4.0/)进行分发。</span><span class="sxs-lookup"><span data-stu-id="9eb46-120">For non-commercial uses (including, but not limited to, educational, research or personal uses), the dataset is distributed under the [CC-BY-SA license](https://creativecommons.org/licenses/by-sa/4.0/).</span></span>

## <a name="create-a-console-application"></a><span data-ttu-id="9eb46-121">创建控制台应用程序</span><span class="sxs-lookup"><span data-stu-id="9eb46-121">Create a console application</span></span>

1. <span data-ttu-id="9eb46-122">在命令提示符处，运行以下命令来创建新的控制台应用程序：</span><span class="sxs-lookup"><span data-stu-id="9eb46-122">In your command prompt, run the following commands to create a new console application:</span></span>

   ```dotnetcli
   dotnet new console -o mySparkBatchApp
   cd mySparkBatchApp
   ```

   <span data-ttu-id="9eb46-123">`dotnet` 命令将创建 `console` 类型的 `new` 应用程序。</span><span class="sxs-lookup"><span data-stu-id="9eb46-123">The `dotnet` command creates a `new` application of type `console` for you.</span></span> <span data-ttu-id="9eb46-124">`-o` 参数将创建名为“mySparkBatchApp”的目录，其中会存储你的应用并填充必需文件。</span><span class="sxs-lookup"><span data-stu-id="9eb46-124">The `-o` parameter creates a directory named *mySparkBatchApp* where your app is stored and populates it with the required files.</span></span> <span data-ttu-id="9eb46-125">`cd mySparkBatchApp` 命令会将目录更改为你刚才创建的应用目录。</span><span class="sxs-lookup"><span data-stu-id="9eb46-125">The `cd mySparkBatchApp` command changes the directory to the app directory you just created.</span></span>

1. <span data-ttu-id="9eb46-126">要在应用中使用 .NET for Apache Spark，请安装 Microsoft.Spark 包。</span><span class="sxs-lookup"><span data-stu-id="9eb46-126">To use .NET for Apache Spark in an app, install the Microsoft.Spark package.</span></span> <span data-ttu-id="9eb46-127">在控制台中运行以下命令：</span><span class="sxs-lookup"><span data-stu-id="9eb46-127">In your console, run the following command:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="create-a-sparksession"></a><span data-ttu-id="9eb46-128">创建 SparkSession</span><span class="sxs-lookup"><span data-stu-id="9eb46-128">Create a SparkSession</span></span>

1. <span data-ttu-id="9eb46-129">将以下附加的 `using` 语句添加到 mySparkBatchApp 中的 Program.cs 文件顶部。</span><span class="sxs-lookup"><span data-stu-id="9eb46-129">Add the following additional `using` statements to the top of the *Program.cs* file in *mySparkBatchApp*.</span></span>

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. <span data-ttu-id="9eb46-130">将以下代码添加到文件命名空间。</span><span class="sxs-lookup"><span data-stu-id="9eb46-130">Add the following code to your project namespace.</span></span> <span data-ttu-id="9eb46-131">稍后在程序中使用 s_referenceData 基于日期进行筛选。</span><span class="sxs-lookup"><span data-stu-id="9eb46-131">*s_referenceData* is used later in the program to filter based on date.</span></span>

   ```csharp
   static readonly DateTime s_referenceDate = new DateTime(2015, 10, 20);
   ```

1. <span data-ttu-id="9eb46-132">在 Main 方法中添加以下代码，以建立新的 SparkSession。</span><span class="sxs-lookup"><span data-stu-id="9eb46-132">Add the following code inside your Main method to establish a new SparkSession.</span></span> <span data-ttu-id="9eb46-133">SparkSession 是使用 Dataset 和 DataFrame API 对 Spark 进行编程的入口点。</span><span class="sxs-lookup"><span data-stu-id="9eb46-133">The SparkSession is the entry point to programming Spark with the Dataset and DataFrame API.</span></span> <span data-ttu-id="9eb46-134">通过调用 `spark` 对象，可在整个程序中访问 Spark 和 DataFrame 功能。</span><span class="sxs-lookup"><span data-stu-id="9eb46-134">By calling the `spark` object, you can access Spark and DataFrame functionality throughout your program.</span></span>

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("GitHub and Spark Batch")
        .GetOrCreate();
   ```

## <a name="prepare-the-data"></a><span data-ttu-id="9eb46-135">准备数据</span><span class="sxs-lookup"><span data-stu-id="9eb46-135">Prepare the data</span></span>

1. <span data-ttu-id="9eb46-136">将输入文件读入 `DataFrame`，这是组织到命名列中的分布式数据的集合。</span><span class="sxs-lookup"><span data-stu-id="9eb46-136">Read the input file into a `DataFrame`, which is a distributed collection of data organized into named columns.</span></span> <span data-ttu-id="9eb46-137">可以通过 <xref:Microsoft.Spark.Sql.DataFrame.Schema%2A> 设置数据列。</span><span class="sxs-lookup"><span data-stu-id="9eb46-137">You can set the columns for your data through <xref:Microsoft.Spark.Sql.DataFrame.Schema%2A>.</span></span> <span data-ttu-id="9eb46-138">使用 <xref:Microsoft.Spark.Sql.DataFrame.Show%2A> 方法可在 DataFrame 中显示数据。</span><span class="sxs-lookup"><span data-stu-id="9eb46-138">Use the <xref:Microsoft.Spark.Sql.DataFrame.Show%2A> method to display the data in your DataFrame.</span></span> <span data-ttu-id="9eb46-139">请确保将 CSV 文件路径更新为下载的 GitHub 数据的位置。</span><span class="sxs-lookup"><span data-stu-id="9eb46-139">Be sure to update the CSV file path to the location of the GitHub data you downloaded.</span></span>

   ```csharp
   DataFrame projectsDf = spark
       .Read()
       .Schema("id INT, url STRING, owner_id INT, " +
       "name STRING, descriptor STRING, language STRING, " +
       "created_at STRING, forked_from INT, deleted STRING" +
       "updated_at STRING")
       .Csv("filepath");

   projectsDf.Show();
   ```

1. <span data-ttu-id="9eb46-140">使用 <xref:Microsoft.Spark.Sql.DataFrame.Na%2A> 方法删除具有 NA (null) 值的行，并使用 <xref:Microsoft.Spark.Sql.DataFrame.Drop%2A> 方法从数据中移除某些列。</span><span class="sxs-lookup"><span data-stu-id="9eb46-140">Use the <xref:Microsoft.Spark.Sql.DataFrame.Na%2A> method to drop rows with NA (null) values, and the <xref:Microsoft.Spark.Sql.DataFrame.Drop%2A> method to remove certain columns from your data.</span></span> <span data-ttu-id="9eb46-141">如果尝试分析与最终分析无关的 null 数据或列，这有助于防止错误。</span><span class="sxs-lookup"><span data-stu-id="9eb46-141">This helps prevent errors if you try to analyze null data or columns that are not relevant to your final analysis.</span></span>

   ```csharp
   // Drop any rows with NA values
   DataFrameNaFunctions dropEmptyProjects = projectsDf.Na();
   DataFrame cleanedProjects = dropEmptyProjects.Drop("any");

   // Remove unnecessary columns
   cleanedProjects = cleanedProjects.Drop("id", "url", "owner_id");
   cleanedProjects.Show();
   ```

## <a name="analyze-the-data"></a><span data-ttu-id="9eb46-142">分析数据</span><span class="sxs-lookup"><span data-stu-id="9eb46-142">Analyze the data</span></span>

<span data-ttu-id="9eb46-143">Spark SQL 允许对数据进行 SQL 调用。</span><span class="sxs-lookup"><span data-stu-id="9eb46-143">Spark SQL allows you to make SQL calls on your data.</span></span> <span data-ttu-id="9eb46-144">常见的方法是将用户定义的函数和 Spark SQL 组合在一起，以将用户定义的函数应用于 DataFrame 的所有行。</span><span class="sxs-lookup"><span data-stu-id="9eb46-144">It's common to combine user-defined functions and Spark SQL to apply a user-defined function to all rows of your DataFrame.</span></span>

<span data-ttu-id="9eb46-145">可以专门调用 `spark.Sql` 来模拟在其他类型的应用中所见的标准 SQL 调用。</span><span class="sxs-lookup"><span data-stu-id="9eb46-145">You can specifically call `spark.Sql` to mimic standard SQL calls seen in other types of apps.</span></span> <span data-ttu-id="9eb46-146">还可以调用 <xref:Microsoft.Spark.Sql.DataFrame.GroupBy%2A> 和 <xref:Microsoft.Spark.Sql.DataFrame.Agg%2A> 等方法，以便专门合并、筛选和执行数据计算。</span><span class="sxs-lookup"><span data-stu-id="9eb46-146">You can also call methods like <xref:Microsoft.Spark.Sql.DataFrame.GroupBy%2A> and <xref:Microsoft.Spark.Sql.DataFrame.Agg%2A> to specifically combine, filter, and perform calculations on your data.</span></span>

<span data-ttu-id="9eb46-147">此应用的目标是获取有关 GitHub 项目数据的一些见解。</span><span class="sxs-lookup"><span data-stu-id="9eb46-147">The goal of this app is to gain some insights about the GitHub projects data.</span></span> <span data-ttu-id="9eb46-148">将以下代码段添加到程序中以分析数据。</span><span class="sxs-lookup"><span data-stu-id="9eb46-148">Add the following code snippets to your program to analyze the data.</span></span>

1. <span data-ttu-id="9eb46-149">添加下面的代码块，找到每种语言的分叉次数。</span><span class="sxs-lookup"><span data-stu-id="9eb46-149">Add the following block of code finds the number of times each language has been forked.</span></span> <span data-ttu-id="9eb46-150">首先，数据按语言分组。</span><span class="sxs-lookup"><span data-stu-id="9eb46-150">First, the data is grouped by language.</span></span> <span data-ttu-id="9eb46-151">然后，采用每种语言的平均分叉数。</span><span class="sxs-lookup"><span data-stu-id="9eb46-151">Then, the average number of forks from each language is taken.</span></span>

   ```csharp
   // Average number of times each language has been forked
   DataFrame groupedDF = cleanedProjects
       .GroupBy("language")
       .Agg(Avg(cleanedProjects["forked_from"]);
   ```

1. <span data-ttu-id="9eb46-152">添加下面的代码块按降序对平均分叉数进行排序，以查看分叉次数最多的语言。</span><span class="sxs-lookup"><span data-stu-id="9eb46-152">Add the following block of code to order the average number of forks in descending order to see which languages are the most forked.</span></span> <span data-ttu-id="9eb46-153">也就是说，将首先显示最大数量的分叉。</span><span class="sxs-lookup"><span data-stu-id="9eb46-153">That is, the largest number of forks will appear first.</span></span>

   ```csharp
   // Sort by most forked languages first
   groupedDF.OrderBy(Desc("avg(forked_from)")).Show();
   ```

1. <span data-ttu-id="9eb46-154">下一个代码块显示最近项目的更新情况。</span><span class="sxs-lookup"><span data-stu-id="9eb46-154">The next block of code shows you how recently projects have been updated.</span></span> <span data-ttu-id="9eb46-155">注册名为“MyUDF”的新用户定义的函数，并将其与在教程开头声明的日期 s_referenceDate 进行比较。</span><span class="sxs-lookup"><span data-stu-id="9eb46-155">You register a new user-defined function called *MyUDF* and compare it with a date, *s_referenceDate*, which was declared at the beginning of the tutorial.</span></span> <span data-ttu-id="9eb46-156">将每个项目的日期与参考日期进行比较。</span><span class="sxs-lookup"><span data-stu-id="9eb46-156">The date for each project is compared against the reference date.</span></span> <span data-ttu-id="9eb46-157">然后，使用 Spark SQL 对每个数据行调用 UDF，以分析数据集中的每个项目。</span><span class="sxs-lookup"><span data-stu-id="9eb46-157">Then, Spark SQL is used to call the UDF on each row of the data to analyze each project in the data set.</span></span>

   ```csharp
   spark.Udf().Register<string, bool>(
       "MyUDF",
       (date) => DateTime.TryParse(date, out DateTime convertedDate) &&
           (convertedDate > s_referenceDate);
   cleanedProjects.CreateOrReplaceTempView("dateView");

   DataFrame dateDf = spark.Sql(
       "SELECT *, MyUDF(dateView.updated_at) AS datebefore FROM dateView");
   dateDf.Show();
   ```

1. <span data-ttu-id="9eb46-158">调用 `spark.Stop()` 结束 SparkSession。</span><span class="sxs-lookup"><span data-stu-id="9eb46-158">Call `spark.Stop()` to end the SparkSession.</span></span>

## <a name="use-spark-submit-to-run-your-app"></a><span data-ttu-id="9eb46-159">使用 spark-submit 运行应用</span><span class="sxs-lookup"><span data-stu-id="9eb46-159">Use spark-submit to run your app</span></span>

1. <span data-ttu-id="9eb46-160">使用以下命令来构建 .NET 应用：</span><span class="sxs-lookup"><span data-stu-id="9eb46-160">Use the following command to build your .NET app:</span></span>

   ```dotnetcli
   dotnet build
   ```

1. <span data-ttu-id="9eb46-161">使用 `spark-submit` 运行应用。</span><span class="sxs-lookup"><span data-stu-id="9eb46-161">Run your app with `spark-submit`.</span></span> <span data-ttu-id="9eb46-162">请确保将包含实际路径的以下命令更新为 Microsoft Spark jar 文件。</span><span class="sxs-lookup"><span data-stu-id="9eb46-162">Be sure to update the following command with the actual paths to your Microsoft Spark jar file.</span></span>

   ```console
   spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /<path>/to/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar dotnet /<path>/to/netcoreapp<version>/mySparkBatchApp.dll
   ```

## <a name="get-the-code"></a><span data-ttu-id="9eb46-163">获取代码</span><span class="sxs-lookup"><span data-stu-id="9eb46-163">Get the code</span></span>

<span data-ttu-id="9eb46-164">可在 GitHub 上查看[完整解决方案](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/GitHubProjects.cs)。</span><span class="sxs-lookup"><span data-stu-id="9eb46-164">You can see the [full solution](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/GitHubProjects.cs) on GitHub.</span></span>

## <a name="next-steps"></a><span data-ttu-id="9eb46-165">后续步骤</span><span class="sxs-lookup"><span data-stu-id="9eb46-165">Next steps</span></span>

<span data-ttu-id="9eb46-166">继续阅读下一篇文章，了解如何使用 .NET for Apache Spark 处理流数据。</span><span class="sxs-lookup"><span data-stu-id="9eb46-166">Advance to the next article to learn how to process streaming data with .NET for Apache Spark.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="9eb46-167">教程：使用 .NET for Apache Spark 进行结构化流式处理</span><span class="sxs-lookup"><span data-stu-id="9eb46-167">Tutorial: Structured Streaming with .NET for Apache Spark</span></span>](streaming.md)
