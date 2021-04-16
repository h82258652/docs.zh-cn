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
# <a name="forms-and-validation"></a><span data-ttu-id="8c51c-103">窗体和验证</span><span class="sxs-lookup"><span data-stu-id="8c51c-103">Forms and validation</span></span>

<span data-ttu-id="8c51c-104">ASP.NET Web Forms 框架包括一组验证服务器控件，用于处理验证输入窗体（`RequiredFieldValidator``CompareValidator``RangeValidator` 等）的用户输入。</span><span class="sxs-lookup"><span data-stu-id="8c51c-104">The ASP.NET Web Forms framework includes a set of validation server controls that handle validating user input entered into a form (`RequiredFieldValidator`, `CompareValidator`, `RangeValidator`, and so on).</span></span> <span data-ttu-id="8c51c-105">ASP.NET Web Forms 框架还支持基于数据注释（`[Required]`、`[StringLength]`、`[Range]` 等）的模型绑定和验证。</span><span class="sxs-lookup"><span data-stu-id="8c51c-105">The ASP.NET Web Forms framework also supports model binding and validating the model based on data annotations (`[Required]`, `[StringLength]`, `[Range]`, and so on).</span></span> <span data-ttu-id="8c51c-106">可以在服务器和客户端上使用基于 JavaScript 的非介式入验证来强制执行验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="8c51c-106">The validation logic can be enforced both on the server and on the client using unobtrusive JavaScript-based validation.</span></span> <span data-ttu-id="8c51c-107">`ValidationSummary` 服务器控件用于向用户显示验证错误的摘要。</span><span class="sxs-lookup"><span data-stu-id="8c51c-107">The `ValidationSummary` server control is used to display a summary of the validation errors to the user.</span></span>

<span data-ttu-id="8c51c-108">Blazor 支持在客户端和服务器之间共享验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="8c51c-108">Blazor supports the sharing of validation logic between both the client and the server.</span></span> <span data-ttu-id="8c51c-109">ASP.NET 提供许多常见服务器验证的预构建 JavaScript 实现。</span><span class="sxs-lookup"><span data-stu-id="8c51c-109">ASP.NET provides pre-built JavaScript implementations of many common server validations.</span></span> <span data-ttu-id="8c51c-110">在很多情况下，开发人员仍需要编写 JavaScript 才能完全实现特定于应用的验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="8c51c-110">In many cases, the developer still has to write JavaScript to fully implement their app-specific validation logic.</span></span> <span data-ttu-id="8c51c-111">可以在服务器和客户端上使用相同的模型类型、数据注释和验证逻辑。</span><span class="sxs-lookup"><span data-stu-id="8c51c-111">The same model types, data annotations, and validation logic can be used on both the server and client.</span></span>

<span data-ttu-id="8c51c-112">Blazor 提供一组输入组件。</span><span class="sxs-lookup"><span data-stu-id="8c51c-112">Blazor provides a set of input components.</span></span> <span data-ttu-id="8c51c-113">输入组件负责将字段数据绑定到模型，并在提交窗体时验证用户输入。</span><span class="sxs-lookup"><span data-stu-id="8c51c-113">The input components handle binding field data to a model and validating the user input when the form is submitted.</span></span>

|<span data-ttu-id="8c51c-114">输入组件</span><span class="sxs-lookup"><span data-stu-id="8c51c-114">Input component</span></span>|<span data-ttu-id="8c51c-115">呈现的 HTML 元素</span><span class="sxs-lookup"><span data-stu-id="8c51c-115">Rendered HTML element</span></span>    |
|---------------|-------------------------|
|`InputCheckbox`|`<input type="checkbox">`|
|`InputDate`    |`<input type="date">`    |
|`InputNumber`  |`<input type="number">`  |
|`InputSelect`  |`<select>`               |
|`InputText`    |`<input>`                |
|`InputTextArea`|`<textarea>`             |

