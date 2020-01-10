---
title: Özel SQLite sürümleri
ms.date: 12/13/2019
description: Yerel SQLite kitaplığı 'nın özel bir sürümünü nasıl kullanacağınızı öğrenin.
ms.openlocfilehash: 8a2646138ea9dbecf412a2e8e0e347e2d71a5b0b
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447148"
---
# <a name="custom-sqlite-versions"></a><span data-ttu-id="2ca81-103">Özel SQLite sürümleri</span><span class="sxs-lookup"><span data-stu-id="2ca81-103">Custom SQLite versions</span></span>

<span data-ttu-id="2ca81-104">Microsoft. Data. SQLite, SQLitePCLRaw üzerine kurulmuştur.</span><span class="sxs-lookup"><span data-stu-id="2ca81-104">Microsoft.Data.Sqlite is built on top of SQLitePCLRaw.</span></span> <span data-ttu-id="2ca81-105">Bir paket kullanarak veya bir SQLitePCLRaw sağlayıcısı yapılandırarak yerel SQLite kitaplığının özel sürümlerini kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ca81-105">You can use custom versions of the native SQLite library by using a bundle or by configuring a SQLitePCLRaw provider.</span></span>

## <a name="bundles"></a><span data-ttu-id="2ca81-106">Paketi</span><span class="sxs-lookup"><span data-stu-id="2ca81-106">Bundles</span></span>

<span data-ttu-id="2ca81-107">SQLitePCLRaw, farklı platformlarda doğru bağımlılıklara kolayca getirmeyi kolaylaştıran paket paketleri sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ca81-107">SQLitePCLRaw provides bundle packages that make it easy to bring in the right dependencies across different platforms.</span></span>

<span data-ttu-id="2ca81-108">Ana Microsoft. Data. SQLite paketi varsayılan olarak SQLitePCLRaw. bundle_e_sqlite3 ' ye getirir.</span><span class="sxs-lookup"><span data-stu-id="2ca81-108">The main Microsoft.Data.Sqlite package brings in SQLitePCLRaw.bundle_e_sqlite3 by default.</span></span>

<span data-ttu-id="2ca81-109">Farklı bir paket kullanmak için, kullanmak istediğiniz paket paketiyle birlikte `Microsoft.Data.Sqlite.Core` paketini de yükleyebilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ca81-109">To use a different bundle, install the `Microsoft.Data.Sqlite.Core` package instead along with the bundle package you want to use.</span></span> <span data-ttu-id="2ca81-110">Paketleri Microsoft. Data. SQLite tarafından otomatik olarak başlatılır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-110">Bundles are automatically initialized by Microsoft.Data.Sqlite.</span></span>

| <span data-ttu-id="2ca81-111">Paket</span><span class="sxs-lookup"><span data-stu-id="2ca81-111">Bundle</span></span> | <span data-ttu-id="2ca81-112">Açıklama</span><span class="sxs-lookup"><span data-stu-id="2ca81-112">Description</span></span> |
| --- | --- |
| <span data-ttu-id="2ca81-113">SQLitePCLRaw. bundle_e_sqlite3</span><span class="sxs-lookup"><span data-stu-id="2ca81-113">SQLitePCLRaw.bundle_e_sqlite3</span></span> | <span data-ttu-id="2ca81-114">Tüm platformlarda SQLite ' un tutarlı bir sürümünü sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ca81-114">Provides a consistent version of SQLite on all platforms.</span></span> <span data-ttu-id="2ca81-115">FTS4, FTS5, JSON1 ve</span><span class="sxs-lookup"><span data-stu-id="2ca81-115">Includes the FTS4, FTS5, JSON1, and</span></span> | <span data-ttu-id="2ca81-116">R \* ağaç uzantıları.</span><span class="sxs-lookup"><span data-stu-id="2ca81-116">R\*Tree extensions.</span></span> <span data-ttu-id="2ca81-117">Bu varsayılandır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-117">This is the default.</span></span> |
| <span data-ttu-id="2ca81-118">SQLitePCLRaw. bundle_green</span><span class="sxs-lookup"><span data-stu-id="2ca81-118">SQLitePCLRaw.bundle_green</span></span> | <span data-ttu-id="2ca81-119">System SQLite kitaplığını kullandığı iOS dışında bundle_e_sqlite3 ile aynıdır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-119">Same as bundle_e_sqlite3, except on iOS where it uses the system SQLite library.</span></span> |
| <span data-ttu-id="2ca81-120">SQLitePCLRaw. bundle_zetetic</span><span class="sxs-lookup"><span data-stu-id="2ca81-120">SQLitePCLRaw.bundle_zetetic</span></span> | <span data-ttu-id="2ca81-121">Zetetik 'dan (dahil değil) resmi SQLCipher derlemelerini kullanır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-121">Uses the official SQLCipher builds from Zetetic (not included).</span></span> |
| <span data-ttu-id="2ca81-122">SQLitePCLRaw. bundle_winsqlite3</span><span class="sxs-lookup"><span data-stu-id="2ca81-122">SQLitePCLRaw.bundle_winsqlite3</span></span> | <span data-ttu-id="2ca81-123">Windows 10 ' da sistem SQLite kitaplığı olan winsqlite3. dll ' yi kullanır.</span><span class="sxs-lookup"><span data-stu-id="2ca81-123">Uses winsqlite3.dll, the system SQLite library on Windows 10.</span></span> |
| <span data-ttu-id="2ca81-124">SQLitePCLRaw. bundle_e_sqlcipher</span><span class="sxs-lookup"><span data-stu-id="2ca81-124">SQLitePCLRaw.bundle_e_sqlcipher</span></span> | <span data-ttu-id="2ca81-125">, Bir SQLCipher 'ın resmi olmayan, açık kaynaklı bir derlemesini sağlar.</span><span class="sxs-lookup"><span data-stu-id="2ca81-125">Provides an unofficial, open-source build of SQLCipher.</span></span> |

