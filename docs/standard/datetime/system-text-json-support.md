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
ms.openlocfilehash: 77b83fae039321a2fe6ef6279a7cab8886484e23
ms.sourcegitcommit: 02cc87f02c46e603ea5925de95af746b7ab46a35
ms.translationtype: MT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/25/2021
ms.locfileid: "107954778"
---
# <a name="datetime-and-datetimeoffset-support-in-systemtextjson"></a><span data-ttu-id="082d1-103">System.Text.Json 中的 DateTime 和 DateTimeOffset 支持</span><span class="sxs-lookup"><span data-stu-id="082d1-103">DateTime and DateTimeOffset support in System.Text.Json</span></span>

<span data-ttu-id="082d1-104">库中的 System.Text.Js<xref:System.DateTime> <xref:System.DateTimeOffset> 根据 ISO 8601:-2019 扩展配置文件分析并写入和值。</span><span class="sxs-lookup"><span data-stu-id="082d1-104">The System.Text.Json library parses and writes <xref:System.DateTime> and <xref:System.DateTimeOffset> values according to the ISO 8601:-2019 extended profile.</span></span>
<span data-ttu-id="082d1-105">[转换器](xref:System.Text.Json.Serialization.JsonConverter%601) 为序列化和反序列化提供自定义支持 <xref:System.Text.Json.JsonSerializer> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-105">[Converters](xref:System.Text.Json.Serialization.JsonConverter%601) provide custom support for serializing and deserializing with <xref:System.Text.Json.JsonSerializer>.</span></span>
<span data-ttu-id="082d1-106">当使用和时，还可以实现自定义支持 <xref:System.Text.Json.Utf8JsonReader> <xref:System.Text.Json.Utf8JsonWriter> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-106">Custom support can also be implemented when using <xref:System.Text.Json.Utf8JsonReader> and <xref:System.Text.Json.Utf8JsonWriter>.</span></span>

## <a name="support-for-the-iso-8601-12019-format"></a><span data-ttu-id="082d1-107">支持 ISO 8601-1:2019 格式</span><span class="sxs-lookup"><span data-stu-id="082d1-107">Support for the ISO 8601-1:2019 format</span></span>

<span data-ttu-id="082d1-108"><xref:System.Text.Json.JsonSerializer>、、 <xref:System.Text.Json.Utf8JsonReader> <xref:System.Text.Json.Utf8JsonWriter> 和 <xref:System.Text.Json.JsonElement> 类型 <xref:System.DateTime> <xref:System.DateTimeOffset> 根据 ISO 8601-1:2019 格式的扩展配置文件分析和写入和文本表示形式; 例如，26T16：59： 57-05：00。</span><span class="sxs-lookup"><span data-stu-id="082d1-108">The <xref:System.Text.Json.JsonSerializer>, <xref:System.Text.Json.Utf8JsonReader>, <xref:System.Text.Json.Utf8JsonWriter>, and <xref:System.Text.Json.JsonElement> types parse and write <xref:System.DateTime> and <xref:System.DateTimeOffset> text representations according to the extended profile of the ISO 8601-1:2019 format; for example, 2019-07-26T16:59:57-05:00.</span></span>

<span data-ttu-id="082d1-109"><xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据可通过以下方式序列化 <xref:System.Text.Json.JsonSerializer> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-109"><xref:System.DateTime> and <xref:System.DateTimeOffset> data can be serialized with <xref:System.Text.Json.JsonSerializer>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/serializing-with-jsonserializer/Program.cs":::

<span data-ttu-id="082d1-110">还可以通过以下方式对其进行反序列化 <xref:System.Text.Json.JsonSerializer> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-110">They can also be deserialized with <xref:System.Text.Json.JsonSerializer>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/deserializing-with-jsonserializer-valid/Program.cs":::

<span data-ttu-id="082d1-111">对于默认选项，输入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 文本表示形式必须符合扩展 ISO 8601-1:2019 配置文件。</span><span class="sxs-lookup"><span data-stu-id="082d1-111">With default options, input <xref:System.DateTime> and <xref:System.DateTimeOffset> text representations must conform to the extended ISO 8601-1:2019 profile.</span></span>
<span data-ttu-id="082d1-112">尝试对不符合配置文件的表示形式进行反序列化将导致 <xref:System.Text.Json.JsonSerializer> 引发 <xref:System.Text.Json.JsonException> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-112">Attempting to deserialize representations that don't conform to the profile will cause <xref:System.Text.Json.JsonSerializer> to throw a <xref:System.Text.Json.JsonException>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/deserializing-with-jsonserializer-error/Program.cs":::

<span data-ttu-id="082d1-113"><xref:System.Text.Json.JsonDocument>提供对 JSON 有效负载（包括 <xref:System.DateTime> 和表示形式）内容的结构化访问 <xref:System.DateTimeOffset> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-113">The <xref:System.Text.Json.JsonDocument> provides structured access to the contents of a JSON payload, including <xref:System.DateTime> and <xref:System.DateTimeOffset> representations.</span></span> <span data-ttu-id="082d1-114">下面的示例演示了当给定温度集合时，可以计算星期一的平均温度：</span><span class="sxs-lookup"><span data-stu-id="082d1-114">The example below shows how, when given a collection of temperatures, the average temperature on Mondays can be calculated:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/computing-with-jsondocument-valid/Program.cs":::

<span data-ttu-id="082d1-115">尝试使用不符合表示形式的有效负载来计算平均温度 <xref:System.DateTime> 将导致 <xref:System.Text.Json.JsonDocument> 引发 <xref:System.FormatException> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-115">Attempting to compute the average temperature given a payload with non-compliant <xref:System.DateTime> representations will cause <xref:System.Text.Json.JsonDocument> to throw a <xref:System.FormatException>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/computing-with-jsondocument-error/Program.cs":::

