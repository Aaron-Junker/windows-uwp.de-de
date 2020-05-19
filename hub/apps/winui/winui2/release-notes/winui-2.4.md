---
title: WinUI 2.4 – Anmerkungen zu dieser Version
description: Versionshinweise zu WinUI 2.4 einschließlich neuer Features und Bugfixe.
ms.date: 05/08/2020
ms.topic: reference
ms.openlocfilehash: 88b75ae5ec0fbda545106ec0e7f7b86033808f70
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580307"
---
# <a name="windows-ui-library-24"></a>Windows-UI-Bibliothek 2.4

WinUI 2.4 ist die neueste offizielle Version der Windows-Benutzeroberflächenbibliothek (WinUI).

WinUI ist ein Open-Source-Projekt, das auf GitHub im [Windows UI Library-Repository](https://aka.ms/winui) gehostet wird. Bitte registrieren Sie alle Fehlerberichte, Featureanforderungen und Communitycodebeiträge in diesem Repository.

WinUI-Versionen: [GitHub-Releaseseite](https://github.com/microsoft/microsoft-ui-xaml/releases)

WinUI-Pakete können mithilfe des NuGet-Paket-Managers zu Visual Studio-Projekten hinzugefügt werden. Weitere Informationen finden Sie unter [Erste Schritte mit der Windows-UI-Bibliothek](../getting-started.md).

Download des NuGet-Pakets: [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml)

## <a name="new-features"></a>Neue Funktionen

### <a name="radialgradientbrush"></a>RadialGradientBrush

Ein RadialGradientBrush wird innerhalb einer Ellipse gezeichnet, die durch die Eigenschaften Center, RadiusX und RadiusY definiert wird. Farben für den Farbverlauf beginnen in der Mitte der Ellipse und enden am Radius.

![Radialer Farbverlaufspinsel](../images/radialgradientbrush.gif)<br>
*Radialer Farbverlaufspinsel*

[Nutzungsrichtlinien](/windows/uwp/design/style/brushes#radial-gradient-brushes)

[API-Referenz](/uwp/api/microsoft.ui.xaml.media.radialgradientbrush)

### <a name="progressring"></a>ProgressRing

Das ProgressRing-Steuerelement ist für modale Interaktionen vorgesehen, in denen die Aktivitäten des Benutzers blockiert werden, bis das ProgressRing-Element nicht mehr angezeigt wird. Verwenden Sie dieses Steuerelement, wenn es für einen Vorgang erforderlich ist, dass die meisten Interaktionen mit der App angehalten werden, bis der Vorgang beendet ist.

![ProgressRing-Steuerelement](../images/progressring.gif)<br>
*ProgressRing-Steuerelement*

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/progress-controls)

[API-Referenz](/uwp/api/microsoft.ui.xaml.controls.progressring)

### <a name="tabview-updates"></a>TabView-Updates

Durch die Updates des TabView-Steuerelements haben Sie mehr Kontrolle über die Darstellung von Registerkarten.

Sie können die Breite nicht ausgewählter Registerkarten festlegen und lediglich ein Symbol anzeigen, um Bildschirmplatz zu sparen:

![Registerkartengrößen des TabView-Steuerelements](..\images\tabview-sizing.gif)<br>
*Registerkartengrößen des TabView-Steuerelements*

Sie können ferner die Schließen-Schaltfläche auf nicht ausgewählten Registerkarten ausblenden, bis der Benutzer mit dem Mauszeiger auf die Registerkarte zeigt (in früheren Versionen wurde sie immer angezeigt):

![TabView-Steuerelement – Draufzeigen zum Schließen](..\images\tabview-closebuttononhover.gif)<br>
*TabView-Steuerelement – Draufzeigen zum Schließen*

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/tab-view)

[API-Referenz](/uwp/api/microsoft.ui.xaml.controls.tabview)

### <a name="dark-theme-updates-to-textbox-family-of-controls"></a>Updates des dunklen Designs für die Familie der TextBox-Steuerelemente

Wenn das dunkle Design aktiviert ist, bleibt die Hintergrundfarbe in der Familie der TextBox-Steuerelemente bei Texteinfügung standardmäßig dunkel (in früheren Versionen wechselte die Hintergrundfarbe während der Texteinfügung zu Weiß).

| Vor | Nach |
| - | - |
| ![Updates des dunklen Designs für TextBox (vorher)](..\images\textbox-darkthemeupdates-before1.gif)<br>*Updates des dunklen Designs für TextBox (vorher)* | ![Updates des dunklen Designs für TextBox (nachher)](..\images\textbox-darkthemeupdates-after1.gif)<br>*Updates des dunklen Designs für TextBox (nachher)* |
| ![Updates des dunklen Designs für TextBox (vorher)](..\images\textbox-darkthemeupdates-before2.gif)<br>*Updates des dunklen Designs für TextBox (vorher)* | ![Updates des dunklen Designs für TextBox (nachher)](..\images\textbox-darkthemeupdates-after2.gif)<br>*Updates des dunklen Designs für TextBox (nachher)* |

Nachfolgend sind einige der Steuerelemente aus der TextBox-Steuerelementfamilie aufgeführt:

- [TextBox](/uwp/api/windows.ui.xaml.controls.textbox)
- [RichEditBox](/uwp/api/windows.ui.xaml.controls.richtextblock)
- [PasswordBox](/uwp/api/windows.ui.xaml.controls.passwordbox)
- [Bearbeitbare ComboBox](/uwp/api/windows.ui.xaml.controls.combobox)
- [AutoSuggestBox](/uwp/api/windows.ui.xaml.controls.autosuggestbox)

### <a name="hierarchical-navigation"></a>Hierarchische Navigation

Das [NavigationView](/uwp/api/microsoft.ui.xaml.controls.navigationview?view=winui-2.4)-Steuerelement unterstützt nun die hierarchische Navigation und umfasst die Anzeigemodi „Left“, „Top“ und „LeftCompact“. Ein hierarchisches NavigationView-Steuerelement ist beim Anzeigen der Kategorien von Seiten, beim Identifizieren von Seiten mit zugeordneten Unterseiten oder bei der Verwendung in Apps mit Seiten im Hubstil nützlich, die Verknüpfungen zu vielen weiteren Seiten aufweisen.

![Hierarchical NavigationView-Steuerelement](..\images\HierarchicalNavView.gif)<br>*Hierarchical NavigationView-Steuerelement*

[Nutzungsrichtlinien](/windows/uwp/design/controls-and-patterns/navigationview#hierarchical-navigation)

[API-Referenz](/uwp/api/microsoft.ui.xaml.controls.navigationview)

## <a name="samples"></a>Beispiele

Beispiele für jedes der beschriebenen WinUI 2.4-Features sind im **XAML-Steuerelementekatalog** verfügbar.

Wenn die XAML-Steuerelementekatalog-App auf Ihrem System nicht installiert ist, rufen Sie sie im [Microsoft Store](https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) ab.

Der Quellcode des XAML-Steuerelementekatalogs kann ferner auf [GitHub](https://github.com/Microsoft/Xaml-Controls-Gallery) angezeigt werden.
