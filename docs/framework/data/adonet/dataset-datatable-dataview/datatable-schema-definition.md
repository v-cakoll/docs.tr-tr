---
title: DataTable Şema Tanımı
ms.date: 03/30/2017
ms.assetid: efbcdda4-f5a9-421d-8be2-4c194c74552f
ms.openlocfilehash: d18af8001fd24f3b21c3e7fd13f9dabb2587b322
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70786383"
---
# <a name="datatable-schema-definition"></a>DataTable Şema Tanımı
Bir tablonun şeması veya yapısı, sütunlar ve kısıtlamalar ile temsil edilir. Bir <xref:System.Data.DataTable> using <xref:System.Data.DataColumn> nesnelerinin ve<xref:System.Data.ForeignKeyConstraint> ve nesnelerinin<xref:System.Data.UniqueConstraint> şemasını tanımlarsınız. Bir tablodaki sütunlar bir veri kaynağındaki sütunlarla eşlenir, deyimlerden hesaplanan değerler içerebilir, değerlerini otomatik olarak artırır veya birincil anahtar değerleri içerebilir.  
  
 Bir tablodaki sütunlara, ilişkilerine ve kısıtlamalara adına göre başvurular büyük/küçük harfe duyarlıdır. Bu nedenle, iki veya daha fazla sütun, ilişki veya kısıtlama, aynı ada sahip ancak büyük/küçük harflere sahip bir tabloda bulunabilir. Örneğin, **Sütun1** ve **Sütun1**olabilir. Böyle durumlarda, bir sütundan birine başvuru, sütun adı ile tam olarak aynı olmalıdır; Aksi takdirde, bir özel durum oluşturulur. Örneğin, **tablom** tabloadı **Sütun1** ve **Sütun1**sütunları Içeriyorsa, **sütünce** adını MyTable olarak adlandırın **. sütunlar ["Sütun1"]** ve **sütas** **MyTable. Columns ["Sütun1"]** . Sütunlardan birine MyTable olarak başvurulmaya çalışılıyor **. ["Sütun1"] sütunları** bir özel durum oluşturur.  
  
 Yalnızca bir sütun, ilişki veya belirli bir ada sahip kısıtlama varsa, büyük/küçük harf duyarlılığı kuralı uygulanmaz. Diğer bir deyişle, tablodaki başka bir sütun, ilişki veya kısıtlama nesnesi söz konusu sütun, ilişki veya kısıtlama nesnesinin adıyla eşleşiyorsa, herhangi bir durumu kullanarak nesneye ad ile başvurabilirsiniz ve hiçbir özel durum oluşturulmaz. Örneğin, tabloda yalnızca **Sütun1**varsa, My kullanarak buna başvurabilirsiniz **. Sütunlar ["SÜTUN1"]** .  
  
> [!NOTE]
> DataTable özelliği bu davranışı etkilemez. <xref:System.Data.DataTable.CaseSensitive%2A> **CaseSensitive** özelliği, bir tablodaki veriler için geçerlidir ve sıralama, arama, filtreleme, kısıtlamaları zorlama vb., ancak sütunlara, ilişkilerine ve kısıtlamalara başvurmaları etkiler.  
  
## <a name="in-this-section"></a>Bu Bölümde  
 [DataTable’a Sütun Ekleme](adding-columns-to-a-datatable.md)  
 **DataColumn** nesnelerini kullanarak bir tablonun sütunlarının nasıl tanımlanacağını açıklar.  
  
 [İfade Sütunları Oluşturma](creating-expression-columns.md)  
 Bir sütunun **Expression** özelliğinin, satırdaki diğer sütunlardan alınan değerlere göre değerleri hesaplamak için nasıl kullanılabileceğini açıklar.  
  
 [AutoIncrement Sütunları Oluşturma](creating-autoincrement-columns.md)  
 Satır başına benzersiz bir sütun değeri sağlamak için bir sütunun sayısal değerleri otomatik olarak arttırmasını nasıl ayarlayabileceğinizi açıklar.  
  
 [Birincil Anahtarlar Tanımlama](defining-primary-keys.md)  
 Bir veya daha fazla **DataColumn** nesnesinden bir tablonun birincil anahtarının nasıl belirtileceğini açıklar.  
  
 [DataTable Kısıtlamaları](datatable-constraints.md)  
 Bir tablodaki sütunlar için yabancı anahtar ve benzersiz kısıtlamaların nasıl tanımlanacağını açıklar.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [DataTables](datatables.md)
- [ADO.NET’e Genel Bakış](../ado-net-overview.md)