<span data-ttu-id="082d1-116">较低级别的 <xref:System.Text.Json.Utf8JsonWriter> 写入 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：</span><span class="sxs-lookup"><span data-stu-id="082d1-116">The lower level <xref:System.Text.Json.Utf8JsonWriter> writes <xref:System.DateTime> and <xref:System.DateTimeOffset> data:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/writing-with-utf8jsonwriter/Program.cs":::

<span data-ttu-id="082d1-117"><xref:System.Text.Json.Utf8JsonReader> 分析 <xref:System.DateTime> 和 <xref:System.DateTimeOffset> 数据：</span><span class="sxs-lookup"><span data-stu-id="082d1-117"><xref:System.Text.Json.Utf8JsonReader> parses <xref:System.DateTime> and <xref:System.DateTimeOffset> data:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/reading-with-utf8jsonreader-valid/Program.cs":::

<span data-ttu-id="082d1-118">尝试使用读取不符合格式 <xref:System.Text.Json.Utf8JsonReader> 将导致其引发 <xref:System.FormatException> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-118">Attempting to read non-compliant formats with <xref:System.Text.Json.Utf8JsonReader> will cause it to throw a <xref:System.FormatException>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/reading-with-utf8jsonreader-error/Program.cs":::

## <a name="custom-support-for-xrefsystemdatetime-and-xrefsystemdatetimeoffset"></a><span data-ttu-id="082d1-119">对和的自定义支持 <xref:System.DateTime><xref:System.DateTimeOffset></span><span class="sxs-lookup"><span data-stu-id="082d1-119">Custom support for <xref:System.DateTime> and <xref:System.DateTimeOffset></span></span>

### <a name="when-using-xrefsystemtextjsonjsonserializer"></a><span data-ttu-id="082d1-120">当使用 <xref:System.Text.Json.JsonSerializer></span><span class="sxs-lookup"><span data-stu-id="082d1-120">When using <xref:System.Text.Json.JsonSerializer></span></span>

<span data-ttu-id="082d1-121">如果希望序列化程序执行自定义分析或格式设置，可以实现 [自定义转换器](xref:System.Text.Json.Serialization.JsonConverter%601)。</span><span class="sxs-lookup"><span data-stu-id="082d1-121">If you want the serializer to perform custom parsing or formatting, you can implement [custom converters](xref:System.Text.Json.Serialization.JsonConverter%601).</span></span>
<span data-ttu-id="082d1-122">以下是一些示例：</span><span class="sxs-lookup"><span data-stu-id="082d1-122">Here are a few examples:</span></span>

#### <a name="using-datetimeoffsetparse-and-datetimeoffsettostring"></a><span data-ttu-id="082d1-123">使用 `DateTime(Offset).Parse` 和 `DateTime(Offset).ToString`</span><span class="sxs-lookup"><span data-stu-id="082d1-123">Using `DateTime(Offset).Parse` and `DateTime(Offset).ToString`</span></span>

<span data-ttu-id="082d1-124">如果无法确定输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式的格式，则可以使用 `DateTime(Offset).Parse` 转换器读取逻辑中的方法。</span><span class="sxs-lookup"><span data-stu-id="082d1-124">If you can't determine the formats of your input <xref:System.DateTime> or <xref:System.DateTimeOffset> text representations, you can use the `DateTime(Offset).Parse` method in your converter read logic.</span></span> <span data-ttu-id="082d1-125">这允许使用。NET 对分析各种 <xref:System.DateTime> 和文本格式的广泛支持 <xref:System.DateTimeOffset> ，包括非 iso 8601 字符串和不符合扩展 iso 8601-1:2019 配置文件的 iso 8601 格式。</span><span class="sxs-lookup"><span data-stu-id="082d1-125">This allows you to use .NET's extensive support for parsing various <xref:System.DateTime> and <xref:System.DateTimeOffset> text formats, including non-ISO 8601 strings and ISO 8601 formats that don't conform to the extended ISO 8601-1:2019 profile.</span></span> <span data-ttu-id="082d1-126">与使用序列化程序的本机实现相比，此方法的性能大大降低。</span><span class="sxs-lookup"><span data-stu-id="082d1-126">This approach is significantly less performant than using the serializer's native implementation.</span></span>

<span data-ttu-id="082d1-127">对于序列化，可以 `DateTime(Offset).ToString` 在转换器写入逻辑中使用方法。</span><span class="sxs-lookup"><span data-stu-id="082d1-127">For serializing, you can use the `DateTime(Offset).ToString` method in your converter write logic.</span></span> <span data-ttu-id="082d1-128">这使您可以 <xref:System.DateTime> <xref:System.DateTimeOffset> 使用任何 [标准日期和时间格式](../base-types/standard-date-and-time-format-strings.md)以及 [自定义日期和时间格式](../base-types/custom-date-and-time-format-strings.md)来编写和值。</span><span class="sxs-lookup"><span data-stu-id="082d1-128">This allows you to write <xref:System.DateTime> and <xref:System.DateTimeOffset> values using any of the [standard date and time formats](../base-types/standard-date-and-time-format-strings.md), and the [custom date and time formats](../base-types/custom-date-and-time-format-strings.md).</span></span>
<span data-ttu-id="082d1-129">与使用序列化程序的本机实现相比，这种性能也明显降低。</span><span class="sxs-lookup"><span data-stu-id="082d1-129">This is also significantly less performant than using the serializer's native implementation.</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/datetime-converter-examples/example1/Program.cs":::

> [!NOTE]
> <span data-ttu-id="082d1-130">在实现时 <xref:System.Text.Json.Serialization.JsonConverter%601> ，和 `T` 为时 <xref:System.DateTime> ， `typeToConvert` 参数将始终为 `typeof(DateTime)` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-130">When implementing <xref:System.Text.Json.Serialization.JsonConverter%601>, and `T` is <xref:System.DateTime>, the `typeToConvert` parameter will always be `typeof(DateTime)`.</span></span>
<span data-ttu-id="082d1-131">参数可用于处理多态事例和使用泛型以高 `typeof(T)` 性能方式获取。</span><span class="sxs-lookup"><span data-stu-id="082d1-131">The parameter is useful for handling polymorphic cases and when using generics to get `typeof(T)` in a performant way.</span></span>

