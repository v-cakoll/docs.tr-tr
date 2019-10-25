---
ms.openlocfilehash: f95c3916f4da8164cf927344f60f2845f04ddc5c
ms.sourcegitcommit: 2e95559d957a1a942e490c5fd916df04b39d73a9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/16/2019
ms.locfileid: "72393985"
---
### <a name="kestrel-transport-abstractions-removed-and-made-public"></a><span data-ttu-id="20d7a-101">Kestrel: kaldırılan ve genel kullanıma açık olan taşıma soyutlamaları</span><span class="sxs-lookup"><span data-stu-id="20d7a-101">Kestrel: Transport abstractions removed and made public</span></span>

<span data-ttu-id="20d7a-102">"Pubternal" API 'lerinden uzaklaşma kapsamında, Kestrel aktarım katmanı API 'Leri `Microsoft.AspNetCore.Connections.Abstractions` kitaplığında ortak arabirim olarak sunulur.</span><span class="sxs-lookup"><span data-stu-id="20d7a-102">As part of moving away from "pubternal" APIs, the Kestrel transport layer APIs are exposed as a public interface in the `Microsoft.AspNetCore.Connections.Abstractions` library.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="20d7a-103">Sunulan sürüm</span><span class="sxs-lookup"><span data-stu-id="20d7a-103">Version introduced</span></span>

<span data-ttu-id="20d7a-104">3.0</span><span class="sxs-lookup"><span data-stu-id="20d7a-104">3.0</span></span>

#### <a name="old-behavior"></a><span data-ttu-id="20d7a-105">Eski davranış</span><span class="sxs-lookup"><span data-stu-id="20d7a-105">Old behavior</span></span>

- <span data-ttu-id="20d7a-106">Taşıma ile ilgili soyutlamalar `Microsoft.AspNetCore.Server.Kestrel.Transport.Abstractions` kitaplığı 'nda mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="20d7a-106">Transport-related abstractions were available in the `Microsoft.AspNetCore.Server.Kestrel.Transport.Abstractions` library.</span></span>
- <span data-ttu-id="20d7a-107">@No__t-0 özelliği kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="20d7a-107">The `ListenOptions.NoDelay` property was available.</span></span>

#### <a name="new-behavior"></a><span data-ttu-id="20d7a-108">Yeni davranış</span><span class="sxs-lookup"><span data-stu-id="20d7a-108">New behavior</span></span>

- <span data-ttu-id="20d7a-109">@No__t-0 arabirimi, `...Transport.Abstractions` kitaplığından en çok kullanılan işlevselliği göstermek için `Microsoft.AspNetCore.Connections.Abstractions` kitaplığında tanıtılmıştı.</span><span class="sxs-lookup"><span data-stu-id="20d7a-109">The `IConnectionListener` interface was introduced in the `Microsoft.AspNetCore.Connections.Abstractions` library to expose the most used functionality from the `...Transport.Abstractions` library.</span></span>
- <span data-ttu-id="20d7a-110">@No__t-0 artık aktarım seçeneklerinde (`LibuvTransportOptions` ve `SocketTransportOptions`) kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="20d7a-110">The `NoDelay` is now available in transport options (`LibuvTransportOptions` and `SocketTransportOptions`).</span></span>
- <span data-ttu-id="20d7a-111">`SchedulingMode` artık kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="20d7a-111">`SchedulingMode` is no longer available.</span></span>

#### <a name="reason-for-change"></a><span data-ttu-id="20d7a-112">Değişiklik nedeni</span><span class="sxs-lookup"><span data-stu-id="20d7a-112">Reason for change</span></span>

<span data-ttu-id="20d7a-113">ASP.NET Core 3,0, "pubternal" API 'Lerinden uzağa taşındı.</span><span class="sxs-lookup"><span data-stu-id="20d7a-113">ASP.NET Core 3.0 has moved away from "pubternal" APIs.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="20d7a-114">Önerilen eylem</span><span class="sxs-lookup"><span data-stu-id="20d7a-114">Recommended action</span></span>

#### <a name="category"></a><span data-ttu-id="20d7a-115">Kategori</span><span class="sxs-lookup"><span data-stu-id="20d7a-115">Category</span></span>

<span data-ttu-id="20d7a-116">ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="20d7a-116">ASP.NET Core</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="20d7a-117">Etkilenen API’ler</span><span class="sxs-lookup"><span data-stu-id="20d7a-117">Affected APIs</span></span>

<span data-ttu-id="20d7a-118">Yok.</span><span class="sxs-lookup"><span data-stu-id="20d7a-118">None</span></span>

<!-- 

### Affected APIs

Not detectable via API analysis

-->