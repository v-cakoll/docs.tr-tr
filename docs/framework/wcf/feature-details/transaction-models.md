---
title: İşlem Modelleri
ms.date: 03/30/2017
ms.assetid: 48a8bc1b-128b-4cf1-a421-8cc73223c340
ms.openlocfilehash: d6c78a5342bf19d19308352cddc241f436bfcb3a
ms.sourcegitcommit: de17a7a0a37042f0d4406f5ae5393531caeb25ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76745327"
---
# <a name="transaction-models"></a>İşlem Modelleri
Bu konu, işlem programlama modelleri ile Microsoft 'un sağladığı altyapı bileşenleri arasındaki ilişkiyi açıklar.  
  
 Windows Communication Foundation (WCF) ' de işlemleri kullanırken, farklı işlem modelleri arasında seçim olmadığınızı, ancak tümleşik ve tutarlı bir modelin farklı katmanlarında çalıştığını anlamak önemlidir.  
  
 Aşağıdaki bölümlerde, üç birincil işlem bileşeni açıklanır.  
  
## <a name="windows-communication-foundation-transactions"></a>Windows Communication Foundation Işlemler  
 WCF 'de işlem desteği, işlem hizmetleri yazmanıza izin verir. Ayrıca, WS-AtomicTransaction (WS-AT) protokolü desteğiyle, uygulamalar, WCF veya üçüncü taraf teknolojisi kullanılarak oluşturulan Web hizmetlerine işlemleri akabilir.  
  
 WCF hizmeti veya uygulamasında, WCF işlem özellikleri, altyapının işlem oluşturma, akış ve zaman eşitlemesini nasıl ve ne zaman bir şekilde kullandığını bildirimli olarak, öznitelikler ve yapılandırma sağlar.  
  
## <a name="systemtransactions-transactions"></a>System. Transactions Işlemleri  
 <xref:System.Transactions> ad alanı hem <xref:System.Transactions.Transaction> sınıfını temel alan açık bir programlama modeli hem de altyapının işlemleri otomatik olarak yönettiği <xref:System.Transactions.TransactionScope> sınıfını kullanan örtük bir programlama modeli sağlar.  
  
 Bu iki modeli kullanarak bir işlem uygulamasının nasıl oluşturulacağı hakkında daha fazla bilgi için bkz. [bir Işlem uygulaması yazma](https://go.microsoft.com/fwlink/?LinkId=94947).  
  
 Bir WCF hizmetinde veya uygulamada <xref:System.Transactions>, bir istemci uygulaması içinde işlem oluşturmak için programlama modeli ve gerektiğinde bir işlem ile açıkça etkileşimde bulunmak için bir hizmet içinde sağlar.  
  
## <a name="msdtc-transactions"></a>MSDTC Işlemleri  
 Microsoft Dağıtılmış İşlem Düzenleyicisi (MSDTC), dağıtılmış işlemler için destek sağlayan bir işlem yöneticisidir.  
  
 Daha fazla bilgi için bkz. [DTC programlayıcı başvurusu](https://docs.microsoft.com/previous-versions/windows/desktop/ms686108(v=vs.85)).  
  
 Bir WCF hizmetinde veya uygulamada, MSDTC, bir istemci veya hizmet içinde oluşturulan işlemlerin koordinasyonu için altyapı sağlar.