#### <a name="using-xrefsystembufferstextutf8parser-and-xrefsystembufferstextutf8formatter"></a><span data-ttu-id="082d1-132">使用 <xref:System.Buffers.Text.Utf8Parser> 和 <xref:System.Buffers.Text.Utf8Formatter></span><span class="sxs-lookup"><span data-stu-id="082d1-132">Using <xref:System.Buffers.Text.Utf8Parser> and <xref:System.Buffers.Text.Utf8Formatter></span></span>

<span data-ttu-id="082d1-133">如果输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式符合 "R"、"l"、"O" 或 "G" [标准日期和时间格式字符串](../base-types/standard-date-and-time-format-strings.md)之一，或者您想要根据这些格式中的一种进行写入，则可以在转换器逻辑中使用快速的基于 utf-8 的分析和格式设置方法。</span><span class="sxs-lookup"><span data-stu-id="082d1-133">You can use fast UTF-8-based parsing and formatting methods in your converter logic if your input <xref:System.DateTime> or <xref:System.DateTimeOffset> text representations are compliant with one of the "R", "l", "O", or "G" [standard date and time format strings](../base-types/standard-date-and-time-format-strings.md), or you want to write according to one of these formats.</span></span> <span data-ttu-id="082d1-134">这比使用和更快 `DateTime(Offset).Parse` `DateTime(Offset).ToString` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-134">This is much faster than using `DateTime(Offset).Parse` and `DateTime(Offset).ToString`.</span></span>

<span data-ttu-id="082d1-135">此示例演示一个自定义转换器，该转换器 <xref:System.DateTime> 根据 ["R" 标准格式](../base-types/standard-date-and-time-format-strings.md#the-rfc1123-r-r-format-specifier)对值进行序列化和反序列化：</span><span class="sxs-lookup"><span data-stu-id="082d1-135">This example shows a custom converter that serializes and deserializes <xref:System.DateTime> values according to [the "R" standard format](../base-types/standard-date-and-time-format-strings.md#the-rfc1123-r-r-format-specifier):</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/datetime-converter-examples/example2/Program.cs":::

> [!NOTE]
> <span data-ttu-id="082d1-136">"R" 标准格式的长度始终为29个字符。</span><span class="sxs-lookup"><span data-stu-id="082d1-136">The "R" standard format will always be 29 characters long.</span></span>
>
> <span data-ttu-id="082d1-137">"L" (小写 "L" ) 格式不附带其他 [标准日期和时间格式字符串](../base-types/standard-date-and-time-format-strings.md) ，因为仅和类型支持此格式 `Utf8Parser` `Utf8Formatter` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-137">The "l" (lowercase "L") format is not documented with the other [standard date and time format strings](../base-types/standard-date-and-time-format-strings.md) because it is supported only by the `Utf8Parser` and `Utf8Formatter` types.</span></span> <span data-ttu-id="082d1-138">格式为小写 RFC 1123 ("R" 格式的小写版本) ，例如： "thu，25 7 2019 月 06:36:07 gmt"。</span><span class="sxs-lookup"><span data-stu-id="082d1-138">The format is lowercase RFC 1123 (a lowercase version of the "R" format), for example: "thu, 25 jul 2019 06:36:07 gmt".</span></span>

#### <a name="using-datetimeoffsetparse-as-a-fallback-to-the-serializers-native-parsing"></a><span data-ttu-id="082d1-139">使用 `DateTime(Offset).Parse` 作为序列化程序的本机解析的回退</span><span class="sxs-lookup"><span data-stu-id="082d1-139">Using `DateTime(Offset).Parse` as a fallback to the serializer's native parsing</span></span>

<span data-ttu-id="082d1-140">如果你通常希望输入 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 数据符合扩展 ISO 8601-1:2019 配置文件，则可以使用序列化程序的本机分析逻辑。</span><span class="sxs-lookup"><span data-stu-id="082d1-140">If you generally expect your input <xref:System.DateTime> or <xref:System.DateTimeOffset> data to conform to the extended ISO 8601-1:2019 profile, you can use the serializer's native parsing logic.</span></span> <span data-ttu-id="082d1-141">您还可以实现一种回退机制。</span><span class="sxs-lookup"><span data-stu-id="082d1-141">You can also implement a fallback mechanism just in case.</span></span>
<span data-ttu-id="082d1-142">此示例显示，在无法 <xref:System.DateTime> 使用分析文本表示形式后 <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)> ，转换器使用成功分析数据 <xref:System.DateTime.Parse(System.String)> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-142">This example shows that, after failing to parse a <xref:System.DateTime> text representation using <xref:System.Text.Json.Utf8JsonReader.TryGetDateTime(System.DateTime@)>, the converter successfully parses the data using <xref:System.DateTime.Parse(System.String)>.</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/datetime-converter-examples/example3/Program.cs":::

#### <a name="using-unix-epoch-date-format"></a><span data-ttu-id="082d1-143">使用 Unix epoch 日期格式</span><span class="sxs-lookup"><span data-stu-id="082d1-143">Using Unix epoch date format</span></span>

<span data-ttu-id="082d1-144">以下转换器将处理带有或不带时区的 Unix epoch 格式 (值，如 `/Date(1590863400000-0700)/` 或 `/Date(1590863400000)/`) ：</span><span class="sxs-lookup"><span data-stu-id="082d1-144">The following converters handle Unix epoch format with or without time zone (values such as `/Date(1590863400000-0700)/` or `/Date(1590863400000)/`):</span></span>

:::code language="csharp" source="../serialization/snippets/system-text-json-how-to-5-0/csharp/CustomConverterUnixEpochDate.cs" id="ConverterOnly":::

