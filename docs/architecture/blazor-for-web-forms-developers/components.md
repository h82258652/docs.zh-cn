---
title: 使用 Blazor 生成可重用的 UI 组件
description: 了解如何使用 Blazor 生成可重用的 UI 组件，以及它们与 ASP.NET Web Forms 控件之间的区别。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 09/18/2019
ms.openlocfilehash: fd560c84c095dffc3718a7709af904d9ba722a18
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "97512764"
---
# <a name="build-reusable-ui-components-with-blazor"></a>使用 Blazor 生成可重用的 UI 组件

ASP.NET Web Forms 的优势之一在于其能够将可重用的用户界面 (UI) 代码片段封装到可重用的 UI 控件中。 可以使用 .ascx 文件在标记中定义自定义用户控件。 还可以在代码中生成具有全面设计器支持的复杂服务器控件。

Blazor 还支持通过组件进行 UI 封装。 组件具有以下特点：

- 是自包含的 UI 块。
- 维护自己的状态和呈现逻辑。
- 可以定义 UI 事件处理程序、绑定到输入数据以及管理其自己的生命周期。
- 通常使用 Razor 语法在 .razor 文件中定义。

## <a name="an-introduction-to-razor"></a>Razor 简介

Razor 是基于 HTML 和 C# 的轻量级标记模板化语言。 借助 Razor，可以在标记和 C# 代码之间无缝转换，以定义组件呈现逻辑。 编译 .razor 文件时，将在 .NET 类中以结构化方式捕获呈现逻辑。 编译类的名称从 .razor 文件名中获取。 命名空间从项目的默认命名空间和文件夹路径中获取，也可以使用 `@namespace` 指令（在下面的 Razor 指令中详细介绍）显式指定命名空间。

组件的呈现逻辑使用常规 HTML 标记创作，并使用 C# 添加动态逻辑。 `@` 字符用于转换为 C#。 Razor 通常很聪明，可猜出你何时切换回 HTML。 例如，以下组件使用当前时间呈现 `<p>` 标记：

```razor
<p>@DateTime.Now</p>
```

若要显式指定 C# 表达式的开头和结尾，请使用括号：

```razor
<p>@(DateTime.Now)</p>
```

使用 Razor 还可以在呈现逻辑中轻松地使用 C# 控制流。 例如，可以有条件地呈现某些 HTML，如下所示：

```razor
@if (value % 2 == 0)
{
    <p>The value was even.</p>
}
```

或者，可以使用常规 C# `foreach` 循环生成项目列表，如下所示：

```razor
<ul>
@foreach (var item in items)
{
    <li>@item.Text</li>
}
</ul>
```

像 ASP.NET Web Forms 中的指令一样，Razor 指令可控制 Razor 组件编译的许多方面。 示例包括组件的以下项：

- 命名空间
- 基类
- 实现的接口
- 泛型参数
- 导入的命名空间
- 路由

Razor 指令以 `@` 字符开头，通常用于文件开头的新行的开头。 例如，`@namespace` 指令定义组件的命名空间：

```razor
@namespace MyComponentNamespace
```

下表总结 Blazor 中使用的各种 Razor 指令及其 ASP.NET Web Forms 等效项（如果存在）。

|指令    |描述|示例|Web Forms 等效项|
|-------------|-----------|-------|--------------------|
|`@attribute` |将类级别属性添加到组件|`@attribute [Authorize]`|None|
|`@code`      |将类成员添加到组件|`@code { ... }`|`<script runat="server">...</script>`|
|`@implements`|实现指定的接口|`@implements IDisposable`|使用代码隐藏|
|`@inherits`  |从指定的基类继承|`@inherits MyComponentBase`|`<%@ Control Inherits="MyUserControlBase" %>`|
|`@inject`    |将服务注入组件|`@inject IJSRuntime JS`|None|
|`@layout`    |指定组件的布局组件|`@layout MainLayout`|`<%@ Page MasterPageFile="~/Site.Master" %>`|
|`@namespace` |设置组件的命名空间|`@namespace MyNamespace`|None|
|`@page`      |指定组件的路由|`@page "/product/{id}"`|`<%@ Page %>`|
|`@typeparam` |指定组件的泛型类型参数|`@typeparam TItem`|使用代码隐藏|
|`@using`     |指定要纳入范围的命名空间|`@using MyComponentNamespace`|在 web.config 中添加命名空间|

