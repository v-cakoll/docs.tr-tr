---
title: Tanımlayıcı adlandırılmış derlemeler
description: Derleme için benzersiz bir kimlik oluşturan ve derleme çakışmalarını engelleyebilen .NET derlemeleri için tanımlayıcı adlar hakkında bilgi edinin.
ms.date: 08/20/2019
helpviewer_keywords:
- strong-named assemblies, about strong-named assemblies
- assemblies [.NET Framework], strong-named
ms.assetid: d4a80263-f3e0-4d81-9b61-f0cbeae3797b
ms.openlocfilehash: 069e42af6f8d49363d0264c0f6167b4afa3acd61
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925663"
---
# <a name="strong-named-assemblies"></a>Tanımlayıcı adlandırılmış derlemeler

Derlemeyi güçlü adlandırma derleme için benzersiz bir kimlik oluşturur ve derleme çakışmalarını önleyebilir.

## <a name="what-makes-a-strong-named-assembly"></a>Tanımlayıcı adlı bütünleştirilmiş kod ne olur?

Tanımlayıcı adlı bütünleştirilmiş kod, derleme ile dağıtılan ortak anahtara karşılık gelen özel anahtar kullanılarak oluşturulur ve derlemenin kendisi olur. Bütünleştirilmiş kod, derlemeyi oluşturan tüm dosyaların adlarını ve karmalarını içeren derleme bildirimini içerir. Aynı tanımlayıcı ada sahip derlemeler özdeş olmalıdır.

Visual Studio 'Yu veya bir komut satırı aracını kullanarak derlemeleri kesin olarak ad olarak belirleyebilirsiniz. Daha fazla bilgi için bkz. [nasıl yapılır: bütünleştirilmiş kodu bir tanımlayıcı ad](sign-strong-name.md) veya [Sn.exe Imzalama (tanımlayıcı ad aracı)](../../framework/tools/sn-exe-strong-name-tool.md).

Tanımlayıcı adlı bir derleme oluşturulduğunda, derleme basit metin adını, sürüm numarasını, isteğe bağlı kültür bilgilerini, dijital imzayı ve imza için kullanılan özel anahtara karşılık gelen ortak anahtarı içerir.

> [!WARNING]
> Güvenlik için tanımlayıcı adlara güvenmeyin. Yalnızca benzersiz bir kimlik sağlarlar.

## <a name="why-strong-name-your-assemblies"></a>Derlemelerinize neden güçlü ad adı veriyor?

.NET Framework için, tanımlayıcı adlı derlemeler aşağıdaki senaryolarda faydalıdır:

- Derlemelerinize tanımlayıcı adlı derlemeler tarafından başvurulmak üzere veya `friend` tanımlayıcı adlı diğer derlemelerden derlemelerinize erişim vermek isteyebilirsiniz.

- Uygulamanın aynı derlemenin farklı sürümlerine erişmesi gerekir. Bu, çakışma olmadan aynı uygulama etki alanında yan yana yüklemek üzere bir derlemenin farklı sürümlerinin olması gerektiği anlamına gelir. Örneğin, aynı basit ada sahip derlemelerde bir API 'nin farklı uzantıları varsa, güçlü adlandırma derlemenin her sürümü için benzersiz bir kimlik sağlar.

- Derlemenizi kullanarak uygulamaların performansını olumsuz şekilde etkilememesini istemezsiniz, bu nedenle derlemenin etki alanı nötr olmasını isteyebilirsiniz. Bu, genel derleme önbelleğine bir etki alanı nötr derleme yüklenmesi gerektiğinden, güçlü adlandırma gerektirir.

- Yayımcı ilkesi uygulayarak uygulamanızın bakımını merkezileştirmek istiyorsunuz, bu, derlemenin genel derleme önbelleğinde yüklü olması gerektiği anlamına gelir.

.NET Core için, tanımlayıcı adlı derlemeler malzeme avantajları sağlamaz.

Açık kaynaklı bir geliştiricisiyseniz ve tanımlayıcı adlı bir derlemenin kimlik avantajlarından .NET Framework daha iyi uyumluluk sağlamak istiyorsanız, kaynak denetim sisteminize bir derlemeyle ilişkili özel anahtarı iade etmeyi göz önünde bulundurun.

## <a name="see-also"></a>Ayrıca bkz.

- [Genel derleme önbelleği](../../framework/app-domains/gac.md)
- [Nasıl yapılır: bir derlemeyi güçlü bir adla Imzalama](sign-strong-name.md)
- [Sn.exe (tanımlayıcı ad aracı)](../../framework/tools/sn-exe-strong-name-tool.md)
- [Tanımlayıcı adlı derlemeler oluşturma ve kullanma](create-use-strong-named.md)