:::code language="csharp" source="../serialization/snippets/system-text-json-how-to-5-0/csharp/CustomConverterUnixEpochDateNoZone.cs" id="ConverterOnly":::

### <a name="when-writing-with-xrefsystemtextjsonutf8jsonwriter"></a><span data-ttu-id="082d1-145">写入时 <xref:System.Text.Json.Utf8JsonWriter></span><span class="sxs-lookup"><span data-stu-id="082d1-145">When writing with <xref:System.Text.Json.Utf8JsonWriter></span></span>

<span data-ttu-id="082d1-146">如果要使用编写自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式 <xref:System.Text.Json.Utf8JsonWriter> ，可以将自定义表示形式设置为 <xref:System.String> 、 `ReadOnlySpan<Byte>` 、 `ReadOnlySpan<Char>` 或 <xref:System.Text.Json.JsonEncodedText> ，然后将其传递给相应的 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A?displayProperty=nameWithType> 或 <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A?displayProperty=nameWithType> 方法。</span><span class="sxs-lookup"><span data-stu-id="082d1-146">If you want to write a custom <xref:System.DateTime> or <xref:System.DateTimeOffset> text representation with <xref:System.Text.Json.Utf8JsonWriter>, you can format your custom representation to a <xref:System.String>, `ReadOnlySpan<Byte>`, `ReadOnlySpan<Char>`, or <xref:System.Text.Json.JsonEncodedText>, then pass it to the corresponding <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue%2A?displayProperty=nameWithType> or <xref:System.Text.Json.Utf8JsonWriter.WriteString%2A?displayProperty=nameWithType> method.</span></span>

<span data-ttu-id="082d1-147">下面的示例演示如何 <xref:System.DateTime> 使用创建自定义格式 <xref:System.DateTime.ToString(System.String,System.IFormatProvider)> ，然后使用方法编写该格式 <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue(System.String)> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-147">The following example shows how a custom <xref:System.DateTime> format can be created with <xref:System.DateTime.ToString(System.String,System.IFormatProvider)>, then written with the <xref:System.Text.Json.Utf8JsonWriter.WriteStringValue(System.String)> method:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/custom-writing-with-utf8jsonwriter/Program.cs":::

### <a name="when-reading-with-xrefsystemtextjsonutf8jsonreader"></a><span data-ttu-id="082d1-148">读取时 <xref:System.Text.Json.Utf8JsonReader></span><span class="sxs-lookup"><span data-stu-id="082d1-148">When reading with <xref:System.Text.Json.Utf8JsonReader></span></span>

<span data-ttu-id="082d1-149">如果要使用读取自定义 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 文本表示形式 <xref:System.Text.Json.Utf8JsonReader> ，可以使用获取当前 JSON 标记的值 <xref:System.String> <xref:System.Text.Json.Utf8JsonReader.GetString> ，然后使用自定义逻辑分析该值。</span><span class="sxs-lookup"><span data-stu-id="082d1-149">If you want to read a custom <xref:System.DateTime> or <xref:System.DateTimeOffset> text representation with <xref:System.Text.Json.Utf8JsonReader>, you can get the value of the current JSON token as a <xref:System.String> using <xref:System.Text.Json.Utf8JsonReader.GetString>, then parse the value using custom logic.</span></span>

<span data-ttu-id="082d1-150">下面的示例演示如何 <xref:System.DateTimeOffset> 使用检索自定义文本表示形式 <xref:System.Text.Json.Utf8JsonReader.GetString> ，然后使用进行分析 <xref:System.DateTimeOffset.ParseExact(System.String,System.String,System.IFormatProvider)> ：</span><span class="sxs-lookup"><span data-stu-id="082d1-150">The following example shows how a custom <xref:System.DateTimeOffset> text representation can be retrieved using <xref:System.Text.Json.Utf8JsonReader.GetString>, then parsed using <xref:System.DateTimeOffset.ParseExact(System.String,System.String,System.IFormatProvider)>:</span></span>

:::code language="csharp" source="snippets/system-text-json-support/csharp/custom-reading-with-utf8jsonreader/Program.cs":::

## <a name="the-extended-iso-8601-12019-profile-in-systemtextjson"></a><span data-ttu-id="082d1-151">System.Text.Js上的扩展 ISO 8601-1:2019 配置文件</span><span class="sxs-lookup"><span data-stu-id="082d1-151">The extended ISO 8601-1:2019 profile in System.Text.Json</span></span>

### <a name="date-and-time-components"></a><span data-ttu-id="082d1-152">日期和时间组件</span><span class="sxs-lookup"><span data-stu-id="082d1-152">Date and time components</span></span>

<span data-ttu-id="082d1-153">在中实现的扩展 ISO 8601-1:2019 配置文件 <xref:System.Text.Json> 定义了日期和时间表示的以下组件。</span><span class="sxs-lookup"><span data-stu-id="082d1-153">The extended ISO 8601-1:2019 profile implemented in <xref:System.Text.Json> defines the following components for date and time representations.</span></span> <span data-ttu-id="082d1-154">这些组件用于定义分析和格式设置 <xref:System.DateTime> 和表示形式时支持的各种粒度级别 <xref:System.DateTimeOffset> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-154">These components are used to define various levels of granularity supported when parsing and formatting <xref:System.DateTime> and <xref:System.DateTimeOffset> representations.</span></span>

