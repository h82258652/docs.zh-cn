---
title: System.Text.Json 中的 DateTime 和 DateTimeOffset 支持
description: 有关如何在库中的 System.Text.Js上支持 DateTime 和 DateTimeOffset 类型的概述。
author: layomia
ms.author: laakinri
ms.date: 07/22/2019
helpviewer_keywords:
- JSON, Serializer, Utf8
- JSON DateTime, JSON DateTimeOffset
- DateTime, DateTimeOffset
- JsonSerializer, Utf8JsonReader, Utf8JsonWriter, JsonElement, JsonDocument
- JSON Serializer, JSON Reader, JSON Writer
- Converter, JSON Converter, DateTime Converter
- ISO, ISO 8601, ISO 8601-1:2019
ms.openlocfilehash: 6b4e79e6c666731e313ed41e25f601df4158b8d4
ms.sourcegitcommit: 4b7f6b348c986556ef805cb6baacfd5b9ec18ed0
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/08/2021
ms.locfileid: "107075415"
---
# <a name="datetime-and-datetimeoffset-support-in-systemtextjson"></a>System.Text.Json 中的 DateTime 和 DateTimeOffset 支持

库中的 System.Text.Js<xref:System.DateTime> <xref:System.DateTimeOffset> 根据 ISO 8601:-2019 扩展配置文件分析并写入和值。
[转换器](xref:System.Text.Json.Serialization.JsonConverter%601) 为序列化和反序列化提供自定义支持 <xref:System.Text.Json.JsonSerializer> 。
当使用和时，还可以实现自定义支持 <xref:System.Text.Json.Utf8JsonReader> <xref:System.Text.Json.Utf8JsonWriter> 。

## <a name="support-for-the-iso-8601-12019-format"></a>支持 ISO 8601-1:2019 格式

<xref:System.Text.Json.JsonSerializer>、、 <xref:System.Text.Json.Utf8JsonReader> <xref:System.Text.Json.Utf8JsonWriter> 和 <xref:System.Text.Json.JsonElement> 类型 <xref:System.DateTime> <xref:System.DateTimeOffset> 根据 ISO 8601-1:2019 格式的扩展配置文件分析和写入和文本表示形式; 例如，26T16：59： 57-05：00。

<xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据可通过以下方式序列化 <xref:System.Text.Json.JsonSerializer> ：

[!code-csharp[example-serializing-with-jsonserializer](~/samples/snippets/standard/datetime/json/csharp/serializing-with-jsonserializer/Program.cs)]

还可以通过以下方式对其进行反序列化 <xref:System.Text.Json.JsonSerializer> ：

[!code-csharp[example-deserializing-with-jsonserializer-valid](~/samples/snippets/standard/datetime/json/csharp/deserializing-with-jsonserializer-valid/Program.cs)]

对于默认选项，输入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 文本表示形式必须符合扩展 ISO 8601-1:2019 配置文件。
尝试对不符合配置文件的表示形式进行反序列化将导致 <xref:System.Text.Json.JsonSerializer> 引发 <xref:System.Text.Json.JsonException> ：

[!code-csharp[example-deserializing-with-jsonserializer-error](~/samples/snippets/standard/datetime/json/csharp/deserializing-with-jsonserializer-error/Program.cs)]

<xref:System.Text.Json.JsonDocument>提供对 JSON 有效负载（包括 <xref:System.DateTime> 和表示形式）内容的结构化访问 <xref:System.DateTimeOffset> 。 下面的示例演示了当给定温度集合时，可以计算星期一的平均温度：

[!code-csharp[example-computing-with-jsondocument-valid](~/samples/snippets/standard/datetime/json/csharp/computing-with-jsondocument-valid/Program.cs)]

尝试使用不符合表示形式的有效负载来计算平均温度 <xref:System.DateTime> 将导致 <xref:System.Text.Json.JsonDocument> 引发 <xref:System.FormatException> ：

[!code-csharp[example-computing-with-jsondocument-error](~/samples/snippets/standard/datetime/json/csharp/computing-with-jsondocument-error/Program.cs)]

较低级别的 <xref:System.Text.Json.Utf8JsonWriter> 写入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：

[!code-csharp[example-writing-with-utf8jsonwriter](~/samples/snippets/standard/datetime/json/csharp/writing-with-utf8jsonwriter/Program.cs)]

<xref:System.Text.Json.Utf8JsonReader> 分析 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：