<span data-ttu-id="2ca81-126">Örneğin, bir SQLCipher 'ın resmi olmayan, açık kaynaklı derlemesini kullanmak için aşağıdaki komutları kullanın.</span><span class="sxs-lookup"><span data-stu-id="2ca81-126">For example, to use the unofficial, open-source build of SQLCipher use the following commands.</span></span>

### <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="2ca81-127">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2ca81-127">.NET Core CLI</span></span>](#tab/netcore-cli)

```dotnetcli
dotnet add package Microsoft.Data.Sqlite.Core
dotnet add package SQLitePCLRaw.bundle_e_sqlcipher
```

### <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2ca81-128">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2ca81-128">Visual Studio</span></span>](#tab/visual-studio)

``` PowerShell
Install-Package Microsoft.Data.Sqlite.Core
Install-Package SQLitePCLRaw.bundle_e_sqlcipher
```

---

## <a name="sqlitepclraw-providers"></a><span data-ttu-id="2ca81-129">SQLitePCLRaw sağlayıcıları</span><span class="sxs-lookup"><span data-stu-id="2ca81-129">SQLitePCLRaw providers</span></span>

<span data-ttu-id="2ca81-130">`SQLitePCLRaw.provider.dynamic_cdecl` paketinden yararlanarak, kendi SQLite yapınızı kullanabilirsiniz.</span><span class="sxs-lookup"><span data-stu-id="2ca81-130">You can use your own build of SQLite by leveraging the `SQLitePCLRaw.provider.dynamic_cdecl` package.</span></span> <span data-ttu-id="2ca81-131">Bu durumda, yerel kitaplığı uygulamanızla dağıtmaktan sorumlu olursunuz.</span><span class="sxs-lookup"><span data-stu-id="2ca81-131">In this case, you're responsible for deploying the native library with your app.</span></span> <span data-ttu-id="2ca81-132">Bu şekilde, uygulamanıza yerel kitaplıklar dağıtmanın ayrıntıları, kullandığınız .NET platformu ve çalışma zamanına bağlı olarak önemli ölçüde farklılık gösterir.</span><span class="sxs-lookup"><span data-stu-id="2ca81-132">Note, the details of deploying native libraries with your app vary considerably depending on which .NET platform and runtime you're using.</span></span>

<span data-ttu-id="2ca81-133">İlk olarak, IGetFunctionPointer uygulamanız gerekir.</span><span class="sxs-lookup"><span data-stu-id="2ca81-133">First, you'll need to implement IGetFunctionPointer.</span></span> <span data-ttu-id="2ca81-134">Uygulama .NET Core üzerinde oldukça basit.</span><span class="sxs-lookup"><span data-stu-id="2ca81-134">The implementation is fairly trivial on .NET Core.</span></span>

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_NativeLibraryAdapter)]

<span data-ttu-id="2ca81-135">Sonra, SQLitePCLRaw sağlayıcısını yapılandırın.</span><span class="sxs-lookup"><span data-stu-id="2ca81-135">Next, configure the SQLitePCLRaw provider.</span></span> <span data-ttu-id="2ca81-136">Uygulamanızda Microsoft. Data. SQLite kullanılmadan önce bunun yapıldığından emin olun.</span><span class="sxs-lookup"><span data-stu-id="2ca81-136">Ensure this is done before Microsoft.Data.Sqlite is used in your app.</span></span> <span data-ttu-id="2ca81-137">Ayrıca, sağlayıcınızı geçersiz kılabilecek bir SQLitePCLRaw demeti paketi kullanmaktan kaçının.</span><span class="sxs-lookup"><span data-stu-id="2ca81-137">Also, avoid using a SQLitePCLRaw bundle package which might override your provider.</span></span>

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/SystemLibrarySample/Program.cs?name=snippet_SetProvider)]