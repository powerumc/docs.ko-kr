---
title: -win32resource
ms.date: 03/13/2018
f1_keywords:
- -win32resource
- win32resource
helpviewer_keywords:
- /win32resource compiler option [Visual Basic]
- -win32resource compiler option [Visual Basic]
- win32resource compiler option [Visual Basic]
ms.assetid: e226946d-19ce-4cc9-91f5-aed24f77aa2b
ms.openlocfilehash: 382dcc6571aa06ecdfc32bf43080c4b7a36eb1f0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69961299"
---
# <a name="-win32resource"></a>-win32resource
Win32 리소스 파일을 출력 파일에 삽입 합니다.  
  
## <a name="syntax"></a>구문  
  
```  
-win32resource:filename  
```  
  
## <a name="arguments"></a>인수  
 `filename`  
 출력 파일에 추가할 리소스 파일의 이름입니다. 공백을 포함 하는 경우 파일 이름을 따옴표 ("")로 묶습니다.  
  
## <a name="remarks"></a>설명  
 Microsoft Windows 리소스 컴파일러 (RC)를 사용 하 여 Win32 리소스 파일을 만들 수 있습니다.  
  
 Win32 리소스는 **파일 탐색기**에서 응용 프로그램을 식별 하는 데 도움이 되는 버전 또는 비트맵 (아이콘) 정보를 포함할 수 있습니다. 을 지정 `-win32resource`하지 않으면 컴파일러에서 어셈블리 버전을 기반으로 버전 정보를 생성 합니다. `-win32resource` 및`-win32icon` 옵션은 함께 사용할 수 없습니다.  
  
 .NET Framework 리소스 파일을 첨부 하려면- [linkresource (Visual Basic)](../../../visual-basic/reference/command-line-compiler/linkresource.md) 을 참조 하 여 .NET Framework 리소스 파일 또는 [-리소스 (Visual Basic)](../../../visual-basic/reference/command-line-compiler/resource.md) 를 참조 하세요.  
  
> [!NOTE]
> 이 `-win32resource` 옵션은 Visual Studio 개발 환경에서 사용할 수 없습니다. 명령줄에서 컴파일하는 경우에만 사용할 수 있습니다.  
  
## <a name="example"></a>예제  
 다음 코드는 Win32 `In.vb` 리소스 파일을 `Rf.res`컴파일하고 연결 합니다.  
  
```console  
vbc -win32resource:rf.res in.vb  
```  
  
## <a name="see-also"></a>참고자료

- [Visual Basic 명령줄 컴파일러](../../../visual-basic/reference/command-line-compiler/index.md)
- [샘플 컴파일 명령줄](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