[!code-csharp[example-reading-with-utf8jsonreader-valid](~/samples/snippets/standard/datetime/json/csharp/reading-with-utf8jsonreader-valid/Program.cs)]

尝试使用读取不符合格式 <xref:System.Text.Json.Utf8JsonReader> 将导致其引发 <xref:System.FormatException> ：

[!code-csharp[example-reading-with-utf8jsonreader-error](~/samples/snippets/standard/datetime/json/csharp/reading-with-utf8jsonreader-error/Program.cs)]

## <a name="custom-support-for-xrefsystemdatetime-and-xrefsystemdatetimeoffset"></a>对和的自定义支持 <xref:System.DateTime><xref:System.DateTimeOffset>

### <a name="when-using-xrefsystemtextjsonjsonserializer"></a>当使用 <xref:System.Text.Json.JsonSerializer>

如果希望序列化程序执行自定义分析或格式设置，可以实现 [自定义转换器](xref:System.Text.Json.Serialization.JsonConverter%601)。
以下是一些示例：

#### <a name="using-datetimeoffsetparse-and-datetimeoffsettostring"></a>使用 `DateTime(Offset).Parse` 和 `DateTime(Offset).ToString`

如果无法确定输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式的格式，则可以使用 `DateTime(Offset).Parse` 转换器读取逻辑中的方法。 这允许使用。NET 对分析各种 <xref:System.DateTime> 和文本格式的广泛支持 <xref:System.DateTimeOffset> ，包括非 iso 8601 字符串和不符合扩展 iso 8601-1:2019 配置文件的 iso 8601 格式。 与使用序列化程序的本机实现相比，此方法的性能大大降低。

对于序列化，可以 `DateTime(Offset).ToString` 在转换器写入逻辑中使用方法。 这使您可以 <xref:System.DateTime> <xref:System.DateTimeOffset> 使用任何 [标准日期和时间格式](../base-types/standard-date-and-time-format-strings.md)以及 [自定义日期和时间格式](../base-types/custom-date-and-time-format-strings.md)来编写和值。
与使用序列化程序的本机实现相比，这种性能也明显降低。

[!code-csharp[example-showing-datetime-parse](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example1/Program.cs)]

> [!NOTE]
> 在实现时 <xref:System.Text.Json.Serialization.JsonConverter%601> ，和 `T` 为时 <xref:System.DateTime> ， `typeToConvert` 参数将始终为 `typeof(DateTime)` 。
参数可用于处理多态事例和使用泛型以高 `typeof(T)` 性能方式获取。

#### <a name="using-xrefsystembufferstextutf8parser-and-xrefsystembufferstextutf8formatter"></a>使用 <xref:System.Buffers.Text.Utf8Parser> 和 <xref:System.Buffers.Text.Utf8Formatter>

如果输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式符合 "R"、"l"、"O" 或 "G" [标准日期和时间格式字符串](../base-types/standard-date-and-time-format-strings.md)之一，或者您想要根据这些格式中的一种进行写入，则可以在转换器逻辑中使用快速的基于 utf-8 的分析和格式设置方法。 这比使用和更快 `DateTime(Offset).Parse` `DateTime(Offset).ToString` 。

此示例演示一个自定义转换器，该转换器 <xref:System.DateTime> 根据 ["R" 标准格式](../base-types/standard-date-and-time-format-strings.md#the-rfc1123-r-r-format-specifier)对值进行序列化和反序列化：

[!code-csharp[example-showing-utf8-parser-and-formatter](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example2/Program.cs)]

> [!NOTE]
> "R" 标准格式的长度始终为29个字符。
>
> "L" (小写 "L" ) 格式不附带其他 [标准日期和时间格式字符串](../base-types/standard-date-and-time-format-strings.md) ，因为仅和类型支持此格式 `Utf8Parser` `Utf8Formatter` 。 格式为小写 RFC 1123 ("R" 格式的小写版本) ，例如： "thu，25 7 2019 月 06:36:07 gmt"。

#### <a name="using-datetimeoffsetparse-as-a-fallback-to-the-serializers-native-parsing"></a>使用 `DateTime(Offset).Parse` 作为序列化程序的本机解析的回退

如果你通常希望输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 数据符合扩展 ISO 8601-1:2019 配置文件，则可以使用序列化程序的本机分析逻辑。 您还可以实现一种回退机制。
此示例显示，在无法 <xref:System.DateTime> 使用分析文本表示形式后 <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)> ，转换器使用成功分析数据 <xref:System.DateTime.Parse(System.String)> 。

