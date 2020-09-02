---
ms.assetid: 0A360481-B649-4E90-9BC4-4449BA7445EF
description: Abfragen von Audio-und Video Encodern und-Decodern, die auf einem Gerät installiert sind.
title: Abfrage für installierte Codecs
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Codec, Encoder, Decoder, Abfrage
ms.localizationpriority: medium
ms.openlocfilehash: f0f1ddff8336594e62ee26b6bf62b062039bf857
ms.sourcegitcommit: c3ca68e87eb06971826087af59adb33e490ce7da
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/02/2020
ms.locfileid: "89363993"
---
# <a name="query-for-codecs-installed-on-a-device"></a>Abfragen von Codecs, die auf einem Gerät installiert sind
Die **[codecquery](/uwp/api/windows.media.core.codecquery)** -Klasse ermöglicht es Ihnen, auf dem aktuellen Gerät installierte Codecs abzufragen. Die Liste der in Windows 10 für verschiedene Gerätefamilien enthaltenen Codecs ist im Artikel [unterstützte Codecs](supported-codecs.md)aufgeführt. da Benutzer und apps jedoch weitere Codecs auf einem Gerät installieren können, empfiehlt es sich, zur Laufzeit die Codec-Unterstützung abzufragen, um zu bestimmen, welche Codecs auf dem aktuellen Gerät verfügbar sind.

Die codecquery-API ist ein Member des **[Windows. Media. Core](/uwp/api/windows.media.core)** -Namespace, sodass Sie diesen Namespace in Ihre APP einschließen müssen.

Die codecquery-API ist ein Member des **[Windows. Media. Core](/uwp/api/windows.media.core)** -Namespace, sodass Sie diesen Namespace in Ihre APP einschließen müssen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetCodecQueryUsing":::

Initialisieren Sie eine neue Instanz der **codecquery** -Klasse, indem Sie den-Konstruktor aufrufen.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetNewCodecQuery":::

Die **[findallasync](/uwp/api/windows.media.core.codecquery.findallasync)** -Methode gibt alle installierten Codecs zurück, die den angegebenen Parametern entsprechen. Diese Parameter enthalten einen **[codeckind](/uwp/api/windows.media.core.codeckind)** -Wert, der angibt, ob Sie Audio-oder Video Codecs oder beides Abfragen, einen **[codeccategory](/uwp/api/windows.media.core.codeccategory)** -Wert, der angibt, ob Sie Codierungen oder Decoders Abfragen, und eine Zeichenfolge, die den unter Typ der Medien Codierung darstellt, für den Sie Abfragen, z. b. H. 264-Video oder MP3-Audiodaten.

Geben Sie eine leere Zeichenfolge oder NULL für den Untertyp Wert an, um Codecs für alle Untertypen zurückzugeben. Im folgenden Beispiel werden alle auf dem Gerät installierten Video Encoder aufgelistet.

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetFindAllEncoders":::

Die untergeordnete Zeichenfolge, die Sie an **findallasync** übergeben, kann entweder eine Zeichen folgen Darstellung der Untertyp-GUID sein, die vom System oder einem FourCC-Code für den Untertyp definiert wird. Der Satz unterstützter untergeordneter Untertypen-GUIDs ist in den Artikeln [audiountertyp-](/windows/desktop/medfound/audio-subtype-guids) GUIDs und [Video Untertyp-GUIDs](/windows/desktop/medfound/video-subtype-guids)aufgelistet, aber die **[codecsubtypes](/uwp/api/windows.media.core.codecsubtypes)** -Klasse stellt Eigenschaften bereit, die die GUID-Werte für jeden unterstützten Untertyp zurückgeben. Weitere Informationen zu FOURCC-Codes finden Sie unter [FOURCC Codes](/windows/desktop/DirectShow/fourcc-codes) 

Im folgenden Beispiel wird der FourCC-Code "H264" angegeben, um zu bestimmen, ob ein H. 264-Video Decoder auf dem Gerät installiert ist. Sie können diese Abfrage ausführen, bevor Sie versuchen, H. 264-Videoinhalte wiederzugeben. Sie können auch nicht unterstützte Codecs zur Wiedergabezeit verarbeiten. Weitere Informationen finden Sie unter [Behandeln von nicht unterstützten Codecs und unbekannten Fehlern beim Öffnen von Medien Elementen](./media-playback-with-mediasource.md#handle-unsupported-codecs-and-unknown-errors-when-opening-media-items).

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsH264Supported":::

Im folgenden Beispiel wird abgefragt, um zu bestimmen, ob ein FLAC-Audioencoder auf dem aktuellen Gerät installiert ist. wenn dies der Fall ist, wird ein **[mediaencodingprofile](/uwp/api/Windows.Media.MediaProperties.MediaEncodingProfile)** -Element für den Untertyp erstellt, das zum Erfassen von Audiodaten in einer Datei oder zum transcodieren von Audiodaten von einem anderen Format in eine FLAC-

:::code language="csharp" source="~/../snippets-windows/windows-uwp/audio-video-camera/TranscodeWin10/cs/MainPage.xaml.cs" id="SnippetIsFLACSupported":::

## <a name="related-topics"></a>Verwandte Themen

* [Medienwiedergabe](media-playback.md)
* [Allgemeine Foto-, Video- und Audioaufnahme mit „MediaCapture“](basic-photo-video-and-audio-capture-with-MediaCapture.md)
* [Transkodieren von Mediendateien](transcode-media-files.md)
* [Unterstützte Codecs](supported-codecs.md)
 

 