Razor 组件还广泛使用元素上的指令属性来控制组件编译的各个方面（事件处理、数据绑定、组件和元素引用等）。 指令属性均遵循常用泛型语法，其中括号中的值是可选的：

```razor
@directive(-suffix(:name))(="value")
```

下表总结 Blazor 中使用的 Razor 指令的各种属性。

|属性    |说明|示例|
|-------------|-----------|-------|
|`@attributes`|呈现属性字典|`<input @attributes="ExtraAttributes" />`|
|`@bind`      |创建双向数据绑定    |`<input @bind="username" @bind:event="oninput" />`|
|`@on{event}` |为指定事件添加事件处理程序|`<button @onclick="IncrementCount">Click me!</button>`|
|`@key`       |指定比较算法要用于保存集合中元素的键|`<DetailsEditor @key="person" Details="person.Details" />`|
|`@ref`       |捕获对组件或 HTML 元素的引用|`<MyDialog @ref="myDialog" />`|

Blazor 使用的各种指令属性（`@onclick`、`@bind`、`@ref` 等）将在下面的部分和后续章节中介绍。

.aspx 和 .ascx 文件中使用的许多语法在 Razor 中具有并行语法。 下面是 ASP.NET Web Forms 语法和 Razor 语法的简单比较。

|功能                      |Web Forms — Web 窗体           |语法               |Razor         |语法 |
|-----------------------------|--------------------|---------------------|--------------|-------|
|指令                   |`<%@ [directive] %>`|`<%@ Page %>`        |`@[directive]`|`@page`|
|代码块                  |`<% %>`             |`<% int x = 123; %>` |`@{ }`        |`@{ int x = 123; }`|
|表达式<br>（HTML 编码）|`<%: %>`            |`<%:DateTime.Now %>` |隐式：`@`<br>显式：`@()`|`@DateTime.Now`<br>`@(DateTime.Now)`|
|注释                     |`<%-- --%>`         |`<%-- Commented --%>`|`@* *@`       |`@* Commented *@`|
|数据绑定                 |`<%# %>`            |`<%# Bind("Name") %>`|`@bind`       |`<input @bind="username" />`|

若要将成员添加到 Razor 组件类，请使用 `@code` 指令。 此方法类似于在 ASP.NET Web Forms 用户控件或页中使用 `<script runat="server">...</script>` 块。

```razor
@code {
    int count = 0;

    void IncrementCount()
    {
        count++;
    }
}
```

因为 Razor 基于 C#，所以必须从 C# 项目 (.csproj) 中进行编译。 无法从 Visual Basic 项目 (.vbproj) 编译 .razor 文件。 仍然可以从 Blazor 项目引用 Visual Basic 项目。 反之亦然。

有关完整的 Razor 语法参考，请参阅 [ASP.NET Core 的 Razor 语法参考](/aspnet/core/mvc/views/razor)。

## <a name="use-components"></a>使用组件

除常规 HTML 外，组件还可以将其他组件用作其呈现逻辑的一部分。 用于在 Razor 中使用组件的语法类似于在 ASP.NET Web Forms 应用中使用用户控件。 使用与组件的类型名称匹配的元素标记指定组件。 例如，可以添加 `Counter` 组件，如下所示：

```razor
<Counter />
```

与 ASP.NET Web Forms 不同，Blazor 中的组件：

- 不使用元素前缀（例如，`asp:`）。
- 不要求在页上或在 web.config 中进行注册。

将 Razor 组件视为 .NET 类型，因为这正是其本质。 如果引用了包含组件的程序集，则该组件可供使用。 若要将组件的命名空间纳入范围，请应用 `@using` 指令：

```razor
@using MyComponentLib

<Counter />
```

如默认的 Blazor 项目所示，通常将 `@using` 指令放入 _Imports.razor 文件中，以便将它们导入同一目录中和子目录中的所有 .razor 文件。

如果组件的命名空间不在范围内，则可以使用其完整类型名称来指定组件，就像在 C# 中一样：

