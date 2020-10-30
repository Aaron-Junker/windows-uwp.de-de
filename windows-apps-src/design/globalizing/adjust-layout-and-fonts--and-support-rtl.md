---
description: Entwerfen Sie Ihre APP, um die Layouts und Schriftarten mehrerer Sprachen zu unterstützen, einschließlich der Fluss Richtung von rechts nach links.
title: Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, Lokalisier barkeit, Lokalisierung, RTL, Ltr
ms.localizationpriority: medium
ms.openlocfilehash: 0e4725f6d26cf1abf42effddd813c31e87926e89
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033793"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“
Entwerfen Sie Ihre APP, um die Layouts und Schriftarten mehrerer Sprachen zu unterstützen, einschließlich der Fluss Richtung von rechts nach links. Die Fluss Richtung ist die Richtung, in der das Skript geschrieben und angezeigt wird, und die Benutzeroberflächen Elemente auf der Seite werden mit dem Auge gescannt.

## <a name="layout-guidelines"></a>Layoutrichtlinien
Sprachen wie Deutsch und Finnisch verwenden in der Regel mehr Zeichen als in englischer Sprache. In der Regel sind die Schriftarten in der Regel höher. Sprachen wie Arabisch und Hebräisch erfordern, dass Layoutpanels und Textelemente in der Lesefolge von rechts nach links (RTL) angeordnet werden.

Aufgrund dieser Variationen der Metriken von übersetztem Text empfiehlt es sich, die absolute Positionierung, die Breite oder die Höhe in der Benutzeroberfläche (UI) nicht zu reparieren. Nutzen Sie stattdessen die dynamischen Layoutmechanismen, die in die Windows-Benutzeroberflächen Elemente integriert sind. So werden z. b. Inhalts Steuerelemente (z. b. Schaltflächen), Element Steuerelemente (z. b. Raster Sichten und Listenansichten) und Layoutpanels (z. b. Raster und StackPanels) automatisch angepasst und standardmäßig an ihren Inhalt angepasst. Pseudo Lokalisierung Ihrer APP, um problematische Rand Fälle zu erkennen, in denen Ihre Benutzeroberflächen Elemente nicht ordnungsgemäß für den Inhalt korrekt sind.

Dynamisches Layout ist das empfohlene Verfahren, das Sie in den meisten Fällen verwenden können. Weniger vorzuziehen, aber immer noch besser als das Backen von Größen in das UI-Markup, besteht darin, layoutwerte als Funktion der Sprache festzulegen. Im folgenden finden Sie ein Beispiel dafür, wie Sie eine Width-Eigenschaft in ihrer App als Ressource verfügbar machen können, die von Lokalisierern entsprechend pro Sprache festgelegt werden kann. Fügen Sie zunächst in der Ressourcen Datei Ihrer APP (. resw) einen Eigenschafts Bezeichner mit dem Namen "TitleText. Width" (anstelle von "TitleText") hinzu. Verwenden Sie dann eine **x:UID** , um dem Eigenschaften Bezeichner ein oder mehrere Benutzeroberflächen Elemente zuzuordnen.

```xaml
<TextBlock x:Uid="TitleText">
```

