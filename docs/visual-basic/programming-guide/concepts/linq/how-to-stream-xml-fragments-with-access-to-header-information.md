---
title: "Практическое руководство: поток XML-фрагментов с доступом к сведениям заголовка (Visual Basic) | Документы Microsoft"
ms.custom: 
ms.date: 2015-07-20
ms.prod: .net
ms.reviewer: 
ms.suite: 
ms.technology:
- devlang-visual-basic
ms.tgt_pltfrm: 
ms.topic: article
dev_langs:
- VB
ms.assetid: effd10df-87c4-4d7a-8a9a-1434d829dca5
caps.latest.revision: 3
author: dotnet-bot
ms.author: dotnetcontent
translation.priority.mt:
- cs-cz
- pl-pl
- pt-br
- tr-tr
translationtype: Machine Translation
ms.sourcegitcommit: a06bd2a17f1d6c7308fa6337c866c1ca2e7281c0
ms.openlocfilehash: 299a938cd4b10dbca308685e389fab76656ac20b
ms.lasthandoff: 03/13/2017

---
# <a name="how-to-stream-xml-fragments-with-access-to-header-information-visual-basic"></a>Практическое руководство: поток XML-фрагментов с доступом к сведениям заголовка (Visual Basic)
Иногда приходится считывать достаточно большие XML-файлы и разрабатывать приложения таким образом, чтобы объем памяти, используемой этим приложением, был прогнозируемым. Если попытаться заполнить XML-дерево большим XML-файлом, используемый объем памяти будет пропорционален размеру файла, то есть излишним. Поэтому следует вместо этого использовать потоки.  
  
 Один из вариантов — написать приложение, используя <xref:System.Xml.XmlReader>.</xref:System.Xml.XmlReader> Тем не менее, может потребоваться использовать [!INCLUDE[vbteclinq](../../../../csharp/includes/vbteclinq_md.md)] для запроса XML-дерева. В этом случае можно написать собственный пользовательский метод оси. Дополнительные сведения см. в разделе [как: запись LINQ в методе оси XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/how-to-write-a-linq-to-xml-axis-method.md).  
  
 Чтобы написать собственный метод оси, необходимо написать небольшой метод, который использует <xref:System.Xml.XmlReader>для считывания узлов, пока достигнет одного из узлов, в которых вы заинтересованы.</xref:System.Xml.XmlReader> Затем метод вызывает метод <xref:System.Xml.Linq.XNode.ReadFrom%2A>, который читает из <xref:System.Xml.XmlReader>и создает экземпляр фрагмента XML.</xref:System.Xml.XmlReader> </xref:System.Xml.Linq.XNode.ReadFrom%2A> После этого можно написать запросы в LINQ на основе пользовательского метода оси.  
  
 Использование потоков лучше всего уместно в ситуациях, когда требуется обработать исходный документ только один раз, и элементы можно обрабатывать в порядке их следования в документе. Некоторые стандартные операторы запроса, такие как <xref:System.Linq.Enumerable.OrderBy%2A>, проходят через источник, собирать все данные, сортируют их и выдают первый элемент в последовательности.</xref:System.Linq.Enumerable.OrderBy%2A> Обратите внимание, что, если вы используете оператор запроса, который материализует источник раньше, чем выбирает первый элемент, вы не сохраните малый объем используемой памяти.  
  
## <a name="example"></a>Пример  
 Иногда проблема становится немного интереснее. В следующем XML-документе потребитель пользовательского метода оси должен знать имя клиента, которому принадлежит каждый элемент.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<Root>  
  <Customer>  
    <Name>A. Datum Corporation</Name>  
    <Item>  
      <Key>0001</Key>  
    </Item>  
    <Item>  
      <Key>0002</Key>  
    </Item>  
    <Item>  
      <Key>0003</Key>  
    </Item>  
    <Item>  
      <Key>0004</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Fabrikam, Inc.</Name>  
    <Item>  
      <Key>0005</Key>  
    </Item>  
    <Item>  
      <Key>0006</Key>  
    </Item>  
    <Item>  
      <Key>0007</Key>  
    </Item>  
    <Item>  
      <Key>0008</Key>  
    </Item>  
  </Customer>  
  <Customer>  
    <Name>Southridge Video</Name>  
    <Item>  
      <Key>0009</Key>  
    </Item>  
    <Item>  
      <Key>0010</Key>  
    </Item>  
  </Customer>  
