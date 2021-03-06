---
title: Kullanıcı tanımlı işlevler
ms.date: 12/13/2019
description: Kullanıcı tanımlı skaler ve toplama işlevleri oluşturmayı öğrenin.
ms.openlocfilehash: 9ee3fbac73215353c8dc82199203b0d25e580cdb
ms.sourcegitcommit: 30a558d23e3ac5a52071121a52c305c85fe15726
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/25/2019
ms.locfileid: "75447176"
---
# <a name="user-defined-functions"></a>Kullanıcı tanımlı işlevler

Çoğu veritabanında kendi işlevlerinizi tanımlamak için kullanabileceğiniz işlemsel bir SQL diyalekti vardır. SQLite ancak, uygulamanızla birlikte işlem içinde çalışır. Yeni bir SQL diyalekti öğrenmeniz yerine uygulamanızın programlama dilini kullanabilirsiniz.

## <a name="scalar-functions"></a>Skaler işlevler

Skaler işlevler, sorgudaki her satır için tek ve skaler bir değer döndürür. Kullanarak <xref:Microsoft.Data.Sqlite.SqliteConnection.CreateFunction%2A>yeni skalar işlevleri tanımlayın ve yerleşik olanları geçersiz kılın.

Bağımsız değişken için desteklenen bir parametre ve dönüş türleri listesi için bkz. [veri türleri.](types.md) `func`

`state` Bağımsız değişkeninin belirtilmesi, bu değeri işlevinin her çağrısına geçilecektir. Kapanışlar önlemek için bunu kullanın.

Bir `isDeterministic` SQLite 'un sorguları derlerken ek iyileştirmeler kullanmasına izin vermek için işlevinizin belirleyici olup olmadığını belirtin.

Aşağıdaki örnek, bir silindirin yarıçapını hesaplamak için bir skalar işlevin nasıl ekleneceğini gösterir.

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/ScalarFunctionSample/Program.cs?name=snippet_CreateFunction)]

## <a name="operators"></a>İşleçler

Aşağıdaki SQLite işleçleri, karşılık gelen skaler işlevler tarafından uygulanır. Bu skaler işlevlerin uygulamanızda tanımlanması, bu işleçlerin davranışını geçersiz kılar.

| İşleç          | İşlev      |
| ----------------- | ------------- |
| X GLOB Y          | glob (Y, X)    |
| Y BENZERI X          | benzer (Y, X)    |
| X GIBI Y KAÇıŞ Z | benzer (Y, X, Z) |
| X MATCH Y         | Eşleştir (Y, X)   |
| X REGEXP Y        | RegExp (Y, X)  |

Aşağıdaki örnek, karşılık gelen işlecini etkinleştirmek için RegExp işlevinin nasıl tanımlanacağını gösterir. SQLite, RegExp işlevinin varsayılan bir uygulamasını içermez.

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/RegularExpressionSample/Program.cs?name=snippet_Regex)]

## <a name="aggregate-functions"></a>Toplama işlevleri

Toplama işlevleri bir sorgudaki tüm satırlar için tek ve toplanmış bir değer döndürür. Kullanarak <xref:Microsoft.Data.Sqlite.SqliteConnection.CreateAggregate%2A>toplama işlevlerini tanımlayın ve geçersiz kılın.

`seed` Bağımsız değişkeni, bağlamın ilk durumunu belirtir. Kapanışlar da önlemek için bunu kullanın.

Bağımsız `func` değişken, satır başına bir kez çağrılır. Son sonucu biriktirmek için bağlamını kullanın. Bağlamı döndürün. Bu model, bağlamın bir değer türü veya sabit olmasını sağlar.

Hayır `resultSelector` belirtilirse, sonuç olarak bağlamın son durumu kullanılır. Bu, Sum ve Count gibi işlevlerin tanımını, yalnızca her bir satırı sayının artmasını ve döndürmesini sağlamak için basitleştirir.

Tüm `resultSelector` satırlarda yineleme yaptıktan sonra, son sonucun bağlamını hesaplamak için belirtin.

Bağımsız değişkeni ve dönüş türleri için desteklenen parametre türlerinin bir listesi için bkz `resultSelector`. [veri türleri](types.md) . `func`

İşleviniz belirleyici ise, SQLite 'un `isDeterministic` sorguları derlerken ek iyileştirmeler kullanmasına izin vermek için belirtin.

Aşağıdaki örnek bir sütunun standart sapmasını hesaplamak için bir toplama işlevi tanımlar.

[!code-csharp[](../../../../samples/snippets/standard/data/sqlite/AggregateFunctionSample/Program.cs?name=snippet_CreateAggregate)]

## <a name="errors"></a>Hatalar

Kullanıcı tanımlı bir işlev bir özel durum oluşturursa, ileti SQLite 'a döndürülür. SQLite daha sonra bir hata ve Microsoft. Data. SQLite, bir SqliteException oluşturur. Daha fazla bilgi için bkz. [veritabanı hataları](database-errors.md).

Varsayılan olarak, hata SQLite hata kodu SQLITE_ERROR (veya 1) olacaktır. Ancak, istediğiniz <xref:Microsoft.Data.Sqlite.SqliteException> <xref:Microsoft.Data.Sqlite.SqliteException.SqliteErrorCode> gibi, işlevinizde bir oluşturarak bunu değiştirebilirsiniz.

## <a name="debugging"></a>Hata Ayıklama

SQLite uygulamanızı doğrudan çağırır. Bu, SQLite sorguları değerlendirirken tetiklenecek kesme noktaları eklemenize olanak sağlar. Tam .NET hata ayıklama deneyimi, Kullanıcı tanımlı işlevlerinizi oluşturmanıza yardımcı olmak için kullanılabilir.

## <a name="see-also"></a>Ayrıca bkz.

* [Veri türleri](types.md)
* [SQLite çekirdek işlevleri](https://www.sqlite.org/lang_corefunc.html)
* [SQLite toplama Işlevleri](https://www.sqlite.org/lang_aggfunc.html)