```razor
<MyComponentLib.Counter />
```

## <a name="component-parameters"></a>组件参数

在 ASP.NET Web Forms 中，可以使用公共属性将参数和数据传递到控件。 这些属性可以使用特性在标记中进行设置，也可以直接在代码中设置。 Blazor 组件以类似的方式工作，尽管组件属性还必须使用 `[Parameter]` 特性进行标记才能被视为组件参数。

以下 `Counter` 组件定义名为 `IncrementAmount` 的组件参数，该参数可用于指定每次单击按钮时 `Counter` 应该递增的数量。

```razor
<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button class="btn btn-primary" @onclick="IncrementCount">Click me</button>

@code {
    int currentCount = 0;

    [Parameter]
    public int IncrementAmount { get; set; } = 1;

    void IncrementCount()
    {
        currentCount+=IncrementAmount;
    }
}
```

若要在 Blazor 中指定组件参数，请像在 ASP.NET Web Forms 中一样使用特性：

```razor
<Counter IncrementAmount="10" />
```

## <a name="event-handlers"></a>事件处理程序

ASP.NET Web Forms 和 Blazor 均提供用于处理 UI 事件的基于事件的编程模型。 此类事件的示例包括按钮单击和文本输入。 在 ASP.NET Web Forms 中，可以使用 HTML 服务器控件处理 DOM 公开的 UI 事件，也可以处理 Web 服务器控件公开的事件。 这些事件通过表单回发请求在服务器上显示。 考虑以下 Web Forms 按钮单击示例：

*Counter.ascx*

```aspx-csharp
<asp:Button ID="ClickMeButton" runat="server" Text="Click me!" OnClick="ClickMeButton_Click" />
```

*Counter.ascx.cs*

```csharp
public partial class Counter : System.Web.UI.UserControl
{
    protected void ClickMeButton_Click(object sender, EventArgs e)
    {
        Console.WriteLine("The button was clicked!");
    }
}
```

在 Blazor 中，可以直接使用 `@on{event}` 形式的指令属性为 DOM UI 事件注册处理程序。 `{event}` 占位符表示事件的名称。 例如，可侦听按钮单击，如下所示：

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    void OnClick()
    {
        Console.WriteLine("The button was clicked!);
    }
}
```

事件处理程序可以接受可选的事件特定参数，以提供有关事件的详细信息。 例如，鼠标事件可以接受 `MouseEventArgs` 参数，但这不是必需的。

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    void OnClick(MouseEventArgs e)
    {
        Console.WriteLine($"Mouse clicked at {e.ScreenX}, {e.ScreenY}.");
    }
}
```

可以使用 lambda 表达式来代替引用事件处理程序的方法组。 借助 lambda 表达式，可以覆盖其他范围内的值。

```razor
@foreach (var buttonLabel in buttonLabels)
{
    <button @onclick="() => Console.WriteLine($"The {buttonLabel} button was clicked!")">@buttonLabel</button>
}
```

事件处理程序可以同步或异步执行。 例如，以下 `OnClick` 事件处理程序异步执行：

```razor
<button @onclick="OnClick">Click me!</button>

@code {
    async Task OnClick()
    {
        var result = await Http.GetAsync("api/values");
    }
}
```

处理事件后，将呈现组件以解释任何组件状态更改。 使用异步事件处理程序时，组件将在处理程序执行完成后立即呈现。 异步 `Task` 完成后，将再次呈现组件。 在异步 `Task` 仍在执行时，此异步执行模式提供呈现部分相应 UI 的机会。

```razor
<button @onclick="ShowMessage">Get message</button>

@if (showMessage)
{
    @if (message == null)
    {
        <p><em>Loading...</em></p>
    }
    else
    {
        <p>The message is: @message</p>
    }
}

@code
{
    bool showMessage = false;
    string message;

    public async Task ShowMessage()
    {
        showMessage = true;
        message = await MessageService.GetMessageAsync();
    }
}
```

组件还可以通过定义类型为 `EventCallback<TValue>` 的组件参数来定义自己的事件。 事件回叫支持 DOM UI 事件处理程序的所有变体：可选参数、同步或异步、方法组或 lambda 表达式。

