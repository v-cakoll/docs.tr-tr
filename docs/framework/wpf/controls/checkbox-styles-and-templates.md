---
title: CheckBox Stilleri ve Şablonları
ms.date: 03/30/2017
helpviewer_keywords:
- states [WPF], CheckBox
- templates [WPF], CheckBox
- parts [WPF], CheckBox
- ControlTemplate [WPF], CheckBox
- CheckBox [WPF], styles and templates
- styles [WPF], CheckBox
ms.assetid: bfdaec96-d101-4d3d-864d-c27e6b621d03
ms.openlocfilehash: b9eee48c01f53e12bbe4a72f84c20eab68d5de23
ms.sourcegitcommit: 17ee6605e01ef32506f8fdc686954244ba6911de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74283814"
---
# <a name="checkbox-styles-and-templates"></a>CheckBox Stilleri ve Şablonları
Bu konuda <xref:System.Windows.Controls.CheckBox> denetimine yönelik stiller ve şablonlar açıklanmaktadır. Denetime benzersiz bir görünüm sağlamak için varsayılan <xref:System.Windows.Controls.ControlTemplate> değiştirebilirsiniz. Daha fazla bilgi için bkz. [Denetim için şablon oluşturma](../../../desktop-wpf/themes/how-to-create-apply-template.md).  
  
## <a name="checkbox-parts"></a>Onay kutusu parçaları  
 <xref:System.Windows.Controls.CheckBox> denetiminde hiç adlandırılmış bölüm yok.  
  
## <a name="checkbox-states"></a>Onay kutusu durumları  
 Aşağıdaki tabloda <xref:System.Windows.Controls.CheckBox> denetimi için görsel durumlar listelenmektedir.  
  
|VisualState adı|VisualStateGroup adı|Açıklama|  
|----------------------|---------------------------|-----------------|  
|Normal|Ortak durumlar|Varsayılan durum.|  
|Gelme olayından|Ortak durumlar|Fare işaretçisi denetimin üzerine yerleştirilir.|  
|Basılan|Ortak durumlar|Denetime basıldığında.|  
|Devre dışı|Ortak durumlar|Denetim devre dışı bırakıldı.|  
|Diğinize|Odaklardaki durumlar|Denetim odağa sahiptir.|  
|Odaklanmadan gözetle|Odaklardaki durumlar|Denetimin odağı yok.|  
|İşaretli|CheckStates|<xref:System.Windows.Controls.Primitives.ToggleButton.IsChecked%2A> `true`.|  
|Olmayan|CheckStates|<xref:System.Windows.Controls.Primitives.ToggleButton.IsChecked%2A> `false`.|  
|Belirsiz|CheckStates|<xref:System.Windows.Controls.Primitives.ToggleButton.IsThreeState%2A> `true`ve <xref:System.Windows.Controls.Primitives.ToggleButton.IsChecked%2A> `null`.|  
|Geçerli|Doğrulama durumları|Denetim, <xref:System.Windows.Controls.Validation> sınıfını ve <xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> iliştirilmiş özelliği `false`kullanır.|  
|Invalidunodaklanmış|Doğrulama durumları|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> ekli özelliği, denetimin odağa sahip `true`.|  
|Invalidodaklanmış|Doğrulama durumları|<xref:System.Windows.Controls.Validation.HasError%2A?displayProperty=nameWithType> ekli özelliği, denetimin odağa sahip `true`.|  
  
## <a name="checkbox-controltemplate-example"></a>CheckBox ControlTemplate örneği  
 Aşağıdaki örnek, <xref:System.Windows.Controls.CheckBox> denetimi için <xref:System.Windows.Controls.ControlTemplate> tanımlanacağını gösterir.  
  
 [!code-xaml[ControlTemplateExamples#CheckBox](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/checkbox.xaml#checkbox)]  
  
 Yukarıdaki örnekte aşağıdaki kaynaklardan biri veya daha fazlası kullanılmaktadır.  
  
 [!code-xaml[ControlTemplateExamples#Resources](~/samples/snippets/csharp/VS_Snippets_Wpf/ControlTemplateExamples/CS/resources/shared.xaml#resources)]  
  
 Tüm örnek için bkz. [ControlTemplates Ile stillendirme örneği](https://github.com/Microsoft/WPF-Samples/tree/master/Styles%20&%20Templates/IntroToStylingAndTemplating).  
  
## <a name="see-also"></a>Ayrıca bkz.

- <xref:System.Windows.FrameworkElement.Style%2A>
- <xref:System.Windows.Controls.ControlTemplate>
- [Denetim Stilleri ve Şablonları](control-styles-and-templates.md)
- [Denetim Özelleştirme](control-customization.md)
- [Stil ve Şablon Oluşturma](../../../desktop-wpf/fundamentals/styles-templates-overview.md)
- [Denetim için şablon oluşturma](../../../desktop-wpf/themes/how-to-create-apply-template.md)