| <span data-ttu-id="082d1-155">组件</span><span class="sxs-lookup"><span data-stu-id="082d1-155">Component</span></span>       | <span data-ttu-id="082d1-156">格式</span><span class="sxs-lookup"><span data-stu-id="082d1-156">Format</span></span>                      | <span data-ttu-id="082d1-157">说明</span><span class="sxs-lookup"><span data-stu-id="082d1-157">Description</span></span>                                                                     |
|-----------------|-----------------------------|---------------------------------------------------------------------------------|
| <span data-ttu-id="082d1-158">Year</span><span class="sxs-lookup"><span data-stu-id="082d1-158">Year</span></span>            | <span data-ttu-id="082d1-159">“yyyy”</span><span class="sxs-lookup"><span data-stu-id="082d1-159">"yyyy"</span></span>                      | <span data-ttu-id="082d1-160">0001-9999</span><span class="sxs-lookup"><span data-stu-id="082d1-160">0001-9999</span></span>                                                                       |
| <span data-ttu-id="082d1-161">月份</span><span class="sxs-lookup"><span data-stu-id="082d1-161">Month</span></span>           | <span data-ttu-id="082d1-162">“MM”</span><span class="sxs-lookup"><span data-stu-id="082d1-162">"MM"</span></span>                        | <span data-ttu-id="082d1-163">01-12</span><span class="sxs-lookup"><span data-stu-id="082d1-163">01-12</span></span>                                                                           |
| <span data-ttu-id="082d1-164">天</span><span class="sxs-lookup"><span data-stu-id="082d1-164">Day</span></span>             | <span data-ttu-id="082d1-165">“dd”</span><span class="sxs-lookup"><span data-stu-id="082d1-165">"dd"</span></span>                        | <span data-ttu-id="082d1-166">01-28、01-29、01-30、01-31 （基于月份/年份）</span><span class="sxs-lookup"><span data-stu-id="082d1-166">01-28, 01-29, 01-30, 01-31 based on month/year</span></span>                                  |
| <span data-ttu-id="082d1-167">小时</span><span class="sxs-lookup"><span data-stu-id="082d1-167">Hour</span></span>            | <span data-ttu-id="082d1-168">“HH”</span><span class="sxs-lookup"><span data-stu-id="082d1-168">"HH"</span></span>                        | <span data-ttu-id="082d1-169">00-23</span><span class="sxs-lookup"><span data-stu-id="082d1-169">00-23</span></span>                                                                           |
| <span data-ttu-id="082d1-170">Minute</span><span class="sxs-lookup"><span data-stu-id="082d1-170">Minute</span></span>          | <span data-ttu-id="082d1-171">“mm”</span><span class="sxs-lookup"><span data-stu-id="082d1-171">"mm"</span></span>                        | <span data-ttu-id="082d1-172">00-59</span><span class="sxs-lookup"><span data-stu-id="082d1-172">00-59</span></span>                                                                           |
| <span data-ttu-id="082d1-173">Second</span><span class="sxs-lookup"><span data-stu-id="082d1-173">Second</span></span>          | <span data-ttu-id="082d1-174">“ss”</span><span class="sxs-lookup"><span data-stu-id="082d1-174">"ss"</span></span>                        | <span data-ttu-id="082d1-175">00-59</span><span class="sxs-lookup"><span data-stu-id="082d1-175">00-59</span></span>                                                                           |
| <span data-ttu-id="082d1-176">第二部分</span><span class="sxs-lookup"><span data-stu-id="082d1-176">Second fraction</span></span> | <span data-ttu-id="082d1-177">“FFFFFFF”</span><span class="sxs-lookup"><span data-stu-id="082d1-177">"FFFFFFF"</span></span>                   | <span data-ttu-id="082d1-178">至少一个数字，最多为16位</span><span class="sxs-lookup"><span data-stu-id="082d1-178">Minimum of one digit, maximum of 16 digits</span></span>                                      |
| <span data-ttu-id="082d1-179">时间偏移</span><span class="sxs-lookup"><span data-stu-id="082d1-179">Time offset</span></span>     | <span data-ttu-id="082d1-180">“K”</span><span class="sxs-lookup"><span data-stu-id="082d1-180">"K"</span></span>                         | <span data-ttu-id="082d1-181">"Z" 或 " (" + "/"-") HH"： "mm"</span><span class="sxs-lookup"><span data-stu-id="082d1-181">Either "Z" or "('+'/'-')HH':'mm"</span></span>                                                |
| <span data-ttu-id="082d1-182">部分时间</span><span class="sxs-lookup"><span data-stu-id="082d1-182">Partial time</span></span>    | <span data-ttu-id="082d1-183">"HH"： "mm"： "ss [FFFFFFF]"</span><span class="sxs-lookup"><span data-stu-id="082d1-183">"HH':'mm':'ss[FFFFFFF]"</span></span>     | <span data-ttu-id="082d1-184">不带 UTC 偏移信息的时间</span><span class="sxs-lookup"><span data-stu-id="082d1-184">Time without UTC offset information</span></span>                                             |
| <span data-ttu-id="082d1-185">完整日期</span><span class="sxs-lookup"><span data-stu-id="082d1-185">Full date</span></span>       | <span data-ttu-id="082d1-186">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"</span><span class="sxs-lookup"><span data-stu-id="082d1-186">"yyyy'-'MM'-'dd"</span></span>            | <span data-ttu-id="082d1-187">日历日期</span><span class="sxs-lookup"><span data-stu-id="082d1-187">Calendar date</span></span>                                                                   |
| <span data-ttu-id="082d1-188">全部时间</span><span class="sxs-lookup"><span data-stu-id="082d1-188">Full time</span></span>       | <span data-ttu-id="082d1-189">"' Partial time'K"</span><span class="sxs-lookup"><span data-stu-id="082d1-189">"'Partial time'K"</span></span>           | <span data-ttu-id="082d1-190">一天中的 UTC 或本地时间与本地时间和 UTC 之间的时间偏移量</span><span class="sxs-lookup"><span data-stu-id="082d1-190">UTC of day or Local time of day with the time offset between local time and UTC</span></span> |
| <span data-ttu-id="082d1-191">日期时间</span><span class="sxs-lookup"><span data-stu-id="082d1-191">Date time</span></span>       | <span data-ttu-id="082d1-192">"" 完整时间 "不是" "完整时间" "</span><span class="sxs-lookup"><span data-stu-id="082d1-192">"'Full date''T''Full time'"</span></span> | <span data-ttu-id="082d1-193">日历日期和当天的时间，例如 2019-07-26T16：59： 57-05：00</span><span class="sxs-lookup"><span data-stu-id="082d1-193">Calendar date and time of day, e.g. 2019-07-26T16:59:57-05:00</span></span>                   |

