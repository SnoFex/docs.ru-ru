---
title: "Совместимость версий в .NET Framework | Документы Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
helpviewer_keywords:
- .NET Framework, version compatibility
- .NET Framework 4.5, compatibility with earlier versions
- .NET Framework versions, compatibility
ms.assetid: 2f25e522-456a-48c3-8a53-e5f39275649f
caps.latest.revision: 35
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: 09f682d9c3a1cf5d42bba878676d84b9328a1a81
ms.contentlocale: ru-ru
ms.lasthandoff: 04/18/2017

---
# <a name="version-compatibility-in-the-net-framework"></a>Совместимость версий в .NET Framework
Обратная совместимость означает, что приложение, разработанное для конкретной версии платформы, будет запускаться и в более поздних версиях этой платформы. В платформе .NET Framework в максимально возможной степени обеспечивается обратная совместимость: исходный код, написанный для одной версии платформы .NET Framework, должен компилироваться в более поздних версиях этой платформы, а двоичные файлы, работающие в одной версии платформы .NET Framework, должны точно так же работать в более поздних версиях этой платформы.  
  
<a name="Apps"></a>   
## <a name="version-compatibility-for-apps"></a>Совместимость версий приложений  
 По умолчанию приложение запускается в той версии платформы .NET Framework, для которой оно было создано. Если эта версия отсутствует и в файле конфигурации приложения не определены поддерживаемые версии, может произойти ошибка инициализации .NET Framework. В этом случае попытка запустить приложение завершится сбоем.  
  
 Чтобы определить конкретные версии, в которых запускается приложение, добавьте в файл конфигурации этого приложения один или несколько элементов [\<supportedRuntime>](../../../docs/framework/configure-apps/file-schema/startup/supportedruntime-element.md). Каждый элемент `<supportedRuntime>` определяет поддерживаемую версию среды выполнения. При этом первый элемент указывает наиболее предпочтительную версию, а последний элемент — наименее предпочтительную версию.  
  
```xml  
  
<configuration>  
   <startup>  
      <supportedRuntime version="v2.0.50727" />  
      <supportedRuntime version="v4.0" />  
   </startup>  
</configuration>  
  
```  
  
 Дополнительные сведения см. в разделе [Практическое руководство. Настройка в приложении поддержки .NET Framework 4 или 4](../../../docs/framework/migration-guide/how-to-configure-an-app-to-support-net-framework-4-or-4-5.md).  
  
## <a name="version-compatibility-for-components"></a>Совместимость версий компонентов  
 Приложение может определять версию платформы .NET Framework, в которой оно запускается, однако компонент не может. Компоненты и библиотеки классов загружаются в контексте конкретного приложения и, следовательно, автоматически запускаются в той версии .NET Framework, в которой запускается это приложение.  
  
 Из-за этого ограничения гарантии совместимости особенно важны для компонентов. Начиная с .NET Framework 4, можно указывать, в какой степени компонент должен оставаться совместимым в различных версиях, применив к этому компоненту атрибут <xref:System.Runtime.Versioning.ComponentGuaranteesAttribute?displayProperty=fullName>. Этот атрибут может использоваться разными средствами для обнаружения возможных нарушений гарантии совместимости в будущих версиях компонента.  
  
