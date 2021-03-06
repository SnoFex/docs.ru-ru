---
title: "Выбор класса коллекции | Документы Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology: dotnet-standard
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- last-in-first-out collections
- first-in-first-out collections
- collections [.NET Framework], selecting collection class
- indexed collections
- Collections classes
- grouping data in collections, selecting collection class
ms.assetid: ba049f9a-ce87-4cc4-b319-3f75c8ddac8a
caps.latest.revision: 20
author: mairaw
ms.author: mairaw
manager: wpickett
translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 403a78e3fc1496b91403b3c42494e34d12607b70
ms.lasthandoff: 04/18/2017

---
# <a name="selecting-a-collection-class"></a>Выбор класса коллекции
Внимательно относитесь к выбору класса коллекции. Неправильный тип может ограничить возможности использования коллекции. Как правило, не рекомендуется использовать типы в пространстве имен <xref:System.Collections>, если приложение не создается специально для .NET Framework версии 1.1. Универсальные и параллельные версии коллекций предпочтительны из-за повышенной безопасности типов и других усовершенствований.  
  
 Ответьте на следующие вопросы.  
  
-   Нужен ли вам последовательный список, элемент которого обычно удаляется сразу после извлечения его значения?  
  
    -   Если да, то рассмотрите возможность использования класса <xref:System.Collections.Queue> или универсального класса <xref:System.Collections.Generic.Queue%601>, если требуется обработка по принципу "первым поступил — первым обслужен" (FIFO). Если требуется обработка по принципу "первым поступил — первым обслужен" (FIFO), можно использовать класс <xref:System.Collections.Stack> или универсальный класс <xref:System.Collections.Generic.Stack%601>. Для безопасного доступа из нескольких потоков используйте параллельные версии <xref:System.Collections.Concurrent.ConcurrentQueue%601> и <xref:System.Collections.Concurrent.ConcurrentStack%601>.  
  
    -   Если нет, то следует использовать другие коллекции.  
  
-   Нужен ли доступ к элементам в определенном порядке (FIFO, LIFO) или в произвольным порядке?  
  
    -   Класс <xref:System.Collections.Queue> и <xref:System.Collections.Generic.Queue%601> или универсальный класс <xref:System.Collections.Concurrent.ConcurrentQueue%601> обеспечивают доступ по принципу FIFO. Дополнительные сведения см. в разделе [Преимущества использования потокобезопасных коллекций](../../../docs/standard/collections/thread-safe/when-to-use-a-thread-safe-collection.md).  
  
    -   Класс <xref:System.Collections.Stack> и <xref:System.Collections.Generic.Stack%601> или универсальный класс <xref:System.Collections.Concurrent.ConcurrentStack%601> обеспечивают доступ по принципу LIFO. Дополнительные сведения см. в разделе [Преимущества использования потокобезопасных коллекций](../../../docs/standard/collections/thread-safe/when-to-use-a-thread-safe-collection.md).  
  
    -   Универсальный класс <xref:System.Collections.Generic.LinkedList%601> предоставляет последовательный доступ от начала к концу списка или наоборот.  
  
-   Требуется ли доступ к каждому элементу по индексу?  
  
    -   Классы <xref:System.Collections.ArrayList> и <xref:System.Collections.Specialized.StringCollection>, а также универсальный класс <xref:System.Collections.Generic.List%601> предоставляют доступ к своим элементам с помощью отсчитываемого от нуля индекса элемента.  
  
    -   Классы <xref:System.Collections.Hashtable>, <xref:System.Collections.SortedList>, <xref:System.Collections.Specialized.ListDictionary> и <xref:System.Collections.Specialized.StringDictionary>, а также универсальные классы <xref:System.Collections.Generic.Dictionary%602> и <xref:System.Collections.Generic.SortedDictionary%602> предоставляют доступ к своим элементам с помощью ключа.  
  
    -   Классы <xref:System.Collections.Specialized.NameObjectCollectionBase> и <xref:System.Collections.Specialized.NameValueCollection>, <xref:System.Collections.ObjectModel.KeyedCollection%602>, а также универсальный класс <xref:System.Collections.Generic.SortedList%602> предоставляют доступ к своим элементам либо с помощью отсчитываемого от нуля индекса элемента, либо с помощью ключа элемента.  
  
