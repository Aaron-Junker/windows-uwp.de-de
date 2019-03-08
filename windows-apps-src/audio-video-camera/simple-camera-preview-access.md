---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen.
title: Anzeigen der Kameravorschau
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 24b2885597599607ca405e858a9f713f5a6af4c7
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644875"
---
# <a name="display-the-camera-preview"></a>Anzeigen der Kameravorschau


Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen. Zum Erstellen einer App, die Fotos und Videos mit der Kamera erfasst, müssen Sie Aufgaben wie das Behandeln der Geräte- und Kameraausrichtung oder das Festlegen von Codierungsoptionen für die erfasste Datei durchführen. Für einige App-Szenarien möchten Sie vielleicht einfach nur den Vorschaudatenstrom von der Kamera anzeigen, ohne sich Gedanken über diese anderen Überlegungen machen zu müssen. Dieser Artikel zeigt, wie dies mit einem Minimum an Code möglich ist. Hinweis: Sie sollten den Vorschaudatenstrom immer ordnungsgemäß beenden, wenn Sie damit fertig sind; führen Sie dazu die folgenden Schritte aus.

Informationen zum Schreiben einer Kamera-App, die Fotos oder Videos aufnimmt, finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie die Verwendung der *webcam*- und *microphone*-Gerätefunktionen durch Ihre App deklarieren. 

**Hinzufügen von Funktionen zu app-manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie die Kontrollkästchen für **Webcam** und **Mikrofon**.

## <a name="add-a-captureelement-to-your-page"></a>Hinzufügen eines CaptureElement zu einer Seite