### <a name="support-for-parsing"></a><span data-ttu-id="082d1-194">支持分析</span><span class="sxs-lookup"><span data-stu-id="082d1-194">Support for parsing</span></span>

<span data-ttu-id="082d1-195">定义以下级别的粒度：</span><span class="sxs-lookup"><span data-stu-id="082d1-195">The following levels of granularity are defined for parsing:</span></span>

1. <span data-ttu-id="082d1-196">"Full date"</span><span class="sxs-lookup"><span data-stu-id="082d1-196">'Full date'</span></span>
    1. <span data-ttu-id="082d1-197">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-yyyy'-'mm'-'dd"</span><span class="sxs-lookup"><span data-stu-id="082d1-197">"yyyy'-'MM'-'dd"</span></span>

2. <span data-ttu-id="082d1-198">"" 完整日期 "不是" "小时" "：" 分钟 "</span><span class="sxs-lookup"><span data-stu-id="082d1-198">"'Full date''T''Hour'':''Minute'"</span></span>
    1. <span data-ttu-id="082d1-199">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"</span><span class="sxs-lookup"><span data-stu-id="082d1-199">"yyyy'-'MM'-'dd'T'HH':'mm"</span></span>

3. <span data-ttu-id="082d1-200">"" 完整日期 "不是" "部分时间" "</span><span class="sxs-lookup"><span data-stu-id="082d1-200">"'Full date''T''Partial time'"</span></span>
    1. <span data-ttu-id="082d1-201">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" (可 [排序 ( "s" ) 格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)) </span><span class="sxs-lookup"><span data-stu-id="082d1-201">"yyyy'-'MM'-'dd'T'HH':'mm':'ss" ([The Sortable ("s") Format Specifier](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier))</span></span>
    2. <span data-ttu-id="082d1-202">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF</span><span class="sxs-lookup"><span data-stu-id="082d1-202">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF"</span></span>

4. <span data-ttu-id="082d1-203">"" 完整日期 "不是" "，时间小时" "：" "分钟" "时间偏移" "</span><span class="sxs-lookup"><span data-stu-id="082d1-203">"'Full date''T''Time hour'':''Minute''Time offset'"</span></span>
    1. <span data-ttu-id="082d1-204">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "mmZ"</span><span class="sxs-lookup"><span data-stu-id="082d1-204">"yyyy'-'MM'-'dd'T'HH':'mmZ"</span></span>
    2. <span data-ttu-id="082d1-205">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM (" + "/"-") HH"： "MM"</span><span class="sxs-lookup"><span data-stu-id="082d1-205">"yyyy'-'MM'-'dd'T'HH':'mm('+'/'-')HH':'mm"</span></span>

5. <span data-ttu-id="082d1-206">"日期时间"</span><span class="sxs-lookup"><span data-stu-id="082d1-206">'Date time'</span></span>
    1. <span data-ttu-id="082d1-207">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"</span><span class="sxs-lookup"><span data-stu-id="082d1-207">"yyyy'-'MM'-'dd'T'HH':'mm':'ssZ"</span></span>
    2. <span data-ttu-id="082d1-208">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"</span><span class="sxs-lookup"><span data-stu-id="082d1-208">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFFZ"</span></span>
    3. <span data-ttu-id="082d1-209">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss (" + "/"-") HH"： "MM"</span><span class="sxs-lookup"><span data-stu-id="082d1-209">"yyyy'-'MM'-'dd'T'HH':'mm':'ss('+'/'-')HH':'mm"</span></span>
    4. <span data-ttu-id="082d1-210">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF ( "+"/"-" ) HH "：" mm "</span><span class="sxs-lookup"><span data-stu-id="082d1-210">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF('+'/'-')HH':'mm"</span></span>

    <span data-ttu-id="082d1-211">此粒度级别符合 [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6)，它是用于交换日期和时间信息的广泛采用的 ISO 8601 配置文件。</span><span class="sxs-lookup"><span data-stu-id="082d1-211">This level of granularity is compliant with [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6), a widely adopted profile of ISO 8601 used for interchanging date and time information.</span></span> <span data-ttu-id="082d1-212">不过，System.Text.Js的实现有一些限制。</span><span class="sxs-lookup"><span data-stu-id="082d1-212">However, there are a few restrictions in the System.Text.Json implementation.</span></span>

    - <span data-ttu-id="082d1-213">RFC 3339 不指定最大的小数位数，但指定如果存在分数第二部分，则至少有一个数字必须在句点之后。</span><span class="sxs-lookup"><span data-stu-id="082d1-213">RFC 3339 does not specify a maximum number of fractional-second digits, but specifies that at least one digit must follow the period, if a fractional-second section is present.</span></span> <span data-ttu-id="082d1-214">System.Text.Js上的实现允许多达16位 (支持与其他编程语言和框架) 互操作，但仅分析前七个。</span><span class="sxs-lookup"><span data-stu-id="082d1-214">The implementation in System.Text.Json allows up to 16 digits (to support interop with other programming languages and frameworks), but parses only the first seven.</span></span> <span data-ttu-id="082d1-215"><xref:System.Text.Json.JsonException>如果读取和实例时有超过16个小数位数，则会引发 `DateTime` `DateTimeOffset` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-215">A <xref:System.Text.Json.JsonException> will be thrown if there are more than 16 fractional second digits when reading `DateTime` and `DateTimeOffset` instances.</span></span>
    - <span data-ttu-id="082d1-216">RFC 3339 允许 "T" 和 "Z" 字符分别为 "t" 或 "z"，但允许应用程序将支持限制为仅大写形式。</span><span class="sxs-lookup"><span data-stu-id="082d1-216">RFC 3339 allows the "T" and "Z" characters to be "t" or "z" respectively, but allows applications to limit support to just the upper-case variants.</span></span> <span data-ttu-id="082d1-217">System.Text.Js上的实现要求它们为 "T" 和 "Z"。</span><span class="sxs-lookup"><span data-stu-id="082d1-217">The implementation in System.Text.Json requires them to be "T" and "Z".</span></span> <span data-ttu-id="082d1-218"><xref:System.Text.Json.JsonException>如果输入负载在读取和实例时包含 "t" 或 "z"，则将引发 `DateTime` `DateTimeOffset` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-218">A <xref:System.Text.Json.JsonException> will be thrown if input payloads contain "t" or "z" when reading `DateTime` and `DateTimeOffset` instances.</span></span>
    - <span data-ttu-id="082d1-219">RFC 3339 指定日期和时间部分由 "T" 分隔，但允许应用程序使用空格分隔它们 ( "" ) 。</span><span class="sxs-lookup"><span data-stu-id="082d1-219">RFC 3339 specifies that the date and time sections are separated by "T", but allows applications to separate them by a space (" ") instead.</span></span> <span data-ttu-id="082d1-220">System.Text.Js要求将日期和时间部分与 "T" 分隔开。</span><span class="sxs-lookup"><span data-stu-id="082d1-220">System.Text.Json requires date and time sections to be separated with "T".</span></span> <span data-ttu-id="082d1-221"><xref:System.Text.Json.JsonException>如果输入负载包含空格 ( "" ) 读取和实例时，将引发 `DateTime` `DateTimeOffset` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-221">A <xref:System.Text.Json.JsonException> will be thrown if input payloads contain a space (" ") when reading `DateTime` and `DateTimeOffset` instances.</span></span>