```razor
<button class="btn btn-primary" @onclick="OnClick">Click me!</button>

@code {
    [Parameter]
    public EventCallback<MouseEventArgs> OnClick { get; set; }
}
```

## <a name="data-binding"></a>数据绑定

Blazor 提供将 UI 组件中的数据绑定到组件状态的简单机制。 此方法与 ASP.NET Web Forms 中用于将数据源中的数据绑定到 UI 控件的功能不同。 我们将在[处理数据](data.md)部分中介绍如何处理来自不同数据源的数据。

若要创建从 UI 组件到组件状态的双向数据绑定，请使用 `@bind` 指令属性。 在下面的示例中，复选框的值绑定到 `isChecked` 字段。

```razor
<input type="checkbox" @bind="isChecked" />

@code {
    bool isChecked;
}
```

呈现组件时，复选框的值将设置为 `isChecked` 字段的值。 当用户切换复选框时，将触发 `onchange` 事件，且 `isChecked` 字段将设置为新值。 在这种情况下，`@bind` 语法等效于以下标记：

```razor
<input value="@isChecked" @onchange="(UIChangeEventArgs e) => isChecked = e.Value" />
```

若要更改用于绑定的事件，请使用 `@bind:event` 属性。

```razor
<input @bind="text" @bind:event="oninput" />
<p>@text</p>

@code {
    string text;
}
```

组件还可以支持将数据绑定到其参数。 若要进行数据绑定，请定义与可绑定参数同名的事件回叫参数。 名称会添加“Changed”后缀。

*PasswordBox.razor*

```razor
Password: <input
    value="@Password"
    @oninput="OnPasswordChanged"
    type="@(showPassword ? "text" : "password")" />

<label><input type="checkbox" @bind="showPassword" />Show password</label>

@code {
    private bool showPassword;

    [Parameter]
    public string Password { get; set; }

    [Parameter]
    public EventCallback<string> PasswordChanged { get; set; }

    private Task OnPasswordChanged(ChangeEventArgs e)
    {
        Password = e.Value.ToString();
        return PasswordChanged.InvokeAsync(Password);
    }
}
```

若要将数据绑定链接到基础 UI 元素，请设置值并直接在 UI 元素上处理事件，而不要使用 `@bind` 属性。

若要绑定到组件参数，请使用 `@bind-{Parameter}` 属性指定要绑定到的参数。

```razor
<PasswordBox @bind-Password="password" />

@code {
    string password;
}
```

## <a name="state-changes"></a>状态更改

如果组件的状态已在常规 UI 事件或事件回叫之外更改，则组件必须手动发出需要再次呈现的信号。 若要发出组件状态已更改的信号，请在组件上调用 `StateHasChanged` 方法。

在下面的示例中，组件显示来自 `AppState` 服务的消息（可由应用的其他部分更新）。 组件向 `AppState.OnChange` 事件注册其 `StateHasChanged` 方法，以便每次消息更新时呈现该组件。

```csharp
public class AppState
{
    public string Message { get; }

    // Lets components receive change notifications
    public event Action OnChange;

    public void UpdateMessage(string message)
    {
        Message = message;
        NotifyStateChanged();
    }

    private void NotifyStateChanged() => OnChange?.Invoke();
}
```

```razor
@inject AppState AppState

<p>App message: @AppState.Message</p>

@code {
    protected override void OnInitialized()
    {
        AppState.OnChange += StateHasChanged
    }
}
```

## <a name="component-lifecycle"></a>组件生命周期

ASP.NET Web Forms 框架为模块、页和控件提供定义完善的生命周期方法。 例如，以下控件为 `Init`、`Load` 和 `UnLoad` 生命周期事件实现事件处理程序：

*Counter.ascx.cs*

```csharp
public partial class Counter : System.Web.UI.UserControl
{
    protected void Page_Init(object sender, EventArgs e) { ... }
    protected void Page_Load(object sender, EventArgs e) { ... }
    protected void Page_UnLoad(object sender, EventArgs e) { ... }
}
```

Blazor 组件也具有定义完善的生命周期。 组件的生命周期可用于初始化组件状态及实现高级组件行为。

