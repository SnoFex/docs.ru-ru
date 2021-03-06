---
title: "Устранение рисков: управление версиями продукта | Документы Майкрософт"
ms.custom: 
ms.date: 03/30/2017
ms.prod: .net-framework
ms.reviewer: 
ms.suite: 
ms.technology:
- dotnet-clr
ms.tgt_pltfrm: 
ms.topic: article
ms.assetid: 1c4de9d7-9aba-427a-8f38-0ab9bfb8f85e
caps.latest.revision: 15
author: rpetrusha
ms.author: ronpet
manager: wpickett
ms.translationtype: Human Translation
ms.sourcegitcommit: 9f5b8ebb69c9206ff90b05e748c64d29d82f7a16
ms.openlocfilehash: a6bf9656dee0e6074a8341997abb0e73dc3666f5
ms.contentlocale: ru-ru
ms.lasthandoff: 04/18/2017

---
# <a name="mitigation-product-versioning"></a>Устранение рисков: управление версиями продукта
В [!INCLUDE[net_v46](../../../includes/net-v46-md.md)] и более поздних версиях управление версиями продукта отличается от предыдущих выпусков платформы .NET Framework (.NET Framework 4, 4.5, 4.5.1 и 4.5.2).  
  
## <a name="product-versioning-changes"></a>Изменения управления версиями продукта  
 Ниже приведено подробное описание изменений.  
  
-   Значение записи `Version` в разделе `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full` изменилось на `4.6.`*xxxxx* в .NET Framework 4.6 и ее доработанных версиях и на `4.7.`*xxxxx* в .NET Framework 4.7. В .NET Framework 4.5, 4.5.1 и 4.5.2 использовался формат `4.5.`*xxxxx*.  
  
-   Управление версиями файлов и продукта для файлов .NET Framework было изменено с более ранней схемы управления версиями `4.0.30319.x` на `4.6.X.0` для .NET Framework 4.6 и ее доработанных выпусков, а также на `4.7.X.0` для .NET Framework 4.7 и ее доработанных выпусков. Вы можете увидеть эти новые значения в **свойствах** файла, щелкнув файл правой кнопкой мыши.  
  
-   Атрибуты <xref:System.Reflection.AssemblyFileVersionAttribute> и <xref:System.Reflection.AssemblyInformationalVersionAttribute> управляемых сборок имеют значения <xref:System.Version> в виде `4.6.X.0` для .NET Framework 4.6 и ее доработанных выпусков и `4.7.X.0` для .NET Framework 4.7.  
  
-   В [!INCLUDE[net_v46](../../../includes/net-v46-md.md)], 4.6.1, 4.6.2 и 4.7 свойство <xref:System.Environment.Version%2A?displayProperty=fullName> возвращает исправленную строку версии `4.0.30319.42000`. В .NET Framework 4, 4.5, 4.5.1 и 4.5.2 оно возвращает строки версии в формате `4.0.30319.xxxxx` (например, "4.0.30319.18010"). Обратите внимание, что использование кода приложения, принимающего новые зависимости от свойства <xref:System.Environment.Version%2A?displayProperty=fullName>, не рекомендуется.  
  
### <a name="handling-the-product-versioning-changes"></a>Обработка изменений управления версиями продукта  
 В общем случае приложения для обнаружения таких сведений, как версия среды выполнения .NET Framework и каталог установки, должны использовать следующие рекомендуемые методы:  
  
-   Инструкции для определения версии среды выполнения .NET см. в разделе [Практическое руководство. Определение установленных версий платформы .NET Framework](../../../docs/framework/migration-guide/how-to-determine-which-versions-are-installed.md).  
  
-   Чтобы определить путь установки платформы .NET Framework, используйте значение записи `InstallPath` в ключе `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\NET Framework Setup\NDP\v4\Full`.  
  
    > [!IMPORTANT]
    >  Имя подраздела — `NET Framework Setup`, а не `.NET Framework Setup`.  
  
-   Чтобы определить путь к каталогу общеязыковой среды выполнения .NET Framework, вызовите метод <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetRuntimeDirectory%2A?displayProperty=fullName>.  
  
-   Чтобы получить версию среды CLR, вызовите метод <xref:System.Runtime.InteropServices.RuntimeEnvironment.GetSystemVersion%2A?displayProperty=fullName>.   Для .NET Framework 4 и ее доработанных выпусков (.NET Framework 4.5, 4.5.1, 4.5.2 и [!INCLUDE[net_v46](../../../includes/net-v46-md.md)], 4.6.1, 4.6.2, 4.7) возвращается строка `v4.0.30319`.  
  
## <a name="see-also"></a>См. также  
 [Изменения среды выполнения](../../../docs/framework/migration-guide/runtime-changes-in-the-net-framework-4-6.md)
 
