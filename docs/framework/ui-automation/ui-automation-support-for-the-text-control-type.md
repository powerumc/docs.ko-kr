---
title: Text 컨트롤 형식에 대한 UI 자동화 지원
ms.date: 03/30/2017
helpviewer_keywords:
- Text control type
- UI Automation, Text control type
- control types, Text
ms.assetid: ab0d0ada-8a71-4547-9c03-aadf675938f2
ms.openlocfilehash: 2194df2b4135f6c6a869d0e73f2d2df66d68993b
ms.sourcegitcommit: 289e06e904b72f34ac717dbcc5074239b977e707
ms.translationtype: MT
ms.contentlocale: ko-KR
ms.lasthandoff: 09/17/2019
ms.locfileid: "71040809"
---
# <a name="ui-automation-support-for-the-text-control-type"></a>Text 컨트롤 형식에 대한 UI 자동화 지원
> [!NOTE]
> 이 설명서는 <xref:System.Windows.Automation> 네임스페이스에 정의된 관리되는 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 클래스를 사용하려는 .NET Framework 개발자를 위한 것입니다. 에 대 한 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] [최신 정보는 Windows Automation API: UI 자동화](https://go.microsoft.com/fwlink/?LinkID=156746).  
  
 이 항목에서는 Text 컨트롤 형식에 대한 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 지원 정보를 제공합니다. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)]에서, 컨트롤 형식은 <xref:System.Windows.Automation.AutomationElement.ControlTypeProperty> 속성을 사용하기 위해 컨트롤이 충족해야 하는 조건 집합입니다. 이 조건에는 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 트리 구조, [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 속성 값, 컨트롤 패턴에 대한 특정 지침이 포함됩니다.  
  
 텍스트 컨트롤은 화면의 텍스트 항목을 나타내는 기본 사용자 인터페이스 항목입니다.  
  
 다음 섹션에서는 Text 컨트롤 형식에 필요한 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 트리 구조, 속성, 컨트롤 패턴, 이벤트를 정의합니다. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 요구 사항은 [!INCLUDE[TLA#tla_winclient](../../../includes/tlasharptla-winclient-md.md)], [!INCLUDE[TLA#tla_win32](../../../includes/tlasharptla-win32-md.md)]또는 [!INCLUDE[TLA#tla_winforms](../../../includes/tlasharptla-winforms-md.md)]의 모든 텍스트 컨트롤에 적용됩니다.  
  
<a name="Required_UI_Automation_Tree_Structure"></a>   
## <a name="required-ui-automation-tree-structure"></a>필요한 UI 자동화 트리 구조  
 다음 표는 텍스트 컨트롤과 관련된 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 트리의 컨트롤 뷰 및 콘텐츠 뷰를 보여주고 각 뷰에 포함될 수 있는 내용에 대해 설명합니다. [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 트리에 대 한 자세한 내용은 [UI 자동화 트리 개요](ui-automation-tree-overview.md)를 참조 하세요.  
  
|컨트롤 뷰|콘텐츠 뷰|  
|------------------|------------------|  
|텍스트|텍스트(콘텐츠인 경우)|  
  
 텍스트 컨트롤을 폼의 레이블 또는 정적 텍스트로 사용할 수 있습니다. 또한 텍스트 컨트롤은 다음 구조 내에 포함될 수 있습니다.  
  
- ListItem  
  
- TreeItem  
  
- DataItem  
  
 텍스트는 종종 다른 컨트롤의 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 를 통해 표시되기 때문에 텍스트 컨트롤이 `NameProperty` 트리의 콘텐츠 뷰에 표시되지 않을 수도 있습니다. 예를 들어, 콤보 상자 컨트롤의 레이블을 지정하는 데 사용되는 텍스트는 컨트롤의 `NameProperty` 값을 통해 노출됩니다. 콤보 상자 컨트롤이 UI 자동화 트리의 콘텐츠 뷰에 있기 때문에 텍스트 컨트롤이 콘텐츠 뷰에 있을 필요는 없습니다. 텍스트 컨트롤의 콘텐츠 뷰에는 항상 자식 항목이 없습니다.  
  
<a name="Required_UI_Automation_Properties"></a>   
## <a name="required-ui-automation-properties"></a>필요한 UI 자동화 속성  
 다음 표에서는 값 또는 정의가 텍스트 컨트롤과 특별히 관련된 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 속성을 나열하여 보여줍니다. 속성에 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 대 한 자세한 내용은 [클라이언트에 대 한 UI 자동화 속성](ui-automation-properties-for-clients.md)을 참조 하세요.  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 속성|값|노트|  
|------------------------------------------------------------------------------------|-----------|-----------|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationIdProperty>|메모를 참조하세요.|이 속성의 값은 애플리케이션의 모든 컨트롤에서 고유해야 합니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty>|메모를 참조하세요.|전체 컨트롤이 포함된 가장 바깥쪽 사각형입니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ClickablePointProperty>|메모를 참조하세요.|경계 사각형이 없는 경우 지원됩니다. 경계 사각형 내의 일부 지점이 클릭 가능하지 않으며 특수화된 적중 테스트를 수행하는 경우 클릭 가능한 지점을 재정의하고 제공하세요.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsKeyboardFocusableProperty>|메모를 참조하세요.|컨트롤이 키보드 포커스를 받을 수 있으면 해당 컨트롤은 이 속성을 지원해야 합니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty>|메모를 참조하세요.|텍스트 도구 모음 컨트롤의 이름에는 항상 txt가 표시됩니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LabeledByProperty>|`Null`|텍스트 컨트롤에는 정적 텍스트 레이블이 없습니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.ControlTypeProperty>|텍스트|이 값은 모든 UI 프레임워크에 대해 동일합니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.LocalizedControlTypeProperty>|"text"|Text 컨트롤 형식에 해당하는 지역화된 문자열입니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsContentElementProperty>|종속|텍스트 컨트롤에 다른 컨트롤의 NameProperty에 노출되지 않은 정보가 있는 경우 텍스트 컨트롤은 콘텐츠가 됩니다.|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsControlElementProperty>|True|텍스트 컨트롤은 항상 컨트롤이어야 합니다.|  
  
<a name="Required_UI_Automation_Control_Patterns"></a>   
## <a name="required-ui-automation-control-patterns"></a>필요한 UI 자동화 컨트롤 패턴  
 다음 표에서는 텍스트 컨트롤에서 지원되는 데 필요한 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 컨트롤 패턴을 나열하여 보여줍니다. 컨트롤 패턴에 대한 자세한 내용은 [UI Automation Control Patterns Overview](ui-automation-control-patterns-overview.md)를 참조하세요.  
  
|컨트롤 패턴|Support(지원)|노트|  
|---------------------|-------------|-----------|  
|<xref:System.Windows.Automation.Provider.IValueProvider>|Never|텍스트는 ValuePattern를 지원하지 않습니다. 텍스트를 편집할 수 있는 경우 이 형식은 Edit 컨트롤 형식입니다.|  
|<xref:System.Windows.Automation.Provider.ITextProvider>|종속|접근성 향상을 위해 텍스트는 Text 컨트롤 패턴을 지원해야 하지만 반드시 필요한 것은 아닙니다. Text 컨트롤 패턴은 텍스트에 다양한 스타일과 특성(예: 색, 굵은 글꼴, 기울임꼴)이 있을 때 유용하며 프레임워크에 따라 다릅니다.|  
|<xref:System.Windows.Automation.Provider.ITableItemProvider>|종속|텍스트 요소가 테이블 컨트롤 내에 포함되어 있으면 이 요소가 지원되어야 합니다.|  
|<xref:System.Windows.Automation.Provider.IRangeValueProvider>|종속|텍스트 요소가 테이블 컨트롤 내에 포함되어 있으면 이 요소가 지원되어야 합니다.|  
  
<a name="Required_UI_Automation_Events"></a>   
## <a name="required-ui-automation-events"></a>필요한 UI 자동화 이벤트  
 다음 표에서는 모든 텍스트 컨트롤에서 지원되는 데 필요한 [!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 이벤트를 나열하여 보여줍니다. 이벤트에 대한 자세한 내용은 [UI Automation Events Overview](ui-automation-events-overview.md)를 참조하세요.  
  
|[!INCLUDE[TLA2#tla_uiautomation](../../../includes/tla2sharptla-uiautomation-md.md)] 이벤트|Support(지원)|노트|  
|---------------------------------------------------------------------------------|-------------|-----------|  
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextSelectionChangedEvent>|필수|없음|  
|<xref:System.Windows.Automation.TextPatternIdentifiers.TextChangedEvent>|필수|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.BoundingRectangleProperty> 속성 변경 이벤트.|필수|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsOffscreenProperty> 속성 변경 이벤트.|필수|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.IsEnabledProperty> 속성 변경 이벤트.|필수|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.NameProperty> 속성 변경 이벤트.|필수|없음|  
|<xref:System.Windows.Automation.ValuePatternIdentifiers.ValueProperty> 속성 변경 이벤트.|Never|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.AutomationFocusChangedEvent>|필수|없음|  
|<xref:System.Windows.Automation.AutomationElementIdentifiers.StructureChangedEvent>|필수|없음|  
  
## <a name="see-also"></a>참고자료

- <xref:System.Windows.Automation.ControlType.Text>
- [UI 자동화 컨트롤 형식 개요](ui-automation-control-types-overview.md)
- [UI 자동화 개요](ui-automation-overview.md)