Blazor 的所有组件生命周期方法都具有同步和异步版本。 组件呈现是同步的。 不能在组件呈现期间运行异步逻辑。 所有异步逻辑都必须作为 `async` 生命周期方法的一部分执行。

### <a name="oninitialized"></a>OnInitialized

`OnInitialized` 和 `OnInitializedAsync` 方法用于初始化组件。 组件通常在首次呈现后初始化。 组件初始化后，可能会在最终释放前呈现多次。 `OnInitialized` 方法类似于 ASP.NET Web Forms 页和控件中的 `Page_Load` 事件。

```csharp
protected override void OnInitialized() { ... }
protected override async Task OnInitializedAsync() { await ... }
```

### <a name="onparametersset"></a>OnParametersSet

当组件已从其父级接收参数并将值分配给属性时，将调用 `OnParametersSet` 和 `OnParametersSetAsync` 方法。 这些方法在组件初始化后以及每次呈现组件时执行。

```csharp
protected override void OnParametersSet() { ... }
protected override async Task OnParametersSetAsync() { await ... }
```

### <a name="onafterrender"></a>OnAfterRender

`OnAfterRender` 和 `OnAfterRenderAsync` 方法在组件完成呈现后调用。 此时，将填充元素和组件引用（在下文中详细介绍这些概念）。 此时已启用与浏览器的交互性功能。 与 DOM 和 JavaScript 执行的交互可以安全地进行。

```csharp
protected override void OnAfterRender(bool firstRender)
{
    if (firstRender)
    {
        ...
    }
}
protected override async Task OnAfterRenderAsync(bool firstRender)
{
    if (firstRender)
    {
        await ...
    }
}
```

在服务器上进行预呈现时，不调用 `OnAfterRender` 和 `OnAfterRenderAsync`。

`firstRender` 参数在首次呈现组件时为 `true`；否则，其值为 `false`。

### <a name="idisposable"></a>IDisposable

从 UI 中移除 Blazor 组件时，该组件可以实现 `IDisposable` 来释放资源。 Razor 组件可以通过使用 `@implements` 指令来实现 `IDispose`：

```razor
@using System
@implements IDisposable

...

@code {
    public void Dispose()
    {
        ...
    }
}
```

## <a name="capture-component-references"></a>捕获组件引用

在 ASP.NET Web Forms 中，通常通过引用控件实例 ID 直接在代码中操作控件实例。 在 Blazor 中，也可以捕获和操作对组件的引用，尽管这种做法不太常见。

若要在 Blazor 中捕获组件引用，请使用 `@ref` 指令属性。 属性的值应该匹配与引用的组件具有相同类型的可设置字段的名称。

```razor
<MyLoginDialog @ref="loginDialog" ... />

@code {
    MyLoginDialog loginDialog;

    void OnSomething()
    {
        loginDialog.Show();
    }
}
```

呈现父组件时，将使用子组件实例填充该字段。 然后，可以在组件实例上调用方法，或以其他方式操作组件实例。

不建议直接使用组件引用来操作组件状态。 这样做会阻止组件在正确的时间自动呈现。

## <a name="capture-element-references"></a>捕获元素引用

Blazor 组件可以捕获对元素的引用。 与 ASP.NET Web Forms 中的 HTML 服务器控件不同，不能在 Blazor 中直接使用元素引用来操作 DOM。 Blazor 使用其 DOM 比较算法处理大部分 DOM 交互。 Blazor 中捕获的元素引用是不透明的。 但是，它们用于在 JavaScript 互操作调用中传递特定元素引用。 有关 JavaScript 互操作的详细信息，请参阅 [ASP.NET Core Blazor JavaScript 互操作](/aspnet/core/blazor/javascript-interop)。

## <a name="templated-components"></a>模板化组件

在 ASP.NET Web Forms 中，可以创建模板化控件。 模板化控件使开发人员可以指定用于呈现容器控件的一部分 HTML。 生成模板化服务器控件的机制很复杂，但它们提供了功能强大的方案，可用于以用户可自定义的方式呈现数据。 模板化控件的示例包括 `Repeater` 和 `DataList`。