</Root>  
```  
  
 Подход, который используется в данном примере, также отслеживает эти данные о заголовках, сохраняет эти данные о заголовках, а затем строит небольшое XML-дерево, которое содержит и эти данные о заголовках, и сведения, которые вы перечисляете. При использовании этого подхода метод оси позволяет получить это новое небольшое XML-дерево. Тогда запрос имеет доступ к данным о заголовках наряду с вашими сведениями.  
  
 При данном подходе используется малый объем памяти. После того как выданы все данные фрагмента XML, ссылки на предыдущий фрагмент не сохраняются, и он становится пригодным для сборки мусора. Обратите внимание, что этот метод создает в куче много объектов с небольшим периодом жизни.  
  
 В следующем примере показано, как реализовать и использовать пользовательский метод оси, который осуществляет потоковую передачу XML-фрагментов из файла, указанного в URI. Эта пользовательская ось специально написана таким образом, что ожидает документа с элементами `Customer`, `Name` и `Item`, упорядоченными, как в указанном документе `Source.xml`. Это упрощенная реализация. Будет подготовлена более надежная реализация анализа неверных документов.  
  
```vb  
Module Module1  
  
    Sub Main()  
        Dim xmlTree = <Root>  
                          <%=  
                              From el In New StreamCustomerItem("Source.xml")  
                              Let itemKey = CInt(el.<Key>.Value)  
                              Where itemKey >= 3 AndAlso itemKey <= 7  
                              Select <Item>  
                                         <Customer><%= el.Parent.<Name>.Value %></Customer>  
                                         <%= el.<Key> %>  
                                     </Item>  
                          %>  
                      </Root>  
  
        Console.WriteLine(xmlTree)  
    End Sub  
  
End Module  
  
Public Class StreamCustomerItem  
    Implements IEnumerable(Of XElement)  
  
    Private _uri As String  
  
    Public Sub New(ByVal uri As String)  
        _uri = uri  
    End Sub  
  
    Public Function GetEnumerator() As IEnumerator(Of XElement) Implements IEnumerable(Of XElement).GetEnumerator  
        Return New StreamCustomerItemEnumerator(_uri)  
    End Function  
  
    Public Function GetEnumerator1() As IEnumerator Implements IEnumerable.GetEnumerator  
        Return Me.GetEnumerator()  
    End Function  
End Class  
  
Public Class StreamCustomerItemEnumerator  
    Implements IEnumerator(Of XElement)  
  
    Private _current As XElement  
    Private _customerName As String  
    Private _reader As Xml.XmlReader  
    Private _uri As String  
  
    Public Sub New(ByVal uri As String)  
        _uri = uri  
        _reader = Xml.XmlReader.Create(_uri)  
        _reader.MoveToContent()  
    End Sub  
  
    Public ReadOnly Property Current As XElement Implements IEnumerator(Of XElement).Current  
        Get  
            Return _current  
        End Get  
    End Property  
  
    Public ReadOnly Property Current1 As Object Implements IEnumerator.Current  
        Get  
            Return Me.Current  
        End Get  
    End Property  
  
    Public Function MoveNext() As Boolean Implements IEnumerator.MoveNext  
        Dim item As XElement  
        Dim name As XElement  
  
        ' Parse the file, save header information when encountered, and return the  
        ' current Item XElement.  
  
        ' loop through Customer elements  
        While _reader.Read()  
            If _reader.NodeType = Xml.XmlNodeType.Element Then  
                Select Case _reader.Name  
                    Case "Customer"  
                        ' move to Name element  
                        While _reader.Read()  
  
                            If _reader.NodeType = Xml.XmlNodeType.Element AndAlso  
                                _reader.Name = "Name" Then  
  
                                name = TryCast(XElement.ReadFrom(_reader), XElement)  
                                _customerName = If(name IsNot Nothing, name.Value, "")  
                                Exit While  
                            End If  
  
                        End While  
                    Case "Item"  
                        item = TryCast(XElement.ReadFrom(_reader), XElement)  
                        Dim tempRoot = <Root>  
                                           <Name><%= _customerName %></Name>  
                                           <%= item %>  
                                       </Root>  
                        _current = item  
                        Return True  
                End Select  
            End If  
        End While  
  
        Return False  
    End Function  
  
    Public Sub Reset() Implements IEnumerator.Reset  
        _reader = Xml.XmlReader.Create(_uri)  
        _reader.MoveToContent()  
    End Sub  
  
#Region "IDisposable Support"  
    Private disposedValue As Boolean ' To detect redundant calls  
  
    ' IDisposable  
    Protected Overridable Sub Dispose(ByVal disposing As Boolean)  
        If Not Me.disposedValue Then  
            If disposing Then  
                _reader.Close()  
            End If  
        End If  
        Me.disposedValue = True  
    End Sub  
  
    Public Sub Dispose() Implements IDisposable.Dispose  
        Dispose(True)  
        GC.SuppressFinalize(Me)  
    End Sub  
#End Region  
  
End Class  
  
```  
  
 Этот код выводит следующие результаты:  
  
```xml  
<Root>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0003</Key>  
  </Item>  
  <Item>  
    <Customer>A. Datum Corporation</Customer>  
    <Key>0004</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0005</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0006</Key>  
  </Item>  
  <Item>  
    <Customer>Fabrikam, Inc.</Customer>  
    <Key>0007</Key>  
  </Item>  
</Root>  
```  
  
## <a name="see-also"></a>См. также  
 [Дополнительно программированию LINQ to XML (Visual Basic)](../../../../visual-basic/programming-guide/concepts/linq/advanced-linq-to-xml-programming.md)
