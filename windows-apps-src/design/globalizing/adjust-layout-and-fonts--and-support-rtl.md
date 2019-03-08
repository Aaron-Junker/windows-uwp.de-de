---
Description: Entwerfen Sie Ihre App so, dass die Layouts und Schriften mehrerer Sprachen unterstützen – beispielsweise auch den Textfluss von rechts nach links (right-to-left, RTL).
title: Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“
ms.assetid: F2522B07-017D-40F1-B3C8-C4D0DFD03AC3
label: Adjust layout and fonts, and support RTL
template: detail.hbs
ms.date: 05/11/2018
ms.topic: article
keywords: Windows 10, UWP, Lokalisierbarkeit, Lokalisierung, rtl, ltr
ms.localizationpriority: medium
ms.openlocfilehash: e428dd068337ecd79992e8e27cd193bed112d9c2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57645305"
---
# <a name="adjust-layout-and-fonts-and-support-rtl"></a>Anpassen von Layout und Schriftarten und Unterstützen von „Von rechts nach links“
Entwerfen Sie Ihre App so, dass die Layouts und Schriften mehrerer Sprachen unterstützen – beispielsweise auch den Textfluss von rechts nach links (right-to-left, RTL). Die Textflussrichtung ist die Richtung, in der ein Text geschrieben und angezeigt wird, und die UI-Elemente auf der Seite werden entsprechend gelesen.

## <a name="layout-guidelines"></a>Layoutrichtlinien
Sprachen wie Deutsch und Finnisch benötigen in der Regel mehr Zeichen als Englisch. Fernöstliche Schriften benötigen normalerweise mehr Höhe. Und bei Sprachen wie Arabisch und Hebräisch muss die Leserichtung in Textfeldern und des App-Elementen von rechts nach links (RTL) verlaufen.

Wegen dieser Unterschiede in den Metriken des übersetzten Textes empfehlen wir, dass Sie keine absolute Positionierung, feste Breiten oder feste Höhen in Ihrer Benutzeroberfläche (UI) verwenden. Nutzen Sie stattdessen die dynamischen Layoutmechanismen, die in den Windows-UI-Elementen integriert sind. Beispielsweise werden Inhaltssteuerelemente (z. B. Schaltflächen), Elementsteuerelemente (z. B. Grid- und Listenansichten) sowie Layoutfelder (z. B. Grids und Stackpanels) automatisch an ihre Inhalte angepasst. Durch die Pseudolokalisierung für die App werden problematische Fälle aufgedeckt, in denen UI-Elemente nicht richtig an den Inhalt angepasst sind.

Die empfohlene Methode ist ein dynamisches Layout, das in den meisten Fällen anwendbar ist. Weniger empfehlenswert, aber immer noch besser als das Festlegen von Größen im Markup für die Benutzeroberfläche, ist das Festlegen von Layoutwerten als Funktion der Sprache. Es folgt ein Beispiel für die Bereitstellung einer Width-Eigenschaft in Ihrer App als Ressource, die von Lokalisierern der jeweiligen Sprache entsprechend festgelegt werden kann. Fügen Sie zunächst in der Ressourcendatei Ihrer App (.resw) eine Eigenschaftskennung mit dem Namen „TitleText.Width” hinzu (anstelle von „TitleText” können Sie einen beliebigen Namen verwenden). Verwenden Sie dann **x:Uid**, um UI-Elemente mit dieser Eigenschaftskennung zu verknüpfen.

```xaml
<TextBlock x:Uid="TitleText">
```

Weitere Informationen zu Zeichenfolgenressourcen-Bezeichner und Ressourcendateien (.resw) finden Sie unter Lokalisieren der Zeichenfolge im Paketmanifest der Benutzeroberfläche und der App.Weitere Informationen zu Ressourcendateien (.resw), Eigenschaften-IDs und **x:Uid** finden Sie unter [Lokalisieren von Zeichenfolgen in der Benutzeroberfläche und im App-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md).

## <a name="fonts"></a>Schriftarten
Verwenden Sie die [**LanguageFont**](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)-Schriftzuordnungsklasse für den programmgesteuerten Zugriff auf die empfohlene Familie und Breite und den empfohlenen Grad und Schnitt der Schriftart für eine spezielle Sprache. Die **LanguageFont**-Klasse ermöglicht den Zugriff auf die richtigen Schriftartinformationen für verschiedene Inhaltskategorien: UI-Kopfzeilen, Benachrichtigungen, Textkörper und Schriften für den Textkörper, die vom Benutzer bearbeitet werden können.

## <a name="mirroring-images"></a>Spiegeln von Bildern
Wenn Ihre App Bilder enthält, die für die Leserichtung rechts nach links (RTL) gespiegelt werden müssen (Umkehrung des Bilds), können Sie die **FlowDirection**-Eigenschaft verwenden.