## <a name="backward-compatibility-and-the-net-framework-45"></a>Обратная совместимость и .NET Framework 4.5  
 Платформа .NET Framework 4.5 и ее доработанные выпуски (4.5.1, 4.5.2, 4.6, 4.6.1, 4.6.2 и 4.7) обратно совместимы с приложениями, созданными с помощью более ранних версий .NET Framework. Иными словами, приложения и компоненты, созданные с использованием предыдущих версий платформы .NET Framework, будут без внесения изменений работать в .NET Framework 4.5. Однако по умолчанию приложения выполняются в той версии среды CLR, для которой они были разработаны, поэтому, чтобы обеспечить возможность выполнения приложения в .NET Framework 4.5, может потребоваться предоставить файл конфигурации. Дополнительные сведения см. в разделе [Совместимость версий для приложений](#Apps) выше.  
  
 На практике эту совместимость можно нарушить на первый взгляд несущественными изменениями в платформе .NET Framework и изменениями в методах программирования. Например, улучшения в производительности в платформе .NET Framework 4.5 могут привести к состоянию гонки, которого не было в предыдущих версиях. Следует также иметь в виду, что такие действия, как использование жестко запрограммированного пути к сборкам .NET Framework, сравнение на равенство с конкретной версией платформы .NET Framework и получение значения частного поля с помощью отражения, нарушают обратную совместимость. Кроме того, каждая версия платформы .NET Framework содержит исправления ошибок и изменения, связанные с безопасностью, которые могут влиять на совместимость некоторых приложений и компонентов.  
  
 Если приложение или компонент не работает в .NET Framework 4.5 (и в доработанных выпусках [!INCLUDE[net_v451](../../../includes/net-v451-md.md)] 4.5.2, 4.6, 4.6.1, 4.6.2 или 4.7) ожидаемым образом, воспользуйтесь следующими контрольными списками.  
  
-   Просмотрите следующие разделы на предмет каких-либо изменений, которые могут повлиять на ваше приложение, и используйте описанный обходной путь:  
  
    -   [Проблемы при миграции на .NET Framework 4](http://go.microsoft.com/fwlink/p/?LinkId=248212)  
  
    -   [Совместимость приложений в 4.5](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-5.md)  
  
    -   [Совместимость приложений в 4.5.1](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-5-1.md)  
  
    -   [Совместимость приложений в 4.5.2](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-5-2.md)  
  
    -   [Совместимость приложений в 4.6](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-6.md)  
  
    -   [Совместимость приложений в 4.6.1](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-6-1.md)  
  
    -   [Совместимость приложений в 4.6.2](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-6-2.md)  

    - [Совместимость приложений в 4.7](../../../docs/framework/migration-guide/application-compatibility-in-the-net-framework-4-6-2.md)
       
-   Если приложение было разработано с помощью .NET Framework 1.1, просмотрите также следующие разделы:  
  
    -   [Изменения в .NET Framework 2.0](http://go.microsoft.com/fwlink/?LinkID=125263)  
  
    -   [Изменения в .NET Framework 3.5 SP1](http://go.microsoft.com/fwlink/?LinkId=186989)  
  
-   Если вы перекомпилируете существующий исходный код для запуска в платформе .NET Framework 4.5 (или ее доработанных выпусках) или разрабатываете новую версию приложения или компонента для запуска в [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] на основе существующей базы исходного кода, просмотрите раздел [Что устарело в .NET Framework](../../../docs/framework/whats-new/whats-obsolete.md) на предмет устаревших типов и членов и используйте описанный обходной путь. (Скомпилированный ранее код будет продолжать работать с типами и членами, которые отмечены как устаревшие.)  
  
-   Если обнаруживается, что изменение в [!INCLUDE[net_v45](../../../includes/net-v45-md.md)] нарушило работу приложения, обратитесь к разделу [Схема параметров среды выполнения](../../../docs/framework/configure-apps/file-schema/runtime/index.md), чтобы определить, можно ли использовать параметры среды выполнения в файле конфигурации приложения для восстановления предыдущего поведения.  
  
-   В случае обнаружения недокументированной проблемы зарегистрируйте ошибку в [Microsoft Connect](http://go.microsoft.com/fwlink/?LinkID=154815) и обратитесь по адресу [netfxcf@microsoft.com](mailto:netfxcf@microsoft.com) с указанием номера ошибки.  
  
## <a name="compatibility-and-side-by-side-execution"></a>Совместимость и параллельное выполнение  
 Если найти подходящий обходной путь для проблемы не удается, помните, что платформа .NET Framework 4.5 (или один из ее доработанных выпусков) работает параллельно с версиями 1.1, 2.0 и 3.5 и представляет собой обновление "на месте", заменяющее версию 4. Для приложений, предназначенных для версий 1.1, 2.0 и 3.5, можно установить на целевом компьютере соответствующую версию .NET Framework и запускать приложение в оптимальной для него среде. Дополнительные сведения о параллельном выполнении см. в разделе [Параллельное выполнение](../../../docs/framework/deployment/side-by-side-execution.md).  
  
## <a name="see-also"></a>См. также  
 [Новые возможности](../../../docs/framework/whats-new/index.md)   
 [Устаревшие классы библиотеки классов](../../../docs/framework/whats-new/whats-obsolete.md)   
 [Совместимость приложений](../../../docs/framework/migration-guide/application-compatibility.md)   
 [Политика жизненного цикла поддержки Microsoft .NET Framework](http://go.microsoft.com/fwlink/p/?LinkId=248212)   
 [Проблемы при миграции на .NET Framework 4](http://go.microsoft.com/fwlink/p/?LinkId=248212)