-   Будет ли каждый элемент содержать только одно значение, сочетание из одного ключа и одного значения или сочетание из одного ключа и нескольких значений?  
  
    -   Одно значение: используйте любую коллекцию, основанную на интерфейсе <xref:System.Collections.IList> или универсальном интерфейсе <xref:System.Collections.Generic.IList%601>.  
  
    -   Один ключ и одно значение: используйте любую коллекцию, основанную на интерфейсе <xref:System.Collections.IDictionary> или универсальном интерфейсе <xref:System.Collections.Generic.IDictionary%602>.  
  
    -   Одно значение с внедренным ключом: используйте универсальный класс <xref:System.Collections.ObjectModel.KeyedCollection%602>.  
  
    -   Один ключ и несколько значений: используйте класс <xref:System.Collections.Specialized.NameValueCollection>.  
  
-   Требуется ли сортировать элементы в порядке, отличном от порядка их поступления?  
  
    -   Класс <xref:System.Collections.Hashtable> сортирует свои элементы по их хэш-кодам.  
  
    -   Класс <xref:System.Collections.SortedList> и универсальные классы <xref:System.Collections.Generic.SortedDictionary%602> и <xref:System.Collections.Generic.SortedList%602> сортируют свои элементы по ключам на основе реализации интерфейса <xref:System.Collections.IComparer> и универсального интерфейса <xref:System.Collections.Generic.IComparer%601>.  
  
    -   <xref:System.Collections.ArrayList> предоставляет метод <xref:System.Collections.ArrayList.Sort%2A> который принимает в качестве параметра реализацию <xref:System.Collections.IComparer>. Его универсальный аналог, универсальный класс <xref:System.Collections.Generic.List%601>, предоставляет метод <xref:System.Collections.Generic.List%601.Sort%2A>, который принимает реализацию универсального интерфейса <xref:System.Collections.Generic.IComparer%601> в качестве параметра.  
  
-   Требуются ли быстрый поиск и получение данных?  
  
    -   <xref:System.Collections.Specialized.ListDictionary> быстрее, чем <xref:System.Collections.Hashtable> для небольших коллекций (10 элементов или меньше). Универсальный класс <xref:System.Collections.Generic.Dictionary%602> обеспечивает более быстрый поиск, чем универсальный класс <xref:System.Collections.Generic.SortedDictionary%602>. Многопоточной реализацией является <xref:System.Collections.Concurrent.ConcurrentDictionary%602>. <xref:System.Collections.Concurrent.ConcurrentBag%601> обеспечивает быструю многопоточную вставку для неупорядоченных данных. Дополнительные сведения об обоих многопоточных типах см. в разделе [Преимущества использования потокобезопасных коллекций](../../../docs/standard/collections/thread-safe/when-to-use-a-thread-safe-collection.md).  
  
-   Требуются ли вам коллекции, принимающие только строки?  
  
    -   <xref:System.Collections.Specialized.StringCollection> (на основе <xref:System.Collections.IList>) и <xref:System.Collections.Specialized.StringDictionary> (на основе <xref:System.Collections.IDictionary>) находятся в пространстве имен <xref:System.Collections.Specialized>.  
  
    -   Кроме того, можно использовать любой из универсальных классов коллекций в пространстве имен <xref:System.Collections.Generic> как строго типизированную строковую коллекцию, указав класс <xref:System.String> в качестве аргумента универсального типа.  
  
## <a name="linq-to-objects-and-plinq"></a>LINQ to Objects и PLINQ  
 Функция LINQ to Objects позволяет использовать запросы LINQ для доступа к объектам в памяти при условии, что тип объекта реализует интерфейс <xref:System.Collections.IEnumerable> или <xref:System.Collections.Generic.IEnumerable%601>. Запросы LINQ обеспечивают общий шаблон для доступа к данным, обычно являются более четкими и удобочитаемыми, чем стандартные циклы `foreach`, а также предоставляют возможности фильтрации, сортировки и группировки. Дополнительные сведения см. в разделе [LINQ to Objects](http://msdn.microsoft.com/library/73cafe73-37cf-46e7-bfa7-97c7eea7ced9).  
  
 Язык PLINQ предоставляет параллельную реализацию языка LINQ to Objects, которая может обеспечить более быстрое выполнение запросов во многих сценариях за счет более эффективного использования многоядерных компьютеров. Дополнительные сведения см. в разделе [Parallel LINQ (PLINQ)](../../../docs/standard/parallel-programming/parallel-linq-plinq.md).  
  
## <a name="see-also"></a>См. также  
 <xref:System.Collections>   
 <xref:System.Collections.Specialized>   
 <xref:System.Collections.Generic>   
 [Потокобезопасные коллекции](../../../docs/standard/collections/thread-safe/index.md)
