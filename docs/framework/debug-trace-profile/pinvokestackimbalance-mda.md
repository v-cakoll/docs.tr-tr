---
title: pInvokeStackImbalance MDA
description: Bir platform çağırma çağrısını yaparken veya bu işlem sırasında bir erişim ihlali veya bellek bozulması sırasında etkinleştirilebilen PInvokeStackImbalance MDA öğesini gözden geçirin.
ms.date: 03/30/2017
helpviewer_keywords:
- signatures, platform invoke
- stack depth
- platform invoke stack imbalance
- MDAs (managed debugging assistants), platform invoke
- platform invoke, run-time errors
- PInvokeStackImbalance MDA
- managed debugging assistants (MDAs), platform invoke
ms.assetid: 34ddc6bd-1675-4f35-86aa-de1645d5c631
ms.openlocfilehash: 89afd3fce3f2a8bffe88d45991ceeb59fc5e5b76
ms.sourcegitcommit: c23d9666ec75b91741da43ee3d91c317d68c7327
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85803670"
---
# <a name="pinvokestackimbalance-mda"></a>PInvokeStackImbalance MDA

`PInvokeStackImbalance`Yönetilen hata ayıklama Yardımcısı (MDA), clr çağırma çağrısından sonra yığın derinliğini algıladığında, özniteliğinde belirtilen çağırma kuralı <xref:System.Runtime.InteropServices.DllImportAttribute> ve yönetilen İmzadaki parametrelerin bildirimi verildiğinde, beklenen yığın derinliğiyle eşleşmez.

`PInvokeStackImbalance`MDA yalnızca 32 bit x86 platformları için uygulanır.

> [!NOTE]
> `PInvokeStackImbalance`MDA, varsayılan olarak devre dışıdır. Visual Studio 2017 ve sonraki sürümlerinde `PInvokeStackImbalance` MDA, **özel durum ayarları** iletişim kutusundaki **yönetilen hata ayıklama yardımcıları** listesinde görüntülenir ( **Debug**  >  **Windows**  >  **özel durum ayarlarını**hata ayıkla ' yı seçtiğinizde görüntülenir). Ancak, **oluşturulduğunda kesmeyi** seçme veya temizleme onay kutusu, MDA ' ı etkinleştirmez veya devre dışı bırakır. Bu yalnızca, MDA etkinleştirildiğinde Visual Studio 'Nun bir özel durum oluşturulup oluşturulmayacağını denetler.

## <a name="symptoms"></a>Belirtiler

Bir uygulama, bir platform çağırma çağrısını yaparken veya takip edildiğinde bir erişim ihlali veya bellek bozulması ile karşılaşır.

## <a name="cause"></a>Nedeni

Platform çağırma çağrısının yönetilen imzası, çağrılan metodun yönetilmeyen imzasıyla eşleşmeyebilir.  Bu uyuşmazlığın nedeni, yönetilen imzanın doğru parametre sayısını bildirmemesi veya parametreler için uygun boyutu belirtmemesidir.  Ayrıca, özniteliği tarafından belirtilen çağırma kuralı <xref:System.Runtime.InteropServices.DllImportAttribute> yönetilmeyen çağrı kuralıyla eşleşmediğinden, MDA da etkinleştirilebilir.

## <a name="resolution"></a>Çözüm

Yönetilen platform çağırma imzasını ve çağırma kuralını inceleyerek yerel hedefin imza ve çağırma kuralına uyduğundan emin olun.  Hem yönetilen hem de yönetilmeyen tarafta çağırma kuralını açıkça belirtmeyi deneyin. Yönetilmeyen işlevin, yönetilmeyen derleyicide oluşan bir hata gibi başka bir nedenle yığına dengesiz olması mümkün olmasa da mümkündür.

## <a name="effect-on-the-runtime"></a>Çalışma zamanında etki

Tüm platform çağırma çağrılarını CLR 'de iyileştirilmemiş olmayan yolu alacak şekilde zorlar.

## <a name="output"></a>Çıktı

MDA iletisi, yığın dengesizlenmesi için yol açan platform çağırma yöntemi çağrısının adını verir. Bir platform çağırma çağrısında bir örnek ileti `SampleMethod` :

**PInvoke işlevi ' SampleMethod ' çağrısı yığına dengesiz. Bu, yönetilen PInvoke imzasının yönetilmeyen hedef imzasıyla eşleşmemesi nedeniyle olasıdır. PInvoke imzasının çağırma kuralı ve parametrelerinin hedef yönetilmeyen imzayla eşleşip eşleştiğinden emin olun.**

## <a name="configuration"></a>Yapılandırma

```xml
<mdaConfig>
  <assistants>
    <pInvokeStackImbalance />
  </assistants>
</mdaConfig>
```

## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Runtime.InteropServices.MarshalAsAttribute>
- [Yönetilen Hata Ayıklama Yardımcıları ile Hataları Tanılama](diagnosing-errors-with-managed-debugging-assistants.md)
- [Birlikte Çalışma Hazırlama](../interop/interop-marshaling.md)
