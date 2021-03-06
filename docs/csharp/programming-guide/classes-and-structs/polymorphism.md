---
title: Çok biçimlilik-C# Programlama Kılavuzu
description: Temel ve türetilmiş sınıflar arasındaki ilişkiyi açıklayan C# gibi nesne odaklı programlama dillerinde önemli bir kavram olan çok biçimlilik hakkında bilgi edinin.
ms.date: 02/08/2020
helpviewer_keywords:
- C# language, polymorphism
- polymorphism [C#]
ms.assetid: 086af969-29a5-4ce8-a993-0b7d53839dab
ms.openlocfilehash: 59b5f5d2d5a8f274845607aeca370c316670bd68
ms.sourcegitcommit: 40de8df14289e1e05b40d6e5c1daabd3c286d70c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/22/2020
ms.locfileid: "86925455"
---
# <a name="polymorphism-c-programming-guide"></a>Çok Biçimlilik (C# Programlama Kılavuzu)

Çok biçimlilik, kapsülleme ve devralma sonrasında nesne odaklı programlama için genellikle üçüncü olarak adlandırılır. Çok biçimlilik, "çoktan şekillendirilmiş" anlamına gelen ve iki ayrı yönü bulunan bir Yunan kelimedir:
  
- Çalışma zamanında, türetilmiş bir sınıfın nesneleri Yöntem parametreleri ve koleksiyonlar ya da diziler gibi yerlerde bir temel sınıfın nesneleri olarak kabul edilebilir. Bu çok biçimlilik gerçekleştiğinde, nesnenin bildirildiği tür artık çalışma zamanı türüyle aynı değildir.
- Temel sınıflar [sanal](../../language-reference/keywords/virtual.md) *yöntemleri*tanımlayabilir ve uygulayabilir ve türetilmiş sınıflar bunları [geçersiz kılabilir](../../language-reference/keywords/override.md) , bu da kendi tanım ve uygulamasını sağlar. Çalışma zamanında, istemci kodu yöntemi çağırdığında, CLR nesnenin çalışma zamanı türünü arar ve sanal yöntemin geçersiz kılmasını çağırır. Kaynak kodunuzda bir temel sınıfta bir yöntemi çağırabilir ve türetilmiş sınıfın yönteminin yürütülmesine neden olabilirsiniz.

Sanal yöntemler, ilişkili nesneler gruplarıyla tek bir şekilde çalışmanıza olanak sağlar. Örneğin, bir kullanıcının çizim yüzeyinde çeşitli şekil türlerini oluşturmalarına olanak tanıyan bir çizim uygulamanız olduğunu varsayalım. Kullanıcı tarafından oluşturulacak belirli şekil türlerini derleme sırasında bilemezsiniz. Bununla birlikte, uygulamanın oluşturulan çeşitli şekil türlerini izlemesi gerekir ve Kullanıcı fare eylemlerine yanıt olarak onları güncelleştirmek zorunda olur. Bu sorunu çözmek için iki temel adımda çok biçimlilik kullanabilirsiniz:

1. Her belirli şekil sınıfının ortak bir temel sınıftan türetildiği bir sınıf hiyerarşisi oluşturun.
1. Temel sınıf yöntemine tek bir çağrı aracılığıyla herhangi bir türetilmiş sınıfta uygun yöntemi çağırmak için bir sanal yöntem kullanın.

İlk olarak, adlı bir temel sınıf `Shape` ve, ve gibi türetilmiş sınıflar `Rectangle` oluşturun `Circle` `Triangle` . `Shape`Sınıfa bir sanal yöntem verin `Draw` ve sınıfın temsil ettiği belirli şekli çizmek için her türetilmiş sınıfta bunu geçersiz kılın. Bir `List<Shape>` nesne oluşturun ve bir `Circle` , `Triangle` , ve ekleyin `Rectangle` .

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#PolymorphismOverview)]

Çizim yüzeyini güncelleştirmek için, listeyi yinelemek ve listedeki her bir nesnede yöntemi çağırmak için bir [foreach](../../language-reference/keywords/foreach-in.md) döngüsü kullanın `Draw` `Shape` . Listedeki her nesnenin tanımlanmış bir türü olsa da `Shape` , çağrılacak çalışma zamanı türü (türetilmiş her sınıfta yöntemin geçersiz kılınan sürümü).

[!code-csharp[Polymorphism overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UsePolymorphism)]

C# dilinde, Kullanıcı tanımlı türler dahil olmak üzere tüm türler öğesinden devraldığı için her tür polimorfik olur <xref:System.Object> .  

## <a name="polymorphism-overview"></a>Çok biçimlilik genel bakış

### <a name="virtual-members"></a>Sanal üyeler

Türetilmiş bir sınıf temel sınıftan devraldığında, temel sınıfın tüm yöntemlerini, alanlarını, özelliklerini ve olaylarını alır. Türetilmiş sınıfın Tasarımcısı, Sanal yöntemlerin davranışı için farklı seçeneklere sahiptir:

- Türetilmiş sınıf, yeni davranışı tanımlayarak temel sınıftaki sanal üyeleri geçersiz kılabilir.
- Türetilmiş sınıf, var olan davranışı koruyarak ve daha fazla türetilmiş sınıfların yöntemi geçersiz kılmasını etkinleştirerek, en yakın temel sınıf yöntemini geçersiz kılmadan devralınır.
- Türetilmiş sınıf, temel sınıf uygulamalarını gizleyen bu üyelerin sanal olmayan yeni uygulamasını tanımlayabilir.

Türetilmiş bir sınıf, yalnızca temel sınıf üyesi [sanal](../../language-reference/keywords/virtual.md) veya [soyut](../../language-reference/keywords/abstract.md)olarak bildirilirse bir temel sınıf üyesini geçersiz kılabilir. Türetilmiş üye, metodun sanal çağrıya katılmayı amaçladığı kesin olarak belirtmek için [override](../../language-reference/keywords/override.md) anahtar sözcüğünü kullanmalıdır. Aşağıdaki kod bir örnek sağlar:

[!code-csharp[Virtual overview](~/samples/snippets/csharp/objectoriented/Inheritance.cs#VirtualMethods)]

Alanlar sanal olamaz; yalnızca Yöntemler, özellikler, olaylar ve Dizin oluşturucular sanal olabilir. Türetilmiş bir sınıf sanal üyeyi geçersiz kıldığında, bu üye, bu sınıfın bir örneği temel sınıfın bir örneği olarak erişildiği zaman bile çağırılır. Aşağıdaki kod bir örnek sağlar:

[!code-csharp[Virtual overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#SnippetTestVirtualMethods)]

Sanal yöntemler ve özellikler, türetilmiş sınıfların bir yöntemin temel sınıf uygulamasını kullanmaya gerek kalmadan bir temel sınıfı genişletmesine imkan tanır. Daha fazla bilgi için bkz. [geçersiz kılma ve yeni anahtar sözcüklerle sürüm oluşturma](./versioning-with-the-override-and-new-keywords.md). Bir arabirim, bir yöntemi veya uygulamasının türetilmiş sınıflara ayrılmakta olduğu yöntemler kümesini tanımlamak için başka bir yol sağlar. Daha fazla bilgi için bkz. [arabirimler](../interfaces/index.md).

### <a name="hide-base-class-members-with-new-members"></a>Temel sınıf üyelerini yeni üyelerle gizle

Türetilmiş sınıfınızın bir temel sınıftaki üye ile aynı ada sahip bir üyeye sahip olmasını istiyorsanız, temel sınıf üyesini gizlemek için [New](../../language-reference/keywords/new-modifier.md) anahtar sözcüğünü kullanabilirsiniz. `new`Anahtar sözcüğü, değiştirilmekte olan bir sınıf üyesinin dönüş türünden önce konur. Aşağıdaki kod bir örnek sağlar:

[!code-csharp[New method overview example](~/samples/snippets/csharp/objectoriented/Inheritance.cs#NewMethods)]

Gizli temel sınıf üyelerine türetilmiş sınıfın örneğini bir temel sınıfın örneğine aktararak istemci kodundan erişilebilir. Örneğin:

[!code-csharp[New method overview usage](~/samples/snippets/csharp/objectoriented/Inheritance.cs#UseNewMethods)]

### <a name="prevent-derived-classes-from-overriding-virtual-members"></a>Türetilmiş sınıfların sanal üyeleri geçersiz kılmasını engelle  

Sanal üyeler, sanal üye ile ilk olarak tarafından tanımlanan sınıf arasında kaç sınıf bildirildiği dikkate almaksızın sanal olarak kalır. Sınıfı `A` bir sanal üye bildiriyorsa ve sınıfından türetilmiş sınıf ve sınıfından türetiliyor, sınıf `B` `A` `C` `B` `C` sanal üyeyi devralır ve sınıfın `B` Bu üye için bir geçersiz kılma olarak bildirilip bildirilmediğine bakılmaksızın onu geçersiz kılabilir. Aşağıdaki kod bir örnek sağlar:

[!code-csharp[Basic hierarchy](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#FirstHierarchy)]

Türetilmiş bir sınıf, bir geçersiz kılma [korumalı](../../language-reference/keywords/sealed.md)olarak bildirerek sanal devralmayı durdurabilir. Devralma durdurulduğunda, `sealed` `override` sınıf üye bildiriminde anahtar sözcükten önce anahtar sözcüğü koyulması gerekir. Aşağıdaki kod bir örnek sağlar:

[!code-csharp[A sealed overridden member](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#SealedOverride)]

Önceki örnekte, yöntemi `DoWork` öğesinden türetilmiş hiçbir sınıf için artık sanal değildir `C` . Türlerine `C` veya türüne saçılması durumunda bile, örnekleri için hala sanal olur `B` `A` . Aşağıdaki örnekte gösterildiği gibi, korumalı Yöntemler, anahtar sözcüğü kullanılarak türetilmiş sınıflar tarafından değiştirilebilir `new` :

[!code-csharp[New method declaration](~/samples/snippets/csharp/objectoriented/Hierarchy.cs#NewDeclaration)]

Bu durumda, `DoWork` `D` türünde bir değişken kullanılarak çağrılırsa `D` , yeni `DoWork` çağırılır. Bir `C` `B` örneğine erişmek için, veya türünde bir değişken `A` kullanılırsa `D` , öğesine yapılan bir çağrı, `DoWork` sanal devralma kurallarını izleyerek bu çağrıları sınıfının uygulamasına yönlendirme `DoWork` `C` işlemini kullanır.

### <a name="access-base-class-virtual-members-from-derived-classes"></a>Türetilmiş sınıflardan temel sınıf sanal üyelerine erişin

Bir yöntemi veya özelliği değiştirilmiş veya geçersiz kılan türetilmiş bir sınıf, anahtar sözcüğünü kullanarak temel sınıftaki yönteme veya özelliğe erişmeye devam edebilir `base` . Aşağıdaki kod bir örnek sağlar:

```csharp
public class Base
{
    public virtual void DoWork() {/*...*/ }
}
public class Derived : Base
{
    public override void DoWork()
    {
        //Perform Derived's work here
        //...
        // Call DoWork on base class
        base.DoWork();
    }
}
```

Daha fazla bilgi için bkz. [Base](../../language-reference/keywords/base.md).

> [!NOTE]
> Sanal üyelerin `base` kendi uygulamalarında bu üyenin temel sınıf uygulamasını çağırmak için kullanılması önerilir. Temel sınıf davranışının oluşmasına izin vermek, türetilmiş sınıfın türetilmiş sınıfa özgü davranış uygulamaya odaklanmalarını sağlar. Temel sınıf uygulama çağrılıp, davranışını temel sınıfın davranışıyla uyumlu hale getirmek için türetilmiş sınıfa kadar olur.

## <a name="in-this-section"></a>Bu bölümde

- [Geçersiz Kılma ve Yeni Anahtar Sözcüklerle Sürüm Oluşturma](./versioning-with-the-override-and-new-keywords.md)
- [Geçersiz Kılmanın ve Yeni Anahtar Sözcüklerin Ne Zaman Kullanılacağını Bilme](./knowing-when-to-use-override-and-new-keywords.md)
- [ToString yöntemini geçersiz kılma](./how-to-override-the-tostring-method.md)

## <a name="see-also"></a>Ayrıca bkz.

- [C# Programlama Kılavuzu](../index.md)
- [Devralma](./inheritance.md)
- [Soyut ve Korumalı Sınıflar ve Sınıf Üyeleri](./abstract-and-sealed-classes-and-class-members.md)
- [Yöntemler](./methods.md)
- [Ekinlikler](../events/index.md)
- [Özellikler](./properties.md)
- [Dizin Oluşturucular](../indexers/index.md)
- [Türler](../types/index.md)
