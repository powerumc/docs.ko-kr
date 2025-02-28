---
title: '연습: WPF에서 Windows Forms 컨트롤 정렬'
ms.date: 04/03/2018
dev_langs:
- csharp
- vb
helpviewer_keywords:
- hybrid applications [WPF interoperability]
- arranging controls [WPF]
ms.assetid: a1db8049-15c7-45d6-ae3d-36a6735cb848
ms.openlocfilehash: fa0181e95a03324c4cfa9395ae57439c260d1c23
ms.sourcegitcommit: a97ecb94437362b21fffc5eb3c38b6c0b4368999
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68972301"
---
# <a name="walkthrough-arranging-windows-forms-controls-in-wpf"></a>연습: WPF에서 Windows Forms 컨트롤 정렬

이 연습에서는 레이아웃 기능을 사용 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 하 여 혼합 응용 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 프로그램에서 컨트롤을 정렬 하는 방법을 보여 줍니다.

이 연습에서 설명하는 작업은 다음과 같습니다.

- 프로젝트 만들기.

- 기본 레이아웃 설정 사용

- 콘텐츠에 맞게 크기 조정

- 절대 위치 사용

- 명시적으로 크기 지정

- 레이아웃 속성 설정

- z 순서 제한 사항 이해

- 도킹

- 표시 유형 설정

- 늘어나지 않는 컨트롤 호스트

- 배율 조정

- 회전

- 안쪽 여백 및 여백 설정

- 동적 레이아웃 컨테이너 사용

이 연습에서 설명 하는 작업의 전체 코드 목록은 [WPF에서 Windows Forms 컨트롤 정렬 샘플](https://go.microsoft.com/fwlink/?LinkID=159971)을 참조 하세요.

작업이 완료 되 면 기반 응용 프로그램의 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]레이아웃 기능을 이해 하 게 됩니다.

## <a name="prerequisites"></a>전제 조건

이 연습을 완료하려면 Visual Studio가 필요합니다.

## <a name="creating-the-project"></a>프로젝트 만들기

### <a name="to-create-and-set-up-the-project"></a>프로젝트를 만들고 설정하려면

1. 이라는 `WpfLayoutHostingWf`WPF 응용 프로그램 프로젝트를 만듭니다.

2. 솔루션 탐색기에서 다음 어셈블리에 대한 참조를 추가합니다.

    - WindowsFormsIntegration

    - System.Windows.Forms

    - System.Drawing

3. MainWindow.xaml을 두 번 클릭하여 XAML 보기에서 엽니다.

4. 요소에서 다음 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 네임 스페이스 매핑을 추가 합니다. <xref:System.Windows.Window>

    ```xaml
    xmlns:wf="clr-namespace:System.Windows.Forms;assembly=System.Windows.Forms"
    ```

