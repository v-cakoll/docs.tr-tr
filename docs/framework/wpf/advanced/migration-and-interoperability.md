---
title: Geçiş ve Birlikte Çalışabilirlik
ms.date: 03/23/2020
f1_keywords:
- AutoGeneratedOrientationPage
helpviewer_keywords:
- Windows Presentation Foundation [WPF], interoperability
- WPF [WPF], migration
- Windows Forms controls [WPF interoperability]
- controls [WPF interoperability]
- Windows Presentation Foundation [WPF], migration
- interoperability [WPF]
- WPF [WPF], interoperability
- migration [WPF]
ms.assetid: d655de05-bf63-4814-bc64-6b3be01c70a2
ms.openlocfilehash: 180ece4f178406c6c3e704ecadaa9a72955ec038
ms.sourcegitcommit: 99b153b93bf94d0fecf7c7bcecb58ac424dfa47c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80249057"
---
# <a name="migration-and-interoperability"></a>Geçiş ve Birlikte Çalışabilirlik

Bu sayfa, uygulamalar ve diğer Microsoft Windows [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] uygulamaları türleri arasında nasıl birlikte çalışma yapılacağını tartışan belgelere bağlantılar içerir.

## <a name="in-this-section"></a>Bu Bölümde

[WPF'den System.Xaml'a geçirilen türler](types-migrated-from-wpf-to-system.md)\
[WPF ve Windows Formlar İnİşara](wpf-and-windows-forms-interoperation.md)\
[WPF ve Win32 İnteroperation](wpf-and-win32-interoperation.md)\
[WPF ve Direct3D9 Birlikte Çalışması](wpf-and-direct3d9-interoperation.md)

## <a name="reference"></a>Başvuru

| Sözleşme Dönemi                                                     | Tanım                                                                                                                                                                                                                                                                                                                                                                                                  |
|----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| <xref:System.Windows.Forms.Integration.WindowsFormsHost> | Windows Forms denetimini sayfa [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] öğesi olarak barındırmak için kullanabileceğiniz bir öğe.                                                                                                                                                                                                                                      |
| <xref:System.Windows.Forms.Integration.ElementHost>      | Bir denetimi barındırmak için kullanabileceğiniz [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] bir Windows Forms denetimi.                                                                                                                                                                                                                                                                 |
| <xref:System.Windows.Interop.HwndSource>                 | Win32 uygulaması içinde bir [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] bölgeyi barındırAr.                                                                                                                                                                                                                                                                                |
| <xref:System.Windows.Interop.HwndHost>                   | Temel sınıf, <xref:System.Windows.Forms.Integration.WindowsFormsHost>tüm HWND tabanlı teknolojilerin bir [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] uygulama tarafından barındırıldığında kullandığı bazı temel işlevleri tanımlar. Bir [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] uygulama içinde win32 penceresi barındırmak için bu alt sınıf. |
| <xref:System.Windows.Interop.BrowserInteropHelper>       | Tarayıcı tarafından barındırılan bir [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] uygulama için tarayıcı ortamının koşullarını raporlamak için yardımcı sınıf.                                                                                                                                                                                                         |

## <a name="related-sections"></a>İlgili Bölümler
