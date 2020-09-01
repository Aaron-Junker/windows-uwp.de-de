---
title: Audio für Spiele
description: Hier erfahren Sie, wie Sie Musik und Sound entwickeln und in Ihr DirectX-Spiel integrieren. Außerdem erfahren Sie, wie Sie Audiosignale verarbeiten, um dynamischen und positionsbezogenen Sound zu erzeugen.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Games, Audiodatei, DirectX
ms.localizationpriority: medium
ms.openlocfilehash: f0493c68fcb3453890f95c123b0c870cf321fe7e
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162914"
---
# <a name="audio-for-games"></a>Audio für Spiele

Hier erfahren Sie, wie Sie Musik und Sound entwickeln und in Ihr DirectX-Spiel integrieren. Außerdem erfahren Sie, wie Sie Audiosignale verarbeiten, um dynamischen und positionsbezogenen Sound zu erzeugen.

Für die Audioprogrammierung empfiehlt sich die Verwendung der [XAudio2](/windows/win32/xaudio2/xaudio2-apis-portal) -Bibliothek in DirectX oder der Windows-Runtime [Audiographen-](../audio-video-camera/audio-graphs.md) APIs. Wir verwenden XAudio2 hier. XAudio2 ist eine untergeordnete Audiobibliothek, die eine grundlegende Signalverarbeitung und -abmischung für Spiele bereitstellt und eine Vielzahl von Formaten unterstützt.

Einfache Sounds sowie eine einfache Musikwiedergabe können auch mithilfe von [Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk) implementiert werden. Microsoft Media Foundation ist eigentlich für die Wiedergabe von Mediendateien und Datenströmen (sowohl Audio als auch Video) konzipiert, kann aber auch in Spielen eingesetzt werden und ist besonders hilfreich bei Filmszenen oder Spielkomponenten ohne Interaktionsmöglichkeit.

## <a name="concepts-at-a-glance"></a>Konzepte auf einen Blick

Im Anschluss finden Sie einige Konzepte für die Audioprogrammierung, die in diesem Abschnitt zur Anwendung kommen.

-   Signale bilden die Grundeinheit der Soundprogrammierung (vergleichbar mit Pixeln bei der Grafik). Die digitalen Signalprozessoren (Digital Signal Processors, DSPs), von denen sie verarbeitet werden, sind gewissermaßen die Pixelshader für den Sound des Spiels. Sie können Signale transformieren, kombinieren oder filtern. Mithilfe der DSP-Programmierung können Sie die Soundeffekte und die Musik Ihres Spiels ganz flexibel und mit der gewünschten Komplexität verändern.
-   Bei Stimmen handelt es sich um einen Submix aus mindestens zwei Signalen. XAudio2 bietet drei Arten von Stimmobjekten: Quellstimme, Submixstimme und Masterstimme. Quellstimmen verarbeiten die vom Client bereitgestellten Audiodaten. Quell- und Submixstimmen senden ihre Ausgabe an mindestens eine Submix- oder Masterstimme. Submix- und Masterstimmen mischen die Audiodaten aller Stimmen, von denen sie Daten erhalten, und verarbeiten das Ergebnis. Masterstimmen schreiben Audiodaten auf ein Audiogerät.
-   Beim Mixing werden mehrere getrennte Stimmen – beispielsweise die Soundeffekte und die Hintergrundgeräusche einer Szene – in einem einzelnen Stream miteinander kombiniert. Beim Submixing werden mehrere getrennte Signale – beispielsweise die Soundkomponenten eines Motorengeräuschs – zu einer Stimme kombiniert.
-   Audioformate. Musik und Soundeffekte für Ihr Spiel können in vielen unterschiedlichen digitalen Formaten gespeichert werden. Zur Auswahl stehen unkomprimierte Formate wie WAV sowie komprimierte Formate wie MP3 und OGG. Je stärker die Komprimierung eines Audiosamples (üblicherweise abzulesen an der Bitrate), desto schlechter die Klangtreue, da die Verringerung der Bitrate höhere Verluste nach sich zieht. Aufgrund der Klangtreueunterschiede bei verschiedenen Komprimierungsschemas und Bitraten empfiehlt es sich, ein wenig zu experimentieren, um eine möglichst gute Lösung für Ihr Spiel zu finden.
-   Samplingrate und Qualität. Sound kann unterschiedliche Samplingraten haben. Mit abnehmender Samplingrate verschlechtert sich die Klangtreue allerdings erheblich. Die Samplingrate für Sound in CD-Qualität beträgt 44,1 kHz (44.100 Hz). Wenn es bei Ihrem Sound nicht auf hohe Klangtreue ankommt, können Sie eine geringere Samplingrate wählen. Eine höhere Samplingrate empfiehlt sich unter Umständen für professionelle Audioanwendungen, bei einem Spiel ist sie dagegen nicht unbedingt erforderlich – es sei denn, das Spiel benötigt Sound mit professioneller Klangtreue.
-   Soundquellen. Soundquellen in XAudio2 sind Punkte, von denen Sound ausgeht – ganz gleich, ob es sich dabei um einen Piepton im Hintergrund oder um einen fetzigen Rocksong aus einer Stereoanlage im Spiel handelt. Die Quellen werden anhand von Weltkoordinaten angegeben.
-   Soundempfänger. Beim Soundempfänger handelt es sich häufig um den Spieler, in aufwendigeren Spielen möglicherweise auch um eine Entität mit künstlicher Intelligenz, die den von einer Soundquelle stammenden Sound verarbeitet. Dieser Sound kann per Submixing dem Audiodatenstrom zugeführt und so für den Spieler wiedergegeben werden. Alternativ können Sie den Sound aber auch verwenden, um eine bestimmte Aktion im Spiel (beispielsweise die Alarmierung einer als Empfänger markierten KI-Wache) auszulösen.