Mithilfe eines [**CaptureElement**](https://msdn.microsoft.com/library/windows/apps/br209278) können Sie den Vorschaudatenstrom innerhalb der XAML-Seite anzeigen.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Verwenden von MediaCapture zum Starten des Vorschaudatenstroms

Das [**MediaCapture**](https://msdn.microsoft.com/library/windows/apps/br241124)-Objekt ist die Schnittstelle Ihrer App mit der Kamera des Geräts. Diese Klasse ist ein Mitglied des Windows.Media.Capture-Namespace. Im Beispiel in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den Namespaces [**Windows.ApplicationModel**](https://msdn.microsoft.com/library/windows/apps/br224691) und [System.Threading.Tasks](https://msdn.microsoft.com/library/windows/apps/xaml/system.threading.tasks.aspx) verwendet.

Fügen Sie using-Direktiven hinzu, um die folgenden Namespaces in die CS-Datei Ihrer Seite einzubeziehen.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Deklarieren Sie eine Klassenmembervariable für das **MediaCapture**-Objekt. Fügen Sie einen booleschen Member hinzu, um nachzuverfolgen, ob die Vorschau der Kamera gerade aktiv ist. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Deklarieren Sie eine Variable vom Typ [**DisplayRequest**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest), um sicherzustellen, dass die Anzeige während der Ausführung der Vorschau nicht deaktiviert wird.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Erstellen Sie eine Hilfsmethode zum Starten der Kameravorschau, die in diesem Beispiel **StartPreviewAsync** genannt wird. Je nach App-Szenario können Sie dies vom **OnNavigatedTo**-Ereignishandler aufrufen, der aufgerufen wird, wenn die Seite geladen wird, oder warten und starten Sie die Vorschau als Reaktion auf UI-Ereignisse.

Erstellen Sie eine neue Instanz der **MediaCapture**-Klasse, und rufen Sie [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) auf, um das Aufnahmegerät zu initialisieren. Diese Methode schlägt u. U. fehl, beispielsweise auf Geräten ohne Kamera, daher sollte der Aufruf aus einem **try**-Block erfolgen. Beim Versuch, die Kamera zu initialisieren, wird eine **UnauthorizedAccessException** ausgelöst, wenn der Benutzer in den Datenschutzeinstellungen des Geräts den Kamerazugriff deaktiviert hat. Diese Ausnahme tritt auch während der Entwicklung auf, wenn Sie Ihrem App-Manifest nicht die richtigen Funktionen hinzugefügt haben.

**Wichtig** Bei einigen Gerätefamilien wird dem Benutzer eine Aufforderung zur Zustimmung des Benutzers angezeigt, bevor Ihrer App der Zugriff auf die Kamera des Geräts gewährt wird. Aus diesem Grund müssen Sie nur [**MediaCapture.InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) aus dem Hauptthread der Benutzeroberfläche aufrufen. Der Versuch, die Kamera von einem anderen Thread aus zu initialisieren, kann zum einem Initialisierungsfehler führen.

Verbinden Sie das **MediaCapture**-Objekt mit der **CaptureElement**-Klasse, indem Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft festlegen. Starten Sie die Vorschau durch Aufrufen von [**StartPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226613). Diese Methode löst eine **FileLoadException** aus, wenn eine andere App exklusive Kontrolle über das Aufnahmegerät hat. Der nächste Abschnitt erläutert, wie Zustandsänderungen bei exklusiver Kontrolle überwacht werden.

Rufen Sie [**RequestActive**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestActive) auf, um sicherzustellen, dass das Gerät während der Ausführung der Vorschau nicht in den Standbymodus wechselt. Legen Sie abschließend die [**DisplayInformation.AutoRotationPreferences**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayInformation.AutoRotationPreferences)-Eigenschaft auf [**Landscape**](https://msdn.microsoft.com/library/windows/apps/Windows.Graphics.Display.DisplayOrientations) fest, um zu verhindern, dass die sich Benutzeroberfläche und das **CaptureElement** drehen, wenn der Benutzer die Ausrichtung des Geräts ändert. Weitere Informationen zum Behandeln von Änderungen an der Geräteausrichtung finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>Behandeln von Änderungen bei exklusiver Kontrolle
Wie im vorherigen Abschnitt erwähnt, löst der Aufruf **StartPreviewAsync** eine **FileLoadException** aus, wenn eine andere App exklusive Kontrolle über das Aufnahmegerät hat. Ab Windows 10 Version 1703 können Sie einen Handler für das [MediaCapture.CaptureDeviceExclusiveControlStatusChanged](https://docs.microsoft.com/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged)-Ereignis registrieren, das bei jeder Statusänderung der exklusiven Kontrolle des Geräts ausgelöst wird. Um den aktuellen Status anzuzeigen, rufen Sie im Handler dieses Ereignisses die Eigenschaft [MediaCaptureDeviceExclusiveControlStatusChangedEventArgs.Status](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) ab. Wenn der neue Status **SharedReadOnlyAvailable** lautet, können Sie die Vorschau derzeit nicht starten und sollten Ihren Benutzer auf der UI informieren. Wenn der neue Status **ExclusiveControlAvailable** lautet, versuchen Sie die Kameravorschau erneut zu starten.

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>Beenden des Vorschaudatenstroms

Wenn Sie den Vorschaudatenstrom nicht mehr benötigen, sollten Sie ihn stets beenden und die dazugehörigen Ressourcen ordnungsgemäß löschen, um sicherzustellen, dass die Kamera für andere Apps auf dem Gerät verfügbar ist. Folgende Schritte sind zum Beenden des Vorschaudatenstroms erforderlich:

-   Wenn die Vorschau der Kamera gerade aktiv ist, rufen Sie [**StopPreviewAsync**](https://msdn.microsoft.com/library/windows/apps/br226622) auf, um den Vorschaudatenstrom zu beenden. Wenn Sie **StopPreviewAsync** aufrufen, während die Vorschau nicht ausgeführt, wird eine Ausnahme ausgelöst.
-   Legen Sie die [**Source**](https://msdn.microsoft.com/library/windows/apps/br209280)-Eigenschaft von **CaptureElement** auf NULL fest. Verwenden Sie [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx), um sicherzustellen, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Rufen Sie die [**Dispose**](https://msdn.microsoft.com/library/windows/apps/dn278858)-Methode des **MediaCapture**-Objekts auf, um es freizugeben. Stellen Sie erneut mit [**CoreDispatcher.RunAsync**](https://msdn.microsoft.com/library/windows/apps/windows.ui.core.coredispatcher.runasync.aspx) sicher, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Legen Sie die **MediaCapture**-Membervariable auf NULL fest.
-   Rufen Sie [**RequestRelease**](https://msdn.microsoft.com/library/windows/apps/Windows.System.Display.DisplayRequest.RequestRelease) auf, wenn der Bildschirm bei Inaktivität ausgeschaltet werden soll.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Sie sollten den Vorschaudatenstrom durch Außerkraftsetzen der [**OnNavigatedFrom**](https://msdn.microsoft.com/library/windows/apps/br227507)-Methode beenden, wenn der Benutzer Ihre Seite verlässt.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Sie sollten den Vorschaudatenstrom auch ordnungsgemäß beenden, wenn die App angehalten wird. Registrieren Sie dazu im Konstruktor der Seite einen Handler für das [**Application.Suspending**](https://msdn.microsoft.com/library/windows/apps/br205860)-Ereignis.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Stellen Sie im **Suspending**-Ereignishandler zunächst sicher, dass die Seite im [**Frame**](https://msdn.microsoft.com/library/windows/apps/br242682) der Anwendung angezeigt wird, indem Sie den Seitentyp mit der [**CurrentSourcePageType**](https://msdn.microsoft.com/library/windows/apps/hh702390)-Eigenschaft vergleichen. Wird die Seite derzeit nicht angezeigt, sollten das **OnNavigatedFrom**-Ereignis bereits ausgelöst und der Vorschaudatenstrom geschlossen worden sein. Wird die Seite angezeigt, rufen Sie ein [**SuspendingDeferral**](https://msdn.microsoft.com/library/windows/apps/br224684)-Objekt aus den an den Handler übergebenen Ereignisargumenten ab, um sicherzustellen, dass das System Ihre App nicht anhält, bis der Vorschaudatenstrom beendet wurde. Rufen Sie nach dem Beenden des Datenstroms die [**Complete**](https://msdn.microsoft.com/library/windows/apps/br224685)-Methode der Verzögerung auf, damit das System die App weiterhin anhalten kann.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Erhalten Sie einen Frame (Vorschau)](get-a-preview-frame.md)
