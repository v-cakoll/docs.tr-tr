---
title: CentOS 'a .NET Core 'u yükler-.NET Core
description: .NET Core SDK ve .NET Core çalışma zamanının CentOS 'a yüklenmesi için çeşitli yollar gösterir.
author: thraka
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 743bd4ce47fdecef512f9605d8ec5503eb6da9ba
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84603141"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-centos"></a><span data-ttu-id="2f752-103">CentOS 'a .NET Core SDK veya .NET Core çalışma zamanı 'nı yükler</span><span class="sxs-lookup"><span data-stu-id="2f752-103">Install .NET Core SDK or .NET Core Runtime on CentOS</span></span>

<span data-ttu-id="2f752-104">.NET Core, CentOS 'da desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2f752-104">.NET Core is supported on CentOS.</span></span> <span data-ttu-id="2f752-105">Bu makalede, CentOS üzerinde .NET Core 'un nasıl yükleneceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="2f752-105">This article describes how to install .NET Core on CentOS.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="2f752-106">Desteklenen dağıtımlar</span><span class="sxs-lookup"><span data-stu-id="2f752-106">Supported distributions</span></span>

<span data-ttu-id="2f752-107">Aşağıdaki tabloda, hem CentOS 7 hem de CentOS 8 ' de şu anda desteklenen .NET Core sürümlerinin bir listesi verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="2f752-107">The following table is a list of currently supported .NET Core releases on both CentOS 7 and CentOS 8.</span></span> <span data-ttu-id="2f752-108">Bu sürümler, [.NET Core sürümü destek sonu](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) veya CentOS sürümü artık desteklenene kadar desteklenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="2f752-108">These versions remain supported until either the version of [.NET Core reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of CentOS is no longer supported.</span></span>

- <span data-ttu-id="2f752-109">✔️, CentOS veya .NET Core sürümünün hala desteklendiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="2f752-109">A ✔️ indicates that the version of CentOS or .NET Core is still supported.</span></span>
- <span data-ttu-id="2f752-110">Bir ❌ , CentOS veya .NET Core sürümünün bu CentOS sürümünde desteklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="2f752-110">A ❌ indicates that the version of CentOS or .NET Core isn't supported on that CentOS release.</span></span>
- <span data-ttu-id="2f752-111">Hem CentOS hem de .NET Core sürümü ✔️ olduğunda, bu işletim sistemi ve .NET birleşimi desteklenir.</span><span class="sxs-lookup"><span data-stu-id="2f752-111">When both a version of CentOS and a version of .NET Core have ✔️, that OS and .NET combination are supported.</span></span>

| <span data-ttu-id="2f752-112">CentOS</span><span class="sxs-lookup"><span data-stu-id="2f752-112">CentOS</span></span>                   | <span data-ttu-id="2f752-113">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="2f752-113">.NET Core 2.1</span></span> | <span data-ttu-id="2f752-114">.NET Core 3,1</span><span class="sxs-lookup"><span data-stu-id="2f752-114">.NET Core 3.1</span></span> | <span data-ttu-id="2f752-115">.NET 5 Preview (yalnızca el ile yüklenir)</span><span class="sxs-lookup"><span data-stu-id="2f752-115">.NET 5 Preview (manual install only)</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="2f752-116">✔️ [8](#centos-8-)</span><span class="sxs-lookup"><span data-stu-id="2f752-116">✔️ [8](#centos-8-)</span></span> | <span data-ttu-id="2f752-117">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="2f752-117">✔️ 2.1</span></span>        | <span data-ttu-id="2f752-118">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="2f752-118">✔️ 3.1</span></span>        | <span data-ttu-id="2f752-119">✔️ 5,0 Preview</span><span class="sxs-lookup"><span data-stu-id="2f752-119">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="2f752-120">✔️ [7](#centos-7-)</span><span class="sxs-lookup"><span data-stu-id="2f752-120">✔️ [7](#centos-7-)</span></span> | <span data-ttu-id="2f752-121">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="2f752-121">✔️ 2.1</span></span>        | <span data-ttu-id="2f752-122">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="2f752-122">✔️ 3.1</span></span>        | <span data-ttu-id="2f752-123">✔️ 5,0 Preview</span><span class="sxs-lookup"><span data-stu-id="2f752-123">✔️ 5.0 Preview</span></span> |

<span data-ttu-id="2f752-124">Aşağıdaki .NET Core sürümleri artık desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="2f752-124">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="2f752-125">Bunlara yönelik İndirilenler hala yayımlandı olarak kalmaya devam eder:</span><span class="sxs-lookup"><span data-stu-id="2f752-125">The downloads for these still remain published:</span></span>

- <span data-ttu-id="2f752-126">3.0</span><span class="sxs-lookup"><span data-stu-id="2f752-126">3.0</span></span>
- <span data-ttu-id="2f752-127">2,2</span><span class="sxs-lookup"><span data-stu-id="2f752-127">2.2</span></span>
- <span data-ttu-id="2f752-128">2.0</span><span class="sxs-lookup"><span data-stu-id="2f752-128">2.0</span></span>

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="how-to-install-other-versions"></a><span data-ttu-id="2f752-129">Diğer sürümleri nasıl yüklenir</span><span class="sxs-lookup"><span data-stu-id="2f752-129">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="centos-8-"></a><span data-ttu-id="2f752-130">CentOS 8 ✔️</span><span class="sxs-lookup"><span data-stu-id="2f752-130">CentOS 8 ✔️</span></span>

<span data-ttu-id="2f752-131">.NET Core 3,1, CentOS 8 için varsayılan paket depolarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="2f752-131">.NET Core 3.1 is available in the default package repositories for CentOS 8.</span></span>

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="centos-7-"></a><span data-ttu-id="2f752-132">CentOS 7 ✔️</span><span class="sxs-lookup"><span data-stu-id="2f752-132">CentOS 7 ✔️</span></span>

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/centos/7/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-yum-install-31](includes/linux-install-31-yum.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="2f752-133">Paket yöneticisinin sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="2f752-133">Troubleshoot the package manager</span></span>

<span data-ttu-id="2f752-134">Bu bölüm, .NET Core 'u yüklemek için Paket Yöneticisi 'ni kullanırken karşılaşabileceğiniz yaygın hatalarla ilgili bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="2f752-134">This section provides information on common errors you may get while using the package manager to install .NET Core.</span></span>

### <a name="failed-to-fetch"></a><span data-ttu-id="2f752-135">Getirilemedi</span><span class="sxs-lookup"><span data-stu-id="2f752-135">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="snap"></a><span data-ttu-id="2f752-136">Bileşenlerinden</span><span class="sxs-lookup"><span data-stu-id="2f752-136">Snap</span></span>

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a><span data-ttu-id="2f752-137">Bağımlılıklar</span><span class="sxs-lookup"><span data-stu-id="2f752-137">Dependencies</span></span>

[!INCLUDE [linux-install-dependencies](includes/linux-install-dependencies.md)]

## <a name="scripted-install"></a><span data-ttu-id="2f752-138">Komut dosyalı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="2f752-138">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="2f752-139">El ile yüklemesi</span><span class="sxs-lookup"><span data-stu-id="2f752-139">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="2f752-140">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="2f752-140">Next steps</span></span>

- [<span data-ttu-id="2f752-141">Öğretici: Visual Studio Code kullanarak .NET Core SDK bir konsol uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="2f752-141">Tutorial: Create a console application with .NET Core SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)