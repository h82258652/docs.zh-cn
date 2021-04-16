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
# <a name="security-authentication-and-authorization-in-aspnet-web-forms-and-blazor"></a><span data-ttu-id="5c766-103">安全性：ASP.NET Web Forms 和 Blazor  中的身份验证和授权</span><span class="sxs-lookup"><span data-stu-id="5c766-103">Security: Authentication and Authorization in ASP.NET Web Forms and Blazor</span></span>

<span data-ttu-id="5c766-104">从 ASP.NET Web Forms 应用程序迁移到 Blazor 几乎肯定需要更新身份验证和授权的执行方式（假设应用程序已配置身份验证）。</span><span class="sxs-lookup"><span data-stu-id="5c766-104">Migrating from an ASP.NET Web Forms application to Blazor will almost certainly require updating how authentication and authorization are performed, assuming the application had authentication configured.</span></span> <span data-ttu-id="5c766-105">本章节介绍如何从 ASP.NET Web Forms 通用提供程序模型（用于成员资格、角色和用户配置文件）进行迁移，以及如何从 Blazor 应用使用 ASP.NET Core Identity。</span><span class="sxs-lookup"><span data-stu-id="5c766-105">This chapter will cover how to migrate from the ASP.NET Web Forms universal provider model (for membership, roles, and user profiles) and how to work with ASP.NET Core Identity from Blazor apps.</span></span> <span data-ttu-id="5c766-106">尽管本章节将介绍大致步骤和注意事项，但可在参考文档中找到详细步骤和脚本。</span><span class="sxs-lookup"><span data-stu-id="5c766-106">While this chapter will cover the high-level steps and considerations, the detailed steps and scripts may be found in the referenced documentation.</span></span>

## <a name="aspnet-universal-providers"></a><span data-ttu-id="5c766-107">ASP.NET 通用提供程序</span><span class="sxs-lookup"><span data-stu-id="5c766-107">ASP.NET universal providers</span></span>

