---
ms.openlocfilehash: 58e65bae1593f23945a971b896a1db4a929b4587
ms.sourcegitcommit: 79a2d6a07ba4ed08979819666a0ee6927bbf1b01
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/28/2019
ms.locfileid: "74567466"
---
### <a name="types-in-microsoftvisualbasicmyservices-namespace-not-available"></a><span data-ttu-id="db7d5-101">Microsoft. VisualBasic. MyServices ad alanındaki türler kullanılamıyor</span><span class="sxs-lookup"><span data-stu-id="db7d5-101">Types in Microsoft.VisualBasic.MyServices namespace not available</span></span>

<span data-ttu-id="db7d5-102"><xref:Microsoft.VisualBasic.MyServices?displayProperty=fullName> ad alanındaki türler kullanılamıyor.</span><span class="sxs-lookup"><span data-stu-id="db7d5-102">The types in the <xref:Microsoft.VisualBasic.MyServices?displayProperty=fullName> namespace are not available.</span></span>

#### <a name="version-introduced"></a><span data-ttu-id="db7d5-103">Sunulan sürüm</span><span class="sxs-lookup"><span data-stu-id="db7d5-103">Version introduced</span></span>

<span data-ttu-id="db7d5-104">.NET Core 3,0 Preview 8</span><span class="sxs-lookup"><span data-stu-id="db7d5-104">.NET Core 3.0 Preview 8</span></span>

#### <a name="change-description"></a><span data-ttu-id="db7d5-105">Açıklamayı Değiştir</span><span class="sxs-lookup"><span data-stu-id="db7d5-105">Change description</span></span>

<span data-ttu-id="db7d5-106"><xref:Microsoft.VisualBasic.MyServices?displayProperty=fullName> ad alanındaki türler bazı .NET Core 3,0 önizleme sürümlerinde mevcuttur.</span><span class="sxs-lookup"><span data-stu-id="db7d5-106">The types in the <xref:Microsoft.VisualBasic.MyServices?displayProperty=fullName> namespace were available in some .NET Core 3.0 Preview releases.</span></span> <span data-ttu-id="db7d5-107">.NET Core 3,0 Preview 9 ' dan itibaren artık kullanılabilir değildir.</span><span class="sxs-lookup"><span data-stu-id="db7d5-107">They are no longer available starting with .NET Core 3.0 Preview 9.</span></span>

<span data-ttu-id="db7d5-108">Gereksiz bütünleştirilmiş kod bağımlılıklarından veya sonraki sürümlerde son değişikliklerden kaçınmak için türler kaldırılmıştır.</span><span class="sxs-lookup"><span data-stu-id="db7d5-108">The types were removed to avoid unnecessary assembly dependencies or breaking changes in subsequent releases.</span></span>

#### <a name="recommended-action"></a><span data-ttu-id="db7d5-109">Önerilen eylem</span><span class="sxs-lookup"><span data-stu-id="db7d5-109">Recommended action</span></span>

<span data-ttu-id="db7d5-110">Kodunuz **Microsoft. VisualBasic. MyServices** türlerinin ve üyelerinin kullanımına bağımlıysa, .NET sınıf kitaplığı 'nda karşılık gelen türler ve Üyeler vardır.</span><span class="sxs-lookup"><span data-stu-id="db7d5-110">If your code depends on the use of **Microsoft.VisualBasic.MyServices** types and their members, there are corresponding types and members in the .NET class library.</span></span> <span data-ttu-id="db7d5-111">Aşağıda, **Microsoft. VisualBasic. MyServices** türlerinin eşdeğer .NET sınıf kitaplığı türlerine eşlenmesi verilmiştir:</span><span class="sxs-lookup"><span data-stu-id="db7d5-111">The following is a mapping of  **Microsoft.VisualBasic.MyServices** types to their equivalent .NET class library types:</span></span>

|<span data-ttu-id="db7d5-112">Microsoft. VisualBasic. MyServices türü</span><span class="sxs-lookup"><span data-stu-id="db7d5-112">Microsoft.VisualBasic.MyServices type</span></span>|<span data-ttu-id="db7d5-113">.NET sınıf kitaplığı türü</span><span class="sxs-lookup"><span data-stu-id="db7d5-113">.NET class library type</span></span>|
|--|--|
|<xref:Microsoft.VisualBasic.MyServices.ClipboardProxy>|<span data-ttu-id="db7d5-114">WPF uygulamaları için <xref:System.Windows.Clipboard?displayProperty=nameWithType>, Windows Forms uygulamalar için <xref:System.Windows.Forms.Clipboard?displayProperty=nameWithType></span><span class="sxs-lookup"><span data-stu-id="db7d5-114"><xref:System.Windows.Clipboard?displayProperty=nameWithType> for WPF applications, <xref:System.Windows.Forms.Clipboard?displayProperty=nameWithType> for Windows Forms applications</span></span>|
|<xref:Microsoft.VisualBasic.MyServices.FileSystemProxy>|<span data-ttu-id="db7d5-115"><xref:System.IO> ad alanındaki türler</span><span class="sxs-lookup"><span data-stu-id="db7d5-115">Types in the <xref:System.IO> namespace</span></span>|
|<xref:Microsoft.VisualBasic.MyServices.RegistryProxy>|<span data-ttu-id="db7d5-116"><xref:Microsoft.Win32> ad alanındaki kayıt defteriyle ilgili türler</span><span class="sxs-lookup"><span data-stu-id="db7d5-116">Registry-related types in the <xref:Microsoft.Win32> namespace</span></span>|
|<xref:Microsoft.VisualBasic.MyServices.SpecialDirectoriesProxy>|<xref:System.Environment.GetFolderPath%2A?displayProperty=nameWithType>|

#### <a name="category"></a><span data-ttu-id="db7d5-117">Kategori</span><span class="sxs-lookup"><span data-stu-id="db7d5-117">Category</span></span>

<span data-ttu-id="db7d5-118">Visual Basic</span><span class="sxs-lookup"><span data-stu-id="db7d5-118">Visual Basic</span></span>

#### <a name="affected-apis"></a><span data-ttu-id="db7d5-119">Etkilenen API’ler</span><span class="sxs-lookup"><span data-stu-id="db7d5-119">Affected APIs</span></span>

- <xref:Microsoft.VisualBasic.MyServices?displayProperty=fullName>

<!--

### Affected APIs

- `N:Microsoft.VisualBasic.MyServices`

-- >
