---
ms.assetid: 9BA3F85A-970F-411C-ACB1-B65768B8548A
description: Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen.
title: Anzeigen der Kameravorschau
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f30611d649e0485a9cc89a162ae49768b05e00d7
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89174504"
---
# <a name="display-the-camera-preview"></a>Anzeigen der Kameravorschau


Dieser Artikel beschreibt, wie Sie in einer UWP (Universelle Windows-Plattform)-App innerhalb einer XAML-Seite schnell den Datenstrom der Kameravorschau anzeigen. Zum Erstellen einer App, die Fotos und Videos mit der Kamera erfasst, müssen Sie Aufgaben wie das Behandeln der Geräte- und Kameraausrichtung oder das Festlegen von Codierungsoptionen für die erfasste Datei durchführen. Für einige App-Szenarien möchten Sie vielleicht einfach nur den Vorschaudatenstrom von der Kamera anzeigen, ohne sich Gedanken über diese anderen Überlegungen machen zu müssen. Dieser Artikel zeigt, wie dies mit einem Minimum an Code möglich ist. Hinweis: Sie sollten den Vorschaudatenstrom immer ordnungsgemäß beenden, wenn Sie damit fertig sind; führen Sie dazu die folgenden Schritte aus.

Informationen zum Schreiben einer Kamera-App, die Fotos oder Videos aufnimmt, finden Sie unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md).

## <a name="add-capability-declarations-to-the-app-manifest"></a>Hinzufügen von Funktionsdeklarationen zum App-Manifest

Damit Ihre App auf die Kamera eines Geräts zugreifen kann, müssen Sie die Verwendung der *webcam*- und *microphone*-Gerätefunktionen durch Ihre App deklarieren. 

**Hinzufügen von Funktionen zum App-Manifest**

1.  Öffnen Sie in Microsoft Visual Studio im **Projektmappen-Explorer** den Designer für das Anwendungsmanifest, indem Sie auf das Element **package.appxmanifest** doppelklicken.
2.  Wählen Sie die Registerkarte **Funktionen** aus.
3.  Aktivieren Sie das Kontrollkästchen für die **Webcam** und das Kontrollkästchen für **Mikrofon**.

## <a name="add-a-captureelement-to-your-page"></a>Hinzufügen eines CaptureElement zu einer Seite

Mithilfe eines [**CaptureElement**](/uwp/api/Windows.UI.Xaml.Controls.CaptureElement) können Sie den Vorschaudatenstrom innerhalb der XAML-Seite anzeigen.

