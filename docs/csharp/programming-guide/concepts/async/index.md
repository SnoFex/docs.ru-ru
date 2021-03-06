---
title: "Асинхронное программирование с использованием ключевых слов Async и Await (C#) | Документация Майкрософт"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-csharp
ms.topic: article
dev_langs:
- CSharp
ms.assetid: 9bcf896a-5826-4189-8c1a-3e35fa08243a
caps.latest.revision: 5
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 931f984b2267d0782cdbad22cb00f7a698de357e
ms.lasthandoff: 03/13/2017

---
# <a name="asynchronous-programming-with-async-and-await-c"></a>Асинхронное программирование с использованием ключевых слов Async и Await (C#)
Асинхронное программирование позволяет избежать появления узких мест производительности и увеличить общую скорость реагирования приложения. Однако традиционные методы создания асинхронных приложений могут оказаться сложными, как в плане написания кода, так и в плане отладки и обслуживания.  
  
 Visual Studio 2012 вводит упрощенный подход асинхронного программирования, который использует асинхронные возможности .NET Framework 4.5 (и более высоких версий) и среды выполнения Windows. Компилятор выполняет сложную работу, которую раньше делал разработчик, при этом логическая структура кода приложения похожа на синхронный код. То есть можно пользоваться всеми преимуществами асинхронного программирования с гораздо меньшими, чем обычно, трудозатратами.  
  
 В этом разделе рассматриваются области и методы использования асинхронного программирования, а также приводятся ссылки на вспомогательные разделы с дополнительными сведениями и примерами.  
  
##  <a name="BKMK_WhentoUseAsynchrony"></a> Async повышает скорость реагирования  
 Асинхронность необходимо использовать при наличии потенциально блокирующих действий — например, когда приложение подключается к Интернету. Доступ к веб-ресурсу иногда осуществляется медленно или с задержкой. Если такое действие блокируется в пределах синхронного процесса, все приложение вынуждено ожидать. В асинхронном процессе приложение может перейти к следующей операции, не зависящей от веб-ресурса, до завершения блокирующей задачи.  
  
 В следующей таблице показаны стандартные области, в которых асинхронное программирование повышает скорость реагирования. Перечисленные интерфейсы API платформы .NET Framework 4.5 и среды выполнения Windows содержат методы, поддерживающие асинхронное программирование.  
  