<span data-ttu-id="8c51c-116">`EditForm` 组件包装这些输入组件，并通过 `EditContext` 协调验证过程。</span><span class="sxs-lookup"><span data-stu-id="8c51c-116">The `EditForm` component wraps these input components and orchestrates the validation process through an `EditContext`.</span></span> <span data-ttu-id="8c51c-117">创建 `EditForm` 时，可以使用 `Model` 参数指定要绑定的模型实例。</span><span class="sxs-lookup"><span data-stu-id="8c51c-117">When creating an `EditForm`, you specify what model instance to bind to using the `Model` parameter.</span></span> <span data-ttu-id="8c51c-118">验证通常是使用数据注释完成的，并且可以进行扩展。</span><span class="sxs-lookup"><span data-stu-id="8c51c-118">Validation is typically done using data annotations, and it's extensible.</span></span> <span data-ttu-id="8c51c-119">若要启用基于数据注释的验证，请将 `DataAnnotationsValidator` 组件添加为 `EditForm` 的子组件。</span><span class="sxs-lookup"><span data-stu-id="8c51c-119">To enable data annotation-based validation, add the `DataAnnotationsValidator` component as a child of the `EditForm`.</span></span> <span data-ttu-id="8c51c-120">`EditForm` 组件提供了一个方便的事件，用于处理有效的 (`OnValidSubmit`) 和无效的 (`OnInvalidSubmit`) 提交。</span><span class="sxs-lookup"><span data-stu-id="8c51c-120">The `EditForm` component provides a convenient event for handling valid (`OnValidSubmit`) and invalid (`OnInvalidSubmit`) submissions.</span></span> <span data-ttu-id="8c51c-121">还有一个更通用的 `OnSubmit` 事件，可以让你自行触发和处理验证。</span><span class="sxs-lookup"><span data-stu-id="8c51c-121">There's also a more generic `OnSubmit` event that lets you trigger and handle the validation yourself.</span></span>

<span data-ttu-id="8c51c-122">若要显示验证错误摘要，请使用 `ValidationSummary` 组件。</span><span class="sxs-lookup"><span data-stu-id="8c51c-122">To display a validation error summary, use the `ValidationSummary` component.</span></span> <span data-ttu-id="8c51c-123">若要显示特定输入字段的验证消息，请使用 `ValidationMessage` 组件，并为指向相应模型成员的 `For` 参数指定 lambda 表达式。</span><span class="sxs-lookup"><span data-stu-id="8c51c-123">To display validation messages for a specific input field, use the `ValidationMessage` component, specifying a lambda expression for the `For` parameter that points to the appropriate model member.</span></span>

<span data-ttu-id="8c51c-124">以下模型类型使用数据注释定义了多个验证规则：</span><span class="sxs-lookup"><span data-stu-id="8c51c-124">The following model type defines several validation rules using data annotations:</span></span>

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

<span data-ttu-id="8c51c-125">以下组件展示了如何基于 `Starship` 模型类型在 Blazor 中构建窗体：</span><span class="sxs-lookup"><span data-stu-id="8c51c-125">The following component demonstrates building a form in Blazor based on the `Starship` model type:</span></span>

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

<span data-ttu-id="8c51c-126">提交窗体后，不会将模型绑定的数据保存到任何数据存储（例如数据库）。</span><span class="sxs-lookup"><span data-stu-id="8c51c-126">After the form submission, the model-bound data hasn't been saved to any data store, like a database.</span></span> <span data-ttu-id="8c51c-127">在 Blazor WebAssembly 应用中，必须将数据发送到服务器。</span><span class="sxs-lookup"><span data-stu-id="8c51c-127">In a Blazor WebAssembly app, the data must be sent to the server.</span></span> <span data-ttu-id="8c51c-128">例如，使用 HTTP POST 请求。</span><span class="sxs-lookup"><span data-stu-id="8c51c-128">For example, using an HTTP POST request.</span></span> <span data-ttu-id="8c51c-129">在 Blazor 服务器应用中，数据已存在于服务器上，但必须进行持久化处理。</span><span class="sxs-lookup"><span data-stu-id="8c51c-129">In a Blazor Server app, the data is already on the server, but it must be persisted.</span></span> <span data-ttu-id="8c51c-130">处理 Blazor 应用中的数据访问是[处理数据](data.md)部分的主题。</span><span class="sxs-lookup"><span data-stu-id="8c51c-130">Handling data access in Blazor apps is the subject of the [Dealing with data](data.md) section.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="8c51c-131">其他资源</span><span class="sxs-lookup"><span data-stu-id="8c51c-131">Additional resources</span></span>

<span data-ttu-id="8c51c-132">有关 Blazor 应用中[窗体和验证](/aspnet/core/blazor/forms-validation)的详细信息，请参阅 Blazor 文档。</span><span class="sxs-lookup"><span data-stu-id="8c51c-132">For more information on [forms and validation](/aspnet/core/blazor/forms-validation) in Blazor apps, see the Blazor documentation.</span></span>

>[!div class="step-by-step"]
><span data-ttu-id="8c51c-133">[上一页](state-management.md)
>[下一页](data.md)</span><span class="sxs-lookup"><span data-stu-id="8c51c-133">[Previous](state-management.md)
[Next](data.md)</span></span>
