---
title: 安全性：ASP.NET Web Forms 和 Blazor 中的身份验证和授权
description: 了解如何在 ASP.NET Web Forms 和 Blazor 中处理身份验证和授权。
author: ardalis
ms.author: daroth
no-loc:
- Blazor
ms.date: 11/20/2020
ms.openlocfilehash: 0344960237a5d9da61eb0d85987c44e136f1be48
ms.sourcegitcommit: 05d0087dfca85aac9ca2960f86c5efd218bf833f
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/30/2021
ms.locfileid: "96509840"
---
# <a name="security-authentication-and-authorization-in-aspnet-web-forms-and-blazor"></a>安全性：ASP.NET Web Forms 和 Blazor  中的身份验证和授权

从 ASP.NET Web Forms 应用程序迁移到 Blazor 几乎肯定需要更新身份验证和授权的执行方式（假设应用程序已配置身份验证）。 本章节介绍如何从 ASP.NET Web Forms 通用提供程序模型（用于成员资格、角色和用户配置文件）进行迁移，以及如何从 Blazor 应用使用 ASP.NET Core Identity。 尽管本章节将介绍大致步骤和注意事项，但可在参考文档中找到详细步骤和脚本。

## <a name="aspnet-universal-providers"></a>ASP.NET 通用提供程序

