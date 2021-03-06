---
title: "Общие атрибуты (C#) | Документы Майкрософт"
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
ms.assetid: 785a0526-6c0e-4599-8c61-ccdc88dd9965
caps.latest.revision: 3
author: BillWagner
ms.author: wiwagn
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Human Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: bafcb0a9a81d97e060acca38b7c0bfca23efdaad
ms.lasthandoff: 03/13/2017

---
# <a name="common-attributes-c"></a>Общие атрибуты (C#)
В этом разделе описываются атрибуты, которые чаще всего используются в программах C#.  
  
-   [Глобальные атрибуты](#Global)  
  
-   [Атрибут Obsolete](#Obsolete)  
  
-   [Атрибут Conditional](#Conditional)  
  
-   [Информационные атрибуты вызывающего объекта](#CallerInfo)  
  
##  <a name="Global"></a> Глобальные атрибуты  
 Большинство атрибутов применяется к определенным элементам языка, таким как классы или методы. Тем не менее некоторые атрибуты являются глобальными — они применяются ко всей сборке или модулю. Например, атрибут <xref:System.Reflection.AssemblyVersionAttribute> можно использовать для встраивания сведений о версии в сборку, например следующим образом:  
  
```csharp  
[assembly: AssemblyVersion("1.0.0.0")]  
```  
  
 Глобальные атрибуты отображаются в исходном коде после любых директив `using` верхнего уровня и перед всеми объявлениями типов, модулей или пространств имен. Глобальные атрибуты могут содержаться в нескольких исходных файлах, однако эти файлы должны быть скомпилированы за один проход компиляции. В проектах C# глобальные атрибуты помещаются в файл AssemblyInfo.cs.  
  
 Атрибуты сборки — это значения, которые предоставляют сведения о сборке. Они делятся на следующие категории:  
  
-   Атрибуты удостоверения сборки  
  
-   Информационные атрибуты  
  
-   Атрибуты манифеста сборки  
  
### <a name="assembly-identity-attributes"></a>Атрибуты удостоверения сборки  
 Три атрибута (со строгим именем, если оно применимо) определяют удостоверение сборки: имя, версию, язык и региональные параметры. Эти атрибуты формируют полное имя сборки и являются обязательными при ссылке на нее в коде. Атрибуты можно использовать для задания версии сборки и языка и региональных параметров. Однако значение имени задается компилятором, в интегрированной среде разработки Visual Studio, в [диалоговом окне сведений о сборке](https://docs.microsoft.com/visualstudio/ide/reference/assembly-information-dialog-box) или в компоновщике сборок (Al.exe) при создании сборки на основе файла, содержащего манифест сборки. Атрибут <xref:System.Reflection.AssemblyFlagsAttribute> указывает, могут ли сосуществовать несколько копий сборки.  
  
 В следующей таблице приведены атрибуты удостоверения.  
  
|Атрибут|Цель|  
|---------------|-------------|  
|<xref:System.Reflection.AssemblyName>|Полностью описывает удостоверение сборки.|  
|<xref:System.Reflection.AssemblyVersionAttribute>|Задает версию сборки.|  
|<xref:System.Reflection.AssemblyCultureAttribute>|Указывает, какой язык и региональные параметры поддерживает сборка.|  
|<xref:System.Reflection.AssemblyFlagsAttribute>|Указывает, поддерживает ли сборка параллельное выполнение на одном компьютере, в одном процессе или в одном домене приложения.|  
  
### <a name="informational-attributes"></a>Информационные атрибуты  
 Информационные атрибуты можно использовать для предоставления дополнительных сведений о компании или продукте в сборке. В следующей таблице приведены информационные атрибуты, определенные в пространстве имен <xref:System.Reflection?displayProperty=fullName>.  
  
|Атрибут|Цель|  
|---------------|-------------|  
|<xref:System.Reflection.AssemblyProductAttribute>|Определяет настраиваемый атрибут, задающий имя продукта для манифеста сборки.|  
|<xref:System.Reflection.AssemblyTrademarkAttribute>|Определяет настраиваемый атрибут, задающий товарный знак для манифеста сборки.|  
|<xref:System.Reflection.AssemblyInformationalVersionAttribute>|Определяет настраиваемый атрибут, задающий информационную версию для манифеста сборки.|  
|<xref:System.Reflection.AssemblyCompanyAttribute>|Определяет настраиваемый атрибут, задающий название организации для манифеста сборки.|  
|<xref:System.Reflection.AssemblyCopyrightAttribute>|Определяет настраиваемый атрибут, задающий уведомление об авторских правах для манифеста сборки.|  
|<xref:System.Reflection.AssemblyFileVersionAttribute>|Дает компилятору указание использовать определенный номер версии для ресурса версии файла Win32.|  
|<xref:System.CLSCompliantAttribute>|Указывает, соответствует ли сборка спецификации CLS.|  
  
### <a name="assembly-manifest-attributes"></a>Атрибуты манифеста сборки  
 Атрибуты манифеста сборки можно использовать для предоставления сведений в манифесте сборки. К ним относится заголовок, описание, псевдоним по умолчанию и конфигурация. В следующей таблице приведены атрибуты манифеста сборки, определенные в пространстве имен <xref:System.Reflection?displayProperty=fullName>.  
  
|Атрибут|Цель|  
|---------------|-------------|  
|<xref:System.Reflection.AssemblyTitleAttribute>|Определяет настраиваемый атрибут, задающий название сборки для манифеста сборки.|  
|<xref:System.Reflection.AssemblyDescriptionAttribute>|Определяет настраиваемый атрибут, задающий описание сборки для манифеста сборки.|  
|<xref:System.Reflection.AssemblyConfigurationAttribute>|Определяет настраиваемый атрибут, задающий конфигурацию сборки для манифеста сборки.|  
|<xref:System.Reflection.AssemblyDefaultAliasAttribute>|Определяет понятный псевдоним по умолчанию для манифеста сборки.|  
  
##  <a name="Obsolete"></a> Атрибут Obsolete  
 Атрибут `Obsolete` помечает сущность программы как нерекомендуемую для использования. Каждый случай использования сущности, помеченной как устаревшая, будет приводить к созданию предупреждения или сообщения об ошибке в зависимости от настройки атрибута. Пример:  
  
```csharp  
[System.Obsolete("use class B")]  
class A  
{  
    public void Method() { }  
}  
class B  
{  
    [System.Obsolete("use NewMethod", true)]  
    public void OldMethod() { }  
    public void NewMethod() { }  
}  
```  
  
 В этом примере атрибут `Obsolete` применяется к классу `A` и к методу `B.OldMethod`. Поскольку для второго аргумента конструктора атрибутов, примененного к `B.OldMethod`, задано значение `true`, этот метод будет вызывать ошибку компилятора, тогда как при использовании класса `A` будет просто создаваться предупреждение. При этом вызов `B.NewMethod` не создает предупреждений или ошибок.  
  
 Строка, предоставленная в качестве первого аргумента конструктору атрибута, будет отображаться как часть предупреждения или ошибки. Например, при использовании с предыдущими определениями следующий код создает два предупреждения и одну ошибку:  
  
```csharp  
// Generates 2 warnings:  
// A a = new A();  
  
// Generate no errors or warnings:  
B b = new B();  
b.NewMethod();  
  
// Generates an error, terminating compilation:  
// b.OldMethod();  
```  
  
 Создается два предупреждения для класса `A`: одно для объявления ссылки на класс, а второе — для конструктора класса.  
  
 Атрибут `Obsolete` может использоваться без аргументов, однако рекомендуется включать пояснение о том, почему соответствующий элемент является устаревшим и что следует использовать вместо него.  
  
 Атрибут `Obsolete` является атрибутом однократного использования и может применяться к любой сущности, допускающей использование атрибутов. `Obsolete` является псевдонимом для <xref:System.ObsoleteAttribute>.  
  
##  <a name="Conditional"></a> Атрибут Conditional  
 Атрибут `Conditional` определяет зависимость выполнения метода от идентификатора предварительной обработки. Атрибут `Conditional` является псевдонимом для <xref:System.Diagnostics.ConditionalAttribute> и может применяться к методу или классу атрибута.  
  
 В этом примере атрибут `Conditional` применяется к методу для включения и отключения отображения диагностической информации для конкретной программы:  
  
```csharp  
#define TRACE_ON  
using System;  
using System.Diagnostics;  
  
public class Trace  
{  
    [Conditional("TRACE_ON")]  
    public static void Msg(string msg)  
    {  
        Console.WriteLine(msg);  
    }  
}  
  
public class ProgramClass  
{  
    static void Main()  
    {  
        Trace.Msg("Now in Main...");  
        Console.WriteLine("Done.");  
    }  
}  
```  
  
 Если идентификатор `TRACE_ON` не определен, выходные данные трассировки не отображаются.  
  
 Атрибут `Conditional` часто используется с идентификатором `DEBUG` для включения функций трассировки и ведения журнала для отладочных сборок (но не сборок выпуска) следующим образом:  
  
```csharp  
[Conditional("DEBUG")]  
static void DebugMethod()  
{  
}  
```  
  
 Когда вызывается метод, помеченный как условный, наличие или отсутствие заданного символа предварительной обработки определяет, включается ли вызов или пропускается. Если этот символ определен, вызов включается; в противном случае вызов пропускается. Использование атрибута `Conditional` — это более понятная, элегантная и менее подверженная ошибкам альтернатива вложению методов в блоки `#if…#endif`, например следующим образом:  
  
```csharp  
#if DEBUG  
    void ConditionalMethod()  
    {  
    }  
#endif  
```  
  
 Условный метод должен быть методом в объявлении класса или структуры и не должен возвращать значение.  
  
### <a name="using-multiple-identifiers"></a>Использование нескольких идентификаторов  
 Если метод содержит несколько атрибутов `Conditional`, вызов метода включается, если определен хотя бы один из условных символов (другими словами, символы логически связаны с помощью оператора ИЛИ). В этом примере наличие `A` или `B` приведет к вызову метода:  
  
```csharp  
[Conditional("A"), Conditional("B")]  
static void DoIfAorB()  
{  
    // ...  
}  
```  
  
 Для получения эффекта логического связывания символов с помощью оператора И можно определить последовательные условные методы. Например, второй метод ниже будет выполняться, только если определены `A` и `B`:  
  
```csharp  
<Conditional("A")>   
Shared Sub DoIfA()  
    DoIfAandB()  
End Sub  
  
<Conditional("B")>   
Shared Sub DoIfAandB()  
    ' Code to execute when both A and B are defined...  
End Sub  
```  
  
### <a name="using-conditional-with-attribute-classes"></a>Использование атрибута Conditional с классами атрибутов  
 Атрибут `Conditional` также может применяться к определению класса атрибута. В этом примере настраиваемый атрибут `Documentation` добавит сведения в метаданные, только если задано значение DEBUG.  
  
```csharp  
[Conditional("DEBUG")]  
public class Documentation : System.Attribute  
{  
    string text;  
  
    public Documentation(string text)  
    {  
        this.text = text;  
    }  
}  
  
class SampleClass  
{  
    // This attribute will only be included if DEBUG is defined.  
    [Documentation("This method displays an integer.")]  
    static void DoWork(int i)  
    {  
        System.Console.WriteLine(i.ToString());  
    }  
}  
```  
  
##  <a name="CallerInfo"></a> Информационные атрибуты вызывающего объекта  
 С помощью информационных атрибутов вызывающего объекта можно получить сведения о вызывающем объекте метода. Можно получить путь к файлу исходного кода, номер строки в исходном коде и имя вызывающего объекта.  
  
 Для получения этих сведений используются атрибуты, которые применяются к необязательным параметрам. Каждый необязательный параметр задает значение по умолчанию. В следующей таблице перечислены информационные атрибуты вызывающего объекта, определенные в пространстве имен <xref:System.Runtime.CompilerServices?displayProperty=fullName>:  
  
|Атрибут|Описание|Тип|  
|---|---|---|  
|<xref:System.Runtime.CompilerServices.CallerFilePathAttribute>|Полный путь исходного файла, содержащего вызывающий объект. Это путь во время компиляции.|`String`|  
|<xref:System.Runtime.CompilerServices.CallerLineNumberAttribute>|Номер строки в исходном файле, из которого вызывается метод.|`Integer`|  
|<xref:System.Runtime.CompilerServices.CallerMemberNameAttribute>|Имя свойства или метода вызывающего объекта. Дополнительные сведения см. в разделе [Сведения о вызывающем объекте (C#)](../../../../csharp/programming-guide/concepts/caller-information.md).|`String`|  
  
 Дополнительные сведения об информационных атрибутах вызывающего объекта см. в разделе [Сведения о вызывающем объекте (C#)](../../../../csharp/programming-guide/concepts/caller-information.md).  
  
## <a name="see-also"></a>См. также  
 <xref:System.Reflection>   
 <xref:System.Attribute>   
 [Руководство по программированию на C#](../../../../csharp/programming-guide/index.md)   
 [Атрибуты](https://msdn.microsoft.com/library/5x6cd29c)   
 [Отражение (C#)](../../../../csharp/programming-guide/concepts/reflection.md)   
 [Обращение к атрибутам с помощью отражения (C#)](../../../../csharp/programming-guide/concepts/attributes/accessing-attributes-by-using-reflection.md)