<span data-ttu-id="082d1-222">如果有秒的小数部分，则必须至少有一个数字; `2019-07-26T00:00:00.` 不允许。</span><span class="sxs-lookup"><span data-stu-id="082d1-222">If there are decimal fractions for seconds, there must be at least one digit; `2019-07-26T00:00:00.` is not allowed.</span></span>
<span data-ttu-id="082d1-223">尽管允许最多包含16个小数位，但仅分析前7个小数。</span><span class="sxs-lookup"><span data-stu-id="082d1-223">While up to 16 fractional digits are allowed, only the first seven are parsed.</span></span> <span data-ttu-id="082d1-224">超出的任何内容都将被视为零。</span><span class="sxs-lookup"><span data-stu-id="082d1-224">Anything beyond that is considered a zero.</span></span>
<span data-ttu-id="082d1-225">例如， `2019-07-26T00:00:00.1234567890` 将按原样进行分析 `2019-07-26T00:00:00.1234567` 。</span><span class="sxs-lookup"><span data-stu-id="082d1-225">For example, `2019-07-26T00:00:00.1234567890` will be parsed as if it is `2019-07-26T00:00:00.1234567`.</span></span>
<span data-ttu-id="082d1-226">这是为了保持与实现的兼容性 <xref:System.DateTime> ，这仅限于此解决方案。</span><span class="sxs-lookup"><span data-stu-id="082d1-226">This is to stay compatible with the <xref:System.DateTime> implementation, which is limited to this resolution.</span></span>

<span data-ttu-id="082d1-227">不支持闰秒。</span><span class="sxs-lookup"><span data-stu-id="082d1-227">Leap seconds are not supported.</span></span>

### <a name="support-for-formatting"></a><span data-ttu-id="082d1-228">支持格式设置</span><span class="sxs-lookup"><span data-stu-id="082d1-228">Support for formatting</span></span>

<span data-ttu-id="082d1-229">为格式化定义了下列粒度级别：</span><span class="sxs-lookup"><span data-stu-id="082d1-229">The following levels of granularity are defined for formatting:</span></span>

1. <span data-ttu-id="082d1-230">"" 完整日期 "不是" "部分时间" "</span><span class="sxs-lookup"><span data-stu-id="082d1-230">"'Full date''T''Partial time'"</span></span>
    1. <span data-ttu-id="082d1-231">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss" (可 [排序 ( "s" ) 格式说明符](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier)) </span><span class="sxs-lookup"><span data-stu-id="082d1-231">"yyyy'-'MM'-'dd'T'HH':'mm':'ss"  ([The Sortable ("s") Format Specifier](../base-types/standard-date-and-time-format-strings.md#the-sortable-s-format-specifier))</span></span>

        <span data-ttu-id="082d1-232">用于设置 <xref:System.DateTime> 不包含秒的小数部分和不含偏移量信息的格式。</span><span class="sxs-lookup"><span data-stu-id="082d1-232">Used to format a <xref:System.DateTime> without fractional seconds and without offset information.</span></span>

    2. <span data-ttu-id="082d1-233">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF</span><span class="sxs-lookup"><span data-stu-id="082d1-233">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF"</span></span>

        <span data-ttu-id="082d1-234">用于设置 <xref:System.DateTime> 带小数秒但不含偏移信息的的格式。</span><span class="sxs-lookup"><span data-stu-id="082d1-234">Used to format a <xref:System.DateTime> with fractional seconds but without offset information.</span></span>

