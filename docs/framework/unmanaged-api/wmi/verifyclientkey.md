---
title: VerifyClientKey 함수 (관리 되지 않는 API 참조)
description: VerifyClientKey 함수는 클라이언트 키의 보안이 올바른지 확인 합니다.
ms.date: 11/06/2017
api_name:
- VerifyClientKey
api_location:
- WMINet_Utils.dll
api_type:
- DLLExport
f1_keywords:
- VerifyClientKey
helpviewer_keywords:
- VerifyClientKey function [.NET WMI and performance counters]
topic_type:
- Reference
author: rpetrusha
ms.author: ronpet
ms.openlocfilehash: b674e959ab93cf76b84e2af41e875a50b7d115f4
ms.sourcegitcommit: d2e1dfa7ef2d4e9ffae3d431cf6a4ffd9c8d378f
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/07/2019
ms.locfileid: "70798194"
---
# <a name="verifyclientkey-function"></a>VerifyClientKey 함수
클라이언트 키가 올바른 보안을 유지하는지 확인합니다.  
  
[!INCLUDE[internalonly-unmanaged](../../../../includes/internalonly-unmanaged.md)]
  
## <a name="syntax"></a>구문  
  
```cpp  
LONG VerifyClientKey(); 
```  

## <a name="return-value"></a>반환 값

함수가 성공 하면 반환 값 `ERROR_SUCCESS` 은 (0)입니다.

함수가 실패 하면 반환 값은 *winerror.h*에 정의 된 0이 아닌 오류 코드입니다.

## <a name="requirements"></a>요구 사항  
 **플랫폼** [시스템 요구 사항](../../get-started/system-requirements.md)을 참조하십시오.  
  
 **헤더:** WMINet_Utils.def  
  
 **.NET Framework 버전:** [!INCLUDE[net_current_v472plus](../../../../includes/net-current-v472plus.md)]  
  
## <a name="see-also"></a>참고자료

- [WMI 및 성능 카운터 (관리 되지 않는 API 참조)](index.md)
