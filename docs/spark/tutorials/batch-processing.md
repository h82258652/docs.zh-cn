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
# <a name="tutorial-do-batch-processing-with-net-for-apache-spark"></a>教程：使用 .NET for Apache Spark 支持批处理操作

在本教程中，了解如何使用 .NET for Apache Spark 进行批处理。 批处理是指静态数据的转换，这意味着源数据已经加载到数据存储中。

批处理通常在需要为进一步分析做好准备的大型、平面数据集上执行。 日志处理和数据仓库是常见的批处理方案。 在这种情况下，你将分析有关 GitHub 项目的信息，如不同项目的分叉次数或项目最近的更新情况。

本教程介绍如何执行下列操作：

> [!div class="checklist"]
>
> * 创建和运行 .NET for Apache Spark 应用程序
> * 将数据读入 DataFrame 并准备进行分析
> * 使用 Spark SQL 处理数据

## <a name="prerequisites"></a>先决条件

如果这是你第一次使用 .NET for Apache Spark，请参阅 [.NET for Apache Spark 入门](get-started.md)教程，了解如何准备环境并运行第一个 .NET for Apache Spark 应用程序。

## <a name="download-the-sample-data"></a>下载示例数据

[GHTorrent](http://ghtorrent.org/) 监视所有公共 GitHub 事件，如有关项目、提交和观察程序的信息，并将这些事件及其结构存储在数据库中。 在不同时间段收集的数据可作为可下载存档。 因为转储文件非常大，所以本指南使用可从 GitHub 下载的[转储文件的截断版本](https://github.com/dotnet/spark/tree/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/projects_smaller.csv)。

> [!NOTE]
> GHTorrent 数据集按照双重许可方案 ([Creative Commons +](https://wiki.creativecommons.org/wiki/CCPlus)) 进行分发。 对于非商业用途（包括但不限于教育、研究或个人用途），数据集按照 [CC-BY-SA 许可](https://creativecommons.org/licenses/by-sa/4.0/)进行分发。

## <a name="create-a-console-application"></a>创建控制台应用程序

1. 在命令提示符处，运行以下命令来创建新的控制台应用程序：

   ```dotnetcli
   dotnet new console -o mySparkBatchApp
   cd mySparkBatchApp
   ```

   `dotnet` 命令将创建 `console` 类型的 `new` 应用程序。 `-o` 参数将创建名为“mySparkBatchApp”的目录，其中会存储你的应用并填充必需文件。 `cd mySparkBatchApp` 命令会将目录更改为你刚才创建的应用目录。

1. 要在应用中使用 .NET for Apache Spark，请安装 Microsoft.Spark 包。 在控制台中运行以下命令：

   ```dotnetcli
   dotnet add package Microsoft.Spark
   ```

## <a name="create-a-sparksession"></a>创建 SparkSession

1. 将以下附加的 `using` 语句添加到 mySparkBatchApp 中的 Program.cs 文件顶部。

   ```csharp
   using System;
   using Microsoft.Spark.Sql;
   using static Microsoft.Spark.Sql.Functions;
   ```

1. 将以下代码添加到文件命名空间。 稍后在程序中使用 s_referenceData 基于日期进行筛选。

   ```csharp
   static readonly DateTime s_referenceDate = new DateTime(2015, 10, 20);
   ```

1. 在 Main 方法中添加以下代码，以建立新的 SparkSession。 SparkSession 是使用 Dataset 和 DataFrame API 对 Spark 进行编程的入口点。 通过调用 `spark` 对象，可在整个程序中访问 Spark 和 DataFrame 功能。

   ```csharp
   SparkSession spark = SparkSession
        .Builder()
        .AppName("GitHub and Spark Batch")
        .GetOrCreate();
   ```

## <a name="prepare-the-data"></a>准备数据

1. 将输入文件读入 `DataFrame`，这是组织到命名列中的分布式数据的集合。 可以通过 <xref:Microsoft.Spark.Sql.DataFrame.Schema%2A> 设置数据列。 使用 <xref:Microsoft.Spark.Sql.DataFrame.Show%2A> 方法可在 DataFrame 中显示数据。 请确保将 CSV 文件路径更新为下载的 GitHub 数据的位置。

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

1. 使用 <xref:Microsoft.Spark.Sql.DataFrame.Na%2A> 方法删除具有 NA (null) 值的行，并使用 <xref:Microsoft.Spark.Sql.DataFrame.Drop%2A> 方法从数据中移除某些列。 如果尝试分析与最终分析无关的 null 数据或列，这有助于防止错误。

   ```csharp
   // Drop any rows with NA values
   DataFrameNaFunctions dropEmptyProjects = projectsDf.Na();
   DataFrame cleanedProjects = dropEmptyProjects.Drop("any");

   // Remove unnecessary columns
   cleanedProjects = cleanedProjects.Drop("id", "url", "owner_id");
   cleanedProjects.Show();
   ```

## <a name="analyze-the-data"></a>分析数据

Spark SQL 允许对数据进行 SQL 调用。 常见的方法是将用户定义的函数和 Spark SQL 组合在一起，以将用户定义的函数应用于 DataFrame 的所有行。

可以专门调用 `spark.Sql` 来模拟在其他类型的应用中所见的标准 SQL 调用。 还可以调用 <xref:Microsoft.Spark.Sql.DataFrame.GroupBy%2A> 和 <xref:Microsoft.Spark.Sql.DataFrame.Agg%2A> 等方法，以便专门合并、筛选和执行数据计算。

此应用的目标是获取有关 GitHub 项目数据的一些见解。 将以下代码段添加到程序中以分析数据。

1. 添加下面的代码块，找到每种语言的分叉次数。 首先，数据按语言分组。 然后，采用每种语言的平均分叉数。

   ```csharp
   // Average number of times each language has been forked
   DataFrame groupedDF = cleanedProjects
       .GroupBy("language")
       .Agg(Avg(cleanedProjects["forked_from"]);
   ```

1. 添加下面的代码块按降序对平均分叉数进行排序，以查看分叉次数最多的语言。 也就是说，将首先显示最大数量的分叉。

   ```csharp
   // Sort by most forked languages first
   groupedDF.OrderBy(Desc("avg(forked_from)")).Show();
   ```

1. 下一个代码块显示最近项目的更新情况。 注册名为“MyUDF”的新用户定义的函数，并将其与在教程开头声明的日期 s_referenceDate 进行比较。 将每个项目的日期与参考日期进行比较。 然后，使用 Spark SQL 对每个数据行调用 UDF，以分析数据集中的每个项目。

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

1. 调用 `spark.Stop()` 结束 SparkSession。

## <a name="use-spark-submit-to-run-your-app"></a>使用 spark-submit 运行应用

1. 使用以下命令来构建 .NET 应用：

   ```dotnetcli
   dotnet build
   ```

1. 使用 `spark-submit` 运行应用。 请确保将包含实际路径的以下命令更新为 Microsoft Spark jar 文件。

   ```console
   spark-submit --class org.apache.spark.deploy.dotnet.DotnetRunner --master local /<path>/to/microsoft-spark-<spark_majorversion-spark_minorversion>_<scala_majorversion.scala_minorversion>-<spark_dotnet_version>.jar dotnet /<path>/to/netcoreapp<version>/mySparkBatchApp.dll
   ```

## <a name="get-the-code"></a>获取代码

可在 GitHub 上查看[完整解决方案](https://github.com/dotnet/spark/blob/main/examples/Microsoft.Spark.CSharp.Examples/Sql/Batch/GitHubProjects.cs)。

## <a name="next-steps"></a>后续步骤

继续阅读下一篇文章，了解如何使用 .NET for Apache Spark 处理流数据。
> [!div class="nextstepaction"]
> [教程：使用 .NET for Apache Spark 进行结构化流式处理](streaming.md)