```xaml
<!-- en-US\localized.xaml -->
<Image ... FlowDirection="LeftToRight" />

<!-- ar-SA\localized.xaml -->
<Image ... FlowDirection="RightToLeft" />
```

Wenn die App ein anderes Bild benötigt, damit dieses richtig umgedreht werden kann, können Sie das Ressourcenverwaltungssystem mit dem `LayoutDirection`-Qualifizierer verwenden (s. LayoutDirection-Abschnitt in [Ressourcen an Sprache, Skalierung und andere Qualifikationsmerkmale anpassen](../../app-resources/tailor-resources-lang-scale-contrast.md#layoutdirection)). Das System wählt ein Bild mit dem Namen `file.layoutdir-rtl.png` aus, wenn die App-Laufzeitsprache (s. [Benutzerprofilsprachen und App-Manifest-Sprachen verstehen](manage-language-and-region.md))auf eine RTL-Sprache festgelegt wird. Diese Vorgehensweise ist möglicherweise nötig, wenn nicht alle Teile des Bilds umgedreht werden.

## <a name="handling-right-to-left-rtl-languages"></a>Behandeln von rechts-nach-links-Sprachen (RTL-Sprachen)
Wenn Ihre App für Rechts-Links-Sprachen (RTL) lokalisiert wird, verwenden Sie die Eigenschaft [**FrameworkElement.FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection), und legen Sie symmetrische Abstände und Ränder fest. Layoutpanels wie [**Grid**](/uwp/api/Windows.UI.Xaml.Controls.Grid?branch=live) skalieren und spiegeln sich automatisch gemäß dem von Ihnen festgelegten Wert für **FlowDirection**.

Legen Sie **FlowDirection** im Stammlayoutpanel (oder Frame) der Seite oder auf der Seite selbst fest. Dadurch erben alle enthaltenen Steuerelemente dieses Eigenschaft.

> [!IMPORTANT]
> Allerdings wird **FlowDirection***nicht* automatisch basierend auf der vom Benutzer in den Windows-Einstellungen ausgewählten Anzeigesprache festgelegt, und die Eigenschaft ändert sich auch nicht dynamisch, wenn der Benutzer die Anzeigesprache wechselt. Stellt der Benutzer beispielsweise die Windows-Einstellungen von Englisch auf Arabisch um, ändert sich die **FlowDirection**-Eigenschaft *nicht* automatisch von links nach rechts zu rechts nach links. Als App-Entwickler müssen Sie **FlowDirection** für die Sprache festlegen, die gerade angezeigt wird.

Die programmatische Technik besteht darin, die Eigenschaft `LayoutDirection` der bevorzugten Benutzeranzeigesprache zu verwenden, um die Eigenschaft [**FlowDirection**](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) festzulegen (siehe Codebeispiel unten). Die meisten Steuerelemente in Windows verwenden **FlowDirection** bereits. Wenn Sie ein benutzerdefiniertes Steuerelement implementieren, sollte es **FlowDirection** verwenden, um entsprechende Layoutänderungen für RTL- und LTR-Sprachen vorzunehmen.

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

Die beschriebene Technik macht **FlowDirection** zu einer Funktion der `LayoutDirection`-Eigenschaft der bevorzugten Anzeigesprache. Wenn Sie diese Logik aus irgendeinem Grund nicht verwenden möchten, können Sie in Ihrer App eine FlowDirection-Eigenschaft als Ressource bereitstellen, die Lokalisierer für jede Sprache, in die sie übersetzen, entsprechend festlegen können.

Fügen Sie zunächst in der Ressourcendatei Ihrer App (.resw) eine Eigenschaften-ID mit dem Namen „MainPage.FlowDirection” hinzu (anstelle von „MainPage” können Sie einen beliebigen Namen verwenden). Verwenden Sie dann **x:Uid**, um Ihr hauptsächliches **Page**-Element (oder dessen Root-Layoutpanel bzw. -Frame) mit dieser Eigenschaftskennung zu verknüpfen.

```xaml
<Page x:Uid="MainPage">
```

Statt von einer einzelnen Codezeile für alle Sprachen sind Sie nun davon abhängig, dass der Übersetzer diese Eigenschaftsressource für jede übersetzte Sprache korrekt „übersetzt”. Seien Sie sich daher bewusst, dass diese Technik eine zusätzliche Quelle für menschliche Fehler sein kann.

## <a name="important-apis"></a>Wichtige APIs
* [FrameworkElement.FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection)
* [LanguageFont](/uwp/api/Windows.Globalization.Fonts.LanguageFont?branch=live)

## <a name="related-topics"></a>Verwandte Themen
* [Lokalisieren von Zeichenfolgen in Ihre Benutzeroberfläche und app-Paketmanifest](../../app-resources/localize-strings-ui-manifest.md)
* [Passen Sie Ihre Ressourcen für die Sprache, Skalierung und andere Kennzeichner](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Grundlegendes zu Benutzersprachen-Profil und app-manifest-Sprachen](manage-language-and-region.md)