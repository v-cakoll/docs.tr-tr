---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
ms.openlocfilehash: bcbc690690993a094bc5360d0c13bddebf8cd615
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414252"
---
# <a name="-win32resource"></a>-win32resource
Çıktı dosyasına bir Win32 kaynak dosyası ekler.  
  
## <a name="syntax"></a>Söz dizimi  
  
```console  
-win32resource:filename  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler  
 `filename`  
 Çıkış dosyanıza eklenecek kaynak dosyasının adı. Bir boşluk içeriyorsa dosya adını tırnak işaretleri ("") içine alın.  
  
## <a name="remarks"></a>Açıklamalar  
 Microsoft Windows Kaynak derleyicisi (RC) ile bir Win32 kaynak dosyası oluşturabilirsiniz.  
  
 Win32 kaynağı, uygulamanızın **Dosya Gezgini**'nde tanımlanmasına yardımcı olan sürüm veya bit eşlem (simge) bilgilerini içerebilir. Belirtmezseniz `-win32resource` , derleyici derleme sürümüne göre sürüm bilgileri oluşturur. `-win32resource`Ve `-win32icon` seçenekleri birbirini dışlıyor.  
  
 Bir .NET Framework kaynak dosyasına başvurmak için bkz. [-linkresource (Visual Basic)](linkresource.md) veya .NET Framework kaynak dosyası iliştirmek için [-Resource (Visual Basic)](resource.md) .  
  
> [!NOTE]
> `-win32resource`Bu seçenek, Visual Studio geliştirme ortamı içinden kullanılamaz; yalnızca komut satırından derlenirken kullanılabilir.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki kod `In.vb` , bir Win32 kaynak dosyasını derler ve iliştirir `Rf.res` :  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Visual Basic komut satırı derleyicisi](index.md)
- [Örnek Derleme Komut Satırları](sample-compilation-command-lines.md)