|Область приложения|Поддерживающие интерфейсы API, которые содержат асинхронные методы|  
|----------------------|------------------------------------------------|  
|Веб-доступ|<xref:System.Net.Http.HttpClient>, [SyndicationClient](http://go.microsoft.com/fwlink/p/?LinkId=259441)|  
|Работа с файлами|[StorageFile](http://go.microsoft.com/fwlink/p/?LinkId=248220), <xref:System.IO.StreamWriter>, <xref:System.IO.StreamReader>, <xref:System.Xml.XmlReader>|  
|Работа с образами|[MediaCapture](http://go.microsoft.com/fwlink/p/?LinkId=261839), [BitmapEncoder](http://go.microsoft.com/fwlink/p/?LinkId=261840), [BitmapDecoder](http://go.microsoft.com/fwlink/p/?LinkId=261841)|  
|Программирование с использованием WCF|[Синхронные и асинхронные операции](http://go.microsoft.com/fwlink/p/?LinkID=192382)|   
  
Асинхронность особенно полезна в приложениях, которые обращаются к потоку пользовательского интерфейса, поскольку все связанные с пользовательским интерфейсом действия обычно используют один поток. В синхронном приложении блокировка одного процесса приводит к блокировке всех процессов. Приложение перестает отвечать, и это выглядит как сбой, а не как ожидание.  
  
 При использовании асинхронных методов приложение продолжает реагировать на действия в пользовательском интерфейсе. Например, можно изменить размер окна или свернуть его, либо закрыть приложение, если не требуется ждать завершения работы.  
  
 Асинхронный подход добавляет эквивалент автоматической передачи в список параметров, выбранных при создании асинхронных операций. То есть разработчик может пользоваться всеми преимуществами традиционного асинхронного программирования, прикладывая гораздо меньше усилий.  
  
##  <a name="BKMK_HowtoWriteanAsyncMethod"></a> Методы Async проще создавать  
 В C# основой асинхронного программирования. являются ключевые слова [async](../../../../csharp/language-reference/keywords/async.md) и [await](../../../../csharp/language-reference/keywords/await.md). Они позволяют использовать ресурсы платформы .NET Framework или среды выполнения Windows для создания асинхронных методов, и это почти так же просто, как создавать синхронные методы. Методы, которые определяются с помощью ключевых слов `async` и `await`, называются асинхронными методами.  
  
 Ниже приводится пример асинхронного метода. Почти все элементы кода должны быть вам знакомы. Комментарии указывают на возможности, добавляемые для создания асинхронности.  
  
 Полный файл примера Windows Presentation Foundation представлен в конце раздела. Также его можно скачать на странице [примера асинхронной работы из руководства по использованию ключевых слов Async и Await](http://go.microsoft.com/fwlink/?LinkID=261549).  
  
```csharp  
// Three things to note in the signature:  
//  - The method has an async modifier.   
//  - The return type is Task or Task<T>. (See "Return Types" section.)  
//    Here, it is Task<int> because the return statement returns an integer.  
//  - The method name ends in "Async."  
async Task<int> AccessTheWebAsync()  
{   
    // You need to add a reference to System.Net.Http to declare client.  
    HttpClient client = new HttpClient();  
  
    // GetStringAsync returns a Task<string>. That means that when you await the  
    // task you'll get a string (urlContents).  
    Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");  
  
    // You can do work here that doesn't rely on the string from GetStringAsync.  
    DoIndependentWork();  
  
    // The await operator suspends AccessTheWebAsync.  
    //  - AccessTheWebAsync can't continue until getStringTask is complete.  
    //  - Meanwhile, control returns to the caller of AccessTheWebAsync.  
    //  - Control resumes here when getStringTask is complete.   
    //  - The await operator then retrieves the string result from getStringTask.  
    string urlContents = await getStringTask;  
  
    // The return statement specifies an integer result.  
    // Any methods that are awaiting AccessTheWebAsync retrieve the length value.  
    return urlContents.Length;  
}  
```  
  
 Если метод `AccessTheWebAsync` не выполняет никакие операции между вызовом метода `GetStringAsync` и его завершением, можно упростить код, описав вызов и ожидание с помощью следующего простого оператора.  
  
```csharp  
string urlContents = await client.GetStringAsync();  
```  
  
 Далее поясняется, почему код предыдущего примера является асинхронным методом.  
  
-   Сигнатура метода включает модификатор `async`.  
  
-   Имя асинхронного метода, как правило, оканчивается суффиксом Async.  
  
-   Возвращаемое значение имеет один из следующих типов:  
  
    -   <xref:System.Threading.Tasks.Task%601>, если метод имеет оператор Return с операндом типа TResult.  
  
    -   <xref:System.Threading.Tasks.Task>, если метод не имеет оператора Return или имеет оператор Return без операнда.  
  
    -   `Void`, если вы создаете асинхронный обработчик событий.  
  
     Дополнительные сведения см. ниже в подразделе "типы возвращаемого значения и параметры".  
  
-   Метод обычно содержит по крайней мере одно выражение await, отмечающее точку, в которой метод не может выполняться до завершения асинхронной операции. На это время метод приостанавливается и управление возвращается к вызывающему объекту метода. В следующем подразделе показано, что происходит в точке приостановки.  
  
 В асинхронных методах с помощью соответствующих ключевых слов и типов указывается, что требуется сделать, а компилятор выполняет остальные задачи, в том числе отслеживает обязательные действия, когда управление возвращается в точку await в приостановленном методе. Некоторые программные процессы, такие как циклы и обработка исключений, сложно добавлять в традиционный асинхронный код. В асинхронном методе эти элементы пишутся почти так же, как в синхронном решении, что очень удобно.  
  
 Дополнительные сведения об асинхронности в предыдущих версиях платформы .NET Framework, см. в статье [TPL and Traditional .NET Framework Asynchronous Programming](http://msdn.microsoft.com/library/e7b31170-a156-433f-9f26-b1fc7cd1776f) (Библиотека параллельных задач и традиционное асинхронное программирование в .NET Framework).  
  
##  <a name="BKMK_WhatHappensUnderstandinganAsyncMethod"></a>Что происходит в методе Async  
 В асинхронном программировании важнее всего понимать, как поток управления перемещается из метода в метод. Следующая схема описывает этот процесс.  
  
 ![Трассировка асинхронной программы](../../../../csharp/programming-guide/concepts/async/media/navigationtrace.png "NavigationTrace")  
  
 Числа в схеме соответствуют следующим шагам.  
  
1.  Обработчик событий вызывает асинхронный метод `AccessTheWebAsync` и ожидает его.  
  
2.  `AccessTheWebAsync` создает экземпляр <xref:System.Net.Http.HttpClient> и вызывает асинхронный метод <xref:System.Net.Http.HttpClient.GetStringAsync%2A>, чтобы загрузить содержимое веб-сайта в формате строки.  
  
3.  В `GetStringAsync` происходит событие, которое приостанавливает ход выполнения. Например, методу необходимо подождать завершения загрузки или произошло другое блокирующее действие. Чтобы избежать блокировки ресурсов, `GetStringAsync` передает управление вызывающему объекту `AccessTheWebAsync`.  
  
     `GetStringAsync` возвращает <xref:System.Threading.Tasks.Task%601>, где `TResult` является строкой, а `AccessTheWebAsync` присваивает задачу переменной `getStringTask`. Задача представляет собой непрерывный процесс для вызова `GetStringAsync` с обязательством создать фактическое значение строки, когда работа будет завершена.  
  
4.  Поскольку значение из процесса `getStringTask` еще не получено, метод `AccessTheWebAsync` может перейти к другим операциям, не зависящим от конечного результата`GetStringAsync`. Эти операции представлены вызовом синхронного метода `DoIndependentWork`.  
  
5.  `DoIndependentWork` — это синхронный метод, который выполняет свой код и возвращает управление вызывающему объекту.  
  
6.  Метод `AccessTheWebAsync` выполнил все операции, для которых не требуется результат процесса `getStringTask`. Далее метод `AccessTheWebAsync` должен вычислить длину загруженной строки и возвратить ее, но не может этого сделать, пока нет строки.  
  
     Поэтому `AccessTheWebAsync` использует оператор await, чтобы приостановить свою работу и передать управление методу, вызвавшему `AccessTheWebAsync`. `AccessTheWebAsync` возвращает вызывающему объекту `Task<int>`. Задача представляет собой обещание создать целочисленный результат, являющийся длиной загруженной строки.  
  
    > [!NOTE]
    >  Если метод `GetStringAsync` (и, следовательно, процесс `getStringTask`) выполнен прежде, чем этого дождется `AccessTheWebAsync`, управление остается у метода `AccessTheWebAsync`. На приостановку метода `AccessTheWebAsync` и последующий возврат к нему были бы потрачены лишние ресурсы, если вызываемый асинхронный процесс (`getStringTask`) уже завершен и AccessTheWebSync не нужно ждать окончательного результата.  
  
     Внутри вызывающего объекта (в данном примере — обработчика событий) шаблон обработки повторяется. Вызывающий объект может выполнять другие операции, не зависящие от результата `AccessTheWebAsync`, во время ожидания этого результата, или сразу ожидать результата.   Обработчик событий ожидает `AccessTheWebAsync`, а `AccessTheWebAsync` ожидает `GetStringAsync`.  
  
7.  `GetStringAsync` завершается и создает строковый результат. Вызов возвращает строковый результат в метод `GetStringAsync`, но не так, как, возможно, ожидалось. (Помните, что метод уже возвратил задачу в шаге 3.) Вместо этого строковый результат хранится в задаче `getStringTask`, которая представляет собой завершение метода. Оператор await извлекает результат из `getStringTask`. Оператор присваивания назначает извлеченный результат `urlContents`.  
  
8.  Если `AccessTheWebAsync` содержит строковый результат, метод может вычислить длину строки. Затем работа `AccessTheWebAsync` также завершена, и ожидающий обработчик событий может возобновить работу. В полном примере в конце этого раздела видно, что обработчик событий извлекает значение длины и выводит результат.  
  
 Если вы недавно занимаетесь асинхронным программированием, рекомендуем обратить внимание на различия между синхронным и асинхронным поведением. Синхронный метод возвращает управление, когда его работа завершается (шаг 5), тогда как асинхронный метод возвращает значение задачи, когда его работа приостанавливается (шаги 3 и 6). Когда асинхронный метод в конечном счете завершает работу, задача помечается как завершенная и результат, при его наличии, сохраняется в задаче.  
  
 Дополнительные сведения о потоке управления см. в статье [Control Flow in Async Programs (C#)](../../../../csharp/programming-guide/concepts/async/control-flow-in-async-programs.md) (Поток управления в асинхронных программах на C#).  
  
##  <a name="BKMK_APIAsyncMethods"></a> Методы Async API  
 Где же найти методы для асинхронного программирования (такие как `GetStringAsync`)? Платформа .NET Framework 4.5 или более поздней версии содержит большое количество элементов, совместимых с `async` и `await`. Они содержат суффикс Async в своем имени и возвращают тип <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>. Например, класс `System.IO.Stream` содержит такие методы, как <xref:System.IO.Stream.CopyToAsync%2A>, <xref:System.IO.Stream.ReadAsync%2A> и <xref:System.IO.Stream.WriteAsync%2A>, а также синхронные методы <xref:System.IO.Stream.CopyTo%2A>, <xref:System.IO.Stream.Read%2A> и <xref:System.IO.Stream.Write%2A>.  
  
 Среда выполнения Windows также содержит множество методов, которые можно использовать в сочетании с `async` и `await` в приложениях Windows. Дополнительные сведения и примеры методов см. в статьях [Краткое руководство: вызов асинхронных API в C# и Visual Basic](http://go.microsoft.com/fwlink/?LinkId=248545), [Асинхронное программирование (приложения среды выполнения Windows)](http://go.microsoft.com/fwlink/?LinkId=259592) и [WhenAny. Связывание .NET Framework и среды выполнения Windows (C# и Visual Basic)](https://msdn.microsoft.com/library/jj635140(v=vs.120).aspx).  
  
##  <a name="BKMK_Threads"></a> Потоки  
 Асинхронные методы используются для неблокирующих операций. Выражение `await` в асинхронном методе не блокирует текущий поток на время выполнения ожидаемой задачи. Вместо этого выражение регистрирует остальную часть метода как продолжение и возвращает управление вызывающему объекту асинхронного метода.  
  
 Ключевые слова `async` и `await` не вызывают создания дополнительных потоков. Асинхронные методы не требуют многопоточности, поскольку асинхронный метод не выполняется в собственном потоке. Метод выполняется в текущем контексте синхронизации и использует время в потоке, только когда метод активен. Метод <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName> можно применять для перемещения операций, использующих ресурсы ЦП, в фоновый поток, однако фоновый поток не имеет смысла применять для процесса, который просто ждет результата.  
  
 Асинхронный подход к асинхронному программированию практически по всем параметрам имеет преимущество перед другими подходами. В частности, он эффективнее, чем <xref:System.ComponentModel.BackgroundWorker>, для привязанных к вводу/выводу операций, поскольку имеет более простой код и разработчику не нужно предотвращать состояние гонки. В сочетании с <xref:System.Threading.Tasks.Task.Run%2A?displayProperty=fullName> асинхронное программирование подходит лучше, чем <xref:System.ComponentModel.BackgroundWorker>, для операций, использующих ресурсы ЦП, поскольку отделяет сведения координации о выполнении кода от действий, которые `Task.Run` перемещает в пул потоков.  
  
##  <a name="BKMK_AsyncandAwait"></a> Async и Await  
 Если с помощью модификатора [async](../../../../csharp/language-reference/keywords/async.md) указать, что метод является асинхронным, появятся следующие две возможности.  
  
-   Асинхронный метод сможет использовать [await](../../../../csharp/language-reference/keywords/await.md) для обозначения точек приостановки. Оператор await сообщает компилятору, что асинхронный метод не может выполняться после этой точки до завершения ожидаемого асинхронного процесса. На это время управление возвращается вызывающему объекту асинхронного метода.  
  
     Приостановка асинхронного метода на выражении `await` не считается выходом из метода и блоки `finally` не выполняются.  
  
-   Сам обозначенный асинхронный метод может ожидаться вызывающими его методами.  
  
 Асинхронный метод обычно содержит одно или несколько вхождений оператора `await`, но отсутствие выражений `await` не вызывает ошибок компилятора. Если асинхронный метод не использует оператор `await` для обозначения точки приостановки, метод выполняется как синхронный, независимо от наличия модификатора `async`. При компиляции таких методов выдается предупреждение.  
  
 `async` и `await` являются контекстными ключевыми словами. Дополнительные сведения и примеры см. в следующих разделах:  
  
-   [async](../../../../csharp/language-reference/keywords/async.md)  
  
-   [await](../../../../csharp/language-reference/keywords/await.md)  
  
##  <a name="BKMK_ReturnTypesandParameters"></a> Типы возвращаемого значения и параметры  
 В программировании на платформе .NET Framework асинхронный метод обычно возвращает <xref:System.Threading.Tasks.Task> или <xref:System.Threading.Tasks.Task%601>. Внутри асинхронного метода оператор `await` применяется к задаче, возвращаемой из вызова другого асинхронного метода.  
  
 В качестве возвращаемого типа указывайте <xref:System.Threading.Tasks.Task%601>, если метод содержит оператор [return](../../../../csharp/language-reference/keywords/return.md) с операндом типа `TResult`.  
  
 В качестве возвращаемого типа используется `Task`, если метод не содержит операторов return или содержит оператор return, который не возвращает операнд.  
  
 В следующем примере показано, как объявить и вызвать метод, который возвращает <xref:System.Threading.Tasks.Task%601> или <xref:System.Threading.Tasks.Task>.  
  
```csharp  
// Signature specifies Task<TResult>  
async Task<int> TaskOfTResult_MethodAsync()  
{  
    int hours;  
    // . . .  
    // Return statement specifies an integer result.  
    return hours;  
}  
  
// Calls to TaskOfTResult_MethodAsync  
Task<int> returnedTaskTResult = TaskOfTResult_MethodAsync();  
int intResult = await returnedTaskTResult;  
// or, in a single statement  
int intResult = await TaskOfTResult_MethodAsync();  
  
// Signature specifies Task  
async Task Task_MethodAsync()  
{  
    // . . .  
    // The method has no return statement.    
}  
  
// Calls to Task_MethodAsync  
Task returnedTask = Task_MethodAsync();  
await returnedTask;  
// or, in a single statement  
await Task_MethodAsync();  
  
```  
  
 Каждая возвращенная задача представляет выполняющуюся работу. Задача инкапсулирует информацию о состоянии асинхронного процесса и, в итоге, либо конечный результат процесса, либо исключение, созданное процессом в случае его сбоя.  
  
 Асинхронный метод может иметь тип возвращаемого значения `void`. Возвращаемый тип в основном используется для определения обработчиков событий, где требуется возвращать тип `void`. Асинхронные обработчики событий часто служат в качестве отправной точки для асинхронных программ.  
  
 Асинхронный метод, который имеет тип возвращаемого значения `void`, невозможно ожидать методом await. Вызывающий объект не может перехватывать исключения, которые выдает такой метод.  
  
 Асинхронный метод не может объявлять параметры [ref](../../../../csharp/language-reference/keywords/ref.md) или [out](../../../../csharp/language-reference/keywords/out.md), но может вызывать методы, которые имеют такие параметры.  
  
 Дополнительные сведения и примеры см. в статье [о типах возвращаемых значений асинхронных операций](../../../../csharp/programming-guide/concepts/async/async-return-types.md). Дополнительные сведения о перехвате исключений в асинхронных методах см. в описании механизма [try-catch](../../../../csharp/language-reference/keywords/try-catch.md).  
  
 При программировании в среде выполнения Windows асинхронные интерфейсы API имеют один из следующих возвращаемых типов, которые похожи на задачи:  
  
-   [IAsyncOperation](http://go.microsoft.com/fwlink/p/?LinkId=261896), аналогичный <xref:System.Threading.Tasks.Task%601>  
  
-   [IAsyncAction](http://go.microsoft.com/fwlink/p/?LinkId=261897), аналогичный <xref:System.Threading.Tasks.Task>  
  
-   [IAsyncActionWithProgress](http://go.microsoft.com/fwlink/p/?LinkId=261898)  
  
-   [IAsyncOperationWithProgress](http://go.microsoft.com/fwlink/p/?LinkID=259454)  
  
 Дополнительные сведения и пример см. в статье [Краткое руководство: вызов асинхронных API в C# и Visual Basic](http://go.microsoft.com/fwlink/p/?LinkId=248545).  
  
##  <a name="BKMK_NamingConvention"></a> Соглашение об именовании  
 По соглашению к именам методов, которые имеют модификатор `async`, добавляется суффикс Async.  
  
 Соглашение можно игнорировать в тех случаях, когда событие, базовый класс или контракт интерфейса предлагает другое имя. Например, не следует переименовывать общие обработчики событий, такие как `Button1_Click`.  
  
##  <a name="BKMK_RelatedTopics"></a> Связанные разделы и примеры для Visual Studio  
  
|Заголовок|Описание|Пример|  
|-----------|-----------------|------------|  
|[Walkthrough: Accessing the Web by Using async and await (C#)](../../../../csharp/programming-guide/concepts/async/walkthrough-accessing-the-web-by-using-async-and-await.md) (Пошаговое руководство. Обращение к веб-сайтам с помощью async и await в C#)|Иллюстрирует преобразование синхронного решения WPF в асинхронное. Приложение загружает ряд веб-сайтов.|[Async Sample: Accessing the Web Walkthrough](http://go.microsoft.com/fwlink/p/?LinkID=255191&clcid=0x409) (Пример использования async. Пошаговое руководство по обращению к веб-сайтам).|  
|[How to: Extend the async Walkthrough by Using Task.WhenAll (C#)](../../../../csharp/programming-guide/concepts/async/how-to-extend-the-async-walkthrough-by-using-task-whenall.md) (Практическое руководство. Расширение пошагового руководства по асинхронным процедурам с использованием метода Task.WhenAll в C#)|Добавляет <xref:System.Threading.Tasks.Task.WhenAll%2A?displayProperty=fullName> к предыдущему пошаговому руководству. Использование `WhenAll` запускает все загрузки одновременно.||  
|[How to: Make Multiple Web Requests in Parallel by Using async and await (C#)](../../../../csharp/programming-guide/concepts/async/how-to-make-multiple-web-requests-in-parallel-by-using-async-and-await.md) (Практическое руководство. Параллельное выполнение нескольких веб-запросов в C# с использованием async и await)|Иллюстрирует, как запустить несколько задач одновременно.|[Async Sample: Make Multiple Web Requests in Parallel](http://go.microsoft.com/fwlink/p/?LinkID=254906&clcid=0x409) (Пример использования async. Параллельное выполнение нескольких веб-запросов).|  
|[Async Return Types (C#)](../../../../csharp/programming-guide/concepts/async/async-return-types.md) (Типы возвращаемых значений асинхронных операций в C#)|Иллюстрирует типы, которые могут возвращать асинхронные методы, и поясняет, когда следует использовать каждый из этих типов.||  
|[Control Flow in Async Programs (C#)](../../../../csharp/programming-guide/concepts/async/control-flow-in-async-programs.md) (Поток управления в асинхронных программах C#)|Выполняет подробную трассировку потока управления через последовательность выражений ожидания в асинхронной программе.|[Async Sample: Control Flow in Async Programs](http://go.microsoft.com/fwlink/p/?LinkID=255285&clcid=0x409) (Пример использования async. Поток управления в асинхронных программах)|  
|[Fine-Tuning Your Async Application (C#)](../../../../csharp/programming-guide/concepts/async/fine-tuning-your-async-application.md) (Тонкая настройка асинхронного приложения в C#)|Иллюстрирует добавление следующих функциональных возможностей в асинхронное решение:<br /><br /> -   [Cancel an Async Task or a List of Tasks (C#)](../../../../csharp/programming-guide/concepts/async/cancel-an-async-task-or-a-list-of-tasks.md) (Отмена асинхронной задачи или списка задач в C#)<br />-   [Cancel Async Tasks after a Period of Time (C#)](../../../../csharp/programming-guide/concepts/async/cancel-async-tasks-after-a-period-of-time.md) (Отмена асинхронных задач после определенного периода времени в C#)<br />-   [Cancel Remaining Async Tasks after One Is Complete (C#)](../../../../csharp/programming-guide/concepts/async/cancel-remaining-async-tasks-after-one-is-complete.md) (Отмена оставшихся асинхронных задач после завершения одной из них в C#)<br />-   [Start Multiple Async Tasks and Process Them As They Complete (C#)](../../../../csharp/programming-guide/concepts/async/start-multiple-async-tasks-and-process-them-as-they-complete.md) (Запуск нескольких асинхронных задач и их обработка по мере завершения в C#)|[Async Sample: Fine Tuning Your Application](http://go.microsoft.com/fwlink/p/?LinkID=255046&clcid=0x409) (Пример использования async. Тонкая настройка асинхронного приложения)|  
|[Handling Reentrancy in Async Apps (C#)](../../../../csharp/programming-guide/concepts/async/handling-reentrancy-in-async-apps.md) (Обработка повторного входа в асинхронных приложениях C#)|Иллюстрирует обработку случаев, в которых активная асинхронная операция перезапускается при выполнении.||  
|[WhenAny. Связывание .NET Framework и среды выполнения Windows (C# и Visual Basic)](https://msdn.microsoft.com/library/jj635140(v=vs.120).aspx)|Демонстрирует создание связи между типами задач в платформе .NET Framework и IAsyncOperations в [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)], позволяющей использовать <xref:System.Threading.Tasks.Task.WhenAny%2A> с методом [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)].|[Async Sample: Bridging between .NET and Windows Runtime (AsTask and WhenAny)](http://go.microsoft.com/fwlink/p/?LinkID=260638) (Пример использования async. Связывание между платформой .NET и средой выполнения Windows с помощью AsTask и WhenAny)|  
|Асинхронная отмена. Связывание .NET Framework и среды выполнения Windows|Демонстрирует создание связи между типами задач в платформе .NET Framework и IAsyncOperations в [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)], позволяющей использовать <xref:System.Threading.CancellationTokenSource> с методом [!INCLUDE[wrt](../../../../csharp/includes/wrt_md.md)].|[Async Sample: Bridging between .NET and Windows Runtime (AsTask & Cancellation)](http://go.microsoft.com/fwlink/p/?LinkId=263004) (Пример использования async. Связывание между платформой .NET и средой выполнения Windows с помощью AsTask и Cancellation)|  
|[Using Async for File Access (C#)](../../../../csharp/programming-guide/concepts/async/using-async-for-file-access.md) (Использование метода async для доступа к файлам в C#)|Иллюстрирует преимущества использования асинхронности и ожидания для доступа к файлам.||  
|[Task-based Asynchronous Pattern (TAP)](http://msdn.microsoft.com/library/8cef1fcf-6f9f-417c-b21f-3fd8bac75007) (Асинхронный шаблон, основанный на задачах (TAP))|Описывает новый шаблон для асинхронности в платформе .NET Framework. Шаблон основан на типах <xref:System.Threading.Tasks.Task> и <xref:System.Threading.Tasks.Task%601>.||  
|[Видеоролики об async на канале Channel 9](http://go.microsoft.com/fwlink/p/?LinkID=267466)|Предоставляет ссылки на различные видеоролики об асинхронном программировании.||  
  
##  <a name="BKMK_CompleteExample"></a> Полный пример  
 Ниже представлен код файла MainWindow.xaml.cs из приложения Windows Presentation Foundation (WPF), которое обсуждается в этой статье. Этот пример можно скачать на странице [примера асинхронной работы из руководства по использованию ключевых слов Async и Await](http://go.microsoft.com/fwlink/p/?LinkID=261549).  
  
```csharp  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.Text;  
using System.Threading.Tasks;  
using System.Windows;  
using System.Windows.Controls;  
using System.Windows.Data;  
using System.Windows.Documents;  
using System.Windows.Input;  
using System.Windows.Media;  
using System.Windows.Media.Imaging;  
using System.Windows.Navigation;  
using System.Windows.Shapes;  
  
// Add a using directive and a reference for System.Net.Http;  
using System.Net.Http;  
  
namespace AsyncFirstExample  
{  
    public partial class MainWindow : Window  
    {  
        // Mark the event handler with async so you can use await in it.  
        private async void StartButton_Click(object sender, RoutedEventArgs e)  
        {  
            // Call and await separately.  
            //Task<int> getLengthTask = AccessTheWebAsync();  
            //// You can do independent work here.  
            //int contentLength = await getLengthTask;  
  
            int contentLength = await AccessTheWebAsync();  
  
            resultsTextBox.Text +=  
                String.Format("\r\nLength of the downloaded string: {0}.\r\n", contentLength);  
        }  
  
        // Three things to note in the signature:  
        //  - The method has an async modifier.   
        //  - The return type is Task or Task<T>. (See "Return Types" section.)  
        //    Here, it is Task<int> because the return statement returns an integer.  
        //  - The method name ends in "Async."  
        async Task<int> AccessTheWebAsync()  
        {   
            // You need to add a reference to System.Net.Http to declare client.  
            HttpClient client = new HttpClient();  
  
            // GetStringAsync returns a Task<string>. That means that when you await the  
            // task you'll get a string (urlContents).  
            Task<string> getStringTask = client.GetStringAsync("http://msdn.microsoft.com");  
  
            // You can do work here that doesn't rely on the string from GetStringAsync.  
            DoIndependentWork();  
  
            // The await operator suspends AccessTheWebAsync.  
            //  - AccessTheWebAsync can't continue until getStringTask is complete.  
            //  - Meanwhile, control returns to the caller of AccessTheWebAsync.  
            //  - Control resumes here when getStringTask is complete.   
            //  - The await operator then retrieves the string result from getStringTask.  
            string urlContents = await getStringTask;  
  
            // The return statement specifies an integer result.  
            // Any methods that are awaiting AccessTheWebAsync retrieve the length value.  
            return urlContents.Length;  
        }  
  
        void DoIndependentWork()  
        {  
            resultsTextBox.Text += "Working . . . . . . .\r\n";  
        }  
    }  
}  
  
// Sample Output:  
  
// Working . . . . . . .  
  
// Length of the downloaded string: 41564.  
```  
  
## <a name="see-also"></a>См. также  
 [async](../../../../csharp/language-reference/keywords/async.md)   
 [await](../../../../csharp/language-reference/keywords/await.md)
