---
title: C# işleçleri-C# başvurusu
ms.date: 04/28/2020
f1_keywords:
- cs.operators
helpviewer_keywords:
- operators [C#]
- operator precedence [C#]
- operator associativity [C#]
- expressions [C#]
ms.assetid: 0301e31f-22ad-49af-ac3c-d5eae7f0ac43
ms.openlocfilehash: 76a9b1efb46af976e59e5f16d3180891ec54ecee
ms.sourcegitcommit: 1cb64b53eb1f253e6a3f53ca9510ef0be1fd06fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82507409"
---
# <a name="c-operators-c-reference"></a>C# işleçleri (C# Başvurusu)

C#, yerleşik türler tarafından desteklenen bir dizi işleç sağlar. Örneğin, [Aritmetik işleçler](arithmetic-operators.md) sayısal Işlenenler ve [Boole mantıksal işleçleri](boolean-logical-operators.md) [bool](../builtin-types/bool.md) işlenenleri olan mantıksal işlemleri gerçekleştirir. Bazı işleçler [aşırı](operator-overloading.md)yüklenebilir. İşleç aşırı yüklemesi ile, Kullanıcı tanımlı bir türün işlenenleri için işleç davranışını belirtebilirsiniz.

Bir [ifadede](../../programming-guide/statements-expressions-operators/expressions.md), işleç önceliği ve ilişkilendirilebilirliği, işlemlerin gerçekleştirileceği sırayı belirlemektir. İşleç önceliği ve ilişkilendirilebilirliği tarafından uygulanan değerlendirmenin sırasını değiştirmek için ayraçları kullanabilirsiniz.

## <a name="operator-precedence"></a>İşleç önceliği

Birden çok işleç içeren bir ifadede, daha yüksek önceliğe sahip işleçler, daha düşük önceliğe sahip işleçlerden önce değerlendirilir. Aşağıdaki örnekte, daha yüksek önceliğe sahip olduğu için çarpma ilk olarak gerçekleştirilir:

```csharp-interactive
var a = 2 + 2 * 2;
Console.WriteLine(a); //  output: 6
```

İşleç önceliğine göre uygulanan değerlendirmenin sırasını değiştirmek için ayraçları kullanın:

```csharp-interactive
var a = (2 + 2) * 2;
Console.WriteLine(a); //  output: 8
```

Aşağıdaki tabloda, en düşük önceliğe göre C# işleçleri listelenmiştir. Her satır içindeki işleçler aynı önceliğe sahiptir.

| İşleçler | Kategori veya ad |
| --------- | ---------------- |
| [x. y](member-access-operators.md#member-access-expression-), [f (x)](member-access-operators.md#invocation-expression-), [a&#91;i&#93;](member-access-operators.md#indexer-operator-), [`x?.y`](member-access-operators.md#null-conditional-operators--and-), [`x?[y]`](member-access-operators.md#null-conditional-operators--and-), [x + +](arithmetic-operators.md#increment-operator-), [x--](arithmetic-operators.md#decrement-operator---), [x!](null-forgiving.md), [New](new-operator.md), [typeof](type-testing-and-cast.md#typeof-operator), [Checked](../keywords/checked.md), [denetimsiz](../keywords/unchecked.md), [Default](default.md), [NameOf](nameof.md), [Delegate](delegate-operator.md), [sizeof](sizeof.md), [stackalloc](stackalloc.md), [x->y](pointer-related-operators.md#pointer-member-access-operator--) | Birincil |
| [+ x](arithmetic-operators.md#unary-plus-and-minus-operators), [-x](arithmetic-operators.md#unary-plus-and-minus-operators), [ \!x](boolean-logical-operators.md#logical-negation-operator-), [~ x](bitwise-and-shift-operators.md#bitwise-complement-operator-), [+ + x](arithmetic-operators.md#increment-operator-), [--x](arithmetic-operators.md#decrement-operator---), [^ x](member-access-operators.md#index-from-end-operator-), [(T) x](type-testing-and-cast.md#cast-expression), [await](await.md), [&x](pointer-related-operators.md#address-of-operator-), [* x](pointer-related-operators.md#pointer-indirection-operator-), [true ve false](true-false-operators.md) | Li |
| [x.. Iz](member-access-operators.md#range-operator-) | Aralık |
| [switch](../../whats-new/csharp-8.md#switch-expressions) | `switch`ifadesini |
| [x * y](arithmetic-operators.md#multiplication-operator-), [x/y](arithmetic-operators.md#division-operator-), [x% y](arithmetic-operators.md#remainder-operator-) | Çarpma|
| [x + y](arithmetic-operators.md#addition-operator-), [x – y](arithmetic-operators.md#subtraction-operator--) | Msal |
| [ \< x \< y](bitwise-and-shift-operators.md#left-shift-operator-), [x >> y](bitwise-and-shift-operators.md#right-shift-operator-) | Shift |
| [x \< y](comparison-operators.md#less-than-operator-), [x > y](comparison-operators.md#greater-than-operator-), [x \<= y](comparison-operators.md#less-than-or-equal-operator-), [x >= y](comparison-operators.md#greater-than-or-equal-operator-), [,](type-testing-and-cast.md#is-operator), [şöyle](type-testing-and-cast.md#as-operator) | İlişkisel ve tür-test |
| [x = = y](equality-operators.md#equality-operator-), [x! = y](equality-operators.md#inequality-operator-) | Eşitlik |
| `x & y` | [Boolean MANTıKSAL ve](boolean-logical-operators.md#logical-and-operator-) veya [BIT düzeyinde mantıksal ve](bitwise-and-shift-operators.md#logical-and-operator-) |
| `x ^ y` | [Boole MANTıKSAL XOR](boolean-logical-operators.md#logical-exclusive-or-operator-) veya [BIT düzeyinde mantıksal XOR](bitwise-and-shift-operators.md#logical-exclusive-or-operator-) |
| <code>x &#124; y</code> | [Boolean MANTıKSAL veya](boolean-logical-operators.md#logical-or-operator-) veya [BIT düzeyinde mantıksal or](bitwise-and-shift-operators.md#logical-or-operator-) |
| [x && y](boolean-logical-operators.md#conditional-logical-and-operator-) | Koşullu VE |
| [x &#124;&#124; y](boolean-logical-operators.md#conditional-logical-or-operator-) | Koşullu VEYA |
| [x?? Iz](null-coalescing-operator.md) | Null birleşim işleci |
| [,? t: f](conditional-operator.md) | Koşullu işleç |
| [x = y](assignment-operator.md), [x + = y](arithmetic-operators.md#compound-assignment), [x-= y](arithmetic-operators.md#compound-assignment), [x * = y](arithmetic-operators.md#compound-assignment), [x/= y](arithmetic-operators.md#compound-assignment), [x% = y](arithmetic-operators.md#compound-assignment), [x &= y](boolean-logical-operators.md#compound-assignment), x [&#124;=](boolean-logical-operators.md#compound-assignment)y, x [^ =](boolean-logical-operators.md#compound-assignment)y, x [ <<=](bitwise-and-shift-operators.md#compound-assignment)y, x [ >>= y](bitwise-and-shift-operators.md#compound-assignment), [x?? = y](null-coalescing-operator.md),[=>](lambda-operator.md) | Atama ve lambda bildirimi |

## <a name="operator-associativity"></a>Operatör ilişkilendirilebilirliği

İşleçler aynı önceliğe sahip olduğunda, işleçlerin ilişkilendirilebilirliği, işlemlerin gerçekleştirilme sırasını belirler:

- *Sola ilişkilendirilebilir* işleçler, soldan sağa doğru sırayla değerlendirilir. [Atama işleçleri](assignment-operator.md) ve [null birleşim işleçleri](null-coalescing-operator.md)hariç, tüm ikili işleçler sola ilişkilendirilebilir. Örneğin, `a + b - c` olarak `(a + b) - c`değerlendirilir.
- *Sağdan ilişkilendirilebilir* işleçler, sağdan sola sırayla değerlendirilir. Atama işleçleri, null birleşim işleçleri ve [koşullu işleç `?:` ](conditional-operator.md) , doğru ilişkilendirilebilir. Örneğin, `x = y = z` olarak `x = (y = z)`değerlendirilir.

İşleç ilişkilendirilebilirliği tarafından uygulanan değerlendirmenin sırasını değiştirmek için ayraçları kullanın:

```csharp-interactive
int a = 13 / 5 / 2;
int b = 13 / (5 / 2);
Console.WriteLine($"a = {a}, b = {b}");  // output: a = 1, b = 6
```

## <a name="operand-evaluation"></a>İşlenen değerlendirmesi

Operatör önceliği ve ilişkilendirilebilirliği ile ilgisiz, bir ifadede işlenen işlenenleri soldan sağa değerlendirilir. Aşağıdaki örneklerde, işleçleri ve işlenenleri değerlendirilen sıra gösterilmektedir:

| İfadeler | Değerlendirme sırası |
| ---------- | ------------------- |
|`a + b`|a, b, +|
|`a + b * c`|a, b, c, *, +|
|`a / b + c * d`|a, b,/, c, d, *, +|
|`a / (b + c) * d`|a, b, c, +,/, d, *|

Genellikle, tüm işleç işlenenleri değerlendirilir. Ancak, bazı işleçler işlenenleri koşullu olarak değerlendirir. Diğer bir deyişle, bu tür bir işlecin en sol işleneninin değeri, (veya hangi) diğer işlenenleri değerlendirileceğini tanımlar. Bu işleçler Koşullu mantıksal [`&&`ve ()](boolean-logical-operators.md#conditional-logical-and-operator-) ve ya da [(`||`)](boolean-logical-operators.md#conditional-logical-or-operator-) işleçleridir, [null birleşim işleçleri `??` ve `??=` ](null-coalescing-operator.md), [null koşullu işleçler `?.` ve `?[]` ](member-access-operators.md#null-conditional-operators--and-)ve [koşullu `?:` ](conditional-operator.md)işleçtir. Daha fazla bilgi için her işlecin açıklamasına bakın.

## <a name="c-language-specification"></a>C# dili belirtimi

Daha fazla bilgi için [C# dil belirtiminin](~/_csharplang/spec/introduction.md) [işleçler](~/_csharplang/spec/expressions.md#operators) bölümüne bakın.

## <a name="see-also"></a>Ayrıca bkz.

- [C# başvurusu](../index.md)
- [İfadeler](../../programming-guide/statements-expressions-operators/expressions.md)