[!code-xml[CaptureElement](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml#SnippetCaptureElement)]



## <a name="use-mediacapture-to-start-the-preview-stream"></a>Verwenden von MediaCapture zum Starten des Vorschaudatenstroms

Das [**MediaCapture**](/uwp/api/Windows.Media.Capture.MediaCapture)-Objekt ist die Schnittstelle Ihrer App mit der Kamera des Geräts. Diese Klasse ist ein Mitglied des Windows.Media.Capture-Namespace. Im Beispiel in diesem Artikel werden neben den in der Standard-Projektvorlage enthaltenen APIs auch APIs aus den Namespaces [**Windows.ApplicationModel**](/uwp/api/Windows.ApplicationModel) und [System.Threading.Tasks](/dotnet/api/system.threading.tasks) verwendet.

Fügen Sie using-Direktiven hinzu, um die folgenden Namespaces in die CS-Datei Ihrer Seite einzubeziehen.

[!code-cs[SimpleCameraPreviewUsing](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSimpleCameraPreviewUsing)]

Deklarieren Sie eine Klassenmembervariable für das **MediaCapture**-Objekt. Fügen Sie einen booleschen Member hinzu, um nachzuverfolgen, ob die Vorschau der Kamera gerade aktiv ist. 

[!code-cs[DeclareMediaCapture](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareMediaCapture)]

Deklarieren Sie eine Variable vom Typ [**DisplayRequest**](/uwp/api/Windows.System.Display.DisplayRequest), um sicherzustellen, dass die Anzeige während der Ausführung der Vorschau nicht deaktiviert wird.

[!code-cs[DeclareDisplayRequest](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetDeclareDisplayRequest)]

Erstellen Sie eine Hilfsmethode, um die Kamera Vorschau zu starten, die in diesem Beispiel " **startpreviewasync** " genannt wird. Abhängig vom Szenario Ihrer APP können Sie dies vom **OnNavigatedTo** -Ereignishandler aus aufrufen, der aufgerufen wird, wenn die Seite geladen wird, oder warten und die Vorschau als Reaktion auf Benutzeroberflächen Ereignisse starten.

Erstellen Sie eine neue Instanz der **MediaCapture**-Klasse, und rufen Sie [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) auf, um das Aufnahmegerät zu initialisieren. Diese Methode schlägt u. U. fehl, beispielsweise auf Geräten ohne Kamera, daher sollte der Aufruf aus einem **try**-Block erfolgen. Beim Versuch, die Kamera zu initialisieren, wird eine **UnauthorizedAccessException** ausgelöst, wenn der Benutzer in den Datenschutzeinstellungen des Geräts den Kamerazugriff deaktiviert hat. Diese Ausnahme tritt auch während der Entwicklung auf, wenn Sie Ihrem App-Manifest nicht die richtigen Funktionen hinzugefügt haben.

**Wichtig** Bei einigen Gerätefamilien wird dem Benutzer eine Aufforderung zur Zustimmung des Benutzers angezeigt, bevor Ihrer App der Zugriff auf die Kamera des Geräts gewährt wird. Aus diesem Grund müssen Sie nur [**MediaCapture.InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) aus dem Hauptthread der Benutzeroberfläche aufrufen. Der Versuch, die Kamera von einem anderen Thread aus zu initialisieren, kann zum einem Initialisierungsfehler führen.

Verbinden Sie das **MediaCapture**-Objekt mit der **CaptureElement**-Klasse, indem Sie die [**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source)-Eigenschaft festlegen. Starten Sie die Vorschau durch Aufrufen von [**StartPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.startpreviewasync). Diese Methode löst eine **FileLoadException** aus, wenn eine andere App eine exklusive Kontrolle über das Erfassungsgerät hat. Im nächsten Abschnitt finden Sie Informationen zur Überwachung von Änderungen in exklusiver Kontrolle.

Rufen Sie [**RequestActive**](/uwp/api/windows.system.display.displayrequest.requestactive) auf, um sicherzustellen, dass das Gerät während der Ausführung der Vorschau nicht in den Standbymodus wechselt. Legen Sie abschließend die [**DisplayInformation.AutoRotationPreferences**](/uwp/api/windows.graphics.display.displayinformation.autorotationpreferences)-Eigenschaft auf [**Landscape**](/uwp/api/Windows.Graphics.Display.DisplayOrientations) fest, um zu verhindern, dass die sich Benutzeroberfläche und das **CaptureElement** drehen, wenn der Benutzer die Ausrichtung des Geräts ändert. Weitere Informationen zum Behandeln von Änderungen an der Geräteausrichtung finden Sie unter [**Handhaben der Geräte- und Bildschirmausrichtung mit „MediaCapture“**](handle-device-orientation-with-mediacapture.md).  

[!code-cs[StartPreviewAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetStartPreviewAsync)]

## <a name="handle-changes-in-exclusive-control"></a>Behandeln von Änderungen im exklusiven Steuerelement
Wie im vorherigen Abschnitt erwähnt, löst **startpreviewasync** eine **FileLoadException** aus, wenn eine andere App eine exklusive Kontrolle über das Erfassungsgerät hat. Ab Windows 10, Version 1703, können Sie einen Handler für das Ereignis [mediacapture. capturedeviceexclusivecontrolstatuschangiregistrieren](/uwp/api/Windows.Media.Capture.MediaCapture.CaptureDeviceExclusiveControlStatusChanged) , das immer dann ausgelöst wird, wenn der ausschließliche Steuerungs Status des Geräts geändert wird. Überprüfen Sie im Handler für dieses Ereignis die [mediacapturedeviceexclusivecontrolstatuschangedeventargs. Status](/uwp/api/windows.media.capture.mediacapturedeviceexclusivecontrolstatuschangedeventargs.Status) -Eigenschaft, um den aktuellen Status anzuzeigen. Wenn der neue Status **sharedreadonlyavailable**lautet, wissen Sie, dass Sie die Vorschau zurzeit nicht starten können, und Sie können die Benutzeroberfläche aktualisieren, um den Benutzer zu benachrichtigen. Wenn der neue Status " **exclusivecontrolavailable**" lautet, können Sie versuchen, die Kamera Vorschau erneut zu starten.

[!code-cs[ExclusiveControlStatusChanged](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetExclusiveControlStatusChanged)]

## <a name="shut-down-the-preview-stream"></a>Beenden des Vorschaudatenstroms

Wenn Sie den Vorschaudatenstrom nicht mehr benötigen, sollten Sie ihn stets beenden und die dazugehörigen Ressourcen ordnungsgemäß löschen, um sicherzustellen, dass die Kamera für andere Apps auf dem Gerät verfügbar ist. Folgende Schritte sind zum Beenden des Vorschaudatenstroms erforderlich:

-   Wenn die Vorschau der Kamera gerade aktiv ist, rufen Sie [**StopPreviewAsync**](/uwp/api/windows.media.capture.mediacapture.stoppreviewasync) auf, um den Vorschaudatenstrom zu beenden. Wenn Sie **StopPreviewAsync** aufrufen, während die Vorschau nicht ausgeführt, wird eine Ausnahme ausgelöst.
-   Legen Sie die [**Source**](/uwp/api/windows.ui.xaml.controls.captureelement.source)-Eigenschaft von **CaptureElement** auf NULL fest. Verwenden Sie [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync), um sicherzustellen, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Rufen Sie die [**Dispose**](/uwp/api/windows.media.capture.mediacapture.dispose)-Methode des **MediaCapture**-Objekts auf, um es freizugeben. Stellen Sie erneut mit [**CoreDispatcher.RunAsync**](/uwp/api/windows.ui.core.coredispatcher.runasync) sicher, dass dieser Aufruf im UI-Thread ausgeführt wird.
-   Legen Sie die **MediaCapture**-Membervariable auf NULL fest.
-   Rufen Sie [**RequestRelease**](/uwp/api/windows.system.display.displayrequest.requestrelease) auf, wenn der Bildschirm bei Inaktivität ausgeschaltet werden soll.

[!code-cs[CleanupCameraAsync](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetCleanupCameraAsync)]

Sie sollten den Vorschaudatenstrom durch Außerkraftsetzen der [**OnNavigatedFrom**](/uwp/api/windows.ui.xaml.controls.page.onnavigatedfrom)-Methode beenden, wenn der Benutzer Ihre Seite verlässt.

[!code-cs[OnNavigatedFrom](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetOnNavigatedFrom)]

Sie sollten den Vorschaudatenstrom auch ordnungsgemäß beenden, wenn die App angehalten wird. Registrieren Sie dazu im Konstruktor der Seite einen Handler für das [**Application.Suspending**](/uwp/api/windows.applicationmodel.core.coreapplication.suspending)-Ereignis.

[!code-cs[RegisterSuspending](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetRegisterSuspending)]

Stellen Sie im **Suspending**-Ereignishandler zunächst sicher, dass die Seite im [**Frame**](/uwp/api/Windows.UI.Xaml.Controls.Frame) der Anwendung angezeigt wird, indem Sie den Seitentyp mit der [**CurrentSourcePageType**](/uwp/api/windows.ui.xaml.controls.frame.currentsourcepagetype)-Eigenschaft vergleichen. Wird die Seite derzeit nicht angezeigt, sollten das **OnNavigatedFrom**-Ereignis bereits ausgelöst und der Vorschaudatenstrom geschlossen worden sein. Wird die Seite angezeigt, rufen Sie ein [**SuspendingDeferral**](/uwp/api/Windows.ApplicationModel.SuspendingDeferral)-Objekt aus den an den Handler übergebenen Ereignisargumenten ab, um sicherzustellen, dass das System Ihre App nicht anhält, bis der Vorschaudatenstrom beendet wurde. Rufen Sie nach dem Beenden des Datenstroms die [**Complete**](/uwp/api/windows.applicationmodel.suspendingdeferral.complete)-Methode der Verzögerung auf, damit das System die App weiterhin anhalten kann.

[!code-cs[SuspendingHandler](./code/SimpleCameraPreview_Win10/cs/MainPage.xaml.cs#SnippetSuspendingHandler)]


## <a name="related-topics"></a>Zugehörige Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Abrufen eines Vorschauframes](get-a-preview-frame.md)