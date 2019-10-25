---
ms.openlocfilehash: a4e20e0468d861138ad801c9dbfa15340b3f388c
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72394239"
---
### <a name="authentication-oauthhandler-exchangecodeasync-signature-changed"></a><span data-ttu-id="70b30-101">Kimlik doğrulaması: OAuthHandler ExchangeCodeAsync imzası değişti</span><span class="sxs-lookup"><span data-stu-id="70b30-101">Authentication: OAuthHandler ExchangeCodeAsync signature changed</span></span>

<span data-ttu-id="70b30-102">ASP.NET Core 3,0 ' de, `OAuthHandler.ExchangeCodeAsync` ' ın imzası şu şekilde değiştirildi:</span><span class="sxs-lookup"><span data-stu-id="70b30-102">In ASP.NET Core 3.0, the signature of `OAuthHandler.ExchangeCodeAsync` was changed from:</span></span>

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(string code, string redirectUri) { throw null; }
```

<span data-ttu-id="70b30-103">Hedef:</span><span class="sxs-lookup"><span data-stu-id="70b30-103">To:</span></span>

```csharp
protected virtual System.Threading.Tasks.Task<Microsoft.AspNetCore.Authentication.OAuth.OAuthTokenResponse> ExchangeCodeAsync(Microsoft.AspNetCore.Authentication.OAuth.OAuthCodeExchangeContext context) { throw null; }
```

#### <a name="version-introduced"></a><span data-ttu-id="70b30-104">Sunulan sürüm</span><span class="sxs-lookup"><span data-stu-id="70b30-104">Version introduced</span></span>

<span data-ttu-id="70b30-105">3.0</span><span class="sxs-lookup"><span data-stu-id="70b30-105">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="70b30-106">Eski davranış</span><span class="sxs-lookup"><span data-stu-id="70b30-106">Old behavior</span></span>

<span data-ttu-id="70b30-107">@No__t-0 ve `redirectUri` dizeleri ayrı bağımsız değişkenler olarak geçirildi.</span><span class="sxs-lookup"><span data-stu-id="70b30-107">The `code` and `redirectUri` strings were passed as separate arguments.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="70b30-108">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="70b30-108">New behavior</span></span>

<span data-ttu-id="70b30-109">`Code` ve `RedirectUri`, `OAuthCodeExchangeContext` Oluşturucusu aracılığıyla ayarlanabilir `OAuthCodeExchangeContext` ' deki özelliklerdir.</span><span class="sxs-lookup"><span data-stu-id="70b30-109">`Code` and `RedirectUri` are properties on `OAuthCodeExchangeContext` that can be set via the `OAuthCodeExchangeContext` constructor.</span></span> <span data-ttu-id="70b30-110">Yeni `OAuthCodeExchangeContext` türü, `OAuthHandler.ExchangeCodeAsync` ' e geçirilen tek bağımsız değişkendir.</span><span class="sxs-lookup"><span data-stu-id="70b30-110">The new `OAuthCodeExchangeContext` type is the only argument passed to `OAuthHandler.ExchangeCodeAsync`.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="70b30-111">Değişiklik nedeni</span><span class="sxs-lookup"><span data-stu-id="70b30-111">Reason for change</span></span>

<span data-ttu-id="70b30-112">Bu değişiklik, ek parametrelerin kırılmamış bir şekilde sağlanmasını sağlar.</span><span class="sxs-lookup"><span data-stu-id="70b30-112">This change allows additional parameters to be provided in a non-breaking manner.</span></span> <span data-ttu-id="70b30-113">Yeni @no__t 0 aşırı yüklemeleri oluşturmanız gerekmez.</span><span class="sxs-lookup"><span data-stu-id="70b30-113">There's no need to create new `ExchangeCodeAsync` overloads.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="70b30-114">Önerilen eylem</span><span class="sxs-lookup"><span data-stu-id="70b30-114">Recommended action</span></span>

<span data-ttu-id="70b30-115">Uygun `code` ve `redirectUri` değerleriyle `OAuthCodeExchangeContext` oluşturun.</span><span class="sxs-lookup"><span data-stu-id="70b30-115">Construct an `OAuthCodeExchangeContext` with the appropriate `code` and `redirectUri` values.</span></span> <span data-ttu-id="70b30-116">@No__t-0 örneği sağlanmalıdır.</span><span class="sxs-lookup"><span data-stu-id="70b30-116">An <xref:Microsoft.AspNetCore.Authentication.AuthenticationProperties> instance must be provided.</span></span> <span data-ttu-id="70b30-117">Bu tek `OAuthCodeExchangeContext` örneği, birden çok bağımsız değişken yerine `OAuthHandler.ExchangeCodeAsync` ' e geçirilebilir.</span><span class="sxs-lookup"><span data-stu-id="70b30-117">This single `OAuthCodeExchangeContext` instance can be passed to `OAuthHandler.ExchangeCodeAsync` instead of multiple arguments.</span></span>

#### <a name="category"></a><span data-ttu-id="70b30-118">Kategori</span><span class="sxs-lookup"><span data-stu-id="70b30-118">Category</span></span>

<span data-ttu-id="70b30-119">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="70b30-119">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="70b30-120">Etkilenen API’ler</span><span class="sxs-lookup"><span data-stu-id="70b30-120">Affected APIs</span></span>

<xref:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler%601.ExchangeCodeAsync(System.String,System.String)?displayProperty=nameWithType>

<!--

#### Affected APIs

`M:Microsoft.AspNetCore.Authentication.OAuth.OAuthHandler`1.ExchangeCodeAsync(System.String,System.String)`

-->