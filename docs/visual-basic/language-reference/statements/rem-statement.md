---
title: REM Deyimi
ms.date: 07/20/2015
f1_keywords:
- vb.'
- vb.Rem
- Rem
- "'"
helpviewer_keywords:
- REM statement [Visual Basic]
- comments, Visual Basic code
- code comments, Visual Basic
- Visual Basic code, comments
- "' comment marker character [Visual Basic]"
ms.assetid: 34126d7f-e0f9-476d-91e6-b31b398615dc
ms.openlocfilehash: 68c898145bd8845c657b6ebb8776a3a9027c359c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404271"
---
# <a name="rem-statement-visual-basic"></a>REM Deyimi (Visual Basic)
Bir programın kaynak kodunda açıklayıcı açıklamalar eklemek için kullanılır.  
  
## <a name="syntax"></a>Sözdizimi  
  
```vb  
REM comment  
' comment  
```  
  
## <a name="parts"></a>Bölümler  
 `comment`  
 İsteğe bağlı. Dahil etmek istediğiniz açıklamanın metni. Anahtar sözcüğü ve arasında bir boşluk gereklidir `REM` `comment` .  
  
## <a name="remarks"></a>Açıklamalar  
 Bir `REM` ifadeyi tek başına bir satıra koyabilirsiniz ya da başka bir deyimden sonraki bir satıra koyabilirsiniz. `REM`Deyimin satırdaki son ifade olması gerekir. Başka bir bildirime uygunsa, `REM` Bu deyimden bir boşluk ile ayrılması gerekir.  
  
 Yerine tek tırnak işareti ( `'` ) kullanabilirsiniz `REM` . Bu, yorumunuz aynı satırdaki başka bir deyime veya bir satırda tek başına oturduğunda geçerlidir.  
  
> [!NOTE]
> Bir `REM` satır devamlılık sırası () kullanarak bir deyime devam edemezsiniz `_` . Bir yorum başladıktan sonra derleyici, özel anlam için karakterleri denetlemez. Birden çok satırlık bir açıklama için, `REM` her satırda başka bir ifade veya bir açıklama simgesi ( `'` ) kullanın.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki örnek, `REM` bir programa açıklayıcı açıklamalar eklemek için kullanılan ifadesini gösterir. Ayrıca, yerine tek tırnak işareti karakteri () kullanmanın alternatifini de gösterir `'` `REM` .  
  
 [!code-vb[VbVbalrStatements#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#6)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Kod Açıklamaları](../../programming-guide/program-structure/comments-in-code.md)
- [Nasıl yapılır: Kodda Deyimleri Bölme ve Birleştirme](../../programming-guide/program-structure/how-to-break-and-combine-statements-in-code.md)
