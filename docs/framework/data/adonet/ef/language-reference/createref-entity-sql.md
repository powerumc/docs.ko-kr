---
title: CREATEREF (Entity SQL)
ms.date: 03/30/2017
ms.assetid: 489828cf-a335-4449-9360-b0d92eec5481
ms.openlocfilehash: cbaea82108dd3debcca972ca15dea248227330ac
ms.sourcegitcommit: 4e2d355baba82814fa53efd6b8bbb45bfe054d11
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70251109"
---
# <a name="createref-entity-sql"></a>CREATEREF (Entity SQL)
entityset의 엔터티에 대한 참조를 작성합니다.  
  
## <a name="syntax"></a>구문  
  
```  
CreateRef(entityset_identifier, row_typed_expression)  
```  
  
## <a name="arguments"></a>인수  
 `entityset_identifier`  
 문자열 리터럴이 아니라 entityset 식별자입니다.  
  
 `row_typed_expression`  
 엔터티 형식의 키 속성에 해당하는 행 형식의 식입니다.  
  
## <a name="remarks"></a>설명  
 `row_typed_expression` 은 엔터티의 키 유형과 구조적으로 동일해야 합니다. 다시 말해서, 필드의 개수와 형식 및 배열 순서가 엔터티 키와 동일해야 합니다.  
  
 아래 예제에서 Order와 BadOrder는 모두 Order 형식의 entityset이며, Id는 Order의 단일 키 속성인 것으로 간주됩니다. 예제에서는 BadOrder의 엔터티에 대한 참조를 생성하는 방법을 설명합니다. 이 참조는 현수 참조일 수 있습니다.  다시 말해서, 참조가 특정 엔터티를 실제로 나타내지 않을 수 있습니다. 이런 경우 해당 참조에 대해 `DEREF` 작업을 수행하면 null이 반환됩니다.  
  
```  
select CreateRef(LOB.BadOrders, row(o.Id))   
from LOB.Orders as o   
```  
  
## <a name="example"></a>예제  
 다음 Entity SQL 쿼리는 CREATEREF 연산자를 사용하여 엔터티 집합 내의 엔터티에 대한 참조를 작성합니다. 쿼리는 AdventureWorks Sales 모델을 기반으로 합니다. 이 쿼리를 컴파일하고 실행하려면 다음 단계를 수행하세요.  
  
1. [방법: StructuralType 결과](../how-to-execute-a-query-that-returns-structuraltype-results.md)를 반환 하는 쿼리를 실행 합니다.  
  
2. 다음 쿼리를 `ExecuteStructuralTypeQuery` 메서드에 인수로 전달합니다.  
  
 [!code-csharp[DP EntityServices Concepts 2#CREATEREF](../../../../../../samples/snippets/csharp/VS_Snippets_Data/dp entityservices concepts 2/cs/entitysql.cs#createref)]  
  
## <a name="see-also"></a>참고자료

- [엔터티 SQL 참조](entity-sql-reference.md)
- [DEREF](deref-entity-sql.md)
- [KEY](key-entity-sql.md)
- [REF](ref-entity-sql.md)
