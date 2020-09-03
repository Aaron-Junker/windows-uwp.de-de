---
title: WinUI 2.2 – Anmerkungen zu dieser Version
description: Versionshinweise zu WinUI 2.2 einschließlich neuer Features und Bugfixe.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: 4c200701e0d845d6c9b9f8797899d88cc72d8c1c
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89154904"
---
# <a name="windows-ui-library-22"></a>Windows-UI-Bibliothek 2.2

WinUI 2.2 ist die neueste offizielle Version der Windows-Benutzeroberflächenbibliothek.

Du kannst deiner App WinUI-Pakete mit dem NuGet-Paket-Manager hinzufügen. Weitere Informationen findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](../getting-started.md).

WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Wir freuen uns über Fehlerberichte, Featureanforderungen und Communitycodebeiträge im [Repository zur Windows-UI-Bibliothek](https://aka.ms/winui).

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 – Versionsverlauf

### <a name="windows-ui-library-22-official-release"></a>Windows-UI-Bibliothek 2.2 – Offizielle Veröffentlichung

AUGUST 2019

[GitHub-Releaseseite](https://github.com/microsoft/microsoft-ui-xaml/releases)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml)

### <a name="new-features"></a>Neue Funktionen

#### <a name="tabview"></a>TabView

![Beispiel](../images/tabview-gif.gif)

#### <a name="description"></a>Beschreibung

Das TabView-Steuerelement ist eine Sammlung von Registerkarten, die jeweils eine neue Seite oder ein neues Dokument in deiner App darstellen. TabView ist nützlich, wenn deine App mehrere Seiten Inhalt aufweist und der Benutzer die Möglichkeit erwartet, Registerkarten hinzuzufügen, zu schließen und neu anzuordnen. Das neue [Windows-Terminal](https://github.com/Microsoft/Terminal) verwendet TabView, um mehrere Befehlszeilenoberflächen anzuzeigen.

#### <a name="documentation"></a>Dokumentation

https://docs.microsoft.com/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2

#### <a name="navigationview-updates"></a>NavigationView-Updates

##### <a name="a-navigationviews-back-button-update"></a>a) Update der Schaltfläche „Zurück“ in NavigationView

![Beispiel](../images/navigationview-back-button.gif)

##### <a name="description"></a>Beschreibung

Im Minimalmodus von NavigationView wird die Schaltfläche „Zurück“ nicht mehr ausgeblendet. Beim Öffnen und Schließen des Bereichs brauchen Benutzer ihren Cursor nicht mehr zu bewegen, um auf die Hamburger-Schaltfläche zu klicken. Dieses Feature funktioniert standardmäßig. Du brauchst keine Codeänderungen vorzunehmen, damit es funktioniert.

##### <a name="b-navigationview---no-auto-padding"></a>b) NavigationView – Kein automatisches Auffüllen

![Beispiel](../images/navigationview-no-auto-padding.png)

##### <a name="description"></a>Beschreibung

App-Entwickler können jetzt bei Verwendung des NavigationView-Steuerelements alle Pixel innerhalb des Fensters ihrer App in Anspruch nehmen und den Bereich der Titelleiste einbeziehen.

##### <a name="documentation"></a>Dokumentation

https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/navigationview#top-whitespace

#### <a name="visual-style-updates"></a>Updates des visuellen Stils

##### <a name="a-corner-radius-update"></a>a) Eckradius-Update

![Beispiel](../images/corner-radius.png)

##### <a name="description"></a>Beschreibung

Das Attribut „CornerRadius“ wurde hinzugefügt. Standardsteuerelemente wurden aktualisiert und verwenden jetzt leicht abgerundete Ecken. Entwickler können den Eckradius problemlos anpassen, um deiner App auf Wunsch einen einzigartigen Look zu geben.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink

https://github.com/microsoft/microsoft-ui-xaml/issues/524

##### <a name="b-border-thickness-update"></a>b) Update der Rahmenstärke

![Beispiel](../images/border-thickness.png)

##### <a name="description"></a>Beschreibung

Die Anpassbarkeit der Eigenschaft BorderThickness wurde verbessert. Standardsteuerelemente wurden aktualisiert, um die Umrisslinien zu verringern und ein klareres, vertrautes Aussehen zu erreichen.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink

https://github.com/microsoft/microsoft-ui-xaml/issues/835

##### <a name="c-button-visual-update"></a>c) Update der Visualisierung von Schaltflächen

![Beispiel](../images/button-hover-visual-update.png)

##### <a name="description"></a>Beschreibung: 
Die Visualisierung von Standardschaltflächen wurde aktualisiert, um die Umrisslinie zu entfernen, die beim Draufzeigen angezeigt wurde, und ein klareres Aussehen zu erreichen.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink:  
https://github.com/microsoft/microsoft-ui-xaml/issues/953

##### <a name="d-splitbutton-visual-update"></a>d) Update der SplitButton-Visualisierung

![Beispiel](../images/splitbutton-visual-update.png)

##### <a name="description"></a>Beschreibung: 
Die standardmäßige Visualisierung von SplitButton wurde aktualisiert, um sie besser von DropDownButton zu unterscheiden.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink: 
https://github.com/microsoft/microsoft-ui-xaml/issues/986

##### <a name="e-toggleswitch-visual-update"></a>e) Update der ToggleSwitch-Visualisierung

![Beispiel](../images/toggleswitch-update.png)

##### <a name="description"></a>Beschreibung: 
Die Standardbreite von ToggleSwitch wurde von 44 px auf 40 px verringert, um visuelle Ausgewogenheit zu erreichen, ohne die Nutzbarkeit einzuschränken.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink: 
https://github.com/microsoft/microsoft-ui-xaml/issues/836

##### <a name="f-checkbox-and-radiobutton-visual-update"></a>f) Update der Visualisierung von CheckBox und RadioButton

![Beispiel](../images/checkbox-radiobutton.png)

##### <a name="description"></a>Beschreibung: 
Die Visualisierungen von CheckBox und RadioButton wurden aktualisiert, um Einheitlichkeit mit den anderen Änderungen von Visualisierungen zu erreichen.

##### <a name="github-spec-link"></a>GitHub-Spezifikationslink: 
https://github.com/microsoft/microsoft-ui-xaml/issues/839

## <a name="examples"></a>Beispiele

Die Beispiel-App des XAML-Steuerelementkatalogs enthält interaktive Demos und Beispielcode für die Verwendung von WinUI-Steuerelementen.

* XAML-Steuerelementkatalog-App aus dem [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) installieren

* Der XAML-Steuerelementkatalog ist auch ein [Open-Source-Projekt auf GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Dokumentation

Anleitungen für Steuerelemente der Windows-UI-Bibliothek sind in der [Dokumentation zu Steuerelementen der universellen Windows-Plattform](/windows/uwp/design/controls-and-patterns/) enthalten.

API-Referenzdokumente findest du hier: [Windows-UI-Bibliotheks-APIs](/uwp/api/overview/winui/)

## <a name="microsoftuixaml-22-version-history"></a>Microsoft.UI.Xaml 2.2 – Versionsverlauf

### <a name="microsoftuixaml-22190702001-prerelease"></a>Microsoft.UI.Xaml 2.2.190702001-Vorschauversion

Juli 2019

[GitHub-Releaseseite](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.2.190702001-prerelease)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190702001-prerelease)

### <a name="experimental-feature"></a>Experimentelles Feature

* [TabView](/uwp/api/microsoft.ui.xaml.controls.tabview?view=winui-2.2)

### <a name="microsoftuixaml-2220190416001-prerelease"></a>Microsoft.UI.Xaml 2.2.20190416001-Vorschauversion

April 2019

[GitHub-Releaseseite](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.2.190416008-prerelease)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.2.190416008-prerelease)

#### <a name="experimental-features"></a>Experimentelle Features

* [FlowLayout](/uwp/api/microsoft.ui.xaml.controls.flowlayout)

* [LayoutPanel](/uwp/api/microsoft.ui.xaml.controls.layoutpanel)

* [RadioButtons](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [ScrollViewer](/uwp/api/microsoft.ui.xaml.controls.scrollviewer)