从 ASP.NET 2.0 开始，ASP.NET Web Forms 平台支持用于各种功能（包括成员资格）的提供程序模型。 通用成员资格提供程序以及可选的角色提供程序通常与 ASP.NET Web Forms 应用程序一起部署。 它提供了一种管理身份验证和授权的可靠且安全的方式，且这种方式现今仍然可以正常运行。 这些通用提供程序的最新产品/服务是 NuGet 包 [Microsoft.AspNet.Providers](https://www.nuget.org/packages/Microsoft.AspNet.Providers)。

通用提供程序适用于包含 `aspnet_Applications`、`aspnet_Membership`、`aspnet_Roles` 和 `aspnet_Users` 等表的 SQL 数据库架构。 通过运行 [aspnet_regsql.exe 命令](/previous-versions/ms229862(v=vs.140))进行配置时，提供程序将安装表和存储过程，这些表和存储过程提供使用基础数据所需的所有必要查询和命令。 数据库架构和这些存储过程与较新的 ASP.NET Identity 和 ASP.NET Core Identity 系统不兼容，因此必须将现有数据迁移到新系统。 图 1 显示为通用提供程序配置的示例表架构。

![通用提供程序架构](./media/security/membership-tables.png)

通用提供程序负责处理用户、成员资格、角色和配置文件。 为用户分配了全局唯一标识符，且基本信息（如 userId、userName 等）存储在 `aspnet_Users` 表中。 身份验证信息（例如密码、密码格式、密码加密盐、锁定计数器和详细信息等）存储在 `aspnet_Membership` 表中。 角色仅包含名称和唯一标识符，它们通过 `aspnet_UsersInRoles` 关联表分配给用户，从而提供多对多关系。

如果现有系统除使用成员资格外还使用角色，则需要将用户帐户、关联密码、角色和角色成员资格迁移到 ASP.NET Core Identity。 还很有可能需要将当前使用 if 语句执行角色检查的代码更新为使用声明性筛选器、属性和/或标记帮助程序。 我们将在本章节的末尾详细介绍迁移注意事项。

### <a name="authorization-configuration-in-web-forms&quot;></a>Web Forms 中的授权配置

若要配置对 ASP.NET Web Forms 应用程序中某些页的授权访问，通常可将特定页或文件夹指定为匿名用户无法访问。 此配置在 web.config 文件中完成：

```xml
<?xml version=&quot;1.0&quot;?>
<configuration>
    <system.web>
      <authentication mode=&quot;Forms&quot;>
        <forms defaultUrl=&quot;~/home.aspx&quot; loginUrl=&quot;~/login.aspx&quot;
          slidingExpiration=&quot;true&quot; timeout=&quot;2880&quot;></forms>
      </authentication>

      <authorization>
        <deny users=&quot;?&quot; />
      </authorization>
    </system.web>
</configuration>
```

`authentication` 配置节为应用程序设置表单身份验证。 `authorization` 节用于禁止匿名用户使用整个应用程序。 但是，可以按位置提供更精细的授权规则，也可以应用基于角色的授权检查。

```xml
<location path=&quot;login.aspx&quot;>
  <system.web>
    <authorization>
      <allow users=&quot;*&quot; />
    </authorization>
  </system.web>
</location>
```

以上配置与第一个配置结合使用时，将允许匿名用户访问登录页，从而替代站点范围内对未经身份验证的用户的限制。

```xml
<location path=&quot;/admin&quot;>
  <system.web>
    <authorization>
      <allow roles=&quot;Administrators&quot; />
      <deny users=&quot;*&quot; />
    </authorization>
  </system.web>
</location>
```

以上配置与其他配置结合使用时，将对 `/admin` 文件夹及其中所有资源的访问权限限制为“管理员”角色的成员。 还可以通过在 `/admin` 文件夹根目录内放置单独的 `web.config` 文件来应用此限制。

### <a name=&quot;authorization-code-in-web-forms&quot;></a>Web Forms 中的授权代码

除使用 `web.config` 配置访问权限外，还可以在 Web Forms 应用程序中以编程方式配置访问权限和行为。 例如，可以根据用户的角色来限制执行某些操作或查看某些数据的能力。

此代码既可以在代码隐藏逻辑中使用，也可以在页本身使用：

```html
<% if (HttpContext.Current.User.IsInRole(&quot;Administrators")) { %>
  <a href="/admin">Go To Admin</a>
<% } %>
```

除检查用户角色成员资格外，还可以确定其是否已经过身份验证（尽管通常最好使用上面介绍的基于位置的配置来完成此操作）。 下面是此方法的示例。

```csharp
protected void Page_Load(object sender, EventArgs e)
{
    if (!User.Identity.IsAuthenticated)
    {
        FormsAuthentication.RedirectToLoginPage();
    }
    if (!Roles.IsUserInRole(User.Identity.Name, "Administrators"))
    {
        MessageLabel.Text = "Only administrators can view this.";
        SecretPanel.Visible = false;
    }
}
```

在上面的代码中，基于角色的访问控制 (RBAC) 用于根据当前用户的角色来确定页的某些元素（例如 `SecretPanel`）是否可见。

通常情况下，ASP.NET Web Forms 应用程序在 `web.config` 文件中配置安全性，然后根据需要在 `.aspx` 页及其相关 `.aspx.cs` 代码隐藏文件中添加其他检查。 大多数应用程序使用通用成员资格提供程序，并经常与其他角色提供程序一起使用。

## <a name="aspnet-core-identity"></a>ASP.NET Core 标识

尽管仍然需要进行身份验证和授权，但与通用提供程序相比，ASP.NET Core Identity 使用一组不同的抽象和假设。 例如，新的 Identity 模型支持第三方身份验证，从而允许用户使用社交媒体帐户或其他受信任的身份验证提供程序进行身份验证。 ASP.NET Core Identity 支持常用页的 UI，例如登录、注销和注册。 它将 EF Core 用于其数据访问，并使用 EF Core 迁移来生成支持其数据模型所需的必要架构。 此 [ASP.NET Core 上的 Identity 简介](/aspnet/core/security/authentication/identity)很好地概要介绍了 ASP.NET Core Identity 包含的功能以及如何开始使用它。 如果尚未在应用程序及其数据库中设置 ASP.NET Core Identity，它将帮助你开始设置。

### <a name="roles-claims-and-policies"></a>角色、声明和策略

通用提供程序和 ASP.NET Core Identity 均支持角色的概念。 可以为用户创建角色，并将用户分配到角色。 用户可以属于任意数量的角色，并且可以在授权实现过程中验证角色成员资格。

除角色外，ASP.NET Core Identity 身份还支持声明和策略的概念。 尽管角色应专门对应于属于该角色的用户应能够访问的一组资源，但声明只是用户标识的一部分。 声明是一个名称值对，表示使用者是什么，而不是使用者可以做什么。

可以直接检查用户的声明并根据这些值确定是否应授予用户访问某个资源的权限。 但是，此类检查通常是重复的，并分散在整个系统中。 更好的方法是定义策略。

授权策略包含一个或多个要求。 策略在 `Startup.cs` 的 `ConfigureServices` 方法中作为授权服务配置的一部分注册。 例如，以下代码片段配置名为“CanadiansOnly”的策略，该策略要求用户拥有值为“Canada”的 Country 声明。

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("CanadiansOnly", policy => policy.RequireClaim(ClaimTypes.Country, "Canada"));
});
```

可以[在文档中详细了解如何创建自定义策略](/aspnet/core/security/authorization/policies)。

无论使用的是策略还是角色，都可以指定 Blazor 应用程序中的特定页要求角色或策略具有 `[Authorize]` 属性（通过 `@attribute` 指令应用）。

要求具有某个角色：

```csharp
@attribute [Authorize(Roles ="administrators")]
```

要求满足某个策略：

```csharp
@attribute [Authorize(Policy ="CanadiansOnly")]
```

如果需要访问代码中用户的身份验证状态、角色或声明，则可以通过两种主要方法实现此功能。 第一种方法是接收身份验证状态作为级联参数。 第二种方法是使用注入的 `AuthenticationStateProvider` 访问状态。 [Blazor 安全文档](/aspnet/core/blazor/security/)中详细介绍了其中每种方法。

以下代码显示如何接收 `AuthenticationState` 作为级联参数：

```csharp
[CascadingParameter]
private Task<AuthenticationState> authenticationStateTask { get; set; }
```

拥有此参数后，可以使用以下代码获取用户：

```csharp
var authState = await authenticationStateTask;
var user = authState.User;
```

以下代码显示如何注入 `AuthenticationStateProvider`：

```csharp
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider
```

拥有提供程序后，可以使用以下代码获取对用户的访问权限：

```csharp
AuthenticationState authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
ClaimsPrincipal user = authState.User;

