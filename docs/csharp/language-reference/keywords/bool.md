---
title: "bool (справочник по C#) | Документы Майкрософт"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
f1_keywords:
- bool_CSharpKeyword
- bool
dev_langs:
- CSharp
helpviewer_keywords:
- bool keyword [C#]
ms.assetid: 551cfe35-2632-4343-af49-33ad12da08e2
caps.latest.revision: 30
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- cs-cz
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- pl-pl
- pt-br
- ru-ru
- tr-tr
- zh-cn
- zh-tw
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: ef8cc9a0584829eeed06e7fc3c2227f0683ef413
ms.lasthandoff: 03/13/2017

---
# <a name="bool-c-reference"></a>bool (Справочник по C#)
Ключевое слово `bool` является псевдонимом <xref:System.Boolean?displayProperty=fullName>. Оно используется для объявления переменных для хранения логических значений, [true](../../../csharp/language-reference/keywords/true.md) и [false](../../../csharp/language-reference/keywords/false.md).  
  
> [!NOTE]
>  Если вам требуется логическая переменная, которая также может принимать значение `null`, используйте `bool?`. Дополнительные сведения см. в разделе [Типы, допускающие значение NULL](../../../csharp/programming-guide/nullable-types/index.md).  
  
## <a name="literals"></a>Литералы  
 Логическое значение можно назначить переменной `bool`. Переменной `bool` также может назначить выражение, результатом которого является `bool`.  
  
 [!code-cs[csrefKeywordsTypes#1](../../../csharp/language-reference/keywords/codesnippet/CSharp/bool_1.cs)]  
  
 Значением переменной `bool` по умолчанию является `false`. Значением переменной `bool?` по умолчанию является `null`.  
  
## <a name="conversions"></a>Преобразования  
 В C++ значение типа `bool` можно преобразовать в значение типа `int`; другими словами, `false` эквивалентно нулю, а `true` эквивалентно ненулевым значениям. В C# не предусмотрено преобразование между типом `bool` и другими типами. Например, следующий оператор `if` недопустим в C#:  
  
 [!code-cs[csrefKeywordsTypes#2](../../../csharp/language-reference/keywords/codesnippet/CSharp/bool_2.cs)]  
  
 Чтобы проверить переменную типа `int`, нужно явно сравнить ее со значением, например с нулем, следующим образом:  
  
 [!code-cs[csrefKeywordsTypes#3](../../../csharp/language-reference/keywords/codesnippet/CSharp/bool_3.cs)]  
  
## <a name="example"></a>Пример  
 В этом примере символ вводится с клавиатуры, и программа проверяет, является ли введенный символ буквой. Если это буква, проверяется ее регистр. Эти проверки выполняются с помощью <xref:System.Char.IsLetter%2A> и <xref:System.Char.IsLower%2A>, которые возвращают тип `bool`:  
  
 [!code-cs[csrefKeywordsTypes#4](../../../csharp/language-reference/keywords/codesnippet/CSharp/bool_4.cs)]  
  
## <a name="c-language-specification"></a>Спецификация языка C#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>См. также  
 [Справочник по C#](../../../csharp/language-reference/index.md)   
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Ключевые слова в C#](../../../csharp/language-reference/keywords/index.md)   
 [Таблица целых типов](../../../csharp/language-reference/keywords/integral-types-table.md)   
 [Таблица встроенных типов](../../../csharp/language-reference/keywords/built-in-types-table.md)   
 [Таблица неявных числовых преобразований](../../../csharp/language-reference/keywords/implicit-numeric-conversions-table.md)   
 [Таблица явных числовых преобразований](../../../csharp/language-reference/keywords/explicit-numeric-conversions-table.md)
