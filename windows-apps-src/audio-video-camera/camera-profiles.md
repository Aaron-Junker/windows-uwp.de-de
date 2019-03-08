---
ms.assetid: 42A06423-670F-4CCC-88B7-3DCEEDDEBA57
description: Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen.
title: Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: fe876b362f09eaca68d84516d413d3f458edf0a6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603405"
---
# <a name="discover-and-select-camera-capabilities-with-camera-profiles"></a>Entdecken und Auswählen von Kamerafunktionen mit Kameraprofilen



Dieser Artikel beschreibt, wie Sie Kameraprofile verwenden, um die Funktionen verschiedener Videoaufzeichnungsgeräte zu ermitteln und zu verwalten. Dazu gehören Aufgaben, z. B. das Auswählen von Profilen, die bestimmte Auflösungen oder Bildfrequenzen unterstützen, von Profilen, die gleichzeitigen Zugriff auf mehrere Kameras unterstützen, sowie von Profilen, die HDR unterstützen.

> [!NOTE] 
> Dieser Artikel baut auf Konzepten und Code auf, die unter [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md) erläutert werden. Dort werden die Schritte für die Implementierung einer grundlegenden Foto- und Videoaufnahme beschrieben. Es wird empfohlen, dass Sie sich mit dem grundlegenden Muster für die Medienerfassung in diesem Artikel vertraut machen, bevor Sie in fortgeschrittene Aufnahmeszenarien einsteigen. Der Code in diesem Artikel setzt voraus, dass Ihre App bereits über eine korrekt initialisierte MediaCapture-Instanz verfügt.

 

## <a name="about-camera-profiles"></a>Informationen zu Kameraprofilen

Kameras auf verschiedenen Geräten unterstützen unterschiedliche Funktionen; dazu gehören beispielsweise die unterschiedlichen unterstützten Auflösungen, die Bildfrequenz für Videoaufnahmen, und ob HDR oder Aufnahmen mit variabler Bildfrequenz unterstützt werden. Dieser Satz von Funktionen wird vom UWP-Medienaufnahmeframework (Universelle Windows-Plattform) in einer [**MediaCaptureVideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)-Klasse gespeichert. Eine Kameraprofil, dargestellt durch ein [**MediaCaptureVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926694)-Objekt, verfügt über drei Gruppen von Medienbeschreibungen: eine für die Fotoaufnahme, eine für die Videoaufnahme und eine weitere für die Videovorschau.

Vor dem Initialisieren des [MediaCapture](capture-photos-and-video-with-mediacapture.md)-Objekts können Sie die Aufnahmegeräte auf dem aktuellen Gerät abfragen, um festzustellen, welche Profile unterstützt werden. Wenn Sie ein unterstütztes Profil auswählen, wissen Sie, dass das Aufnahmegerät alle Funktionen in den Medienbeschreibungen des Profils unterstützt. Dadurch entfällt die Notwendigkeit eines Trial-and-Error-Ansatzes, um zu ermitteln, welche Kombinationen von Funktionen auf einem bestimmten Gerät unterstützt werden.

[!code-cs[BasicInitExample](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetBasicInitExample)]

In den Codebeispielen in diesem Artikel wird diese minimale Initialisierung durch die Ermittlung der Kameraprofile ersetzt. Diese unterstützen unterschiedliche Funktionen, mit denen das Medienaufnahmegerät initialisiert wird.

## <a name="find-a-video-device-that-supports-camera-profiles"></a>Suchen eines Videogeräts, das Kameraprofile unterstützt

