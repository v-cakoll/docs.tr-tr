---
title: "'typename' türündeki 'IsNot' işleneni, 'typename' boş değer atanabilir bir tür olduğundan, sadece 'Nothing' ile karşılaştırılabilir."
ms.date: 07/20/2015
f1_keywords:
- bc32128
- vbc32128
helpviewer_keywords:
- BC32128
ms.assetid: 1155b23a-ad75-4bab-b9da-73f35c767a36
ms.openlocfilehash: fb61b04021bd844fade94413b4f3b28b82f6411b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84402804"
---
# <a name="isnot-operand-of-type-typename-can-only-be-compared-to-nothing-because-typename-is-a-nullable-type"></a>'typename' türündeki 'IsNot' işleneni, 'typename' boş değer atanabilir bir tür olduğundan, sadece 'Nothing' ile karşılaştırılabilir.

Null yapılabilir değer türü olarak belirtilen bir değişken işleci kullanmaktan başka bir ifadeyle karşılaştırıldı `Nothing` `IsNot` .

**Hata kimliği:** BC32128

## <a name="to-correct-this-error"></a>Bu hatayı düzeltmek için

Null yapılabilir bir türü işlecini kullanarak dışındaki bir ifadeyle karşılaştırmak için `Nothing` `IsNot` , `GetType` Aşağıdaki örnekte gösterildiği gibi, null yapılabilir türde yöntemi çağırın ve sonucu ifadesiyle karşılaştırın.

```vb
Dim number? As Integer = 5

If number IsNot Nothing Then
  If number.GetType() IsNot Type.GetType("System.Int32") Then

  End If
End If
```

## <a name="see-also"></a>Ayrıca bkz.

- [Null yapılabilir değer türleri](../../programming-guide/language-features/data-types/nullable-value-types.md)
- [IsNot İşleci](../operators/isnot-operator.md)
