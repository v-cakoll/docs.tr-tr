---
title: özellik
ms.date: 03/30/2017
ms.assetid: a941c53f-fc97-42c2-8832-0fb9f1d55c06
ms.openlocfilehash: d1e20a6570c458041ec5d8ececbfa291ca9e4612
ms.sourcegitcommit: 22be09204266253d45ece46f51cc6f080f2b3fd6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/07/2019
ms.locfileid: "73735398"
---
# <a name="property"></a>özellik
*Özellikler* [varlık türlerinin](entity-type.md) ve [karmaşık türlerin](complex-type.md)temel yapı taşlarıdır. Özellikler bir varlık türü örneği veya karmaşık tür örneği içereceği verilerin şeklini ve özelliklerini tanımlar. Kavramsal modeldeki özellikler, bir sınıfta tanımlanan özelliklerle benzerdir. Bir sınıftaki özellikler, sınıfın şeklini tanımlar ve nesneler hakkında bilgi taşıyan bir kavramsal modeldeki Özellikler bir varlık türünün şeklini tanımlar ve varlık türü örnekleri hakkında bilgi taşır.  
  
> [!NOTE]
> Bu konu başlığı altında açıklandığı gibi özellikler, gezinti özelliklerinden farklıdır. Daha fazla bilgi için bkz. [gezinme özellikleri](navigation-property.md).  
  
 Bir özellik tanımı aşağıdaki bilgileri içerir:  
  
- Özellik adı. Istenir  
  
- Bir özellik türü. Istenir  
  
- Bir dizi [model](facet.md). (İsteğe bağlı)  
  
 Bir özellik, ilkel veriler (örneğin, bir dize, bir tamsayı veya Boole değeri) veya yapılandırılmış veriler (karmaşık bir tür gibi) içerebilir. İlkel türdeki özellikler de skaler özellikler olarak adlandırılır. Daha fazla bilgi için bkz. [varlık veri modeli: Ilkel veri türleri](entity-data-model-primitive-data-types.md).  
  
> [!NOTE]
> Karmaşık bir tür, karmaşık türler olan özelliklere sahip olabilir.  
  
## <a name="example"></a>Örnek  
 Aşağıdaki diyagramda üç varlık türü olan kavramsal bir model gösterilmektedir: `Book`, `Publisher`ve `Author`. Her varlık türünün bazı özellikleri vardır, ancak her bir özelliğin tür bilgisi diyagramda verilmez. [Varlık anahtarları](entity-key.md) olan Özellikler (anahtar) ile gösterilir.  
  
 ![Üç varlık türüne sahip örnek model](./media/property/example-model-three-entity-types.gif)  
  
 [ADO.NET Entity Framework](./ef/index.md) kavramsal model tanımlamak için kavramsal şema tanım dili ([csdl](/ef/ef6/modeling/designer/advanced/edmx/csdl-spec)) adlı bir etki ALANıNA özgü dil (DSL) kullanır. Aşağıdaki CSDL `Book` varlık türünü tanımlar (Yukarıdaki diyagramda gösterildiği gibi) ve XML özniteliklerini kullanarak her bir özelliğin türünü ve adını gösterir. İsteğe bağlı bir model, `Nullable`bir XML özniteliği kullanılarak da tanımlanır.  
  
 [!code-xml[EDM_Example_Model#EntityExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books.edmx#entityexample)]  
  
 Diyagramda gösterilen özelliklerden biri karmaşık tür özelliği olabilir. Örneğin, `Publisher` varlık türündeki `Address` özelliği, `StreetAddress`, `City`, `StateOrProvince`, `Country`ve `PostalCode`gibi çeşitli skaler özelliklerden oluşan karmaşık bir tür özelliği olabilir. Böyle bir karmaşık türün CSDL temsili aşağıdaki gibi olacaktır:  
  
 [!code-xml[EDM_Example_Model#ComplexTypeExample](../../../../samples/snippets/xml/VS_Snippets_Data/edm_example_model/xml/books2.edmx#complextypeexample)]  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Varlık Veri Modeli Temel Kavramları](entity-data-model-key-concepts.md)
- [Varlık Veri Modeli](entity-data-model.md)
