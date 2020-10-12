---
title: WinUI 2.1 – Anmerkungen zu dieser Version
description: Versionshinweise zu WinUI 2.1 einschließlich neuer Features und Bugfixe.
ms.date: 07/15/2020
ms.topic: article
ms.openlocfilehash: f5087e9f5059a568e92f972c04b25d8c618015f2
ms.sourcegitcommit: a30808f38583f7c88fb5f54cd7b7e0b604db9ba6
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/06/2020
ms.locfileid: "91762875"
---
# <a name="windows-ui-library-21"></a>Windows-UI-Bibliothek 2.1

Die neueste offizielle Version der Windows UI-Bibliothek – WinUI 2.1 – wurde am 8. April 2019 veröffentlicht 

Mit WinUI erhältst du viele der aktuellsten Features der Windows UX-Plattform, einschließlich aktueller Fluent-Steuerelemente und Formatvorlagen, die in einer sofort nutzbaren Weise zur Verfügung stehen und mit Windows 10 Anniversary Update (14393) kompatibel sind. Der [XAML-Steuerelementkatalog](/windows/uwp/design/controls-and-patterns/#xaml-controls-gallery) gibt Beispiele, anhand derer du all die coolen neuen Features erkunden kannst, die der Bibliothek hinzugefügt wurden.

Das [WinUI 2.1 NuGet-Paket](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004) herunterladen

Du kannst dich entscheiden, die WinUI-Pakete in deiner App zu verwenden, und dazu den NuGet-Paket-Manager einsetzen: Weitere Informationen findest du unter [Erste Schritte mit der Windows-UI-Bibliothek](/uwp/toolkits/winui/getting-started).

WinUI ist ein Open-Source-Projekt, das auf GitHub gehostet wird. Wir freuen uns über Fehlerberichte, Featureanforderungen und Communitycodebeiträge im [Repository zur Windows-UI-Bibliothek](https://aka.ms/winui).

## <a name="whats-new-in-this-release"></a>Neuigkeiten in dieser Version

### <a name="itemsrepeater"></a>ItemsRepeater

Verwende ein ItemsRepeater-Steuerelement, um benutzerdefinierte Sammlungsoberflächen mit einem flexiblen Layoutsystem, benutzerdefinierten Ansichten und Virtualisierung zu erstellen.
Im Gegensatz zu ListView stellt ItemsRepeater keine umfassende Benutzeroberfläche bereit– ItemsRepeater hat keine Standardbenutzeroberfläche und stellt keine Richtlinien hinsichtlich Fokus, Auswahl, oder Benutzerinteraktion bereit. Stattdessen ist das Steuerelement ein Baustein, den du dazu verwenden kannst, deine eigenen einzigartigen sammlungsbasierten Oberflächen und benutzerdefinierten Steuerelemente zu erstellen. Es unterstützt die Schaffung umfassender und leistungsfähigerer Benutzererfahrungen.

![Kurzes Video, das das Verhalten des Steuerelements „ItemsRepeater“ zeigt.](../images/ItemsRepeater%20-%20MSN%20News.gif)

[Dokumentation](/windows/uwp/design/controls-and-patterns/items-repeater)

### <a name="animatedvisualplayer"></a>AnimatedVisualPlayer

Der AnimatedVisualPlayer hostet und steuert die Wiedergabe von animierten visuellen Elementen, mit denen du deine App um sehr leistungsfähige benutzerdefinierte Bewegungsgrafiken bereichern kannst. Beispielsweise wird AnimatedVisualPlayer verwendet, um Lottie-Animationen anzuzeigen und zu steuern.

![Kurzes Video, das das Verhalten des Steuerelements „AnimatedVisualPlayer“ zeigt.](../images/AnimatedVisualPlayerUpdated.gif)

[Dokumentation](/windows/communitytoolkit/animations/lottie)

### <a name="teachingtip"></a>TeachingTip

TeachingTip bietet ein fesselndes, mit dem Fluent-Design kompatibles Verfahren, Benutzer mit inhaltsreichen Tipps anzuleiten und zu informieren, die sich nicht aufdrängen. TeachingTip kann die Aufmerksamkeit auf neue oder wichtige Funktionen lenken, Benutzern die Erledigung von Aufgaben erklären und den Workflow verbessern, indem im Kontext relevante Informationen für die aktuelle Aufgabe angezeigt werden.

![Kurzes Video, das das Verhalten des Steuerelements „TeachingTip“ zeigt.](../images/TeachingTipUpdated.gif)

[Dokumentation](/windows/uwp/design/controls-and-patterns/dialogs-and-flyouts/teaching-tip)

### <a name="radiomenuflyoutitem"></a>RadioMenuFlyoutItem

Bietet die Möglichkeit, Optionen im Format von Optionsfeldern in einer Menüleiste zu verwenden. Dies macht es möglich, Optionen mit Aufzählungspunkten zu Gruppen zusammenzufassen, die wie die Stationstasten eines Autoradios aneinander gekoppelt sind. Die Logik wird dem Entwickler abgenommen.

![Screenshot, der das Verhalten des Steuerelements „RadioMenuFlyoutItem“ zeigt.](../images/RadioMenuFlyoutItem1.png)

[Dokumentation](/windows/uwp/design/controls-and-patterns/menus#create-a-menu-flyout-or-a-context-menu)

### <a name="compactdensity"></a>CompactDensity

Der Kompaktmodus ermöglicht es Entwicklern, komfortable Benutzererfahrungen für eine beliebige Anzahl von Szenarien zu erstellen. Durch einfaches Hinzufügen eines Ressourcenwörterbuchs zu deiner Anwendung kannst du durchschnittlich ~33 % mehr Benutzeroberfläche unterbringen.

![Screenshot, der das Verhalten des Steuerelements „CompactDensity“ zeigt.](../images/CompactDensityUpdated.png)

[Dokumentation](/windows/uwp/design/style/spacing)

### <a name="shadows"></a>Schatten

![Beispiel](../images/shadow.gif)

Das Erstellen einer visuellen Hierarchie in deiner Benutzeroberfläche macht es einfach, die Benutzeroberfläche zu durchsuchen und transportiert die Information, was wichtig ist und beachtet werden muss. Die Erhöhung der Rechte, das In-den-Vordergrund-Stellen bestimmter Elemente deiner Benutzeroberfläche, wird häufig verwendet, um eine solche Hierarchie in Software zu erreichen. 

Seit dem Windows 10-Update aus Mai 2019 fügen viele unserer allgemeinen Steuerelemente Hervorhebung standardmäßig mithilfe von Z-Tiefe und Schatten hinzu. Die Steuerelemente NavigationView und TeachingTip in WinUI 2.1 weisen ebenfalls standardmäßig Schatten auf, wenn sie unter einem Betriebssystem mit dem Windows 10-Update aus Mai 2019 ausgeführt werden. Die vollständige Liste der Steuerelemente, die standardmäßig über Schatten verfügen, und Informationen zur Verwendung der zusätzlichen APIs stehen nach der Veröffentlichung des Windows 10-Updates aus Mai 2019 bereit, der Link wird hier eingefügt.

## <a name="examples"></a>Beispiele

Die Beispiel-App des XAML-Steuerelementkatalogs enthält interaktive Demos und Beispielcode für die Verwendung von WinUI-Steuerelementen.

* XAML-Steuerelementkatalog-App aus dem [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt) installieren

* Der XAML-Steuerelementkatalog ist auch ein [Open-Source-Projekt auf GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Dokumentation

Anleitungen für Steuerelemente der Windows-UI-Bibliothek sind in der [Dokumentation zu Steuerelementen der universellen Windows-Plattform](/windows/uwp/design/controls-and-patterns/) enthalten.

API-Referenzdokumente findest du hier: [Windows-UI-Bibliotheks-APIs](/uwp/api/overview/winui/)

## <a name="microsoftuixaml-21-version-history"></a>Microsoft.UI.Xaml 2.1 Version History

### <a name="microsoftuixaml-21-official-release"></a>Microsoft.UI.Xaml 2.1 – offizielle Version

April 2019

[GitHub-Releaseseite](https://github.com/Microsoft/microsoft-ui-xaml/releases)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190405004)

#### <a name="new-feature-not-included-in-earlier-pre-releases"></a>Neues Feature (in früheren Vorabversionen nicht enthalten)

* [CompactDensity](/windows/uwp/design/style/spacing): Der Kompaktmodus ermöglicht es Entwicklern, komfortable Benutzererfahrungen für eine beliebige Anzahl von Szenarien zu erstellen. Durch einfaches Hinzufügen eines Ressourcenwörterbuchs zu deiner Anwendung kannst du durchschnittlich ~33 % mehr Benutzeroberfläche unterbringen.

* Schatten: Das Erstellen einer visuellen Hierarchie in deiner Benutzeroberfläche macht es einfach, die Benutzeroberfläche zu durchsuchen und transportiert die Information, was wichtig ist und beachtet werden muss. Die Erhöhung der Rechte, das In-den-Vordergrund-Stellen bestimmter Elemente deiner Benutzeroberfläche, wird häufig verwendet, um eine solche Hierarchie in Software zu erreichen. Viele unserer allgemeinen Steuerelemente fügen Hervorhebung standardmäßig mithilfe von Z-Tiefe und Schatten hinzu.  

### <a name="microsoftuixaml-21190218001-prerelease"></a>Microsoft.UI.Xaml 2.1.190218001-Vorschauversion

Februar 2019

[GitHub-Releaseseite](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190219001-prerelease)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190218001-prerelease)

Neue experimentelle Features:

* [TeachingTip-Steuerelement](https://github.com/Microsoft/microsoft-ui-xaml/issues/21)  
  Dieses neue Steuerelement stellt eine Möglichkeit für deine App dar, Benutzer in der Anwendung mit inhaltsreichen Benachrichtigungen zu leiten und zu informieren, ohne aufdringlich zu sein. TeachingTip kann die Aufmerksamkeit auf eine neue oder wichtige Funktion lenken, Benutzern die Erledigung von Aufgaben erklären und den Workflow von Benutzern verbessern, indem im Kontext relevante Informationen für die aktuelle Aufgabe angezeigt werden.

### <a name="microsoftuixaml-21190131001-prerelease"></a>Microsoft.UI.Xaml 2.1.190131001-Vorschauversion

Februar 2019

[GitHub-Releaseseite](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.190131001-prerelease)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.190131001-prerelease)

Neue experimentelle Features:

* [AnimatedVisualPlayer](/uwp/api/microsoft.ui.xaml.controls.animatedvisualplayer)  
  Dieses neue Steuerelement ermöglicht die Wiedergabe komplexer Hochleistungs-Vektoranimationen, einschließlich der [Lottie-](https://github.com/airbnb/lottie)Animationen, die mit [Lottie-Windows](/windows/communitytoolkit/animations/lottie) erstellt wurden.

### <a name="microsoftuixaml-21181217001-prerelease"></a>Microsoft.UI.Xaml 2.1.181217001-Vorschauversion

Dezember 2018

[GitHub-Releaseseite](https://github.com/Microsoft/microsoft-ui-xaml/releases/tag/v2.1.181217001-prerelease)

[Download des NuGet-Pakets](https://www.nuget.org/packages/Microsoft.UI.Xaml/2.1.181217001-prerelease)

Neue experimentelle Features:

* [ItemsRepeater](/uwp/api/microsoft.ui.xaml.controls.itemsrepeater)

* [Optionsschaltflächen](/uwp/api/microsoft.ui.xaml.controls.radiobuttons)

* [RadioMenuFlyoutItem](/uwp/api/microsoft.ui.xaml.controls.radiomenuflyoutitem)