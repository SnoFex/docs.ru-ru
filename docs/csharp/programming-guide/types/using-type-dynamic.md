---
title: "Использование типа dynamic (руководство по программированию на C#) | Документы Майкрософт"
ms.date: 2015-07-20
ms.prod: .net
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
helpviewer_keywords:
- dynamic [C#], about dynamic type
- dynamic type [C#]
ms.assetid: 3828989d-c967-4a51-b948-857ebc8fdf26
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
ms.translationtype: Human Translation
ms.sourcegitcommit: fe32676f0e39ed109a68f39584cf41aec5f5ce90
ms.openlocfilehash: 7e2310df174a7c38fafba3fed4e4bd3de4fa377a
ms.contentlocale: ru-ru
ms.lasthandoff: 05/10/2017

---
# <a name="using-type-dynamic-c-programming-guide"></a>Использование типа dynamic (Руководство по программированию на C#)
[!INCLUDE[csharp_dev10_long](../../../csharp/programming-guide/classes-and-structs/includes/csharp_dev10_long_md.md)] добавляет новый тип `dynamic`. Этот тип является статическим, но объект типа `dynamic` обходит проверку статического типа. В большинстве случаев он работает как тип `object`. Во время компиляции предполагается, что элемент, типизированный как `dynamic`, поддерживает любые операции. Это значит, что вам не придется задумываться о том, получает ли объект значение из API COM, из динамического языка, такого как IronPython, из модели DOM HTML, из отражения или из другой части программы. При этом если код недопустимый, ошибки перехватываются во время выполнения.  
  
 Например, если метод экземпляра `exampleMethod1` в следующем коде имеет только один параметр, компилятор определяет, что первый вызов метода `ec.exampleMethod1(10, 4)` недопустим, поскольку содержит два аргумента. Этот вызов приводит к ошибке компилятора. Второй вызов метода `dynamic_ec.exampleMethod1(10, 4)` компилятором не проверяется, поскольку `dynamic_ec` имеет тип `dynamic`. В результате ошибка компилятора не возникает. Тем не менее ошибка не может оставаться незамеченной неограниченное время. Она перехватывается во время выполнения и вызывает исключение времени выполнения.  
  
 [!code-cs[CsProgGuideTypes#50](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_1.cs)]  
  
 [!code-cs[CsProgGuideTypes#56](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_2.cs)]  
  
 Роль компилятора в этих примерах заключается в том, чтобы объединить информацию о том, какие действия каждый оператор будет выполнять с объектом или выражением, типизированным как `dynamic`. Во время выполнения сохраненная информация проверяется, а все недействительные операторы вызывают исключение времени выполнения.  
  
 Результатом большинства динамических операций является сам `dynamic`. Например, при наведении указателя мыши на `testSum` в следующем примере IntelliSense отображает тип **(локальная переменная) dynamic testSum**.  
  
 [!code-cs[CsProgGuideTypes#51](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_3.cs)]  
  
 Операции, в которых результатом не является `dynamic`, включают преобразования из `dynamic` в другой тип, а также вызовы конструктора, которые включают аргументы типа `dynamic`. Например, `testInstance` в следующих объявлениях имеет тип `ExampleClass`, а не `dynamic`.  
  
 [!code-cs[CsProgGuideTypes#52](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_4.cs)]  
  
 Примеры преобразований приводятся в следующем разделе — "Преобразования".  
  
## <a name="conversions"></a>Преобразования  
 Преобразования динамических объектов в другие типы выполнять очень просто. Это позволяет разработчику переключаться между динамическим и нединамическим поведением.  
  
 Любой объект можно преобразовать в динамический тип неявно, как показано в следующих примерах.  
  
 [!code-cs[CsProgGuideTypes#53](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_5.cs)]  
  
 И наоборот, явное преобразование можно динамически применить к любому выражению типа `dynamic`.  
  
 [!code-cs[CsProgGuideTypes#54](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_6.cs)]  
  
## <a name="overload-resolution-with-arguments-of-type-dynamic"></a>Разрешение перегрузки с аргументами типа dynamic  
 Разрешение перегрузки возникают во время выполнения, а не компиляции, если один или несколько аргументов в вызове метода имеют тип `dynamic` либо получатель вызова метода имеет тип `dynamic`. В следующем примере, если единственный доступный метод `exampleMethod2` определен как принимающий аргумент строки, отправка `d1` как аргумента не вызывает ошибку компилятора, но создает исключение времени выполнения. Разрешение перегрузки завершается ошибкой во время выполнения, поскольку `d1` имеет тип времени выполнения `int`, а `exampleMethod2` требуется строка.  
  
 [!code-cs[CsProgGuideTypes#55](../../../csharp/programming-guide/nullable-types/codesnippet/CSharp/using-type-dynamic_7.cs)]  
  
## <a name="dynamic-language-runtime"></a>Среда DLR  
 Среда выполнения динамического языка (DLR) — это новый API в [!INCLUDE[net_v40_short](../../../csharp/programming-guide/types/includes/net_v40_short_md.md)]. Она предоставляет инфраструктуру, которая поддерживает тип `dynamic` в C#, а также реализацию динамических языков программирования, таких как IronPython и IronRuby. Дополнительные сведения о DLR см. в разделе [Общие сведения о среде DLR](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md).  
  
## <a name="com-interop"></a>COM-взаимодействие  
 [!INCLUDE[csharp_dev10_long](../../../csharp/programming-guide/classes-and-structs/includes/csharp_dev10_long_md.md)] включает несколько функций, улучшающих взаимодействие с интерфейсами API COM, такими как API автоматизации Office. К таким усовершенствованиям относятся применение типа `dynamic` и [ именованных и необязательных аргументов](../../../csharp/programming-guide/classes-and-structs/named-and-optional-arguments.md).  
  
 Многие методы COM допускают вариативность в типах аргументов и возвращают тип, назначая типы как `object`. В связи с этим для согласования со строго типизированными переменными в C# необходимо явно приводить значения. При компиляции с использованием параметра [/link (параметры компилятора C#)](../../../csharp/language-reference/compiler-options/link-compiler-option.md) введение типа `dynamic` позволяет обрабатывать вхождения `object` в сигнатурах COM, как если бы они имели тип `dynamic`, и таким образом избежать большей части приведения. Например, следующие инструкции показывают, как получить доступ к ячейке в электронной таблице Microsoft Office Excel с типом `dynamic` и без типа `dynamic`.  
  
 [!code-cs[csOfficeWalkthrough#12](../../../csharp/programming-guide/interop/codesnippet/CSharp/using-type-dynamic_8.cs)]  
  
 [!code-cs[csOfficeWalkthrough#13](../../../csharp/programming-guide/interop/codesnippet/CSharp/using-type-dynamic_9.cs)]  
  
## <a name="related-topics"></a>Связанные разделы  
  
|Заголовок|Описание|  
|-----------|-----------------|  
|[dynamic](../../../csharp/language-reference/keywords/dynamic.md)|Описывает применение ключевого слова `dynamic`.|  
|[Общие сведения о среде DLR](../../../framework/reflection-and-codedom/dynamic-language-runtime-overview.md)|Содержит общие сведения о среде DLR — среде выполнения, которая добавляет в общую среду языковой среды исполнения (CLR) набор служб для динамических языков.|  
|[Пошаговое руководство. Создание и использование динамических объектов](../../../csharp/programming-guide/types/walkthrough-creating-and-using-dynamic-objects.md)|Содержит пошаговые инструкции по созданию пользовательских динамических объектов и проекта, который обращается к библиотеке `IronPython`.|  
|[Практическое руководство. Доступ к объектам взаимодействия Office с помощью функций языка Visual C# 2010](../../../csharp/programming-guide/interop/how-to-access-office-onterop-objects.md)|Содержит сведения о создании проекта с использованием именованных и необязательных аргументов, типа `dynamic` и других усовершенствований, которые упрощают доступ к объектам Office API.|
