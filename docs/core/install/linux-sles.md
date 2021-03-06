---
title: SLES-.NET Core 'a .NET Core 'u yükler
description: SLES 'e .NET Core SDK ve .NET Core çalışma zamanı yüklemenin çeşitli yollarını gösterir.
author: adegeo
ms.author: adegeo
ms.date: 06/04/2020
ms.openlocfilehash: 8f64efcc8206b47855871104e5b6914570c06da0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619422"
---
# <a name="install-net-core-sdk-or-net-core-runtime-on-sles"></a>SLES 'e .NET Core SDK veya .NET Core çalışma zamanı yüklemesi

.NET Core, SLES 'de desteklenir. Bu makalede, SLES 'de .NET Core 'un nasıl yükleneceği açıklanır.

[!INCLUDE [linux-intro-sdk-vs-runtime](includes/linux-intro-sdk-vs-runtime.md)]

## <a name="supported-distributions"></a>Desteklenen dağıtımlar

Aşağıdaki tabloda, hem SLES 12 SP2 hem de SLES 15 üzerinde şu anda desteklenen .NET Core sürümlerinin bir listesi verilmiştir. Bu sürümler, [.NET Core sürümü destek sonu](https://dotnet.microsoft.com/platform/support/policy/dotnet-core) veya SLES sürümü artık desteklenene kadar desteklenmeye devam eder.

- ✔️, SLES veya .NET Core sürümünün hala desteklendiğini gösterir.
- Bir ❌ , SLES veya .NET Core sürümünün bu SLES sürümünde desteklenmediğini belirtir.
- SLES ve .NET Core sürümlerinin her ikisi de ✔️ olduğunda, bu işletim sistemi ve .NET birleşimi desteklenir.

| SLES                   | .NET Core 2.1 | .NET Core 3,1 | .NET 5 Preview (yalnızca el ile yüklenir) |
|------------------------|---------------|---------------|----------------|
| ✔️ [15](#sles-15-)     | ✔️ 2,1        | ✔️ 3,1        | ✔️ 5,0 Preview |
| ✔️ [12 SP2](#sles-12-) | ✔️ 2,1        | ✔️ 3,1        | ✔️ 5,0 Preview |

Aşağıdaki .NET Core sürümleri artık desteklenmemektedir. Bunlara yönelik İndirilenler hala yayımlandı olarak kalmaya devam eder:

- 3.0
- 2,2
- 2.0

## <a name="how-to-install-other-versions"></a>Diğer sürümleri nasıl yüklenir

[!INCLUDE [package-manager-switcher](./includes/package-manager-heading-hack-pkgname.md)]

## <a name="sles-15-"></a>SLES 15 ✔️

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/15/packages-microsoft-prod.rpm
```

Şu anda, SLES 15 Microsoft Repository kurulum paketi *Microsoft-prod. repo* dosyasını yanlış dizine yüklüyor ve .NET Core paketlerini bulmadan zypper 'yi engellemektedir. Bu sorunu gidermek için doğru dizinde bir oluşturmaksızın oluşturun.

```bash
sudo ln -s /etc/yum.repos.d/microsoft-prod.repo /etc/zypp/repos.d/microsoft-prod.repo
```

[!INCLUDE [linux-zyp-install-31](includes/linux-install-31-zyp.md)]

## <a name="sles-12-"></a>SLES 12 ✔️

.NET Core, SLES 12 ailesi için en az SP2 gerektirir.

[!INCLUDE [linux-prep-intro-generic](includes/linux-prep-intro-generic.md)]

```bash
sudo rpm -Uvh https://packages.microsoft.com/config/sles/12/packages-microsoft-prod.rpm
```

[!INCLUDE [linux-zyp-install-31](includes/linux-install-31-zyp.md)]

## <a name="troubleshoot-the-package-manager"></a>Paket yöneticisinin sorunlarını giderme

Bu bölüm, .NET Core 'u yüklemek için Paket Yöneticisi 'ni kullanırken karşılaşabileceğiniz yaygın hatalarla ilgili bilgiler sağlar.

### <a name="failed-to-fetch"></a>Getirilemedi

[!INCLUDE [package-manager-failed-to-fetch-rpm](includes/package-manager-failed-to-fetch-rpm.md)]

## <a name="dependencies"></a>Bağımlılıklar

Bir paket yöneticisi ile yüklediğinizde, bu kitaplıklar sizin için yüklenir. Ancak, .NET Core 'u el ile yüklüyorsanız veya kendi kendine içerilen bir uygulama yayımlarsanız, bu kitaplıkların yüklü olduğundan emin olmanız gerekir:

- krb5
- libıu
- libopenssl1_1

Hedef çalışma zamanı ortamının OpenSSL sürümü 1,1 veya daha yeniyse, **COMPAT-openssl10**yüklemeniz gerekir.

Bağımlılıklar hakkında daha fazla bilgi için bkz. [kendi Içindeki Linux uygulamaları](https://github.com/dotnet/core/blob/master/Documentation/self-contained-linux-apps.md).

*System. Drawing. Common* derlemesini kullanan .NET Core uygulamaları için aşağıdaki bağımlılığa de ihtiyacınız olacaktır:

- [libgdiplus (sürüm 6.0.1 veya üzeri)](https://www.mono-project.com/docs/gui/libgdiplus/)

  > [!WARNING]
  > En son bir *libgdiplus* sürümünü sisteminize mono deposunu ekleyerek yükleyebilirsiniz. Daha fazla bilgi için bkz. <https://www.mono-project.com/download/stable/>.

## <a name="scripted-install"></a>Komut dosyalı yüklemesi

[!INCLUDE [linux-install-scripted](includes/linux-install-scripted.md)]

## <a name="manual-install"></a>El ile yüklemesi

[!INCLUDE [linux-install-manual](includes/linux-install-manual.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Öğretici: Visual Studio Code kullanarak .NET Core SDK bir konsol uygulaması oluşturma](../tutorials/with-visual-studio-code.md)
