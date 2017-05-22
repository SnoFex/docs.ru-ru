---
title: "Использование структур (руководство по программированию в C#) | Документы Майкрософт"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- structs [C#], using
ms.assetid: cea4a459-9eb9-442b-8d08-490e0797ba38
caps.latest.revision: 28
author: BillWagner
ms.author: wiwagn
translation.priority.ht:
- de-de
- es-es
- fr-fr
- it-it
- ja-jp
- ko-kr
- ru-ru
- zh-cn
- zh-tw
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 8cb5fa79de38294add5cebdd38537636591de126
ms.lasthandoff: 03/13/2017

---
# <a name="using-structs-c-programming-guide"></a>Использование структур (Руководство по программированию на C#)
Тип `struct` подходит для представления простых объектов, таких как `Point`, `Rectangle`и `Color`. Хотя point удобно представить в виде [класса](../../../csharp/language-reference/keywords/class.md) с [автоматически реализуемыми свойствами](../../../csharp/programming-guide/classes-and-structs/auto-implemented-properties.md), в некоторых сценариях [структура](../../../csharp/language-reference/keywords/struct.md) может оказаться более эффективной. Например, при объявлении массива из 1000 объектов `Point` потребуется выделить дополнительную память для ссылки на каждый объект. В этом случае использование структуры будет более экономичным вариантом. Поскольку [!INCLUDE[dnprdnshort](../../../csharp/getting-started/includes/dnprdnshort_md.md)] содержит объект с именем <xref:System.Drawing.Point>, структура в этом примере называется "CoOrds".  
  
 [!code-cs[csProgGuideObjects#1](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/using-structs_1.cs)]  
  
 Определение конструктора по умолчанию (без параметров) для структуры является ошибочным. Кроме того, ошибкой будет и инициализация поля экземпляра в основной части структуры. Члены структуры можно инициализировать только с помощью параметризованного конструктора или путем доступа к членам по отдельности после объявления структуры. Любые закрытые или иным образом недоступные члены можно инициализировать только в конструкторе.  
  
 При создании объекта структуры с помощью оператора [new](../../../csharp/language-reference/keywords/new.md) также вызывается соответствующий конструктор. В отличие от классов, создавать структуры можно без оператора `new` . В этом случае вызов конструктора не выполняется, что способствует более эффективному выделению ресурсов. Однако поля остаются неназначенными и объект нельзя использовать до инициализации всех полей.  
  
 Если структура содержит ссылочный тип в качестве члена, конструктор по умолчанию члена необходимо вызывать явным образом. В противном случае член останется неназначенными и структуру использовать нельзя. (Это приводит к ошибке компилятора CS0171.)  
  
 В отличие от классов, структуры не поддерживают наследование. Структура не может наследовать от другой структуры или класса и не может быть базовой для класса. Однако структуры наследуют от базового класса <xref:System.Object>. Структуры могут реализовывать интерфейсы именно так, как это делают классы.  
  
 Нельзя объявить класс с помощью ключевого слова `struct`. В C# классы и структуры имеют разную семантику. Структура является типом значения, а класс — ссылочным типом. Дополнительные сведения см. в разделе [Типы значений](../../../csharp/language-reference/keywords/value-types.md).  
  
 Если не требуется использовать семантику ссылочного типа, система может более эффективно обрабатывать небольшой класс, объявленный как структура.  
  
## <a name="example-1"></a>Пример 1  
  
### <a name="description"></a>Описание  
 В этом примере показана инициализация `struct` с использованием конструктора по умолчанию и параметризованного конструктора.  
  
### <a name="code"></a>Код  
 [!code-cs[csProgGuideObjects#1](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/using-structs_1.cs)]  
  
 [!code-cs[csProgGuideObjects#2](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/using-structs_2.cs)]  
  
## <a name="example-2"></a>Пример 2  
  
### <a name="description"></a>Описание  
 В этом примере демонстрируется уникальная возможность структур. Здесь создается объект CoOrds без использования оператора `new` . Если заменить слово `struct` на слово `class`, программа не будет скомпилирована.  
  
### <a name="code"></a>Код  
 [!code-cs[csProgGuideObjects#1](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/using-structs_1.cs)]  
  
 [!code-cs[csProgGuideObjects#3](../../../csharp/programming-guide/classes-and-structs/codesnippet/CSharp/using-structs_3.cs)]  
  
## <a name="see-also"></a>См. также  
 [Руководство по программированию на C#](../../../csharp/programming-guide/index.md)   
 [Классы и структуры](../../../csharp/programming-guide/classes-and-structs/index.md)   
 [Структуры](../../../csharp/programming-guide/classes-and-structs/structs.md)
