---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen.
title: Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: f42b58794b62753ff325cb4fc23202e5a48047d4
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89364023"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen



Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

 

## <a name="about-camera-profiles"></a>Informationen zu Kameraprofilen

Kameras auf verschiedenen Geräten unterstützen unterschiedliche Funktionen; dazu gehören beispielsweise die unterschiedlichen unterstützten Auflösungen, die Bildfrequenz für Videoaufnahmen, und ob HDR oder Aufnahmen mit variabler Bildfrequenz unterstützt werden. Dieser Satz von Funktionen wird vom UWP-Medienaufnahmeframework (Universelle Windows-Plattform) in einer [**MediaCaptureVideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription)-Klasse gespeichert. Eine Kameraprofil, dargestellt durch ein [**MediaCaptureVideoProfile**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfile)-Objekt, verfügt über drei Gruppen von Medienbeschreibungen: eine für die Fotoaufnahme, eine für die Videoaufnahme und eine weitere für die Videovorschau.

Vor dem Initialisieren des [MediaCapture](./index.md)-Objekts können Sie die Aufnahmegeräte auf dem aktuellen Gerät abfragen, um festzustellen, welche Profile unterstützt werden. Wenn Sie ein unterstütztes Profil auswählen, wissen Sie, dass das Aufnahmegerät alle Funktionen in den Medienbeschreibungen des Profils unterstützt. Dadurch entfällt die Notwendigkeit eines Trial-and-Error-Ansatzes, um zu ermitteln, welche Kombinationen von Funktionen auf einem bestimmten Gerät unterstützt werden.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetBasicInitExample":::

In den Codebeispielen in diesem Artikel wird diese minimale Initialisierung durch die Ermittlung der Kameraprofile ersetzt. Diese unterstützen unterschiedliche Funktionen, mit denen das Medienaufnahmegerät initialisiert wird.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Suchen eines Videogeräts, das Kameraprofile unterstützt

Bevor Sie unterstützte Kameraprofile suchen, sollten Sie ein Aufnahmegerät suchen, das die Verwendung von Kameraprofilen unterstützt. Die im folgenden Beispiel definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode verwendet die [**DeviceInformaion.FindAllAsync**](/uwp/api/windows.devices.enumeration.deviceinformation.findallasync)-Methode zum Abrufen einer Liste aller verfügbaren Videoaufnahmegeräte. Sie durchläuft alle Geräte in der Liste und ruft die statische Methode [**IsVideoProfileSupported**](/uwp/api/windows.media.capture.mediacapture.isvideoprofilesupported) für jedes Gerät auf, um festzustellen, ob es Videoprofile unterstützt. Sie durchläuft auch die [**EnclosureLocation.Panel**](/uwp/api/windows.devices.enumeration.enclosurelocation.panel)-Eigenschaft für jedes Gerät, sodass Sie angeben können, ob Sie eine Kamera auf der Vorderseite oder auf der Rückseite des Geräts bevorzugen.

Wenn in dem angegebenen Bereich ein Gerät gefunden wird, das Kameraprofile unterstützt, wird der [**Id**](/uwp/api/windows.devices.enumeration.deviceinformation.id)-Wert mit der ID-Zeichenfolge des Geräts zurückgegeben.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetVideoProfileSupportedDeviceIdAsync":::

Wenn die von der **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode zurückgegebene Geräte-ID NULL oder eine leere Zeichenfolge ist, gibt es in dem angegebenen Bereich kein Gerät, das Kameraprofile unterstützt. In diesem Fall sollten Sie das Medienaufnahmegerät ohne Profile initialisieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetDeviceWithProfileSupport":::

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Auswählen eines Profils basierend auf der unterstützten Auflösung und Bildfrequenz

Um ein Profil mit bestimmten Funktionen auszuwählen, zum Beispiel mit der Fähigkeit, eine bestimmte Auflösung oder Bildfrequenz zu erzielen, sollten Sie zuerst die oben definierte Hilfsmethode aufrufen, um die ID eines Aufnahmegeräts abzurufen, das die Verwendung von Kameraprofilen unterstützt.

