---
title: 中断性变更：安全性：Cookie 名称编码已删除
description: 了解 ASP.NET Core 5.0 中的以下中断性变更：安全性：Cookie 名称编码已删除
ms.author: scaddie
ms.date: 10/01/2020
ms.openlocfilehash: a50ef0ee34b7148d13b01f96e754d1cb47d40819
ms.sourcegitcommit: 089068389671f6f9e15fd67dcbfb0145bf72f1fb
ms.translationtype: HT
ms.contentlocale: zh-CN
ms.lasthandoff: 04/06/2021
ms.locfileid: "106497881"
---
# <a name="security-cookie-name-encoding-removed"></a><span data-ttu-id="feb39-103">安全性：Cookie 名称编码已删除</span><span class="sxs-lookup"><span data-stu-id="feb39-103">Security: Cookie name encoding removed</span></span>

<span data-ttu-id="feb39-104">[HTTP Cookie 标准](https://tools.ietf.org/html/rfc6265#section-4.1.1) 仅允许在 Cookie 名称和值中使用特定字符。</span><span class="sxs-lookup"><span data-stu-id="feb39-104">The [HTTP cookie standard](https://tools.ietf.org/html/rfc6265#section-4.1.1) allows only specific characters in cookie names and values.</span></span> <span data-ttu-id="feb39-105">为了支持不允许的字符，ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="feb39-105">To support disallowed characters, ASP.NET Core:</span></span>

* <span data-ttu-id="feb39-106">在创建响应 Cookie 时进行编码。</span><span class="sxs-lookup"><span data-stu-id="feb39-106">Encodes when creating a response cookie.</span></span>
* <span data-ttu-id="feb39-107">在读取请求 Cookie 时进行解码。</span><span class="sxs-lookup"><span data-stu-id="feb39-107">Decodes when reading a request cookie.</span></span>

<span data-ttu-id="feb39-108">在 ASP.NET Core 5.0 中，此编码行为因安全问题发生了更改。</span><span class="sxs-lookup"><span data-stu-id="feb39-108">In ASP.NET Core 5.0, this encoding behavior changed in response to a security concern.</span></span>

<span data-ttu-id="feb39-109">有关讨论，请参阅 GitHub 问题 [dotnet/aspnetcore#23578](https://github.com/dotnet/aspnetcore/issues/23578)。</span><span class="sxs-lookup"><span data-stu-id="feb39-109">For discussion, see GitHub issue [dotnet/aspnetcore#23578](https://github.com/dotnet/aspnetcore/issues/23578).</span></span>

## <a name="version-introduced"></a><span data-ttu-id="feb39-110">引入的版本</span><span class="sxs-lookup"><span data-stu-id="feb39-110">Version introduced</span></span>

<span data-ttu-id="feb39-111">5.0 预览版 8</span><span class="sxs-lookup"><span data-stu-id="feb39-111">5.0 Preview 8</span></span>

## <a name="old-behavior"></a><span data-ttu-id="feb39-112">旧行为</span><span class="sxs-lookup"><span data-stu-id="feb39-112">Old behavior</span></span>

<span data-ttu-id="feb39-113">对响应 Cookie 名称进行编码。</span><span class="sxs-lookup"><span data-stu-id="feb39-113">Response cookie names are encoded.</span></span> <span data-ttu-id="feb39-114">对请求 Cookie 名称进行解码。</span><span class="sxs-lookup"><span data-stu-id="feb39-114">Request cookie names are decoded.</span></span>

## <a name="new-behavior"></a><span data-ttu-id="feb39-115">新行为</span><span class="sxs-lookup"><span data-stu-id="feb39-115">New behavior</span></span>

<span data-ttu-id="feb39-116">删除了 Cookie 名称的编码和解码。</span><span class="sxs-lookup"><span data-stu-id="feb39-116">Encoding and decoding of cookie names was removed.</span></span> <span data-ttu-id="feb39-117">对于先前[受支持的 ASP.NET Core 版本](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)，团队计划就地缓解解码问题。</span><span class="sxs-lookup"><span data-stu-id="feb39-117">For prior [supported versions](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) of ASP.NET Core, the team plans to mitigate the decoding issue in-place.</span></span> <span data-ttu-id="feb39-118">此外，使用无效的 Cookie 名称调用 <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append%2A?displayProperty=nameWithType> 将引发 <xref:System.ArgumentException> 类型的异常。</span><span class="sxs-lookup"><span data-stu-id="feb39-118">Additionally, calling <xref:Microsoft.AspNetCore.Http.IResponseCookies.Append%2A?displayProperty=nameWithType> with an invalid cookie name throws an exception of type <xref:System.ArgumentException>.</span></span> <span data-ttu-id="feb39-119">Cookie 值的编码和解码保持不变。</span><span class="sxs-lookup"><span data-stu-id="feb39-119">Encoding and decoding of cookie values remains unchanged.</span></span>

## <a name="reason-for-change"></a><span data-ttu-id="feb39-120">更改原因</span><span class="sxs-lookup"><span data-stu-id="feb39-120">Reason for change</span></span>

<span data-ttu-id="feb39-121">在[多个 Web 框架](https://github.com/advisories/GHSA-j6w9-fv6q-3q52)中发现了问题。</span><span class="sxs-lookup"><span data-stu-id="feb39-121">An issue was discovered in [multiple web frameworks](https://github.com/advisories/GHSA-j6w9-fv6q-3q52).</span></span> <span data-ttu-id="feb39-122">编码和解码可以使攻击者使用诸如 `__%48ost-` 的编码值来假冒 `__Host-` 之类的保留前缀，从而绕过称为 [Cookie 前缀](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-prefixes-00)的安全功能。</span><span class="sxs-lookup"><span data-stu-id="feb39-122">The encoding and decoding could allow an attacker to bypass a security feature called [cookie prefixes](https://tools.ietf.org/html/draft-ietf-httpbis-cookie-prefixes-00) by spoofing reserved prefixes like `__Host-` with encoded values like `__%48ost-`.</span></span> <span data-ttu-id="feb39-123">此攻击需要利用辅助攻击（例如跨站点脚本 (XSS) 漏洞）在网站中注入欺诈性 Cookie。</span><span class="sxs-lookup"><span data-stu-id="feb39-123">The attack requires a secondary exploit to inject the spoofed cookies, such as a cross-site scripting (XSS) vulnerability, in the website.</span></span> <span data-ttu-id="feb39-124">默认情况下，ASP.NET Core 或者 `Microsoft.Owin` 库或模板中不使用这些前缀。</span><span class="sxs-lookup"><span data-stu-id="feb39-124">These prefixes aren't used by default in ASP.NET Core or `Microsoft.Owin` libraries or templates.</span></span>

## <a name="recommended-action"></a><span data-ttu-id="feb39-125">建议操作</span><span class="sxs-lookup"><span data-stu-id="feb39-125">Recommended action</span></span>

<span data-ttu-id="feb39-126">如果要将项目迁移到 ASP.NET Core 5.0 或更高版本，请确保其 Cookie 名称符合[令牌规范要求](https://tools.ietf.org/html/rfc2616#section-2.2)：ASCII 字符不包括控件和分隔符 `"(" | ")" | "<" | ">" | "@" | "," | ";" | ":" | "\" | <"> | "/" | "[" | "]" | "?" | "=" | "{" | "}" | SP | HT`。</span><span class="sxs-lookup"><span data-stu-id="feb39-126">If you're moving projects to ASP.NET Core 5.0 or later, ensure that their cookie names conform to the [token specification requirements](https://tools.ietf.org/html/rfc2616#section-2.2): ASCII characters excluding controls and separators `"(" | ")" | "<" | ">" | "@" | "," | ";" | ":" | "\" | <"> | "/" | "[" | "]" | "?" | "=" | "{" | "}" | SP | HT`.</span></span> <span data-ttu-id="feb39-127">如果在 Cookie 名称或其他 HTTP 标头中使用非 ASCII 字符，则可能导致服务器异常或客户端不正确地往返。</span><span class="sxs-lookup"><span data-stu-id="feb39-127">The use of non-ASCII characters in cookie names or other HTTP headers may cause an exception from the server or be improperly round-tripped by the client.</span></span>

## <a name="affected-apis"></a><span data-ttu-id="feb39-128">受影响的 API</span><span class="sxs-lookup"><span data-stu-id="feb39-128">Affected APIs</span></span>

- <xref:Microsoft.AspNetCore.Http.HttpRequest.Cookies%2A?displayProperty=nameWithType>
- <xref:Microsoft.AspNetCore.Http.HttpResponse.Cookies%2A?displayProperty=nameWithType>
- `Microsoft.Owin.IOwinRequest.Cookies`
- `Microsoft.Owin.IOwinResponse.Cookies`

<!--

### Category

ASP.NET Core

### Affected APIs

- `Overload:Microsoft.AspNetCore.Http.HttpRequest.Cookies`
- `Overload:Microsoft.AspNetCore.Http.HttpResponse.Cookies`
- `P:Microsoft.Owin.IOwinRequest.Cookies`
- `P:Microsoft.Owin.IOwinResponse.Cookies`

-->
