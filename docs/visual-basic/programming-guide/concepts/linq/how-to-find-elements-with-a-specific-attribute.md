---
title: 'Nasıl yapılır: Belirli bir Özniteliğe Sahip Öğeleri Bulma (XPath-LINQ to XML)'
ms.date: 07/20/2015
ms.assetid: 4bb38d2c-bc7c-4196-8909-aaf41fb86b28
ms.openlocfilehash: 4b625fcccc834f860072ad92587bbfd7ed5ec4ad
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84364779"
---
# <a name="how-to-find-elements-with-a-specific-attribute-xpath-linq-to-xml-visual-basic"></a>Nasıl yapılır: belirli bir özniteliğe sahip öğeleri bulma (XPath-LINQ to XML) (Visual Basic)
Bazen belirli bir özniteliğe sahip olan tüm öğeleri bulmak isteyebilirsiniz. Özniteliğin içeriğiyle ilgili endişeleriniz yok. Bunun yerine, özniteliğinin varlığına göre ' ı seçmek istersiniz.  
  
 XPath ifadesi:  
  
 `./*[@Select]`  
  
## <a name="example"></a>Örnek  
 Aşağıdaki kod yalnızca özniteliği olan öğeleri seçer `Select` .  
  
```vb  
Dim doc As XElement = _
    <Root>  
        <Child1>1</Child1>  
        <Child2 Select='true'>2</Child2>  
        <Child3>3</Child3>  
        <Child4 Select='true'>4</Child4>  
        <Child5>5</Child5>  
    </Root>  
  
' LINQ to XML query  
Dim list1 As IEnumerable(Of XElement) = _  
    From el In doc.Elements() _  
    Where el.@Select <> Nothing _  
    Select el  
  
' XPath expression  
Dim list2 As IEnumerable(Of XElement) = DirectCast(doc.XPathEvaluate _  
    ("./*[@Select]"), IEnumerable).Cast(Of XElement)()  
  
If list1.Count() = list2.Count() And _  
    list1.Intersect(list2).Count() = list1.Count() Then  
    Console.WriteLine("Results are identical")  
Else  
    Console.WriteLine("Results differ")  
End If  
  
For Each el As XElement In list1  
    Console.WriteLine(el)  
Next  
```  
  
 Bu örnek aşağıdaki çıktıyı üretir:  
  
```console
Results are identical  
<Child2 Select="true">2</Child2>  
<Child4 Select="true">4</Child4>  
```  
  
## <a name="see-also"></a>Ayrıca bkz.

- [XPath kullanıcıları için LINQ to XML (Visual Basic)](linq-to-xml-for-xpath-users.md)
