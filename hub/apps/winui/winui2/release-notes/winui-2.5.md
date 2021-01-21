---
title: WinUI 2.5 – Anmerkungen zu dieser Version
description: Versionshinweise zu WinUI 2.5 einschließlich neuer Features und Bugfixes.
ms.date: 12/01/2020
ms.topic: article
ms.openlocfilehash: 63cd87caff4425d3a5e29852d84c33e09c68ab84
ms.sourcegitcommit: 617344ae1a1f5b580c938b61e910d99120b73626
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/21/2021
ms.locfileid: "98620844"
---
# <a name="windows-ui-library-25"></a>Windows-UI-Bibliothek 2.5

WinUI 2.5 ist die neueste offizielle Version der Windows-UI-Bibliothek (WinUI).

WinUI ist ein Open-Source-Projekt, das auf GitHub im [Windows UI Library-Repository](https://aka.ms/winui) gehostet wird. Bitte registrieren Sie alle Fehlerberichte, Featureanforderungen und Communitycodebeiträge in diesem Repository.

WinUI-Versionen: [GitHub-Releaseseite](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI-Pakete können mithilfe des NuGet-Paket-Managers zu Visual Studio-Projekten hinzugefügt werden. Weitere Informationen finden Sie unter [Erste Schritte mit der Windows-UI-Bibliothek](../getting-started.md).

Download des NuGet-Pakets: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Neue Funktionen

### <a name="infobar"></a>InfoBar

Das Steuerelement [InfoBar](/windows/uwp/design/controls-and-patterns/infobar) wird verwendet, um App-weite Statusmeldungen anzuzeigen, die für Benutzer deutlich sichtbar, aber auch nicht aufdringlich sind. Das Steuerelement enthält eine Eigenschaft „Severity“ (Schweregrad), mit der der Typ der angezeigten Meldung angegeben wird, sowie eine Option zum Festlegen einer Schaltfläche für eigene Handlungsaufforderungen oder Links. Wird das InfoBar-Steuerelement inline mit anderem Benutzeroberflächeninhalt angezeigt, können Sie zudem angeben, ob es immer sichtbar ist oder ob es vom Benutzer abgewiesen werden kann.

Dieses Beispiel zeigt ein InfoBar-Steuerelement im Standardzustand mit einer Schließen-Schaltfläche und einer Meldung.

:::image type="content" source="../images/infobar-default-title-message.png" alt-text="Beispiel eines InfoBar-Steuerelements im Standardzustand mit einer Schließen-Schaltfläche und einer Meldung.":::

Dieses animierte Beispiel zeigt ein InfoBar-Steuerelement mit verschiedenen Schweregraden und benutzerdefinierten Meldungen.

:::image type="content" source="../images/infobar-severity-animated.gif" alt-text="Animiertes Beispiel von InfoBar-Schweregraden und benutzerdefinierten Meldungen.":::

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/infobar)

[API-Referenz](/windows/winui/api/microsoft.ui.xaml.controls.infobar)

### <a name="determinate-progressring"></a>Bestimmtes ProgressRing-Steuerelement

Der bestimmte Status des [ProgressRing](/windows/uwp/design/controls-and-patterns/progress-controls)-Steuerelements gibt an, zu welchem Prozentsatz eine Aufgabe abgeschlossen ist. Dieses Element sollte für Vorgänge verwendet werden, deren Dauer bekannt ist und deren Fortschritt nicht die Interaktion des Benutzers mit der App blockieren sollte.

Das folgende animierte Bild veranschaulicht ein bestimmte ProgressRing-Steuerelement.

:::image type="content" source="../images/progressring-determinate-mode-animated.gif" alt-text="Animiertes Beispiel für ein bestimmtes ProgressRing-Steuerelement.":::<br>

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/progress-controls#progress-controls-best-practices)

[API-Referenz](/windows/winui/api/microsoft.ui.xaml.controls.progressring)


### <a name="navigationview-footermenuitems"></a>FooterMenuItems-Eigenschaft des NavigationView-Steuerelements

Verwenden Sie die FooterMenuItems-Eigenschaft des NavigationView-Steuerelements, um Navigationselemente an das Ende des Navigationsbereichs zu platzieren (im Gegensatz zur MenuItems-Eigenschaft, mit der Elemente an den Anfang des Bereichs platziert werden).

Die folgende Abbildung zeigt ein NavigationView-Steuerelement mit den Navigationselementen *Konto*, *Ihr Warenkorb* und *Hilfe* im Fußzeilenmenü.

:::image type="content" source="../images/navigationview-footermenuitems.png" alt-text="Beispiel für ein NavigationView-Steuerelement mit den Navigationselementen „Konto“, „Ihr Warenkorb“ und „Hilfe“ im Fußzeilenmenü":::.

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/navigationview?#footer-menu-items)

[API-Referenz](/windows/winui/api/microsoft.ui.xaml.controls.navigationview.footermenuitems)

## <a name="samples"></a>Beispiele

Die Beispiel-App **XAML-Steuerelementekatalog** enthält Beispiele für jede dieser WinUI-Features und -Steuerelemente.

Wenn Sie die App **XAML-Steuerelementekatalog** installiert und auf die neueste Version aktualisiert haben:

- Sehen Sie sich das [InfoBar](xamlcontrolsgallery:/item/InfoBar)-Steuerelement in Aktion an.
- Sehen Sie sich das [ProgressRing](xamlcontrolsgallery:/item/ProgressRing)-Steuerelement in Aktion an.
- Sehen Sie sich das [NavigationView](xamlcontrolsgallery:/item/NavigationView)-Steuerelement in Aktion an.

Wenn die XAML-Steuerelementekatalog-App auf Ihrem System nicht installiert ist, rufen Sie sie im [Microsoft Store](https://aka.ms/xamlgalleryapp) ab.

Der Quellcode des XAML-Steuerelementekatalogs kann ferner auf [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) angezeigt, geklont und erstellt werden.

## <a name="other-updates"></a>Andere Updates

In unserer Liste [Relevante Änderungen](https://github.com/microsoft/microsoft-ui-xaml/releases/tag/v2.5.0) finden Sie viele der in diesem Release behobenen GitHub-Probleme.
