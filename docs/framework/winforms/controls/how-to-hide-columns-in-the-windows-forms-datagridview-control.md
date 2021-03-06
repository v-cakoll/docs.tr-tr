---
title: DataGridView Denetimindeki sütunları gizleme
description: DataGridViewColumn. Visible özelliğini false olarak ayarlayarak Windows Forms DataGridView Denetimindeki sütunları programlama yoluyla gizlemeyi öğrenin.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- DataGridView control [Windows Forms], hiding columns
- data grids [Windows Forms], hiding columns
- columns [Windows Forms], hiding
ms.assetid: 3f94143a-2ef0-49a5-a22a-b2e6f9289642
ms.openlocfilehash: 27e9f331151acd68d76233bc7dbb09c2d870afde
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85618057"
---
# <a name="how-to-hide-columns-in-the-windows-forms-datagridview-control"></a>Nasıl yapılır: Windows Forms DataGridView Denetiminde Sütunları Gizleme
Bazen Windows Forms denetiminde kullanılabilir olan sütunlardan yalnızca bazılarını göstermek isteyeceksiniz <xref:System.Windows.Forms.DataGridView> . Örneğin, yönetim kimlik bilgilerine sahip kullanıcılara bir çalışan maaş sütununu diğer kullanıcılardan gizleyerek göstermek isteyebilirsiniz. Alternatif olarak, denetimi çok sayıda sütun içeren bir veri kaynağına ve yalnızca bir kısmını göstermek isteyebilirsiniz. Bu durumda, genellikle, görüntülemeden İlgilendiğiniz sütunları gizlemeniz yerine kaldıracaksınız.  
  
 <xref:System.Windows.Forms.DataGridView>Denetimde, <xref:System.Windows.Forms.DataGridViewColumn.Visible%2A> sütunun özellik değeri sütunun görüntülenip görüntülenmediğini belirler.  
  
 Visual Studio 'da bu görev için destek vardır.  Ayrıca bkz. [nasıl yapılır: Tasarımcıyı kullanarak Windows Forms DataGridView Denetimindeki sütunları gizleme](hide-columns-in-the-datagrid-using-the-designer.md).  
  
### <a name="to-hide-a-column-programmatically"></a>Bir sütunu programlı bir şekilde gizlemek için  
  
- <xref:System.Windows.Forms.DataGridViewColumn.Visible%2A?displayProperty=nameWithType>Özelliğini olarak ayarlayın `false` . `CustomerID`Veri bağlama sırasında otomatik olarak oluşturulan bir sütunu gizlemek için aşağıdaki kod örneğini bir <xref:System.Windows.Forms.DataGridView.DataBindingComplete> olay işleyicisine yerleştirin.  
  
     [!code-csharp[System.Windows.Forms.DataGridViewMisc#063](~/samples/snippets/csharp/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/CS/datagridviewmisc.cs#063)]
     [!code-vb[System.Windows.Forms.DataGridViewMisc#063](~/samples/snippets/visualbasic/VS_Snippets_Winforms/System.Windows.Forms.DataGridViewMisc/VB/datagridviewmisc.vb#063)]  
  
## <a name="compiling-the-code"></a>Kod Derleniyor  
 Bu örnek şunları gerektirir:  
  
- Adında <xref:System.Windows.Forms.DataGridView> `dataGridView1` bir sütun içeren adlı bir denetim `CustomerID` .  
  
- <xref:System?displayProperty=nameWithType>Ve <xref:System.Windows.Forms?displayProperty=nameWithType> derlemelerine başvurular.  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Windows.Forms.DataGridView>
- <xref:System.Windows.Forms.DataGridViewColumn.Visible%2A?displayProperty=nameWithType>
- [Windows Forms DataGridView Denetimindeki Temel Sütun, Satır ve Hücre Özellikleri](basic-column-row-and-cell-features-wf-datagridview-control.md)
- [Nasıl yapılır: Otomatik Oluşturulan Sütunları Windows Forms DataGridView Denetiminden Kaldırma](remove-autogenerated-columns-from-a-wf-datagridview-control.md)
- [Nasıl yapılır: Windows Forms DataGridView Denetiminde Sütunların Sırasını Değiştirme](how-to-change-the-order-of-columns-in-the-windows-forms-datagridview-control.md)