if (user.Identity.IsAuthenticated)
{
  // work with user.Claims and/or user.Roles
}
```

**注意：** 本章节稍后将介绍的 `AuthorizeView` 组件提供声明性方法来控制用户在页或组件上看到的内容。

若要使用用户和声明（在 Blazor Server 应用程序中），可能还需要注入 `UserManager<T>`（默认使用 `IdentityUser`），该对象可用于枚举和修改用户的声明。 首先注入类型并将其分配到属性：

```csharp
@inject UserManager<IdentityUser> MyUserManager
```

然后使用它来处理用户的声明。 下面的示例演示如何在用户上添加和保存声明：

```csharp
private async Task AddCountryClaim()
{
    var authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
    var user = authState.User;
    var identityUser = await MyUserManager.FindByNameAsync(user.Identity.Name);

    if (!user.HasClaim(c => c.Type == ClaimTypes.Country))
    {
        // stores the claim in the cookie
        ClaimsIdentity id = new ClaimsIdentity();
        id.AddClaim(new Claim(ClaimTypes.Country, "Canada"));
        user.AddIdentity(id);

        // save the claim in the database
        await MyUserManager.AddClaimAsync(identityUser, new Claim(ClaimTypes.Country, "Canada"));
    }
}
```

如果需要使用角色，请遵循相同的方法。 可能需要插入 `RoleManager<T>`（默认类型为 `IdentityRole`）来列出和管理角色本身。

**注意：** 在 Blazor WebAssembly 项目中，将需要提供服务器 API 来执行这些操作（而不是直接使用 `UserManager<T>` 或 `RoleManager<T>`）。 Blazor WebAssembly 客户端应用程序将通过安全调用为此目的公开的 API 终结点来管理声明和/或角色。

### <a name="migration-guide"></a>迁移指南

从 ASP.NET Web Forms 和通用提供程序迁移到 ASP.NET Core Identity 需要执行多个步骤：

1. 在目标数据库中创建 ASP.NET Core Identity 数据库架构
2. 将数据从通用提供程序架构迁移到 ASP.NET Core Identity 架构
3. 将配置从 `web.config` 迁移到中间件和服务（通常在 `Startup.cs` 中）
4. 使用控件和条件更新单个页，以使用标记帮助程序和新的标识 API。

以下部分详细介绍了这些步骤。

### <a name="creating-the-aspnet-core-identity-schema"></a>创建 ASP.NET Core Identity 架构

有多种方法可以创建用于 ASP.NET Core Identity 的必要表结构。 最简单的方法是创建新的 ASP.NET Core Web 应用程序。 选择“Web 应用程序”，然后“更改身份验证”以使用“个人用户帐户”。

![具有个人用户帐户的新项目](./media/security/individual-user-accounts.png)

在命令行中，可以通过运行 `dotnet new webapp -au Individual` 来执行相同的操作。 创建应用后，请运行它并在站点上注册。 应该触发如下所示的页：

![应用迁移页](./media/security/apply-migrations-page.png)

单击“应用迁移”按钮，此时应为你创建必要的数据库表。 此外，迁移文件应在项目中显示，如下所示：

![迁移文件](./media/security/migration-files.png)

可以使用以下命令行工具自行运行迁移，而无需运行 Web 应用程序：

```powershell
dotnet ef database update
```

如果希望运行脚本来将新架构应用到现有数据库，则可以从命令行编写这些迁移的脚本。 运行以下命令以生成脚本：

```powershell
dotnet ef migrations script -o auth.sql
```

上面的命令将在输出文件 `auth.sql` 中生成 SQL 脚本，然后可以针对所需的任何数据库运行该脚本。 如果在运行 `dotnet ef` 命令时遇到任何问题，请[确保系统上已安装 EF Core 工具](/ef/core/miscellaneous/cli/dotnet)。

如果源表上还有其他列，则需要在新架构中为这些列标识最佳位置。 一般情况下，在 `aspnet_Membership` 表上找到的列应映射到 `AspNetUsers` 表。 `aspnet_Roles` 上的列应映射到 `AspNetRoles`。 `aspnet_UsersInRoles` 表上的所有其他列都将添加到 `AspNetUserRoles` 表中。

还值得考虑将所有其他列放到单独的表中。 这样，将来的迁移就无需考虑默认标识架构的此类自定义项。

### <a name="migrating-data-from-universal-providers-to-aspnet-core-identity"></a>将数据从通用提供程序迁移到 ASP.NET Core Identity

拥有目标表架构后，下一步是将用户和角色记录迁移到新架构。 可在[此处](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)找到架构区别的完整列表，包括哪些列映射到哪些新列。

若要将用户从成员资格迁移到新的标识表，应[遵循文档中所述的步骤](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)。 完成这些步骤并提供脚本后，用户将需要在下次登录时更改其密码。

可以迁移用户密码，但该过程涉及的操作要多得多。 要求用户在迁移过程中更新密码，并鼓励他们使用新的唯一密码，这可能增强应用程序的整体安全性。

### <a name="migrating-security-settings-from-webconfig-to-startupcs"></a>将安全设置从 web.config 迁移到 Startup.cs

如上所述，在应用程序的 `web.config` 文件中配置了 ASP.NET 成员资格和角色提供程序。 由于 ASP.NET Core 应用未绑定到 IIS 并使用单独的系统进行配置，因此必须在其他位置配置这些设置。 在大多数情况下，ASP.NET Core Identity 在 `Startup.cs` 文件中配置。 打开先前创建的 Web 项目（用于生成标识表架构），并查看其 `Startup.cs` 文件。

默认的 ConfigureServices 方法添加对 EF Core 和 Identity 的支持：

```csharp
// This method gets called by the runtime. Use this method to add services to the container.
public void ConfigureServices(IServiceCollection services)
{
    services.AddDbContext<ApplicationDbContext>(options =>
        options.UseSqlServer(
            Configuration.GetConnectionString("DefaultConnection")));

    services.AddDefaultIdentity<IdentityUser>(options => options.SignIn.RequireConfirmedAccount = true)
        .AddEntityFrameworkStores<ApplicationDbContext>();

    services.AddRazorPages();
}
```

`AddDefaultIdentity` 扩展方法用于将 Identity 配置为使用默认的 `ApplicationDbContext` 和框架的 `IdentityUser` 类型。 如果使用自定义 `IdentityUser`，请确保在此处指定其类型。 如果这些扩展方法在应用程序中无法正常工作，请检查是否具有适当的 using 语句，以及是否具有必需的 NuGet 包引用。 例如，项目应具有引用的 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 和 `Microsoft.AspNetCore.Identity.UI` 包的某些版本。

同样，在 `Startup.cs` 中，应该看到为站点配置的必要中间件。 具体来说，应设置 `UseAuthentication` 和 `UseAuthorization` 且其应该在正确的位置。

```csharp

