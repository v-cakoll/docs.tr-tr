---
title: 'Nasıl yapılır: Bir Uygulama için ToolStrip Oluşturucusunu Ayarlama'
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Renderer property [Windows Forms]
- ToolStripProfessionalRenderer class [Windows Forms]
- ToolStrip control [Windows Forms]
- MenuStrip control [Windows Forms]
- toolbars [Windows Forms], customizing
ms.assetid: 46acef3e-9844-4ae8-9a2e-3006fe99cadf
ms.openlocfilehash: c9250b723e68ac97d1486a896392bf64c66cdfbc
ms.sourcegitcommit: c7a7e1468bf0fa7f7065de951d60dfc8d5ba89f5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/14/2019
ms.locfileid: "65591599"
---
# <a name="how-to-set-the-toolstrip-renderer-for-an-application"></a>Nasıl yapılır: Bir Uygulama için ToolStrip Oluşturucusunu Ayarlama
Görünümünü özelleştirebilirsiniz, <xref:System.Windows.Forms.ToolStrip> denetimleri tek tek veya tüm <xref:System.Windows.Forms.ToolStrip> denetimleri, uygulamanızda.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki kod örneği için özel Oluşturucu seçmeli olarak uygulamak gösterilmiştir bir <xref:System.Windows.Forms.ToolStrip> denetimi ve bir <xref:System.Windows.Forms.MenuStrip> denetimi.  
  
 Bu kod örneği kullanmak için derlemek ve uygulamayı çalıştırın ve ardından gelen özel Perforator kapsamını seçin <xref:System.Windows.Forms.ComboBox> denetimi. Tıklayın **Uygula** Oluşturucu ayarlamak için.  
  
 Özel menü öğesi işleme görmek için seçin <xref:System.Windows.Forms.MenuStrip> seçeneğini <xref:System.Windows.Forms.ComboBox> tıklayın, Denetim **Uygula**ve ardından açın **dosya** menü öğesi.  
  
 [!code-csharp[System.Windows.Forms.ToolStrip.Misc#1](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.Misc/CS/Program.cs#1)]
 [!code-vb[System.Windows.Forms.ToolStrip.Misc#1](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.Misc/VB/Program.vb#1)]  
[!code-csharp[System.Windows.Forms.ToolStrip.Misc#70](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.Misc/CS/Program.cs#70)]
[!code-vb[System.Windows.Forms.ToolStrip.Misc#70](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.ToolStrip.Misc/VB/Program.vb#70)]  
  
 Ayarlama <xref:System.Windows.Forms.ToolStripManager.Renderer%2A?displayProperty=nameWithType> özel Oluşturucu tümüne uygulamak için özellik <xref:System.Windows.Forms.ToolStrip> denetimleri, uygulamanızda.  
  
 Ayarlama <xref:System.Windows.Forms.ToolStrip.Renderer%2A?displayProperty=nameWithType> özel Oluşturucu için bireysel uygulamak için özellik <xref:System.Windows.Forms.ToolStrip> denetimi.  
  
## <a name="compiling-the-code"></a>Kod Derleniyor  
 Bu örnek gerektirir:  
  
- System.Windows.Forms System.Design ve System.Drawing derlemelerine başvurular.  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Windows.Forms.ToolStripManager>
- <xref:System.Windows.Forms.MenuStrip>
- <xref:System.Windows.Forms.ToolStrip>
- <xref:System.Windows.Forms.ToolStripProfessionalRenderer>
- [ToolStrip Denetimi](toolstrip-control-windows-forms.md)
