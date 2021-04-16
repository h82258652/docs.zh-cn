---
title: 页面、路由和布局
description: 了解如何执行以下操作：在 Blazor 创建页面、使用客户端路由和管理页面布局。
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 09/19/2019
ms.openlocfilehash: 714ba0be7c2014895a75250a47e6ce448863eb6c
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "88267784"
---
# <a name="pages-routing-and-layouts"></a>页面、路由和布局

ASP.NET Web Forms 应用由在 .aspx 文件中定义的页面组成。 每个页面的地址都基于其在项目中的物理文件路径。 浏览器向页面发出请求时，页面内容会动态呈现在服务器上。 页面的 HTML 标记及其服务器控件的渲染帐户。

在 Blazor 中，应用中的每个页面都是一个组件，通常在 .razor 文件中定义，具有一个或多个指定路由。 路由大多数发生在客户端，而不涉及特定的服务器请求。 浏览器首先向应用的根地址发出请求。 然后 Blazor 应用中的根 `Router` 组件会截获导航请求，并将其发送到正确的组件。

Blazor 还支持深层链接。 浏览器向特定路由器而非应用的根发出请求时，将发生深层链接。 发送到服务器的深层链接请求会路由到 Blazor 应用，然后该应用会将请求客户端路由到正确的组件。

ASP.NET Web Forms 中的一个简单页面可能包含以下标记：

Name.aspx

```aspx-csharp
<%@ Page Title="Name" Language="C#" MasterPageFile="~/Site.Master" AutoEventWireup="true" CodeBehind="Name.aspx.cs" Inherits="WebApplication1.Name" %>

<asp:Content ID="BodyContent" ContentPlaceHolderID="MainContent" runat="server">
    <div>
        What is your name?<br />
        <asp:TextBox ID="TextBox1" runat="server"></asp:TextBox>
        <asp:Button ID="Button1" runat="server" Text="Submit" OnClick="Button1_Click" />
    </div>
    <div>
        <asp:Literal ID="Literal1" runat="server" />
    </div>
</asp:Content>
```

Name.aspx.cs

```csharp
public partial class Name : System.Web.UI.Page
{
    protected void Button1_Click1(object sender, EventArgs e)
    {
        Literal1.Text = "Hello " + TextBox1.Text;
    }
}
```

Blazor 应用中的相应页面如下所示：

Name.razor

```razor
@page "/Name"
@layout MainLayout

<div>
    What is your name?<br />
    <input @bind="text" />
    <button @onclick="OnClick">Submit</button>
</div>
<div>
    @if (name != null)
    {
        @:Hello @name
    }
</div>

@code {
    string text;
    string name;

    void OnClick() {
        name = text;
    }
}
```

## <a name="create-pages"></a>创建页面

要在 Blazor 中创建页面，请创建一个组件并添加 `@page` Razor 指令以指定该组件的路由。 `@page` 指令采用单个参数，这是要添加到该组件的路由模板。

```razor
@page "/counter"
```

路由模板参数是必需的。 与 ASP.NET Web Forms 不同，指向 Blazor 组件的路由不是根据其文件位置推断的（虽然将来可能会添加该功能）。

路由模板语法是用于在 ASP.NET Web Forms 中进行路由的基本语法。 路由参数在模板中用大括号指定。 Blazor 会将路由值绑定到具有相同名称（区分大小写）的组件参数。

```razor
@page "/product/{id}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public string Id { get; set; }
}
```

还可以为路由参数值指定约束。 例如，将产品 ID 约束为 `int`：

```razor
@page "/product/{id:int}"

<h1>Product @Id</h1>

@code {
    [Parameter]
    public int Id { get; set; }
}
```

