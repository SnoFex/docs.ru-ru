---
title: "out (универсальный модификатор) (справочник по C#) | Документы Майкрософт"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- covariance, out keyword [C#]
- out keyword [C#]
ms.assetid: f8c20dec-a8bc-426a-9882-4076b1db1e00
caps.latest.revision: 15
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
ms.openlocfilehash: a5b488eab5966a556b3e3c91ae8c748d11e61367
ms.lasthandoff: 03/13/2017

---
# <a name="out-generic-modifier-c-reference"></a>out (универсальный модификатор) (Справочник по C#)
Для параметров универсального типа ключевое слово `out` указывает, что параметр типа является ковариантным. Ключевое слово `out` может применяться в универсальных интерфейсах и делегатах.  
  
 Ковариация позволяет использовать производные типы со степенью наследования больше, нежели у типа, заданного универсальным параметром. Благодаря этому можно осуществлять неявное преобразование классов, реализующих вариантные интерфейсы, и неявное преобразование типов делегатов. Ковариация и контравариантность поддерживаются для ссылочных типов, но не для типов значений.  
  
 Интерфейс с параметром ковариантного типа позволяет своим методам возвращать аргументы производных типов, степень наследования у которых больше, чем у параметра типа. Например, поскольку в .NET Framework 4 в <xref:System.Collections.Generic.IEnumerable%601> тип T является ковариантным, можно назначить объект типа `IEnumerabe(Of String)` объекту типа `IEnumerable(Of Object)` без применения каких-либо специальных методов преобразования.  
  
 Ковариантный делегат может быть назначен другому делегату того же типа, но с производным параметром универсального типа большей степени.  
  
 Дополнительные сведения см. в разделе [Ковариация и контравариантность](http://msdn.microsoft.com/library/a58cc086-276f-4f91-a366-85b7f95f38b8).  
  
## <a name="example"></a>Пример  
 В следующем примере показано, как объявить, расширить и реализовать ковариантный универсальный интерфейс. В нем также показано, как использовать неявное преобразование для классов, реализующих ковариантный интерфейс.  
  
 [!code-cs[csVarianceKeywords#3](../../../csharp/language-reference/keywords/codesnippet/CSharp/out-generic-modifier_1.cs)]  
  
 В универсальном интерфейсе параметр типа может быть объявлен ковариантным, если он удовлетворяет следующим условиям:  
  
-   Параметр типа используется только в качестве типа значения, возвращаемого методами интерфейса, и не используется в качестве типа аргументов метода.  
  
    > [!NOTE]
    >  Существует одно исключение из данного правила. Если в ковариантном интерфейсе в качестве параметра метода используется контравариантный универсальный делегат, ковариантный тип можно использовать в качестве параметра универсального типа для этого делегата. Дополнительные сведения о ковариантных и контравариантных универсальных методах-делегатах см. в разделе [Вариативность в делегатах](http://msdn.microsoft.com/library/e3b98197-6c5b-4e55-9c6e-9739b60645ca) и [Использование вариативности в универсальных методах-делегатах Func и Action](http://msdn.microsoft.com/library/e69c4f39-09aa-4c6d-a752-08cc767d8290).  
  
-   Параметр типа не используется в качестве универсального ограничения для методов интерфейса.  
  
## <a name="example"></a>Пример  
 В следующем примере кода показано, как объявить ковариантный универсальный метод-делегат. В нем также показано, как выполнить неявное преобразование типов делегатов.  
  
 [!code-cs[csVarianceKeywords#4](../../../csharp/language-reference/keywords/codesnippet/CSharp/out-generic-modifier_2.cs)]  
  
 В универсальном методе-делегате тип может быть объявлен ковариантным, если он используется только как тип значения, возвращаемого методом, и не используется для аргументов метода.  
  
## <a name="c-language-specification"></a>Спецификация языка C#  
 [!INCLUDE[CSharplangspec](../../../csharp/language-reference/keywords/includes/csharplangspec_md.md)]  
  
## <a name="see-also"></a>См. также  
 [Вариативность в универсальных интерфейсах](http://msdn.microsoft.com/library/e14322da-1db3-42f2-9a67-397daddd6b6a)   
 [in](../../../csharp/language-reference/keywords/in-generic-modifier.md)   
 [Модификаторы](../../../csharp/language-reference/keywords/modifiers.md)