[!code-csharp[example-showing-datetime-parse-as-fallback](~/samples/snippets/standard/datetime/json/csharp/datetime-converter-examples/example3/Program.cs)]

#### <a name="using-unix-epoch-date-format"></a>使用 Unix epoch 日期格式

下面的转换器用时区格式处理 Unix epoch (值，如 `/Date(1590863400000-0700)/`) ：

:::code language="csharp" source="../serialization/snippets/system-text-json-how-to-5-0/csharp/CustomConverterUnixEpochDate.cs" id="ConverterOnly":::

### <a name="when-writing-with-xrefsystemtextjsonutf8jsonwriter"></a>写入时 <xref:System.Text.Json.Utf8JsonWriter>

如果要使用编写自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式 <xref:System.Text.Json.Utf8JsonWriter> ，可以将自定义表示形式设置为 <xref:System.String> 、 `ReadOnlySpan<Byte>` 、 `ReadOnlySpan<Char>` 或 <xref:System.Text.Json.JsonEncodedText> ，然后将其传递给相应的 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A?displayProperty=nameWithType> 或 <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A?displayProperty=nameWithType> 方法。

下面的示例演示如何 <xref:System.DateTime> 使用创建自定义格式 <xref:System.DateTime.ToString(System.String,System.IFormatProvider)> ，然后使用方法编写该格式 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue(System.String)> ：

[!code-csharp[example-custom-writing-with-utf8jsonwriter](~/samples/snippets/standard/datetime/json/csharp/custom-writing-with-utf8jsonwriter/Program.cs)]

### <a name="when-reading-with-xrefsystemtextjsonutf8jsonreader"></a>读取时 <xref:System.Text.Json.Utf8JsonReader>

如果要使用读取自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式 <xref:System.Text.Json.Utf8JsonReader> ，可以使用获取当前 JSON 标记的值 <xref:System.String> <xref:System.Text.Json.Utf8JsonReader.GetString> ，然后使用自定义逻辑分析该值。

下面的示例演示如何 <xref:System.DateTimeOffset> 使用检索自定义文本表示形式 <xref:System.Text.Json.Utf8JsonReader.GetString> ，然后使用进行分析 <xref:System.DateTimeOffset.ParseExact(System.String,System.String,System.IFormatProvider)> ：

[!code-csharp[example-custom-reading-with-utf8jsonreader](~/samples/snippets/standard/datetime/json/csharp/custom-reading-with-utf8jsonreader/Program.cs)]

## <a name="the-extended-iso-8601-12019-profile-in-systemtextjson"></a>System.Text.Js上的扩展 ISO 8601-1:2019 配置文件

### <a name="date-and-time-components"></a>日期和时间组件

在中实现的扩展 ISO 8601-1:2019 配置文件 <xref:System.Text.Json> 定义了日期和时间表示的以下组件。 这些组件用于定义分析和格式设置 <xref:System.DateTime> 和表示形式时支持的各种粒度级别 <xref:System.DateTimeOffset> 。

| 组件       | 格式                      | 描述                                                                     |
|-----------------|-----------------------------|---------------------------------------------------------------------------------|
| 年龄            | “yyyy”                      | 0001-9999                                                                       |
| 月份           | “MM”                        | 01-12                                                                           |
| 日期             | “dd”                        | 01-28、01-29、01-30、01-31 （基于月份/年份）                                  |
| 小时            | “HH”                        | 00-23                                                                           |
| Minute          | “mm”                        | 00-59                                                                           |
| 秒          | “ss”                        | 00-59                                                                           |
| 第二部分 | “FFFFFFF”                   | 至少一个数字，最多为16位                                      |
| 时间偏移     | “K”                         | "Z" 或 " (" + "/"-") HH"： "mm"                                                |
| 部分时间    | "HH"： "mm"： "ss [FFFFFFF]"     | 不带 UTC 偏移信息的时间                                             |
| 完整日期       | "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"            | 日历日期                                                                   |
| 全部时间       | "' Partial time'K"           | 一天中的 UTC 或本地时间与本地时间和 UTC 之间的时间偏移量 |
| 日期时间       | "" 完整时间 "不是" "完整时间" " | 日历日期和当天的时间，例如 2019-07-26T16：59： 57-05：00                   |

### <a name="support-for-parsing"></a>支持分析

定义以下级别的粒度：

1. "Full date"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"

2. "" 完整日期 "不是" "小时" "：" 分钟 "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"

3. "" 完整日期 "不是" "部分时间" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" (可 [排序 ( "s" ) 格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)) 
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF

4. "" 完整日期 "不是" "，时间小时" "：" "分钟" "时间偏移" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "mmZ"
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM (" + "/"-") HH"： "MM"

5. "日期时间"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"
    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"
    3. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss (" + "/"-") HH"： "MM"
    4. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF ( "+"/"-" ) HH "：" mm "

    此粒度级别符合 [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6)，它是用于交换日期和时间信息的广泛采用的 ISO 8601 配置文件。 不过，System.Text.Js的实现有一些限制。

    - RFC 3339 不指定最大的小数位数，但指定如果存在分数第二部分，则至少有一个数字必须在句点之后。 System.Text.Js上的实现允许多达16位 (支持与其他编程语言和框架) 互操作，但仅分析前七个。 <xref:System.Text.Json.JsonException>如果读取和实例时有超过16个小数位数，则会引发 `DateTime` `DateTimeOffset` 。
    - RFC 3339 允许 "T" 和 "Z" 字符分别为 "t" 或 "z"，但允许应用程序将支持限制为仅大写形式。 System.Text.Js上的实现要求它们为 "T" 和 "Z"。 <xref:System.Text.Json.JsonException>如果输入负载在读取和实例时包含 "t" 或 "z"，则将引发 `DateTime` `DateTimeOffset` 。
    - RFC 3339 指定日期和时间部分由 "T" 分隔，但允许应用程序使用空格分隔它们 ( "" ) 。 System.Text.Js要求将日期和时间部分与 "T" 分隔开。 <xref:System.Text.Json.JsonException>如果输入负载包含空格 ( "" ) 读取和实例时，将引发 `DateTime` `DateTimeOffset` 。

如果有秒的小数部分，则必须至少有一个数字; `2019-07-26T00:00:00.` 不允许。
尽管允许最多包含16个小数位，但仅分析前7个小数。 超出的任何内容都将被视为零。
例如， `2019-07-26T00:00:00.1234567890` 将按原样进行分析 `2019-07-26T00:00:00.1234567` 。
这是为了保持与实现的兼容性 <xref:System.DateTime> ，这仅限于此解决方案。

不支持闰秒。

### <a name="support-for-formatting"></a>支持格式设置

为格式化定义了下列粒度级别：

1. "" 完整日期 "不是" "部分时间" "
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" (可 [排序 ( "s" ) 格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)) 

        用于设置 <xref:System.DateTime> 不包含秒的小数部分和不含偏移量信息的格式。

    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF

        用于设置 <xref:System.DateTime> 带小数秒但不含偏移信息的的格式。

2. "日期时间"
    1. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"

        用于设置不包含 <xref:System.DateTime> 秒的小数部分，但具有 UTC 偏移量。

    2. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"

        用于设置 <xref:System.DateTime> 带有秒小数部分和 UTC 偏移量的。

    3. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss (" + "/"-") HH"： "MM"

        用于设置 <xref:System.DateTime> 或不包含 <xref:System.DateTimeOffset> 秒的小数部分，但使用本地偏移量。

    4. "yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF ( "+"/"-" ) HH "：" mm "

        用于设置 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 的小数部分或局部偏移量的格式。

    此粒度级别与 [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6)兼容。

如果或实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式 <xref:System.DateTime> <xref:System.DateTimeOffset> 在其小数部分中有尾随零，则 <xref:System.Text.Json.JsonSerializer> 和将在 <xref:System.Text.Json.Utf8JsonWriter> 没有尾随零的情况下格式化实例的表示形式。
例如，如果 <xref:System.DateTime> 实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式为，则 `2019-04-24T14:50:17.1010000Z` 将 `2019-04-24T14:50:17.101Z` 按和格式化 <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.Utf8JsonWriter> 。

如果或实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式 <xref:System.DateTime> <xref:System.DateTimeOffset> 在其小数部分中均为零，则 <xref:System.Text.Json.JsonSerializer> 和将在 <xref:System.Text.Json.Utf8JsonWriter> 没有秒小数部分的情况下格式化实例的表示形式。
例如，如果 <xref:System.DateTime> 实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式为，则 `2019-04-24T14:50:17.0000000+02:00` 将 `2019-04-24T14:50:17+02:00` 按和格式化 <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.Utf8JsonWriter> 。

如果截断秒的小数部分数字，则可以使用这些最小的输出来保留往返行程中的信息。

最多只能写入7个小数位数。 这与 <xref:System.DateTime> 实现（仅限于此解决方案）一致。