Erstellen Sie ein neues [**MediaCaptureInitializationSettings**](/uwp/api/Windows.Media.Capture.MediaCaptureInitializationSettings)-Objekt, und übergeben Sie die ausgewählte Geräte-ID. Als Nächstes rufen Sie die statische Methode [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) auf, um eine Liste aller vom Gerät unterstützten Kameraprofile zu erhalten.

In diesem Beispiel wird eine „Linq“-Abfragemethode verwendet, die im verwendeten **System.Linq**-Namespace enthalten ist, um ein Profil mit einem [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription)-Objekt auszuwählen. Die Eigenschaften [**Width**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.width), [**Height**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.height) und [**FrameRate**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.framerate) entsprechen dabei den angeforderten Werten. Wenn eine Übereinstimmung gefunden wird, werden [**VideoProfile**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.videoprofile) und [**RecordMediaDescription**](/uwp/api/windows.media.capture.mediacaptureinitializationsettings.recordmediadescription) der **MediaCaptureInitializationSettings**-Klasse auf die Werte des anonymen Typs festgelegt, der von der „Linq“-Abfrage zurückgegeben wurde. Wenn keine Übereinstimmung gefunden wird, wird das Standardprofil verwendet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindWVGA30FPSProfile":::

Nachdem Sie das **MediaCaptureInitializationSettings**-Objekt mit dem gewünschten Kameraprofil aufgefüllt haben, rufen Sie einfach [**InitializeAsync**](/uwp/api/windows.media.capture.mediacapture.initializeasync) in Ihrem Medienaufnahmeobjekt auf, um es mit dem gewünschten Profil zu konfigurieren.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetInitCaptureWithProfile":::

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Verwenden von Medien Frame-Quell Gruppen zum erhalten von Profilen

Ab Windows 10, Version 1803, können Sie die Klasse " [**mediaframesourcegroup**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup) " verwenden, um vor dem Initialisieren des **mediacapture** -Objekts Kameraprofile mit bestimmten Funktionen zu erhalten. Mit Frame Quell Gruppen können Gerätehersteller Gruppen von Sensoren oder Aufzeichnungsfunktionen als einzelnes virtuelles Gerät darstellen. Dies ermöglicht Berechnungs Szenarios, wie z. b. die Verwendung von tiefen-und Farbkameras, kann aber auch verwendet werden, um Kameraprofile für einfache Aufzeichnungs Szenarios auszuwählen. Weitere Informationen zur Verwendung von **mediaframesourcegroup**finden Sie unter [Verarbeiten von Medien Frames mit mediaframereader](process-media-frames-with-mediaframereader.md).

Die folgende Beispiel Methode zeigt, wie Sie mithilfe von **mediaframesourcegroup** -Objekten nach einem Kamera Profil suchen können, das ein bekanntes Video Profil unterstützt, z. b. ein solches, das HDR oder eine Variable Fotosequenz unterstützt. Zum Abrufen einer Liste aller Medien Frame-Quell Gruppen, die auf dem aktuellen Gerät verfügbar sind, müssen Sie zunächst " [**mediaframesourcegroup. findallasync**](/uwp/api/windows.media.capture.frames.mediaframesourcegroup.findallasync) " aufrufen. Durchlaufen Sie jede Quell Gruppe, und wenden Sie [**mediacapture. findknownvideoprofiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) an, um eine Liste aller Video Profile für die aktuelle Quell Gruppe zu erhalten, die das angegebene Profil unterstützen, in diesem Fall HDR mit WCG Photo. Wenn ein Profil gefunden wird, das die Kriterien erfüllt, erstellen Sie ein neues **mediacaptureinitializationsettings** -Objekt, und legen Sie die **Videoprofile-Datei** auf das Profil auswählen und **videodeviceid** auf die **ID** -Eigenschaft der aktuellen Medien Frame-Quell Gruppe fest. Sie könnten z. b. den Wert " **knownvideoprofile. hdrwithwcgvideo** " an diese Methode übergeben, um Medien Erfassungs Einstellungen zu erhalten, die HDR-Video unterstützen. Übergeben Sie **knownvideoprofile. variablephotosequence** , um Einstellungen zu erhalten, die die Variable Fotosequenz unterstützen.

 :::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindKnownVideoProfile":::

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Verwenden bekannter Profile für die Suche nach einem Profil, das HDR-Video unterstützt (Legacy-Technik)