Weitere Informationen zu Ressourcen Dateien (. resw), Eigenschafts Bezeichners und **x:UID** finden Sie unter Lokalisieren von Zeichen folgen [in der Benutzeroberfläche und im App-Paket Manifest](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Fonts
Verwenden Sie die Klasse [**languagefont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live) Font-Mapping für den programmgesteuerten Zugriff auf die empfohlene Schriftfamilie, Größe, Gewichtung und Stil für eine bestimmte Sprache. Die **languagefont** -Klasse ermöglicht den Zugriff auf die richtigen Schriftart Informationen für verschiedene Inhalts Kategorien, z. b. Benutzeroberflächen Header, Benachrichtigungen, Textkörper Text und vom Benutzer bearbeitbare Dokument Text Schriftarten.

## <a name="mirroring-images"></a>Spiegeln von Bildern
Wenn Ihre APP Bilder enthält, die gespiegelt werden müssen (das heißt, das gleiche Bild kann gekippt werden), können Sie **FlowDirection** verwenden.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Wenn Ihre APP ein anderes Image zum ordnungsgemäßen Kippen des Images benötigt, können Sie das Resource Management-System mit dem `LayoutDirection` Qualifizierer verwenden (Weitere Informationen finden Sie im Abschnitt LayoutDirection unter [Anpassen Ihrer Ressourcen für Sprache, Skalierung und andere Qualifizierer](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). Das System wählt ein Image aus `file.layoutdir-rtl.png` , wenn die APP-Lauf Zeit Sprache (Siehe Grundlegendes zu [Benutzerprofil Sprachen und App-Manifest-Sprachen](manage-language-and-region.md)) auf eine RTL-Sprache festgelegt ist. Diese Vorgehensweise ist möglicherweise nötig, wenn nicht alle Teile des Bilds umgedreht werden.

## <a name="handling-right-to-left-rtl-languages"></a>Behandeln von rechts-nach-links (RTL)-Sprachen
Wenn Ihre APP für die Sprachen von rechts nach links (RTL) lokalisiert ist, verwenden Sie die [**FrameworkElement. FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) -Eigenschaft, und legen Sie symmetrische Abstände und Ränder fest. Layoutpanels wie [**Raster**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) Skalierung und kippen automatisch mit dem Wert der **FlowDirection** , den Sie festlegen.

Legen Sie **FlowDirection** im Stammlayoutpanel (oder Frame) Ihrer Seite oder auf der Seite selbst fest. Dies bewirkt, dass alle in enthaltenen Steuerelemente diese Eigenschaft erben.

> [!IMPORTANT]
> Die **Fluss Richtung** wird jedoch *nicht* automatisch basierend auf der ausgewählten Anzeige Sprache des Benutzers in den Windows-Einstellungen festgelegt. und ändert sich nicht dynamisch als Reaktion auf die Anzeige Sprache des Benutzer Wechsels. Wenn der Benutzer z. b. die Windows-Einstellungen von Englisch auf Arabisch wechselt *, wird die* **FlowDirection** -Eigenschaft von links nach rechts von links nach rechts von rechts nach links geändert. Als App-Entwickler müssen Sie **FlowDirection** entsprechend der Sprache festlegen, die Sie zurzeit anzeigen.

Die programmgesteuerte Technik ist die Verwendung der- `LayoutDirection` Eigenschaft der bevorzugten Benutzer Anzeige Sprache zum Festlegen der [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) -Eigenschaft (siehe das Codebeispiel unten). Die meisten in Windows enthaltenen Steuerelemente verwenden bereits **FlowDirection** . Wenn Sie ein benutzerdefiniertes Steuerelement implementieren, sollte es **FlowDirection** verwenden, um geeignete Layoutänderungen für RTL-und LTR-Sprachen vorzunehmen.

```csharp    
this.languageTag = Windows.Globalization.ApplicationLanguages.Languages[0];

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

var flowDirectionSetting = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["LayoutDirection"];
if (flowDirectionSetting == "LTR")
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.LeftToRight;
}
else
{
    this.layoutRoot.FlowDirection = Windows.UI.Xaml.FlowDirection.RightToLeft;
}
```

```cppwinrt
#include <winrt/Windows.ApplicationModel.Resources.Core.h>
#include <winrt/Windows.Globalization.h>
...

m_languageTag = Windows::Globalization::ApplicationLanguages::Languages().GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView().QualifierValues().Lookup(L"LayoutDirection");
if (flowDirectionSetting == L"LTR")
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::LeftToRight);
}
else
{
    layoutRoot().FlowDirection(Windows::UI::Xaml::FlowDirection::RightToLeft);
}
```

```cpp
this->languageTag = Windows::Globalization::ApplicationLanguages::Languages->GetAt(0);

// For bidirectional languages, determine flow direction for the root layout panel, and all contained UI.

auto flowDirectionSetting = Windows::ApplicationModel::Resources::Core::ResourceContext::GetForCurrentView()->QualifierValues->Lookup("LayoutDirection");
if (flowDirectionSetting == "LTR")
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::LeftToRight;
}
else
{
    this->layoutRoot->FlowDirection = Windows::UI::Xaml::FlowDirection::RightToLeft;
}
```

Das obige Verfahren macht **FlowDirection** zu einer Funktion der- `LayoutDirection` Eigenschaft der bevorzugten Benutzer Anzeige Sprache. Wenn Sie diese Logik aus irgendeinem Grund nicht benötigen, können Sie eine FlowDirection-Eigenschaft in ihrer App als Ressource verfügbar machen, die Lokalisierer für jede Sprache festlegen können, in die Sie übersetzt werden.

Fügen Sie zunächst in der Ressourcen Datei Ihrer APP (. resw) einen Eigenschafts Bezeichner mit dem Namen "MainPage. FlowDirection" (anstelle von "MainPage") hinzu, um einen beliebigen Namen zu verwenden. Verwenden Sie dann eine **x:UID** , um das Haupt **Seiten** Element (oder dessen Stammlayoutpanel oder Frame) mit diesem Eigenschaften Bezeichner zuzuordnen.

```xaml
<Page x:Uid="MainPage">
```

Anstatt eine einzelne Codezeile für alle Sprachen zu verwenden, hängt dies davon ab, dass diese Eigenschafts Ressource für jede übersetzte Sprache ordnungsgemäß übersetzt wird. Beachten Sie, dass es bei der Verwendung dieses Verfahrens zusätzliche Möglichkeiten für menschliche Fehler gibt.

## <a name="important-apis"></a>Wichtige APIs
* [FrameworkElement. FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [Languagefont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
* [Passen Sie Ihre Ressourcen für Sprache, Skalierung und andere Qualifizierer an.](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md)