还可以通过定义类型为 `RenderFragment` 或 `RenderFragment<T>` 的组件参数来模板化 Blazor 组件。 `RenderFragment` 表示可随后由组件呈现的 Razor 标记块。 `RenderFragment<T>` 是 Razor 标记块，其接受可在呈现片段时指定的参数。

### <a name="child-content"></a>子内容

Blazor 组件可以将其子内容作为 `RenderFragment` 进行捕获并将该内容作为组件呈现的一部分呈现。 若要捕获子内容，请定义类型为 `RenderFragment` 的组件参数并将其命名为 `ChildContent`。

*ChildContentComponent.razor*

```razor
<h1>Component with child content</h1>

<div>@ChildContent</div>

@code {
    [Parameter]
    public RenderFragment ChildContent { get; set; }
}
```

然后，父组件可以使用常规 Razor 语法提供子内容。

```razor
<ChildContentComponent>
    <ChildContent>
        <p>The time is @DateTime.Now</p>
    </ChildContent>
</ChildContentComponent>
```

### <a name="template-parameters"></a>模板参数

模板化 Blazor 组件还可以定义类型为 `RenderFragment` 或 `RenderFragment<T>` 的多个组件参数。 `RenderFragment<T>` 的参数可以在调用时指定。 若要为组件指定泛型类型参数，请使用 `@typeparam` Razor 指令。

*SimpleListView.razor*

```razor
@typeparam TItem

@Heading

<ul>
@foreach (var item in Items)
{
    <li>@ItemTemplate(item)</li>
}
</ul>

@code {
    [Parameter]
    public RenderFragment Heading { get; set; }

    [Parameter]
    public RenderFragment<TItem> ItemTemplate { get; set; }

    [Parameter]
    public IEnumerable<TItem> Items { get; set; }
}
```

在使用模板化组件时，可以使用与参数名称匹配的子元素来指定模板参数。 作为元素传递的类型为 `RenderFragment<T>` 的组件参数有一个名为 `context` 的隐式参数。 可以使用子元素上的 `Context` 属性来更改此实现参数的名称。 可以使用与类型参数名称匹配的属性来指定任何泛型类型参数。 如果可能，将推断类型参数：

```razor
<SimpleListView Items="messages" TItem="string">
    <Heading>
        <h1>My list</h1>
    </Heading>
    <ItemTemplate Context="message">
        <p>The message is: @message</p>
    </ItemTemplate>
</SimpleListView>
```

此组件的输出如下所示：

```html
<h1>My list</h1>
<ul>
    <li><p>The message is: message1</p></li>
    <li><p>The message is: message2</p></li>
<ul>
```

## <a name="code-behind"></a>代码隐藏

Blazor 组件通常在单个 .razor 文件中创作。 但也可以使用代码隐藏文件来分隔代码和标记。 若要使用组件文件，请添加与组件文件的文件名匹配，但添加了 .cs 扩展名的 C# 文件 (Counter.razor.cs)。 使用 C# 文件定义组件的基类。 可以根据需要命名基类，但通常将基类命名为与组件类相同的名称，并添加 `Base` 扩展名 (`CounterBase`)。 基于组件的类也必须从 `ComponentBase` 中派生。 然后，在 Razor 组件文件中，添加 `@inherits` 指令以指定组件的基类 (`@inherits CounterBase`)。

*Counter.razor*

```razor
@inherits CounterBase

<h1>Counter</h1>

<p>Current count: @currentCount</p>

<button @onclick="IncrementCount">Click me</button>
```

*Counter.razor.cs*

```csharp
public class CounterBase : ComponentBase
{
    protected int currentCount = 0;

    protected void IncrementCount()
    {
        currentCount++;
    }
}
```

组件成员在基类中的可见性必须为 `protected` 或 `public` 才能对组件类可见。

## <a name="additional-resources"></a>其他资源

以上内容并不是对 Blazor 组件的所有方面的详尽介绍。 有关如何[创建和使用 ASP.NET Core Razor 组件](/aspnet/core/blazor/components)的详细信息，请参阅 Blazor 文档。

>[!div class="step-by-step"]
>[上一页](app-startup.md)
>[下一页](pages-routing-layouts.md)
