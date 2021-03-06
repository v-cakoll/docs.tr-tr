---
title: Soyundan gelenler yöntemini kullanarak tek bir soyundan bulma (C#)
ms.date: 07/20/2015
ms.assetid: 6f735be9-0293-4680-8007-ca9d96bfebed
ms.openlocfilehash: 59d8cfb93ec527a6ceaa58b422a154e16d712533
ms.sourcegitcommit: 7588136e355e10cbc2582f389c90c127363c02a5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "74141203"
---
# <a name="how-to-find-a-single-descendant-using-the-descendants-method-c"></a>Soyundan gelenler yöntemini kullanarak tek bir soyundan bulma (C#)
Tek bir <xref:System.Xml.Linq.XContainer.Descendants%2A> benzersiz adlandırılmış öğeyi bulmak için kodu hızla yazmak için eksen yöntemini kullanabilirsiniz. Bu teknik, belirli bir ada sahip belirli bir soyundan bulmak istediğinizde özellikle yararlıdır. İstenilen öğeye gitmek için kodu yazabilirsiniz, ancak <xref:System.Xml.Linq.XContainer.Descendants%2A> genellikle daha hızlı ve daha kolay ekseni kullanarak kod yazmaktır.  
  
## <a name="example"></a>Örnek  
 Bu örnekte <xref:System.Linq.Enumerable.First%2A> standart sorgu işleci kullanır.  
  
```csharp  
XElement root = XElement.Parse(@"<Root>  
  <Child1>  
    <GrandChild1>GC1 Value</GrandChild1>  
  </Child1>  
  <Child2>  
    <GrandChild2>GC2 Value</GrandChild2>  
  </Child2>  
  <Child3>  
    <GrandChild3>GC3 Value</GrandChild3>  
  </Child3>  
  <Child4>  
    <GrandChild4>GC4 Value</GrandChild4>  
  </Child4>  
</Root>");  
string grandChild3 = (string)  
    (from el in root.Descendants("GrandChild3")  
    select el).First();  
Console.WriteLine(grandChild3);  
```  
  
 Bu kod aşağıdaki çıktıyı üretir:  
  
```output  
GC3 Value  
```  
  
## <a name="example"></a>Örnek  
 Aşağıdaki örnek, ad alanında olan XML için aynı sorguyu gösterir. Daha fazla bilgi için [Bkz. NameSpaces Genel Bakış (LINQ - XML) (C#)](namespaces-overview-linq-to-xml.md).  
  
```csharp  
XElement root = XElement.Parse(@"<aw:Root xmlns:aw='http://www.adventure-works.com'>  
  <aw:Child1>  
    <aw:GrandChild1>GC1 Value</aw:GrandChild1>  
  </aw:Child1>  
  <aw:Child2>  
    <aw:GrandChild2>GC2 Value</aw:GrandChild2>  
  </aw:Child2>  
  <aw:Child3>  
    <aw:GrandChild3>GC3 Value</aw:GrandChild3>  
  </aw:Child3>  
  <aw:Child4>  
    <aw:GrandChild4>GC4 Value</aw:GrandChild4>  
  </aw:Child4>  
</aw:Root>");  
XNamespace aw = "http://www.adventure-works.com";  
string grandChild3 = (string)  
    (from el in root.Descendants(aw + "GrandChild3")  
     select el).First();  
Console.WriteLine(grandChild3);  
```  
  
 Bu kod aşağıdaki çıktıyı üretir:  
  
```output  
GC3 Value  
```  
