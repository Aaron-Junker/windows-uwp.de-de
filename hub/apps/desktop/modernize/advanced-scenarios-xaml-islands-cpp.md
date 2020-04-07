---
description: In diesem Artikel werden erweiterte XAML Islands-Hostingszenarien für C++-Win32-Apps erläutert.
title: Erweiterte Szenarien für XAML Islands in C++-Win32-Apps
ms.date: 03/23/2020
ms.topic: article
keywords: Windows 10, UWP, CPP, Win32, XAML Islands, umschlossene Steuerelemente, Standardsteuerelemente
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 50ee005fc0de52a3e0217a71fb3d391445c486db
ms.sourcegitcommit: c660def841abc742600fbcf6ed98e1f4f7beb8cc
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/24/2020
ms.locfileid: "80226234"
---
# <a name="advanced-scenarios-for-xaml-islands-in-c-win32-apps"></a>Erweiterte Szenarien für XAML Islands in C++-Win32-Apps

In den Artikeln [Hosten eines UWP-Standardsteuerelements](host-standard-control-with-xaml-islands-cpp.md) und [Hosten eines benutzerdefinierten UWP-Steuerelements](host-custom-control-with-xaml-islands-cpp.md) wurden Anweisungen und Beispiele zum Hosten von XAML Islands in einer C++-Win32-App bereitgestellt. Die Codebeispiele in diesen Artikeln decken jedoch keine fortgeschrittenen Szenarien ab, die Desktopanwendungen möglicherweise unterstützen müssen, um ein reibungsloses Benutzererlebnis zu ermöglichen. Der vorliegende Artikel bietet Anleitungen für einige dieser Szenarien sowie Verweise auf zugehörige Codebeispiele.

## <a name="keyboard-input"></a>Tastatureingabe

