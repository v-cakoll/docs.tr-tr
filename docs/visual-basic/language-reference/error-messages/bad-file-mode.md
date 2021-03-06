---
title: Hatalı dosya modu
ms.date: 07/20/2015
f1_keywords:
- vbrID54
ms.assetid: 74891e96-884b-4c8d-872d-cd11ae272372
ms.openlocfilehash: 534ea2d8316dc29cace798c5ad9b7697a290026f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409875"
---
# <a name="bad-file-mode"></a>Hatalı dosya modu
Dosya içeriğini düzenleme bölümünde kullanılan deyimler, dosyanın açıldığı moda uygun olmalıdır. Olası nedenler şunlardır:  
  
- `FilePutObject`Or `FileGetObject` deyimleri sıralı bir dosya belirtir.  
  
- Bir `Print` ifade, veya dışında bir erişim modu için açılan bir dosya `Output` belirtir `Append` .  
  
- Bir `Input` ifade, bir erişim modu için açılan bir dosyayı belirtir`Input`  
  
- Salt okunurdur bir dosyaya yazma girişimi.  
  
## <a name="to-correct-this-error"></a>Bu hatayı düzeltmek için  
  
- ' In `FilePutObject` `FileGetObject` yalnızca veya erişim için açık olan dosyalara başvuruda bulunduğundan emin olun `Random` `Binary` .  
  
- `Print`Ya da erişim modu için açılmış bir dosya belirttiğinden emin olun `Output` `Append` . Aksi takdirde, verileri dosyaya yerleştirmek için farklı bir ifade kullanın veya dosyayı uygun bir modda yeniden açın.  
  
- `Input`İçin açılmış bir dosya belirttiğinden emin olun `Input` . Aksi takdirde, verileri dosyaya yerleştirmek veya uygun modda dosyayı yeniden açmak için farklı bir ifade kullanın.  
  
- Salt okunurdur bir dosyaya yazıyorsanız dosyanın okuma/yazma durumunu değiştirin veya yazmaya çalışmayın.  
  
- Nesnesinde bulunan işlevleri kullanın `My.Computer.FileSystem` .  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:Microsoft.VisualBasic.FileSystem>
- [Sorun Giderme: Metin Dosyalarını Okuma ve Yazma](../../developing-apps/programming/drives-directories-files/troubleshooting-reading-from-and-writing-to-text-files.md)
