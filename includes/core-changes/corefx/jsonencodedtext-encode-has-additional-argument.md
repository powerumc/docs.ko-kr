---
ms.openlocfilehash: 377f22409558c21d1c57f6214c13572dedf9e419
ms.sourcegitcommit: 56f1d1203d0075a461a10a301459d3aa452f4f47
ms.translationtype: HT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/24/2019
ms.locfileid: "71216950"
---
### <a name="jsonencodedtextencode-methods-have-an-additional-javascriptencoder-argument"></a>JsonEncodedText.Encode 메서드에 JavaScriptEncoder 인수 추가

.Net Core 3.0 미리 보기 8부터 <xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType> 메서드는 선택적 <xref:System.Text.Encodings.Web.JavaScriptEncoder> 인수를 포함합니다.

#### <a name="details"></a>세부 정보

.NET Core 3.0에는 새 형식인 xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType>가 포함되었습니다. .Net Core 3.0 미리 보기 8부터 모든 <xref:System.Text.Json.JsonEncodedText.Encode%2A?displayProperty=nameWithType> 메서드 오버로드의 서명이 선택적 <xref:System.Text.Encodings.Web.JavaScriptEncoder> 매개 변수를 포함하도록 변경되었습니다. 이 변경은 다른 또는 사용자 지정 인코더를 허용하기 위해 실시된 것입니다.

.Net Core 3.0 미리 보기 7에서 `Encode` 메서드의 서명은 다음과 같습니다.

```csharp
namespace System.Text.Json
{
    public readonly struct JsonEncodedText
    {
        public static JsonEncodedText Encode(ReadOnlySpan<byte> utf8Value);
        public static JsonEncodedText Encode(ReadOnlySpan<char> value);
        public static JsonEncodedText Encode(string value);
    }
}
```

.Net Core 3.0 미리 보기 8 버전 이상에서 동일한 `Encode` 메서드의 서명은 다음과 같습니다.

```csharp
namespace System.Text.Json
{
    public readonly struct JsonEncodedText
    {
        public static JsonEncodedText Encode(ReadOnlySpan<byte> utf8Value, JavaScriptEncoder encoder = null);
        public static JsonEncodedText Encode(ReadOnlySpan<char> value, JavaScriptEncoder encoder = null);
        public static JsonEncodedText Encode(string value, JavaScriptEncoder encoder = null);
    }
}
```

#### <a name="version-introduced"></a>도입된 버전

.NET Core 3.0 미리 보기 8

#### <a name="recommended-action"></a>권장 작업

이는 이진 주요 변경 내용일 뿐입니다. .NET Core 3.0 미리 보기 8 버전 이상에 대해 다시 컴파일하면 런타임 문제가 모두 해결됩니다.

#### <a name="category"></a>범주

CoreFx

#### <a name="affected-apis"></a>영향을 받는 API

<xref:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan%7BSystem.Byte%7D,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>
<xref:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan%7BSystem.Char%7D,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>
<xref:System.Text.Json.JsonEncodedText.Encode(System.String,System.Text.Encodings.Web.JavaScriptEncoder)?displayProperty=nameWithType>

<!--

### Affected APIs

- `M:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan{System.Byte},System.Text.Encodings.Web.JavaScriptEncoder)`
- `M:System.Text.Json.JsonEncodedText.Encode(System.ReadOnlySpan{System.Char},System.Text.Encodings.Web.JavaScriptEncoder)`
- `M:System.Text.Json.JsonEncodedText.Encode(System.String,System.Text.Encodings.Web.JavaScriptEncoder)`

-->