Um Tastatureingaben für XAML Islands ordnungsgemäß verarbeiten zu können, muss deine Anwendung alle Windows-Meldungen an das UWP-XAML-Framework übergeben, sodass bestimmte Nachrichten richtig verarbeitet werden können. Wandle hierzu an einigen Stellen in deiner Anwendung mit Zugriff auf die Nachrichtenschleife das **DesktopWindowXamlSource**-Objekt in eine **IDesktopWindowXamlSourceNative2**-COM-Schnittstelle um. Rufe anschließend die **PreTranslateMessage**-Methode dieser Schnittstelle auf, und übergib die aktuelle Nachrichtenschleife.

  * **C++ Win32:** : Die App kann in der zugehörigen Hauptnachrichtenschleife **PreTranslateMessage** direkt aufrufen. Ein Beispiel findest du in der Datei [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp#L16).

  * **WPF:** Die App kann **PreTranslateMessage** aus dem Ereignishandler für das Ereignis [ComponentDispatcher.ThreadFilterMessage](https://docs.microsoft.com/dotnet/api/system.windows.interop.componentdispatcher.threadfiltermessage) aufrufen. Ein Beispiel findest du in der Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs#L177) im Windows-Community-Toolkit.

  * **Windows Forms:** Die App kann **PreTranslateMessage** aus einer Überschreibung für die [Control.PreprocessMessage](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.preprocessmessage)-Methode aufrufen. Ein Beispiel findest du in der Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs#L100) im Windows-Community-Toolkit.

## <a name="keyboard-focus-navigation"></a>Navigation mit Tastaturfokus

Wenn der Benutzer mithilfe der Tastatur durch die Benutzeroberflächenelemente in deiner Anwendung navigiert (beispielsweise durch Drücken der **TAB**-Taste oder mithilfe der Richtungs-/Pfeiltasten), musst du den Fokus programmgesteuert in das und aus dem **DesktopWindowXamlSource**-Objekt verlagern. Wenn der Benutzer bei der Tastaturnavigation **DesktopWindowXamlSource** erreicht, verlagere den Fokus in das erste [Windows.UI.Xaml.UIElement](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement)-Element in der Navigationsreihenfolge für deine Benutzeroberfläche. Fahre fort, den Fokus auf die folgenden **Windows.UI.Xaml.UIElement** Objekte zu verlagern, während der Benutzer die Elemente durchläuft, und bewege dann den Fokus zurück aus **DesktopWindowXamlSource** und in das übergeordnete Benutzeroberflächenelement.  

Die UWP-XAML-Hosting-API stellt verschiedene Typen und Member bereit, um diese Aufgaben auszuführen.

* Wenn die Tastaturnavigation **DesktopWindowXamlSource** erreicht, wird das Ereignis [GotFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.gotfocus) ausgelöst. Verarbeite dieses Ereignis, und verlagere den Fokus programmgesteuert auf das erste gehostete **Windows.UI.Xaml.UIElement**, indem du die [NavigateFocus](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.navigatefocus)-Methode verwendest.

* Wenn der Benutzer das letzte Element in **DesktopWindowXamlSource** erreicht, das den Fokus erhalten kann, und die **TAB**-Taste oder eine Pfeiltaste drückt, wird das [TakeFocusRequested](https://docs.microsoft.com/uwp/api/windows.ui.xaml.hosting.desktopwindowxamlsource.takefocusrequested)-Ereignis ausgelöst. Verarbeite dieses Ereignis, und verschiebe den Fokus programmgesteuert auf das nächste Element in der Hostanwendung, das den Fokus erhalten kann. Beispielsweise kannst du in einer WPF-Anwendung, bei der **DesktopWindowXamlSource** in einem [System.Windows.Interop.HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) gehostet wird, den Fokus mithilfe der Methode [MoveFocus](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.movefocus) auf das nächste Element in der Hostanwendung verlagern, das den Fokus erhalten kann.

Beispiele zur Veranschaulichung dieses Vorgangs im Kontext einer funktionierenden Beispielanwendung findest du in den folgenden Codedateien:

  * **C++/Win32**: Siehe Datei [XamlBridge.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/XamlBridge.cpp).

  * **WPF:** Siehe Datei [WindowsXamlHostBase.Focus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Focus.cs) im Windows-Community-Toolkit.  

  * **Windows Forms:** Siehe Datei [WindowsXamlHostBase.KeyboardFocus.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.KeyboardFocus.cs) im Windows-Community-Toolkit.

## <a name="handle-layout-changes"></a>Verarbeiten von Layoutänderungen

Wenn der Benutzer die Größe des übergeordneten Benutzeroberflächenelements ändert, musst du die erforderlichen Layoutänderungen verarbeiten, damit die UWP-Steuerelemente wie erwartet angezeigt werden. Nachfolgend werden einige wichtige Szenarien aufgeführt, die zu berücksichtigen sind.

* In einer C++-Win32-Anwendung kann deine Anwendung beim Verarbeiten der WM_SIZE-Nachricht das gehostete XAML Islands-Element mit der Funktion [SetWindowPos](https://docs.microsoft.com/windows/desktop/api/winuser/nf-winuser-setwindowpos) neu positionieren. Ein Beispiel hierzu findest du in der Codedatei [SampleApp.cpp](https://github.com/microsoft/Xaml-Islands-Samples/blob/master/Samples/Win32/SampleCppApp/SampleApp.cpp#L170).

* Wenn das übergeordnete Benutzeroberflächenelement die Größe des rechteckigen Bereichs annehmen muss, die für das in **DesktopWindowXamlSource** gehostete **Windows.UI.Xaml.UIElement** benötigt wird ist, rufe die [Measure](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.measure)-Methode von **Windows.UI.Xaml.UIElement** auf. Beispiel:

    * In einer WPF-Anwendung kannst du hierzu die [MeasureOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.measureoverride)-Methode von [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) verwenden, der **DesktopWindowXamlSource** hostet. Ein Beispiel findest du in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows-Community-Toolkit.

    * In einer Windows Forms-Anwendung kannst du hierzu die [GetPreferredSize](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.getpreferredsize)-Methode des [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)-Elements verwenden, das **DesktopWindowXamlSource** hostet. Ein Beispiel findest du in der Datei [WindowsXamlHostBase.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows-Community-Toolkit.

* Wenn sich die Größe des übergeordneten Benutzeroberflächenelements ändert, rufe die [Arrange](https://docs.microsoft.com/uwp/api/windows.ui.xaml.uielement.arrange)-Methode für das **Windows.UI.Xaml.UIElement** auf, das du in **DesktopWindowXamlSource** hostest. Beispiel:

    * In einer WPF-Anwendung kannst du hierzu die [ArrangeOverride](https://docs.microsoft.com/dotnet/api/system.windows.frameworkelement.arrangeoverride)-Methode von [HwndHost](https://docs.microsoft.com/dotnet/api/system.windows.interop.hwndhost) verwenden, der **DesktopWindowXamlSource** hostet. Ein Beispiel findest du in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Wpf.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows-Community-Toolkit.

    * In einer Windows Forms-Anwendung kannst du hierzu möglicherweise den Handler für das [SizeChanged](https://docs.microsoft.com/dotnet/api/system.windows.forms.control.sizechanged)-Ereignis des [Control](https://docs.microsoft.com/dotnet/api/system.windows.forms.control)-Elements verwenden, das **DesktopWindowXamlSource** hostet. Ein Beispiel findest du in der Datei [WindowsXamlHost.Layout.cs](https://github.com/windows-toolkit/Microsoft.Toolkit.Win32/blob/master/Microsoft.Toolkit.Forms.UI.XamlHost/WindowsXamlHostBase.Layout.cs) im Windows-Community-Toolkit.

## <a name="handle-dpi-changes"></a>Verarbeiten von DPI-Änderungen

Das UWP-XAML-Framework verarbeitet DPI-Änderungen für gehostete UWP-Steuerelemente automatisch (beispielsweise, wenn der Benutzer das Fenster per von einem Monitor zu einem anderen Monitor mit unterschiedlichem DPI-Wert zieht). Für optimale Ergebnisse wird empfohlen, deine Windows Forms-, WPF- oder C++ Win32-Anwendung so zu konfigurieren, dass sie den DPI-Wert des jeweiligen Monitors berücksichtigt.

Um deine Anwendung entsprechend zu konfigurieren, fügst du deinem Projekt ein [paralleles Assemblymanifest](https://docs.microsoft.com/windows/desktop/SbsCs/application-manifests) hinzu und legst das Element **\<dpiAwareness\>** -Element auf **PerMonitorV2** fest. Weitere Informationen zu diesem Wert findest du in der Beschreibung für [DPI_AWARENESS_CONTEXT_PER_MONITOR_AWARE_V2](https://docs.microsoft.com/windows/desktop/hidpi/dpi-awareness-context).

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly xmlns="urn:schemas-microsoft-com:asm.v1" manifestVersion="1.0">
    <application xmlns="urn:schemas-microsoft-com:asm.v3">
        <windowsSettings>
            <dpiAwareness xmlns="http://schemas.microsoft.com/SMI/2016/WindowsSettings">PerMonitorV2</dpiAwareness>
        </windowsSettings>
    </application>
</assembly>
```

## <a name="related-topics"></a>Verwandte Themen

* [Hosten von UWP-XAML-Steuerelementen in Desktop-Apps (XAML Islands)](xaml-islands.md)
* [Verwenden der UWP-XAML-Hosting-API in einer C++-Win32-App](using-the-xaml-hosting-api.md)
* [Hosten eines UWP-Standardsteuerelements in einer C++-Win32-App](host-standard-control-with-xaml-islands-cpp.md)
* [Hosten eines benutzerdefinierten UWP-Steuerelements in einer C++-Win32-App](host-custom-control-with-xaml-islands-cpp.md)
* [XAML Islands-Codebeispiele](https://github.com/microsoft/Xaml-Islands-Samples)
* [C++-Win32-XAML Islands-Beispiel](https://github.com/microsoft/Xaml-Islands-Samples/tree/master/Samples/Win32/SampleCppApp)
