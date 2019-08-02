---
Description: Zeigen Sie verschiedene Teile der app in separaten Fenstern an.
title: Anzeigen mehrerer Ansichten für eine App
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: ee49b5fe5b5956e9069ea196c4d2e029b3a15763
ms.sourcegitcommit: 3cc6eb3bab78f7e68c37226c40410ebca73f82a9
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/02/2019
ms.locfileid: "68729517"
---
# <a name="show-multiple-views-for-an-app"></a>Anzeigen mehrerer Ansichten für eine App

![Drahtmodell, das eine App mit mehreren Fenstern zeigt.](images/multi-view.gif)

Sie können Ihren Benutzern zu mehr Produktivität verhelfen, indem Sie ihnen ermöglichen, unabhängige Teile der App in separaten Fenstern anzuzeigen. Wenn Sie mehrere Fenster für eine APP erstellen, wird jedes Fenster in der Taskleiste separat angezeigt. Die Benutzer können App-Fenster unabhängig voneinander verschieben, deren Größe ändern, Fenster anzeigen und ausblenden und zwischen App-Fenstern wechseln, als würde es sich um separate Apps handeln.

> **Wichtige APIs:** [Windows. UI. viewmanagement-Namespace](/uwp/api/windows.ui.viewmanagement), [Windows. UI. windowmanagement-Namespace](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>Wann sollte eine App mehrere Ansichten verwenden?

Es gibt eine Vielzahl von Szenarien, die von mehreren Ansichten profitieren können. Hier finden Sie einige Beispiele:

- Eine E-Mail-App, die Benutzern beim Verfassen einer neuen E-Mail eine Liste der empfangenen Nachrichten anzeigt.
- Eine Adressbuch-App, mit der Benutzer Kontaktinformationen für mehrere Personen nebeneinander vergleichen können.
- Eine Musikplayer-App, mit der Benutzer beim Durchsuchen einer Liste anderer verfügbarer Musiktitel sehen können, was wiedergegeben wird.
- Eine Notizen-App, mit der Benutzer Informationen von einer Seite in eine andere kopieren können.
- Eine Lese-App, mit der Benutzer mehrere Artikel für das spätere Lesen öffnen können, um zunächst alle wichtigen Schlagzeilen zu durchsuchen.

Während jedes App-Layout einzigartig ist, empfehlen wir Ihnen, eine Schaltfläche für ein „Neues Fenster” an einem geeigneten Ort zu platzieren, wie etwa die obere rechte Ecke des Inhalts, der in einem neuen Fenster geöffnet werden kann. Sie sollten auch eine [Kontextmenü](../controls-and-patterns/menus.md) Option "in einem neuen Fenster öffnen" einschließen.

Informationen zum Erstellen separater Instanzen Ihrer APP (anstelle von separaten Fenstern für die gleiche Instanz) finden Sie unter [Erstellen einer UWP-App mit mehreren Instanzen](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Fenster Hosts

Es gibt verschiedene Möglichkeiten, wie UWP-Inhalte in einer APP gehostet werden können.

- [Corewindow](/uwp/api/windows.ui.core.corewindow)/-[applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)

     Eine App-Ansicht ist die 1:1-Zuordnung eines Threads und eines Fensters, das die App zur Anzeige von Inhalten verwendet. Die erste Ansicht, die beim Starten der App erstellt wird, wird als *Hauptansicht* bezeichnet. Jede corewindow/applicationview wird in einem eigenen Thread betrieben. Wenn Sie an verschiedenen UI-Threads arbeiten müssen, können Sie mehrere Fenster Anwendungen erschweren.

    Die Hauptansicht für Ihre APP wird immer in einer applicationview gehostet. Der Inhalt in einem sekundären Fenster kann in einer applicationview oder in einem appwindow gehostet werden.

    Informationen zum Verwenden von applicationview zum Anzeigen sekundärer Fenster in der App finden Sie unter [Verwenden von applicationview](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    Appwindow vereinfacht die Erstellung von UWP-apps mit mehreren Fenstern, da Sie auf demselben UI-Thread betrieben wird, aus dem Sie erstellt wurde.

    Die appwindow-Klasse und andere APIs im [windowmanagement](/uwp/api/windows.ui.windowmanagement) -Namespace sind ab Windows 10, Version 1903 (SDK 18362) verfügbar. Wenn Ihre APP auf frühere Versionen von Windows 10 ausgerichtet ist, müssen Sie die applicationview zum Erstellen sekundärer Fenster verwenden.

    Informationen zum Verwenden von appwindow zum Anzeigen sekundärer Fenster in der App finden Sie unter [Verwenden von appwindow](app-window.md).

    > [!NOTE]
    > Appwindow befindet sich derzeit in der Vorschau Phase. Dies bedeutet, dass Sie apps, die appwindow verwenden, an den Store übermitteln können, einige Plattform-und frameworkkomponenten jedoch bekanntermaßen nicht mit appwindow arbeiten (siehe [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [Desktopwindowxamlsource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML-Inseln)

     UWP-XAML-Inhalte in einer Win32-app (mit HWND), die auch als XAML-Inseln bezeichnet wird, werden in einer desktopwindowxamlsource gehostet.

    Weitere Informationen zu XAML-Inseln finden [Sie unter Verwenden der UWP-XAML-Hosting-API in einer Desktop Anwendung](/windows/apps/desktop/modernize/using-the-xaml-hosting-api) .

### <a name="make-code-portable-across-windowing-hosts"></a>Codieren von Code über windowinghosts

Wenn XAML-Inhalt in einem [corewindow](/uwp/api/windows.ui.core.corewindow)-Fenster angezeigt wird, sind immer ein zugeordnetes [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview) -und XAML- [Fenster](/uwp/api/windows.ui.xaml.window)vorhanden. Sie können APIs für diese Klassen verwenden, um Informationen wie die Fenster Begrenzungen zu erhalten. Zum Abrufen einer Instanz dieser Klassen verwenden Sie die statische [corewindow. getforcurrentthread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread) -Methode, die [applicationview. getforcurrentview](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview) -Methode oder die [Window. Current](/uwp/api/windows.ui.xaml.window.current) -Eigenschaft. Außerdem gibt es viele Klassen, die das `GetForCurrentView` Muster verwenden, um eine Instanz der Klasse abzurufen, z. b. [Displayinformation. getforcurrentview](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Diese APIs funktionieren, weil es nur eine einzige Struktur von XAML-Inhalt für eine corewindow/applicationview gibt, sodass der XAML-Code den Kontext kennt, in dem er gehostet wird, dass corewindow/applicationview.

Wenn XAML-Inhalt innerhalb von appwindow oder desktopwindowxamlsource ausgeführt wird, können mehrere Strukturen von XAML-Inhalt gleichzeitig im selben Thread ausgeführt werden. In diesem Fall erhalten diese APIs nicht die richtigen Informationen, da der Inhalt nicht mehr in der aktuellen corewindow/applicationview (und im XAML-Fenster) ausgeführt wird.

Um sicherzustellen, dass Ihr Code auf allen windowinghosts ordnungsgemäß funktioniert, sollten Sie APIs ersetzen, die auf [corewindow](/uwp/api/windows.ui.core.corewindow), [applicationview](/uwp/api/windows.ui.viewmanagement.applicationview)und [Window](/uwp/api/windows.ui.xaml.window) basieren, mit neuen APIs, die ihren Kontext aus der [xamlroot](/uwp/api/windows.ui.xaml.xamlroot) -Klasse erhalten.
Die xamlroot-Klasse stellt eine Struktur von XAML-Inhalten und Informationen über den Kontext dar, in dem Sie gehostet wird, egal, ob es sich um ein corewindow-, appwindow-oder desktopwindowxamlsource-Element handelt. Diese Abstraktions Ebene ermöglicht das Schreiben desselben Codes, unabhängig davon, in welchem windowinghost das XAML ausgeführt wird.

Diese Tabelle zeigt Code, der nicht ordnungsgemäß auf windowinghosts funktioniert, sowie den neuen portablen Code, durch den Sie ihn ersetzen können, sowie einige APIs, die Sie nicht ändern müssen.

| Wenn Sie... | Ersetzen durch... |
| - | - |
| Corewindow. getforcurrentthread (). [Begrenzungen](/uwp/api/windows.ui.core.corewindow.bounds) | _UIElement_. Xamlroot. [Größe](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Corewindow. getforcurrentthread (). [SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _UIElement_. Xamlroot. [Geändert](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow. [Sichtbar](/uwp/api/windows.ui.core.corewindow.visible) | _UIElement_. Xamlroot. [Ishostvisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow. [Visibilitychanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _UIElement_. Xamlroot. [Geändert](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| Corewindow. getforcurrentthread (). [GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Unverändert. Dies wird in appwindow und desktopwindowxamlsource unterstützt. |
| Corewindow. getforcurrentthread (). [GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Unverändert. Dies wird in appwindow und desktopwindowxamlsource unterstützt. |
| Ster. [Aktuell](/uwp/api/windows.ui.xaml.window.current) | Gibt das XAML-Hauptfenster Objekt zurück, das eng an das aktuelle corewindow-Objekt gebunden ist. Siehe Hinweis nach dieser Tabelle. |
| Window. Current. [Begrenzungen](/uwp/api/windows.ui.xaml.window.bounds) | _UIElement_. Xamlroot. [Größe](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window. Current. [Inhalt](/uwp/api/windows.ui.xaml.window.content) | UIElement root = _UIElement_. Xamlroot. [Inhalt](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window. Current. [Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Unverändert. Dies wird in appwindow und desktopwindowxamlsource unterstützt. |
| VisualTreeHelper. [Getopenpopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>In XAML-Inseln führt dies zu einem Fehler. In appwindow-apps werden dadurch geöffnete Popups im Hauptfenster zurückgegeben. | VisualTreeHelper. [Getopenpopupsforxamlroot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot) (_UIElement_. Xamlroot) |
| FocusManager. [Getfocu-Delta Element](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager. [Getfocu-Delta Element](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_) (_UIElement_. Xamlroot) |
| contentdialog. showasync () | contentdialog. [Xamlroot](/uwp/api/windows.ui.xaml.uielement.xamlroot) _UIElement_.  =  Xamlroot;<br/>contentdialog. showasync (); |
| menuflyout. showat (null, neuer Punkt (10, 10)); | menuflyout. [Xamlroot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) _UIElement_.  =  Xamlroot;<br/>menuflyout. showat (null, neuer Punkt (10, 10)); |

> [!NOTE]
> Bei XAML-Inhalt in einer desktopwindowxamlsource ist ein corewindow/-Fenster auf dem Thread vorhanden, aber es ist immer unsichtbar und hat eine Größe von 1 x 1. Der Zugriff auf die APP ist weiterhin möglich, es gibt jedoch keine sinnvollen Begrenzungen oder Sichtbarkeit.
>
>Bei XAML-Inhalten in appwindow wird immer genau ein corewindow im gleichen Thread vorhanden sein. Wenn Sie eine `GetForCurrentView` -oder `GetForCurrentThread` -API aufgerufen haben, gibt diese API ein Objekt zurück, das den Status des corewindow im Thread widerspiegelt, nicht jedoch die appwindows, die möglicherweise in diesem Thread ausgeführt werden.


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Bieten Sie einen klaren Einstiegspunkt zur sekundären Ansicht anhand des Symbols „Neues Fenster öffnen” an.
- Übermitteln Sie den Benutzern den Zweck der sekundäre Ansicht.
- Stellen Sie sicher, dass Ihre APP in einer einzigen Ansicht voll funktionsfähig ist und dass Benutzer eine sekundäre Ansicht nur aus Gründen der Benutzer Sicht öffnen.
- Verlassen Sie sich nicht auf die sekundäre Ansicht, um Benachrichtigungen oder andere vorübergehende visuelle Elemente anzubieten.

## <a name="related-topics"></a>Verwandte Themen

- [Verwenden von appwindow](app-window.md)
- [Verwenden von applicationview](application-view.md)
- [Applicationviewswitcher](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- ["Kreatenewview"](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