<span data-ttu-id="5c766-108">从 ASP.NET 2.0 开始，ASP.NET Web Forms 平台支持用于各种功能（包括成员资格）的提供程序模型。</span><span class="sxs-lookup"><span data-stu-id="5c766-108">Since ASP.NET 2.0, the ASP.NET Web Forms platform has supported a provider model for a variety of features, including membership.</span></span> <span data-ttu-id="5c766-109">通用成员资格提供程序以及可选的角色提供程序通常与 ASP.NET Web Forms 应用程序一起部署。</span><span class="sxs-lookup"><span data-stu-id="5c766-109">The universal membership provider, along with the optional role provider, is commonly deployed with ASP.NET Web Forms applications.</span></span> <span data-ttu-id="5c766-110">它提供了一种管理身份验证和授权的可靠且安全的方式，且这种方式现今仍然可以正常运行。</span><span class="sxs-lookup"><span data-stu-id="5c766-110">It offers a robust and secure way to manage authentication and authorization that continues to work well today.</span></span> <span data-ttu-id="5c766-111">这些通用提供程序的最新产品/服务是 NuGet 包 [Microsoft.AspNet.Providers](https://www.nuget.org/packages/Microsoft.AspNet.Providers)。</span><span class="sxs-lookup"><span data-stu-id="5c766-111">The most recent offering of these universal providers is available as a NuGet package, [Microsoft.AspNet.Providers](https://www.nuget.org/packages/Microsoft.AspNet.Providers).</span></span>

<span data-ttu-id="5c766-112">通用提供程序适用于包含 `aspnet_Applications`、`aspnet_Membership`、`aspnet_Roles` 和 `aspnet_Users` 等表的 SQL 数据库架构。</span><span class="sxs-lookup"><span data-stu-id="5c766-112">The Universal Providers work with a SQL database schema that includes tables like `aspnet_Applications`, `aspnet_Membership`, `aspnet_Roles`, and `aspnet_Users`.</span></span> <span data-ttu-id="5c766-113">通过运行 [aspnet_regsql.exe 命令](/previous-versions/ms229862(v=vs.140))进行配置时，提供程序将安装表和存储过程，这些表和存储过程提供使用基础数据所需的所有必要查询和命令。</span><span class="sxs-lookup"><span data-stu-id="5c766-113">When configured by running the [aspnet_regsql.exe command](/previous-versions/ms229862(v=vs.140)), the providers install tables and stored procedures that provide all of the necessary queries and commands to work with the underlying data.</span></span> <span data-ttu-id="5c766-114">数据库架构和这些存储过程与较新的 ASP.NET Identity 和 ASP.NET Core Identity 系统不兼容，因此必须将现有数据迁移到新系统。</span><span class="sxs-lookup"><span data-stu-id="5c766-114">The database schema and these stored procedures are not compatible with newer ASP.NET Identity and ASP.NET Core Identity systems, so existing data must be migrated into the new system.</span></span> <span data-ttu-id="5c766-115">图 1 显示为通用提供程序配置的示例表架构。</span><span class="sxs-lookup"><span data-stu-id="5c766-115">Figure 1 shows an example table schema configured for universal providers.</span></span>

![通用提供程序架构](./media/security/membership-tables.png)

<span data-ttu-id="5c766-117&quot;>通用提供程序负责处理用户、成员资格、角色和配置文件。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-117&quot;>The universal provider handles users, membership, roles, and profiles.</span></span> <span data-ttu-id=&quot;5c766-118&quot;>为用户分配了全局唯一标识符，且基本信息（如 userId、userName 等）存储在 `aspnet_Users` 表中。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-118&quot;>Users are assigned globally unique identifiers and basic information like userId, userName etc. are stored in the `aspnet_Users` table.</span></span> <span data-ttu-id=&quot;5c766-119&quot;>身份验证信息（例如密码、密码格式、密码加密盐、锁定计数器和详细信息等）存储在 `aspnet_Membership` 表中。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-119&quot;>Authentication information, such as password, password format, password salt, lockout counters and details, etc. are stored in the `aspnet_Membership` table.</span></span> <span data-ttu-id=&quot;5c766-120&quot;>角色仅包含名称和唯一标识符，它们通过 `aspnet_UsersInRoles` 关联表分配给用户，从而提供多对多关系。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-120&quot;>Roles consist simply of names and unique identifiers, which are assigned to users via the `aspnet_UsersInRoles` association table, providing a many-to-many relationship.</span></span>

<span data-ttu-id=&quot;5c766-121&quot;>如果现有系统除使用成员资格外还使用角色，则需要将用户帐户、关联密码、角色和角色成员资格迁移到 ASP.NET Core Identity。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-121&quot;>If your existing system is using roles in addition to membership, you will need to migrate the user accounts, the associated passwords, the roles, and the role membership into ASP.NET Core Identity.</span></span> <span data-ttu-id=&quot;5c766-122&quot;>还很有可能需要将当前使用 if 语句执行角色检查的代码更新为使用声明性筛选器、属性和/或标记帮助程序。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-122&quot;>You will also most likely need to update your code where you're currently performing role checks using if statements to instead leverage declarative filters, attributes, and/or tag helpers.</span></span> <span data-ttu-id=&quot;5c766-123&quot;>我们将在本章节的末尾详细介绍迁移注意事项。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-123&quot;>We will review migration considerations in greater detail at the end of this chapter.</span></span>

### <a name=&quot;authorization-configuration-in-web-forms&quot;></a><span data-ttu-id=&quot;5c766-124&quot;>Web Forms 中的授权配置</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-124&quot;>Authorization configuration in Web Forms</span></span>

<span data-ttu-id=&quot;5c766-125&quot;>若要配置对 ASP.NET Web Forms 应用程序中某些页的授权访问，通常可将特定页或文件夹指定为匿名用户无法访问。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-125&quot;>To configure authorized access to certain pages in an ASP.NET Web Forms application, typically you specify that certain pages or folders are inaccessible to anonymous users.</span></span> <span data-ttu-id=&quot;5c766-126&quot;>此配置在 web.config 文件中完成：</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-126&quot;>This configuration is done in the web.config file:</span></span>

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

<span data-ttu-id=&quot;5c766-127&quot;>`authentication` 配置节为应用程序设置表单身份验证。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-127&quot;>The `authentication` configuration section sets up the forms authentication for the application.</span></span> <span data-ttu-id=&quot;5c766-128&quot;>`authorization` 节用于禁止匿名用户使用整个应用程序。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-128&quot;>The `authorization` section is used to disallow anonymous users for the entire application.</span></span> <span data-ttu-id=&quot;5c766-129&quot;>但是，可以按位置提供更精细的授权规则，也可以应用基于角色的授权检查。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-129&quot;>However, you can provide more granular authorization rules on a per-location basis as well as apply role-based authorization checks.</span></span>

```xml
<location path=&quot;login.aspx&quot;>
  <system.web>
    <authorization>
      <allow users=&quot;*&quot; />
    </authorization>
  </system.web>
</location>
```

<span data-ttu-id=&quot;5c766-130&quot;>以上配置与第一个配置结合使用时，将允许匿名用户访问登录页，从而替代站点范围内对未经身份验证的用户的限制。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-130&quot;>The above configuration, when combined with the first one, would allow anonymous users to access the login page, overriding the site-wide restriction on non-authenticated users.</span></span>

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

<span data-ttu-id=&quot;5c766-131&quot;>以上配置与其他配置结合使用时，将对 `/admin` 文件夹及其中所有资源的访问权限限制为“管理员”角色的成员。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-131&quot;>The above configuration, when combined with the others, restricts access to the `/admin` folder and all resources within it to members of the &quot;Administrators&quot; role.</span></span> <span data-ttu-id=&quot;5c766-132&quot;>还可以通过在 `/admin` 文件夹根目录内放置单独的 `web.config` 文件来应用此限制。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-132&quot;>This restriction could also be applied by placing a separate `web.config` file within the `/admin` folder root.</span></span>

### <a name=&quot;authorization-code-in-web-forms&quot;></a><span data-ttu-id=&quot;5c766-133&quot;>Web Forms 中的授权代码</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-133&quot;>Authorization code in Web Forms</span></span>

<span data-ttu-id=&quot;5c766-134&quot;>除使用 `web.config` 配置访问权限外，还可以在 Web Forms 应用程序中以编程方式配置访问权限和行为。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-134&quot;>In addition to configuring access using `web.config`, you can also programmatically configure access and behavior in your Web Forms application.</span></span> <span data-ttu-id=&quot;5c766-135&quot;>例如，可以根据用户的角色来限制执行某些操作或查看某些数据的能力。</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-135&quot;>For instance, you can restrict the ability to perform certain operations or view certain data based on the user's role.</span></span>

<span data-ttu-id=&quot;5c766-136&quot;>此代码既可以在代码隐藏逻辑中使用，也可以在页本身使用：</span><span class=&quot;sxs-lookup&quot;><span data-stu-id=&quot;5c766-136&quot;>This code can be used both in code-behind logic as well as in the page itself:</span></span>

```html
<% if (HttpContext.Current.User.IsInRole(&quot;Administrators")) { %>
  <a href="/admin">Go To Admin</a>
<% } %>
```

<span data-ttu-id="5c766-137">除检查用户角色成员资格外，还可以确定其是否已经过身份验证（尽管通常最好使用上面介绍的基于位置的配置来完成此操作）。</span><span class="sxs-lookup"><span data-stu-id="5c766-137">In addition to checking user role membership, you can also determine if they are authenticated (though often this is better done using the location-based configuration covered above).</span></span> <span data-ttu-id="5c766-138">下面是此方法的示例。</span><span class="sxs-lookup"><span data-stu-id="5c766-138">Below is an example of this approach.</span></span>

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

<span data-ttu-id="5c766-139">在上面的代码中，基于角色的访问控制 (RBAC) 用于根据当前用户的角色来确定页的某些元素（例如 `SecretPanel`）是否可见。</span><span class="sxs-lookup"><span data-stu-id="5c766-139">In the code above, role-based access control (RBAC) is used to determine whether certain elements of the page, such as a `SecretPanel`, are visible based on the current user's role.</span></span>

<span data-ttu-id="5c766-140">通常情况下，ASP.NET Web Forms 应用程序在 `web.config` 文件中配置安全性，然后根据需要在 `.aspx` 页及其相关 `.aspx.cs` 代码隐藏文件中添加其他检查。</span><span class="sxs-lookup"><span data-stu-id="5c766-140">Typically, ASP.NET Web Forms applications configure security within the `web.config` file and then add additional checks where needed in `.aspx` pages and their related `.aspx.cs` code-behind files.</span></span> <span data-ttu-id="5c766-141">大多数应用程序使用通用成员资格提供程序，并经常与其他角色提供程序一起使用。</span><span class="sxs-lookup"><span data-stu-id="5c766-141">Most applications leverage the universal membership provider, frequently with the additional role provider.</span></span>

## <a name="aspnet-core-identity"></a><span data-ttu-id="5c766-142">ASP.NET Core 标识</span><span class="sxs-lookup"><span data-stu-id="5c766-142">ASP.NET Core Identity</span></span>

<span data-ttu-id="5c766-143">尽管仍然需要进行身份验证和授权，但与通用提供程序相比，ASP.NET Core Identity 使用一组不同的抽象和假设。</span><span class="sxs-lookup"><span data-stu-id="5c766-143">Although still tasked with authentication and authorization, ASP.NET Core Identity uses a different set of abstractions and assumptions when compared to the universal providers.</span></span> <span data-ttu-id="5c766-144">例如，新的 Identity 模型支持第三方身份验证，从而允许用户使用社交媒体帐户或其他受信任的身份验证提供程序进行身份验证。</span><span class="sxs-lookup"><span data-stu-id="5c766-144">For example, the new Identity model supports third party authentication, allowing users to authenticate using a social media account or other trusted authentication provider.</span></span> <span data-ttu-id="5c766-145">ASP.NET Core Identity 支持常用页的 UI，例如登录、注销和注册。</span><span class="sxs-lookup"><span data-stu-id="5c766-145">ASP.NET Core Identity supports UI for commonly needed pages like login, logout, and register.</span></span> <span data-ttu-id="5c766-146">它将 EF Core 用于其数据访问，并使用 EF Core 迁移来生成支持其数据模型所需的必要架构。</span><span class="sxs-lookup"><span data-stu-id="5c766-146">It leverages EF Core for its data access, and uses EF Core migrations to generate the necessary schema required to support its data model.</span></span> <span data-ttu-id="5c766-147">此 [ASP.NET Core 上的 Identity 简介](/aspnet/core/security/authentication/identity)很好地概要介绍了 ASP.NET Core Identity 包含的功能以及如何开始使用它。</span><span class="sxs-lookup"><span data-stu-id="5c766-147">This [introduction to Identity on ASP.NET Core](/aspnet/core/security/authentication/identity) provides a good overview of what is included with ASP.NET Core Identity and how to get started working with it.</span></span> <span data-ttu-id="5c766-148">如果尚未在应用程序及其数据库中设置 ASP.NET Core Identity，它将帮助你开始设置。</span><span class="sxs-lookup"><span data-stu-id="5c766-148">If you haven't already set up ASP.NET Core Identity in your application and its database, it will help you get started.</span></span>

### <a name="roles-claims-and-policies"></a><span data-ttu-id="5c766-149">角色、声明和策略</span><span class="sxs-lookup"><span data-stu-id="5c766-149">Roles, claims, and policies</span></span>

<span data-ttu-id="5c766-150">通用提供程序和 ASP.NET Core Identity 均支持角色的概念。</span><span class="sxs-lookup"><span data-stu-id="5c766-150">Both the universal providers and ASP.NET Core Identity support the concept of roles.</span></span> <span data-ttu-id="5c766-151">可以为用户创建角色，并将用户分配到角色。</span><span class="sxs-lookup"><span data-stu-id="5c766-151">You can create roles for users and assign users to roles.</span></span> <span data-ttu-id="5c766-152">用户可以属于任意数量的角色，并且可以在授权实现过程中验证角色成员资格。</span><span class="sxs-lookup"><span data-stu-id="5c766-152">Users can belong to any number of roles, and you can verify role membership as part of your authorization implementation.</span></span>

<span data-ttu-id="5c766-153">除角色外，ASP.NET Core Identity 身份还支持声明和策略的概念。</span><span class="sxs-lookup"><span data-stu-id="5c766-153">In addition to roles, ASP.NET Core identity supports the concepts of claims and policies.</span></span> <span data-ttu-id="5c766-154">尽管角色应专门对应于属于该角色的用户应能够访问的一组资源，但声明只是用户标识的一部分。</span><span class="sxs-lookup"><span data-stu-id="5c766-154">While a role should specifically correspond to a set of resources a user in that role should be able to access, a claim is simply part of a user's identity.</span></span> <span data-ttu-id="5c766-155">声明是一个名称值对，表示使用者是什么，而不是使用者可以做什么。</span><span class="sxs-lookup"><span data-stu-id="5c766-155">A claim is a name value pair that represents what the subject is, not what the subject can do.</span></span>

<span data-ttu-id="5c766-156">可以直接检查用户的声明并根据这些值确定是否应授予用户访问某个资源的权限。</span><span class="sxs-lookup"><span data-stu-id="5c766-156">It is possible to directly inspect a user's claims and determine based on these values whether a user should be given access to a resource.</span></span> <span data-ttu-id="5c766-157">但是，此类检查通常是重复的，并分散在整个系统中。</span><span class="sxs-lookup"><span data-stu-id="5c766-157">However, such checks are often repetitive and scattered throughout the system.</span></span> <span data-ttu-id="5c766-158">更好的方法是定义策略。</span><span class="sxs-lookup"><span data-stu-id="5c766-158">A better approach is to define a *policy*.</span></span>

<span data-ttu-id="5c766-159">授权策略包含一个或多个要求。</span><span class="sxs-lookup"><span data-stu-id="5c766-159">An authorization policy consists of one or more requirements.</span></span> <span data-ttu-id="5c766-160">策略在 `Startup.cs` 的 `ConfigureServices` 方法中作为授权服务配置的一部分注册。</span><span class="sxs-lookup"><span data-stu-id="5c766-160">Policies are registered as part of the authorization service configuration in the `ConfigureServices` method of `Startup.cs`.</span></span> <span data-ttu-id="5c766-161">例如，以下代码片段配置名为“CanadiansOnly”的策略，该策略要求用户拥有值为“Canada”的 Country 声明。</span><span class="sxs-lookup"><span data-stu-id="5c766-161">For example, the following code snippet configures a policy called "CanadiansOnly", which has the requirement that the user has the Country claim with the value of "Canada".</span></span>

```csharp
services.AddAuthorization(options =>
{
    options.AddPolicy("CanadiansOnly", policy => policy.RequireClaim(ClaimTypes.Country, "Canada"));
});
```

<span data-ttu-id="5c766-162">可以[在文档中详细了解如何创建自定义策略](/aspnet/core/security/authorization/policies)。</span><span class="sxs-lookup"><span data-stu-id="5c766-162">You can [learn more about how to create custom policies in the documentation](/aspnet/core/security/authorization/policies).</span></span>

<span data-ttu-id="5c766-163">无论使用的是策略还是角色，都可以指定 Blazor 应用程序中的特定页要求角色或策略具有 `[Authorize]` 属性（通过 `@attribute` 指令应用）。</span><span class="sxs-lookup"><span data-stu-id="5c766-163">Whether you're using policies or roles, you can specify that a particular page in your Blazor application requires that role or policy with the `[Authorize]` attribute, applied with the `@attribute` directive.</span></span>

<span data-ttu-id="5c766-164">要求具有某个角色：</span><span class="sxs-lookup"><span data-stu-id="5c766-164">Requiring a role:</span></span>

```csharp
@attribute [Authorize(Roles ="administrators")]
```

<span data-ttu-id="5c766-165">要求满足某个策略：</span><span class="sxs-lookup"><span data-stu-id="5c766-165">Requiring a policy be satisfied:</span></span>

```csharp
@attribute [Authorize(Policy ="CanadiansOnly")]
```

<span data-ttu-id="5c766-166">如果需要访问代码中用户的身份验证状态、角色或声明，则可以通过两种主要方法实现此功能。</span><span class="sxs-lookup"><span data-stu-id="5c766-166">If you need access to a user's authentication state, roles, or claims in your code, there are two primary ways to achieve this functionality.</span></span> <span data-ttu-id="5c766-167">第一种方法是接收身份验证状态作为级联参数。</span><span class="sxs-lookup"><span data-stu-id="5c766-167">The first is to receive the authentication state as a cascading parameter.</span></span> <span data-ttu-id="5c766-168">第二种方法是使用注入的 `AuthenticationStateProvider` 访问状态。</span><span class="sxs-lookup"><span data-stu-id="5c766-168">The second is to access the state using an injected `AuthenticationStateProvider`.</span></span> <span data-ttu-id="5c766-169">[Blazor 安全文档](/aspnet/core/blazor/security/)中详细介绍了其中每种方法。</span><span class="sxs-lookup"><span data-stu-id="5c766-169">The details of each of these approaches are described in the [Blazor Security documentation](/aspnet/core/blazor/security/).</span></span>

<span data-ttu-id="5c766-170">以下代码显示如何接收 `AuthenticationState` 作为级联参数：</span><span class="sxs-lookup"><span data-stu-id="5c766-170">The following code shows how to receive the `AuthenticationState` as a cascading parameter:</span></span>

```csharp
[CascadingParameter]
private Task<AuthenticationState> authenticationStateTask { get; set; }
```

<span data-ttu-id="5c766-171">拥有此参数后，可以使用以下代码获取用户：</span><span class="sxs-lookup"><span data-stu-id="5c766-171">With this parameter in place, you can get the user using this code:</span></span>

```csharp
var authState = await authenticationStateTask;
var user = authState.User;
```

<span data-ttu-id="5c766-172">以下代码显示如何注入 `AuthenticationStateProvider`：</span><span class="sxs-lookup"><span data-stu-id="5c766-172">The following code shows how to inject the `AuthenticationStateProvider`:</span></span>

```csharp
@using Microsoft.AspNetCore.Components.Authorization
@inject AuthenticationStateProvider AuthenticationStateProvider
```

<span data-ttu-id="5c766-173">拥有提供程序后，可以使用以下代码获取对用户的访问权限：</span><span class="sxs-lookup"><span data-stu-id="5c766-173">With the provider in place, you can gain access to the user with the following code:</span></span>

```csharp
AuthenticationState authState = await AuthenticationStateProvider.GetAuthenticationStateAsync();
ClaimsPrincipal user = authState.User;

if (user.Identity.IsAuthenticated)
{
  // work with user.Claims and/or user.Roles
}
```

<span data-ttu-id="5c766-174">**注意：** 本章节稍后将介绍的 `AuthorizeView` 组件提供声明性方法来控制用户在页或组件上看到的内容。</span><span class="sxs-lookup"><span data-stu-id="5c766-174">**Note:** The `AuthorizeView` component, covered later in this chapter, provides a declarative way to control what a user sees on a page or component.</span></span>

<span data-ttu-id="5c766-175">若要使用用户和声明（在 Blazor Server 应用程序中），可能还需要注入 `UserManager<T>`（默认使用 `IdentityUser`），该对象可用于枚举和修改用户的声明。</span><span class="sxs-lookup"><span data-stu-id="5c766-175">To work with users and claims (in Blazor Server applications) you may also need to inject a `UserManager<T>` (use `IdentityUser` for default) which you can use to enumerate and modify claims for a user.</span></span> <span data-ttu-id="5c766-176">首先注入类型并将其分配到属性：</span><span class="sxs-lookup"><span data-stu-id="5c766-176">First inject the type and assign it to a property:</span></span>

```csharp
@inject UserManager<IdentityUser> MyUserManager
```

<span data-ttu-id="5c766-177">然后使用它来处理用户的声明。</span><span class="sxs-lookup"><span data-stu-id="5c766-177">Then use it to work with the user's claims.</span></span> <span data-ttu-id="5c766-178">下面的示例演示如何在用户上添加和保存声明：</span><span class="sxs-lookup"><span data-stu-id="5c766-178">The following sample shows how to add and persist a claim on a user:</span></span>

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

<span data-ttu-id="5c766-179">如果需要使用角色，请遵循相同的方法。</span><span class="sxs-lookup"><span data-stu-id="5c766-179">If you need to work with roles, follow the same approach.</span></span> <span data-ttu-id="5c766-180">可能需要插入 `RoleManager<T>`（默认类型为 `IdentityRole`）来列出和管理角色本身。</span><span class="sxs-lookup"><span data-stu-id="5c766-180">You may need to inject a `RoleManager<T>` (use `IdentityRole` for default type) to list and manage the roles themselves.</span></span>

<span data-ttu-id="5c766-181">**注意：** 在 Blazor WebAssembly 项目中，将需要提供服务器 API 来执行这些操作（而不是直接使用 `UserManager<T>` 或 `RoleManager<T>`）。</span><span class="sxs-lookup"><span data-stu-id="5c766-181">**Note:** In Blazor WebAssembly projects, you will need to provide server APIs to perform these operations (instead of using `UserManager<T>` or `RoleManager<T>` directly).</span></span> <span data-ttu-id="5c766-182">Blazor WebAssembly 客户端应用程序将通过安全调用为此目的公开的 API 终结点来管理声明和/或角色。</span><span class="sxs-lookup"><span data-stu-id="5c766-182">A Blazor WebAssembly client application would manage claims and/or roles by securely calling API endpoints exposed for this purpose.</span></span>

### <a name="migration-guide"></a><span data-ttu-id="5c766-183">迁移指南</span><span class="sxs-lookup"><span data-stu-id="5c766-183">Migration guide</span></span>

<span data-ttu-id="5c766-184">从 ASP.NET Web Forms 和通用提供程序迁移到 ASP.NET Core Identity 需要执行多个步骤：</span><span class="sxs-lookup"><span data-stu-id="5c766-184">Migrating from ASP.NET Web Forms and universal providers to ASP.NET Core Identity requires several steps:</span></span>

1. <span data-ttu-id="5c766-185">在目标数据库中创建 ASP.NET Core Identity 数据库架构</span><span class="sxs-lookup"><span data-stu-id="5c766-185">Create ASP.NET Core Identity database schema in the destination database</span></span>
2. <span data-ttu-id="5c766-186">将数据从通用提供程序架构迁移到 ASP.NET Core Identity 架构</span><span class="sxs-lookup"><span data-stu-id="5c766-186">Migrate data from universal provider schema to ASP.NET Core Identity schema</span></span>
3. <span data-ttu-id="5c766-187">将配置从 `web.config` 迁移到中间件和服务（通常在 `Startup.cs` 中）</span><span class="sxs-lookup"><span data-stu-id="5c766-187">Migrate configuration from the `web.config` to middleware and services, typically in `Startup.cs`</span></span>
4. <span data-ttu-id="5c766-188">使用控件和条件更新单个页，以使用标记帮助程序和新的标识 API。</span><span class="sxs-lookup"><span data-stu-id="5c766-188">Update individual pages using controls and conditionals to use tag helpers and new identity APIs.</span></span>

<span data-ttu-id="5c766-189">以下部分详细介绍了这些步骤。</span><span class="sxs-lookup"><span data-stu-id="5c766-189">Each of these steps is described in detail in the following sections.</span></span>

### <a name="creating-the-aspnet-core-identity-schema"></a><span data-ttu-id="5c766-190">创建 ASP.NET Core Identity 架构</span><span class="sxs-lookup"><span data-stu-id="5c766-190">Creating the ASP.NET Core Identity schema</span></span>

<span data-ttu-id="5c766-191">有多种方法可以创建用于 ASP.NET Core Identity 的必要表结构。</span><span class="sxs-lookup"><span data-stu-id="5c766-191">There are several ways to create the necessary table structure used for ASP.NET Core Identity.</span></span> <span data-ttu-id="5c766-192">最简单的方法是创建新的 ASP.NET Core Web 应用程序。</span><span class="sxs-lookup"><span data-stu-id="5c766-192">The simplest is to create a new ASP.NET Core Web application.</span></span> <span data-ttu-id="5c766-193">选择“Web 应用程序”，然后“更改身份验证”以使用“个人用户帐户”。</span><span class="sxs-lookup"><span data-stu-id="5c766-193">Choose Web Application and then change Authentication to use Individual User Accounts.</span></span>

![具有个人用户帐户的新项目](./media/security/individual-user-accounts.png)

<span data-ttu-id="5c766-195">在命令行中，可以通过运行 `dotnet new webapp -au Individual` 来执行相同的操作。</span><span class="sxs-lookup"><span data-stu-id="5c766-195">From the command line, you can do the same thing by running `dotnet new webapp -au Individual`.</span></span> <span data-ttu-id="5c766-196">创建应用后，请运行它并在站点上注册。</span><span class="sxs-lookup"><span data-stu-id="5c766-196">Once the app has been created, run it and register on the site.</span></span> <span data-ttu-id="5c766-197">应该触发如下所示的页：</span><span class="sxs-lookup"><span data-stu-id="5c766-197">You should trigger a page like the one shown below:</span></span>

![应用迁移页](./media/security/apply-migrations-page.png)

<span data-ttu-id="5c766-199">单击“应用迁移”按钮，此时应为你创建必要的数据库表。</span><span class="sxs-lookup"><span data-stu-id="5c766-199">Click on the "Apply Migrations" button and the necessary database tables should be created for you.</span></span> <span data-ttu-id="5c766-200">此外，迁移文件应在项目中显示，如下所示：</span><span class="sxs-lookup"><span data-stu-id="5c766-200">In addition, the migration files should appear in your project, as shown:</span></span>

![迁移文件](./media/security/migration-files.png)

<span data-ttu-id="5c766-202">可以使用以下命令行工具自行运行迁移，而无需运行 Web 应用程序：</span><span class="sxs-lookup"><span data-stu-id="5c766-202">You can run the migration yourself, without running the web application, using this command-line tool:</span></span>

```powershell
dotnet ef database update
```

<span data-ttu-id="5c766-203">如果希望运行脚本来将新架构应用到现有数据库，则可以从命令行编写这些迁移的脚本。</span><span class="sxs-lookup"><span data-stu-id="5c766-203">If you would rather run a script to apply the new schema to an existing database, you can script these migrations from the command-line.</span></span> <span data-ttu-id="5c766-204">运行以下命令以生成脚本：</span><span class="sxs-lookup"><span data-stu-id="5c766-204">Run this command to generate the script:</span></span>

```powershell
dotnet ef migrations script -o auth.sql
```

<span data-ttu-id="5c766-205">上面的命令将在输出文件 `auth.sql` 中生成 SQL 脚本，然后可以针对所需的任何数据库运行该脚本。</span><span class="sxs-lookup"><span data-stu-id="5c766-205">The above command will produce a SQL script in the output file `auth.sql`, which can then be run against whatever database you like.</span></span> <span data-ttu-id="5c766-206">如果在运行 `dotnet ef` 命令时遇到任何问题，请[确保系统上已安装 EF Core 工具](/ef/core/miscellaneous/cli/dotnet)。</span><span class="sxs-lookup"><span data-stu-id="5c766-206">If you have any trouble running `dotnet ef` commands, [make sure you have the EF Core tools installed on your system](/ef/core/miscellaneous/cli/dotnet).</span></span>

<span data-ttu-id="5c766-207">如果源表上还有其他列，则需要在新架构中为这些列标识最佳位置。</span><span class="sxs-lookup"><span data-stu-id="5c766-207">In the event you have additional columns on your source tables, you will need to identify the best location for these columns in the new schema.</span></span> <span data-ttu-id="5c766-208">一般情况下，在 `aspnet_Membership` 表上找到的列应映射到 `AspNetUsers` 表。</span><span class="sxs-lookup"><span data-stu-id="5c766-208">Generally, columns found on the `aspnet_Membership` table should be mapped to the `AspNetUsers` table.</span></span> <span data-ttu-id="5c766-209">`aspnet_Roles` 上的列应映射到 `AspNetRoles`。</span><span class="sxs-lookup"><span data-stu-id="5c766-209">Columns on `aspnet_Roles` should be mapped to `AspNetRoles`.</span></span> <span data-ttu-id="5c766-210">`aspnet_UsersInRoles` 表上的所有其他列都将添加到 `AspNetUserRoles` 表中。</span><span class="sxs-lookup"><span data-stu-id="5c766-210">Any additional columns on the `aspnet_UsersInRoles` table would be added to the `AspNetUserRoles` table.</span></span>

<span data-ttu-id="5c766-211">还值得考虑将所有其他列放到单独的表中。</span><span class="sxs-lookup"><span data-stu-id="5c766-211">It's also worth considering putting any additional columns on separate tables.</span></span> <span data-ttu-id="5c766-212">这样，将来的迁移就无需考虑默认标识架构的此类自定义项。</span><span class="sxs-lookup"><span data-stu-id="5c766-212">So that future migrations won't need to take into account such customizations of the default identity schema.</span></span>

### <a name="migrating-data-from-universal-providers-to-aspnet-core-identity"></a><span data-ttu-id="5c766-213">将数据从通用提供程序迁移到 ASP.NET Core Identity</span><span class="sxs-lookup"><span data-stu-id="5c766-213">Migrating data from universal providers to ASP.NET Core Identity</span></span>

<span data-ttu-id="5c766-214">拥有目标表架构后，下一步是将用户和角色记录迁移到新架构。</span><span class="sxs-lookup"><span data-stu-id="5c766-214">Once you have the destination table schema in place, the next step is to migrate your user and role records to the new schema.</span></span> <span data-ttu-id="5c766-215">可在[此处](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)找到架构区别的完整列表，包括哪些列映射到哪些新列。</span><span class="sxs-lookup"><span data-stu-id="5c766-215">A complete list of the schema differences, including which columns map to which new columns, can be found [here](/aspnet/core/migration/proper-to-2x/membership-to-core-identity).</span></span>

<span data-ttu-id="5c766-216">若要将用户从成员资格迁移到新的标识表，应[遵循文档中所述的步骤](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)。</span><span class="sxs-lookup"><span data-stu-id="5c766-216">To migrate your users from membership to the new identity tables, you should [follow the steps described in the documentation](/aspnet/core/migration/proper-to-2x/membership-to-core-identity).</span></span> <span data-ttu-id="5c766-217">完成这些步骤并提供脚本后，用户将需要在下次登录时更改其密码。</span><span class="sxs-lookup"><span data-stu-id="5c766-217">After following these steps and the script provided, your users will need to change their password the next time they log in.</span></span>

<span data-ttu-id="5c766-218">可以迁移用户密码，但该过程涉及的操作要多得多。</span><span class="sxs-lookup"><span data-stu-id="5c766-218">It is possible to migrate user passwords but the process is much more involved.</span></span> <span data-ttu-id="5c766-219">要求用户在迁移过程中更新密码，并鼓励他们使用新的唯一密码，这可能增强应用程序的整体安全性。</span><span class="sxs-lookup"><span data-stu-id="5c766-219">Requiring users to update their passwords as part of the migration process, and encouraging them to use new, unique passwords, is likely to enhance the overall security of the application.</span></span>

### <a name="migrating-security-settings-from-webconfig-to-startupcs"></a><span data-ttu-id="5c766-220">将安全设置从 web.config 迁移到 Startup.cs</span><span class="sxs-lookup"><span data-stu-id="5c766-220">Migrating security settings from web.config to Startup.cs</span></span>

<span data-ttu-id="5c766-221">如上所述，在应用程序的 `web.config` 文件中配置了 ASP.NET 成员资格和角色提供程序。</span><span class="sxs-lookup"><span data-stu-id="5c766-221">As noted above, ASP.NET membership and role providers are configured in the application's `web.config` file.</span></span> <span data-ttu-id="5c766-222">由于 ASP.NET Core 应用未绑定到 IIS 并使用单独的系统进行配置，因此必须在其他位置配置这些设置。</span><span class="sxs-lookup"><span data-stu-id="5c766-222">Since ASP.NET Core apps are not tied to IIS and use a separate system for configuration, these settings must be configured elsewhere.</span></span> <span data-ttu-id="5c766-223">在大多数情况下，ASP.NET Core Identity 在 `Startup.cs` 文件中配置。</span><span class="sxs-lookup"><span data-stu-id="5c766-223">For the most part, ASP.NET Core Identity is configured in the `Startup.cs` file.</span></span> <span data-ttu-id="5c766-224">打开先前创建的 Web 项目（用于生成标识表架构），并查看其 `Startup.cs` 文件。</span><span class="sxs-lookup"><span data-stu-id="5c766-224">Open the web project that was created earlier (to generate the identity table schema) and review its `Startup.cs` file.</span></span>

<span data-ttu-id="5c766-225">默认的 ConfigureServices 方法添加对 EF Core 和 Identity 的支持：</span><span class="sxs-lookup"><span data-stu-id="5c766-225">The default ConfigureServices method adds support for EF Core and Identity:</span></span>

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

<span data-ttu-id="5c766-226">`AddDefaultIdentity` 扩展方法用于将 Identity 配置为使用默认的 `ApplicationDbContext` 和框架的 `IdentityUser` 类型。</span><span class="sxs-lookup"><span data-stu-id="5c766-226">The `AddDefaultIdentity` extension method is used to configure Identity to use the default `ApplicationDbContext` and the framework's `IdentityUser` type.</span></span> <span data-ttu-id="5c766-227">如果使用自定义 `IdentityUser`，请确保在此处指定其类型。</span><span class="sxs-lookup"><span data-stu-id="5c766-227">If you're using a custom `IdentityUser`, be sure to specify its type here.</span></span> <span data-ttu-id="5c766-228">如果这些扩展方法在应用程序中无法正常工作，请检查是否具有适当的 using 语句，以及是否具有必需的 NuGet 包引用。</span><span class="sxs-lookup"><span data-stu-id="5c766-228">If these extension methods aren't working in your application, check that you have the appropriate using statements and that you have the necessary NuGet package references.</span></span> <span data-ttu-id="5c766-229">例如，项目应具有引用的 `Microsoft.AspNetCore.Identity.EntityFrameworkCore` 和 `Microsoft.AspNetCore.Identity.UI` 包的某些版本。</span><span class="sxs-lookup"><span data-stu-id="5c766-229">For example, your project should have some version of the `Microsoft.AspNetCore.Identity.EntityFrameworkCore` and `Microsoft.AspNetCore.Identity.UI` packages referenced.</span></span>

<span data-ttu-id="5c766-230">同样，在 `Startup.cs` 中，应该看到为站点配置的必要中间件。</span><span class="sxs-lookup"><span data-stu-id="5c766-230">Also in `Startup.cs` you should see the necessary middleware configured for the site.</span></span> <span data-ttu-id="5c766-231">具体来说，应设置 `UseAuthentication` 和 `UseAuthorization` 且其应该在正确的位置。</span><span class="sxs-lookup"><span data-stu-id="5c766-231">Specifically, `UseAuthentication` and `UseAuthorization` should be set up, and in the proper location.</span></span>

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

<span data-ttu-id="5c766-232">ASP.NET Identity 不配置从 `Startup.cs` 对位置的匿名访问或基于角色的访问。</span><span class="sxs-lookup"><span data-stu-id="5c766-232">ASP.NET Identity does not configure anonymous or role-based access to locations from `Startup.cs`.</span></span> <span data-ttu-id="5c766-233">需要将所有位置特定的授权配置数据迁移到 ASP.NET Core 中的筛选器。</span><span class="sxs-lookup"><span data-stu-id="5c766-233">You will need to migrate any location-specific authorization configuration data to filters in ASP.NET Core.</span></span> <span data-ttu-id="5c766-234">记下哪些文件夹和页将需要此类更新。</span><span class="sxs-lookup"><span data-stu-id="5c766-234">Make note of which folders and pages will require such updates.</span></span> <span data-ttu-id="5c766-235">你将在下一部分中进行这些更改。</span><span class="sxs-lookup"><span data-stu-id="5c766-235">You will make these changes in the next section.</span></span>

### <a name="updating-individual-pages-to-use-aspnet-core-identity-abstractions"></a><span data-ttu-id="5c766-236">更新各个页以使用 ASP.NET Core Identity 抽象</span><span class="sxs-lookup"><span data-stu-id="5c766-236">Updating individual pages to use ASP.NET Core Identity abstractions</span></span>

<span data-ttu-id="5c766-237">在 ASP.NET Web Forms 应用程序中，如果具有用于拒绝匿名用户访问某些页或文件夹的 `web.config` 设置，则可以通过向此类页添加 `[Authorize]` 属性来迁移这些更改：</span><span class="sxs-lookup"><span data-stu-id="5c766-237">In your ASP.NET Web Forms application, if you had `web.config` settings to deny access to certain pages or folders to anonymous users, you would migrate these changes by adding the `[Authorize]` attribute to such pages:</span></span>

```razor
@attribute [Authorize]
```

<span data-ttu-id="5c766-238">如果进一步拒绝不属于某个角色的用户进行访问，则同样可以通过添加指定角色的属性来迁移此行为：</span><span class="sxs-lookup"><span data-stu-id="5c766-238">If you further had denied access except to those users belonging to a certain role, you would likewise migrate this behavior by adding an attribute specifying a role:</span></span>

```razor
@attribute [Authorize(Roles ="administrators")]
```

<span data-ttu-id="5c766-239">`[Authorize]` 属性仅适用于通过 Blazor 路由器到达的 `@page` 组件。</span><span class="sxs-lookup"><span data-stu-id="5c766-239">The `[Authorize]` attribute only works on `@page` components that are reached via the Blazor Router.</span></span> <span data-ttu-id="5c766-240">该属性不适用于子组件，子组件应使用 `AuthorizeView`。</span><span class="sxs-lookup"><span data-stu-id="5c766-240">The attribute does not work with child components, which should instead use `AuthorizeView`.</span></span>

<span data-ttu-id="5c766-241">如果页标记中有用于确定是否向某个用户显示某些代码的逻辑，则可以使用 `AuthorizeView` 组件进行替换。</span><span class="sxs-lookup"><span data-stu-id="5c766-241">If you have logic within page markup for determining whether to display some code to a certain user, you can replace this with the `AuthorizeView` component.</span></span> <span data-ttu-id="5c766-242">[AuthorizeView 组件](/aspnet/core/blazor/security#authorizeview-component)根据用户是否获得查看授权来有选择地显示 UI。</span><span class="sxs-lookup"><span data-stu-id="5c766-242">The [AuthorizeView component](/aspnet/core/blazor/security#authorizeview-component) selectively displays UI depending on whether the user is authorized to see it.</span></span> <span data-ttu-id="5c766-243">它还公开可用于访问用户信息的 `context` 变量。</span><span class="sxs-lookup"><span data-stu-id="5c766-243">It also exposes a `context` variable that can be used to access user information.</span></span>

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

<span data-ttu-id="5c766-244">可以通过从配置了 `[CascadingParameter]` 属性的 `Task<AuthenticationState` 访问用户来访问过程逻辑中的身份验证状态。</span><span class="sxs-lookup"><span data-stu-id="5c766-244">You can access the authentication state within procedural logic by accessing the user from a `Task<AuthenticationState` configured with the `[CascadingParameter]` attribute.</span></span> <span data-ttu-id="5c766-245">借助此配置，你可以访问用户，这让你能够确定他们是否已经过身份验证以及他们是否属于特定角色。</span><span class="sxs-lookup"><span data-stu-id="5c766-245">This configuration will get you access to the user, which can let you determine if they are authenticated and if they belong to a particular role.</span></span> <span data-ttu-id="5c766-246">如果需要在过程中评估策略，则可以注入 `IAuthorizationService` 的实例并在其上调用 `AuthorizeAsync` 方法。</span><span class="sxs-lookup"><span data-stu-id="5c766-246">If you need to evaluate a policy procedurally, you can inject an instance of the `IAuthorizationService` and calls the `AuthorizeAsync` method on it.</span></span> <span data-ttu-id="5c766-247">以下示例代码演示如何获取用户信息以及如何允许授权用户执行受 `content-editor` 策略限制的任务。</span><span class="sxs-lookup"><span data-stu-id="5c766-247">The following sample code demonstrates how to get user information and allow an authorized user to perform a task restricted by the `content-editor` policy.</span></span>

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

<span data-ttu-id="5c766-248">需要先将 `AuthenticationState` 设置为级联值，然后才能将其绑定到此类级联参数。</span><span class="sxs-lookup"><span data-stu-id="5c766-248">The `AuthenticationState` first need to be set up as a cascading value before it can be bound to a cascading parameter like this.</span></span> <span data-ttu-id="5c766-249">通常使用 `CascadingAuthenticationState` 组件完成此操作。</span><span class="sxs-lookup"><span data-stu-id="5c766-249">That's typically done using the `CascadingAuthenticationState` component.</span></span> <span data-ttu-id="5c766-250">此配置通常在 `App.razor` 中完成：</span><span class="sxs-lookup"><span data-stu-id="5c766-250">This configuration is typically done in `App.razor`:</span></span>

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

## <a name="summary"></a><span data-ttu-id="5c766-251">总结</span><span class="sxs-lookup"><span data-stu-id="5c766-251">Summary</span></span>

<span data-ttu-id="5c766-252">Blazor 使用与 ASP.NET Core 相同的安全模型，即 ASP.NET Core Identity。</span><span class="sxs-lookup"><span data-stu-id="5c766-252">Blazor uses the same security model as ASP.NET Core, which is ASP.NET Core Identity.</span></span> <span data-ttu-id="5c766-253">从通用提供程序迁移到 ASP.NET Core Identity 相对简单，前提是没有对原始数据架构应用太多自定义项。</span><span class="sxs-lookup"><span data-stu-id="5c766-253">Migrating from universal providers to ASP.NET Core Identity is relatively straightforward, assuming not too much customization was applied to the original data schema.</span></span> <span data-ttu-id="5c766-254">迁移数据后，Blazor 应用中的身份验证和授权工作将得到良好记录，并具有针对大多数安全要求的可配置和编程支持。</span><span class="sxs-lookup"><span data-stu-id="5c766-254">Once the data has been migrated, working with authentication and authorization in Blazor apps is well documented, with configurable as well as programmatic support for most security requirements.</span></span>

## <a name="references"></a><span data-ttu-id="5c766-255">参考</span><span class="sxs-lookup"><span data-stu-id="5c766-255">References</span></span>

- [<span data-ttu-id="5c766-256">ASP.NET Core 上的 Identity 简介</span><span class="sxs-lookup"><span data-stu-id="5c766-256">Introduction to Identity on ASP.NET Core</span></span>](/aspnet/core/security/authentication/identity)
- [<span data-ttu-id="5c766-257">从 ASP.NET Membership 身份验证迁移到 ASP.NET Core 2.0 Identity</span><span class="sxs-lookup"><span data-stu-id="5c766-257">Migrate from ASP.NET Membership authentication to ASP.NET Core 2.0 Identity</span></span>](/aspnet/core/migration/proper-to-2x/membership-to-core-identity)
- [<span data-ttu-id="5c766-258">将身份验证和标识迁移到 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5c766-258">Migrate Authentication and Identity to ASP.NET Core</span></span>](/aspnet/core/migration/identity)
- [<span data-ttu-id="5c766-259">ASP.NET Core Blazor 身份验证和授权</span><span class="sxs-lookup"><span data-stu-id="5c766-259">ASP.NET Core Blazor authentication and authorization</span></span>](/aspnet/core/blazor/security/)

>[!div class="step-by-step"]
><span data-ttu-id="5c766-260">[上一页](config.md)
>[下一页](migration.md)</span><span class="sxs-lookup"><span data-stu-id="5c766-260">[Previous](config.md)
[Next](migration.md)</span></span>
