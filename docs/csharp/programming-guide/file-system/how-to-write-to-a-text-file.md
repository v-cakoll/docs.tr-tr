---
title: Metin dosyasına nasıl yazılır - C# Programlama Kılavuzu
ms.date: 07/20/2015
f1_keywords:
- TextWriter.WriteLine
- StreamWriter.Close
helpviewer_keywords:
- files [C#], text files
- text, writing to files [C#]
ms.assetid: 2e99f184-d88b-4719-a7f1-d9ec482aa809
ms.openlocfilehash: ac16fb971eae5654a55e2f1753d78a6458f03315
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "75712253"
---
# <a name="how-to-write-to-a-text-file-c-programming-guide"></a>Metin dosyasına nasıl yazılır (C# Programlama Kılavuzu)
Bu örnekler bir metin dosyasına yazmanın çeşitli yollarını göstermektedir. İlk iki örnek, herhangi bir <xref:System.IO.File?displayProperty=nameWithType> metin dosyasına herhangi `IEnumerable<string>` bir ve bir dize her öğeyi yazmak için sınıfta statik kolaylık yöntemleri kullanın. Örnek 3, dosyaya yazarken her satırı ayrı ayrı işlemeniz gerektiğinde dosyaya nasıl metin ekleyeceğinizi gösterir. Örnekler 1-3 dosyadaki varolan tüm içeriğin üzerine yazılır, ancak örnek 4, varolan bir dosyaya metin ekini gösterir.  
  
 Bu örneklerin tümü dosyalara dize edebi yazılır. Bir dosyaya yazılan metni biçimlendirmek <xref:System.String.Format%2A> istiyorsanız, yöntem veya C# [dize enterpolasyon](../../language-reference/tokens/interpolated.md) özelliğini kullanın.  
  
## <a name="example"></a>Örnek  
 [!code-csharp[csFilesandFolders#3](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csFilesAndFolders/CS/FileIteration.cs#3)]  
  
## <a name="robust-programming"></a>Güçlü Programlama  
 Aşağıdaki koşullar özel bir duruma neden olabilir:  
  
- Dosya mevcut ve salt okunur.  
  
- Yol adı çok uzun olabilir.  
  
- Disk dolu olabilir.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [C# Programlama Kılavuzu](../index.md)
- [Dosya Sistemi ve Kayıt Defteri (C# Programlama Kılavuzu)](./index.md)
- [Örnek: Koleksiyonu Uygulama Depolamasına Kaydet](https://code.msdn.microsoft.com/CSWinStoreAppSaveCollection-bed5d6e6)
