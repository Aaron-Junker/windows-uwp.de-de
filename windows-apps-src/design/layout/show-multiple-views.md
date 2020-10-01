---
description: Sie können Ihren Benutzern zu mehr Produktivität verhelfen, indem Sie ihnen ermöglichen, unabhängige Teile der App in separaten Fenstern anzuzeigen.
title: Anzeigen mehrerer Ansichten für eine App
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 2b25ceaac900868479c9145b9dad5977ca213f03
ms.sourcegitcommit: eda7bbe9caa9d61126e11f0f1a98b12183df794d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 09/24/2020
ms.locfileid: "91218483"
---
# <a name="show-multiple-views-for-an-app"></a>Anzeigen mehrerer Ansichten für eine App

![Drahtmodell, das eine App mit mehreren Fenstern zeigt](images/multi-view.gif)

Du kannst deinen Benutzern zu mehr Produktivität verhelfen, indem du ihnen ermöglichst, unabhängige Teile der App in separaten Fenstern anzuzeigen. Wenn du für eine App mehrere Fenster erstellst, verhält sich jedes Fenster anders. Die Benutzer können App-Fenster unabhängig voneinander verschieben, deren Größe ändern, Fenster anzeigen und ausblenden und zwischen App-Fenstern wechseln, als würde es sich um separate Apps handeln.

> **Wichtige APIs:** [Namespace Windows.UI.ViewManagement](/uwp/api/windows.ui.viewmanagement), [Namespace Windows.UI.WindowManagement](/uwp/api/windows.ui.windowmanagement)

## <a name="when-should-an-app-use-multiple-views"></a>Wann sollte eine App mehrere Ansichten verwenden?

Es gibt eine Reihe von Szenarien, die von mehreren Ansichten profitieren können. Hier finden Sie einige Beispiele:

- Eine E-Mail-App, die Benutzern beim Verfassen einer neuen E-Mail eine Liste der empfangenen Nachrichten anzeigt
- Eine Adressbuch-App, mit der Benutzer Kontaktinformationen für mehrere Personen nebeneinander vergleichen können
- Eine Musikplayer-App, mit der Benutzer beim Durchsuchen einer Liste anderer verfügbarer Musiktitel sehen können, was wiedergegeben wird
- Eine Notizen-App, mit der Benutzer Informationen von einer Seite in eine andere kopieren können
- Eine Lese-App, mit der Benutzer mehrere Artikel zur späteren Lektüre öffnen können, nachdem sie die Gelegenheit hatten, alle wichtigen Schlagzeilen durchzulesen

Während jedes App-Layout einzigartig ist, empfehlen wir dir, eine Schaltfläche für ein „Neues Fenster” an geeigneter Stelle zu platzieren, wie etwa in der rechten oberen Ecke des Inhalts, der in einem neuen Fenster geöffnet werden kann. Erwäge außerdem, die Kontextmenüoption [In einem neuen Fenster öffnen](../controls-and-patterns/menus.md) einzufügen.

Weitere Informationen zum Erstellen separaten Instanzen deiner App (anstelle separater Fenster für dieselbe Instanz) findest du unter [Erstellen einer Windows-App mit mehreren Instanzen](../../launch-resume/multi-instance-uwp.md).

## <a name="windowing-hosts"></a>Hosts für Windowing

Es gibt verschiedene Möglichkeiten, wie Windows-Inhalte in einer App gehostet werden können.

- [CoreWindow](/uwp/api/windows.ui.core.corewindow)/[ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview)

     Eine App-Ansicht ist die 1:1-Zuordnung eines Threads und eines Fensters, das die App zur Anzeige von Inhalten verwendet. Die erste Ansicht, die beim Starten der App erstellt wird, wird als *Hauptansicht* bezeichnet. Jede CoreWindow/ApplicationView-Instanz agiert in einem eigenen Thread. Wenn du an verschiedenen Benutzeroberflächenthreads arbeiten musst, kann dies Anwendungen mit mehreren Fenstern komplizierter machen.

    Die Hauptansicht deiner App wird stets in ApplicationView gehostet. Inhalt in einem sekundären Fenster kann in ApplicationView oder AppWindow gehostet werden.

    Informationen zum Verwenden von ApplicationView zum Anzeigen sekundärer Fenster in deiner App findest du unter [Verwenden von ApplicationView](application-view.md).