## <a name="design-considerations"></a>Überlegungen zum Entwurf

Audio spielt beim Entwerfen und Entwickeln von Spielen eine immens wichtige Rolle. Viele Spieler erinnern sich noch an eher durchschnittliche Spiele, die allein dank ihres einprägsamen Soundtracks, dank großartiger Sprecher und einer tollen Soundabmischung oder allgemein dank einer herausragenden Audioproduktion Kultstatus erreicht haben. Musik und Sound definieren die Persönlichkeit eines Spiels. Sie liefern auch das Hauptmotiv, das das Spiel definiert und von ähnlichen Spielen abhebt. Der Aufwand, den Sie für die Ausarbeitung und Entwicklung des Audioprofils Ihres Spiel betreiben, zahlt sich in jedem Fall aus.

Mit positionsbezogenem 3D-Sound können Sie die 3D-Grafik Ihres Spiels um ein zusätzliches immersives Element erweitern. Falls Sie ein komplexes Spiel entwickeln, das eine Welt simuliert oder einen filmähnlichen Stil erfordert, sollten Sie die Verwendung von Techniken für positionsbezogenen 3D-Sound in Betracht ziehen, um den Spieler voll und ganz in die Spielwelt eintauchen zu lassen.

## <a name="directx-audio-development-roadmap"></a>Roadmap für DirectX-Audioentwicklung

### <a name="xaudio2-conceptual-resources"></a>Konzeptionelle Ressourcen zu XAudio2

