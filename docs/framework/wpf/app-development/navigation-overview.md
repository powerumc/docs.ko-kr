---
title: 탐색 개요
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- loose XAML files [WPF]
- windows [WPF]
- Start page [WPF]
- HTML files [WPF]
- structured navigation [WPF]
- fragment navigation [WPF]
- URIs (Uniform Resource Identifiers)
- custom objects [WPF]
- Uniform Resource Identifiers (URIs)
- pages [WPF]
- frames [WPF]
- navigation hosts [WPF]
- journals [WPF]
- lifetimes [WPF]
- retaining content state [WPF]
- content state [WPF]
- programmatic navigation [WPF]
- hyperlinks [WPF]
ms.assetid: 86ad2143-606a-4e34-bf7e-51a2594248b8
ms.openlocfilehash: 574449f95ee9632d37f277d61806802457494df0
ms.sourcegitcommit: 68653db98c5ea7744fd438710248935f70020dfb
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69964582"
---
# <a name="navigation-overview"></a>탐색 개요

WPF (Windows Presentation Foundation)에서는 두 가지 유형의 응용 프로그램 (독립 실행형 응용 프로그램 및 [!INCLUDE[TLA#tla_xbap#plural](../../../../includes/tlasharptla-xbapsharpplural-md.md)])에서 사용할 수 있는 브라우저 스타일 탐색을 지원 합니다. 탐색 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 을 위해 콘텐츠를 패키징하는 <xref:System.Windows.Controls.Page> 클래스를 제공 합니다. 를 사용 하거나를 사용 <xref:System.Windows.Controls.Page> 하 여 <xref:System.Windows.Navigation.NavigationService>프로그래밍 방식으로 한 개 <xref:System.Windows.Documents.Hyperlink>에서 다른 방식으로 탐색할 수 있습니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]는 저널을 사용하여 탐색했던 페이지를 기억했다가 다시 해당 페이지로 돌아옵니다.

<xref:System.Windows.Controls.Page>,, 및 저널은에서 제공 하 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]는 탐색 지원의 핵심을 형성 합니다. <xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Navigation.NavigationService> 이 개요에서는 느슨한 [!INCLUDE[TLA#tla_xaml](../../../../includes/tlasharptla-xaml-md.md)] 파일, HTML 파일 및 개체에 대 한 탐색이 포함 된 고급 탐색 지원을 포함 하기 전에 이러한 기능을 자세히 설명 합니다.

> [!NOTE]
> 이 항목에서 "browser" 라는 용어는 현재 Microsoft Internet Explorer 및 Firefox를 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 포함 하는 응용 프로그램을 호스팅할 수 있는 브라우저만 나타냅니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 특정 기능이 특정 브라우저 에서만 지원 되는 경우 브라우저 버전을 라고 합니다.

## <a name="navigation-in-wpf-applications"></a>WPF 애플리케이션에서 탐색

이 항목에서는의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]주요 탐색 기능에 대 한 개요를 제공 합니다. 이러한 기능은 독립 실행형 응용 프로그램 및 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]모두에서 사용할 수 있지만이 항목에서는 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)]의 컨텍스트 내에서 이러한 기능을 제공 합니다.

> [!NOTE]
> 이 항목에서는를 빌드하고 배포 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]하는 방법에 대해서는 설명 하지 않습니다. 에 대 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]한 자세한 내용은 [WPF XAML 브라우저 응용 프로그램 개요](wpf-xaml-browser-applications-overview.md)를 참조 하세요.

이 섹션에서는 다음과 같은 탐색 측면에 대해 설명합니다.

