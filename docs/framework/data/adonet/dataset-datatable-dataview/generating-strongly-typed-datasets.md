---
title: 강력한 형식의 데이터 세트 생성
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 54333cbf-bb43-4314-a7d4-6dc1dd1c44b3
ms.openlocfilehash: f1c1fd77bed700fae8e5a658da8b267120518ca9
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70786298"
---
# <a name="generating-strongly-typed-datasets"></a>강력한 형식의 데이터 세트 생성
Xsd (xml 스키마 정의 언어) 표준을 준수 하는 xml 스키마를 제공 하는 경우 Windows SDK (소프트웨어 개발 <xref:System.Data.DataSet> 키트)와 함께 제공 되는 xsd.exe 도구를 사용 하 여 강력한 형식의를 생성할 수 있습니다.  
  
 데이터베이스 테이블에서 xsd를 만들려면 또는 <xref:System.Data.DataSet.WriteXmlSchema%2A> [Visual Studio에서 데이터 집합 작업](/visualstudio/data-tools/dataset-tools-in-visual-studio)을 참조 하세요.  
  
 다음 코드에서는이 도구를 사용 하 여 **데이터 집합** 을 생성 하는 구문을 보여 줍니다.  
  
```  
xsd.exe /d /l:CS XSDSchemaFileName.xsd /eld /n:XSDSchema.Namespace  
```  
  
 이 구문에서 지시문은 `/d` 도구에 **데이터 집합**을 생성 하도록 지시 하 고는 `/l:` 도구에 사용할 언어 (예: C# 또는 Visual Basic .net)를 알려 줍니다. 선택적 `/eld` 지시문은 LINQ to DataSet를 사용 하 여 생성 된 **데이터 집합** 에 대해 쿼리할 수 있도록 지정 합니다. 이 옵션은 `/d` 옵션도 함께 지정한 경우에 사용합니다. 자세한 내용은 [형식화 된 데이터 집합 쿼리](../querying-typed-datasets.md)를 참조 하세요. 선택적 `/n:` 지시문은 도구에서 **xsdschema. namespace**라는 **데이터 집합** 에 대 한 네임 스페이스를 생성 하도록 지시 합니다. 이 명령을 실행하면 XSDSchemaFileName.cs가 생성되며, 이 파일을 컴파일하여 ADO.NET 응용 프로그램에서 사용할 수 있습니다. 생성된 코드는 라이브러리나 모듈로 컴파일할 수 있습니다.  
  
 다음 코드는 생성된 코드를 C# 컴파일러(csc.exe)를 사용하여 라이브러리로 컴파일하는 구문을 보여 줍니다.  
  
```  
csc.exe /t:library XSDSchemaFileName.cs /r:System.dll /r:System.Data.dll  
```  
  
 `/t:` 지시문은 도구에 라이브러리로 컴파일하라는 것을, `/r:` 지시문은 컴파일에 필요한 종속 라이브러리를 지정합니다. 이 명령을 실행하면 XSDSchemaFileName.dll이 생성되며, 이 dll은 `/r:` 지시문으로 ADO.NET 응용 프로그램을 컴파일할 때 컴파일러로 전달할 수 있습니다.  
  
 다음 코드는 ADO.NET 응용 프로그램에서 XSD.exe로 전달된 네임스페이스에 액세스하는 구문을 보여 줍니다.  
  
```vb  
Imports XSDSchema.Namespace  
```  
  
```csharp  
using XSDSchema.Namespace;  
```  
  
 다음 코드 예제에서는 **customerdataset** 이라는 형식화 된 **데이터 집합** 을 사용 하 여 **Northwind** 데이터베이스에서 고객 목록을 로드 합니다. **Fill** 메서드를 사용 하 여 데이터가 로드 되 면이 예제에서는 형식화 된 **customersrow** (**DataRow**) 개체를 사용 하 여 **Customers** 테이블의 각 고객을 반복 합니다. 이렇게 하면 **DataColumnCollection**를 통하지 않고 **CustomerID** 열에 직접 액세스할 수 있습니다.  
  
```vb  
Dim customers As CustomerDataSet= New CustomerDataSet()  
Dim adapter As SqlDataAdapter New SqlDataAdapter( _  
  "SELECT * FROM dbo.Customers;", _  
  "Data Source=(local);Integrated " & _  
  "Security=SSPI;Initial Catalog=Northwind")  
  
adapter.Fill(customers, "Customers")  
  
Dim customerRow As CustomerDataSet.CustomersRow  
For Each customerRow In customers.Customers  
  Console.WriteLine(customerRow.CustomerID)  
Next  
```  
  
```csharp  
CustomerDataSet customers = new CustomerDataSet();  
SqlDataAdapter adapter = new SqlDataAdapter(  
  "SELECT * FROM dbo.Customers;",  
  "Data Source=(local);Integrated " +  
  "Security=SSPI;Initial Catalog=Northwind");  
  
adapter.Fill(customers, "Customers");  
  
foreach(CustomerDataSet.CustomersRow customerRow in customers.Customers)  
  Console.WriteLine(customerRow.CustomerID);  
```  
  
 다음은 예제에서 사용한 XML 스키마입니다.  
  
```xml  
<?xml version="1.0" encoding="utf-8"?>  
<xs:schema id="CustomerDataSet" xmlns="" xmlns:xs="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">  
  <xs:element name="CustomerDataSet" msdata:IsDataSet="true">  
    <xs:complexType>  
      <xs:choice maxOccurs="unbounded">  
        <xs:element name="Customers">  
          <xs:complexType>  
            <xs:sequence>  
              <xs:element name="CustomerID" type="xs:string" minOccurs="0" />  
            </xs:sequence>  
          </xs:complexType>  
        </xs:element>  
      </xs:choice>  
    </xs:complexType>  
  </xs:element>  
</xs:schema>  
```  
  
## <a name="see-also"></a>참고자료

- <xref:System.Data.DataColumnCollection>
- <xref:System.Data.DataSet>
- [형식화된 데이터 집합](typed-datasets.md)
- [DataSet, DataTable 및 DataView](index.md)
- [ADO.NET 개요](../ado-net-overview.md)
