---
title: Fedora-.NET Core 'a .NET Core 'u yükler
description: .NET Core SDK ve .NET Core çalışma zamanını Fedora 'ya yüklemenin çeşitli yollarını gösterir.
author: thraka
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 52aa9409ec876e0d1eaef22fb739046941fda897
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84603092"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-fedora"></a><span data-ttu-id="ab663-103">Fedora üzerinde .NET Core SDK veya .NET Core çalışma zamanı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="ab663-103">Install .NET Core SDK or .NET Core Runtime on Fedora</span></span>

<span data-ttu-id="ab663-104">.NET Core, Fedora 'da desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ab663-104">.NET Core is supported on Fedora.</span></span> <span data-ttu-id="ab663-105">Bu makalede, Fedora üzerinde .NET Core 'un nasıl yükleneceği açıklanır.</span><span class="sxs-lookup"><span data-stu-id="ab663-105">This article describes how to install .NET Core on Fedora.</span></span> <span data-ttu-id="ab663-106">Bir Fedora sürümü destek dışı kaldığında, .NET Core artık bu sürümle desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="ab663-106">When a Fedora version falls out of support, .NET Core is no longer supported with that version.</span></span> <span data-ttu-id="ab663-107">Ancak, bu yönergeler desteklenmese de, bu sürümler üzerinde çalışan .NET Core 'u almanıza yardımcı olabilir.</span><span class="sxs-lookup"><span data-stu-id="ab663-107">However, these instructions may help you to get .NET Core running on those versions, even though it isn't supported.</span></span>

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

[!INCLUDE [linux-install-package-manager-x64-vs-arm](includes/linux-install-package-manager-x64-vs-arm.md)]

## <a name="supported-distributions"></a><span data-ttu-id="ab663-108">Desteklenen dağıtımlar</span><span class="sxs-lookup"><span data-stu-id="ab663-108">Supported distributions</span></span>