// This method gets called by the runtime. Use this method to configure the HTTP request pipeline.
public void Configure(IApplicationBuilder app, IWebHostEnvironment env)
{
    if (env.IsDevelopment())
    {
        app.UseDeveloperExceptionPage();
        app.UseDatabaseErrorPage();
    }
    else
    {
        app.UseExceptionHandler("/Error");
        // The default HSTS value is 30 days. You may want to change this for production scenarios, see https://aka.ms/aspnetcore-hsts.
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();

    app.UseRouting();

    app.UseAuthentication();
    app.UseAuthorization();

    app.UseEndpoints(endpoints =>
    {
        endpoints.MapRazorPages();
    });
}
```

ASP.NET Identity 不配置从 `Startup.cs` 对位置的匿名访问或基于角色的访问。 需要将所有位置特定的授权配置数据迁移到 ASP.NET Core 中的筛选器。 记下哪些文件夹和页将需要此类更新。 你将在下一部分中进行这些更改。

### <a name="updating-individual-pages-to-use-aspnet-core-identity-abstractions"></a>更新各个页以使用 ASP.NET Core Identity 抽象

在 ASP.NET Web Forms 应用程序中，如果具有用于拒绝匿名用户访问某些页或文件夹的 `web.config` 设置，则可以通过向此类页添加 `[Authorize]` 属性来迁移这些更改：

```razor
@attribute [Authorize]
```

如果进一步拒绝不属于某个角色的用户进行访问，则同样可以通过添加指定角色的属性来迁移此行为：

```razor
@attribute [Authorize(Roles ="administrators")]
```

`[Authorize]` 属性仅适用于通过 Blazor 路由器到达的 `@page` 组件。 该属性不适用于子组件，子组件应使用 `AuthorizeView`。

如果页标记中有用于确定是否向某个用户显示某些代码的逻辑，则可以使用 `AuthorizeView` 组件进行替换。 [AuthorizeView 组件](/aspnet/core/blazor/security#authorizeview-component)根据用户是否获得查看授权来有选择地显示 UI。 它还公开可用于访问用户信息的 `context` 变量。

```razor
<AuthorizeView>
    <Authorized>
        <h1>Hello, @context.User.Identity.Name!</h1>
        <p>You can only see this content if you are authenticated.</p>
    </Authorized>
    <NotAuthorized>
        <h1>Authentication Failure!</h1>
        <p>You are not signed in.</p>
    </NotAuthorized>
</AuthorizeView>
```

可以通过从配置了 `[CascadingParameter]` 属性的 `Task<AuthenticationState` 访问用户来访问过程逻辑中的身份验证状态。 借助此配置，你可以访问用户，这让你能够确定他们是否已经过身份验证以及他们是否属于特定角色。 如果需要在过程中评估策略，则可以注入 `IAuthorizationService` 的实例并在其上调用 `AuthorizeAsync` 方法。 以下示例代码演示如何获取用户信息以及如何允许授权用户执行受 `content-editor` 策略限制的任务。

```razor
@using Microsoft.AspNetCore.Authorization
@inject IAuthorizationService AuthorizationService

<button @onclick="@DoSomething">Do something important</button>

@code {
    [CascadingParameter]
    private Task<AuthenticationState> authenticationStateTask { get; set; }

    private async Task DoSomething()
    {
        var user = (await authenticationStateTask).User;

        if (user.Identity.IsAuthenticated)
        {
            // Perform an action only available to authenticated (signed-in) users.
        }

        if (user.IsInRole("admin"))
        {
            // Perform an action only available to users in the 'admin' role.
        }

        if ((await AuthorizationService.AuthorizeAsync(user, "content-editor"))
            .Succeeded)
        {
            // Perform an action only available to users satisfying the
            // 'content-editor' policy.
        }
    }
}
```

需要先将 `AuthenticationState` 设置为级联值，然后才能将其绑定到此类级联参数。 通常使用 `CascadingAuthenticationState` 组件完成此操作。 此配置通常在 `App.razor` 中完成：

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData"
                DefaultLayout="@typeof(MainLayout)" />
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```

## <a name="summary"></a>总结

Blazor 使用与 ASP.NET Core 相同的安全模型，即 ASP.NET Core Identity。 从通用提供程序迁移到 ASP.NET Core Identity 相对简单，前提是没有对原始数据架构应用太多自定义项。 迁移数据后，Blazor 应用中的身份验证和授权工作将得到良好记录，并具有针对大多数安全要求的可配置和编程支持。

## <a name="references"></a>参考

- [ASP.NET Core 上的 Identity 简介](/aspnet/core/security/authentication/identity)
- [从 ASP.NET Membership 身份验证迁移到 ASP.NET Core 2.0 Identity](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)
- [将身份验证和标识迁移到 ASP.NET Core](/aspnet/core/migration/identity)
- [ASP.NET Core Blazor 身份验证和授权](/aspnet/core/blazor/security/)

>[!div class="step-by-step"]
>[上一页](config.md)
>[下一页](migration.md)
