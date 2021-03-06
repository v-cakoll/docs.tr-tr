---
title: <list> -C# Programlama Kılavuzu
ms.date: 07/20/2015
f1_keywords:
- list
- <list>
helpviewer_keywords:
- list C# XML tag
- listheader C# XML tag
- <listheader> C# XML tag
- item C# XML tag
- <item> C# XML tag
- <list> C# XML tag
ms.assetid: c9620b1b-c2e6-43f1-ab88-8ab47308ffec
ms.openlocfilehash: 78eec992671dab1aa59717a007a8e3a2662f6e87
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287343"
---
# <a name="list-c-programming-guide"></a>\<list>(C# Programlama Kılavuzu)

## <a name="syntax"></a>Söz dizimi

```xml
<list type="bullet|number|table">
    <listheader>
        <term>term</term>
        <description>description</description>
    </listheader>
    <item>
        <term>term</term>
        <description>description</description>
    </item>
</list>
```

## <a name="parameters"></a>Parametreler

- `term`

  ' De tanımlanacak bir terim `description` .

- `description`

  Bir madde işareti veya numaralandırılmış listedeki bir öğe ya da bir tanımı `term` .
  
## <a name="remarks"></a>Açıklamalar

`<listheader>`Blok, bir tablo ya da tanım listesinin başlık satırını tanımlamak için kullanılır. Bir tablo tanımlarken, yalnızca başlıktaki terim için bir giriş sağlamanız gerekir.

Listedeki her öğe bir `<item>` blokla belirtilir. Bir tanım listesi oluştururken, hem hem de belirtmeniz gerekecektir `term` `description` . Ancak, bir tablo, madde işaretli liste veya numaralandırılmış liste için yalnızca bir giriş sağlamanız gerekir `description` .

Bir liste veya tablo gerektiği kadar çok `<item>` blok içerebilir.

Belge açıklamalarını bir dosyaya işlemek için [-doc](../../language-reference/compiler-options/doc-compiler-option.md) ile derleyin.

## <a name="example"></a>Örnek

[!code-csharp[csProgGuideDocComments#6](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideDocComments/CS/DocComments.cs#6)]

## <a name="see-also"></a>Ayrıca bkz.

- [C# programlama kılavuzu](../index.md)
- [Belge açıklamaları için önerilen etiketler](./recommended-tags-for-documentation-comments.md)