Bevor Sie unterstützte Kameraprofile suchen, sollten Sie ein Aufnahmegerät suchen, das die Verwendung von Kameraprofilen unterstützt. Die im folgenden Beispiel definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode verwendet die [**DeviceInformaion.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/br225432)-Methode zum Abrufen einer Liste aller verfügbaren Videoaufnahmegeräte. Sie durchläuft alle Geräte in der Liste und ruft die statische Methode [**IsVideoProfileSupported**](https://msdn.microsoft.com/library/windows/apps/dn926714) für jedes Gerät auf, um festzustellen, ob es Videoprofile unterstützt. Sie durchläuft auch die [**EnclosureLocation.Panel**](https://msdn.microsoft.com/library/windows/apps/br229906)-Eigenschaft für jedes Gerät, sodass Sie angeben können, ob Sie eine Kamera auf der Vorderseite oder auf der Rückseite des Geräts bevorzugen.

Wenn in dem angegebenen Bereich ein Gerät gefunden wird, das Kameraprofile unterstützt, wird der [**Id**](https://msdn.microsoft.com/library/windows/apps/br225437)-Wert mit der ID-Zeichenfolge des Geräts zurückgegeben.

[!code-cs[GetVideoProfileSupportedDeviceIdAsync](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetVideoProfileSupportedDeviceIdAsync)]

Wenn die von der **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode zurückgegebene Geräte-ID NULL oder eine leere Zeichenfolge ist, gibt es in dem angegebenen Bereich kein Gerät, das Kameraprofile unterstützt. In diesem Fall sollten Sie das Medienaufnahmegerät ohne Profile initialisieren.

[!code-cs[GetDeviceWithProfileSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetDeviceWithProfileSupport)]

## <a name="select-a-profile-based-on-supported-resolution-and-frame-rate"></a>Auswählen eines Profils basierend auf der unterstützten Auflösung und Bildfrequenz

Um ein Profil mit bestimmten Funktionen auszuwählen, zum Beispiel mit der Fähigkeit, eine bestimmte Auflösung oder Bildfrequenz zu erzielen, sollten Sie zuerst die oben definierte Hilfsmethode aufrufen, um die ID eines Aufnahmegeräts abzurufen, das die Verwendung von Kameraprofilen unterstützt.

Erstellen Sie ein neues [**MediaCaptureInitializationSettings**](https://msdn.microsoft.com/library/windows/apps/br226573)-Objekt, und übergeben Sie die ausgewählte Geräte-ID. Als Nächstes rufen Sie die statische Methode [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) auf, um eine Liste aller vom Gerät unterstützten Kameraprofile zu erhalten.

In diesem Beispiel wird eine „Linq“-Abfragemethode verwendet, die im verwendeten **System.Linq**-Namespace enthalten ist, um ein Profil mit einem [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705)-Objekt auszuwählen. Die Eigenschaften [**Width**](https://msdn.microsoft.com/library/windows/apps/dn926700), [**Height**](https://msdn.microsoft.com/library/windows/apps/dn926697) und [**FrameRate**](https://msdn.microsoft.com/library/windows/apps/dn926696) entsprechen dabei den angeforderten Werten. Wenn eine Übereinstimmung gefunden wird, werden [**VideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn926679) und [**RecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926678) der **MediaCaptureInitializationSettings**-Klasse auf die Werte des anonymen Typs festgelegt, der von der „Linq“-Abfrage zurückgegeben wurde. Wenn keine Übereinstimmung gefunden wird, wird das Standardprofil verwendet.

[!code-cs[FindWVGA30FPSProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindWVGA30FPSProfile)]

Nachdem Sie das **MediaCaptureInitializationSettings**-Objekt mit dem gewünschten Kameraprofil aufgefüllt haben, rufen Sie einfach [**InitializeAsync**](https://msdn.microsoft.com/library/windows/apps/br226598) in Ihrem Medienaufnahmeobjekt auf, um es mit dem gewünschten Profil zu konfigurieren.

[!code-cs[InitCaptureWithProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetInitCaptureWithProfile)]

## <a name="use-media-frame-source-groups-to-get-profiles"></a>Verwenden von Framequellgruppen von Medien, um Profile abzurufen

Ab Windows 10, Version 1803, können Sie die [**MediaFrameSourceGroup**](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframesourcegroup)-Klasse verwenden, um Kameraprofile mit spezifischen Funktionen vor dem Initialisieren des **MediaCapture** Objekts zu verwenden. Framequellgruppen ermöglichen Geräteherstellern das Darstellen der Gruppen von Sensoren oder das Erfassen von Funktionen als ein einziges virtuelles Gerät. Dies ermöglicht Computerphotographieszenarien wie z. B. die gemeinsame Verwendung von Tiefen- und Farbkameras. Es kann jedoch auch verwendet werden, um Kameraprofile für einfache Aufnahmeszenarien auszuwählen. Weitere Informationen zur Verwendung von **MediaFrameSourceGroup** finden Sie unter [Verarbeiten von Medienframes mit "MediaFrameReader"](process-media-frames-with-mediaframereader.md).

Die folgende Beispielmethode zeigt, wie Sie **MediaFrameSourceGroup**-Objekte verwenden, um ein Kameraprofil zu suchen, das ein bekanntes Videoprofil verwendet wie z. B. ein Profil, das HDR oder variable Fotosequenz unterstützt. Erhalten Sie die Liste der verfügbaren Framequellgruppen von Medien auf dem Gerät durch Aufrufen von [**MediaFrameSourceGroup.FindAllAsync**](https://msdn.microsoft.com/library/windows/apps/Windows.Media.Capture.Frames.MediaFrameSourceGroup.FindAllAsync). Durchlaufen Sie die einzelnen Gruppe, und rufen Sie [**MediaCapture.FindKnownVideoProfiles**](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture.findknownvideoprofiles) für eine Liste aller Video-Profile für die aktuelle Quellgruppe auf, die das angegebene Profil unterstützen, in diesem Fall HDR mit WCG Foto. Wenn ein Profil, das den Kriterien entspricht, gefunden wird, erstellen Sie ein neues **MediaCaptureInitializationSettings**-Objekt, und legen Sie das **VideoProfile** auf das ausgewählte Profil fest und **VideoDeviceId** auf die **ID**-Eigenschaft der aktuellen Framequellgruppe von Medien. Sie können z. B. den Wert **KnownVideoProfile.HdrWithWcgVideo** in dieser Methode übergeben, um Medien Capture-Einstellungen zu erhalten, die HDR-Videos unterstützen. Übergeben Sie **KnownVideoProfile.VariablePhotoSequence** zum Abrufen von Einstellungen, die die variable Fotosequenz unterstützen.

 [!code-cs[FindKnownVideoProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindKnownVideoProfile)]

## <a name="use-known-profiles-to-find-a-profile-that-supports-hdr-video-legacy-technique"></a>Verwenden bekannter Profile zum Suchen eines Profils, das HDR-Video unterstützt (Ältere Technik)

> [!NOTE] 
> Die in diesem Abschnitt beschriebenen APIs sind ab Windows 10, Version 1803 veraltet. Lesen Sie den vorherigen Abschnitt **Verwenden von Framequellgruppen von Medien, um Profile abzurufen**.

Das Auswählen eines Profils, das HDR unterstützt, beginnt wie die anderen Szenarien. Erstellen Sie eine **"mediacaptureinitializationsettings"** und eine Zeichenfolge, die die Capture-Geräte-ID enthalten soll. Fügen Sie eine boolesche Variable hinzu, die aufzeichnet, ob HDR-Video unterstützt wird.

[!code-cs[GetHdrProfileSetup](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetHdrProfileSetup)]

Verwenden Sie die oben definierte **GetVideoProfileSupportedDeviceIdAsync**-Hilfsmethode, um die Geräte-ID für ein Aufnahmegerät abzurufen, das Kameraprofile unterstützt.

[!code-cs[FindDeviceHDR](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindDeviceHDR)]

Die statische Methode [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710) gibt die Kameraprofile zurück, die von dem angegebenen Gerät unterstützt werden, das wiederum von dem angegebenen [**KnownVideoProfile**](https://msdn.microsoft.com/library/windows/apps/dn948843)-Wert kategorisiert wird. In diesem Szenario wird der **VideoRecording**-Wert angegeben, um nur Kameraprofile zurückzugeben, die Videoaufnahmen unterstützen.

Durchlaufen Sie die zurückgegebene Liste der Kameraprofile. Durchlaufen Sie für jedes Kameraprofil jede [**VideoProfileMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926695)-Klasse im Profil, und überprüfen Sie, ob die [**IsHdrVideoSupported**](https://msdn.microsoft.com/library/windows/apps/dn926698)-Eigenschaft auf „true“ festgelegt ist. Sobald eine passende Medienbeschreibung gefunden wird, unterbrechen Sie die Schleife und weisen die Profil- und Beschreibungsobjekte dem **MediaCaptureInitializationSettings**-Objekt zu.

[!code-cs[FindHDRProfile](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetFindHDRProfile)]

## <a name="determine-if-a-device-supports-simultaneous-photo-and-video-capture"></a>Ermitteln, ob ein Gerät die gleichzeitige Aufnahme von Fotos und Videos unterstützt

Viele Geräte unterstützen die gleichzeitige Aufnahme von Fotos und Videos. Um festzustellen, ob ein Aufnahmegerät dies unterstützt, rufen Sie [**MediaCapture.FindAllVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926708) auf, um alle vom Gerät unterstützten Kameraprofile abzurufen. Verwenden Sie eine Linkabfrage, um ein Profil zu suchen, das mindestens einen Eintrag für [**SupportedPhotoMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926703) und [**SupportedRecordMediaDescription**](https://msdn.microsoft.com/library/windows/apps/dn926705) aufweist, was bedeutet, dass das Profil die gleichzeitige Aufnahme unterstützt.

[!code-cs[GetPhotoAndVideoSupport](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetGetPhotoAndVideoSupport)]

Sie können diese Abfrage einschränken und nur nach Profilen suchen, die zusätzlich zur gleichzeitigen Videoaufnahme bestimmte Auflösungen oder andere Funktionen unterstützen. Sie können auch die [**MediaCapture.FindKnownVideoProfiles**](https://msdn.microsoft.com/library/windows/apps/dn926710)-Methode verwenden und den [**BalancedVideoAndPhoto**](https://msdn.microsoft.com/library/windows/apps/dn948843)-Wert angeben, um Profile abzurufen, die die gleichzeitige Aufnahme unterstützen. Das Abfragen aller Profile liefert aber genauere Ergebnisse.

## <a name="related-topics"></a>Verwandte Themen

* [Kamera](camera.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
 

 




