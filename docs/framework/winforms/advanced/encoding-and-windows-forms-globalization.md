---
title: 인코딩 및 Windows Forms 전역화
ms.date: 03/30/2017
helpviewer_keywords:
- ListView control [Windows Forms], lack of Unicode support
- DateTimePicker control [Windows Forms], lack of Unicode support
- TrackBar control [Windows Forms], lack of Unicode support
- ImageList component [Windows Forms], lack of Unicode support
- Unicode [Windows Forms]
- ToolBar control [Windows Forms], lack of Unicode support
- international applications [Windows Forms], character display
- StatusBar control [Windows Forms], lack of Unicode support
- MonthCalendar control [Windows Forms], lack of Unicode support
- international characters
- TabControl control [Windows Forms], lack of Unicode support
- Windows Forms, encoding
- TreeView control [Windows Forms], lack of Unicode support
- ProgressBar control [Windows Forms], Unicode-enabled forms
- localization [Windows Forms], character sets
- globalization [Windows Forms], character sets
ms.assetid: 22e8965d-a712-42b3-8167-3ee346bd70f9
ms.openlocfilehash: 60ca9f7ba2f716b5dab1b0276bc3cd07ddd8f65c
ms.sourcegitcommit: 878ca7550b653114c3968ef8906da2b3e60e3c7a
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 10/02/2019
ms.locfileid: "71736879"
---
# <a name="encoding-and-windows-forms-globalization"></a>인코딩 및 Windows Forms 전역화

Windows Forms 애플리케이션은 완전히 유니코드를 사용할 수 있으므로 플랫폼, 프로그램 또는 언어에 관계없이 각 문자가 고유한 숫자로 표시됩니다. 유니코드에 대 한 자세한 내용은 [unicode 컨소시엄 웹 사이트](https://www.unicode.org)를 참조 하십시오.

## <a name="benefits-of-unicode"></a>유니코드의 이점

유니코드 사용 폼의 이점에는 힌디어와 같은 유니코드 전용 스크립트로 작업할 수 있는 기능이 포함됩니다. 또한 단일 폼에서 여러 언어를 사용할 수 있습니다. 유니코드에서는 모든 문자가 2바이트 길이이므로 더블바이트 문자를 나타내기 위한 특별한 작업이 필요하지 않습니다. 모든 플랫폼에서 작동하는 단일 코드 집합을 작성할 수도 있습니다. 이는 Windows NT 및 Windows 98와 같은 다른 플랫폼에 대해 다른 코드를 작성 해야 했던 이전 버전의 Visual Basic에서 변경 된 내용입니다.

그러나 특정 컨트롤은 Windows 98 및 Windows Millennium Edition에서 유니코드를 지원 하지 않습니다. 이러한 컨트롤은 모두 공용 컨트롤에서 상속 되며 Windows 코드 페이지를 사용 하 여 ANSI로 데이터를 처리 합니다. 해당 컨트롤은 <xref:System.Windows.Forms.TabControl>, <xref:System.Windows.Forms.ListView>, <xref:System.Windows.Forms.TreeView>, <xref:System.Windows.Forms.DateTimePicker>, <xref:System.Windows.Forms.MonthCalendar>, <xref:System.Windows.Forms.TrackBar>, <xref:System.Windows.Forms.ProgressBar>, <xref:System.Windows.Forms.ImageList>, <xref:System.Windows.Forms.ToolBar> 및 <xref:System.Windows.Forms.StatusBar>입니다. 따라서 나열된 플랫폼의 이 컨트롤에는 유니코드 데이터를 표시할 수 없습니다. 예를 들어 영어 Windows 98 운영 체제에서 일본어 문자를 표시할 수 없습니다.

<xref:System.Windows.Forms.ToolBar> 및 <xref:System.Windows.Forms.StatusBar> 컨트롤에 대한 유니코드 인식 대체 항목으로 이러한 이전 컨트롤을 대체하는 <xref:System.Windows.Forms.ToolStrip> 및 <xref:System.Windows.Forms.StatusStrip> 컨트롤을 사용합니다. 애플리케이션에서 시각적 요소 간에 비슷한 모양과 느낌을 유지 관리하려면 <xref:System.Windows.Forms.MainMenu> 대신 메뉴 렌더링에 <xref:System.Windows.Forms.MenuStrip> 컨트롤을 사용합니다. <xref:System.Windows.Forms.ToolStrip> 및 <xref:System.Windows.Forms.StatusStrip>과 마찬가지로, <xref:System.Windows.Forms.MenuStrip>도 유니코드 문자를 처리하고 표시할 수 있습니다.

## <a name="see-also"></a>참조

- [응용 프로그램 Windows Forms 전역화](globalizing-windows-forms.md)
