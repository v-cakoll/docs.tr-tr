---
title: Öznitelik Tabanlı Eşleme
ms.date: 03/30/2017
ms.assetid: 6dd89999-f415-4d61-b8c8-237d23d7924e
ms.openlocfilehash: 1e11a2efc3d1afa56a27d6e2c60149a509511080
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70248071"
---
# <a name="attribute-based-mapping"></a>Öznitelik Tabanlı Eşleme
[!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]öznitelikleri uygulayarak veya bir dış eşleme [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] dosyası kullanarak bir SQL Server veritabanını nesne modeliyle eşler. Bu konu, öznitelik tabanlı yaklaşımı özetler.  
  
 En basit formunda [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] , bir veritabanını bir <xref:System.Data.Linq.DataContext>, bir sınıfa, bir tabloya ve sütunları ve bu sınıfların özellikleriyle ilişkilerini eşler. Öznitelikleri, nesne modelinizdeki devralma hiyerarşisini eşlemek için de kullanabilirsiniz. Daha fazla bilgi için [nasıl yapılır: Visual Basic veya C# ](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)içinde nesne modeli oluşturun.  
  
 Visual Studio kullanan geliştiriciler genellikle Nesne İlişkisel Tasarımcısı kullanarak öznitelik tabanlı eşleme gerçekleştirir. Ayrıca, SQLMetal komut satırı aracını kullanabilir veya öznitelikleri kendiniz kodlayabilirsiniz. Daha fazla bilgi için [nasıl yapılır: Visual Basic veya C# ](how-to-generate-the-object-model-in-visual-basic-or-csharp.md)içinde nesne modeli oluşturun.  
  
> [!NOTE]
> Ayrıca, bir dış XML dosyası kullanarak da eşleyebilirsiniz. Daha fazla bilgi için bkz. [dış eşleme](external-mapping.md).  
  
 Aşağıdaki bölümlerde, öznitelik tabanlı eşleme daha ayrıntılı olarak açıklanır. Daha fazla bilgi için bkz <xref:System.Data.Linq.Mapping> . ad alanı.  
  
## <a name="databaseattribute-attribute"></a>DatabaseAttribute özniteliği  
 Bağlantı tarafından bir ad sağlanmadığında veritabanının varsayılan adını belirtmek için bu özniteliği kullanın. Bu öznitelik isteğe bağlıdır, ancak kullanıyorsanız, aşağıdaki tabloda açıklandığı gibi <xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A> özelliğini uygulamanız gerekir.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A>|Dize|Bakýn<xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A>|<xref:System.Data.Linq.Mapping.DatabaseAttribute.Name%2A> Özelliği ile birlikte kullanıldığında, veritabanının adını belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.DatabaseAttribute>.  
  
## <a name="tableattribute-attribute"></a>TableAttribute özniteliği  
 Bir sınıfı bir veritabanı tablosu veya görünümüyle ilişkili bir varlık sınıfı olarak belirlemek için bu özniteliği kullanın. [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)]Bu özniteliğe sahip sınıfları kalıcı sınıflar olarak değerlendirir. Aşağıdaki tabloda <xref:System.Data.Linq.Mapping.TableAttribute.Name%2A> özelliği açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.TableAttribute.Name%2A>|Dize|Sınıf adıyla aynı dize|Bir sınıfı bir veritabanı tablosuyla ilişkili bir varlık sınıfı olarak belirler.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.TableAttribute>.  
  
## <a name="columnattribute-attribute"></a>ColumnAttribute özniteliği  
 Bir veritabanı tablosundaki bir sütunu temsil etmek üzere bir varlık sınıfının bir üyesini belirlemek için bu özniteliği kullanın. Bu özniteliği herhangi bir alan veya özelliğe uygulayabilirsiniz.  
  
 Yalnızca sütun olarak tanımlayabilen Üyeler veritabanına yapılan değişiklikleri [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] kaydettiğinde alınır ve kalıcı hale getirilir. Bu özniteliği olmayan Üyeler kalıcı olmayan ve ekleme veya güncelleştirme için gönderilmemiş kabul edilir.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.AutoSync%2A>|AutoSync|hiçbir zaman|Bir ekleme veya güncelleştirme işleminden sonra değeri almak için ortak dil çalışma zamanı (CLR) verir.<br /><br /> Seçenekler: Always, hiçbir zaman, OnUpdate, OnInsert.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.CanBeNull%2A>|Boole değeri|`true`|Bir sütunun null değer içerebileceğini gösterir.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.DbType%2A>|Dize|Çıkarılan veritabanı sütun türü|Veritabanı sütunu türünü belirtmek için veritabanı türlerini ve değiştiricilerini kullanır.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.Expression%2A>|Dize|Olmamalıdır|Veritabanında hesaplanan sütunu tanımlar.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsDbGenerated%2A>|Boole değeri|`false`|Bir sütunun, veritabanının otomatik olarak oluşturduğu değerleri içerdiğini belirtir.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsDiscriminator%2A>|Boole değeri|`false`|Sütunun [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] devralma hiyerarşisi için bir Ayrıştırıcı değeri içerdiğini belirtir.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsPrimaryKey%2A>|Boole değeri|`false`|Bu sınıf üyesinin, tablonun birincil anahtarlarının bir parçası olan veya içeren bir sütunu temsil ettiğini belirtir.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A>|Boole değeri|`false`|Üyenin sütun türünü bir veritabanı zaman damgası veya sürüm numarası olarak tanımlar.|  
|<xref:System.Data.Linq.Mapping.ColumnAttribute.UpdateCheck%2A>|UpdateCheck|`Always`, üyesi <xref:System.Data.Linq.Mapping.ColumnAttribute.IsVersion%2A> olmadığı `true` için|İyimser eşzamanlılık [!INCLUDE[vbtecdlinq](../../../../../../includes/vbtecdlinq-md.md)] çakışmalarının algılanma yaklaşımının nasıl yapılacağını belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.ColumnAttribute>.  
  
> [!NOTE]
> AssociationAttribute ve ColumnAttribute Storage Özellik değerleri büyük/küçük harfe duyarlıdır. Örneğin, AssociationAttribute. Storage özelliğinin özniteliğinde kullanılan değerlerin, kodun başka bir yerinde kullanılan ilgili özellik adları için büyük/küçük harf ile eşleştiğinden emin olun. Bu, genellikle büyük/küçük harf duyarlı olmayan, hatta Visual Basic dahil olmak üzere tüm .NET programlama dilleri için geçerlidir. Depolama özelliği hakkında daha fazla bilgi için bkz <xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A?displayProperty=nameWithType>.  
  
## <a name="associationattribute-attribute"></a>AssociationAttribute özniteliği  
 Bu özniteliği, veritabanındaki bir ilişkilendirmeyi temsil edecek bir özelliği (örneğin, birincil anahtar ilişkisine yabancı anahtar) belirlemek için kullanın. İlişkiler hakkında daha fazla bilgi için bkz [. nasıl yapılır: Veritabanı Ilişkilerini](how-to-map-database-relationships.md)eşleyin.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.DeleteOnNull%2A>|Boole değeri|`false`|Yabancı anahtar üyeleri atanamayan bir ilişkiye yerleştirildiğinde, ilişkilendirme null olarak ayarlandığında nesneyi siler.|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.DeleteRule%2A>|Dize|Yok.|Bir ilişkiye silme davranışı ekler.|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.IsForeignKey%2A>|Boole değeri|`false`|True ise, üyeyi bir veritabanı ilişkisini temsil eden bir ilişkilendirmede yabancı anahtar olarak belirler.|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.IsUnique%2A>|Boole değeri|`false`|True ise yabancı anahtarda bir benzersizlik kısıtlamasını gösterir.|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.OtherKey%2A>|Dize|İlgili sınıfın KIMLIĞI|Hedef varlık sınıfının bir veya daha fazla üyesini, ilişkilendirmenin diğer tarafında anahtar değerler olarak belirler.|  
|<xref:System.Data.Linq.Mapping.AssociationAttribute.ThisKey%2A>|Dize|Kapsayan sınıfın KIMLIĞI|İlişkilendirmenin bu tarafındaki anahtar değerlerini temsil etmek için bu varlık sınıfının üyelerini belirler.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.AssociationAttribute>.  
  
> [!NOTE]
> AssociationAttribute ve ColumnAttribute Storage Özellik değerleri büyük/küçük harfe duyarlıdır. Örneğin, AssociationAttribute. Storage özelliğinin özniteliğinde kullanılan değerlerin, kodun başka bir yerinde kullanılan ilgili özellik adları için büyük/küçük harf ile eşleştiğinden emin olun. Bu, genellikle büyük/küçük harf duyarlı olmayan, hatta Visual Basic dahil olmak üzere tüm .NET programlama dilleri için geçerlidir. Depolama özelliği hakkında daha fazla bilgi için bkz <xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A?displayProperty=nameWithType>.  
  
## <a name="inheritancemappingattribute-attribute"></a>InheritanceMappingAttribute özniteliği  
 Devralma hiyerarşisini eşlemek için bu özniteliği kullanın.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Code%2A>|Dize|Yok. Değer sağlanmalıdır.|Ayrıştırıcıyı ait kod değerini belirtir.|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.IsDefault%2A>|Boole değeri|`false`|True ise, depodaki hiçbir ayrıştırıcı değeri belirtilen değerlerden herhangi birini eşleştirirken, bu türden bir nesne oluşturur.|  
|<xref:System.Data.Linq.Mapping.InheritanceMappingAttribute.Type%2A>|Tür|Yok. Değer sağlanmalıdır.|Hiyerarşideki sınıfın türünü belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.InheritanceMappingAttribute>.  
  
## <a name="functionattribute-attribute"></a>FunctionAttribute özniteliği  
 Bu özniteliği, bir saklı yordamı veya veritabanında kullanıcı tanımlı işlevi temsil eden bir yöntemi belirlemek için kullanın.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.FunctionAttribute.IsComposable%2A>|Boole değeri|`false`|Yanlışsa, saklı yordama eşlemeyi gösterir. True ise, Kullanıcı tanımlı bir işleve eşlemeyi gösterir.|  
|<xref:System.Data.Linq.Mapping.FunctionAttribute.Name%2A>|Dize|Veritabanında ad ile aynı dize|Saklı yordamın veya Kullanıcı tanımlı işlevin adını belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.FunctionAttribute>.  
  
## <a name="parameterattribute-attribute"></a>ParameterAttribute özniteliği  
 Saklı yordam yöntemlerinde giriş parametrelerini eşlemek için bu özniteliği kullanın.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ParameterAttribute.DbType%2A>|Dize|Yok.|Veritabanı türünü belirtir.|  
|<xref:System.Data.Linq.Mapping.ParameterAttribute.Name%2A>|Dize|Veritabanında parametre adıyla aynı dize|Parametre için bir ad belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.ParameterAttribute>.  
  
## <a name="resulttypeattribute-attribute"></a>ResultTypeAttribute özniteliği  
 Bu özniteliği bir sonuç türü belirtmek için kullanın.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.ResultTypeAttribute.Type%2A>|Tür|Seçim|Döndürülen <xref:System.Data.Linq.IMultipleResults>saklı yordamlara eşlenmiş yöntemlerde kullanılır. Saklı yordam için geçerli veya beklenen tür eşlemelerini bildirir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.ResultTypeAttribute>.  
  
## <a name="dataattribute-attribute"></a>DataAttribute özniteliği  
 Adları ve özel depolama alanlarını belirtmek için bu özniteliği kullanın.  
  
 Aşağıdaki tabloda bu özniteliğin özellikleri açıklanmaktadır.  
  
|Özellik|Tür|Varsayılan|Açıklama|  
|--------------|----------|-------------|-----------------|  
|<xref:System.Data.Linq.Mapping.DataAttribute.Name%2A>|Dize|Veritabanındaki adla aynı|Tablo, sütun vb. adı belirtir.|  
|<xref:System.Data.Linq.Mapping.DataAttribute.Storage%2A>|Dize|Ortak erişimciler|Temel alınan depolama alanının adını belirtir.|  
  
 Daha fazla bilgi için bkz. <xref:System.Data.Linq.Mapping.DataAttribute>.  
  
## <a name="see-also"></a>Ayrıca bkz.

- [Başvuru](reference.md)
