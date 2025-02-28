---
title: -recurse
ms.date: 03/13/2018
helpviewer_keywords:
- /recurse compiler option [Visual Basic]
- -recurse compiler option [Visual Basic]
- recurse compiler option [Visual Basic]
ms.assetid: 84a0b670-33ae-44c4-a46a-b90388809317
ms.openlocfilehash: 4281c7bf5a7972d323e1e649aaef437c7ee901ff
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69956274"
---
# <a name="-recurse"></a>-recurse
지정 된 디렉터리 또는 프로젝트 디렉터리의 모든 자식 디렉터리에 있는 소스 코드 파일을 컴파일합니다.  
  
## <a name="syntax"></a>구문  
  
```  
-recurse:[dir\]file  
```  
  
## <a name="arguments"></a>인수  
 `dir`  
 선택 사항입니다. 검색을 시작하려는 디렉터리입니다. 지정 하지 않으면 프로젝트 디렉터리에서 검색이 시작 됩니다.  
  
 `file`  
 필수 요소. 검색할 파일입니다. 와일드카드 문자가 허용됩니다.  
  
## <a name="remarks"></a>설명  
 를 사용 `-recurse`하지 않고 파일 이름에 와일드 카드를 사용 하 여 프로젝트 디렉터리에서 일치 하는 모든 파일을 컴파일할 수 있습니다. 출력 파일 이름이 지정 되지 않은 경우 컴파일러는 처리 된 첫 번째 입력 파일의 출력 파일 이름을 기반으로 합니다. 이 파일은 일반적으로 사전순으로 볼 때 컴파일되는 파일 목록의 첫 번째 파일입니다. 따라서 `-out` 옵션을 사용 하 여 출력 파일을 지정 하는 것이 좋습니다.  
  
> [!NOTE]
> 이 `-recurse` 옵션은 Visual Studio 개발 환경에서 사용할 수 없습니다. 명령줄에서 컴파일하는 경우에만 사용할 수 있습니다.  
  
## <a name="example"></a>예제  
 다음 명령은 현재 디렉터리에 있는 모든 Visual Basic 파일을 컴파일합니다.  
  
```console
vbc *.vb  
```  
  
 다음 명령은 `Test\ABC` 디렉터리의 모든 Visual Basic 파일 및 그 아래에 있는 모든 디렉터리를 컴파일한 다음를 생성 `Test.ABC.dll`합니다.  
  
```console
vbc -target:library -out:Test.ABC.dll -recurse:Test\ABC\*.vb  
```  
  
## <a name="see-also"></a>참고자료

- [Visual Basic 명령줄 컴파일러](../../../visual-basic/reference/command-line-compiler/index.md)
- [-out (Visual Basic)](../../../visual-basic/reference/command-line-compiler/out.md)
- [샘플 컴파일 명령줄](../../../visual-basic/reference/command-line-compiler/sample-compilation-command-lines.md)
