---
title: .NET framework teknolojilerini .NET Core üzerinde kullanılamaz
description: .NET Core üzerinde kullanılabilir olan .NET Framework teknolojileri hakkında bilgi edinin
author: cartermp
ms.author: mairaw
ms.date: 12/7/2018
ms.openlocfilehash: 8b43c15a942e0effab486e5399325bec746484a2
ms.sourcegitcommit: c6f69b0cf149f6b54483a6d5c2ece222913f43ce
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/08/2019
ms.locfileid: "55904907"
---
# <a name="net-framework-technologies-unavailable-on-net-core"></a><span data-ttu-id="4185a-103">.NET framework teknolojilerini .NET Core üzerinde kullanılamaz</span><span class="sxs-lookup"><span data-stu-id="4185a-103">.NET Framework technologies unavailable on .NET Core</span></span>

<span data-ttu-id="4185a-104">.NET Framework kitaplıkları için çeşitli teknolojilerden, uygulama etki alanları, uzaktan iletişim, kod erişim güvenliği (CAS) ve güvenlik gibi .NET Core ile kullanmak için kullanılamaz.</span><span class="sxs-lookup"><span data-stu-id="4185a-104">Several technologies available to .NET Framework libraries aren't available for use with .NET Core, such as AppDomains, Remoting, Code Access Security (CAS), and Security Transparency.</span></span> <span data-ttu-id="4185a-105">Kitaplıklarınızı bir veya daha fazla teknolojiler kullanıyorsa, aşağıda ana hatlarıyla belirtilen alternatif yaklaşımlar göz önünde bulundurun.</span><span class="sxs-lookup"><span data-stu-id="4185a-105">If your libraries rely on one or more of these technologies, consider the alternative approaches outlined below.</span></span> <span data-ttu-id="4185a-106">Corefx'te takım API uyumluluğu hakkında daha fazla bilgi için tutan bir [davranış değişiklikleri/compat sonu ve kullanım dışı/eski API'ler listesi](https://github.com/dotnet/corefx/wiki/ApiCompat) github'da.</span><span class="sxs-lookup"><span data-stu-id="4185a-106">For more information on API compatibility, the CoreFX team maintains a [List of behavioral changes/compat breaks and deprecated/legacy APIs](https://github.com/dotnet/corefx/wiki/ApiCompat) at GitHub.</span></span>

<span data-ttu-id="4185a-107">API veya teknoloji yalnızca şu anda uygulanmadı çünkü kasıtlı olarak desteklenmeyen var. kapsıyor değil.</span><span class="sxs-lookup"><span data-stu-id="4185a-107">Just because an API or technology isn't currently implemented doesn't imply it's intentionally unsupported.</span></span> <span data-ttu-id="4185a-108">GitHub depoları karşılaştığınız bir sorunu tasarıma, ancak böyle bir göstergesi bulamazsanız, lütfen dosyası, bir sorunu görmek .NET Core için ilk arayacağı [dotnet/corefx'te depo sorunları](https://github.com/dotnet/corefx/issues) istemek için github'da Özel API'ler ve teknolojileri için.</span><span class="sxs-lookup"><span data-stu-id="4185a-108">You should first search the GitHub repositories for .NET Core to see if a particular issue you encounter is by design, but if you cannot find such an indicator, please file an issue in the [dotnet/corefx repository issues](https://github.com/dotnet/corefx/issues) at GitHub to ask for specific APIs and technologies.</span></span> <span data-ttu-id="4185a-109">[Sorunları istekleri taşıma](https://github.com/dotnet/corefx/labels/port-to-core) ile işaretlenmiş `port-to-core` etiketi.</span><span class="sxs-lookup"><span data-stu-id="4185a-109">[Porting requests in the issues](https://github.com/dotnet/corefx/labels/port-to-core) are marked with the `port-to-core` label.</span></span>

## <a name="appdomains"></a><span data-ttu-id="4185a-110">AppDomains</span><span class="sxs-lookup"><span data-stu-id="4185a-110">AppDomains</span></span>

<span data-ttu-id="4185a-111">Uygulama etki alanları (uygulama etki alanları) uygulamaları birbirinden yalıtın.</span><span class="sxs-lookup"><span data-stu-id="4185a-111">Application domains (AppDomains) isolate apps from one another.</span></span> <span data-ttu-id="4185a-112">Uygulama etki alanları, çalışma zamanı desteği gerektirir ve genellikle oldukça pahalıdır.</span><span class="sxs-lookup"><span data-stu-id="4185a-112">AppDomains require runtime support and are generally quite expensive.</span></span> <span data-ttu-id="4185a-113">Ek uygulama etki alanları oluşturma desteklenmiyor...</span><span class="sxs-lookup"><span data-stu-id="4185a-113">Creating additional app domains is not supported..</span></span> <span data-ttu-id="4185a-114">Gelecekte bu özelliği eklemeyi planlıyoruz yok.</span><span class="sxs-lookup"><span data-stu-id="4185a-114">We don't plan on adding this capability in future.</span></span> <span data-ttu-id="4185a-115">Kod bir ayırma işlemi için ayrı işlemler öneririz veya alternatif olarak kapsayıcıları kullanma.</span><span class="sxs-lookup"><span data-stu-id="4185a-115">For code isolation, we recommend separate processes or using containers as an alternative.</span></span> <span data-ttu-id="4185a-116">Dinamik derlemeler yüklenmesi için yeni öneririz <xref:System.Runtime.Loader.AssemblyLoadContext> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4185a-116">For the dynamic loading of assemblies, we recommend the new <xref:System.Runtime.Loader.AssemblyLoadContext> class.</span></span>

<span data-ttu-id="4185a-117">.NET Framework'ten kod geçişi kolaylaştırmak için .NET Core bazı sunan <xref:System.AppDomain> API yüzeyi.</span><span class="sxs-lookup"><span data-stu-id="4185a-117">To make code migration from .NET Framework easier, .NET Core exposes some of the <xref:System.AppDomain> API surface.</span></span> <span data-ttu-id="4185a-118">Bazı API'leri işlev normal olarak (örneğin, <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType>), bazı üyeleri hiçbir şey yapma (örneğin, <xref:System.AppDomain.SetCachePath%2A>), ve bunlardan bazıları throw <xref:System.PlatformNotSupportedException> (örneğin, <xref:System.AppDomain.CreateDomain%2A>).</span><span class="sxs-lookup"><span data-stu-id="4185a-118">Some of the APIs function normally (for example, <xref:System.AppDomain.UnhandledException?displayProperty=nameWithType>), some members do nothing (for example, <xref:System.AppDomain.SetCachePath%2A>), and some of them throw <xref:System.PlatformNotSupportedException> (for example, <xref:System.AppDomain.CreateDomain%2A>).</span></span> <span data-ttu-id="4185a-119">Kullandığınız karşı türlerini işaretleyin [ `System.AppDomain` başvuru kaynağı](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.Extensions/src/System/AppDomain.cs) içinde [dotnet/corefx'te GitHub deposu](https://github.com/dotnet/corefx)ettiğinizden emin uygulanan sürümünüzle eşleşen dalı seçin.</span><span class="sxs-lookup"><span data-stu-id="4185a-119">Check the types you use against the [`System.AppDomain` reference source](https://github.com/dotnet/corefx/blob/master/src/System.Runtime.Extensions/src/System/AppDomain.cs) in the [dotnet/corefx GitHub repository](https://github.com/dotnet/corefx), making sure to select the branch that matches your implemented version.</span></span>

## <a name="remoting"></a><span data-ttu-id="4185a-120">Uzaktan iletişim</span><span class="sxs-lookup"><span data-stu-id="4185a-120">Remoting</span></span>

<span data-ttu-id="4185a-121">.NET uzaktan iletişim sorunlu bir mimari belirlenmiştir.</span><span class="sxs-lookup"><span data-stu-id="4185a-121">.NET Remoting was identified as a problematic architecture.</span></span> <span data-ttu-id="4185a-122">Bu, artık desteklenmeyen AppDomain arası iletişim için kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4185a-122">It's used for cross-AppDomain communication, which is no longer supported.</span></span> <span data-ttu-id="4185a-123">Ayrıca, uzaktan iletişimi sürdürmek pahalı olan çalışma zamanı desteği gerektirir.</span><span class="sxs-lookup"><span data-stu-id="4185a-123">Also, Remoting requires runtime support, which is expensive to maintain.</span></span> <span data-ttu-id="4185a-124">Bu nedenlerle, .NET Core, .NET uzaktan iletişim desteklenmez ve gelecekte destek eklemeyi planlıyoruz yok.</span><span class="sxs-lookup"><span data-stu-id="4185a-124">For these reasons, .NET Remoting isn't supported on .NET Core, and we don't plan on adding support for it in the future.</span></span>

<span data-ttu-id="4185a-125">İşlemler arasında iletişim için işlemler arası iletişim (IPC) mekanizmasını Remoting, alternatif olarak gibi göz önünde bulundurun <xref:System.IO.Pipes> veya <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> sınıfı.</span><span class="sxs-lookup"><span data-stu-id="4185a-125">For communication across processes, consider inter-process communication (IPC) mechanisms as an alternative to Remoting, such as the <xref:System.IO.Pipes> or the <xref:System.IO.MemoryMappedFiles.MemoryMappedFile> class.</span></span>

<span data-ttu-id="4185a-126">Makine arasında ağ tabanlı bir çözüm alternatif olarak kullanın.</span><span class="sxs-lookup"><span data-stu-id="4185a-126">Across machines, use a network-based solution as an alternative.</span></span> <span data-ttu-id="4185a-127">Tercihen, HTTP gibi bir düşük ek yük düz metin protokol kullanın.</span><span class="sxs-lookup"><span data-stu-id="4185a-127">Preferably, use a low-overhead plain text protocol, such as HTTP.</span></span> <span data-ttu-id="4185a-128">[Kestrel web sunucusu](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel), ASP.NET Core tarafından kullanılan web sunucusu, burada bir seçenektir.</span><span class="sxs-lookup"><span data-stu-id="4185a-128">The [Kestrel web server](https://docs.microsoft.com/aspnet/core/fundamentals/servers/kestrel), the web server used by ASP.NET Core, is an option here.</span></span> <span data-ttu-id="4185a-129">Ayrıca kullanmayı <xref:System.Net.Sockets> ağ tabanlı, makineler arası senaryolar için.</span><span class="sxs-lookup"><span data-stu-id="4185a-129">Also consider using <xref:System.Net.Sockets> for network-based, cross-machine scenarios.</span></span> <span data-ttu-id="4185a-130">Daha fazla seçenek için bkz: [.NET açık kaynak Geliştirici projeler: Mesajlaşma](https://github.com/Microsoft/dotnet/blob/master/dotnet-developer-projects.md#messaging).</span><span class="sxs-lookup"><span data-stu-id="4185a-130">For more options, see [.NET Open Source Developer Projects: Messaging](https://github.com/Microsoft/dotnet/blob/master/dotnet-developer-projects.md#messaging).</span></span>

## <a name="code-access-security-cas"></a><span data-ttu-id="4185a-131">Kod Erişimi Güvenliği (CAS)</span><span class="sxs-lookup"><span data-stu-id="4185a-131">Code Access Security (CAS)</span></span>

<span data-ttu-id="4185a-132">Çalışma zamanı veya bir yönetilen uygulama veya kitaplık kullanır veya çalışan hangi kaynaklara sınırlamak için framework kullanan, korumalı alana alma [.NET Framework üzerinde desteklenmiyor](~/docs/framework/misc/code-access-security.md) ve .NET Core, bu nedenle de desteklenmez.</span><span class="sxs-lookup"><span data-stu-id="4185a-132">Sandboxing, which relies on the runtime or the framework to constrain which resources a managed application or library uses or runs, [isn't supported on .NET Framework](~/docs/framework/misc/code-access-security.md) and therefore is also not supported on .NET Core.</span></span> <span data-ttu-id="4185a-133">CA güvenlik sınırı davranılması devam etmek için ayrıcalıkların oluştuğu .NET Framework ve çalışma zamanı içinde çok fazla durumlar vardır.</span><span class="sxs-lookup"><span data-stu-id="4185a-133">There are too many cases in the .NET Framework and the runtime where an elevation of privileges occurs to continue treating CAS as a security boundary.</span></span> <span data-ttu-id="4185a-134">Ayrıca, CA uygulaması daha karmaşık hale getirir ve genellikle doğruluk performans şifrelemelerini kullanmak istemediğiniz uygulamalar için.</span><span class="sxs-lookup"><span data-stu-id="4185a-134">In addition, CAS makes the implementation more complicated and often has correctness-performance implications for applications that don't intend to use it.</span></span>

<span data-ttu-id="4185a-135">En düşük ayrıcalık kümesi ile çalışan işlemleri için sanallaştırma, kapsayıcıları veya kullanıcı hesapları gibi işletim sistemi tarafından sağlanan güvenlik sınırları kullanın.</span><span class="sxs-lookup"><span data-stu-id="4185a-135">Use security boundaries provided by the operating system, such as virtualization, containers, or user accounts for running processes with the minimum set of privileges.</span></span>

## <a name="security-transparency"></a><span data-ttu-id="4185a-136">Güvenlik saydamlık</span><span class="sxs-lookup"><span data-stu-id="4185a-136">Security Transparency</span></span>

<span data-ttu-id="4185a-137">Benzer şekilde CA'ları, güvenlik saydamlık korumalı kod güvenlik kritik kod, bildirim temelli bir biçimde ayırır ancak olan [artık bir güvenlik sınırı olarak desteklenen](~/docs/framework/misc/security-transparent-code.md).</span><span class="sxs-lookup"><span data-stu-id="4185a-137">Similar to CAS, Security Transparency separates sandboxed code from security critical code in a declarative fashion but is [no longer supported as a security boundary](~/docs/framework/misc/security-transparent-code.md).</span></span> <span data-ttu-id="4185a-138">Bu özellik tarafından Silverlight yoğun olarak kullanılır.</span><span class="sxs-lookup"><span data-stu-id="4185a-138">This feature is heavily used by Silverlight.</span></span> 

<span data-ttu-id="4185a-139">En az çalışan işlemleri için sanallaştırma, kapsayıcıları veya kullanıcı hesapları gibi işletim sistemi tarafından sağlanan güvenlik sınırları kullanmanız ayrıcalık kümesi.</span><span class="sxs-lookup"><span data-stu-id="4185a-139">Use security boundaries provided by the operating system, such as virtualization, containers, or user accounts for running processes with the least set of privileges.</span></span>

>[!div class="step-by-step"]
>[<span data-ttu-id="4185a-140">Next</span><span class="sxs-lookup"><span data-stu-id="4185a-140">Next</span></span>](third-party-deps.md)