- [페이지 구현](#CreatingAXAMLPage)

- [시작 페이지 구성](#Configuring_a_Start_Page)

- [호스트 창 제목, 너비 및 높이 구성](#ConfiguringAXAMLPage)

- [하이퍼링크 탐색](#NavigatingBetweenXAMLPages)

- [조각 탐색](#FragmentNavigation)

- [탐색 서비스](#NavigationService)

- [탐색 서비스를 사용하여 프로그래밍 방식으로 탐색](#Programmatic_Navigation_with_the_Navigation_Service)

- [탐색 수명](#Navigation_Lifetime)

- [저널을 사용하여 탐색 기억](#NavigationHistory)

- [페이지 수명 및 저널](#PageLifetime)

- [탐색 기록을 사용하여 콘텐츠 상태 유지](#RetainingContentStateWithNavigationHistory)

- [쿠키](#Cookies)

- [구조적 탐색](#Structured_Navigation)

<a name="CreatingAXAMLPage"></a>

### <a name="implementing-a-page"></a>페이지 구현

에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)].NET Framework 개체, 사용자 지정 개체, 열거형 값, 사용자 컨트롤, [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일 및 HTML 파일을 포함 하는 여러 콘텐츠 형식으로 이동할 수 있습니다. 그러나를 사용 <xref:System.Windows.Controls.Page>하 여 콘텐츠를 가장 일반적이 고 편리한 방식으로 패키지할 수 있습니다. 또한에서는 <xref:System.Windows.Controls.Page> 탐색 관련 기능을 구현 하 여 모양을 개선 하 고 개발을 간소화 합니다.

를 <xref:System.Windows.Controls.Page>사용 하 여 다음과 같은 태그를 사용 하 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 여 탐색 가능한 콘텐츠 페이지를 선언적으로 구현할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#Page1XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page1.xaml#page1xaml)]

태그 <xref:System.Windows.Controls.Page> 에서`Page` [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] [!INCLUDE[TLA#tla_xml](../../../../includes/tlasharptla-xml-md.md)] 구현 되는은 루트 요소 이며 네임 스페이스 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 선언이 필요 합니다. 요소 `Page` 는 탐색 하 고 표시 하려는 콘텐츠를 포함 합니다. 다음 태그와 같이 `Page.Content` property 요소를 설정 하 여 콘텐츠를 추가 합니다.

[!code-xaml[NavigationOverviewSnippets#Page2XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page2.xaml#page2xaml)]

`Page.Content`는 하나의 자식 요소만 포함할 수 있습니다. 앞의 예제에서 콘텐츠는 단일 문자열 “Hello, Page!”입니다. 실제로 레이아웃 컨트롤을 자식 요소 ( [레이아웃](../advanced/layout.md)참조)로 사용 하 여 콘텐츠를 포함 하 고 작성 하는 것이 일반적입니다.

`Page` 요소의 자식 요소는 <xref:System.Windows.Controls.Page> 의 콘텐츠로 간주 되며, 따라서 명시적 `Page.Content` 선언을 사용할 필요가 없습니다. 다음 태그는 이전 샘플을 선언적으로 구현한 것입니다.

[!code-xaml[NavigationOverviewSnippets#Page3XAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Page3.xaml#page3xaml)]

이 경우 `Page.Content` 는 `Page` 요소의 자식 요소를 사용 하 여 자동으로 설정 됩니다. 자세한 내용은 [WPF 콘텐츠 모델](../controls/wpf-content-model.md)을 참조하세요.

태그 전용 <xref:System.Windows.Controls.Page> 은 콘텐츠를 표시 하는 데 유용 합니다. 그러나은 <xref:System.Windows.Controls.Page> 사용자가 페이지와 상호 작용할 수 있도록 하는 컨트롤을 표시할 수도 있으며, 이벤트를 처리 하 고 응용 프로그램 논리를 호출 하 여 사용자 상호 작용에 응답할 수 있습니다. 대화형 <xref:System.Windows.Controls.Page> 은 다음 예제에 표시 된 것과 같이 태그와 코드 숨김으로 이루어진 조합을 사용 하 여 구현 됩니다.

[!code-xaml[XBAPAppDefSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml#homepagemarkup)]

[!code-csharp[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/HomePage.xaml.cs#homepagecodebehind)]
[!code-vb[XBAPAppDefSnippets#HomePageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/HomePage.xaml.vb#homepagecodebehind)]

태그 파일 및 코드 숨김 파일을 함께 사용하려면 다음과 같은 구성이 필요합니다.

- 태그에서 요소는 `Page` `x:Class` 특성을 포함 해야 합니다. 응용 프로그램을 빌드할 때 `x:Class` 태그 파일에가 있으면 MSBuild (Microsoft build engine)가에서 <xref:System.Windows.Controls.Page> 파생 되 고 `x:Class` 특성으로 `partial` 지정 된 이름을 가진 클래스를 만듭니다. 이렇게 하려면 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 스키마에 대 한 [!INCLUDE[TLA2#tla_xml](../../../../includes/tla2sharptla-xml-md.md)] 네임 스페이스 선언을 추가 해야 합니다 `xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"` (). 생성 `partial` 된 클래스는 `InitializeComponent`이벤트를 등록 하 고 태그에 구현 된 속성을 설정 하기 위해 호출 되는를 구현 합니다.

- 코드를 사용할 `partial` 때 클래스는 태그의 `x:Class` 특성으로 지정 되는 동일한 이름의 클래스 여야 하며에서 <xref:System.Windows.Controls.Page>파생 되어야 합니다. 이렇게 하면 응용 프로그램을 빌드할 때 태그 파일에 대해 생성 `partial` 되는 클래스와 코드 숨김이 연결 될 수 있습니다 ( [WPF 응용 프로그램 빌드](building-a-wpf-application-wpf.md)참조).

- 코드 숨김으로 클래스는 <xref:System.Windows.Controls.Page> `InitializeComponent` 메서드를 호출 하는 생성자를 구현 해야 합니다. `InitializeComponent`는 태그 파일의 생성 `partial` 된 클래스에 의해 구현 되어 이벤트를 등록 하 고 태그에 정의 된 속성을 설정 합니다.

> [!NOTE]
> 를 사용 하 여 <xref:System.Windows.Controls.Page> [!INCLUDE[TLA#tla_visualstu](../../../../includes/tlasharptla-visualstu-md.md)]프로젝트에 새를 추가 하는 <xref:System.Windows.Controls.Page> 경우는 태그와 코드 숨김으로 모두 사용 하 여 구현 되며, 태그와 코드 숨김이 있는 파일 간의 연결을 만드는 데 필요한 구성을 포함 합니다. 여기에서 설명 합니다.

가 있으면 <xref:System.Windows.Controls.Page>해당 위치로 이동할 수 있습니다. 응용 프로그램이 탐색 하 <xref:System.Windows.Controls.Page> 는 첫 번째를 지정 하려면 시작 <xref:System.Windows.Controls.Page>을 구성 해야 합니다.

<a name="Configuring_a_Start_Page"></a>

### <a name="configuring-a-start-page"></a>시작 페이지 구성

[!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]를 사용하려면 브라우저에 특정 규모의 응용 프로그램 인프라가 호스트되어야 합니다. 에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]클래스는 <xref:System.Windows.Application> 필요한 응용 프로그램 인프라를 설정 하는 응용 프로그램 정의의 일부입니다 ( [응용 프로그램 관리 개요](application-management-overview.md)참조).

응용 프로그램 정의는 일반적으로 태그와 코드를 모두 사용 하 여 구현 되며 마크업 파일은 MSBuild`ApplicationDefinition` 항목으로 구성 됩니다. 다음은에 대 한 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)]응용 프로그램 정의입니다.

[!code-xaml[XBAPAppDefSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

[!code-csharp[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/XBAPAppDefSnippets/CSharp/App.xaml.cs#xbapapplicationdefinitioncodebehind)]
[!code-vb[XBAPAppDefSnippets#XBAPApplicationDefinitionCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/XBAPAppDefSnippets/VisualBasic/Application.xaml.vb#xbapapplicationdefinitioncodebehind)]

는를 시작할 때 <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page> [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 자동으로로드되는인시작을지정하기위해[!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 응용 프로그램 정의를 사용할 수 있습니다. 이렇게 하려면 원하는 <xref:System.Windows.Application.StartupUri%2A> [!INCLUDE[TLA#tla_uri](../../../../includes/tlasharptla-uri-md.md)] 에<xref:System.Windows.Controls.Page>대해 속성을로 설정 합니다.

> [!NOTE]
> 대부분의 경우는 <xref:System.Windows.Controls.Page> 응용 프로그램으로 컴파일되거나 응용 프로그램으로 배포 됩니다. 이러한 경우 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 를 <xref:System.Windows.Controls.Page> 식별 하는는 pack 체계를 준수 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]하는 인 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 팩입니다. Pack [!INCLUDE[TLA2#tla_uri#plural](../../../../includes/tla2sharptla-urisharpplural-md.md)] 은 [WPF의 pack uri](pack-uris-in-wpf.md)에 자세히 설명 되어 있습니다. 아래에 설명된 http 체계를 사용하여 콘텐츠를 탐색할 수도 있습니다.

다음 예제와 <xref:System.Windows.Application.StartupUri%2A> 같이 태그에서 선언적으로 설정할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#XBAPApplicationDefinitionMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/App.xaml#xbapapplicationdefinitionmarkup)]

이 예제 `StartupUri` 에서 특성은 홈페이지를 식별 하는 상대 pack [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 을 사용 하 여 설정 됩니다. [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 이 시작 되 면 페이지를 자동으로 탐색 하 고 표시 합니다. 다음 그림은 웹 서버에서 시작 된를 보여 줍니다 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] .

![XBAP 페이지](./media/navigation-overview/xbap-launched-from-a-web-server.png "웹 서버에서 시작 된 XBAP를 표시 합니다.")

> [!NOTE]
> 의 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]개발 및 배포에 대 한 자세한 내용은 [wpf XAML 브라우저 응용 프로그램 개요](wpf-xaml-browser-applications-overview.md) 및 [wpf 응용 프로그램 배포](deploying-a-wpf-application-wpf.md)를 참조 하세요.

<a name="ConfiguringAXAMLPage"></a>

### <a name="configuring-the-host-windows-title-width-and-height"></a>호스트 창 제목, 너비 및 높이 구성

위의 그림에서 알 수 있는 한 가지 사항은 브라우저와 탭 패널 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)]의 제목이의입니다. 이 제목은 길이가 길며 모양이나 정보 제공 측면 모두에서 그다지 유용하지 않습니다. 이러한 이유로 <xref:System.Windows.Controls.Page> 는 <xref:System.Windows.Controls.Page.WindowTitle%2A> 속성을 설정 하 여 제목을 변경 하는 방법을 제공 합니다. 또한 각각 및 <xref:System.Windows.Controls.Page.WindowWidth%2A> <xref:System.Windows.Controls.Page.WindowHeight%2A>를 설정 하 여 브라우저 창의 너비와 높이를 구성할 수 있습니다.

<xref:System.Windows.Controls.Page.WindowTitle%2A>, <xref:System.Windows.Controls.Page.WindowWidth%2A> 및<xref:System.Windows.Controls.Page.WindowHeight%2A> 은 다음 예제와 같이 태그에서 선언적으로 설정할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#HomePageMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/HomePage.xaml#homepagemarkup)]

결과는 다음 그림에 나와 있습니다.

![창 제목, 높이, 너비](./media/navigation-overview/window-title-width-height.png "그러면 구성할 수 있는 창 제목, 높이 및 너비가 표시 됩니다.")

<a name="NavigatingBetweenXAMLPages"></a>

### <a name="hyperlink-navigation"></a>하이퍼링크 탐색

일반적인 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 은 여러 페이지로 구성 됩니다. 한 페이지에서 다른 페이지로 이동 하는 가장 간단한 방법은를 <xref:System.Windows.Documents.Hyperlink>사용 하는 것입니다. 다음 태그에 표시 된 <xref:System.Windows.Documents.Hyperlink> `Hyperlink` 요소를 <xref:System.Windows.Controls.Page> 사용 하 여를에 선언적으로 추가할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml1)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml2)]
[!code-xaml[NavigationOverviewSnippets#HyperlinkXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithHyperlink.xaml#hyperlinkxaml3)]

요소 `Hyperlink` 에는 다음이 필요 합니다.

- 특성에 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 지정 된 <xref:System.Windows.Controls.Page> 대로 탐색할의 pack입니다. `NavigateUri`

- 텍스트 및 이미지와 같이 사용자가 탐색을 시작 하기 위해 클릭할 수 있는 콘텐츠 ( `Hyperlink` 요소에 포함 될 수 있는 내용에 대 한 참조 <xref:System.Windows.Documents.Hyperlink>)입니다.

다음 그림에서는이 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] <xref:System.Windows.Controls.Page> 있는를 보여 줍니다. <xref:System.Windows.Documents.Hyperlink>

![하이퍼링크가 있는 페이지](./media/navigation-overview/xbap-with-a-page-with-a-hyperlink.png "그러면 하이퍼링크가 있는 페이지가 포함 된 XBAP가 표시 됩니다.")

짐작할 수 <xref:System.Windows.Documents.Hyperlink> 있듯이을 클릭 하면에서 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] `NavigateUri` 특성으로 식별 된로 <xref:System.Windows.Controls.Page> 이동 합니다. 또한는 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] Internet Explorer의 최근 페이지 목록에 <xref:System.Windows.Controls.Page> 이전 항목을 추가 합니다. 다음 그림에서 이를 확인할 수 있습니다.

![뒤로 및 앞으로 단추](./media/navigation-overview/back-and-forward-navigation.png "뒤로 및 앞으로 단추를 사용 하 여 탐색 합니다.")

또한에서 다른 항목 <xref:System.Windows.Controls.Page> 으로의 탐색을 지원할 뿐만 아니라 <xref:System.Windows.Documents.Hyperlink> 조각 탐색도 지원 합니다.

<a name="FragmentNavigation"></a>

### <a name="fragment-navigation"></a>조각 탐색

*조각 탐색* 은 현재 <xref:System.Windows.Controls.Page> 또는 다른 <xref:System.Windows.Controls.Page>의 콘텐츠 조각 탐색입니다. 에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]콘텐츠 조각은 명명 된 요소에 포함 된 콘텐츠입니다. 명명 된 요소는 해당 `Name` 특성 집합이 있는 요소입니다. 다음 태그는 콘텐츠 조각이 포함 `TextBlock` 된 명명 된 요소를 보여 줍니다.

[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup2)]
[!code-xaml[NavigationOverviewSnippets#PageWithContentFragmentsMARKUP3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithFragments.xaml#pagewithcontentfragmentsmarkup3)]

에서 <xref:System.Windows.Documents.Hyperlink> 콘텐츠 조각으로 이동 하려면 특성에 `NavigateUri` 다음이 포함 되어야 합니다.

- 콘텐츠 조각을 탐색할 <xref:System.Windows.Controls.Page> 의입니다.[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]

- “#” 문자입니다.

- 콘텐츠 조각이 포함 <xref:System.Windows.Controls.Page> 된의 요소 이름입니다.

조각은 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 다음과 같은 형식입니다.

*PageURI* `#` *ElementName*

다음은 콘텐츠 조각으로 이동 하도록 `Hyperlink` 구성 된의 예제를 보여 줍니다.

[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml1)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml2)]
[!code-xaml[NavigationOverviewSnippets#PageThatNavigatesXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageThatNavigatesToFragment.xaml#pagethatnavigatesxaml3)]

> [!NOTE]
> 이 섹션에서는의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]기본 조각 탐색 구현에 대해 설명 합니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]를 사용 하면 <xref:System.Windows.Navigation.NavigationService.FragmentNavigation?displayProperty=nameWithType> 이벤트를 처리 해야 하는 사용자 고유의 조각 탐색 구성표를 구현할 수도 있습니다.

> [!IMPORTANT]
> HTTP를 통해 페이지를 검색할 수 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 있는 경우에만 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 페이지 ( `Page` 루트 요소로 된 태그 전용 파일)의 조각으로 이동할 수 있습니다.
>
> 그러나 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 페이지에서 자체 조각을 탐색할 수 있습니다.

<a name="NavigationService"></a>

### <a name="navigation-service"></a>탐색 서비스

사용자는 특정 <xref:System.Windows.Controls.Page>에 대 한 탐색을 시작할 <xref:System.Windows.Navigation.NavigationService> 수있지만,페이지를찾고다운로드하는작업은클래스를통해수행됩니다.<xref:System.Windows.Documents.Hyperlink> 기본적으로는 <xref:System.Windows.Documents.Hyperlink>와 같은 클라이언트 코드를 대신 하 여 탐색 요청을 처리 하는 기능을 제공합니다.<xref:System.Windows.Navigation.NavigationService> 또한에서는 <xref:System.Windows.Navigation.NavigationService> 탐색 요청을 추적 하 고 영향을 주는 높은 수준의 지원을 구현 합니다.

을 <xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> <xref:System.Windows.Controls.Page> 클릭 하면을 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]호출 하 여 지정 된 팩에서을 찾아서 다운로드 합니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 다운로드 <xref:System.Windows.Controls.Page> 된는 루트 개체가 다운로드 <xref:System.Windows.Controls.Page>된 인스턴스인 개체 트리로 변환 됩니다. 루트 <xref:System.Windows.Controls.Page> 개체에 대 한 참조는 <xref:System.Windows.Navigation.NavigationService.Content%2A?displayProperty=nameWithType> 속성에 저장 됩니다. 탐색 된 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 콘텐츠의 pack은 <xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> 속성에 저장 되는 반면는 <xref:System.Windows.Navigation.NavigationService.CurrentSource%2A?displayProperty=nameWithType> 탐색 된 마지막 페이지의 팩 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 을 저장 합니다.

> [!NOTE]
> [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 응용 프로그램에 현재 활성 상태가 <xref:System.Windows.Navigation.NavigationService>둘 이상 있을 수 있습니다. 자세한 내용은이 항목의 뒷부분에 나오는 [탐색 호스트](#Navigation_Hosts) 를 참조 하십시오.

<a name="Programmatic_Navigation_with_the_Navigation_Service"></a>

### <a name="programmatic-navigation-with-the-navigation-service"></a>탐색 서비스를 사용하여 프로그래밍 방식으로 탐색

사용자를 대신 하 여 탐색 <xref:System.Windows.Navigation.NavigationService> 이 <xref:System.Windows.Documents.Hyperlink>태그에서 선언적으로 구현 되는지 여부를 <xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Navigation.NavigationService> 알 필요가 없습니다. 즉,의 <xref:System.Windows.Documents.Hyperlink> 직접 또는 간접 부모가 탐색 호스트 ( [탐색](#Navigation_Hosts)호스트 참조) <xref:System.Windows.Documents.Hyperlink> 이면 탐색 호스트의 탐색 서비스를 찾아서 사용 하 여 탐색 요청을 처리할 수 있습니다.

그러나 다음을 포함 하 여를 직접 사용 <xref:System.Windows.Navigation.NavigationService> 해야 하는 경우도 있습니다.

- 매개 변수가 없는 생성자를 사용 <xref:System.Windows.Controls.Page> 하 여를 인스턴스화해야 하는 경우

- 로 이동 하기 전에에서 속성을 설정 <xref:System.Windows.Controls.Page> 해야 하는 경우

- 탐색 해야 하는를 런타임에만 확인할 수 있는 경우 <xref:System.Windows.Controls.Page>

이러한 경우 <xref:System.Windows.Navigation.NavigationService.Navigate%2A> <xref:System.Windows.Navigation.NavigationService> 개체의 메서드를 호출 하 여 프로그래밍 방식으로 탐색을 시작 하는 코드를 작성 해야 합니다. 이는에 대 <xref:System.Windows.Navigation.NavigationService>한 참조를 가져오는 데 필요 합니다.

#### <a name="getting-a-reference-to-the-navigationservice"></a>NavigationService에 대한 참조 가져오기

[탐색 호스트](#Navigation_Hosts) 섹션 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 에서 설명 하는 이유로 응용 프로그램에는 둘 이상의 <xref:System.Windows.Navigation.NavigationService>를 사용할 수 있습니다. 즉 <xref:System.Windows.Navigation.NavigationService>, 코드에서를 찾는 방법이 필요 합니다 .이는 <xref:System.Windows.Navigation.NavigationService> 일반적으로 현재 <xref:System.Windows.Controls.Page>를 탐색 하는입니다. 메서드를 <xref:System.Windows.Navigation.NavigationService> `static` 호출 하여에대한참조를가져올수있습니다.<xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A?displayProperty=nameWithType> 특정 <xref:System.Windows.Navigation.NavigationService> <xref:System.Windows.Controls.Page> <xref:System.Windows.Navigation.NavigationService.GetNavigationService%2A> 를 탐색 하는를 가져오려면에 대 한 참조를 메서드의 인수로 전달 합니다. <xref:System.Windows.Controls.Page> 다음 코드에서는 현재 <xref:System.Windows.Navigation.NavigationService> <xref:System.Windows.Controls.Page>에 대 한를 가져오는 방법을 보여 줍니다.

[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPage.xaml.cs#getnscodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPage.xaml.vb#getnscodebehind2)]

에 <xref:System.Windows.Navigation.NavigationService> 대한<xref:System.Windows.Controls.Page> 를찾기<xref:System.Windows.Controls.Page.NavigationService%2A> 위한 바로 가기로는 속성을 구현 합니다. <xref:System.Windows.Controls.Page> 다음 예제에서 이를 확인할 수 있습니다.

[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind1)]
[!code-csharp[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/GetNSPageShortCut.xaml.cs#getnsshortcutcodebehind2)]
[!code-vb[NavigationOverviewSnippets#GetNSShortcutCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/GetNSPageShortCut.xaml.vb#getnsshortcutcodebehind2)]

> [!NOTE]
> 는 <xref:System.Windows.Controls.Page> <xref:System.Windows.Navigation.NavigationService> <xref:System.Windows.Controls.Page> 가 이벤트를<xref:System.Windows.FrameworkElement.Loaded> 발생 시킬 때만에 대 한 참조를 가져올 수 있습니다.

#### <a name="programmatic-navigation-to-a-page-object"></a>프로그래밍 방식으로 Page 개체 탐색

다음 예제에서는를 사용 <xref:System.Windows.Navigation.NavigationService> 하 여 프로그래밍 방식으로 <xref:System.Windows.Controls.Page>로 이동 하는 방법을 보여 줍니다. 로 탐색 되는를 매개 <xref:System.Windows.Controls.Page> 변수가 없는 단일 생성자를 사용 하 여 인스턴스화할 수 있으므로 프로그래밍 방식이 필요 합니다. 매개 <xref:System.Windows.Controls.Page> 변수가 없는 생성자가 있는는 다음 태그와 코드에 표시 됩니다.

[!code-xaml[NavigationOverviewSnippets#PageWithNonDefaultConstructorXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml#pagewithnondefaultconstructorxaml)]

[!code-csharp[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithNonDefaultConstructor.xaml.cs#pagewithnondefaultconstructorcodebehind)]
[!code-vb[NavigationOverviewSnippets#PageWithNonDefaultConstructorCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithNonDefaultConstructor.xaml.vb#pagewithnondefaultconstructorcodebehind)]

매개 <xref:System.Windows.Controls.Page> 변수가 없는 생성자를 <xref:System.Windows.Controls.Page> 사용 하 여를 탐색 하는는 다음 태그와 코드에 표시 됩니다.

[!code-xaml[NavigationOverviewSnippets#NSNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml#nsnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSNavigationPage.xaml.cs#nsnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSNavigationPage.xaml.vb#nsnavigationpagecodebehind)]

이를 클릭 하면를 인스턴스화하여 <xref:System.Windows.Controls.Page> 에서 매개 변수가 없는 생성자를 사용 하 고 <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> 메서드를 호출 하 여 탐색을 시작 합니다. <xref:System.Windows.Controls.Page> <xref:System.Windows.Documents.Hyperlink> <xref:System.Windows.Navigation.NavigationService.Navigate%2A>는 <xref:System.Windows.Navigation.NavigationService> 팩[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]이 아닌에서 탐색 하는 개체에 대 한 참조를 허용 합니다.

#### <a name="programmatic-navigation-with-a-pack-uri"></a>Pack URI를 사용하여 프로그래밍 방식으로 탐색

프로그래밍 방식으로 pack [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 을 생성 해야 하는 경우 (예: 런타임에만 pack [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 을 확인할 수 있는 경우) 메서드를 <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> 사용할 수 있습니다. 다음 예제에서 이를 확인할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#NSUriNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml#nsurinavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSUriNavigationPage.xaml.cs#nsurinavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#NSUriNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSUriNavigationPage.xaml.vb#nsurinavigationpagecodebehind)]

#### <a name="refreshing-the-current-page"></a>현재 페이지 새로 고침

속성에 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 저장 된 팩과 동일한 팩이 있는 경우 이다운로드되지않습니다.<xref:System.Windows.Controls.Page> <xref:System.Windows.Navigation.NavigationService.Source%2A?displayProperty=nameWithType> 에서 현재 페이지를 다시 다운로드 하도록 하려면 다음 예제와 같이 <xref:System.Windows.Navigation.NavigationService.Refresh%2A?displayProperty=nameWithType> 메서드를 호출 하면 됩니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]

[!code-xaml[NavigationOverviewSnippets#NSRefreshNavigationPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml#nsrefreshnavigationpagexaml1)]

[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind1)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NSRefreshNavigationPage.xaml.cs#nsrefreshnavigationpagecodebehind2)]
[!code-vb[NavigationOverviewSnippets#NSRefreshNavigationPageCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/NSRefreshNavigationPage.xaml.vb#nsrefreshnavigationpagecodebehind2)]

<a name="Navigation_Lifetime"></a>

### <a name="navigation-lifetime"></a>탐색 수명

지금까지 본 것처럼 탐색을 시작하는 방법은 여러 가지가 있습니다. 탐색을 시작 하 고 탐색을 진행 하는 동안에서 <xref:System.Windows.Navigation.NavigationService>구현 하는 다음 이벤트를 사용 하 여 탐색을 추적 하 고 영향을 줄 수 있습니다.

- <xref:System.Windows.Navigation.NavigationService.Navigating>. 새 탐색이 요청되면 발생합니다. 탐색을 취소하는 데 사용될 수 있습니다.

- <xref:System.Windows.Navigation.NavigationService.NavigationProgress>. 탐색 진행 정보를 제공하기 위해 다운로드하는 동안 정기적으로 발생합니다.

- <xref:System.Windows.Navigation.NavigationService.Navigated>. 페이지를 찾아서 다운로드할 때 발생합니다.

- <xref:System.Windows.Navigation.NavigationService.NavigationStopped>. 을 호출 <xref:System.Windows.Navigation.NavigationService.StopLoading%2A>하 여 탐색이 중지 되거나 현재 탐색이 진행 되는 동안 새 탐색이 요청 될 때 발생 합니다.

- <xref:System.Windows.Navigation.NavigationService.NavigationFailed>. 요청된 콘텐츠를 탐색하는 동안 오류가 있으면 발생합니다.

- <xref:System.Windows.Navigation.NavigationService.LoadCompleted>. 탐색된 콘텐츠가 로드 및 구문 분석되고 렌더링을 시작할 때 발생합니다.

- <xref:System.Windows.Navigation.NavigationService.FragmentNavigation>. 다음과 같이 콘텐츠 조각에 대한 탐색이 시작될 때 발생합니다.

  - 원하는 조각이 현재 콘텐츠에 있는 경우 즉시

  - 소스 콘텐츠를 로드한 후 다른 콘텐츠에 원하는 조각이 있는 경우

탐색 이벤트는 다음 그림과 같이 순서대로 발생합니다.

![페이지 탐색 흐름 차트](./media/navigation-overview/order-of-navigation-events.png "페이지 탐색 이벤트 흐름 차트")

일반적으로는 <xref:System.Windows.Controls.Page> 이러한 이벤트에 대해 염려 하지 않습니다. 응용 프로그램에 관심이 있을 가능성이 더 높습니다. 이러한 이벤트는 클래스에 의해 발생 하기도 합니다 <xref:System.Windows.Application> .

- <xref:System.Windows.Application.Navigating?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationProgress?displayProperty=nameWithType>

- <xref:System.Windows.Application.Navigated?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationFailed?displayProperty=nameWithType>

- <xref:System.Windows.Application.NavigationStopped?displayProperty=nameWithType>

- <xref:System.Windows.Application.LoadCompleted?displayProperty=nameWithType>

- <xref:System.Windows.Application.FragmentNavigation?displayProperty=nameWithType>

이벤트가 발생할 때마다 클래스는 <xref:System.Windows.Application> 해당 이벤트를 발생 시킵니다. <xref:System.Windows.Navigation.NavigationService> <xref:System.Windows.Controls.Frame>및 <xref:System.Windows.Navigation.NavigationWindow> 는 각각의 범위 내에서 탐색을 검색 하는 동일한 이벤트를 제공 합니다.

경우에 따라이 이벤트에 관심이 있을수있습니다.<xref:System.Windows.Controls.Page> 예를 들어는 <xref:System.Windows.Controls.Page> 이벤트를 <xref:System.Windows.Navigation.NavigationService.Navigating?displayProperty=nameWithType> 처리 하 여 자체 탐색을 취소할지 여부를 결정할 수 있습니다. 다음 예제에서 이를 확인할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#CancelNavigationPageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml#cancelnavigationpagexaml)]

[!code-csharp[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/CancelNavigationPage.xaml.cs#cancelnavigationpagecodebehind)]
[!code-vb[NavigationOverviewSnippets#CancelNavigationPageCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/CancelNavigationPage.xaml.vb#cancelnavigationpagecodebehind)]

앞의 예제와 같이의 탐색 이벤트 <xref:System.Windows.Controls.Page>를 사용 하 여 처리기를 등록 하는 경우에는 이벤트 처리기의 등록도 취소 해야 합니다. 그렇지 않으면 탐색이 저널을 사용 하 여 탐색을 기억 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] <xref:System.Windows.Controls.Page> 하는 방법과 관련 하 여 부작용이 발생할 수 있습니다.

<a name="NavigationHistory"></a>

### <a name="remembering-navigation-with-the-journal"></a>저널을 사용하여 탐색 기억

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]는 두 개의 스택(뒤로 스택 및 앞으로 스택)을 사용하여 탐색한 페이지를 기억합니다. 현재 <xref:System.Windows.Controls.Page> 에서 새 <xref:System.Windows.Controls.Page> 또는 기존 <xref:System.Windows.Controls.Page>로 이동 하는 경우 현재 <xref:System.Windows.Controls.Page> 가 *뒤로 스택에*추가 됩니다. 현재 <xref:System.Windows.Controls.Page> 에서 이전 <xref:System.Windows.Controls.Page>으로 다시 이동 하면 현재 <xref:System.Windows.Controls.Page> 가 *전방 스택에*추가 됩니다. 뒤로 스택, 앞으로 스택 및 이들을 관리하는 기능을 총칭하여 저널이라고 합니다. 뒤로 스택 및 전달 스택의 각 항목은 <xref:System.Windows.Navigation.JournalEntry> 클래스의 인스턴스이고, *저널 항목*이라고 합니다.

#### <a name="navigating-the-journal-from-internet-explorer"></a>Internet Explorer에서 저널 탐색

개념적으로 저널은 Internet Explorer의 **뒤로** 및 **앞으로** 단추와 동일한 방식으로 작동 합니다. 다음 그림을 참조하세요.

![뒤로 및 앞으로 단추](./media/navigation-overview/back-and-forward-navigation.png "뒤로 및 앞으로 단추를 사용 하 여 탐색 합니다.")

Internet [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] explorer에서 호스팅되는의 경우 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 는 저널을 internet explorer 탐색 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 에 통합 합니다. 이 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 를 통해 사용자는 Internet Explorer의 **뒤로**, **앞**으로 및 **최근 페이지** 단추를 사용 하 여의 페이지를 탐색할 수 있습니다.

> [!IMPORTANT]
> Internet Explorer에서 사용자가로 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)]이동 하는 경우, 활성 상태로 유지 되지 않은 페이지의 저널 항목만 저널에 유지 됩니다. 페이지를 활성 상태로 유지 하는 방법에 대 한 설명은이 항목의 뒷부분에 나오는 [페이지 수명 및 저널](#PageLifetime) 을 참조 하세요.

기본적으로 <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page> InternetExplorer의최근페이지목록에표시되는각에대한텍스트는에대한[!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 입니다. 대부분의 경우 이는 사용자에게 특히 의미가 없습니다. 다행히도 다음 옵션 중 하나를 사용하여 텍스트를 변경할 수 있습니다.

1. 연결 `JournalEntry.Name` 된 특성 값입니다.

2. `Page.Title` 특성 값입니다.

3. [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 현재 `Page.WindowTitle` 에<xref:System.Windows.Controls.Page>대 한 특성 값 및입니다.

4. 현재 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)]에 대한 <xref:System.Windows.Controls.Page>입니다. (기본값)

옵션이 나열되는 순서는 텍스트를 찾는 우선 순위와 일치합니다. 예를 들어가 `JournalEntry.Name` 설정 된 경우 다른 값은 무시 됩니다.

다음 예제에서는 `Page.Title` 특성을 사용 하 여 저널 항목에 대해 표시 되는 텍스트를 변경 합니다.

[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup1)]
[!code-xaml[NavigationOverviewSnippets#PageTitleMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml#pagetitlemarkup2)]

[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind1)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind1)]
[!code-csharp[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/PageWithTitle.xaml.cs#pagetitlecodebehind2)]
[!code-vb[NavigationOverviewSnippets#PageTitleCODEBEHIND2](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigationOverviewSnippets/VisualBasic/PageWithTitle.xaml.vb#pagetitlecodebehind2)]

#### <a name="navigating-the-journal-using-wpf"></a>WPF를 사용하여 저널 탐색

사용자는 Internet Explorer에서 **뒤로**, **앞**으로 및 **최근 페이지** 를 사용 하 여 저널을 탐색할 수 있지만에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]제공 하는 선언적 및 프로그래밍 방식 메커니즘을 모두 사용 하 여 저널을 탐색할 수도 있습니다. 이 작업을 수행 하는 한 가지 이유는 [!INCLUDE[TLA2#tla_ui#plural](../../../../includes/tla2sharptla-uisharpplural-md.md)] 페이지에서 사용자 지정 탐색을 제공 하는 것입니다.

에 의해 <xref:System.Windows.Input.NavigationCommands>노출 된 탐색 명령을 사용 하 여 선언적으로 저널 탐색 지원을 추가할 수 있습니다. 다음 예제에서는 `BrowseBack` 탐색 명령을 사용 하는 방법을 보여 줍니다.

[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml1)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml2)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml3)]
[!code-xaml[NavigationOverviewSnippets#NavigationCommandsPageXAML4](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/NavigationCommandsPage.xaml#navigationcommandspagexaml4)]

<xref:System.Windows.Navigation.NavigationService> 클래스의 다음 멤버 중 하나를 사용 하 여 프로그래밍 방식으로 저널을 탐색할 수 있습니다.

- <xref:System.Windows.Navigation.NavigationService.GoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.GoForward%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoBack%2A>

- <xref:System.Windows.Navigation.NavigationService.CanGoForward%2A>

이 항목의 뒷부분에 나오는 [탐색 기록을 사용 하 여 콘텐츠 상태 유지](#RetainingContentStateWithNavigationHistory) 에 설명 된 대로 저널을 프로그래밍 방식으로 조작할 수도 있습니다.

<a name="PageLifetime"></a>

### <a name="page-lifetime-and-the-journal"></a>페이지 수명 및 저널

[!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 그래픽, 애니메이션 및 미디어를 비롯 한 다양 한 콘텐츠를 포함 하는 여러 페이지를 포함 하는을 고려 합니다. 이와 같은 페이지의 메모리 사용량은 특히 비디오 및 오디오 미디어가 사용될 경우 매우 클 수 있습니다. 저널에서로 탐색 된 페이지를 "기억" 하는 경우 이러한 페이지를 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 통해 크고 많은 양의 메모리를 신속 하 게 사용할 수 있습니다.

이러한 이유로 저널의 기본 동작은 <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page> 개체에 대 한 참조가 아니라 각 업무 일지 항목에 메타 데이터를 저장 하는 것입니다. 저널 항목을 탐색 하는 경우 해당 <xref:System.Windows.Controls.Page> 메타 데이터는 지정 <xref:System.Windows.Controls.Page>된의 새 인스턴스를 만드는 데 사용 됩니다. 결과적으로 탐색 되는 <xref:System.Windows.Controls.Page> 각에는 다음 그림에 나와 있는 수명이 있습니다.

![페이지 수명](./media/navigation-overview/navigated-page-lifetime.png "그러면 페이지를 탐색할 때 수명이 표시 됩니다.")

기본 업무 일지 동작을 사용 하면 메모리 사용을 줄일 수 있지만 페이지당 렌더링 성능이 저하 될 수 있습니다. 특히 많은 콘텐츠가 <xref:System.Windows.Controls.Page> 있는 경우를 다시 인스턴스화하면 시간이 많이 걸릴 수 있습니다. 저널에서 <xref:System.Windows.Controls.Page> 인스턴스를 유지 해야 하는 경우에는 두 가지 방법으로 그릴 수 있습니다. 먼저 <xref:System.Windows.Navigation.NavigationService.Navigate%2A?displayProperty=nameWithType> 메서드를 호출 하 여 프로그래밍 방식 <xref:System.Windows.Controls.Page> 으로 개체를 탐색할 수 있습니다.

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 둘째, <xref:System.Windows.Controls.Page.KeepAlive%2A> 속성을로 `false` <xref:System.Windows.Controls.Page> 설정하여저널에서의인스턴스를유지하도록지정할수있습니다(기본값은).`true` 다음 예제와 같이 태그에서 선언적으로 설정할 <xref:System.Windows.Controls.Page.KeepAlive%2A> 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#KeepAlivePageXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/KeepAlivePage.xaml#keepalivepagexaml)]

활성 상태를 유지 <xref:System.Windows.Controls.Page> 하는의 수명은이 아닌 것과 약간 다릅니다. 활성 상태로 유지 되 <xref:System.Windows.Controls.Page> 는를 처음으로 탐색 하는 경우에는 활성 상태로 유지 되지 <xref:System.Windows.Controls.Page> 않는와 마찬가지로 인스턴스화됩니다. 그러나의 인스턴스가 저널에 유지 되므로 <xref:System.Windows.Controls.Page> 저널에 유지 되는 동안에는 다시 인스턴스화되지 않습니다. 따라서가 탐색 될 <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page> 때마다를 호출 해야 하는 초기화 논리가 있는 경우이를 생성자에서 <xref:System.Windows.FrameworkElement.Loaded> 이벤트 처리기로 이동 해야 합니다. 다음 그림 <xref:System.Windows.FrameworkElement.Loaded> 에 표시 된 것 처럼 및 <xref:System.Windows.FrameworkElement.Unloaded> 이벤트는 각각 <xref:System.Windows.Controls.Page> 이 및에서 탐색 될 때마다 발생 합니다.

![로드 및 언로드된 이벤트가 발생] 하는 경우 (./media/navigation-overview/loaded-and-unloaded-events.png "로드 및 언로드된 이벤트는 페이지를 탐색할 때 발생 합니다.")

<xref:System.Windows.Controls.Page> 가 활성 상태로 유지 되지 않으면 다음 중 하나를 수행 하면 안 됩니다.

- 이 페이지 또는 이 페이지의 일부에 대한 참조 저장

- 이벤트 처리기를 이 페이지에서 구현되지 않는 이벤트에 등록

이러한 작업 중 하나를 수행 하면가 저널에서 <xref:System.Windows.Controls.Page> 제거 된 후에도 메모리에 유지 되도록 하는 참조가 생성 됩니다.

일반적으로 활성 상태를 <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page> 유지 하지 않는 기본 동작을 사용 하는 것이 좋습니다. 그러나 이는 다음 절에서 설명하는 상태 의미와 관련이 있습니다.

<a name="RetainingContentStateWithNavigationHistory"></a>

### <a name="retaining-content-state-with-navigation-history"></a>탐색 기록을 사용하여 콘텐츠 상태 유지

이 활성 상태로 유지 되지 않고 사용자 로부터 데이터를 수집 하는 컨트롤이 있는 경우 사용자가로 이동 하 여 다시 <xref:System.Windows.Controls.Page>이동 하는 경우 데이터는 어떻게 되나요? <xref:System.Windows.Controls.Page> 경험을 바탕으로 사용자는 이전에 입력한 데이터가 표시될 것이라고 예상합니다. 불행 하 게도 각 탐색을 사용 <xref:System.Windows.Controls.Page> 하 여의 새 인스턴스가 만들어지므로 데이터를 수집 하는 컨트롤이 다시 인스턴스화되고 데이터가 손실 됩니다.

다행히 저널은 컨트롤 데이터를 포함 하 여 탐색 <xref:System.Windows.Controls.Page> 간에 데이터를 기억할 수 있도록 지원 합니다. 특히 각 <xref:System.Windows.Controls.Page> 의 저널 항목은 연결 된 <xref:System.Windows.Controls.Page> 상태에 대 한 임시 컨테이너 역할을 합니다. 다음 단계에서는을 <xref:System.Windows.Controls.Page> 탐색할 때이 지원이 사용 되는 방법을 간략하게 설명 합니다.

1. 현재 <xref:System.Windows.Controls.Page> 에 대 한 항목이 저널에 추가 됩니다.

2. 의 <xref:System.Windows.Controls.Page> 상태는 해당 페이지에 대 한 업무 일지 항목과 함께 저장 되며,이 항목은 백 스택에 추가 됩니다.

3. 새 <xref:System.Windows.Controls.Page> 를 탐색 합니다.

저널을 사용 <xref:System.Windows.Controls.Page> 하 여 페이지를 다시 탐색 하면 다음 단계가 수행 됩니다.

1. <xref:System.Windows.Controls.Page> (뒤로 스택의 맨 위 저널 항목)가 인스턴스화됩니다.

2. 는에 대 한 저널 항목과 함께 저장 된 상태로 새로 고쳐집니다. <xref:System.Windows.Controls.Page> <xref:System.Windows.Controls.Page>

3. <xref:System.Windows.Controls.Page> 를 다시 탐색 합니다.

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]에서는 다음 컨트롤이 사용 <xref:System.Windows.Controls.Page>될 때 자동으로이 지원을 사용 합니다.

- <xref:System.Windows.Controls.CheckBox>

- <xref:System.Windows.Controls.ComboBox>

- <xref:System.Windows.Controls.Expander>

- <xref:System.Windows.Controls.Frame>

- <xref:System.Windows.Controls.ListBox>

- <xref:System.Windows.Controls.ListBoxItem>

- <xref:System.Windows.Controls.MenuItem>

- <xref:System.Windows.Controls.ProgressBar>

- <xref:System.Windows.Controls.RadioButton>

- <xref:System.Windows.Controls.Slider>

- <xref:System.Windows.Controls.TabControl>

- <xref:System.Windows.Controls.TabItem>

- <xref:System.Windows.Controls.TextBox>

에서 <xref:System.Windows.Controls.Page> 이러한 컨트롤을 사용 하는 경우에는 다음 그림 <xref:System.Windows.Controls.ListBox> 에 <xref:System.Windows.Controls.Page> 표시 된 것 처럼 탐색 전체에 데이터를 입력 합니다.

![상태를 기억할 컨트롤이 있는 페이지](./media/navigation-overview/data-remembered-across-page-navigations.png "입력 한 데이터는 페이지 탐색 사이에 저장 됩니다.")

에 <xref:System.Windows.Controls.Page> 앞의 목록에 있는 컨트롤이 아닌 다른 컨트롤이 있거나 상태가 사용자 지정 개체에 저장 된 경우에는 탐색 사이 <xref:System.Windows.Controls.Page> 에서 저널의 상태가 기억나지 않게 코드를 작성 해야 합니다.

여러 탐색에서 <xref:System.Windows.Controls.Page> 작은 상태를 기억할 필요가 있는 경우 <xref:System.Windows.FrameworkPropertyMetadata.Journal%2A?displayProperty=nameWithType> 메타 데이터 플래그로 구성 된 종속성 속성 (참조 <xref:System.Windows.DependencyProperty>)을 사용할 수 있습니다.

여러 탐색에서 기억할 필요가 <xref:System.Windows.Controls.Page> 있는 상태가 여러 데이터를 구성 하는 경우에는 단일 클래스에서 상태를 캡슐화 하 고 <xref:System.Windows.Navigation.IProvideCustomContentState> 인터페이스를 구현 하는 코드를 적게 사용 하는 것을 알 수 있습니다.

자체에서 이동 하지 <xref:System.Windows.Controls.Page>않고 단일의 다양 한 상태를 탐색 해야 하는 경우 및 <xref:System.Windows.Navigation.NavigationService.AddBackEntry%2A?displayProperty=nameWithType>를 사용할 <xref:System.Windows.Navigation.IProvideCustomContentState> 수 있습니다. <xref:System.Windows.Controls.Page>

<a name="Cookies"></a>

### <a name="cookies"></a>쿠키

응용 프로그램에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 데이터를 저장할 수 있는 또 다른 방법은 <xref:System.Windows.Application.SetCookie%2A> 및 <xref:System.Windows.Application.GetCookie%2A> 메서드를 사용 하 여 생성, 업데이트 및 삭제 되는 쿠키를 사용 하는 것입니다. 에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 만들 수 있는 쿠키는 다른 유형의 웹 응용 프로그램에서 사용 하는 쿠키와 동일 합니다. 쿠키는 응용 프로그램 세션 중에 클라이언트 컴퓨터에서 응용 프로그램에 의해 저장 되는 임의의 데이터 조각입니다. 쿠키 데이터는 일반적으로 다음과 같은 이름/값 쌍 형식을 사용합니다.

*이름* `=` *값*

데이터가에 <xref:System.Windows.Application.SetCookie%2A>전달 될 때 쿠키가 설정 되어야 하는 <xref:System.Uri> 위치의와 함께 쿠키는 메모리 내에서 만들어지며 현재 응용 프로그램 세션 기간 동안만 사용할 수 있습니다. 이 유형의 쿠키를 *세션 쿠키*라고 합니다.

애플리케이션 세션 간에 쿠키를 저장하려면 다음 형식을 사용하여 쿠키에 만료 날짜를 추가해야 합니다.

*이름* `=` *값* `; expires=DAY, DD-MMM-YYYY HH:MM:SS GMT`

만료 날짜가 있는 쿠키는 쿠키가 만료 될 때까지 현재 Windows 설치의 임시 인터넷 파일 폴더에 저장 됩니다. 이러한 쿠키는 응용 프로그램 세션 간에 지속 되므로 *영구 쿠키* 라고 합니다.

메서드를 호출 <xref:System.Windows.Application.GetCookie%2A> 하 여 <xref:System.Windows.Application.SetCookie%2A> 쿠키를 메서드로 설정한 위치의를 <xref:System.Uri> 전달 하 여 세션과 영구 쿠키를 모두 검색할 수 있습니다.

에서 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]쿠키를 지 원하는 몇 가지 방법은 다음과 같습니다.

- [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]독립 실행형 응용 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 프로그램 및는 모두 쿠키를 만들고 관리할 수 있습니다.

- 에서 만든 쿠키는 브라우저에서 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 액세스할 수 있습니다.

- 동일한 도메인의 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]에서 쿠키를 만들고 공유할 수 있습니다.

- [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]동일한 도메인의 및 HTML 페이지에서 쿠키를 만들고 공유할 수 있습니다.

- 및 느슨한 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 페이지에서 웹 요청을 만들 때 쿠키가 디스패치 됩니다.

- 최상위 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 와[!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] iframe에서 호스트 되는 모두 쿠키에 액세스할 수 있습니다.

- 의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 쿠키 지원은 지원 되는 모든 브라우저에 대해 동일 합니다.

- Internet Explorer에서 쿠키와 관련 된 P3P 정책은 특히 자사 및 타사 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]와 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]관련 하 여에서 적용 됩니다.

<a name="Structured_Navigation"></a>

### <a name="structured-navigation"></a>구조적 탐색

간에 <xref:System.Windows.Controls.Page> 데이터를 전달 해야 하는 경우 데이터를 <xref:System.Windows.Controls.Page>의 매개 변수가 없는 생성자에 인수로 전달할 수 있습니다. 이 방법을 사용 하는 경우에 <xref:System.Windows.Controls.Page> 는 활성 상태를 유지 해야 합니다. 그렇지 않으면 다음 <xref:System.Windows.Controls.Page>에로 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 이동할 때 매개 변수가 없는 생성자를 사용 <xref:System.Windows.Controls.Page> 하 여를 다시 인스턴스화합니다.

또는에서 전달 해야 하는 데이터로 설정 된 속성을 구현할 수있습니다.<xref:System.Windows.Controls.Page> 그러나이 데이터를 탐색 하는에 <xref:System.Windows.Controls.Page> 데이터를 다시 <xref:System.Windows.Controls.Page> 전달 해야 하는 경우에는 복잡 해질 수 있습니다. 문제는 탐색이 탐색 된 후에이 반환 되도록 하는 <xref:System.Windows.Controls.Page> 메커니즘이 기본적으로 지원 되지 않는다는 것입니다. 기본적으로 탐색은 호출/반환 의미 체계를 지원하지 않습니다. 이 문제를 해결 하기 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 위해에서는 <xref:System.Windows.Navigation.PageFunction%601> <xref:System.Windows.Controls.Page> 가 예측 가능 하 고 구조화 된 방식으로로 반환 되도록 하는 데 사용할 수 있는 클래스를 제공 합니다. 자세한 내용은 [구조적 탐색 개요](structured-navigation-overview.md)를 참조 하세요.

<a name="The_NavigationWindow_Class"></a>

## <a name="the-navigationwindow-class"></a>NavigationWindow 클래스

지금까지 탐색 가능한 콘텐츠로 애플리케이션을 빌드하는 데 가장 많이 사용되는 탐색 서비스 영역을 살펴보았습니다. 이러한 서비스는에 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]국한 되지 않지만 컨텍스트에서 설명 했습니다. 최신 운영 체제 및 Windows 응용 프로그램은 최신 사용자의 브라우저 환경을 활용 하 여 브라우저 스타일 탐색을 독립 실행형 응용 프로그램에 통합 합니다. 일반적인 예는 다음과 같습니다.

- **Word 동의어 사전**: 단어 선택 항목을 탐색 합니다.

- **파일 탐색기**: 파일 및 폴더를 탐색 합니다.

- **마법사**: 복잡 한 작업을 여러 페이지로 분리 하 여 탐색할 수 있습니다. Windows 기능 추가 및 제거를 처리 하는 Windows 구성 요소 마법사를 예로 들 수 있습니다.

브라우저 스타일 탐색을 독립 실행형 응용 프로그램에 통합 하려면 <xref:System.Windows.Navigation.NavigationWindow> 클래스를 사용할 수 있습니다. <xref:System.Windows.Navigation.NavigationWindow>에서 파생 <xref:System.Windows.Window> 되 고에서 제공 하는 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 탐색에 대해 동일한 지원으로 확장 합니다. 독립 실행형 응용 <xref:System.Windows.Navigation.NavigationWindow> 프로그램의 주 창 또는 대화 상자와 같은 보조 창으로를 사용할 수 있습니다.

<xref:System.Windows.Navigation.NavigationWindow> [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] ( ,<xref:System.Windows.Controls.Page>등)에서 대부분의 최상위 클래스와 마찬가지로를 구현 하려면 태그와 코드 숨김으로 이루어진 조합을 사용 합니다.<xref:System.Windows.Window> 다음 예제에서 이를 확인할 수 있습니다.

[!code-xaml[IntroToNavNavigationWindowSnippets#NavigationWindowMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml#navigationwindowmarkup)]

[!code-csharp[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/MainWindow.xaml.cs#navigationwindowcodebehind)]
[!code-vb[IntroToNavNavigationWindowSnippets#NavigationWindowCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/MainWindow.xaml.vb#navigationwindowcodebehind)]

이 코드는 <xref:System.Windows.Navigation.NavigationWindow> <xref:System.Windows.Navigation.NavigationWindow> 가 열릴 때 <xref:System.Windows.Controls.Page> (page.xaml)를 자동으로 탐색 하는를 만듭니다. 가 주 응용 프로그램 창인 경우 특성을 `StartupUri` 사용 하 여 시작할 수 있습니다. <xref:System.Windows.Navigation.NavigationWindow> 다음 태그에서 이를 확인할 수 있습니다.

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchNavWindow](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/App.xaml#applaunchnavwindow)]

다음 그림에서는을 <xref:System.Windows.Navigation.NavigationWindow> 독립 실행형 응용 프로그램의 주 창으로 보여 줍니다.

![주 창](./media/navigation-overview/navigation-window-as-main-window.png "주 창으로 서의 탐색 창")

이 그림에서는 앞의 예제에서 <xref:System.Windows.Navigation.NavigationWindow> <xref:System.Windows.Navigation.NavigationWindow> 구현 코드에 설정 되지 않은 경우에도에 제목이 있음을 볼 수 있습니다. 대신, 다음 코드에 표시 된 <xref:System.Windows.Controls.Page.WindowTitle%2A> 속성을 사용 하 여 제목을 설정 합니다.

[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP1](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup1)]
[!code-xaml[IntroToNavNavigationWindowSnippets#HomePageMARKUP2](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/HomePage.xaml#homepagemarkup2)]

및 속성을 설정 하면에 <xref:System.Windows.Navigation.NavigationWindow>도 영향을 줍니다. <xref:System.Windows.Controls.Page.WindowHeight%2A> <xref:System.Windows.Controls.Page.WindowWidth%2A>

일반적으로 동작 또는 해당 모양을 <xref:System.Windows.Navigation.NavigationWindow> 사용자 지정 해야 하는 경우 사용자 고유의을 구현 합니다. 두 방법을 모두 사용하지 않으려면 바로 가기를 사용할 수 있습니다. 독립 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 실행형 <xref:System.Windows.Application.StartupUri%2A> <xref:System.Windows.Controls.Page> 응용프로그램<xref:System.Windows.Navigation.NavigationWindow> 에서의pack을로지정하면에서자동으로를만들어를호스팅합니다.<xref:System.Windows.Controls.Page> <xref:System.Windows.Application> 다음 태그에서는 이 기능을 설정하는 방법을 보여 줍니다.

[!code-xaml[IntroToNavNavigationWindowSnippets#AppLaunchPage](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/AnotherApp.xaml#applaunchpage)]

대화 상자 <xref:System.Windows.Navigation.NavigationWindow>와 같은 보조 응용 프로그램 창을으로 사용 하려면 다음 예제의 코드를 사용 하 여 열 수 있습니다.

[!code-csharp[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/csharp/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/CSharp/DialogOwnerWindow.xaml.cs#createnwdialogbox)]
[!code-vb[IntroToNavNavigationWindowSnippets#CreateNWDialogBox](~/samples/snippets/visualbasic/VS_Snippets_Wpf/IntroToNavNavigationWindowSnippets/VisualBasic/DialogOwnerWindow.xaml.vb#createnwdialogbox)]

다음 그림에서는 결과를 보여 줍니다.

![대화 상자](./media/navigation-overview/navigation-window-as-dialog-box.png "대화 상자로 서의 탐색 창")

여기에서 볼 수 있듯이 <xref:System.Windows.Navigation.NavigationWindow> 은 사용자가 저널을 탐색할 수 있는 Internet Explorer 스타일의 **뒤로** 및 **앞으로** 단추를 표시 합니다. 이러한 단추는 다음 그림에 나와 있는 것처럼 동일한 사용자 환경을 제공합니다.

![NavigationWindow의 뒤로 및 앞으로 단추](./media/navigation-overview/back-and-forward-buttons-in-navigation-window.png "탐색 창의 뒤로 및 앞으로 단추")

페이지에서 자체 저널 탐색 지원 및 UI를 제공 하는 <xref:System.Windows.Navigation.NavigationWindow.ShowsNavigationUI%2A> 경우 속성의 값을로 `false`설정 하 <xref:System.Windows.Navigation.NavigationWindow> 여에 표시 되는 **뒤로** 및 **앞으로** 단추를 숨길 수 있습니다.

또는의 사용자 지정 지원을 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 사용 하 여 <xref:System.Windows.Navigation.NavigationWindow> 자체의를 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 바꿀 수 있습니다.

<a name="Frame_in_Standalone_Applications"></a>

## <a name="the-frame-class"></a>Frame 클래스

브라우저와 <xref:System.Windows.Navigation.NavigationWindow> 는 모두 탐색 가능한 콘텐츠를 호스트 하는 창입니다. 애플리케이션의 콘텐츠가 전체 창에서 호스트될 필요가 없는 경우가 있습니다. 대신, 이러한 콘텐츠는 다른 콘텐츠 내에 호스트됩니다. 클래스를 <xref:System.Windows.Controls.Frame> 사용 하 여 탐색 가능한 콘텐츠를 다른 콘텐츠에 삽입할 수 있습니다. <xref:System.Windows.Controls.Frame>는 <xref:System.Windows.Navigation.NavigationWindow> 및[!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)]와 동일한 지원을 제공 합니다.

다음 예제에서는 <xref:System.Windows.Controls.Frame> `Frame` 요소를 사용 하 여를 <xref:System.Windows.Controls.Page> 선언적으로에 추가 하는 방법을 보여 줍니다.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPage.xaml#framehostpagexaml3)]

이 태그는가 `Source` [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] <xref:System.Windows.Controls.Page> `Frame` 처음으로탐색해야하는의pack을사용하여요소의특성을설정합니다.<xref:System.Windows.Controls.Frame> 다음 그림에서는 여러 페이지 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 간에 탐색 <xref:System.Windows.Controls.Page> 된가 있는 <xref:System.Windows.Controls.Frame> 을 보여 줍니다.

![여러 페이지 간에 탐색 된 프레임](./media/navigation-overview/frame-navigation-between-multiple-pages.png "그러면 여러 페이지 간의 프레임 탐색이 표시 됩니다.")

콘텐츠 내에서를 사용 <xref:System.Windows.Controls.Frame> 해야 하는 것은 아닙니다. <xref:System.Windows.Controls.Page> 의 콘텐츠 내에서을 <xref:System.Windows.Controls.Frame> 호스트 하는 것도 일반적입니다. <xref:System.Windows.Window>

기본적으로에서는 <xref:System.Windows.Controls.Frame> 다른 저널이 없는 경우에만 자체 저널을 사용 합니다. <xref:System.Windows.Navigation.NavigationWindow> <xref:System.Windows.Navigation.NavigationWindow> [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 이또는<xref:System.Windows.Controls.Frame> 내에서 호스팅되는 콘텐츠의 일부인 경우는 또는에 속한 저널을 사용 합니다. [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] <xref:System.Windows.Controls.Frame> 그러나 경우에 <xref:System.Windows.Controls.Frame> 따라 자체 저널을 담당 해야 할 수도 있습니다. 한 가지 이유는에서 <xref:System.Windows.Controls.Frame>호스팅하는 페이지 내에서 저널 탐색을 허용 하는 것입니다. 다음 그림에서 이를 확인할 수 있습니다.

![프레임 및 페이지 다이어그램](./media/navigation-overview/journal-navigation-within-pages-hosted-by-a-frame.png "프레임에서 호스트 되는 페이지 내에서 저널 탐색을 표시 합니다.")

이 <xref:System.Windows.Controls.Frame> 경우의 <xref:System.Windows.Controls.Frame.JournalOwnership%2A> 속성을<xref:System.Windows.Navigation.JournalOwnership.OwnsJournal>로설정하 여 자체의 저널을 사용 하도록를 구성할 수 있습니다. <xref:System.Windows.Controls.Frame> 다음 태그에서 이를 확인할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageOwnJournalXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnJournal.xaml#framehostpageownjournalxaml3)]

다음 그림에서는 자체 저널을 사용 <xref:System.Windows.Controls.Frame> 하는 내에서 탐색 하는 경우의 효과를 보여 줍니다.

![자체 저널을 사용 하는 프레임](./media/navigation-overview/frame-uses-its-own-journal.png "이는 자체 저널을 사용 하는 프레임 내에서 탐색의 효과를 보여 줍니다.")

저널 항목은 Internet Explorer가 아닌의 탐색 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] <xref:System.Windows.Controls.Frame>에서 표시 됩니다.

> [!NOTE]
> 이에서 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]호스팅되는 콘텐츠의 일부인 경우는 자체 저널을 사용하므로자체탐색을표시합니다.<xref:System.Windows.Controls.Frame> <xref:System.Windows.Window> <xref:System.Windows.Controls.Frame>

사용자 환경 <xref:System.Windows.Controls.Frame> 에서 탐색 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)]을 표시 하지 않고 자체 업무 일지를 제공 해야 하는 경우 <xref:System.Windows.Controls.Frame.NavigationUIVisibility%2A> 를로 <xref:System.Windows.Visibility.Hidden>설정 하 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 여 탐색을 숨길 수 있습니다. 다음 태그에서 이를 확인할 수 있습니다.

[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml1)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml2)]
[!code-xaml[NavigationOverviewSnippets#FrameHostPageHidesUIXAML3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHostPageOwnHiddenJournal.xaml#framehostpagehidesuixaml3)]

<a name="Navigation_Hosts"></a>

## <a name="navigation-hosts"></a>탐색 호스트

<xref:System.Windows.Controls.Frame>및 <xref:System.Windows.Navigation.NavigationWindow> 는 탐색 호스트 라고 하는 클래스입니다. *탐색 호스트* 는 콘텐츠를 탐색 하 고 표시할 수 있는 클래스입니다. 이를 위해 각 탐색 호스트는 자체 <xref:System.Windows.Navigation.NavigationService> 및 저널을 사용 합니다. 다음 그림은 탐색 호스트의 기본 구성을 보여 줍니다.

![탐색기 다이어그램](./media/navigation-overview/navigation-host-construction.png "탐색 호스트의 기본 생성")

기본적으로 <xref:System.Windows.Navigation.NavigationWindow> 및 <xref:System.Windows.Controls.Frame> 는 브라우저에서 호스팅될 때에서 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 제공 하는 것과 동일한 탐색 지원 기능을 제공 합니다.

및 저널 <xref:System.Windows.Navigation.NavigationService> 을 사용 하는 것 외에도 탐색 호스트는 <xref:System.Windows.Navigation.NavigationService> 을 구현 하는 동일한 멤버를 구현 합니다. 다음 그림에서 이를 확인할 수 있습니다.

![프레임 및 NavigationWindow의 저널](./media/navigation-overview/navigation-window-and-frame.png "탐색 창 및 프레임")

이를 통해 직접 탐색 지원을 프로그래밍할 수 있습니다. 에서 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 호스트되<xref:System.Windows.Controls.Frame> 는에 대 한 사용자 지정 탐색을 제공 해야 하는 경우이를 고려할 수 있습니다. <xref:System.Windows.Window> 또한 두 형식은 `BackStack` 모두 (<xref:System.Windows.Navigation.NavigationWindow.BackStack%2A?displayProperty=nameWithType>, <xref:System.Windows.Controls.Frame.BackStack%2A?displayProperty=nameWithType>) 및 `ForwardStack` (<xref:System.Windows.Navigation.NavigationWindow.ForwardStack%2A?displayProperty=nameWithType>, <xref:System.Windows.Controls.Frame.ForwardStack%2A?displayProperty=nameWithType>)를 포함 하 여 추가 탐색 관련 멤버를 구현 하 여 백 엔드의 저널 항목을 열거할 수 있도록 합니다. 스택 및 전달 스택 각각입니다.

앞서 언급했듯이 애플리케이션에는 둘 이상의 저널이 있을 수 있습니다. 다음 그림은 이러한 상황이 발생할 수 있는 예제를 제공합니다.

![한 응용 프로그램 내의 여러 저널](./media/navigation-overview/multiple-journals-in-one-application.png "응용 프로그램에서 둘 이상의 저널에 대 한 예제입니다.")

<a name="Navigating_to_Content_Other_than_Pages"></a>

## <a name="navigating-to-content-other-than-xaml-pages"></a>XAML 페이지 이외의 콘텐츠 탐색

이 항목 <xref:System.Windows.Controls.Page> 전체에서 및 팩 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 은의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]다양 한 탐색 기능을 보여 주기 위해 사용 되었습니다. 그러나 응용 프로그램 <xref:System.Windows.Controls.Page> 으로 컴파일되는는 탐색할 수 있는 유일한 콘텐츠 형식이 아니라 pack [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 만 콘텐츠를 식별 하는 유일한 방법입니다.

이 단원에서 설명 하는 것 처럼 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일, HTML 파일 및 개체로 이동할 수도 있습니다.

<a name="Navigating_to_Loose_XAML_Files"></a>

### <a name="navigating-to-loose-xaml-files"></a>XAML 사용 완화 파일 탐색

느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일은 다음과 같은 특징이 있는 파일입니다.

- 에는 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] (즉, 코드 없음)만 포함 됩니다.

- 적절한 네임스페이스 선언이 있습니다.

- .xaml 파일 이름 확장명이 있습니다.

예를 들어 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일인 Person으로 저장 된 다음 콘텐츠를 살펴보겠습니다.

[!code-xaml[NavigationOverviewSnippets#LooseXAML](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/Person.xaml#loosexaml)]

파일을 두 번 클릭하면 브라우저가 열리고 콘텐츠를 탐색 및 표시합니다. 다음 그림에서 이를 확인할 수 있습니다.

![PERSON .xaml 파일의 콘텐츠 표시](./media/navigation-overview/contents-of-person-xaml-file.png "PERSON .xaml 파일의 내용을 표시 합니다.")

다음에서 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일을 표시할 수 있습니다.

- 로컬 컴퓨터, 인트라넷 또는 인터넷의 웹 사이트

- [!INCLUDE[TLA#tla_unc](../../../../includes/tlasharptla-unc-md.md)] 파일 공유.

- 로컬 디스크

느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일은 브라우저의 즐겨찾기에 추가 하거나 브라우저의 홈 페이지에 추가할 수 있습니다.

> [!NOTE]
> 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 페이지를 게시 하 고 시작 하는 방법에 대 한 자세한 내용은 [WPF 응용 프로그램 배포](deploying-a-wpf-application-wpf.md)를 참조 하세요.

느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 사용에 대 한 제한 사항 중 하나는 부분 신뢰로 실행 하기에 안전한 콘텐츠만 호스트할 수 있다는 것입니다. 예 `Window` 를 들어는 느슨한 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 파일의 루트 요소가 될 수 없습니다. 자세한 내용은 [WPF 부분 신뢰 보안](../wpf-partial-trust-security.md)을 참조하세요.

<a name="Navigating_to_HTML_Files_Using_Frame"></a>

### <a name="navigating-to-html-files-by-using-frame"></a>프레임을 사용하여 HTML 파일 탐색

짐작할 수 있듯이 HTML로 이동할 수도 있습니다. Http 체계를 사용 하는 [!INCLUDE[TLA2#tla_uri](../../../../includes/tla2sharptla-uri-md.md)] 를 제공 하기만 하면 됩니다. 예를 들어 다음 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 은 HTML 페이지로 이동 하는을 <xref:System.Windows.Controls.Frame> 보여 줍니다.

[!code-xaml[NavigationOverviewSnippets#FrameHtmlNavMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigationOverviewSnippets/CSharp/FrameHTMLNavPage.xaml#framehtmlnavmarkup)]

HTML로 이동 하려면 특수 권한이 필요 합니다. 예를 들어 인터넷 영역 부분 신뢰 보안 [!INCLUDE[TLA2#tla_xbap](../../../../includes/tla2sharptla-xbap-md.md)] 샌드박스에서 실행 되는에서 탐색할 수 없습니다. 자세한 내용은 [WPF 부분 신뢰 보안](../wpf-partial-trust-security.md)을 참조하세요.

<a name="Navigating_to_HTML_Files_Using_WebBrowser"></a>

### <a name="navigating-to-html-files-by-using-the-webbrowser-control"></a>WebBrowser 컨트롤을 사용하여 HTML 파일 탐색

컨트롤 <xref:System.Windows.Controls.WebBrowser> 은 HTML 문서 호스팅, 탐색 및 스크립트/관리 코드 상호 운용성을 지원 합니다. <xref:System.Windows.Controls.WebBrowser> 컨트롤에 대 한 자세한 내용은을 참조 <xref:System.Windows.Controls.WebBrowser>하십시오.

와 <xref:System.Windows.Controls.Frame>마찬가지로를 사용 하 여 <xref:System.Windows.Controls.WebBrowser> HTML로 이동 하려면 특수 권한이 필요 합니다. 예를 들어 부분 신뢰 응용 프로그램에서는 원본 사이트에 있는 HTML로만 이동할 수 있습니다. 자세한 내용은 [WPF 부분 신뢰 보안](../wpf-partial-trust-security.md)을 참조하세요.

<a name="Navigating_to_Objects"></a>

### <a name="navigating-to-custom-objects"></a>사용자 지정 개체 탐색

사용자 지정 개체로 저장 되는 데이터가 있는 경우 해당 데이터를 표시 하는 한 가지 방법은 해당 개체에 <xref:System.Windows.Controls.Page> 바인딩되는 콘텐츠를 사용 하 여를 만드는 것입니다 ( [데이터 바인딩 개요](../data/data-binding-overview.md)참조). 개체를 표시하기 위해 전체 페이지를 만드는 오버헤드가 필요하지 않으면 페이지를 직접 탐색할 수 있습니다.

다음 코드 `Person` 에서 구현 된 클래스를 살펴보겠습니다.

[!code-csharp[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/Person.cs#personclasscode)]
[!code-vb[NavigateToObjectSnippets#PersonClassCODE](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/Person.vb#personclasscode)]

이로 이동 하려면 다음 코드에서 보여 <xref:System.Windows.Navigation.NavigationWindow.Navigate%2A?displayProperty=nameWithType> 주는 것 처럼 메서드를 호출 합니다.

[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject1](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject1)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject2](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject2)]
[!code-xaml[NavigateToObjectSnippets#PageThatNavsToObject3](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml#pagethatnavstoobject3)]

[!code-csharp[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/HomePage.xaml.cs#pagethatnavstoobjectcodebehind)]
[!code-vb[NavigateToObjectSnippets#PageThatNavsToObjectCODEBEHIND](~/samples/snippets/visualbasic/VS_Snippets_Wpf/NavigateToObjectSnippets/VisualBasic/HomePage.xaml.vb#pagethatnavstoobjectcodebehind)]

다음 그림에서는 결과를 보여 줍니다.

![클래스로 이동 하는 페이지입니다] . (./media/navigation-overview/page-navigates-to-an-object.png "개체를 탐색 하는 페이지의 예입니다.")

이 그림에서 유용한 콘텐츠가 표시되지 않는 것을 볼 수 있습니다. 실제로 표시 되는 값은 `ToString` **Person** 개체에 대 한 메서드의 반환 값입니다. 기본적으로이 값은 개체를 나타내는 데 사용할 수 있는 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 유일한 값입니다. 보다 의미 있는 정보 `ToString` 를 반환 하도록 메서드를 재정의할 수 있지만 여전히 문자열 값입니다. 의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 프레젠테이션 기능을 활용 하는 데 사용할 수 있는 방법 중 하나는 데이터 템플릿을 사용 하는 것입니다. 특정 형식의 개체와 연결할 수 있는 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 데이터 템플릿을 구현할 수 있습니다. 다음 코드는 `Person` 개체에 대 한 데이터 템플릿을 보여 줍니다.

[!code-xaml[NavigateToObjectSnippets#DataTemplateMARKUP](~/samples/snippets/csharp/VS_Snippets_Wpf/NavigateToObjectSnippets/CSharp/App.xaml#datatemplatemarkup)]

여기서 데이터 템플릿은 `Person` `DataType` 특성에서 `x:Type` 태그 확장을 사용 하 여 형식과 연결 됩니다. 그런 다음 데이터 템플릿은 요소 `TextBlock` (참조 <xref:System.Windows.Controls.TextBlock>) `Person` 를 클래스의 속성에 바인딩합니다. 다음 그림은 `Person` 개체의 업데이트 된 모양을 보여 줍니다.

![데이터 템플릿이 있는 클래스로 이동](./media/navigation-overview/navigating-to-a-class.png "데이터 템플릿이 있는 클래스로 이동 합니다.")

이 기술의 장점은 데이터 템플릿을 다시 사용하여 애플리케이션의 어디에서든 일관적으로 개체를 표시할 수 있다는 점입니다.

데이터 템플릿에 대 한 자세한 내용은 참조 하세요. [데이터 템플릿 개요](../data/data-templating-overview.md)합니다.

<a name="Security"></a>

## <a name="security"></a>보안

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]탐색 지원을 사용 [!INCLUDE[TLA2#tla_xbap#plural](../../../../includes/tla2sharptla-xbapsharpplural-md.md)] 하면 인터넷을 통해 탐색할 수 있으며 응용 프로그램에서 타사 콘텐츠를 호스트할 수 있습니다. 응용 프로그램과 사용자 모두에 게 유해한 동작을 방지 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 하기 위해에서는 [보안](../security-wpf.md) 및 [WPF 부분 신뢰 보안](../wpf-partial-trust-security.md)에 설명 된 다양 한 보안 기능을 제공 합니다.

## <a name="see-also"></a>참고자료

- <xref:System.Windows.Application.SetCookie%2A>
- <xref:System.Windows.Application.GetCookie%2A>
- [응용 프로그램 관리 개요](application-management-overview.md)
- [WPF의 Pack URI](pack-uris-in-wpf.md)
- [구조적 탐색 개요](structured-navigation-overview.md)
- [탐색 토폴로지 개요](navigation-topologies-overview.md)
- [방법 항목](navigation-how-to-topics.md)
- [WPF 응용 프로그램 배포](deploying-a-wpf-application-wpf.md)