- [AppWindow](/uwp/api/windows.ui.windowmanagement.appwindow)

    AppWindow vereinfacht das Erstellen von Windows-Apps mit mehreren Fenstern, da es im selben Benutzeroberflächenthread arbeitet, in dem es erstellt wurde.

    Die AppWindow-Klasse und andere APIs im Namespace [WindowManagement](/uwp/api/windows.ui.windowmanagement) sind ab Windows 10 Version 1903 (SDK 18362) verfügbar. Wenn deine App auf frühere Versionen von Windows 10 ausgerichtet ist, musst du sekundäre Fenster mithilfe von ApplicationView erstellen.

    Informationen zum Verwenden von AppWindow zum Anzeigen sekundärer Fenster in deiner App findest du unter [Verwenden von AppWindow](app-window.md).

    > [!NOTE]
    > AppWindow ist derzeit in der Vorschauphase. Du kannst daher Apps, die AppWindow verwenden, an den Store übermitteln. Es ist jedoch von einigen Plattform- und Frameworkkomponenten bekannt, dass sie nicht mit AppWindow funktionieren (siehe [Einschränkungen](/uwp/api/windows.ui.windowmanagement.appwindow#limitations)).
- [DesktopWindowXamlSource](/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource) (XAML Islands)

     UWP-XAML-Inhalte in einer Win32-Anwendung ( unter Verwendung von HWND), auch als XAML Islands bekannt, werden in DesktopWindowXamlSource gehostet.

    Weitere Informationen zu XAML Islands findest du unter [Verwenden der UWP-XAML-Hosting-API in einer Desktopanwendung](/windows/apps/desktop/modernize/using-the-xaml-hosting-api).

### <a name="make-code-portable-across-windowing-hosts"></a>Code als zwischen Hosts für Windowing portierbar gestalten

Wenn XAML-Inhalt in [CoreWindow](/uwp/api/windows.ui.core.corewindow) angezeigt wird, gibt es immer eine zugehörige [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) und ein XAML [Window](/uwp/api/windows.ui.xaml.window). Du kannst APIs für diese Klassen verwenden, um Informationen wie beispielsweise die Fensterbegrenzungen abzurufen. Verwende zum Abrufen einer Instanz dieser Klassen die statische [CoreWindow.GetForCurrentThread](/uwp/api/windows.ui.core.corewindow.getforcurrentthread)-Methode, die [ApplicationView.GetForCurrentView](/uwp/api/windows.ui.viewmanagement.applicationview.getforcurrentview)-Methode oder die [Window.Current](/uwp/api/windows.ui.xaml.window.current)-Eigenschaft. Darüber hinaus gibt es viele Klassen, die das `GetForCurrentView`-Muster verwenden, um eine Instanz der Klasse abzurufen, wie z. B. [DisplayInformation.GetForCurrentView](/uwp/api/windows.graphics.display.displayinformation.getforcurrentview).

Diese APIs funktionieren, weil es nur eine einzige Struktur von XAML-Inhalten für eine CoreWindow/ApplicationView-Instanz gibt, sodass XAML weiß, dass der Kontext, in dem es gehostet wird, diese CoreWindow/ApplicationView-Instanz ist.

Wenn XAML-Inhalt innerhalb von AppWindow oder DesktopWindowXamlSource ausgeführt wird, kann es mehrere Strukturen von XAML-Inhalt geben, die gleichzeitig im selben Thread ausgeführt werden. In diesem Fall liefern diese APIs nicht die richtigen Informationen, da der Inhalt nicht mehr innerhalb der aktuellen CoreWindow/ApplicationView-Instanz (und des aktuellen XAML-Fensters) ausgeführt wird.

Um sicherzustellen, dass dein Code bei allen Hosts für Windowing ordnungsgemäß funktioniert, solltest du APIs, die auf [CoreWindow](/uwp/api/windows.ui.core.corewindow), [ApplicationView](/uwp/api/windows.ui.viewmanagement.applicationview) und [Window](/uwp/api/windows.ui.xaml.window) aufsetzen, durch neue APIs ersetzen, die ihren Kontext aus der [XamlRoot](/uwp/api/windows.ui.xaml.xamlroot)-Klasse beziehen.
Die XamlRoot-Klasse stellt eine Struktur von XAML-Inhalten und Informationen zum Kontext dar, in dem sie gehostet wird, und zwar unabhängig davon, ob es sich um CoreWindow, AppWindow oder DesktopWindowXamlSource handelt. Mithilfe dieser Abstraktionsschicht kannst du den gleichen Code unabhängig davon schreiben, in welchem Host für Windowing der XAML-Code ausgeführt wird.

Diese Tabelle zeigt Code, der bei Hosts für Windowing nicht einwandfrei funktioniert, und den neuen portierbaren Code, durch den du ihn ersetzen kannst, sowie einige APIs, die du nicht ändern musst.

| Vorher | Nachher |
| - | - |
| CoreWindow.GetForCurrentThread().[Bounds](/uwp/api/windows.ui.core.corewindow.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| CoreWindow.GetForCurrentThread().[SizeChanged](/uwp/api/windows.ui.core.corewindow.sizechanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.[Visible](/uwp/api/windows.ui.core.corewindow.visible) | _uiElement_.XamlRoot.[IsHostVisible](/uwp/api/windows.ui.xaml.xamlroot.ishostvisible) |
| CoreWindow.[VisibilityChanged](/uwp/api/windows.ui.core.corewindow.visibilitychanged) | _uiElement_.XamlRoot.[Changed](/uwp/api/windows.ui.xaml.xamlroot.changed) |
| CoreWindow.GetForCurrentThread().[GetKeyState](/uwp/api/windows.ui.core.corewindow.getkeystate) | Unverändert. Wird in AppWindow and DesktopWindowXamlSource unterstützt. |
| CoreWindow.GetForCurrentThread().[GetAsyncKeyState](/uwp/api/windows.ui.core.corewindow.getasynckeystate) | Unverändert. Wird in AppWindow and DesktopWindowXamlSource unterstützt. |
| Window.[Current](/uwp/api/windows.ui.xaml.window.current) | Gibt das zentrale Window-Objekt von XAML zurück, das eng an das aktuelle CoreWindow gebunden ist. Siehe den Hinweis unter dieser Tabelle. |
| Window.Current.[Bounds](/uwp/api/windows.ui.xaml.window.bounds) | _uiElement_.XamlRoot.[Size](/uwp/api/windows.ui.xaml.xamlroot.size) |
| Window.Current.[Content](/uwp/api/windows.ui.xaml.window.content) | UIElement root =  _uiElement_.XamlRoot.[Content](/uwp/api/windows.ui.xaml.xamlroot.content) |
| Window.Current.[Compositor](/uwp/api/windows.ui.xaml.window.compositor) | Unverändert. Wird in AppWindow and DesktopWindowXamlSource unterstützt. |
| VisualTreeHelper.[FindElementsInHostCoordinates](/uwp/api/windows.ui.xaml.media.visualtreehelper.findelementsinhostcoordinates)<br>Der UIElement-Parameter ist zwar optional, die Methode löst aber eine Ausnahme aus, wenn ein auf einem Island gehostetes UIElement nicht angegeben wird. | Geben Sie die _uiElement_.XamlRoot als UIElement an, anstatt es leer zu lassen. |
| VisualTreeHelper.[GetOpenPopups](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopups)<br/>In XAML Islands-Apps führt dies zu einem Fehler. In AppWindow-Apps führt dies zu geöffneten Popups im Hauptfenster. | VisualTreeHelper.[GetOpenPopupsForXamlRoot](/uwp/api/windows.ui.xaml.media.visualtreehelper.getopenpopupsforxamlroot)(_uiElement_.XamlRoot) |
| FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement) | FocusManager.[GetFocusedElement](/uwp/api/windows.ui.xaml.input.focusmanager.getfocusedelement#Windows_UI_Xaml_Input_FocusManager_GetFocusedElement_Windows_UI_Xaml_XamlRoot_)(_uiElement_.XamlRoot) |
| contentDialog.ShowAsync() | contentDialog.[XamlRoot](/uwp/api/windows.ui.xaml.uielement.xamlroot) = _uiElement_.XamlRoot;<br/>contentDialog.ShowAsync(); |
| menuFlyout.ShowAt(null, new Point(10, 10)); | menuFlyout.[XamlRoot](/uwp/api/windows.ui.xaml.controls.primitives.flyoutbase.xamlroot) = _uiElement_.XamlRoot;<br/>menuFlyout.ShowAt(null, new Point(10, 10)); |

> [!NOTE]
> Für XAML-Inhalte in DesktopWindowXamlSource gibt es zwar ein CoreWindow/Window im Thread, aber es ist stets unsichtbar und hat eine Größe von 1x1. Es ist zwar immer noch für die App zugänglich, gibt aber keine sinnvollen Begrenzungen oder Sichtbarkeit zurück.
>
>Für XAML-Inhalt in AppWindow gibt es immer genau ein CoreWindow im selben Thread. Wenn du eine `GetForCurrentView`- oder `GetForCurrentThread`-API aufrufst, gibt diese API ein Objekt zurück, das den Zustand von CoreWindow im Thread widerspiegelt, und nicht eines der AppWindows, die möglicherweise in diesem Thread ausgeführt werden.


## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen

- Biete einen klaren Einstiegspunkt zur sekundären Ansicht mithilfe der Glyphe „Neues Fenster öffnen” an.
- Vermittle Benutzern den Zweck der sekundäre Ansicht.
- Stelle sicher, dass deine App in einer Ansicht voll funktionsfähig ist und dass Benutzer nur aus praktischen Gründen eine sekundäre Ansicht öffnen.
- Nutze die sekundäre Ansicht nicht, um Benachrichtigungen oder andere vorübergehende visuelle Elemente bereitzustellen.

## <a name="related-topics"></a>Zugehörige Themen

- [Verwenden von AppWindow](app-window.md)
- [Verwenden von ApplicationView](application-view.md)
- [ApplicationViewSwitcher](/uwp/api/Windows.UI.ViewManagement.ApplicationViewSwitcher)
- [CreateNewView](/uwp/api/windows.applicationmodel.core.coreapplication.createnewview)
