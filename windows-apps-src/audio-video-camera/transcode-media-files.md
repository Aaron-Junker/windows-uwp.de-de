---
ms.assetid: A1A0D99A-DCBF-4A14-80B9-7106BEF045EC
description: Mit den Windows.Media.Transcoding-APIs können Sie Videodateien von einem Format in ein anderes transcodieren.
title: Transkodieren von Mediendateien
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 1a6eb19ca5954b3ce71ecbaefe3339bee78f8717
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616765"
---
# <a name="transcode-media-files"></a>Transkodieren von Mediendateien



Mit den [**Windows.Media.Transcoding**](https://msdn.microsoft.com/library/windows/apps/br207105)-APIs können Sie Videodateien in ein anderes Format transcodieren.

Beim *Transcodieren* wird eine digitale Mediendatei (etwa eine Video- oder Audiodatei) in ein anderes Format konvertiert. Dies geschieht in der Regel durch Decodieren und erneutes Codieren der Datei. Beispiel: Sie konvertieren eine Windows Media-Datei in MP4, um sie auf einem Gerät wiederzugeben, das das MP4-Format unterstützt. Oder Sie können eine Videodatei mit hoher Auflösung in eine niedrigere Auflösung konvertieren. In diesem Fall kann die neu codierte Datei den gleichen Codec wie die Originaldatei verwenden, sie besitzt jedoch ein anderes Codierungsprofil.

## <a name="set-up-your-project-for-transcoding"></a>Einrichten des Projekts für die Transcodierung

Zusätzlich zu den Namespaces, auf die von der Standardprojektvorlage verwiesen wird, müssen Sie auf die folgenden Namespaces verweisen, um Mediendateien mit dem Code in diesem Artikel transcodieren zu können:

[!code-cs[Using](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetUsing)]

## <a name="select-source-and-destination-files"></a>Auswählen der Quell- und Zieldateien

Die Art, wie Ihre App die Quell- und Zieldateien für die Transcodierung ermittelt, hängt von der Implementierung ab. In diesem Beispiel werden eine [**FileOpenPicker**](https://msdn.microsoft.com/library/windows/apps/br207847)-Klasse und eine [**FileSavePicker**](https://msdn.microsoft.com/library/windows/apps/br207871)-Klasse verwendet, um Benutzern die Auswahl einer Quell- und Zieldatei zu ermöglichen.

[!code-cs[TranscodeGetFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeGetFile)]

## <a name="create-a-media-encoding-profile"></a>Erstellen eines Mediencodierungsprofils

Das Codierungsprofil enthält die Einstellungen, die die Codierungsart der Zieldatei festlegen. Hier stehen die meisten Optionen zum Transcodieren einer Datei zur Verfügung.

Die [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)-Klasse stellt statische Methoden zum Erstellen vordefinierter Codierungsprofile bereit:

### <a name="methods-for-creating-audio-only-encoding-profiles"></a>Methoden zum Erstellen von Codierungsprofilen, die nur Audio enthalten

Methode  |Profil  |
---------|---------|
[**CreateAlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createalac)     |Apple Lossless Audio Codec (ALAC)-Audio         |
[**CreateFlac**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createflac)     |Free Lossless Audio Codec (FLAC)-Audio.         |
[**CreateM4a**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createm4a)     |AAC-Audio (M4A)         |
[**CreateMp3**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp3)     |MP3-Audio         |
[**CreateWav**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwav)     |WAV-Audio         |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv)     |Windows Media Audio (WMA)         |

### <a name="methods-for-creating-audio--video-encoding-profiles"></a>Methoden zum Erstellen von Audio-/Videocodierungsprofilen

Methode  |Profil  |
---------|---------|
[**CreateAvi**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createavi) |AVI |
[**CreateHevc**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createhevc) |High Efficiency Video Coding (HEVC)-Video, auch als H.265-Video bezeichnet |
[**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4) |MP4-Video (H.264-Video und AAC-Audio) |
[**CreateWmv**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createwmv) |Windows Media Video (WMV) |


Der folgende Code erstellt ein Profil für MP4-Video:

[!code-cs[TranscodeMediaProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeMediaProfile)]

Die statische [**CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4)-Methode erstellt ein MP4-Codierungsprofil. Der Parameter für diese Methode gibt die Zielauflösung für das Video an. In diesem Fall bedeutet [**VideoEncodingQuality.hd720p**](https://msdn.microsoft.com/library/windows/apps/hh701290): 1280 x 720 Pixel mit 30 Bildern pro Sekunde. ("720p" steht für 720 progressive Scanzeilen pro Frame). Die anderen Methoden zum Erstellen von vordefinierten Profile, die alle folgen diesem Muster.

Alternativ können Sie mit der [**MediaEncodingProfile.CreateFromFileAsync**](https://msdn.microsoft.com/library/windows/apps/hh701047)-Methode ein Profil erstellen, das einer vorhandenen Mediendatei entspricht. Wenn Sie die genauen gewünschten Codierungseinstellungen kennen, können Sie auch ein neues [**MediaEncodingProfile**](https://msdn.microsoft.com/library/windows/apps/hh701026)-Objekt erstellen und die Profildetails ausfüllen.

## <a name="transcode-the-file"></a>Transcodieren der Datei

Erstellen Sie zum Transcodieren der Datei ein neues [**MediaTranscoder**](https://msdn.microsoft.com/library/windows/apps/br207080)-Objekt, und rufen Sie die [**MediaTranscoder.PrepareFileTranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700936)-Methode auf. Übergeben Sie die Quelldatei, die Zieldatei und das Codierungsprofil. Rufen Sie dann die [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)-Methode für das [**PrepareTranscodeResult**](https://msdn.microsoft.com/library/windows/apps/hh700941)-Objekt auf, das mit dem asynchronen Transcodierungsvorgang zurückgegeben wurde.

[!code-cs[TranscodeTranscodeFile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeTranscodeFile)]

## <a name="respond-to-transcoding-progress"></a>Reaktion auf Transcodierungsfortschritt

Sie können Reaktionsereignisse registrieren, wenn sich der Fortschritt der asynchronen [**TranscodeAsync**](https://msdn.microsoft.com/library/windows/apps/hh700946)-Klasse ändert. Diese Ereignisse sind Teil des asynchronen Programmierungsframeworks für Apps für die universelle Windows-Plattform (UWP) und nicht für die Transcodierungs-API spezifisch.

[!code-cs[TranscodeCallbacks](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetTranscodeCallbacks)]


## <a name="encode-a-metadata-stream"></a>Codieren Sie einen Metadatenstrom
Ab Windows 10, Version 1803, können Sie von zeitgesteuerten Metadaten beim einschließen beim Transcodieren von Mediendateien. Im Gegensatz zu den video transcodieren Profil vorstehenden Beispielen das Verwenden der integrierten mediencodierung Erstellungsmethoden, wie z. B. [ **MediaEncodingProfile.CreateMp4**](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.mediaencodingprofile.createmp4), Sie müssen manuell erstellen, die Metadaten-Codierung das Profil, das den Typ der Metadaten unterstützen, die Sie codieren.

Dieser erste Schritt beim Erstellen von einem Metadaten-Incoding-Profil ist die Erstellung einer [**TimedMetadataEncodingProperties**] Objekt, das beschreibt, die Codierung der Metadaten zu transcodierende. Die Untertyp-Eigenschaft ist eine GUID, die den Typ der Metadaten angibt. Die Codierung Details für jeden Metadatentyp ist eine proprietäre und wird nicht von Windows bereitgestellt. In diesem Beispiel wird die GUID für GoPro Metadaten (Gprs) verwendet. Als Nächstes [ **SetFormatUserData** ](https://docs.microsoft.com/uwp/api/windows.media.mediaproperties.timedmetadataencodingproperties.setformatuserdata) wird aufgerufen, um ein binäres Blob, beschreibt das Stream-Format, das für die Metadaten-Format festgelegt. Als Nächstes wird ein **TimedMetadataStreamDescriptor**(https://docs.microsoft.com/uwp/api/windows.media.core.timedmetadatastreamdescriptor) wird von der Codierung Eigenschaften mit Getter, erstellt und ein Track-Bezeichnung und den Namen einer Anwendung Lesen des Datenstroms Endcoded Metadatenstreams identifizieren ermöglichen, und optional der Name des Datenstroms in der Benutzeroberfläche angezeigt. 
 
[!code-cs[GetStreamDescriptor](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetStreamDescriptor)]

Nach dem Erstellen der **TimedMetadataStreamDescriptor**, können Sie erstellen eine **MediaEncodingProfile** , beschreibt das Video, Audio und Metadaten in der Datei codiert werden. Die **TimedMetadataStreamDescriptor** erstellt im letzten Beispiel wird in dieser Beispiel-Hilfsfunktion übergeben und hinzugefügt wird die **MediaEncodingProfile** durch Aufrufen von [  **SetTimedMetadataTracks**](https://docs.microsoft.com/en-us/uwp/api/windows.media.mediaproperties.mediaencodingprofile.settimedmetadatatracks).

[!code-cs[GetMediaEncodingProfile](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetGetMediaEncodingProfile)]
 

 




