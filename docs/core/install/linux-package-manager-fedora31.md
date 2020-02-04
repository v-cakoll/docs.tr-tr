---
title: .NET Core 'u Fedora 31-Package Manager-.NET Core 'a yükler
description: .NET Core SDK ve çalışma zamanını Fedora 31 ' de yüklemek için bir paket Yöneticisi kullanın.
author: thraka
ms.author: adegeo
ms.date: 12/17/2019
ms.openlocfilehash: 28bda3676f99037e565080e1ff3f9d89a67d0d69
ms.sourcegitcommit: cdf5084648bf5e77970cbfeaa23f1cab3e6e234e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2020
ms.locfileid: "76920776"
---
# <a name="fedora-31-package-manager---install-net-core"></a><span data-ttu-id="e99a5-103">Fedora 31 Paket Yöneticisi-.NET Core 'ı yükler</span><span class="sxs-lookup"><span data-stu-id="e99a5-103">Fedora 31 Package Manager - Install .NET Core</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-switcher.md)]

<span data-ttu-id="e99a5-104">Bu makalede, Fedora 31 ' de .NET Core yüklemek için bir paket yöneticisi 'nin nasıl kullanılacağı açıklanır.</span><span class="sxs-lookup"><span data-stu-id="e99a5-104">This article describes how to use a package manager to install .NET Core on Fedora 31.</span></span> <span data-ttu-id="e99a5-105">Çalışma zamanını yüklüyorsanız, hem .NET Core 'u hem de ASP.NET Core çalışma zamanlarını içerdiğinden [ASP.NET Core çalışma zamanını](#install-the-aspnet-core-runtime)yüklemenizi öneririz.</span><span class="sxs-lookup"><span data-stu-id="e99a5-105">If you're installing the runtime, we suggest you install the [ASP.NET Core runtime](#install-the-aspnet-core-runtime), as it includes both .NET Core and ASP.NET Core runtimes.</span></span>

## <a name="register-microsoft-key-and-feed"></a><span data-ttu-id="e99a5-106">Microsoft anahtar ve akışını Kaydet</span><span class="sxs-lookup"><span data-stu-id="e99a5-106">Register Microsoft key and feed</span></span>

<span data-ttu-id="e99a5-107">.NET yüklemeden önce şunları yapmanız gerekir:</span><span class="sxs-lookup"><span data-stu-id="e99a5-107">Before installing .NET, you'll need to:</span></span>

- <span data-ttu-id="e99a5-108">Microsoft anahtarını kaydettirin.</span><span class="sxs-lookup"><span data-stu-id="e99a5-108">Register the Microsoft key.</span></span>
- <span data-ttu-id="e99a5-109">Ürün deposunu kaydedin.</span><span class="sxs-lookup"><span data-stu-id="e99a5-109">Register the product repository.</span></span>
- <span data-ttu-id="e99a5-110">Gerekli bağımlılıkları yükler.</span><span class="sxs-lookup"><span data-stu-id="e99a5-110">Install required dependencies.</span></span>

<span data-ttu-id="e99a5-111">Bu, makine başına yalnızca bir kez yapılmalıdır.</span><span class="sxs-lookup"><span data-stu-id="e99a5-111">This only needs to be done once per machine.</span></span>

<span data-ttu-id="e99a5-112">Bir Terminal açın ve aşağıdaki komutları çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e99a5-112">Open a terminal and run the following commands.</span></span>

```bash
sudo rpm --import https://packages.microsoft.com/keys/microsoft.asc
sudo wget -q -O /etc/yum.repos.d/microsoft-prod.repo https://packages.microsoft.com/config/fedora/31/prod.repo
```

## <a name="install-the-net-core-sdk"></a><span data-ttu-id="e99a5-113">.NET Core SDK 'i yükler</span><span class="sxs-lookup"><span data-stu-id="e99a5-113">Install the .NET Core SDK</span></span>

<span data-ttu-id="e99a5-114">Yükleme için kullanılabilen ürünleri güncelleştirin, ardından .NET Core SDK yükleme.</span><span class="sxs-lookup"><span data-stu-id="e99a5-114">Update the products available for installation, then install the .NET Core SDK.</span></span> <span data-ttu-id="e99a5-115">Terminalinizde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e99a5-115">In your terminal, run the following command.</span></span>

```bash
sudo dnf install dotnet-sdk-3.1
```

## <a name="install-the-aspnet-core-runtime"></a><span data-ttu-id="e99a5-116">ASP.NET Core çalışma zamanını yükler</span><span class="sxs-lookup"><span data-stu-id="e99a5-116">Install the ASP.NET Core runtime</span></span>

<span data-ttu-id="e99a5-117">Yükleme için kullanılabilen ürünleri güncelleştirin, sonra ASP.NET çalışma zamanını yükleme.</span><span class="sxs-lookup"><span data-stu-id="e99a5-117">Update the products available for installation, then install the ASP.NET runtime.</span></span> <span data-ttu-id="e99a5-118">Terminalinizde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e99a5-118">In your terminal, run the following command.</span></span>

```bash
sudo dnf install aspnetcore-runtime-3.1
```

## <a name="install-the-net-core-runtime"></a><span data-ttu-id="e99a5-119">.NET Core çalışma zamanını yükler</span><span class="sxs-lookup"><span data-stu-id="e99a5-119">Install the .NET Core runtime</span></span>

<span data-ttu-id="e99a5-120">Yükleme için kullanılabilen ürünleri güncelleştirin ve ardından .NET Core çalışma zamanı 'nı yükleme.</span><span class="sxs-lookup"><span data-stu-id="e99a5-120">Update the products available for installation, then install the .NET Core runtime.</span></span> <span data-ttu-id="e99a5-121">Terminalinizde aşağıdaki komutu çalıştırın.</span><span class="sxs-lookup"><span data-stu-id="e99a5-121">In your terminal, run the following command.</span></span>

```bash
sudo dnf install dotnet-runtime-3.1
```

## <a name="how-to-install-other-versions"></a><span data-ttu-id="e99a5-122">Diğer sürümleri nasıl yüklenir</span><span class="sxs-lookup"><span data-stu-id="e99a5-122">How to install other versions</span></span>

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="troubleshoot-the-package-manager"></a><span data-ttu-id="e99a5-123">Paket yöneticisinin sorunlarını giderme</span><span class="sxs-lookup"><span data-stu-id="e99a5-123">Troubleshoot the package manager</span></span>

<span data-ttu-id="e99a5-124">Bu bölüm, .NET Core 'u yüklemek için Paket Yöneticisi 'ni kullanırken karşılaşabileceğiniz yaygın hatalarla ilgili bilgiler sağlar.</span><span class="sxs-lookup"><span data-stu-id="e99a5-124">This section provides information on common errors you may get while using the package manager to install .NET Core.</span></span>

### <a name="failed-to-fetch"></a><span data-ttu-id="e99a5-125">Getirilemedi</span><span class="sxs-lookup"><span data-stu-id="e99a5-125">Failed to fetch</span></span>

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]