有关 Blazor 支持的路由约束的完整列表，请参阅[路由约束](/aspnet/core/blazor/routing#route-constraints)。

## <a name="router-component"></a>路由器组件

Blazor 中的路由由 `Router` 组件处理。 `Router` 组件通常在应用的根组件 (App.razor) 中使用。

```razor
<Router AppAssembly="@typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="@routeData" DefaultLayout="@typeof(MainLayout)" />
    </Found>
    <NotFound>
        <LayoutView Layout="@typeof(MainLayout)">
            <p>Sorry, there's nothing at this address.</p>
        </LayoutView>
    </NotFound>
</Router>
```

`Router` 组件会在指定的 `AppAssembly` 和 `AdditionalAssemblies`（可选）中发现可路由组件。 浏览器进行导航时，如果有路由与地址匹配，`Router` 会拦截导航并呈现其 `Found` 参数的内容和提取的 `RouteData`，否则 `Router` 会呈现其 `NotFound` 参数。

`RouteView` 组件负责呈现由 `RouteData` 指定的匹配组件及其布局（如果有）。 如果匹配组件没有布局，则使用可选择指定的 `DefaultLayout`。

`LayoutView` 组件在指定布局内呈现其子内容。 本章节稍后将详细介绍布局。

## <a name="navigation"></a>导航

在 ASP.NET Web Forms 中，通过将重定向响应返回到浏览器来触发到其他页面的导航。 例如：

```csharp
protected void NavigateButton_Click(object sender, EventArgs e)
{
    Response.Redirect("Counter");
}
```

通常无法在 Blazor 中返回重定向响应。 Blazor 不使用“请求-答复”模式。 但可以像使用 JavaScript 那样，直接触发浏览器导航。

Blazor 提供 `NavigationManager` 服务，该服务可用于：

- 获取当前浏览器地址
- 获取基址
- 触发导航
- 更改地址时收到通知

要导航到其他地址，请使用 `NavigateTo` 方法：

```razor
@page "/"
@inject NavigationManager NavigationManager

<button @onclick="Navigate">Navigate</button>

@code {
    void Navigate() {
        NavigationManager.NavigateTo("counter");
    }
}
```

有关所有 `NavigationManager` 成员的描述，请参阅 [URI 和导航状态帮助程序](/aspnet/core/blazor/routing#uri-and-navigation-state-helpers)。

## <a name="base-urls"></a>基 URL

如果 Blazor 应用部署在基路径下，那么需要在页面元数据中使用 `<base>` 标记指定用于路由到工作属性的基 URL。 如果应用的主机页面是使用 Razor 来实现服务器呈现的，那么可使用 `~/` 语法指定应用的基址。 如果主机页面是静态 HTML，则需要显式指定基 URL。

```html
<base href="~/" />
```

## <a name="page-layout"></a>页面布局

ASP.NET Web Forms 中的页面布局由母版页处理。 母版页定义包含一个或多个内容占位符的模板，这些内容占位符由各个页面提供。 母版页在 .master 文件中定义，并以 `<%@ Master %>` 指令开头。 .master 文件内容的编码方式与 .aspx 页面的编码方式相同，但添加了 `<asp:ContentPlaceHolder>` 控件以标记页面可以提供内容的位置 。

*Site.master*

```aspx-csharp
<%@ Master Language="C#" AutoEventWireup="true" CodeBehind="Site.master.cs" Inherits="WebApplication1.SiteMaster" %>

<!DOCTYPE html>
<html lang="en">
<head runat="server">
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title><%: Page.Title %> - My ASP.NET Application</title>
    <link href="~/favicon.ico" rel="shortcut icon" type="image/x-icon" />
</head>
<body>
    <form runat="server">
        <div class="container body-content">
            <asp:ContentPlaceHolder ID="MainContent" runat="server">
            </asp:ContentPlaceHolder>
            <hr />
            <footer>
                <p>&copy; <%: DateTime.Now.Year %> - My ASP.NET Application</p>
            </footer>
        </div>
    </form>
</body>
</html>
```

在 Blazor 中，使用布局组件处理页面布局。 布局组件继承自 `LayoutComponentBase`，后者定义类型 `RenderFragment` 的单个 `Body` 属性，该属性可用于呈现页面的内容。

MainLayout.razor

```razor
@inherits LayoutComponentBase
<h1>Main layout</h1>
<div>
    @Body
</div>
```

呈现具有布局的的页面时，页面会在指定布局的内容内呈现，呈现位置位于布局呈现其 `Body` 属性的位置。

要将布局应用到页面，请使用 `@layout` 指令：

```razor
@layout MainLayout
```

可以使用 _Imports.razor 文件指定文件夹和子文件夹中的所有组件的布局。 还可使用[路由器组件](#router-component)指定所有页面的默认布局。

母版页可以定义多个内容占位符，但是 Blazor 中的布局仅有单个 `Body` 属性。 Blazor 布局组件的此限制可能会在将来的版本中得到解决。

ASP.NET Web Forms 中的母版页可以嵌套。 即母版页也可能使用母版页。 Blazor 中的布局组件也可能是嵌套的。 可将布局组件应用于布局组件。 内部布局的内容可以在外部布局中呈现。

ChildLayout.razor

```razor
@layout MainLayout
<h2>Child layout</h2>
<div>
    @Body
</div>
```

Index.razor

```razor
@page "/"
@layout ChildLayout
<p>I'm in a nested layout!</p>
```

为页面呈现的输出如下：

```html
<h1>Main layout</h1>
<div>
    <h2>Child layout</h2>
    <div>
        <p>I'm in a nested layout!</p>
    </div>
</div>
```

Blazor 中的布局通常不会定义页面的根 HTML 元素（`<html>`、`<body>` 和 `<head>` 等）。 根 HTML 元素是在 Blazor 应用的主机页中定义，该页用于呈现应用的初始 HTML 内容（请参阅[启动Blazor](project-structure.md#bootstrap-blazor)）。 主机页可以使用周围的标记呈现应用的多个根组件。

Blazor 中的组件（包括页面）无法呈现 `<script>` 标记。 存在此呈现限制是因为 `<script>` 标记一经加载就无法更改。 如果尝试使用 Razor 语法动态呈现标记，可能会出现意外行为。 应将所有 `<script>` 标记添加到应用的主机页。

>[!div class="step-by-step"]
>[上一页](components.md)
>[下一页](state-management.md)
