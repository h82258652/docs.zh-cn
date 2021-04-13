---
ms.openlocfilehash: b75f9ce8e878c935dcc5fd9dc292369720b895e9
ms.sourcegitcommit: e16315d9f1ff355f55ff8ab84a28915be0a8e42b
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 03/25/2021
ms.locfileid: "105152830"
---
### <a name="http-browser-samesite-changes-impact-authentication"></a><span data-ttu-id="0f3c5-101">HTTP：浏览器的 SameSite 更改会影响身份验证</span><span class="sxs-lookup"><span data-stu-id="0f3c5-101">HTTP: Browser SameSite changes impact authentication</span></span>

<span data-ttu-id="0f3c5-102">某些浏览器（如 Chrome 和 Firefox）对 Cookie 的 `SameSite` 实现进行了中断性变更。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-102">Some browsers, such as Chrome and Firefox, made breaking changes to their implementations of `SameSite` for cookies.</span></span> <span data-ttu-id="0f3c5-103">这些变更会影响 OpenID Connect 和 WS 联合身份验证等远程身份验证方案，必须通过发送 `SameSite=None` 来选择退出。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-103">The changes impact remote authentication scenarios, such as OpenID Connect and WS-Federation, which must opt out by sending `SameSite=None`.</span></span> <span data-ttu-id="0f3c5-104">但是，`SameSite=None` 会在 iOS 12 和其他浏览器的某些较早版本上中断运行。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-104">However, `SameSite=None` breaks on iOS 12 and some older versions of other browsers.</span></span> <span data-ttu-id="0f3c5-105">应用需探查这些版本，并忽略 `SameSite`。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-105">The app needs to sniff these versions and omit `SameSite`.</span></span>