5. 요소에서 <xref:System.Windows.Controls.Grid.ShowGridLines%2A> 속성을로 `true` 설정 하 고 5 개의 행과 3 개의 열을 정의 합니다. <xref:System.Windows.Controls.Grid>

     [!code-xaml[WpfLayoutHostingWfWithXaml#2](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#2)]

## <a name="using-default-layout-settings"></a>기본 레이아웃 설정 사용

기본적으로 요소는 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 호스트 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 된 컨트롤의 레이아웃을 처리 합니다.

### <a name="to-use-default-layout-settings"></a>기본 레이아웃 설정을 사용하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#3](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#3)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 컨트롤이에 표시 됩니다<xref:System.Windows.Controls.Canvas>. [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] <xref:System.Windows.Forms.Button?displayProperty=nameWithType> 호스팅된 컨트롤은 해당 콘텐츠를 기반으로 크기가 조정 되 고 <xref:System.Windows.Forms.Integration.WindowsFormsHost> , 요소는 호스트 된 컨트롤에 맞게 크기가 조정 됩니다.

## <a name="sizing-to-content"></a>콘텐츠에 맞게 크기 조정

요소 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 는 호스트 된 컨트롤의 크기가 콘텐츠를 제대로 표시 하도록 크기가 지정 되도록 합니다.

### <a name="to-size-to-content"></a>콘텐츠 크기를 조정하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#4](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#4)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 두 개의 새 단추 컨트롤 크기가 길면 긴 텍스트 문자열 및 더 큰 글꼴 크기를 올바르게 표시 하 고 <xref:System.Windows.Forms.Integration.WindowsFormsHost> , 호스트 된 컨트롤에 맞게 요소의 크기를 조정 합니다.

## <a name="using-absolute-positioning"></a>절대 위치 사용

절대 위치 지정을 사용 하 여 UI <xref:System.Windows.Forms.Integration.WindowsFormsHost> (사용자 인터페이스)의 어디에 나 요소를 배치할 수 있습니다.

### <a name="to-use-absolute-positioning"></a>절대 위치를 사용하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#5](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#5)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 요소 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 는 표 셀의 위쪽에서 20 픽셀, 왼쪽부터 20 픽셀에 배치 됩니다.

## <a name="specifying-size-explicitly"></a>명시적으로 크기 지정

<xref:System.Windows.FrameworkElement.Width%2A> <xref:System.Windows.Forms.Integration.WindowsFormsHost> 및<xref:System.Windows.FrameworkElement.Height%2A> 속성을 사용 하 여 요소의 크기를 지정할 수 있습니다.

### <a name="to-specify-size-explicitly"></a>명시적으로 크기를 지정하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#6](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#6)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 요소 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 는 기본 레이아웃 설정 보다 작은 50 픽셀의 크기 (70 픽셀 이상)로 설정 됩니다. [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 컨트롤의 내용이 알맞게 다시 정렬 됩니다.

## <a name="setting-layout-properties"></a>레이아웃 속성 설정

항상 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 요소의 속성을 사용 하 여 호스트 된 컨트롤의 레이아웃 관련 속성을 설정 합니다. 호스트된 컨트롤에서 직접 레이아웃 속성을 설정하면 의도하지 않은 결과가 생성됩니다.

 에서 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 호스팅된 컨트롤의 레이아웃 관련 속성을 설정 해도 아무런 영향을 주지 않습니다.

### <a name="to-see-the-effects-of-setting-properties-on-the-hosted-control"></a>호스트된 컨트롤에서 속성 설정의 결과를 확인하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#7](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#7)]

2. 솔루션 탐색기에서 MainWindow.xaml. vb 또는 MainWindow.xaml.cs를 두 번 클릭하여 코드 편집기에서 엽니다.

3. 다음 코드를 `MainWindow` 클래스 정의에 복사 합니다.

     [!code-csharp[WpfLayoutHostingWfWithXaml#101](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml.cs#101)]
     [!code-vb[WpfLayoutHostingWfWithXaml#101](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/VisualBasic/Window1.xaml.vb#101)]

4. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다.

5. **클릭** 하 여 단추를 클릭 합니다. 이벤트 `button1_Click` 처리기는 호스트 된 <xref:System.Windows.Forms.Control.Top%2A> 컨트롤 <xref:System.Windows.Forms.Control.Left%2A> 의 및 속성을 설정 합니다. 이렇게 하면 호스트 된 컨트롤이 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 요소 내에서 위치가 변경 됩니다. 호스트는 동일한 화면 영역을 유지하지만 호스트된 컨트롤은 잘립니다. 대신 호스팅된 컨트롤이 항상 요소를 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 채워야 합니다.

## <a name="understanding-z-order-limitations"></a>Z 순서 제한 사항 이해

표시 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 되는 요소는 항상 다른 WPF 요소 위에 그려지고 z 순서에 영향을 받지 않습니다. 이 z 순서 동작을 확인 하려면 다음을 수행 합니다.

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#8](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#8)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. <xref:System.Windows.Forms.Integration.WindowsFormsHost> 요소가 label 요소 위에 그려집니다.

## <a name="docking"></a>도킹

<xref:System.Windows.Forms.Integration.WindowsFormsHost>요소가 도킹 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 을 지원 합니다. 연결 된 속성을 설정 하 여 호스트 된 컨트롤을 <xref:System.Windows.Controls.DockPanel> 요소에 도킹 합니다. <xref:System.Windows.Controls.DockPanel.Dock%2A>

### <a name="to-dock-a-hosted-control"></a>호스트된 컨트롤을 도킹하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#9](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#9)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 요소 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 는 <xref:System.Windows.Controls.DockPanel> 요소의 오른쪽에 도킹 됩니다.

## <a name="setting-visibility"></a>표시 유형 설정

요소에 대 한 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] <xref:System.Windows.UIElement.Visibility%2A> 속성을 설정 하 여 컨트롤을 보이지 않거나 축소할 수 있습니다. <xref:System.Windows.Forms.Integration.WindowsFormsHost> 컨트롤이 보이지 않으면 표시되지는 않지만 레이아웃 공간은 사용합니다. 컨트롤이 축소되면 표시되지 않고 레이아웃 공간도 자치하지 않습니다.

### <a name="to-set-the-visibility-of-a-hosted-control"></a>호스트된 컨트롤의 표시 유형을 설정하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#10](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#10)]

2. MainWindow.xaml.vb 또는 MainWindow.xaml.cs에서 다음 코드를 클래스 정의에 복사합니다.

     [!code-csharp[WpfLayoutHostingWfWithXaml#102](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml.cs#102)]
     [!code-vb[WpfLayoutHostingWfWithXaml#102](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/VisualBasic/Window1.xaml.vb#102)]

3. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다.

4. 표시 되지 **않는** 단추를 클릭 하 여 요소를 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 표시 하지 않도록 설정 합니다.

5. **축소 하려면 클릭** 단추를 클릭 하 여 레이아웃 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 에서 요소를 완전히 숨깁니다. [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 컨트롤이 축소 되 면 주변 요소가 공간을 차지 하도록 다시 정렬 됩니다.

## <a name="hosting-a-control-that-does-not-stretch"></a>늘어나지 않는 컨트롤 호스트

일부 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] 컨트롤의 크기는 고정 되어 있으며 레이아웃에서 사용 가능한 공간을 채우도록 확장 되지 않습니다. 예를 들어, <xref:System.Windows.Forms.MonthCalendar> 컨트롤은 고정 된 공간에 월을 표시 합니다.

### <a name="to-host-a-control-that-does-not-stretch"></a>늘어나지 않는 컨트롤을 호스트하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#11](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#11)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. <xref:System.Windows.Forms.Integration.WindowsFormsHost> 요소가 표 형태 창에서 가운데에 맞춰지고 사용 가능한 공간을 채우도록 스트레치 되지 않습니다. 창이 충분히 크면 호스팅된 <xref:System.Windows.Forms.MonthCalendar> 컨트롤에 의해 표시 되는 두 개 이상의 월이 표시 될 수 있지만 이러한 항목은 행의 가운데에 표시 됩니다. 레이아웃 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 엔진은 사용 가능한 공간을 채우도록 크기를 조정할 수 없는 요소를 가운데에 맞춥니다.

## <a name="scaling"></a>배율 조정

WPF 요소와 달리 대부분의 Windows Forms 컨트롤은 지속적으로 확장할 수 없습니다. 사용자 지정 크기 조정을 제공 하려면 메서드를 <xref:System.Windows.Forms.Integration.WindowsFormsHost.ScaleChild%2A?displayProperty=nameWithType> 재정의 합니다.

### <a name="to-scale-a-hosted-control-by-using-the-default-behavior"></a>기본 동작을 사용하여 호스트된 컨트롤의 배율을 조정하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#12](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#12)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 호스트된 컨트롤과 해당 주변 요소가 0.5의 비율로 배율 조정됩니다. 그러나 호스트된 컨트롤의 글꼴은 배율 조정되지 않습니다.

<!-- This could use an example of custom scaling. -->

## <a name="rotating"></a>회전

WPF 요소와 달리 Windows Forms 컨트롤은 회전을 지원 하지 않습니다. 회전 <xref:System.Windows.Forms.Integration.WindowsFormsHost> 변환이 적용 될 때 요소는 다른 WPF 요소와 회전 하지 않습니다. 180도를 초과 하는 회전 값은 <xref:System.Windows.Forms.Integration.WindowsFormsHost.LayoutError> 이벤트를 발생 시킵니다.

### <a name="to-see-the-effect-of-rotation-in-a-hybrid-application"></a>혼합 애플리케이션에서 회전의 결과를 확인하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#13](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#13)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 호스트된 컨트롤은 회전되지 않지만 주변 요소는 180도 각도로 회전됩니다. 요소를 표시하려면 창의 크기를 조정해야 할 수 있습니다.

## <a name="setting-padding-and-margins"></a>안쪽 여백 및 여백 설정

레이아웃의 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 안쪽 여백 및 여백은의 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)]안쪽 여백 및 여백과 비슷합니다. 요소에 <xref:System.Windows.Controls.Control.Padding%2A> <xref:System.Windows.FrameworkElement.Margin%2A> 및 속성을 설정 하기만 하면 됩니다. <xref:System.Windows.Forms.Integration.WindowsFormsHost>

### <a name="to-set-padding-and-margins-for-a-hosted-control"></a>호스트된 컨트롤에 대해 안쪽 여백 및 여백을 설정하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#14](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#14)]
    [!code-xaml[WpfLayoutHostingWfWithXaml#15](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#15)]

2. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 안쪽 여백 및 여백 설정은에서 [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)] [!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)]적용 되는 것과 같은 방식으로 호스트 된 컨트롤에 적용 됩니다.

## <a name="using-dynamic-layout-containers"></a>동적 레이아웃 컨테이너 사용

[!INCLUDE[TLA#tla_winforms](../../../../includes/tlasharptla-winforms-md.md)]에서는 및 <xref:System.Windows.Forms.FlowLayoutPanel> <xref:System.Windows.Forms.TableLayoutPanel>라는 두 개의 동적 레이아웃 컨테이너를 제공 합니다. 이러한 컨테이너는 레이아웃에서 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] 사용할 수도 있습니다.

### <a name="to-use-a-dynamic-layout-container"></a>동적 레이아웃 컨테이너를 사용하려면

1. 다음 XAML을 <xref:System.Windows.Controls.Grid> 요소에 복사 합니다.

     [!code-xaml[WpfLayoutHostingWfWithXaml#16](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml#16)]

2. MainWindow.xaml.vb 또는 MainWindow.xaml.cs에서 다음 코드를 클래스 정의에 복사합니다.

     [!code-csharp[WpfLayoutHostingWfWithXaml#103](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml.cs#103)]
     [!code-vb[WpfLayoutHostingWfWithXaml#103](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/VisualBasic/Window1.xaml.vb#103)]

3. 생성자에서 `InitializeFlowLayoutPanel` 메서드에 호출을 추가합니다.

     [!code-csharp[WpfLayoutHostingWfWithXaml#104](~/samples/snippets/csharp/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/CSharp/Window1.xaml.cs#104)]
     [!code-vb[WpfLayoutHostingWfWithXaml#104](~/samples/snippets/visualbasic/VS_Snippets_Wpf/WpfLayoutHostingWfWithXaml/VisualBasic/Window1.xaml.vb#104)]

4. F5 키를 눌러 애플리케이션을 빌드하고 실행합니다. 요소 <xref:System.Windows.Forms.Integration.WindowsFormsHost> <xref:System.Windows.Controls.DockPanel>는를 채우고기본<xref:System.Windows.Forms.FlowLayoutPanel.FlowDirection%2A>컨트롤의 자식 컨트롤을 정렬합니다.<xref:System.Windows.Forms.FlowLayoutPanel>

## <a name="see-also"></a>참고자료

- <xref:System.Windows.Forms.Integration.ElementHost>
- <xref:System.Windows.Forms.Integration.WindowsFormsHost>
- [Visual Studio에서 XAML 디자인](/visualstudio/designers/designing-xaml-in-visual-studio)
- [WindowsFormsHost 요소에 대한 레이아웃 고려 사항](layout-considerations-for-the-windowsformshost-element.md)
- [WPF 샘플에서 Windows Forms 컨트롤 정렬](https://go.microsoft.com/fwlink/?LinkID=159971)
- [연습: WPF에서 Windows Forms 복합 컨트롤 호스팅](walkthrough-hosting-a-windows-forms-composite-control-in-wpf.md)
- [연습: Windows Forms에서 WPF 복합 컨트롤 호스팅](walkthrough-hosting-a-wpf-composite-control-in-windows-forms.md)