XAudio2 ist die Audiomixingbibliothek für DirectX, die in erster Linie zum Entwickeln von Audiomodulen mit hoher Leistung für Spiele gedacht ist. Spieleentwicklern, die Soundeffekte und Hintergrundmusik zu ihren modernen Spielen hinzufügen möchten, bietet XAudio2 ein Modul für Audiodiagramme und zum Mischen mit geringer Latenz und Unterstützung für dynamische Puffer, synchrone Wiedergabe genau nach Beispiel und implizite Quellratenumwandlung.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-introduction">Einführung in XAudio2</a></p></td>
<td align="left"><p>Dieses Thema enthält eine Liste der von XAudio2 unterstützten Audioprogrammierfeatures.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/getting-started">Erste Schritte mit XAudio2</a></p></td>
<td align="left"><p>Dieses Thema enthält Informationen zu wichtigen XAudio2-Konzepten, XAudio2-Versionen und zum RIFF-Audioformat.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/common-audio-concepts">Allgemeine Konzepte für die Audioprogrammierung</a></p></td>
<td align="left"><p>Dieses Thema bietet einen Überblick über allgemeine Audiokonzepte, mit denen ein Audioentwickler vertraut sein sollte.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-voices">XAudio2-Stimmen</a></p></td>
<td align="left"><p>Dieses Thema bietet einen Überblick über XAudio2-Stimmen, die für Submixing und das Verarbeiten und Mastern von Audiodaten verwendet werden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-callbacks">XAudio2-Rückrufe</a></p></td>
<td align="left"><p>In diesem Thema werden die XAudio2-Rückrufe behandelt, mit denen Unterbrechungen bei der Audiowiedergabe verhindert werden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/audio-graphs">XAudio2-Audiodiagramme</a></p></td>
<td align="left"><p>In diesem Thema werden die XAudio2-Audioverarbeitungsdiagramme behandelt. Diese empfangen eine Reihe von Audiostreams vom Client als Eingabe, verarbeiten sie und übergeben das Endergebnis an ein Audiogerät.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-audio-effects">XAudio2-Audioeffekte</a></p></td>
<td align="left"><p>In diesem Thema werden XAudio2-Audioeffekte behandelt, die eingehende Audiodaten empfangen und vor der Weitergabe bestimmte Vorgänge für die Daten (z. B. einen Halleffekt) ausführen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/xaudio2-streaming-audio-data">Streamen von Audiodaten mit XAudio2</a></p></td>
<td align="left"><p>In diesem Thema wird das Audiostreaming mit XAudio2 behandelt.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/x3daudio">X3DAudio</a></p></td>
<td align="left"><p>In diesem Thema wird X3DAudio vorgestellt. Dabei handelt es sich um eine API, die in Verbindung mit XAudio2 verwendet wird, um die Illusion eines 3D-Klangeffekts zu erzeugen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/programming-reference">XAudio2-Programmierreferenz</a></p></td>
<td align="left"><p>Dieser Abschnitt enthält die vollständige Referenz für die XAudio2-APIs.</p></td>
</tr>
</tbody>
</table>