2. <span data-ttu-id="082d1-235">"日期时间"</span><span class="sxs-lookup"><span data-stu-id="082d1-235">'Date time'</span></span>
    1. <span data-ttu-id="082d1-236">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ssZ"</span><span class="sxs-lookup"><span data-stu-id="082d1-236">"yyyy'-'MM'-'dd'T'HH':'mm':'ssZ"</span></span>

        <span data-ttu-id="082d1-237">用于设置不包含 <xref:System.DateTime> 秒的小数部分，但具有 UTC 偏移量。</span><span class="sxs-lookup"><span data-stu-id="082d1-237">Used to format a <xref:System.DateTime> without fractional seconds but with a UTC offset.</span></span>

    2. <span data-ttu-id="082d1-238">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFFZ"</span><span class="sxs-lookup"><span data-stu-id="082d1-238">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFFZ"</span></span>

        <span data-ttu-id="082d1-239">用于设置 <xref:System.DateTime> 带有秒小数部分和 UTC 偏移量的。</span><span class="sxs-lookup"><span data-stu-id="082d1-239">Used to format a <xref:System.DateTime> with fractional seconds and with a UTC offset.</span></span>

    3. <span data-ttu-id="082d1-240">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss (" + "/"-") HH"： "MM"</span><span class="sxs-lookup"><span data-stu-id="082d1-240">"yyyy'-'MM'-'dd'T'HH':'mm':'ss('+'/'-')HH':'mm"</span></span>

        <span data-ttu-id="082d1-241">用于设置 <xref:System.DateTime> 或不包含 <xref:System.DateTimeOffset> 秒的小数部分，但使用本地偏移量。</span><span class="sxs-lookup"><span data-stu-id="082d1-241">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> without fractional seconds but with a local offset.</span></span>

    4. <span data-ttu-id="082d1-242">"yyyy'-'mm'-'dd't'hh-YYYY'-'MM'-'DD'T'HH-Yyyy'-'mm'-'dd't'hh"： "MM"： "ss"。FFFFFFF ( "+"/"-" ) HH "：" mm "</span><span class="sxs-lookup"><span data-stu-id="082d1-242">"yyyy'-'MM'-'dd'T'HH':'mm':'ss'.'FFFFFFF('+'/'-')HH':'mm"</span></span>

        <span data-ttu-id="082d1-243">用于设置 <xref:System.DateTime> 或 <xref:System.DateTimeOffset> 的小数部分或局部偏移量的格式。</span><span class="sxs-lookup"><span data-stu-id="082d1-243">Used to format a <xref:System.DateTime> or <xref:System.DateTimeOffset> with fractional seconds and with a local offset.</span></span>

    <span data-ttu-id="082d1-244">此粒度级别与 [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6)兼容。</span><span class="sxs-lookup"><span data-stu-id="082d1-244">This level of granularity is compliant with [RFC 3339](https://tools.ietf.org/html/rfc3339#section-5.6).</span></span>

<span data-ttu-id="082d1-245">如果或实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式 <xref:System.DateTime> <xref:System.DateTimeOffset> 在其小数部分中有尾随零，则 <xref:System.Text.Json.JsonSerializer> 和将在 <xref:System.Text.Json.Utf8JsonWriter> 没有尾随零的情况下格式化实例的表示形式。</span><span class="sxs-lookup"><span data-stu-id="082d1-245">If the [round-trip format](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) representation of a <xref:System.DateTime> or <xref:System.DateTimeOffset> instance has trailing zeros in its fractional seconds, then <xref:System.Text.Json.JsonSerializer> and <xref:System.Text.Json.Utf8JsonWriter> will format a representation of the instance without trailing zeros.</span></span>
<span data-ttu-id="082d1-246">例如，如果 <xref:System.DateTime> 实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式为，则 `2019-04-24T14:50:17.1010000Z` 将 `2019-04-24T14:50:17.101Z` 按和格式化 <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.Utf8JsonWriter> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-246">For example, a <xref:System.DateTime> instance whose [round-trip format](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) representation is `2019-04-24T14:50:17.1010000Z`, will be formatted as `2019-04-24T14:50:17.101Z` by <xref:System.Text.Json.JsonSerializer> and <xref:System.Text.Json.Utf8JsonWriter>.</span></span>

<span data-ttu-id="082d1-247">如果或实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式 <xref:System.DateTime> <xref:System.DateTimeOffset> 在其小数部分中均为零，则 <xref:System.Text.Json.JsonSerializer> 和将在 <xref:System.Text.Json.Utf8JsonWriter> 没有秒小数部分的情况下格式化实例的表示形式。</span><span class="sxs-lookup"><span data-stu-id="082d1-247">If the [round-trip format](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) representation of a <xref:System.DateTime> or <xref:System.DateTimeOffset> instance has all zeros in its fractional seconds, then <xref:System.Text.Json.JsonSerializer> and <xref:System.Text.Json.Utf8JsonWriter> will format a representation of the instance without fractional seconds.</span></span>
<span data-ttu-id="082d1-248">例如，如果 <xref:System.DateTime> 实例的 [往返格式](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) 表示形式为，则 `2019-04-24T14:50:17.0000000+02:00` 将 `2019-04-24T14:50:17+02:00` 按和格式化 <xref:System.Text.Json.JsonSerializer> <xref:System.Text.Json.Utf8JsonWriter> 。</span><span class="sxs-lookup"><span data-stu-id="082d1-248">For example, a <xref:System.DateTime> instance whose [round-trip format](../base-types/standard-date-and-time-format-strings.md#the-round-trip-o-o-format-specifier) representation is `2019-04-24T14:50:17.0000000+02:00`, will be formatted as `2019-04-24T14:50:17+02:00` by <xref:System.Text.Json.JsonSerializer> and <xref:System.Text.Json.Utf8JsonWriter>.</span></span>

<span data-ttu-id="082d1-249">如果截断秒的小数部分数字，则可以使用这些最小的输出来保留往返行程中的信息。</span><span class="sxs-lookup"><span data-stu-id="082d1-249">Truncating zeros in fractional-second digits allows the smallest output needed to preserve information on a round trip to be written.</span></span>

<span data-ttu-id="082d1-250">最多只能写入7个小数位数。</span><span class="sxs-lookup"><span data-stu-id="082d1-250">A maximum of 7 fractional-second digits are written.</span></span> <span data-ttu-id="082d1-251">这与 <xref:System.DateTime> 实现（仅限于此解决方案）一致。</span><span class="sxs-lookup"><span data-stu-id="082d1-251">This aligns with the <xref:System.DateTime> implementation, which is limited to this resolution.</span></span>
