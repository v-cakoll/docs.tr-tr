---
title: Nesneleri Katmanlama
description: Daha karmaşık kullanıcı arabirimleri oluşturmak için Windows Forms Denetimlerinde ve alt formlarda nesneleri nasıl katmanleyeceğinizi öğrenin.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms controls, layering
- controls [Windows Forms], layering
- z order
- controls [Windows Forms], positioning
- z-order
ms.assetid: 1acc4281-2976-4715-86f4-bda68134baaf
author: jillre
ms.author: jillfra
manager: jillfra
ms.openlocfilehash: 6269b09c56963fefd500b9e1e6c9d7f51f9619cf
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174516"
---
# <a name="how-to-layer-objects-on-windows-forms"></a>Nasıl yapılır: Windows Forms üzerinde katman nesneleri

Karmaşık bir kullanıcı arabirimi oluşturduğunuzda veya birden çok belge arabirimi (MDI) formuyla çalışıyorsanız, genellikle daha karmaşık kullanıcı arabirimleri (UI) oluşturmak için hem denetimleri hem de alt formları katman halinde katman yapmak isteyeceksiniz. Bir grup bağlamı içinde denetimleri ve pencereleri taşımak ve izlemek için *z sırasını*değiştirebilirsiniz. Z düzeni, formun z ekseni (derinlik) üzerinde bir formdaki denetimlerin görsel katmandır. Z düzeninin en üstündeki pencere diğer tüm pencereler ile çakışıyor. Diğer tüm pencereler, z düzeninin alt kısmındaki pencereyle çakışıyor.

## <a name="to-layer-controls-at-design-time"></a>Tasarım zamanında katman denetimlerine

1. Visual Studio 'da, katman yapmak istediğiniz bir denetim seçin.

2. **Biçim** menüsünde, **sipariş**' i seçin ve ardından **öne getir** veya **geri gönder**' i seçin.

## <a name="to-layer-controls-programmatically"></a>Denetimleri programlı olarak katman halinde

<xref:System.Windows.Forms.Control.BringToFront%2A> <xref:System.Windows.Forms.Control.SendToBack%2A> Denetimlerin z düzenini değiştirmek için ve yöntemlerini kullanın.

Örneğin, bir <xref:System.Windows.Forms.TextBox> Denetim `txtFirstName` başka bir denetimin altında ise ve en üstte olmasını istiyorsanız aşağıdaki kodu kullanın:

```vb
txtFirstName.BringToFront()
```

```csharp
txtFirstName.BringToFront();
```

```cpp
txtFirstName->BringToFront();
```

> [!NOTE]
> Windows Forms *Denetim kapsamayı*destekler. Denetim kapsama, bir denetim içindeki denetim sayısı gibi, kapsayan bir denetim içinde çok sayıda denetim yerleştirmeyi içerir <xref:System.Windows.Forms.RadioButton> <xref:System.Windows.Forms.GroupBox> . Daha sonra, içeren denetim içindeki denetimleri katmandan yönetebilirsiniz. Grup kutusunun taşınması, içinde olduklarından, denetimleri de taşır.

## <a name="see-also"></a>Ayrıca bkz.

- [Windows Forms denetimleri](index.md)
- [Ayrı Windows Forms Denetimlerini Etiketleme ve Kısayollarını Sunma](labeling-individual-windows-forms-controls-and-providing-shortcuts-to-them.md)
- [Windows Forms'ta Kullanılacak Denetimler](controls-to-use-on-windows-forms.md)
- [İşleve Göre Windows Forms Denetimleri](windows-forms-controls-by-function.md)
