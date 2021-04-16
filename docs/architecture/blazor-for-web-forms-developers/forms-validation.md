---
title: 窗体和验证
description: 了解如何在 Blazor 中使用客户端侧验证生成窗体。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
- Blazor WebAssembly
ms.date: 09/19/2019
ms.openlocfilehash: d2dce23996e996a736b04c9cdd1ccf3b549ff3ff
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "88267550"
---
# <a name="forms-and-validation"></a>窗体和验证

ASP.NET Web Forms 框架包括一组验证服务器控件，用于处理验证输入窗体（`RequiredFieldValidator``CompareValidator``RangeValidator` 等）的用户输入。 ASP.NET Web Forms 框架还支持基于数据注释（`[Required]`、`[StringLength]`、`[Range]` 等）的模型绑定和验证。 可以在服务器和客户端上使用基于 JavaScript 的非介式入验证来强制执行验证逻辑。 `ValidationSummary` 服务器控件用于向用户显示验证错误的摘要。

Blazor 支持在客户端和服务器之间共享验证逻辑。 ASP.NET 提供许多常见服务器验证的预构建 JavaScript 实现。 在很多情况下，开发人员仍需要编写 JavaScript 才能完全实现特定于应用的验证逻辑。 可以在服务器和客户端上使用相同的模型类型、数据注释和验证逻辑。

Blazor 提供一组输入组件。 输入组件负责将字段数据绑定到模型，并在提交窗体时验证用户输入。

|输入组件|呈现的 HTML 元素    |
|---------------|-------------------------|
|`InputCheckbox`|`<input type="checkbox">`|
|`InputDate`    |`<input type="date">`    |
|`InputNumber`  |`<input type="number">`  |
|`InputSelect`  |`<select>`               |
|`InputText`    |`<input>`                |
|`InputTextArea`|`<textarea>`             |

`EditForm` 组件包装这些输入组件，并通过 `EditContext` 协调验证过程。 创建 `EditForm` 时，可以使用 `Model` 参数指定要绑定的模型实例。 验证通常是使用数据注释完成的，并且可以进行扩展。 若要启用基于数据注释的验证，请将 `DataAnnotationsValidator` 组件添加为 `EditForm` 的子组件。 `EditForm` 组件提供了一个方便的事件，用于处理有效的 (`OnValidSubmit`) 和无效的 (`OnInvalidSubmit`) 提交。 还有一个更通用的 `OnSubmit` 事件，可以让你自行触发和处理验证。

若要显示验证错误摘要，请使用 `ValidationSummary` 组件。 若要显示特定输入字段的验证消息，请使用 `ValidationMessage` 组件，并为指向相应模型成员的 `For` 参数指定 lambda 表达式。

以下模型类型使用数据注释定义了多个验证规则：

```csharp
using System;
using System.ComponentModel.DataAnnotations;

public class Starship
{
    [Required]
    [StringLength(16,
        ErrorMessage = "Identifier too long (16 character limit).")]
    public string Identifier { get; set; }

    public string Description { get; set; }

    [Required]
    public string Classification { get; set; }

    [Range(1, 100000,
        ErrorMessage = "Accommodation invalid (1-100000).")]
    public int MaximumAccommodation { get; set; }

    [Required]
    [Range(typeof(bool), "true", "true",
        ErrorMessage = "This form disallows unapproved ships.")]
    public bool IsValidatedDesign { get; set; }

    [Required]
    public DateTime ProductionDate { get; set; }
}
```

以下组件展示了如何基于 `Starship` 模型类型在 Blazor 中构建窗体：

```razor
<h1>New Ship Entry Form</h1>

<EditForm Model="@starship" OnValidSubmit="@HandleValidSubmit">
    <DataAnnotationsValidator />
    <ValidationSummary />

    <p>
        <label for="identifier">Identifier: </label>
        <InputText id="identifier" @bind-Value="starship.Identifier" />
        <ValidationMessage For="() => starship.Identifier" />
    </p>
    <p>
        <label for="description">Description (optional): </label>
        <InputTextArea id="description" @bind-Value="starship.Description" />
    </p>
    <p>
        <label for="classification">Primary Classification: </label>
        <InputSelect id="classification" @bind-Value="starship.Classification">
            <option value="">Select classification ...</option>
            <option value="Exploration">Exploration</option>
            <option value="Diplomacy">Diplomacy</option>
            <option value="Defense">Defense</option>
        </InputSelect>
        <ValidationMessage For="() => starship.Classification" />
    </p>
    <p>
        <label for="accommodation">Maximum Accommodation: </label>
        <InputNumber id="accommodation" @bind-Value="starship.MaximumAccommodation" />
        <ValidationMessage For="() => starship.MaximumAccommodation" />
    </p>
    <p>
        <label for="valid">Engineering Approval: </label>
        <InputCheckbox id="valid" @bind-Value="starship.IsValidatedDesign" />
        <ValidationMessage For="() => starship.IsValidatedDesign" />
    </p>
    <p>
        <label for="productionDate">Production Date: </label>
        <InputDate id="productionDate" @bind-Value="starship.ProductionDate" />
        <ValidationMessage For="() => starship.ProductionDate" />
    </p>

    <button type="submit">Submit</button>
</EditForm>

@code {
    private Starship starship = new Starship();

    private void HandleValidSubmit()
    {
        // Save the data
    }
}
```

提交窗体后，不会将模型绑定的数据保存到任何数据存储（例如数据库）。 在 Blazor WebAssembly 应用中，必须将数据发送到服务器。 例如，使用 HTTP POST 请求。 在 Blazor 服务器应用中，数据已存在于服务器上，但必须进行持久化处理。 处理 Blazor 应用中的数据访问是[处理数据](data.md)部分的主题。

## <a name="additional-resources"></a>其他资源

有关 Blazor 应用中[窗体和验证](/aspnet/core/blazor/forms-validation)的详细信息，请参阅 Blazor 文档。

>[!div class="step-by-step"]
>[上一页](state-management.md)
>[下一页](data.md)
