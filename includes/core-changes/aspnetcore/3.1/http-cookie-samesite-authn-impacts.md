---
ms.openlocfilehash: b75f9ce8e878c935dcc5fd9dc292369720b895e9
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105152830"
---
### <a name="http-browser-samesite-changes-impact-authentication"></a>HTTP：浏览器的 SameSite 更改会影响身份验证

某些浏览器（如 Chrome 和 Firefox）对 Cookie 的 `SameSite` 实现进行了中断性变更。 这些变更会影响 OpenID Connect 和 WS 联合身份验证等远程身份验证方案，必须通过发送 `SameSite=None` 来选择退出。 但是，`SameSite=None` 会在 iOS 12 和其他浏览器的某些较早版本上中断运行。 应用需探查这些版本，并忽略 `SameSite`。

有关此问题的讨论，请参阅 [dotnet/aspnetcore#14996](https://github.com/dotnet/aspnetcore/issues/14996)。

#### <a name="version-introduced"></a>引入的版本

3.1 预览版 1

#### <a name="old-behavior"></a>旧行为

`SameSite` 是对 HTTP Cookie 的 2016 草案标准扩展。 它旨在减少跨站点请求伪造 (CSRF)。 它最初设计成一项功能，服务器可通过添加新参数选择加入该功能。 ASP.NET Core 2.0 添加了对 `SameSite` 的初始支持。

#### <a name="new-behavior"></a>新行为

Google 提出了一项不向后兼容的新草案标准。 该标准将默认模式更改为 `Lax`并添加了用于选择退出的新条目 `None`。`Lax` 可满足大多数应用 Cookie；但是，它会造成 OpenID Connect 和 WS 联合身份验证登录等跨站点方案中断。 由于请求流程不同，大多数 OAuth 登录不受影响。 新的 `None` 参数会导致实现先前草案标准的客户端（例如 iOS 12）出现兼容性问题。 Chrome 80 将包含这些更改。 有关 Chrome 产品发布日程表，请查看 [SameSite 更新](https://www.chromium.org/updates/same-site)。

已更新 ASP.NET Core 3.1 来实现新的 `SameSite` 行为。 该更新重新定义了 `SameSiteMode.None` 的行为以发出 `SameSite=None`，并添加了一个新值 `SameSiteMode.Unspecified` 以忽略 `SameSite` 属性。 现在，所有 Cookie API 都默认为 `Unspecified`，但某些使用 Cookie 的组件设置了更特定于其方案的值，例如 OpenID Connect 相关性和 nonce Cookie。

有关此方面的其他最新更改，请参阅 [HTTP：某些 Cookie SameSite 默认值已更改为“None”](../../../../docs/core/compatibility/3.0.md#http-some-cookie-samesite-defaults-changed-to-none)。 在 ASP.NET Core 3.0 中，大多数默认值已从 <xref:Microsoft.AspNetCore.Http.SameSiteMode.Lax?displayProperty=nameWithType> 更改为 <xref:Microsoft.AspNetCore.Http.SameSiteMode.None?displayProperty=nameWithType>（但仍使用之前的标准）。

#### <a name="reason-for-change"></a>更改原因

浏览器和规范更改如前文所述。

#### <a name="recommended-action"></a>建议操作

与远程站点交互（例如通过第三方登录）的应用需要：

* 在多个浏览器中测试这些方案。
* 应用[支持旧版浏览器](#support-older-browsers)中讨论的 Cookie 策略浏览器探查缓解措施。

有关测试和浏览器探查说明，请参阅下一部分。

##### <a name="determine-if-youre-affected"></a>确定你是否受到影响

使用可选择采用新行为的客户端版本测试 Web 应用。 Chrome、Firefox 和 Microsoft Edge Chromium 都具有可用于测试的新的“选择加入”功能标志。 在应用修补程序（特别是 Safari）后，验证应用是否与较旧的客户端版本兼容。 有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。

##### <a name="chrome"></a>Chrome

Chrome 78 及更高版本生成误导性的测试结果。 这些版本具有临时缓解措施，允许 Cookie 的使用时间小于两分钟。 启用合适的测试标志后，Chrome 76 和 77 会生成更准确的结果。 要测试新行为，请将 `chrome://flags/#same-site-by-default-cookies` 切换为“已启用”。 据报告，Chrome 75 及更早版本使用新的 `None` 设置时失败。 有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。

Google 不提供较旧的 Chrome 版本。 但是，你可下载较旧版本的 Chromium，这将足以用于测试。 按照[下载 Chromium](https://www.chromium.org/getting-involved/download-chromium) 的说明进行操作。

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

##### <a name="safari"></a>Safari

Safari 12 严格执行了先前的草案，如果它在 Cookie 中检测到新的 `None` 值，则将失败。 必须通过[支持旧版浏览器](#support-older-browsers)中所示的浏览器探查代码来避免这种情况。 请确保使用 Microsoft 身份验证库 (MSAL)、Active Directory 身份验证库 (ADAL) 或所使用的任何库来测试 Safari 12 和 13 以及基于 WebKit 的 OS 样式的登录。 问题取决于基础 OS 版本。 已知 OSX Mojave 10.14 和 iOS 12 存在与新行为相关的兼容性问题。 升级到 OSX Catalina 10.15 或 iOS 13 会解决此问题。 Safari 当前没有用于测试新规范行为的选择加入标志。

##### <a name="firefox"></a>Firefox

通过在具有功能标志 `network.cookie.sameSite.laxByDefault` 的 `about:config` 页面上选择加入，可在版本 68 及更高版本上测试 Firefox 对新标准的支持。 Firefox 旧版本未报告兼容性问题。

##### <a name="microsoft-edge"></a>Microsoft Edge

虽然 Microsoft Edge 支持旧的 `SameSite` 标准，但从版本 44 开始，它与新标准不存在任何兼容性问题。

##### <a name="microsoft-edge-chromium"></a>Microsoft Edge Chromium

功能标志为 `edge://flags/#same-site-by-default-cookies`。 使用 Microsoft Edge Chromium 78 进行测试时，未发现兼容性问题。

##### <a name="electron"></a>Electron

Electron 的版本包括较早版本的 Chromium。 例如，Microsoft Teams 使用的 Electron 版本为 Chromium 66，该版本呈现了较旧的行为。 使用你的产品所用的 Electron 版本执行你自己的兼容性测试。 有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。

##### <a name="support-older-browsers"></a>支持旧版浏览器

2016 `SameSite` 标准要求将未知值视为 `SameSite=Strict` 值。 因此，任何支持原始标准的旧版浏览器都可能在检测到 `SameSite` 属性具有 `None` 值时中断。 如果 Web 应用要支持这些旧版浏览器，它们必须实现浏览器探查。 ASP.NET Core 不会为你实现浏览器探查，因为 `User-Agent` 请求标头值非常不稳定，每周都会更改。 相反，Cookie 策略中的扩展点允许添加特定于 `User-Agent` 的逻辑。

在 Startup.cs 中，添加以下代码：

```csharp
private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        // TODO: Use your User Agent library of choice here.
        if (/* UserAgent doesn't support new behavior */)
        {
            options.SameSite = SameSiteMode.Unspecified;
        }
    }
}

public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.MinimumSameSitePolicy = SameSiteMode.Unspecified;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

public void Configure(IApplicationBuilder app)
{
    // Before UseAuthentication or anything else that writes cookies.
    app.UseCookiePolicy();

    app.UseAuthentication();
    // code omitted for brevity
}
```

##### <a name="opt-out-switches"></a>“选择退出”开关

通过 `Microsoft.AspNetCore.SuppressSameSiteNone` 兼容性开关，可暂时选择退出新的 ASP.NET Core Cookie 行为。 将以下 JSON 添加到项目的 runtimeconfig.template.json 文件中：

```json
{
  "configProperties": {
    "Microsoft.AspNetCore.SuppressSameSiteNone": "true"
  }
}
```

##### <a name="other-versions"></a>其他版本

下述项的相关 `SameSite` 修补程序即将发布：

* ASP.NET Core 2.1、2.2 和 3.0
* `Microsoft.Owin` 4.1
* `System.Web`（用于 .NET Framework 4.7.2 及更高版本）

#### <a name="category"></a>类别

ASP.NET

#### <a name="affected-apis"></a>受影响的 API

- <xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite%2A?displayProperty=fullName>
- <xref:Microsoft.AspNetCore.Http.SameSiteMode?displayProperty=fullName>
- <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
- <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite%2A?displayProperty=fullName>

<!--

#### Affected APIs

- `Overload:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy`
- `Overload:Microsoft.AspNetCore.Http.CookieBuilder.SameSite`
- `Overload:Microsoft.AspNetCore.Http.CookieOptions.SameSite`
- `T:Microsoft.AspNetCore.Http.SameSiteMode`
- `T:Microsoft.Net.Http.Headers.SameSiteMode`
- `Overload:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite`

-->
