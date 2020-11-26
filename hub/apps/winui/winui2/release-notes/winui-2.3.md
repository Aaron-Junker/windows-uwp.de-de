---
title: WinUI 2.3 – Anmerkungen zu dieser Version
description: Versionshinweise zu WinUI 2.3 einschließlich neuer Features und Bugfixe.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: ad589b15b5481b5b7ff402fc2043afd71c139479
ms.sourcegitcommit: 67c4d4ecda4ffe5f1a233de5e8555ca2228e8489
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 11/19/2020
ms.locfileid: "94933065"
---
# <a name="windows-ui-library-23"></a>Windows-UI-Bibliothek 2.3

WinUI 2.3 ist die neueste offizielle Version der Windows-Benutzeroberflächenbibliothek (WinUI).

WinUI ist ein Open-Source-Projekt, das auf GitHub im [Windows UI Library-Repository](https://aka.ms/winui) gehostet wird. Bitte registrieren Sie alle Fehlerberichte, Featureanforderungen und Communitycodebeiträge in diesem Repository.

WinUI-Versionen: [GitHub-Releaseseite](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI-Pakete können mithilfe des NuGet-Paket-Managers zu Visual Studio-Projekten hinzugefügt werden. Weitere Informationen finden Sie unter [Erste Schritte mit der Windows-UI-Bibliothek](../getting-started.md).

Download des NuGet-Pakets: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Neue Funktionen

### <a name="progress-bar-visual-refresh"></a>Visuelle Aktualisierung der Statusanzeige

Die **ProgressBar** weist zwei verschiedene visuelle Darstellungen auf.

#### <a name="indeterminate-progress-bar"></a>Unbestimmte Statusanzeige

Zeigt, dass aktuell eine Aufgabe ausgeführt wird, blockiert die Benutzerinteraktion aber nicht.

![Unbestimmte Statusanzeige](../images/IndeterminateProgressBar.gif)

#### <a name="determinate-progress-bar"></a>Festgelegte Statusanzeige

Zeigt, wie viel Fortschritt bei einer bekannten Menge Arbeit erzielt wurde. 

![Festgelegte Statusanzeige](../images/DeterminateProgressBar.gif)

[Dokumentationslink](/windows/uwp/design/controls-and-patterns/progress-controls)

[Beispiellink](/windows/uwp/design/controls-and-patterns/progress-controls#examples)

### <a name="numberbox"></a>NumberBox

Eine **NumberBox** stellt ein Steuerelement dar, das zum Anzeigen und Bearbeiten von Zahlen verwendet werden kann. Dies unterstützt Validierung, inkrementelle Schritte und das Berechnen von Inlineberechnungen einfacher Gleichungen, wie Multiplikation, Division, Addition und Subtraktion.

![NumberBox](../images/NumberBoxGif.gif)

[Dokumentations- und Beispiellink](/windows/uwp/design/controls-and-patterns/number-box)

### <a name="radiobuttons"></a>RadioButtons

**RadioButtons** ist ein neues Containersteuerelement, das es Ihnen ermöglicht, problemlos Gruppen von verwandten RadioButton-Elementen zu erstellen, während zugleich die Funktionen „Keyboardunterstützung“ und „Sprachausgabe“ ordnungsgemäß unterstützt werden

![Screenshot der drei Optionsfelder, wobei das dritte ausgewählt ist.](../images/RadioButtons.png)

[Dokumentations- und Beispiellink](https://github.com/microsoft/microsoft-ui-xaml-specs/blob/c8d3d3668af546091656dfc37436b13cd062f52d/active/radiobuttons/RadioButtons_Spec.md)

## <a name="examples"></a>Beispiele

Die Beispiel-App des XAML-Steuerelementkatalogs enthält interaktive Demos und Beispielcode für die Verwendung von WinUI-Steuerelementen.

* XAML-Steuerelementkatalog-App aus dem [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) installieren

* Der XAML-Steuerelementkatalog ist auch ein [Open-Source-Projekt auf GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Dokumentation

Anleitungen für Steuerelemente der Windows-UI-Bibliothek sind in der [Dokumentation zu Steuerelementen der universellen Windows-Plattform](/windows/uwp/design/controls-and-patterns/) enthalten.

API-Referenzdokumente findest du hier: [Windows-UI-Bibliotheks-APIs](/windows/winui/api/)