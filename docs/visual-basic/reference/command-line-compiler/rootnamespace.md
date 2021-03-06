---
title: -rootnamespace
ms.date: 03/13/2018
f1_keywords:
- /rootnamespace
- rootnamespace
helpviewer_keywords:
- /rootnamespace compiler option [Visual Basic]
- -rootnamespace compiler option [Visual Basic]
- rootnamespace compiler option [Visual Basic]
ms.assetid: e9245edf-6bef-420d-a7c7-324117752783
ms.openlocfilehash: b6a2f3ba0772d8f8c8c6a762a1be01703d21b778
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403141"
---
# <a name="-rootnamespace"></a>-rootnamespace
Tüm tür bildirimleri için bir ad alanı belirtir.  
  
## <a name="syntax"></a>Söz dizimi  
  
```console  
-rootnamespace:namespace  
```  
  
## <a name="arguments"></a>Bağımsız değişkenler  
  
|Terim|Tanım|  
|---|---|  
|`namespace`|Geçerli proje için tüm tür bildirimlerinin içine alınacağı ad alanının adı.|  
  
## <a name="remarks"></a>Açıklamalar  
 Visual Studio tümleşik geliştirme ortamında oluşturulan bir projeyi derlemek için Visual Studio yürütülebilir dosyası (devenv. exe) kullanırsanız, `-rootnamespace` özelliğinin değerini belirtmek için kullanın <xref:VSLangProj80.VBProjectProperties3.RootNamespace%2A> . Daha fazla bilgi için bkz. [Devenv komut satırı anahtarları](/visualstudio/ide/reference/devenv-command-line-switches) .  
  
 `Ildasm.exe`Çıkış dosyanızdaki ad alanı adlarını görüntülemek için ortak dil çalışma zamanı MSIL Disassembler () kullanın.  
  
|Visual Studio tümleşik geliştirme ortamında Set-RootNamespace|  
|---|  
|1. **Çözüm Gezgini**bir proje seçili olmalıdır. **Proje** menüsünde **Özellikler**' e tıklayın. <br />2. **uygulama** sekmesine tıklayın.<br />3. **kök ad alanı** kutusundaki değeri değiştirin.|  
  
## <a name="example"></a>Örnek  
 Aşağıdaki kod, `In.vb` ad alanındaki tüm tür bildirimlerini derler ve içine alır `mynamespace` .  
  
```console
vbc -rootnamespace:mynamespace in.vb  
```  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Visual Basic komut satırı derleyicisi](index.md)
- [Ildadsm. exe (Il ayırıcı)](../../../framework/tools/ildasm-exe-il-disassembler.md)
- [Örnek Derleme Komut Satırları](sample-compilation-command-lines.md)
