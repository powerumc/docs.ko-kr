---
ms.openlocfilehash: 0bad5c7944a06527cd71606c686762656f33c925
ms.sourcegitcommit: 33c8d6f7342a4bb2c577842b7f075b0e20a2fa40
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/12/2019
ms.locfileid: "70929175"
---
# <a name="voice-and-tone-guidelines"></a>어투 및 어조 지침

IT 전문가 및 개발자를 비롯한 다양한 사용자가 .NET을 학습하고 일반 작업에서 사용하기 위해 문서를 읽습니다.
사용자의 개발 작업을 지원하는 유용한 설명서를 작성하는 것이 목표입니다. 아래의 지침을 따르면 이러한 설명서를 작성할 수 있습니다. 스타일 가이드에는 다음 권장 사항이 포함되어 있습니다.

- [대화형 어조 사용](#use-a-conversational-tone)
- [2인칭 시점으로 작성](#write-in-2nd-person)
- [능동태 사용](#use-active-voice)
- [5등급 독자 수준을 대상으로 지정](#target-a-fifth-grade-reading-level)
- [미래 시제 방지](#avoid-future-tense)
- [개념 - 유용한 이유](#what-is-it-so-what)

이 스타일 가이드를 통해 각 권장 사항의 예를 살펴볼 수 있습니다. Microsoft는 .NET용 설명서를 작성할 때 진행할 수 있는 예제를 제공하기 위해 당사 지침에 따라 이 가이드를 작성했습니다. 또한 Microsoft의 지침을 따르지 않는 경우의 문서 내용을 확인할 수 있도록 대조되는 샘플도 제공합니다.

## <a name="details-on-each-guideline"></a>각 지침에 대한 상세 설명

### <a name="use-a-conversational-tone"></a>대화형 어조 사용
#### <a name="appropriate-style"></a>적절한 스타일:
Microsoft의 설명서에서는 대화형 어조가 권장됩니다. 즉, 자습서나 설명을 확인할 때 작성자와 대화를 하고 있다는 느낌을 받아야 합니다.
사용자에게 대화하는 것처럼 편안하면서도 유용한 정보를 제공할 수 있어야 합니다. 독자가 작성자의 개념 설명을 듣고 있는 것처럼 느껴야 합니다.

#### <a name="inappropriate-style"></a>부적절한 스타일:
대화형 스타일과 기술 주제를 학문적으로 다루는 스타일은 크게 다릅니다. 이러한 리소스는 매우 유용하지만, 작성자는 Microsoft의 설명서와는 매우 다른 스타일로 해당 문서를 작성했습니다. 예를 들어 학술지를 읽을 때 접할 수 있는 어조와 작성 스타일은 이러한 리소스와는 크게 다릅니다.
학술지의 경우 지루한 주제에 대해 무미건조한 설명을 읽고 있다고 느낄 것입니다.  

위에 나오는 첫 번째 단락의 경우 Microsoft에서 권장하는 대화형 스타일을 따릅니다. 그리고 두 번째 단락은 보다 학술적인 스타일입니다. 두 스타일 간의 차이점은 즉시 파악하실 수 있을 것입니다. 

### <a name="write-in-second-person"></a>2인칭 시점으로 작성
#### <a name="appropriate-style"></a>적절한 스타일:
독자에게 직접 이야기하는 것처럼 문서를 작성해야 합니다. 즉, 2인칭 표현을 자주 사용해야 합니다. 2인칭을 사용한다고 해서 항상 '당신/여러분' 등의 표현을 사용해야 하는 것은 아닙니다. 독자에게 직접 이야기하듯이 작성하면 됩니다. 명령형 문장을 작성합니다.
독자가 익히도록 할 내용을 전달해야 하기 때문입니다.

#### <a name="inappropriate-style"></a>부적절한 스타일: 
작성자가 3인칭 스타일로 문서를 작성하도록 선택할 수도 있습니다. 이러한 모델에서 작성자는 독자를 지칭하는 데 사용할 대명사나 명사를 찾아야 합니다. 이러한 3인칭 스타일은 독자가 몰입하고 재미있게 읽기가 어렵습니다.

첫 번째 단락은 권장 스타일을 따릅니다. 두 번째 단락은 3인칭을 사용합니다. 설명서는 2인칭으로 작성하세요. 그러면 훨씬 쉽게 읽을 수 있습니다.

### <a name="use-active-voice"></a>능동태 사용

능동태로 문서를 작성합니다. 능동태란 문장의 주체가 해당 문장에서 설명하는 작업(동사)을 수행한다는 의미입니다. 반면 수동태는 문장의 주체에 대해 작업이 수행된다는 의미로 문장이 정렬되는 방식입니다. 다음의 두 예를 대조해 보세요.

>컴파일러가 소스 코드를 실행 파일로 변환했습니다.

>소스 코드가 컴파일러에 의해 실행 파일로 변환되었습니다.

첫 번째 문장에는 능동태를 사용합니다. 두 번째 문장은 수동태로 작성되었습니다.
이 두 문장은 각 스타일의 또 다른 예를 제공합니다.

보다 쉽게 읽을 수 있는 능동태를 사용하는 것이 좋습니다. 수동태는 읽기가 더 어려울 수 있습니다.

### <a name="target-a-fifth-grade-reading-level"></a>5등급 독자 수준을 대상으로 지정

이 최종 지침을 제공하는 이유는, 대부분의 독자가 영어를 모국어로 사용하지 않기 때문입니다.
즉, 작성하는 문서는 전 세계의 대상에게 제공됩니다. 따라서 독자가 영어를 완벽하게 이해하지 못할 수도 있음을 감안해야 합니다.

하지만 문서 작성 대상이 기술 전문가라는 점도 잊어서는 안 됩니다. 즉, 독자가 프로그래밍 및 특정 프로그래밍 용어에 대해 알고 있다고 가정할 수 있습니다. 예를 들어 독자는 개체 지향 프로그래밍, 클래스, 개체, 함수, 메서드 등의 용어를 잘 알고 있을 것입니다. 그러나 문서를 확인하는 모든 독자에게 공식 컴퓨터 공학 학위가 있는 것은 아닙니다. 따라서 'Idempotent'와 같은 용어를 사용하는 경우에는 해당 정의를 제공해야 합니다.

>Close() 메서드는 idempotent입니다. 즉, 여러 번 호출해도 한 번 호출하는 것과 결과가 동일합니다.

### <a name="avoid-future-tense"></a>미래 시제 방지
영어가 아닌 일부 언어에서 미래 시제의 개념은 영어와 같지 않습니다. 미래 시제를 사용하면 문서를 읽기가 어려워집니다. 또한 미래 시제를 사용하는 경우 나올 수 있는 명백한 질문은 미래의 시점입니다. “PowerShell을 배우면 유용할 것입니다”라고 말한다면, 독자는 언제 유용할 것이냐고 질문할 것입니다. 따라서 “PowerShell을 사용하면 유용합니다”라고 말하는 것이 좋습니다.

### <a name="what-is-it---so-what"></a>개념 - 유용한 이유
새 개념을 독자에게 소개할 때마다 개념을 정의하고 개념이 유용한 이유를 설명합니다. 독자가 이점(또는 다른 사항)을 이해할 수 있으려면 개념을 파악하는 것이 중요합니다. 