> [!NOTE] 
> Die in diesem Abschnitt beschriebenen APIs sind ab Windows 10, Version 1803, veraltet. Weitere Informationen finden Sie im vorherigen Abschnitt **Verwenden von Medien Frame-Quell Gruppen zum erhalten von Profilen**.

Das Auswählen eines Profils, das HDR unterstützt, beginnt wie die anderen Szenarien. Erstellen Sie **mediacaptureinitializationsettings** und eine Zeichenfolge, die die Erfassungsgeräte-ID enthalten soll. Fügen Sie eine boolesche Variable hinzu, die aufzeichnet, ob HDR-Video unterstützt wird.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetHdrProfileSetup":::

Verwenden Sie die oben definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode, um die Geräte-ID für ein Aufnahmegerät abzurufen, das Kameraprofile unterstützt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindDeviceHDR":::

Die statische Methode [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) gibt die Kameraprofile zurück, die von dem angegebenen Gerät unterstützt werden, das wiederum von dem angegebenen [**KnownVideoProfile**](/uwp/api/Windows.Media.Capture.KnownVideoProfile)-Wert kategorisiert wird. In diesem Szenario wird der **VideoRecording**-Wert angegeben, um nur Kameraprofile zurückzugeben, die Videoaufnahmen unterstützen.

Durchlaufen Sie die zurückgegebene Liste der Kameraprofile. Durchlaufen Sie für jedes Kameraprofil jede [**VideoProfileMediaDescription**](/uwp/api/Windows.Media.Capture.MediaCaptureVideoProfileMediaDescription)-Klasse im Profil, und überprüfen Sie, ob die [**IsHdrVideoSupported**](/uwp/api/windows.media.capture.mediacapturevideoprofilemediadescription.ishdrvideosupported)-Eigenschaft auf „true“ festgelegt ist. Sobald eine passende Medienbeschreibung gefunden wird, unterbrechen Sie die Schleife und weisen die Profil- und Beschreibungsobjekte dem **MediaCaptureInitializationSettings**-Objekt zu.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetFindHDRProfile":::

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Ermitteln, ob ein Gerät die gleichzeitige Aufnahme von Fotos und Videos unterstützt

Viele Geräte unterstützen die gleichzeitige Aufnahme von Fotos und Videos. Um festzustellen, ob ein Aufnahmegerät dies unterstützt, rufen Sie [**MediaCapture.FindAllVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findallvideoprofiles) auf, um alle vom Gerät unterstützten Kameraprofile abzurufen. Verwenden Sie eine Linkabfrage, um ein Profil zu suchen, das mindestens einen Eintrag für [**SupportedPhotoMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedphotomediadescription) und [**SupportedRecordMediaDescription**](/uwp/api/windows.media.capture.mediacapturevideoprofile.supportedrecordmediadescription) aufweist, was bedeutet, dass das Profil die gleichzeitige Aufnahme unterstützt.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/BasicMediaCaptureWin10/cs/MainPage.xaml.cs" id="SnippetGetPhotoAndVideoSupport":::

Sie können diese Abfrage einschränken und nur nach Profilen suchen, die zusätzlich zur gleichzeitigen Videoaufnahme bestimmte Auflösungen oder andere Funktionen unterstützen. Sie können auch die [**MediaCapture.FindKnownVideoProfiles**](/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles)-Methode verwenden und den [**BalancedVideoAndPhoto**](/uwp/api/Windows.Media.Capture.KnownVideoProfile)-Wert angeben, um Profile abzurufen, die die gleichzeitige Aufnahme unterstützen. Das Abfragen aller Profile liefert aber genauere Ergebnisse.

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 
