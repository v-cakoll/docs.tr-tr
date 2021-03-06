---
title: Dosya Kodlamaları
ms.date: 07/20/2015
helpviewer_keywords:
- character encodings
- files [Visual Basic], encoding
- Unicode, file encoding
- file encoding
ms.assetid: ea2c5f5f-bbb1-4150-9928-b9951fa6bc57
ms.openlocfilehash: f906b2f2d747a7950c70a24549bbf5423e5b87b4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401751"
---
# <a name="file-encodings-visual-basic"></a>Dosya Kodlamaları (Visual Basic)

Karakter kodlamaları olarak da bilinen dosya kodlamaları, metin işleme sırasında karakterlerin nasıl temsil edileceğini belirtir. Tek bir kodlama, bir veya işleyemeyen dil karakterleri açısından başka bir şekilde tercih edilebilir, ancak UNICODE genellikle tercih edilir.

Dosyadan okuma veya yazma yaparken, dosya kodlamaları yanlış şekilde eşleştirilirken özel durumlar veya hatalı sonuçlar oluşabilir.

## <a name="types-of-encodings"></a>Kodlamalar türleri

Unicode, dosyalarla çalışırken tercih edilen kodlanıyor. Unicode, yayımlamak için kullanılan teknik semboller ve özel karakterler de dahil olmak üzere modern bilgi işlem 'da kullanılan tüm karakterleri temsil etmek için 16 bit kod değerlerini kullanan dünya çapındaki bir karakter kodlama standardıdır.

Önceki karakter kodlama standartları, 8 bit kod değerleri kullanan Windows ANSI karakter kümesi veya belirli bir dilde veya coğrafi bölgede kullanılan karakterleri göstermek için 8 bit değer bileşimleri gibi geleneksel karakter kümelerinden oluşur.

## <a name="encoding-class"></a>Kodlama sınıfı

<xref:System.Text.Encoding>Sınıfı bir karakter kodlamasını temsil eder. Bu tabloda, kullanılabilir kodlamalar türü listelenmekte ve her biri açıklanmaktadır.

|Name|Description|
|---|---|
|<xref:System.Text.ASCIIEncoding>|Unicode karakterlerinin ASCII karakter kodlamasını temsil eder.|
|<xref:System.Text.UnicodeEncoding>|Unicode karakterlerinin UTF-16 kodlamasını temsil eder.|
|<xref:System.Text.UTF32Encoding>|Unicode karakterlerinin UTF-32 kodlamasını temsil eder.|
|<xref:System.Text.UTF7Encoding>|Unicode karakterlerinin UTF-7 kodlamasını temsil eder.|
|<xref:System.Text.UTF8Encoding>|Unicode karakterlerinin UTF-8 kodlamasını temsil eder.|

## <a name="see-also"></a>Ayrıca bkz.

- [Dosyalardan Okuma](reading-from-files.md)
- [Dosyalara Yazma](writing-to-files.md)
