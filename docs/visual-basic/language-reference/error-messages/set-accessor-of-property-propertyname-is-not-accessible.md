---
title: "'<propertyname>' özelliğinin 'Set' erişimcisine erişilemiyor"
ms.date: 07/20/2015
f1_keywords:
- vbc31102
- bc31102
helpviewer_keywords:
- BC31102
ms.assetid: 6f7b31b7-3656-4ae1-8851-90f5f4c6950a
ms.openlocfilehash: 077533a5b1fe241b61ded9516ad8f450d7dbbf5e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400350"
---
# <a name="set-accessor-of-property-propertyname-is-not-accessible"></a>'\<propertyname>' özelliğinin 'Set' erişimcisine erişilemiyor
Bir ifade, özelliğin yordamına erişimi olmayan bir özelliğin değerini depolamaya çalışır `Set` .  
  
 [Set deyimleri](../statements/set-statement.md) , [Property deyiminden](../statements/property-statement.md)daha kısıtlayıcı bir erişim düzeyiyle işaretlenmişse, aşağıdaki durumlarda özellik değerini ayarlama girişimi başarısız olabilir:  
  
- `Set`Ifade [Private](../modifiers/private.md) olarak işaretlenir ve çağıran kod, özelliğin tanımlandığı sınıfın veya yapının dışındadır.  
  
- `Set`Ifade [korumalı](../modifiers/protected.md) olarak işaretlenir ve çağıran kod, özelliğin tanımlandığı sınıf veya yapı içinde ya da türetilmiş bir sınıfta değil.  
  
- `Set`Ifade [arkadaş](../modifiers/friend.md) olarak işaretlenmiş ve çağıran kod, özelliğin tanımlandığı derlemede değil.  
  
 **Hata kimliği:** BC31102  
  
## <a name="to-correct-this-error"></a>Bu hatayı düzeltmek için  
  
- Özelliği tanımlayan kaynak kodun denetimine sahipseniz, `Set` yordamı özelliğin kendisiyle aynı erişim düzeyiyle bildirmeyi göz önünde bulundurun.  
  
- Özelliği tanımlayan kaynak kodu denetimine sahip değilseniz veya `Set` yordam erişim düzeyini özelliğin kendisinden daha fazla kısıtlamanız gerekiyorsa, özellik değerini ayarlayan ifadeyi özelliğe daha iyi erişimi olan bir kod bölgesine taşımayı deneyin.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Özellik Yordamları](../../programming-guide/language-features/procedures/property-procedures.md)
- [Nasıl yapılır: Bir Özelliği Karışık Erişim Düzeyleriyle Bildirme](../../programming-guide/language-features/procedures/how-to-declare-a-property-with-mixed-access-levels.md)
