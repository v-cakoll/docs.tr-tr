---
title: Yönetilmeyen türler - C# başvurusu
ms.date: 09/06/2019
helpviewer_keywords:
- unmanaged type [C#]
ms.openlocfilehash: 9dd2ab4e044b8a86bfe72a6fcf2481b8e1e80bf4
ms.sourcegitcommit: 2514f4e3655081dcfe1b22470c0c28500f952c42
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/18/2020
ms.locfileid: "79507236"
---
# <a name="unmanaged-types-c-reference"></a>Yönetilmeyen türler (C# başvurusu)

Bir tür, aşağıdaki türlerden herhangi biri yse **yönetilmeyen** bir türdür:

- `sbyte`, `byte` `short`, `ushort` `int`, `uint` `long`, `ulong` `char`, `float` `double`, `decimal`, , , , veya`bool`
- Herhangi bir [enum](enum.md) türü
- Herhangi bir [işaretçi](../../programming-guide/unsafe-code-pointers/pointer-types.md) türü
- Yalnızca yönetilmeyen türlerdeki alanları içeren ve C# 7.3 ve daha önce kullanıcı [tarafından](struct.md) tanımlanan yapıt türü yapılı bir tür değildir (en az bir tür bağımsız değişkeniçeren bir tür)

C# 7.3 ile başlayarak, [ `unmanaged` bir](../../programming-guide/generics/constraints-on-type-parameters.md#unmanaged-constraint) tür parametresi işaretçi olmayan, nullable yönetilemez bir tür olduğunu belirtmek için kısıtlama kullanabilirsiniz.

C# 8.0 ile başlayarak, aşağıdaki örnekte görüldüğü gibi, yalnızca yönetilmeyen türlerde alanlar içeren *yapılı* bir yapı türü de yönetilemez:

[!code-csharp[unmanaged constructed types](snippets/UnmanagedTypes.cs#ProgramExample)]

Genel bir yapı hem yönetilmeyen hem de yönetilmeyen yapılandırılan türlerin kaynağı olabilir. Önceki örnek, genel bir yapı `Coords<T>` tanımlar ve yönetilmeyen yapılandırılan türlere örnekler sunar. Yönetilmeyen bir tür değil `Coords<object>`örneğidir. Yönetilmeyen `object` türdeki alanlara sahip olduğu için yönetilmez. *Tüm* yapılandırılan türlerin yönetilmeyen türler olmasını `unmanaged` istiyorsanız, genel bir yapının tanımındaki kısıtlamayı kullanın:

[!code-csharp[unmanaged constraint in type definition](snippets/UnmanagedTypes.cs#AlwaysUnmanaged)]

## <a name="c-language-specification"></a>C# dili belirtimi

Daha fazla bilgi için [C# dil belirtiminin](~/_csharplang/spec/introduction.md) [Pointer türleri](~/_csharplang/spec/unsafe-code.md#pointer-types) bölümüne bakın.

## <a name="see-also"></a>Ayrıca bkz.

- [C# başvurusu](../index.md)
- [İşaretçi türleri](../../programming-guide/unsafe-code-pointers/pointer-types.md)
- [Bellek ve aralıkla ilgili türler](../../../standard/memory-and-spans/index.md)
- [sizeof işleci](../operators/sizeof.md)
- [stackalloc](../operators/stackalloc.md)
