---
title: 'Nasıl yapılır: Yazı Tipi Aileleri ve Yazı Tipleri Oluşturma'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- font families [Windows Forms], constructing
- fonts [Windows Forms], constructing
ms.assetid: d3a4a223-9492-4b54-9afd-db1c31c3cefd
ms.openlocfilehash: 2d609525858c7a8ff77c0b86900b4fc7d6b4e39a
ms.sourcegitcommit: b1cfd260928d464d91e20121f9bdba7611c94d71
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2019
ms.locfileid: "67505949"
---
# <a name="how-to-construct-font-families-and-fonts"></a>Nasıl yapılır: Yazı Tipi Aileleri ve Yazı Tipleri Oluşturma
GDI +'da yazı tipleri ile aynı yazı tipi ancak farklı stillerde yazı tipi aileleri gruplandırır. Örneğin, aşağıdaki yazı tipleri Arial yazı tipi ailesini içerir:  
  
- Arial normal  
  
- Arial kalın  
  
- Arial İtalik  
  
- Arial Kalın İtalik  
  
 GDI +'da form aileleri için dört stilleri kullanır: normal, kalın, italik ve kalın italik. Sıfat gibi *daraltmak* ve *yuvarlatılmış* stilleri; dikkate alınmaz aile adı bir parçası olduklarından değil. Örneğin, dar Arial yazı tipi ailesi aşağıdaki üyeleri ile şöyledir:  
  
- Arial dar normal  
  
- Arial dar kalın  
  
- Arial dar italik  
  
- Dar Arial Kalın İtalik  
  
 GDI + ile metin çizebilirsiniz önce oluşturmak ihtiyacınız bir <xref:System.Drawing.FontFamily> nesnesi ve bir <xref:System.Drawing.Font> nesne. <xref:System.Drawing.FontFamily> Nesnesini belirtir (örneğin, Arial), yazı tipi ve <xref:System.Drawing.Font> boyutu, stil ve birimler nesnesini belirtir.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki örnek, bir Normal stili Arial yazı tipi boyutu 16 piksel olarak oluşturur. Aşağıdaki kodda, ilk bağımsız değişkenin geçirilecek <xref:System.Drawing.Font.%23ctor%2A> oluşturucudur <xref:System.Drawing.FontFamily> nesne. İkinci bağımsız değişkeni Dördüncü bağımsız değişkeni tarafından tanımlanan birimleri cinsinden ölçülen yazı tipi boyutunu belirtir. Üçüncü bağımsız değişken stil tanımlar.  
  
 <xref:System.Drawing.GraphicsUnit.Pixel> bir üyesidir <xref:System.Drawing.GraphicsUnit> numaralandırma ve <xref:System.Drawing.FontStyle.Regular> üyesi <xref:System.Drawing.FontStyle> sabit listesi.  
  
 [!code-csharp[System.Drawing.FontsAndText#61](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Drawing.FontsAndText/CS/Class1.cs#61)]
 [!code-vb[System.Drawing.FontsAndText#61](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Drawing.FontsAndText/VB/Class1.vb#61)]  
  
## <a name="compiling-the-code"></a>Kod Derleniyor  
 Yukarıdaki örnekte, Windows Forms ile kullanılmak üzere tasarlanmıştır ve gerektirir <xref:System.Windows.Forms.PaintEventArgs> `e`, parametre olduğu <xref:System.Windows.Forms.PaintEventHandler>.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Yazı Tipleri ve Metin Kullanma](using-fonts-and-text.md)
- [Windows Forms’da Grafikler ve Çizim](graphics-and-drawing-in-windows-forms.md)
