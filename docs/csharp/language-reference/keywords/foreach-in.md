---
title: C# foreach ifadesi
ms.date: 07/22/2020
f1_keywords:
- foreach
- foreach_CSharpKeyword
helpviewer_keywords:
- foreach keyword [C#]
- foreach statement [C#]
- in keyword [C#]
ms.assetid: 5a9c5ddc-5fd3-457a-9bb6-9abffcd874ec
ms.openlocfilehash: 4af431d29e538c1516efeaad3008eaa3b2229ece
ms.sourcegitcommit: 04022ca5d00b2074e1b1ffdbd76bec4950697c4c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87104238"
---
# <a name="foreach-in-c-reference"></a>foreach, in (C# Başvurusu)

`foreach`Deyimi, <xref:System.Collections.IEnumerable?displayProperty=nameWithType> <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> Aşağıdaki örnekte gösterildiği gibi, veya arabirimini uygulayan tür örneğindeki her öğe için bir deyimi veya bir deyim bloğunu yürütür:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="1" interactive="try-dotnet-method" :::

`foreach`İfade bu türlerle sınırlı değildir. Bunu, aşağıdaki koşullara uyan herhangi bir türde bir örnekle kullanabilirsiniz:

- bir tür `GetEnumerator` , dönüş türü Class, struct veya Interface türünde olan public parametresiz metoda sahiptir,
- metodun dönüş türü, `GetEnumerator` ortak `Current` özelliğine ve dönüş türü olan public parametresiz metoda sahiptir `MoveNext` <xref:System.Boolean> .

Aşağıdaki örnek, `foreach` <xref:System.Span%601?displayProperty=nameWithType> hiçbir arabirim uygulamayan tür örneği ile ifadesini kullanır:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="2" :::

C# 7,3 ile başlayarak, Numaralandırıcı `Current` özelliği bir [Başvuru dönüş değeri](ref.md#reference-return-values) döndürürse ( `ref T` burada `T` bir koleksiyon öğesinin türüdür), `ref` `ref readonly` Aşağıdaki örnekte gösterildiği gibi, veya değiştiricisini içeren bir yineleme değişkeni bildirebilirsiniz:

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="RefSpan" :::

C# 8,0 ' den başlayarak, `await foreach` zaman uyumsuz bir veri akışını (diğer bir deyişle, arabirimi uygulayan koleksiyon türü) kullanmak için ifadesini kullanabilirsiniz <xref:System.Collections.Generic.IAsyncEnumerable%601> . Next öğesi zaman uyumsuz olarak alındığında döngünün her yinelemesi askıya alınabilir. Aşağıdaki örnek, ifadesinin nasıl kullanılacağını gösterir `await foreach` :

:::code language="csharp" source="snippets/IterationKeywordsExamples.cs" id="AwaitForeach" :::

Akış öğeleri varsayılan olarak yakalanan bağlamda işlenir. Bağlam yakalamayı devre dışı bırakmak istiyorsanız, <xref:System.Threading.Tasks.TaskAsyncEnumerableExtensions.ConfigureAwait%2A?displayProperty=nameWithType> genişletme yöntemini kullanın. Eşitleme bağlamları ve geçerli bağlamı yakalama hakkında daha fazla bilgi için bkz. [görev tabanlı zaman uyumsuz model](../../../standard/asynchronous-programming-patterns/consuming-the-task-based-asynchronous-pattern.md)kullanma. Zaman uyumsuz akışlar hakkında daha fazla bilgi için [C# 8,0 'deki](../../whats-new/csharp-8.md) yenilikler makalesindeki [zaman uyumsuz akışlar](../../whats-new/csharp-8.md#asynchronous-streams) bölümüne bakın.

Deyimdeki herhangi bir noktada `foreach` , [Break](break.md) ifadesini kullanarak ya da [Continue](continue.md) ifadesini kullanarak döngünün bir sonraki yinelemesine adımla döngüyü kesebilirsiniz. Ayrıca `foreach` [goto](goto.md), [Return](return.md)veya [throw](throw.md) deyimleriyle bir döngüden çıkabilirsiniz.

`foreach`İfade öğesine uygulanırsa `null` , bir oluşturulur <xref:System.NullReferenceException> . Deyimin kaynak koleksiyonu `foreach` boşsa, `foreach` Döngünün gövdesi yürütülmez ve atlanır.

## <a name="type-of-an-iteration-variable"></a>Yineleme değişkeni türü

`var` `foreach` Aşağıdaki kodda gösterildiği gibi, derleyicinin deyimdeki bir yineleme değişkeni türünü saymasına izin vermek için anahtar sözcüğünü kullanabilirsiniz:

```csharp
foreach (var item in collection) { }
```

Aşağıdaki kodda gösterildiği gibi, bir yineleme değişkeni türünü de açıkça belirtebilirsiniz:

```csharp
IEnumerable<T> collection = new T[5];
foreach (V item in collection) { }
```

Önceki biçimde, `T` bir koleksiyon öğesinin türü örtük olarak veya `V` bir yineleme değişkeni türüne açıkça dönüştürülebilir olmalıdır. ' Den açık bir dönüştürme `T` `V` çalışma zamanında başarısız olursa, `foreach` ifade bir oluşturur <xref:System.InvalidCastException> . Örneğin, `T` korumalı olmayan bir sınıf türü ise, `V` Uygulamasız olsa da herhangi bir arabirim türü olabilir `T` . Çalışma zamanında, bir koleksiyon öğesinin türü, öğesinden türetilen `T` ve gerçekten uygulayan bir öğe olabilir `V` . Böyle bir durum yoksa, bir oluşturulur <xref:System.InvalidCastException> .

## <a name="c-language-specification"></a>C# dili belirtimi

Daha fazla bilgi için [C# dil belirtiminin](~/_csharplang/spec/introduction.md) [foreach ifadesi](~/_csharplang/spec/statements.md#the-foreach-statement) bölümüne bakın.

## <a name="see-also"></a>Ayrıca bkz.

- [C# başvurusu](../index.md)
- [C# anahtar sözcükleri](index.md)
- [Dizilerle foreach kullanma](../../programming-guide/arrays/using-foreach-with-arrays.md)
- [for deyimi](for.md)
