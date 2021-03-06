---
title: Kullanıcı tanımlı dönüşüm işleçleri - C# başvurusu
description: C#'da özel örtük ve açık tür dönüşümlerini nasıl tanımlayın öğrenin.
ms.date: 07/09/2019
f1_keywords:
- explicit_CSharpKeyword
- implicit_CSharpKeyword
helpviewer_keywords:
- explicit keyword [C#]
- implicit keyword [C#]
- conversion operator [C#]
- user-defined conversion [C#]
ms.openlocfilehash: b59fc27be31f1a38e2a6c3cabd82598933b5ed53
ms.sourcegitcommit: 43cbde34970f5f38f30c43cd63b9c7e2e83717ae
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81121398"
---
# <a name="user-defined-conversion-operators-c-reference"></a>Kullanıcı tanımlı dönüşüm işleçleri (C# başvurusu)

Kullanıcı tanımlı bir tür, başka bir türe özel örtük veya açık bir dönüştürme tanımlayabilir.

Örtük dönüşümler çağrılması için özel sözdizimi gerektirmez ve atamalar ve yöntem çağrıları gibi çeşitli durumlarda oluşabilir. Önceden tanımlanmış C# örtük dönüşümleri her zaman başarılı olur ve hiçbir zaman bir özel durum atmaz. Kullanıcı tanımlı örtük dönüşümler de bu şekilde kullanılmalıdır. Özel bir dönüştürme bir özel durum atabiliyor veya bilgi kaybedebilirse, bunu açık bir dönüştürme olarak tanımlayın.

Kullanıcı tanımlı [dönüşümler, is](type-testing-and-cast.md#is-operator) ve [operatörler tarafından](type-testing-and-cast.md#as-operator) dikkate alınmaz. Kullanıcı tanımlı açık dönüştürme çağırmak için [döküm ifadesini](type-testing-and-cast.md#cast-expression) kullanın.

Örtük `operator` `implicit` veya `explicit` açık bir dönüştürmeyi tanımlamak için anahtar kelimeleri ve veya anahtar kelimeleri kullanın. Dönüştürmeyi tanımlayan tür, kaynak türü veya bu dönüşümün hedef türü olmalıdır. İki kullanıcı tanımlı tür arasında bir dönüşüm iki türden birinde tanımlanabilir.

Aşağıdaki örnek, örtük ve açık bir dönüştürmenin nasıl tanımlanabildiğini gösterir:

[!code-csharp[implicit an explicit conversions](snippets/UserDefinedConversions.cs)]

Önceden tanımlanmış `operator` bir C# işlecinin aşırı yüklenmesi için de anahtar kelime yi kullanırsınız. Daha fazla bilgi için operatör [aşırı yüklemesi'ne](operator-overloading.md)bakın.

## <a name="c-language-specification"></a>C# dili belirtimi

Daha fazla bilgi için [C# dil belirtiminin](~/_csharplang/spec/introduction.md)aşağıdaki bölümlerine bakın:

- [Dönüşüm operatörleri](~/_csharplang/spec/classes.md#conversion-operators)
- [Kullanıcı tanımlı dönüştürmeler](~/_csharplang/spec/conversions.md#user-defined-conversions)
- [Örtülü dönüşümler](~/_csharplang/spec/conversions.md#implicit-conversions)
- [Açık dönüşümler](~/_csharplang/spec/conversions.md#explicit-conversions)

## <a name="see-also"></a>Ayrıca bkz.

- [C# başvurusu](../index.md)
- [C# işleçleri](index.md)
- [İşleç aşırı yüklemesi](operator-overloading.md)
- [Tür testi ve atama işleçleri](type-testing-and-cast.md)
- [Döküm ve tür dönüştürme](../../programming-guide/types/casting-and-type-conversions.md)
- [Tasarım yönergeleri - Dönüşüm operatörleri](../../../standard/design-guidelines/operator-overloads.md#conversion-operators)
- [C'de zincirlenmiş kullanıcı tanımlı açık dönüşümler #](https://docs.microsoft.com/archive/blogs/ericlippert/chained-user-defined-explicit-conversions-in-c)