<span data-ttu-id="0f3c5-106">有关此问题的讨论，请参阅 [dotnet/aspnetcore#14996](https://github.com/dotnet/aspnetcore/issues/14996)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-106">For discussion on this issue, see [dotnet/aspnetcore#14996](https://github.com/dotnet/aspnetcore/issues/14996).</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="0f3c5-107">引入的版本</span><span class="sxs-lookup"><span data-stu-id="0f3c5-107">Version introduced</span></span>

<span data-ttu-id="0f3c5-108">3.1 预览版 1</span><span class="sxs-lookup"><span data-stu-id="0f3c5-108">3.1 Preview 1</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="0f3c5-109">旧行为</span><span class="sxs-lookup"><span data-stu-id="0f3c5-109">Old behavior</span></span>

<span data-ttu-id="0f3c5-110">`SameSite` 是对 HTTP Cookie 的 2016 草案标准扩展。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-110">`SameSite` is a 2016 draft standard extension to HTTP cookies.</span></span> <span data-ttu-id="0f3c5-111">它旨在减少跨站点请求伪造 (CSRF)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-111">It's intended to mitigate Cross-Site Request Forgery (CSRF).</span></span> <span data-ttu-id="0f3c5-112">它最初设计成一项功能，服务器可通过添加新参数选择加入该功能。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-112">This was originally designed as a feature the servers would opt into by adding the new parameters.</span></span> <span data-ttu-id="0f3c5-113">ASP.NET Core 2.0 添加了对 `SameSite` 的初始支持。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-113">ASP.NET Core 2.0 added initial support for `SameSite`.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="0f3c5-114">新行为</span><span class="sxs-lookup"><span data-stu-id="0f3c5-114">New behavior</span></span>

<span data-ttu-id="0f3c5-115">Google 提出了一项不向后兼容的新草案标准。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-115">Google proposed a new draft standard that isn't backwards compatible.</span></span> <span data-ttu-id="0f3c5-116">该标准将默认模式更改为 `Lax`并添加了用于选择退出的新条目 `None`。`Lax` 可满足大多数应用 Cookie；但是，它会造成 OpenID Connect 和 WS 联合身份验证登录等跨站点方案中断。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-116">The standard changes the default mode to `Lax` and adds a new entry `None` to opt out. `Lax` suffices for most app cookies; however, it breaks cross-site scenarios like OpenID Connect and WS-Federation login.</span></span> <span data-ttu-id="0f3c5-117">由于请求流程不同，大多数 OAuth 登录不受影响。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-117">Most OAuth logins aren't affected because of differences in how the request flows.</span></span> <span data-ttu-id="0f3c5-118">新的 `None` 参数会导致实现先前草案标准的客户端（例如 iOS 12）出现兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-118">The new `None` parameter causes compatibility problems with clients that implemented the prior draft standard (for example, iOS 12).</span></span> <span data-ttu-id="0f3c5-119">Chrome 80 将包含这些更改。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-119">Chrome 80 will include the changes.</span></span> <span data-ttu-id="0f3c5-120">有关 Chrome 产品发布日程表，请查看 [SameSite 更新](https://www.chromium.org/updates/same-site)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-120">See [SameSite Updates](https://www.chromium.org/updates/same-site) for the Chrome product launch timeline.</span></span>

<span data-ttu-id="0f3c5-121">已更新 ASP.NET Core 3.1 来实现新的 `SameSite` 行为。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-121">ASP.NET Core 3.1 has been updated to implement the new `SameSite` behavior.</span></span> <span data-ttu-id="0f3c5-122">该更新重新定义了 `SameSiteMode.None` 的行为以发出 `SameSite=None`，并添加了一个新值 `SameSiteMode.Unspecified` 以忽略 `SameSite` 属性。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-122">The update redefines the behavior of `SameSiteMode.None` to emit `SameSite=None` and adds a new value `SameSiteMode.Unspecified` to omit the `SameSite` attribute.</span></span> <span data-ttu-id="0f3c5-123">现在，所有 Cookie API 都默认为 `Unspecified`，但某些使用 Cookie 的组件设置了更特定于其方案的值，例如 OpenID Connect 相关性和 nonce Cookie。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-123">All cookie APIs now default to `Unspecified`, though some components that use cookies set values more specific to their scenarios such as the OpenID Connect correlation and nonce cookies.</span></span>

<span data-ttu-id="0f3c5-124">有关此方面的其他最新更改，请参阅 [HTTP：某些 Cookie SameSite 默认值已更改为“None”](../../../../docs/core/compatibility/3.0.md#http-some-cookie-samesite-defaults-changed-to-none)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-124">For other recent changes in this area, see [HTTP: Some cookie SameSite defaults changed to None](../../../../docs/core/compatibility/3.0.md#http-some-cookie-samesite-defaults-changed-to-none).</span></span> <span data-ttu-id="0f3c5-125">在 ASP.NET Core 3.0 中，大多数默认值已从 <xref:Microsoft.AspNetCore.Http.SameSiteMode.Lax?displayProperty=nameWithType> 更改为 <xref:Microsoft.AspNetCore.Http.SameSiteMode.None?displayProperty=nameWithType>（但仍使用之前的标准）。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-125">In ASP.NET Core 3.0, most defaults were changed from <xref:Microsoft.AspNetCore.Http.SameSiteMode.Lax?displayProperty=nameWithType> to <xref:Microsoft.AspNetCore.Http.SameSiteMode.None?displayProperty=nameWithType> (but still using the prior standard).</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="0f3c5-126">更改原因</span><span class="sxs-lookup"><span data-stu-id="0f3c5-126">Reason for change</span></span>

<span data-ttu-id="0f3c5-127">浏览器和规范更改如前文所述。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-127">Browser and specification changes as outlined in the preceding text.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="0f3c5-128">建议操作</span><span class="sxs-lookup"><span data-stu-id="0f3c5-128">Recommended action</span></span>

<span data-ttu-id="0f3c5-129">与远程站点交互（例如通过第三方登录）的应用需要：</span><span class="sxs-lookup"><span data-stu-id="0f3c5-129">Apps that interact with remote sites, such as through third-party login, need to:</span></span>

* <span data-ttu-id="0f3c5-130">在多个浏览器中测试这些方案。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-130">Test those scenarios on multiple browsers.</span></span>
* <span data-ttu-id="0f3c5-131">应用[支持旧版浏览器](#support-older-browsers)中讨论的 Cookie 策略浏览器探查缓解措施。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-131">Apply the cookie policy browser sniffing mitigation discussed in [Support older browsers](#support-older-browsers).</span></span>

<span data-ttu-id="0f3c5-132">有关测试和浏览器探查说明，请参阅下一部分。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-132">For testing and browser sniffing instructions, see the following section.</span></span>

##### <a name="determine-if-youre-affected"></a><span data-ttu-id="0f3c5-133">确定你是否受到影响</span><span class="sxs-lookup"><span data-stu-id="0f3c5-133">Determine if you're affected</span></span>

<span data-ttu-id="0f3c5-134">使用可选择采用新行为的客户端版本测试 Web 应用。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-134">Test your web app using a client version that can opt into the new behavior.</span></span> <span data-ttu-id="0f3c5-135">Chrome、Firefox 和 Microsoft Edge Chromium 都具有可用于测试的新的“选择加入”功能标志。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-135">Chrome, Firefox, and Microsoft Edge Chromium all have new opt-in feature flags that can be used for testing.</span></span> <span data-ttu-id="0f3c5-136">在应用修补程序（特别是 Safari）后，验证应用是否与较旧的客户端版本兼容。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-136">Verify that your app is compatible with older client versions after you've applied the patches, especially Safari.</span></span> <span data-ttu-id="0f3c5-137">有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-137">For more information, see [Support older browsers](#support-older-browsers).</span></span>

##### <a name="chrome"></a><span data-ttu-id="0f3c5-138">Chrome</span><span class="sxs-lookup"><span data-stu-id="0f3c5-138">Chrome</span></span>

<span data-ttu-id="0f3c5-139">Chrome 78 及更高版本生成误导性的测试结果。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-139">Chrome 78 and later yield misleading test results.</span></span> <span data-ttu-id="0f3c5-140">这些版本具有临时缓解措施，允许 Cookie 的使用时间小于两分钟。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-140">Those versions have a temporary mitigation in place and allow cookies less than two minutes old.</span></span> <span data-ttu-id="0f3c5-141">启用合适的测试标志后，Chrome 76 和 77 会生成更准确的结果。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-141">With the appropriate test flags enabled, Chrome 76 and 77 yield more accurate results.</span></span> <span data-ttu-id="0f3c5-142">要测试新行为，请将 `chrome://flags/#same-site-by-default-cookies` 切换为“已启用”。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-142">To test the new behavior, toggle `chrome://flags/#same-site-by-default-cookies` to enabled.</span></span> <span data-ttu-id="0f3c5-143">据报告，Chrome 75 及更早版本使用新的 `None` 设置时失败。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-143">Chrome 75 and earlier are reported to fail with the new `None` setting.</span></span> <span data-ttu-id="0f3c5-144">有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-144">For more information, see [Support older browsers](#support-older-browsers).</span></span>

<span data-ttu-id="0f3c5-145">Google 不提供较旧的 Chrome 版本。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-145">Google doesn't make older Chrome versions available.</span></span> <span data-ttu-id="0f3c5-146">但是，你可下载较旧版本的 Chromium，这将足以用于测试。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-146">You can, however, download older versions of Chromium, which will suffice for testing.</span></span> <span data-ttu-id="0f3c5-147">按照[下载 Chromium](https://www.chromium.org/getting-involved/download-chromium) 的说明进行操作。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-147">Follow the instructions at [Download Chromium](https://www.chromium.org/getting-involved/download-chromium).</span></span>

* [<span data-ttu-id="0f3c5-148">Chromium 76 Win64</span><span class="sxs-lookup"><span data-stu-id="0f3c5-148">Chromium 76 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [<span data-ttu-id="0f3c5-149">Chromium 74 Win64</span><span class="sxs-lookup"><span data-stu-id="0f3c5-149">Chromium 74 Win64</span></span>](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

##### <a name="safari"></a><span data-ttu-id="0f3c5-150">Safari</span><span class="sxs-lookup"><span data-stu-id="0f3c5-150">Safari</span></span>

<span data-ttu-id="0f3c5-151">Safari 12 严格执行了先前的草案，如果它在 Cookie 中检测到新的 `None` 值，则将失败。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-151">Safari 12 strictly implemented the prior draft and fails if it sees the new `None` value in cookies.</span></span> <span data-ttu-id="0f3c5-152">必须通过[支持旧版浏览器](#support-older-browsers)中所示的浏览器探查代码来避免这种情况。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-152">This must be avoided via the browser sniffing code shown in [Support older browsers](#support-older-browsers).</span></span> <span data-ttu-id="0f3c5-153">请确保使用 Microsoft 身份验证库 (MSAL)、Active Directory 身份验证库 (ADAL) 或所使用的任何库来测试 Safari 12 和 13 以及基于 WebKit 的 OS 样式的登录。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-153">Ensure you test Safari 12 and 13 as well as WebKit-based, OS-style logins using Microsoft Authentication Library (MSAL), Active Directory Authentication Library (ADAL), or whichever library you're using.</span></span> <span data-ttu-id="0f3c5-154">问题取决于基础 OS 版本。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-154">The problem is dependent on the underlying OS version.</span></span> <span data-ttu-id="0f3c5-155">已知 OSX Mojave 10.14 和 iOS 12 存在与新行为相关的兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-155">OSX Mojave 10.14 and iOS 12 are known to have compatibility problems with the new behavior.</span></span> <span data-ttu-id="0f3c5-156">升级到 OSX Catalina 10.15 或 iOS 13 会解决此问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-156">Upgrading to OSX Catalina 10.15 or iOS 13 fixes the problem.</span></span> <span data-ttu-id="0f3c5-157">Safari 当前没有用于测试新规范行为的选择加入标志。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-157">Safari doesn't currently have an opt-in flag for testing the new specification behavior.</span></span>

##### <a name="firefox"></a><span data-ttu-id="0f3c5-158">Firefox</span><span class="sxs-lookup"><span data-stu-id="0f3c5-158">Firefox</span></span>

<span data-ttu-id="0f3c5-159">通过在具有功能标志 `network.cookie.sameSite.laxByDefault` 的 `about:config` 页面上选择加入，可在版本 68 及更高版本上测试 Firefox 对新标准的支持。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-159">Firefox support for the new standard can be tested on version 68 and later by opting in on the `about:config` page with the feature flag `network.cookie.sameSite.laxByDefault`.</span></span> <span data-ttu-id="0f3c5-160">Firefox 旧版本未报告兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-160">No compatibility issues have been reported on older versions of Firefox.</span></span>

##### <a name="microsoft-edge"></a><span data-ttu-id="0f3c5-161">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="0f3c5-161">Microsoft Edge</span></span>

<span data-ttu-id="0f3c5-162">虽然 Microsoft Edge 支持旧的 `SameSite` 标准，但从版本 44 开始，它与新标准不存在任何兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-162">While Microsoft Edge supports the old `SameSite` standard, as of version 44 it didn't have any compatibility problems with the new standard.</span></span>

##### <a name="microsoft-edge-chromium"></a><span data-ttu-id="0f3c5-163">Microsoft Edge Chromium</span><span class="sxs-lookup"><span data-stu-id="0f3c5-163">Microsoft Edge Chromium</span></span>

<span data-ttu-id="0f3c5-164">功能标志为 `edge://flags/#same-site-by-default-cookies`。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-164">The feature flag is `edge://flags/#same-site-by-default-cookies`.</span></span> <span data-ttu-id="0f3c5-165">使用 Microsoft Edge Chromium 78 进行测试时，未发现兼容性问题。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-165">No compatibility issues were observed when testing with Microsoft Edge Chromium 78.</span></span>

##### <a name="electron"></a><span data-ttu-id="0f3c5-166">Electron</span><span class="sxs-lookup"><span data-stu-id="0f3c5-166">Electron</span></span>

<span data-ttu-id="0f3c5-167">Electron 的版本包括较早版本的 Chromium。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-167">Versions of Electron include older versions of Chromium.</span></span> <span data-ttu-id="0f3c5-168">例如，Microsoft Teams 使用的 Electron 版本为 Chromium 66，该版本呈现了较旧的行为。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-168">For example, the version of Electron used by Microsoft Teams is Chromium 66, which exhibits the older behavior.</span></span> <span data-ttu-id="0f3c5-169">使用你的产品所用的 Electron 版本执行你自己的兼容性测试。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-169">Perform your own compatibility testing with the version of Electron your product uses.</span></span> <span data-ttu-id="0f3c5-170">有关详细信息，请参阅[支持旧版浏览器](#support-older-browsers)。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-170">For more information, see [Support older browsers](#support-older-browsers).</span></span>

##### <a name="support-older-browsers"></a><span data-ttu-id="0f3c5-171">支持旧版浏览器</span><span class="sxs-lookup"><span data-stu-id="0f3c5-171">Support older browsers</span></span>

<span data-ttu-id="0f3c5-172">2016 `SameSite` 标准要求将未知值视为 `SameSite=Strict` 值。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-172">The 2016 `SameSite` standard mandated that unknown values be treated as `SameSite=Strict` values.</span></span> <span data-ttu-id="0f3c5-173">因此，任何支持原始标准的旧版浏览器都可能在检测到 `SameSite` 属性具有 `None` 值时中断。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-173">Consequently, any older browsers that support the original standard may break when they see a `SameSite` property with a value of `None`.</span></span> <span data-ttu-id="0f3c5-174">如果 Web 应用要支持这些旧版浏览器，它们必须实现浏览器探查。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-174">Web apps must implement browser sniffing if they intend to support these old browsers.</span></span> <span data-ttu-id="0f3c5-175">ASP.NET Core 不会为你实现浏览器探查，因为 `User-Agent` 请求标头值非常不稳定，每周都会更改。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-175">ASP.NET Core doesn't implement browser sniffing for you because `User-Agent` request header values are highly unstable and change on a weekly basis.</span></span> <span data-ttu-id="0f3c5-176">相反，Cookie 策略中的扩展点允许添加特定于 `User-Agent` 的逻辑。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-176">Instead, an extension point in the cookie policy allows you to add `User-Agent`-specific logic.</span></span>

<span data-ttu-id="0f3c5-177">在 Startup.cs 中，添加以下代码：</span><span class="sxs-lookup"><span data-stu-id="0f3c5-177">In *Startup.cs*, add the following code:</span></span>

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

##### <a name="opt-out-switches"></a><span data-ttu-id="0f3c5-178">“选择退出”开关</span><span class="sxs-lookup"><span data-stu-id="0f3c5-178">Opt-out switches</span></span>

<span data-ttu-id="0f3c5-179">通过 `Microsoft.AspNetCore.SuppressSameSiteNone` 兼容性开关，可暂时选择退出新的 ASP.NET Core Cookie 行为。</span><span class="sxs-lookup"><span data-stu-id="0f3c5-179">The `Microsoft.AspNetCore.SuppressSameSiteNone` compatibility switch enables you to temporarily opt out of the new ASP.NET Core cookie behavior.</span></span> <span data-ttu-id="0f3c5-180">将以下 JSON 添加到项目的 runtimeconfig.template.json 文件中：</span><span class="sxs-lookup"><span data-stu-id="0f3c5-180">Add the following JSON to a *runtimeconfig.template.json* file in your project:</span></span>

```json
{
  "configProperties": {
    "Microsoft.AspNetCore.SuppressSameSiteNone": "true"
  }
}
```

##### <a name="other-versions"></a><span data-ttu-id="0f3c5-181">其他版本</span><span class="sxs-lookup"><span data-stu-id="0f3c5-181">Other Versions</span></span>

<span data-ttu-id="0f3c5-182">下述项的相关 `SameSite` 修补程序即将发布：</span><span class="sxs-lookup"><span data-stu-id="0f3c5-182">Related `SameSite` patches are forthcoming for:</span></span>

* <span data-ttu-id="0f3c5-183">ASP.NET Core 2.1、2.2 和 3.0</span><span class="sxs-lookup"><span data-stu-id="0f3c5-183">ASP.NET Core 2.1, 2.2, and 3.0</span></span>
* <span data-ttu-id="0f3c5-184">`Microsoft.Owin` 4.1</span><span class="sxs-lookup"><span data-stu-id="0f3c5-184">`Microsoft.Owin` 4.1</span></span>
* <span data-ttu-id="0f3c5-185">`System.Web`（用于 .NET Framework 4.7.2 及更高版本）</span><span class="sxs-lookup"><span data-stu-id="0f3c5-185">`System.Web` (for .NET Framework 4.7.2 and later)</span></span>

#### <a name="category"></a><span data-ttu-id="0f3c5-186">类别</span><span class="sxs-lookup"><span data-stu-id="0f3c5-186">Category</span></span>

<span data-ttu-id="0f3c5-187">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="0f3c5-187">ASP.NET</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="0f3c5-188">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="0f3c5-188">Affected APIs</span></span>

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