### <a name="xaudio2-how-to-resources"></a>XAudio2-Anleitungsressourcen

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--initialize-xaudio2">So wird's gemacht: Initialisieren von XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie XAudio2 für die Audiowiedergabe initialisieren, indem Sie eine Instanz des XAudio2-Moduls und eine Masterstimme erstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--load-audio-data-files-in-xaudio2">So wird's gemacht: Laden von Datendateien in XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die zur Wiedergabe von Audiodaten in XAudio2 erforderlichen Strukturen füllen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--play-a-sound-with-xaudio2">So wird's gemacht: Wiedergeben von Ton mit XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie zuvor geladene Audiodaten in XAudio2 wiedergeben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-submix-voices">So wird's gemacht: Verwenden von Submixstimmen</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Gruppen von Stimmen festlegen, um ihre Ausgabe an dieselbe Submixstimme zu senden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-source-voice-callbacks">So wird's gemacht: Verwenden der Rückrufe für Quellstimmen</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die XAudio2-Quellstimmrückrufe verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-engine-callbacks">So wird's gemacht: Verwenden der Modulrückrufe</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie die XAudio2-Modulrückrufe verwenden.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">So wird's gemacht: Erstellen eines grundlegenden Audioverarbeitungsdiagramms</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie ein Audioverarbeitungsdiagramm erstellen, das sich aus einer Masterstimme und einer Quellstimme zusammensetzt.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--dynamically-add-or-remove-voices-from-an-audio-graph">So wird's gemacht: Dynamisches Hinzufügen und Entfernen von Stimmen zu bzw. aus einem Audiodiagramm</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Submixstimmen zu einem Diagramm hinzufügen oder daraus entfernen, das anhand der Schritte unter <a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--build-a-basic-audio-processing-graph">So wird's gemacht: Erstellen eines grundlegenden Audioverarbeitungsdiagramms</a> erstellt wurde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-effect-chain">So wird's gemacht: Erstellen einer Effektkette</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie eine Effektkette auf eine Stimme anwenden, um die benutzerdefinierte Verarbeitung der Audiodaten für diese Stimme zu ermöglichen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--create-an-xapo">So wird's gemacht: Erstellen eines XAPOs</a></p></td>
<td align="left"><p>Erfahren Sie, wie Sie <a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapo"><strong>ixapo</strong></a> implementieren, um ein XAudio2-Audioverarbeitungs Objekt (xapo) zu erstellen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--add-run-time-parameter-support-to-an-xapo">So wird's gemacht: Hinzufügen der Laufzeitparameterunterstützung zu einem XAPO</a></p></td>
<td align="left"><p>Erfahren Sie, wie Sie einem xapo Unterstützung für Lauf Zeitparameter hinzufügen, indem Sie die <a href="https://docs.microsoft.com/windows/desktop/api/xapo/nn-xapo-ixapoparameters"><strong>ixapoparameters</strong></a> -Schnittstelle implementieren.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-an-xapo-in-xaudio2">So wird's gemacht: Verwenden eines XAPOs in XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie einen Effekt verwenden, der als XAPO in einer XAudio2-Effektkette implementiert wurde.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--use-xapofx-in-xaudio2">So wird's gemacht: Verwenden von XAPOFX in XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie einen der Effekte in XAPOFX in einer XAudio2-Effektkette verwenden.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--stream-a-sound-from-disk">So wird's gemacht: Streamen von Sound von einem Datenträger</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie Audiodaten in XAudio2 streamen, indem Sie einen separaten Thread zum Lesen eines Audiopuffers erstellen und diesen Thread mithilfe von Rückrufen steuern.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--integrate-x3daudio-with-xaudio2">So wird's gemacht: Integrieren von X3DAudio in XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie mit X3DAudio die Lautstärke- und Tonhöhenwerte für XAudio2-Stimmen sowie die Parameter für den integrierten XAudio2-Halleffekt bereitstellen.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/how-to--group-audio-methods-as-an-operation-set">So wird's gemacht: Gruppieren von Audiomethoden als Vorgangssatz</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie XAudio2-Vorgangssätze verwenden, damit eine Gruppe von Methodenaufrufen gleichzeitig wirksam wird.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/xaudio2/debugging-audio-glitches-in-xaudio2">Debuggen von Audiostörungen in XAudio2</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie der Debuggingprotokolliergrad für XAudio2 festgelegt wird.</p></td>
</tr>
</tbody>
</table>

### <a name="media-foundation-resources"></a>Media Foundation-Ressourcen

