---
title: "Сравнение и сортировка в коллекциях | Документация Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- sorting data, collections
- IComparable.CompareTo method
- Collections classes
- Equals method
- collections [.NET Framework], comparisons
ms.assetid: 5e4d3b45-97f0-423c-a65f-c492ed40e73b
caps.latest.revision: 11
author: mairaw
ms.author: mairaw
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 0da0bed43cb7871f522b94b134afb164d8ee3ab5
ms.lasthandoff: 04/18/2017

---
# <a name="comparisons-and-sorts-within-collections"></a>Сравнение и сортировка в коллекциях
Классы <xref:System.Collections> выполняют сравнения почти во всех процессах управления коллекциями, будь то поиск элемента для удаления или возвращение значения пары "ключ — значение".  
  
 В коллекциях обычно используется компаратор проверки на равенство и (или) компаратор упорядочения. Для сравнения используются две конструкции.  
  
<a name="BKMK_Checkingforequality"></a>   
## <a name="checking-for-equality"></a>Проверка на равенство  
 Такие методы, как `Contains`, <xref:System.Collections.IList.IndexOf%2A>, <xref:System.Collections.Generic.List%601.LastIndexOf%2A> и `Remove`, используют функцию сравнения равенства для элементов коллекции. Если коллекция является универсальной, то элементы проверяются на равенство согласно следующим правилам.  
  
-   Если тип T реализует универсальный интерфейс <xref:System.IEquatable%601>, функцией сравнения проверки на равенство является метод <xref:System.IEquatable%601.Equals%2A> этого интерфейса.  
  
-   Если тип T не реализует <xref:System.IEquatable%601>, используется <xref:System.Object.Equals%2A?displayProperty=fullName>.  
  
 Кроме того, некоторые перегрузки конструктора для коллекций словаря принимают реализацию <xref:System.Collections.Generic.IEqualityComparer%601>, которая используется для сравнения ключей на равенство. Для примера см. конструктор <xref:System.Collections.Generic.Dictionary%602.%23ctor%2A?displayProperty=fullName>.  
  
<a name="BKMK_Determiningsortorder"></a>   
## <a name="determining-sort-order"></a>Определение порядка сортировки  
 Такие методы, как `BinarySearch` и `Sort`, используют компаратор упорядочения для элементов коллекции. Сравнение может проводиться между элементами коллекции или между элементом и заданным значением. В процессе сравнения объектов существует понятие `default comparer` и `explicit comparer`.  
  
 Для реализации интерфейса **IComparable** функция сравнения по умолчанию использует по крайней мере один из сравниваемых объектов. Интерфейс **IComparable** рекомендуется реализовать во всех классах, используемых в качестве значений в коллекциях списков или в качестве ключей в коллекциях словарей. В универсальной коллекции сравнение на равенство определяется в соответствии со следующими правилами.  
  
-   Если тип T реализует универсальный интерфейс <xref:System.IComparable%601?displayProperty=fullName>, функцией сравнения по умолчанию является метод <xref:System.IComparable%601.CompareTo%28%600%29?displayProperty=fullName> этого интерфейса.  
  
-   Если тип T реализует неуниверсальный интерфейс <xref:System.IComparable?displayProperty=fullName>, функцией сравнения по умолчанию является метод <xref:System.IComparable.CompareTo%28System.Object%29?displayProperty=fullName> этого интерфейса.  
  
-   Если тип T не реализует никакого интерфейса, компаратор по умолчанию отсутствует, а компаратор или делегат сравнения должен быть предоставлен явно.  
  
 Для осуществления явных сравнений некоторые методы принимают реализацию **IComparer** в качестве параметра. Например, метод <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=fullName> принимает реализацию <xref:System.Collections.Generic.IComparer%601?displayProperty=fullName>.  
  
 Текущее значение языка и региональных параметров системы может влиять на сравнения и сортировки в рамках коллекции. По умолчанию сравнения и сортировки в классах **Collections** зависят от языка и региональных параметров. Чтобы игнорировать настройку языка и региональных параметров и таким образом получить согласованные результаты сравнения и сортировки, используйте <xref:System.Globalization.CultureInfo.InvariantCulture%2A> с перегрузками элементов, которые принимают <xref:System.Globalization.CultureInfo>. Дополнительные сведения см. в статьях о [выполнении строковых операций без учета языка и региональных параметров в коллекциях](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-collections.md) и [выполнении строковых операций без учета языка и региональных параметров в массивах](../../../docs/standard/globalization-localization/performing-culture-insensitive-string-operations-in-arrays.md).  
  
<a name="BKMK_Equalityandsortexample"></a>   
## <a name="equality-and-sort-example"></a>Пример сортировки и проверки на равенство  
 В следующем примере демонстрируется реализация <xref:System.IEquatable%601> и <xref:System.IComparable%601> в простом бизнес-объекте. Кроме того, после того как объект сохранится в списке и будет отсортирован, вы увидите, что вызов метода <xref:System.Collections.Generic.List%601.Sort> приведет к использованию функции сравнения по умолчанию для типа `Part`, а метод <xref:System.Collections.Generic.List%601.Sort%28System.Comparison%7B%600%7D%29> будет реализован с помощью анонимного метода.  
  
 [!code-csharp[System.Collections.Generic.List.Sort#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.collections.generic.list.sort/cs/program.cs#1)]
 [!code-vb[System.Collections.Generic.List.Sort#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.collections.generic.list.sort/vb/module1.vb#1)]  
  
## <a name="see-also"></a>См. также  
 <xref:System.Collections.IComparer>   
 <xref:System.IEquatable%601>   
 <xref:System.Collections.Generic.IComparer%601>   
 <xref:System.IComparable>   
 <xref:System.IComparable%601>
