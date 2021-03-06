---
title: 'Nasıl yapılır: BMP Resmini PNG Resmine Dönüştürme'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- BMP images [Windows Forms], converting to PNG
- image formats [Windows Forms], converting between
ms.assetid: 9d4a692d-73ac-4ce3-9e05-9ec321e8fbd6
ms.openlocfilehash: 59200941aa0f872a0bd99510bfaa8a8b878a9061
ms.sourcegitcommit: 2701302a99cafbe0d86d53d540eb0fa7e9b46b36
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64648297"
---
# <a name="how-to-convert-a-bmp-image-to-a-png-image"></a>Nasıl yapılır: BMP Resmini PNG Resmine Dönüştürme
Bunun aktardığınızda genellikle, bir görüntü dosyası biçiminden dönüştürme yapmak isteyeceksiniz. Çağırarak bu dönüştürme kolayca yapabilirsiniz <xref:System.Drawing.Image.Save%2A> yöntemi <xref:System.Drawing.Image> sınıfı ve belirterek <xref:System.Drawing.Imaging.ImageFormat> için istenen resim dosyası biçimi.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki örnek, bir türden bir BMP görüntüsünü yükler ve görüntü PNG biçiminde kaydeder.  
  
 [!code-csharp[UsingImageEncodersDecoders#4](~/samples/snippets/csharp/VS_Snippets_Winforms/UsingImageEncodersDecoders/CS/Form1.cs#4)]
 [!code-vb[UsingImageEncodersDecoders#4](~/samples/snippets/visualbasic/VS_Snippets_Winforms/UsingImageEncodersDecoders/VB/Form1.vb#4)]  
  
## <a name="compiling-the-code"></a>Kod Derleniyor  
 Bu örnek gerektirir:  
  
- Bir Windows Forms uygulaması.  
  
- Bir başvuru `System.Drawing.Imaging` ad alanı.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Nasıl yapılır: Yüklenen Kodlayıcıları listeleme](how-to-list-installed-encoders.md)
- [Yönetilen GDI+'da Görüntü Kodlayıcıları ve Kod Çözücüleri Kullanma](using-image-encoders-and-decoders-in-managed-gdi.md)
- [Bit Eşlem Türleri](types-of-bitmaps.md)