Media Foundation (MF) ist eine Medienplattform zum Streamen von Audio- und Videodaten. Mithilfe der Media Foundation-APIs können Sie mit verschiedenen Algorithmen codierte und komprimierte Audio- und Videodaten streamen. Die Plattform ist nicht für Echtzeitspielszenarien konzipiert. Stattdessen bietet sie leistungsstarke Tools und breite Codec-Unterstützung für eine lineare Aufnahme und Präsentation von Audio- und Videokomponenten.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/about-the-media-foundation-sdk">Info über Media Foundation</a></p></td>
<td align="left"><p>Dieser Abschnitt enthält allgemeine Informationen zu den Media Foundation-APIs und die verfügbaren Tools zu ihrer Unterstützung.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming--essential-concepts">Media Foundation: Grundlegende Konzepte</a></p></td>
<td align="left"><p>In diesem Thema werden einige Konzepte vorgestellt, die Sie vor dem Schreiben einer Media Foundation-Anwendung kennen müssen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-architecture">Media Foundation-Architektur</a></p></td>
<td align="left"><p>In diesem Abschnitt werden das allgemeine Design von Microsoft Media Foundation sowie die Mediengrundtypen und die verwendete Verarbeitungspipeline beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-capture">Audio-/Videoaufzeichnung</a></p></td>
<td align="left"><p>In diesem Thema wird die Verwendung von Microsoft Media Foundation zum Aufzeichnen von Audio- und Videodaten beschrieben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/audio-video-playback">Audio-/Videowiedergabe</a></p></td>
<td align="left"><p>In diesem Thema wird die Implementierung der Audio- oder Videowiedergabe in Ihrer App beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/supported-media-formats-in-media-foundation">Unterstützte Medienformate in Media Foundation</a></p></td>
<td align="left"><p>In diesem Thema sind die Medienformate aufgeführt, für die Microsoft Media Foundation systemeigene Unterstützung bietet. (Drittanbieter können zusätzliche Formate durch Erstellung benutzerdefinierter Plug-Ins unterstützen.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/encoding-and-file-authoring">Codierung und Dateierstellung</a></p></td>
<td align="left"><p>In diesem Thema wird die Verwendung von Microsoft Media Foundation zum Codieren von Audio- und Videodaten und zum Erstellen von Mediendateien beschrieben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/windows-media-codecs">Windows Media-Codecs</a></p></td>
<td align="left"><p>In diesem Thema wird beschrieben, wie Sie mit den Features der Windows Media-Codecs für Audio- und Videodaten komprimierte Datenströme erzeugen und wiedergeben.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-programming-reference">Media Foundation-Programmierreferenz</a></p></td>
<td align="left"><p>Dieser Abschnitt enthält Referenzinformationen für die Media Foundation-APIs.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/desktop/medfound/media-foundation-sdk-samples">Media Foundation-SDK-Beispiele</a></p></td>
<td align="left"><p>In diesem Abschnitt sind Beispiel-Apps aufgeführt, die die Verwendung von Media Foundation veranschaulichen.</p></td>
</tr>
</tbody>
</table>

### <a name="windows-runtime-xaml-media-types"></a>Medientypen der Windows-Runtime-XAML

Wenn Sie [DirectX-XAML-Interop](/previous-versions/windows/apps/hh825871(v=win.10))verwenden, können Sie die Windows-Runtime XAML-Medien-APIs in Ihre UWP-apps integrieren, indem Sie DirectX mit C++ für einfachere Spiel Szenarios verwenden.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Thema</th>
<th align="left">BESCHREIBUNG</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.MediaElement"><strong>Windows. UI. XAML. Controls. Media Element</strong></a></p></td>
<td align="left"><p>XAML-Element, das ein Objekt darstellt, das Audio- oder Videodaten oder beide Datentypen enthält.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/index">Audio, Video und Kamera</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie grundlegende Audio- und Videoinhalte in Ihrer UWP-App (Universelle Windows-Plattform) integrieren.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie in Ihrer UWP-App eine lokal gespeicherte Mediendatei wiedergeben.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/media-playback">MediaElement</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie in Ihrer UWP-App eine Mediendatei mit geringer Wartezeit streamen.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://docs.microsoft.com/windows/uwp/audio-video-camera/media-casting">Medienumwandlung</a></p></td>
<td align="left"><p>Hier erfahren Sie, wie Sie mit dem Vertrag für "Wiedergeben auf" Medien aus Ihrer UWP-App auf anderen Geräten streamen.</p></td>
</tr>
</tbody>
</table>

## <a name="reference"></a>Verweis

-   [XAudio2-Einführung](/windows/desktop/xaudio2/xaudio2-introduction)
-   [XAudio2-Programmieranleitung](/windows/desktop/xaudio2/programming-guide)
-   [Übersicht über Microsoft Media Foundation](/windows/desktop/medfound/microsoft-media-foundation-sdk)

## <a name="related-topics"></a>Zugehörige Themen

-   [XAudio2-Programmieranleitung](/windows/desktop/xaudio2/programming-guide)