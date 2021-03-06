---
title: XElement Sınıfına Genel Bakış
ms.date: 07/20/2015
ms.assetid: 52331fcd-6023-4d19-b423-7b24f2d86ded
ms.openlocfilehash: a0e50c8a5a14150ee09a328f4dcdd5bc88363621
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84413225"
---
# <a name="xelement-class-overview-visual-basic"></a>XElement sınıfına genel bakış (Visual Basic)
<xref:System.Xml.Linq.XElement>Sınıfı, içindeki temel sınıflardan biridir [!INCLUDE[sqltecxlinq](~/includes/sqltecxlinq-md.md)] . Bir XML öğesini temsil eder. Bu sınıfı, öğeler oluşturmak için kullanabilirsiniz; öğenin içeriğini değiştirme; alt öğeleri ekleyin, değiştirin veya silin; özniteliği bir öğeye ekleyin; veya metin biçimindeki bir öğenin içeriğini seri hale getirme. Ayrıca, ve gibi diğer sınıflarla da birlikte kullanabilirsiniz <xref:System.Xml?displayProperty=nameWithType> <xref:System.Xml.XmlReader> <xref:System.Xml.XmlWriter> <xref:System.Xml.Xsl.XslCompiledTransform> .  
  
## <a name="xelement-functionality"></a>XElement Işlevselliği  
 Bu konu, sınıfının sunduğu işlevselliği açıklar <xref:System.Xml.Linq.XElement> .  
  
### <a name="constructing-xml-trees"></a>XML ağaçları oluşturma  
 Aşağıdakiler dahil olmak üzere çeşitli yollarla XML ağaçları oluşturabilirsiniz:  
  
- Kodda bir XML ağacı oluşturabilirsiniz. Daha fazla bilgi için bkz. [xml ağaçları oluşturma (Visual Basic)](creating-xml-trees.md).  
  
- Bir <xref:System.IO.TextReader> metin dosyası veya bir Web adresi (URL) dahil olmak üzere çeşitli kaynaklardan XML ayrıştırabilirsiniz. Daha fazla bilgi için bkz. [XML 'ı ayrıştırma (Visual Basic)](parsing-xml.md).  
  
- <xref:System.Xml.XmlReader>Ağacı doldurmak için kullanabilirsiniz. Daha fazla bilgi için bkz. <xref:System.Xml.Linq.XNode.ReadFrom%2A>.  
  
- İçine içerik yazabileceği bir modülünüzün varsa, bir <xref:System.Xml.XmlWriter> <xref:System.Xml.Linq.XContainer.CreateWriter%2A> yazıcı oluşturmak, yazıcıyı modüle geçirmek ve ardından <xref:System.Xml.XmlWriter> XML ağacını doldurmak için öğesine yazılan içeriği kullanmak için yöntemini kullanabilirsiniz.  
  
 Ancak, bir XML ağacı oluşturmanın en yaygın yolu aşağıdaki gibidir:  
  
```vb  
Dim contacts As XElement = _  
    <Contacts>  
        <Contact>  
            <Name>Patrick Hines</Name>  
            <Phone>206-555-0144</Phone>  
            <Address>  
                <Street1>123 Main St</Street1>  
                <City>Mercer Island</City>  
                <State>WA</State>  
                <Postal>68042</Postal>  
            </Address>  
        </Contact>  
    </Contacts>  
```  
  
 Bir XML ağacı oluşturmaya yönelik başka bir çok yaygın teknik, aşağıdaki örnekte gösterildiği gibi bir XML ağacını doldurmak için bir LINQ sorgusunun sonuçlarının kullanılmasını içerir:  
  
```vb  
Dim srcTree As XElement = _  
    <Root>  
        <Element>1</Element>  
        <Element>2</Element>  
        <Element>3</Element>  
        <Element>4</Element>  
        <Element>5</Element>  
    </Root>  
Dim xmlTree As XElement = _  
    <Root>  
        <Child>1</Child>  
        <Child>2</Child>  
        <%= From el In srcTree.Elements() _  
            Where el.Value > 2 _  
            Select el %>  
    </Root>  
Console.WriteLine(xmlTree)  
```  
  
 Bu örnek aşağıdaki çıktıyı üretir:  
  
```xml  
<Root>  
  <Child>1</Child>  
  <Child>2</Child>  
  <Element>3</Element>  
  <Element>4</Element>  
  <Element>5</Element>  
</Root>  
```  
  
### <a name="serializing-xml-trees"></a>XML Ağaçlarını Serileştirme  
 XML ağacını bir <xref:System.IO.File> , a veya olarak seri hale getirebilirsiniz <xref:System.IO.TextWriter> <xref:System.Xml.XmlWriter> .  
  
 Daha fazla bilgi için bkz. [XML ağaçlarını serileştirme (Visual Basic)](serializing-xml-trees.md).  
  
### <a name="retrieving-xml-data-via-axis-methods"></a>XML verilerini eksen yöntemleri aracılığıyla alma  
 Öznitelikleri, alt öğeleri, alt öğeleri ve üst öğeleri almak için eksen yöntemlerini kullanabilirsiniz. LINQ sorguları eksen yöntemleri üzerinde çalışır ve bir XML ağacı üzerinde gezinmek ve işlemek için çeşitli esnek ve güçlü yollar sunar.  
  
 Daha fazla bilgi için bkz. [LINQ to XML eksenleri (Visual Basic)](linq-to-xml-axes.md).  
  
### <a name="querying-xml-trees"></a>XML Ağaçlarını Sorgulama  
 XML ağacından veri çıkaran LINQ sorguları yazabilirsiniz.  
  
 Daha fazla bilgi için bkz. [XML ağaçlarını sorgulama (Visual Basic)](querying-xml-trees.md).  
  
### <a name="modifying-xml-trees"></a>XML ağaçlarını değiştirme  
 Bir öğeyi, içeriğini veya özniteliklerini değiştirme gibi çeşitli yollarla değiştirebilirsiniz. Ayrıca bir öğeyi üst öğesinden da kaldırabilirsiniz.  
  
 Daha fazla bilgi için bkz. [XML ağaçlarını değiştirme (LINQ to XML) (Visual Basic)](modifying-xml-trees-linq-to-xml.md).  
  
## <a name="see-also"></a>Ayrıca bkz.

- [LINQ to XML programlamaya genel bakış (Visual Basic)](linq-to-xml-programming-overview.md)
