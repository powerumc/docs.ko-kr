---
title: 그래픽 및 멀티미디어
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- media [WPF], features
- video effects [WPF]
- sound effects [WPF]
- animation [WPF], features
- graphics features [WPF]
- transition effects [WPF]
ms.assetid: 1817d9dc-3d6c-46cb-afc8-63b0bae35e37
ms.openlocfilehash: a770bcbbc8ac553c55e9dda5097abec8790182e5
ms.sourcegitcommit: d6e27023aeaffc4b5a3cb4b88685018d6284ada4
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 07/09/2019
ms.locfileid: "67663733"
---
# <a name="graphics-and-multimedia"></a>그래픽 및 멀티미디어

<a name="introduction"></a>
[!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)] 멀티미디어, 벡터 그래픽, 애니메이션 및 콘텐츠 컴퍼지션을, 쉽게 흥미로운 사용자 인터페이스 및 콘텐츠를 빌드하는 개발자를 위한 지원을 제공 합니다. [!INCLUDE[TLA#tla_visualstu](../../../../includes/tlasharptla-visualstu-md.md)]를 사용하여 벡터 그래픽이나 복잡한 애니메이션을 만든 후 미디어를 응용 프로그램에 통합할 수 있습니다.

이 항목에서는 그래픽, 전환 효과, 소리 및 비디오를 애플리케이션에 추가할 수 있도록 하는 [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)]의 그래픽, 애니메이션 및 미디어 기능을 소개합니다.

> [!NOTE]
> Windows 서비스에서 WPF 유형을 사용해서는 안 됩니다. Windows 서비스에서 WPF 유형을 사용하려고 하면 서비스가 예상대로 작동하지 않을 수 있습니다.

<a name="whats_new_with_graphics_and_multimedia_in_wpf_4"></a>

## <a name="whats-new-with-graphics-and-multimedia-in-wpf-4"></a>WPF 4에 포함된 그래픽 및 멀티미디어의 새로운 기능

그래픽 및 애니메이션에 관련해서 몇 가지 사항이 변경되었습니다.

- 레이아웃 조정

  개체 가장자리가 픽셀 디바이스 중앙에 놓이면 DPI 독립적 그래픽 시스템은 흐릿한 가장자리 또는 반투명 가장자리와 같은 렌더링 아티팩트를 만들 수 있습니다. 이전 버전의 WPF에는 이러한 경우를 처리하는 데 도움이 되는 픽셀 맞추기 기능이 포함되어 있습니다. Silverlight 2에서는 가장자리가 전체 픽셀 경계에 딱 맞게 요소를 이동하는 또 다른 방법인 레이아웃 조정이 도입되었습니다. WPF는 이제 사용 하 여 레이아웃을 <xref:System.Windows.FrameworkElement.UseLayoutRounding%2A> 연결 된 속성을 <xref:System.Windows.FrameworkElement>입니다.

- 캐시된 컴퍼지션

  새 사용 하 여 <xref:System.Windows.Media.BitmapCache> 고 <xref:System.Windows.Media.BitmapCacheBrush> 클래스 비트맵으로 시각적 트리의 복잡 한 부분을 캐시 하 고 렌더링 시간을 크게 향상 시킬 수 있습니다. 비트맵은 마우스 클릭 같은 사용자 입력에 응답하며, 브러시처럼 다른 요소에 칠할 수 있습니다.

- 픽셀 셰이더 3 지원

  WPF 4 기반으로 구축 된 <xref:System.Windows.Media.Effects.ShaderEffect> 픽셀 셰이더 (PS) 버전 3.0 사용 하 여 효과 쓰는 응용 프로그램을 함으로써 WPF 3.5 SP1에 도입 된 지원. PS 3.0 셰이더 모델은 PS 2.0보다 더 정교해졌으며 지원되는 하드웨어에 더 많은 영향을 미칠 수 있습니다.

- 감속/가속 함수

  감속/가속 함수를 사용하여 애니메이션을 개선하고 동작을 좀 더 강력히 제어할 수 있습니다. 예를 들어, 적용할 수 있습니다는 <xref:System.Windows.Media.Animation.ElasticEase> 애니메이션이 애니메이션에 튕기는 동작을 제공 합니다. 자세한 내용은 감속/가속 형식을 참조 된 <xref:System.Windows.Media.Animation> 네임 스페이스입니다.

<a name="graphics_and_rendering"></a>

## <a name="graphics-and-rendering"></a>그래픽 및 렌더링

WPF는 고품질의 2차원 그래픽을 지원합니다. 기능으로는 브러시, 기하 도형, 이미지, 도형 및 변환 기능이 있습니다. 자세한 내용은 [그래픽](graphics.md)을 참조하세요. 그래픽 요소 렌더링을 기반으로 합니다 <xref:System.Windows.Media.Visual> 클래스입니다. 화면의 시각적 개체 구조는 시각적 트리로 설명됩니다. 자세한 내용은 [WPF 그래픽 렌더링 개요](wpf-graphics-rendering-overview.md)를 참조하세요.

### <a name="2-d-shapes"></a>2차원 도형

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 일반적으로 사용 되는, 벡터 기반의 2d 도형 다음 그림에 나와 있는 타원, 사각형 등의 라이브러리를 제공 합니다.

![다이어그램 표시 타원 및 사각형입니다.](./media/index/two-deminsional-shapes-ellipses-rectangles.png)

이러한 내장 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 도형은 단순히 도형이 아닙니다. 키보드 및 마우스 입력을 포함하는 가장 일반적인 컨트롤에서 기대하는 많은 기능을 구현하는 프로그래밍 가능 요소입니다. 다음 예제에서는 처리 하는 방법을 보여 줍니다 합니다 <xref:System.Windows.UIElement.MouseUp> 를 클릭 하 여 발생 하는 이벤트는 <xref:System.Windows.Shapes.Ellipse> 요소입니다.

```xaml
<Window
  xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
  xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
  x:Class="Window1" >
  <Ellipse Fill="LightBlue" MouseUp="ellipseButton_MouseUp" />
</Window>
```

```csharp
public partial class Window1  : Window
{
    void ellipseButton_MouseUp(object sender, MouseButtonEventArgs e)
    {
        MessageBox.Show("You clicked the ellipse!");
    }
}
```

```vb
Partial Public Class Window1
    Inherits Window
    Private Sub ellipseButton_MouseUp(ByVal sender As Object, ByVal e As MouseButtonEventArgs)
        MessageBox.Show("You clicked the ellipse!")
    End Sub
End Class
```

다음 그림에서는 이전 [!INCLUDE[TLA2#tla_xaml](../../../../includes/tla2sharptla-xaml-md.md)] 태그 및 코드 숨김에 대한 출력을 보여 줍니다.

!["You clicked the ellipse!" 라는 메시지 상자](./media/index/messagebox-text-output.png)

자세한 내용은 [WPF에서 Shape 및 기본 그리기 개요](shapes-and-basic-drawing-in-wpf-overview.md)를 참조하세요. 기본 샘플을 보려면 [도형 요소 샘플](https://go.microsoft.com/fwlink/?LinkID=160037)을 참조하세요.

### <a name="2-d-geometries"></a>2차원 기하 도형

경우는 2 차원 셰이프는 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 제공 권한이 사용할 수 없는 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 기 하 도형 및 경로를 직접 만들에 대 한 지원. 다음 그림에서는 기하 도형을 그리기 브러시로 사용하여 도형을 만들고 다른 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 요소를 클리핑하는 방법을 보여 줍니다.

![기 하 도형을 만들어야 사용 하는 방법을 보여주는 스크린샷.](./media/index/use-geometries-create-shapes.png)

자세한 내용은 [기하 도형 개요](geometry-overview.md)를 참조하세요. 기본 샘플을 보려면 [기하 도형 샘플](https://go.microsoft.com/fwlink/?LinkID=159989)을 참조하세요.

### <a name="2-d-effects"></a>2차원 효과

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 다양 한 효과 만드는 데 사용할 수는 2 차원 클래스 라이브러리를 제공 합니다. 2 차원 렌더링 기능이 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 그릴 수 있는 기능도 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)] 그라데이션, 비트맵, 그림 및 비디오; 요소 회전을 사용 하 여 조작 하 고 크기 조정 및 기울이기와 합니다. 다음 그림에서는 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 브러시를 사용하여 획득할 수 있는 많은 효과의 예를 보여 줍니다.

![다양 한 WPF 브러시 및 그리기 요소를 보여 주는 그림입니다.](./media/index/brushes-paint-elements.png)

자세한 내용은 [WPF 브러시 개요](wpf-brushes-overview.md)를 참조하세요. 기본 샘플을 보려면 [브러시 샘플](https://go.microsoft.com/fwlink/?LinkID=159973)을 참조하세요.

<a name="rendering"></a>

## <a name="3-d-rendering"></a>3차원 렌더링

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 2 차원 그래픽 지원 통합 되는 3 차원 렌더링 기능 집합을 제공 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 더 흥미로운 레이아웃을 만들 수 있는 순서로 [!INCLUDE[TLA2#tla_ui](../../../../includes/tla2sharptla-ui-md.md)], 및 데이터 시각화. 스펙트럼의 한쪽 끝 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 2 차원 이미지는 다음 그림에서는 3 차원 도형 표면 위에 렌더링할 수 있습니다.

![다른 질감을 사용 하 여 3 차원 도형을 보여 주는 샘플의 스크린샷.](./media/index/visual-three-dimensional-shape.png)

자세한 내용은 [3차원 그래픽 개요](3-d-graphics-overview.md)를 참조하세요. 기본 샘플을 보려면 [3차원 단색 샘플](https://go.microsoft.com/fwlink/?LinkID=159964)을 참조하세요.

<a name="animation"></a>

## <a name="animation"></a>애니메이션

애니메이션으로 컨트롤 및 요소가 커지거나, 흔들리거나, 회전하거나, 사라지도록 하여 흥미로운 페이지 전환 등을 만들 수 있습니다. [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]에서는 대부분의 속성에 애니메이션 효과를 줄 수 있을 뿐 아니라 대부분의 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)] 개체에도 애니메이션 효과를 줄 수 있으므로 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]를 사용하여 만든 사용자 지정 개체에 애니메이션 효과를 줄 수도 있습니다.

![애니메이션이 적용 된 큐브의 스크린샷입니다.](./media/index/animate-custom-objects.png)

자세한 내용은 [애니메이션 개요](animation-overview.md)를 참조하세요. 기본 샘플을 보려면 [애니메이션 예제 갤러리](https://go.microsoft.com/fwlink/?LinkID=159969)를 참조하세요.

<a name="media"></a>

## <a name="media"></a>미디어

이미지, 비디오 및 오디오는 미디어를 통해 정보 및 사용자 환경을 전달하는 방법입니다.

### <a name="images"></a>이미지

아이콘, 배경 및 애니메이션 일부를 포함하는 이미지는 대부분의 애플리케이션에서 핵심적인 부분입니다. 이미지를 자주 사용해야 하므로 [!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]는 여러 가지 방법으로 이미지로 작업하는 기능을 제공합니다. 다음 그림에서는 해당 방법 중 하나만 보여 줍니다.

![스타일 샘플 스크린 샷](../controls/./media/stylingintro-eventtriggers.png "StylingIntro_EventTriggers")

자세한 내용은 [이미징 개요](imaging-overview.md)를 참조하세요.

### <a name="video-and-audio"></a>비디오 및 오디오

[!INCLUDE[TLA2#tla_wpf](../../../../includes/tla2sharptla-wpf-md.md)]에서 제공하는 그래픽 기능 중 핵심 기능은 비디오 및 오디오를 포함하는 멀티미디어로 작업할 수 있도록 지원하는 것입니다. 다음 예제에서는 미디어 플레이어를 애플리케이션에 삽입하는 방법을 보여 줍니다.

```xaml
<MediaElement Source="media\numbers.wmv" Width="450" Height="250" />
```

<xref:System.Windows.Controls.MediaElement> 비디오 및 오디오를 재생할 수 이며 사용자 지정 쉽게 만들 수 있도록 충분히 확장 가능 [!INCLUDE[TLA2#tla_ui#plural](../../../../includes/tla2sharptla-uisharpplural-md.md)]합니다.

자세한 내용은 [멀티미디어 개요](multimedia-overview.md)를 참조하세요.

## <a name="see-also"></a>참고자료

- <xref:System.Windows.Media>
- <xref:System.Windows.Media.Animation>
- <xref:System.Windows.Media.Media3D>
- [2차원 그래픽 및 이미징](../advanced/optimizing-performance-2d-graphics-and-imaging.md)
- [WPF에서 Shape 및 기본 그리기 개요](shapes-and-basic-drawing-in-wpf-overview.md)
- [단색 및 그라데이션을 사용한 그리기 개요](painting-with-solid-colors-and-gradients-overview.md)
- [이미지, 그림 및 시각적 표시로 그리기](painting-with-images-drawings-and-visuals.md)
- [애니메이션 및 타이밍 방법 항목](animation-and-timing-how-to-topics.md)
- [3차원 그래픽 개요](3-d-graphics-overview.md)
- [멀티미디어 개요](multimedia-overview.md)
