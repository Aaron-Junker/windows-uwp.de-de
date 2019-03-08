---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Führen Sie eine Abfrage nach Audio- und Video-Encodern und -Decodern durch, die auf einem Gerät installiert ist.
title: Abfrage für installierte Codecs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, Codecs, Encoder, Decoder, Abfrage
ms.localizationpriority: medium
ms.openlocfilehash: 4241aad5a01617d6a002c6f5d6da0a4bb1455616
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57593865"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Abfrage installierter Codecs auf einem Gerät
Mit der **[CodecQuery](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery)**-Klasse können Sie auf dem aktuellen Gerät installierte Codecs abfragen. Die Liste der Codecs, die in Windows 10 für andere Gerätefamilien enthalten sind, finden Sie im Artikel [Unterstützte Codecs](supported-codecs.md). Da aber Benutzer und Apps zusätzliche Codecs auf einem Gerät installieren können, sollten Sie zur Laufzeit die Codec-Unterstützung abfragen, um festzustellen, welche Codecs auf dem aktuellen Gerät verfügbar sind.

Die CodecQuery-API ist ein Mitglied des **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)**-Namespace, daher müssen Sie diesen Namespace in Ihre App einbeziehen.

Die CodecQuery-API ist ein Mitglied des **[Windows.Media.Core](https://docs.microsoft.com/uwp/api/windows.media.core)**-Namespace, daher müssen Sie diesen Namespace in Ihre App einbeziehen.

[!code-cs[CodecQueryUsing](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetCodecQueryUsing)]

Initialisieren Sie eine neue Instanz der **CodecQuery**-Klasse, indem Sie den Konstruktor aufrufen.

[!code-cs[NewCodecQuery](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetNewCodecQuery)]

Die **[FindAllAsync](https://docs.microsoft.com/uwp/api/windows.media.core.codecquery.findallasync)**-Methode gibt alle installierten Codecs zurück, die den angegebenen Parametern entsprechen. Diese Parameter umfassen einen **[CodecKind](https://docs.microsoft.com/uwp/api/windows.media.core.codeckind)**-Wert, der angibt, ob Sie Audio- oder Video-Codecs oder beide abfragen, einen **[CodecCategory](https://docs.microsoft.com/uwp/api/windows.media.core.codeccategory)**-Wert, der angibt, ob Sie Encoder oder Decoder abfragen, und eine Zeichenfolge, die den Untertyp der abgefragten Mediencodierung, z. B. H.264-Video oder MP3-Audio, angibt.

Geben Sie eine leere Zeichenfolge oder null für den Untertypwert an, um Codecs für alle Untertypen zurückgeben. Im folgenden Beispiel werden alle auf dem Gerät installierten Video-Encoder aufgelistet.

[!code-cs[FindAllEncoders](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetFindAllEncoders)]

Die Untertypzeichenfolge, die Sie an **FindAllAsync** übergeben, kann entweder eine Zeichenfolgendarstellung des Untertyps GUID, die vom System definiert wird, oder ein FOURCC-Code für den Untertyp sein. Die unterstützten [Medienuntertyp-GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa372553(v=vs.85).aspx) sind in den Artikeln [Videountertyp-GUIDs](https://msdn.microsoft.com/library/windows/desktop/aa370819(v=vs.85).aspx) und Videountertyp-GUIDs aufgeführt, die **[CodecSubtypes](https://docs.microsoft.com/uwp/api/windows.media.core.codecsubtypes)**-Klasse stellt aber Eigenschaften bereit, die die GUID-Werte für jeden unterstützten Untertyp zurückgeben. Weitere Informationen zu FOURCC-Codes finden Sie unter [FOURCC-Codes](https://msdn.microsoft.com/library/windows/desktop/dd375802(v=vs.85).aspx). 

Im folgenden Beispiel wird der FOURCC-Code „H264“ festgelegt, um zu ermitteln, ob ein H.264-Video-Decoder auf dem Gerät installiert ist. Sie können diese Abfrage ausführen, bevor Sie versuchen, H.264-Video-Inhalte wiederzugeben. Sie können auch nicht unterstützte Codecs bei der Wiedergabe behandeln. Weitere Informationen finden Sie unter [Behandeln von nicht unterstützten Codecs und unbekannten Fehlern beim Öffnen von Medienelementen](https://docs.microsoft.com/windows/uwp/audio-video-camera/media-playback-with-mediasource#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

[!code-cs[IsH264Supported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsH264Supported)]

Im folgenden Beispiel wird abgefragt, ob ein FLAC-Audio-Encoder auf dem aktuellen Gerät installiert ist. Wenn ja, wird ein **[MediaEncodingProfile](https://docs.microsoft.com/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** für den Untertyp erstellt, mit dem Audioinhalt in eine Datei aufgenommen oder Audioinhalt aus einem anderen Format in eine FLAC-Audiodatei transkodiert werden kann.

[!code-cs[IsFLACSupported](./code/TranscodeWin10/cs/MainPage.xaml.cs#SnippetIsFLACSupported)]

## <a name="related-topics"></a>Verwandte Themen

* [Medienwiedergabe](media-playback.md)
* [Erfassen Sie grundlegende Foto, Video- und Audiodateien mit MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transcodieren von Mediendateien](transcode-media-files.md)
* [Unterstützte codecs](supported-codecs.md)
 

 