<span data-ttu-id="ab663-109">Aşağıdaki tabloda, şu anda desteklenen .NET Core sürümlerinin ve desteklenen Fedora sürümlerinin bir listesi verilmiştir.</span><span class="sxs-lookup"><span data-stu-id="ab663-109">The following table is a list of currently supported .NET Core releases and the versions of Fedora they're supported on.</span></span> <span data-ttu-id="ab663-110">Bu sürümler, [.NET Core 'un sürümü destek sonuna](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) veya [Fedora sürümü yaşam sonuna](https://fedoraproject.org/wiki/End_of_life)ulaşana kadar desteklenmeye devam eder.</span><span class="sxs-lookup"><span data-stu-id="ab663-110">These versions remain supported until either the version of [.NET Core reaches end-of-support](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) or the version of [Fedora reaches end-of-life](https://fedoraproject.org/wiki/End_of_life).</span></span>

- <span data-ttu-id="ab663-111">✔️, Fedora veya .NET Core sürümünün hala desteklendiğini gösterir.</span><span class="sxs-lookup"><span data-stu-id="ab663-111">A ✔️ indicates that the version of Fedora or .NET Core is still supported.</span></span>
- <span data-ttu-id="ab663-112">Bir ❌ , Fedora veya .NET Core sürümünün bu Fedora sürümünde desteklenmediğini belirtir.</span><span class="sxs-lookup"><span data-stu-id="ab663-112">A ❌ indicates that the version of Fedora or .NET Core isn't supported on that Fedora release.</span></span>
- <span data-ttu-id="ab663-113">Hem Fedora hem de bir .NET Core sürümü ✔️ olduğunda, bu işletim sistemi ve .NET birleşimi desteklenir.</span><span class="sxs-lookup"><span data-stu-id="ab663-113">When both a version of Fedora and a version of .NET Core have ✔️, that OS and .NET combination are supported.</span></span>

| <span data-ttu-id="ab663-114">Fedora</span><span class="sxs-lookup"><span data-stu-id="ab663-114">Fedora</span></span>                   | <span data-ttu-id="ab663-115">.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="ab663-115">.NET Core 2.1</span></span> | <span data-ttu-id="ab663-116">.NET Core 3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-116">.NET Core 3.1</span></span> | <span data-ttu-id="ab663-117">.NET 5 Preview (yalnızca el ile yüklenir)</span><span class="sxs-lookup"><span data-stu-id="ab663-117">.NET 5 Preview (manual install only)</span></span> |
|--------------------------|---------------|---------------|----------------|
| <span data-ttu-id="ab663-118">✔️ [32](linux-fedora.md#fedora-32-)</span><span class="sxs-lookup"><span data-stu-id="ab663-118">✔️ [32](linux-fedora.md#fedora-32-)</span></span> | <span data-ttu-id="ab663-119">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-119">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-120">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-120">✔️ 3.1</span></span>        | <span data-ttu-id="ab663-121">✔️ 5,0 Preview</span><span class="sxs-lookup"><span data-stu-id="ab663-121">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="ab663-122">✔️ [31](linux-fedora.md#fedora-31-)</span><span class="sxs-lookup"><span data-stu-id="ab663-122">✔️ [31](linux-fedora.md#fedora-31-)</span></span> | <span data-ttu-id="ab663-123">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-123">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-124">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-124">✔️ 3.1</span></span>        | <span data-ttu-id="ab663-125">✔️ 5,0 Preview</span><span class="sxs-lookup"><span data-stu-id="ab663-125">✔️ 5.0 Preview</span></span> |
| <span data-ttu-id="ab663-126">❌[30](linux-fedora.md#fedora-30-)</span><span class="sxs-lookup"><span data-stu-id="ab663-126">❌ [30](linux-fedora.md#fedora-30-)</span></span> | <span data-ttu-id="ab663-127">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-127">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-128">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-128">✔️ 3.1</span></span>        | <span data-ttu-id="ab663-129">❌5,0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="ab663-129">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="ab663-130">❌[29](linux-fedora.md#fedora-29-)</span><span class="sxs-lookup"><span data-stu-id="ab663-130">❌ [29](linux-fedora.md#fedora-29-)</span></span> | <span data-ttu-id="ab663-131">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-131">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-132">✔️ 3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-132">✔️ 3.1</span></span>        | <span data-ttu-id="ab663-133">❌5,0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="ab663-133">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="ab663-134">❌[28](linux-fedora.md#fedora-28-)</span><span class="sxs-lookup"><span data-stu-id="ab663-134">❌ [28](linux-fedora.md#fedora-28-)</span></span> | <span data-ttu-id="ab663-135">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-135">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-136">❌3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-136">❌ 3.1</span></span>        | <span data-ttu-id="ab663-137">❌5,0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="ab663-137">❌ 5.0 Preview</span></span> |
| <span data-ttu-id="ab663-138">❌[27](linux-fedora.md#fedora-27-)</span><span class="sxs-lookup"><span data-stu-id="ab663-138">❌ [27](linux-fedora.md#fedora-27-)</span></span> | <span data-ttu-id="ab663-139">✔️ 2,1</span><span class="sxs-lookup"><span data-stu-id="ab663-139">✔️ 2.1</span></span>        | <span data-ttu-id="ab663-140">❌3,1</span><span class="sxs-lookup"><span data-stu-id="ab663-140">❌ 3.1</span></span>        | <span data-ttu-id="ab663-141">❌5,0 Önizleme</span><span class="sxs-lookup"><span data-stu-id="ab663-141">❌ 5.0 Preview</span></span> |

<span data-ttu-id="ab663-142">Aşağıdaki .NET Core sürümleri artık desteklenmemektedir.</span><span class="sxs-lookup"><span data-stu-id="ab663-142">The following versions of .NET Core are no longer supported.</span></span> <span data-ttu-id="ab663-143">Bunlara yönelik İndirilenler hala yayımlandı olarak kalmaya devam eder:</span><span class="sxs-lookup"><span data-stu-id="ab663-143">The downloads for these still remain published:</span></span>

- <span data-ttu-id="ab663-144">3.0</span><span class="sxs-lookup"><span data-stu-id="ab663-144">3.0</span></span>
- <span data-ttu-id="ab663-145">2,2</span><span class="sxs-lookup"><span data-stu-id="ab663-145">2.2</span></span>
- <span data-ttu-id="ab663-146">2.0</span><span class="sxs-lookup"><span data-stu-id="ab663-146">2.0</span></span>

## <a name="how-to-install-other-versions"></a><span data-ttu-id="ab663-147">Diğer sürümleri nasıl yüklenir</span><span class="sxs-lookup"><span data-stu-id="ab663-147">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="fedora-32-"></a><span data-ttu-id="ab663-148">Fedora 32 ✔️</span><span class="sxs-lookup"><span data-stu-id="ab663-148">Fedora 32 ✔️</span></span>

<span data-ttu-id="ab663-149">.NET Core 3,1, Fedora 32 için varsayılan paket depolarında kullanılabilir.</span><span class="sxs-lookup"><span data-stu-id="ab663-149">.NET Core 3.1 is available in the default package repositories for Fedora 32.</span></span>

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="fedora-31-"></a><span data-ttu-id="ab663-150">Fedora 31 ✔️</span><span class="sxs-lookup"><span data-stu-id="ab663-150">Fedora 31 ✔️</span></span>

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/31/prod.repo
```

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="fedora-30-"></a><span data-ttu-id="ab663-151">Fedora 30❌</span><span class="sxs-lookup"><span data-stu-id="ab663-151">Fedora 30 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-fedora.md)]

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/30/prod.repo
```

[!INCLUDE [linux-dnf-install-31](includes/linux-install-31-dnf.md)]

## <a name="fedora-29-"></a><span data-ttu-id="ab663-152">Fedora 29❌</span><span class="sxs-lookup"><span data-stu-id="ab663-152">Fedora 29 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-fedora.md)]

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/29/prod.repo
```

[!INCLUDE [linux-dnf-install-30](includes/linux-install-30-dnf.md)]

## <a name="fedora-28-"></a><span data-ttu-id="ab663-153">Fedora 28❌</span><span class="sxs-lookup"><span data-stu-id="ab663-153">Fedora 28 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-fedora.md)]

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/28/prod.repo
```

[!INCLUDE [linux-dnf-install-20](includes/linux-install-20-dnf.md)]

## <a name="fedora-27-"></a><span data-ttu-id="ab663-154">Fedora 27❌</span><span class="sxs-lookup"><span data-stu-id="ab663-154">Fedora 27 ❌</span></span>

[!INCLUDE [linux-not-supported](includes/linux-not-supported-fedora.md)]

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/27/prod.repo
```

[!INCLUDE [linux-dnf-install-20](includes/linux-install-20-dnf.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="ab663-155">Paket yöneticisinin sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="ab663-155">Troubleshoot the package manager</span></span>

<span data-ttu-id="ab663-156">Bu bölüm, .NET Core 'u yüklemek için Paket Yöneticisi 'ni kullanırken karşılaşabileceğiniz yaygın hatalarla ilgili bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="ab663-156">This section provides information on common errors you may get while using the package manager to install .NET Core.</span></span>

### <a name="failed-to-fetch"></a><span data-ttu-id="ab663-157">Getirilemedi</span><span class="sxs-lookup"><span data-stu-id="ab663-157">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="snap"></a><span data-ttu-id="ab663-158">Bileşenlerinden</span><span class="sxs-lookup"><span data-stu-id="ab663-158">Snap</span></span>

[!INCLUDE [linux-install-snap](includes/linux-install-snap.md)]

## <a name="dependencies"></a><span data-ttu-id="ab663-159">Bağımlılıklar</span><span class="sxs-lookup"><span data-stu-id="ab663-159">Dependencies</span></span>

[!INCLUDE [linux-install-dependencies](includes/linux-install-dependencies.md)]

## <a name="scripted-install"></a><span data-ttu-id="ab663-160">Komut dosyalı yüklemesi</span><span class="sxs-lookup"><span data-stu-id="ab663-160">Scripted install</span></span>

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a><span data-ttu-id="ab663-161">El ile yüklemesi</span><span class="sxs-lookup"><span data-stu-id="ab663-161">Manual install</span></span>

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a><span data-ttu-id="ab663-162">Sonraki adımlar</span><span class="sxs-lookup"><span data-stu-id="ab663-162">Next steps</span></span>

- [<span data-ttu-id="ab663-163">Öğretici: Visual Studio Code kullanarak .NET Core SDK bir konsol uygulaması oluşturma</span><span class="sxs-lookup"><span data-stu-id="ab663-163">Tutorial: Create a console application with .NET Core SDK using Visual Studio Code</span></span>](../tutorials/with-visual-studio-code.md)