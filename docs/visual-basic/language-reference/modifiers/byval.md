---
title: ByVal
ms.date: 07/20/2015
f1_keywords:
- vb.ByVal
- ByVal
helpviewer_keywords:
- ByVal keyword [Visual Basic], contexts
- ByVal keyword [Visual Basic]
ms.assetid: 1eaf4e58-b305-4785-9e3d-e416b9c75598
ms.openlocfilehash: c4cdafafa6bae3246c0512e28f94fde7e88d230b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84373030"
---
# <a name="byval-visual-basic"></a>ByVal (Visual Basic)
Çağrılan yordamın veya özelliğin, çağıran koddaki bağımsız değişkeni temel alan bir değişkenin değerini değiştirememesi için bir bağımsız değişkenin [değere göre](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)geçtiğini belirtir. Değiştirici belirtilmemişse, ByVal varsayılandır.

> [!NOTE]
> Varsayılan olduğu için, `ByVal` anahtar sözcüğünü Yöntem imzalarında açıkça belirtmeniz gerekmez. Gürültülü kod üretme eğilimi gösterir ve genellikle varsayılan olmayan `ByRef` anahtar kelimeye daha fazla bakmakta olur.

## <a name="remarks"></a>Açıklamalar
 `ByVal`Değiştirici şu bağlamlarda kullanılabilir:

 [Declare Deyimi](../statements/declare-statement.md)

 [Function Deyimi](../statements/function-statement.md)
  
 [Operator Deyimi](../statements/operator-statement.md)
  
 [Property Deyimi](../statements/property-statement.md)
  
 [Sub Deyimi](../statements/sub-statement.md)

## <a name="example"></a>Örnek
 Aşağıdaki örnek, `ByVal` bir başvuru türü bağımsız değişkeniyle parametre geçirme mekanizmasının kullanımını gösterir. Örnekte, bağımsız değişkeni `c1` , sınıfının bir örneğidir `Class1` . `ByVal`yordamlardaki kodun başvuru bağımsız değişkeninin temel alınan değerini değiştirmesini önler, `c1` ancak erişilebilir alanları ve özellikleri korumaz `c1` .

 [!code-vb[VbVbalrKeywords#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrKeywords/VB/Class5.vb#10)]

## <a name="see-also"></a>Ayrıca bkz.

- [Anahtar sözcükler](../keywords/index.md)
- [Bağımsız Değişkenleri Değere ve Başvuruya Göre Geçirme](../../programming-guide/language-features/procedures/passing-arguments-by-value-and-by-reference.md)
