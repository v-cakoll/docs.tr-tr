---
title: "'type1' türünün değeri 'type2' olarak dönüştürülemez"
ms.date: 07/20/2015
f1_keywords:
- vbc31194
- bc31194
helpviewer_keywords:
- BC31194
ms.assetid: 03d50c31-addd-4c90-9c53-725b84f9782e
ms.openlocfilehash: f19f157bd4c76f481aa3232bc33c2a0c6ac21367
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84400285"
---
# <a name="value-of-type-type1-cannot-be-converted-to-type2"></a>'type1' türünün değeri 'type2' olarak dönüştürülemez
' Type1 ' türünün değeri ' type2 ' olarak dönüştürülemez. ' ' Öğesinin ilk öğesinin dize değerini almak için ' Value ' özelliğini kullanabilirsiniz \<parentElement> .  
  
 Bir XML sabit değerini örtük olarak belirli bir türe atama girişiminde bulunuldu. XML sabit değeri örtük olarak belirtilen türe atanamaz.  
  
 **Hata kimliği:** BC31194  
  
## <a name="to-correct-this-error"></a>Bu hatayı düzeltmek için  
  
- `Value`Değeri olarak başvurmak IÇIN XML sabit değerinin özelliğini kullanın `String` . `CType` <xref:System.Convert> Değeri belirtilen tür olarak atamak için işlevi, başka bir tür dönüştürme işlevini veya sınıfını kullanın.  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Convert>
- [Tür Dönüştürme İşlevleri](../functions/type-conversion-functions.md)
- [XML Değişmez Değerleri](../xml-literals/index.md)
- [XML](../../programming-guide/language-features/xml/index.md)
