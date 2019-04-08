---
Description: Sie können die Bildschirmfotos, Logos und andere Grafikobjekte (z. B. Trailer und Werbebilder) auswählen, die im Store-Eintrag Ihrer App enthalten sein sollen.
title: App-Screenshots, -Bilder und -Trailer
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Trailer, Video, Screenshot, Bild, Symbol, Store-Eintrag, Store-Eintragsbilder
ms.localizationpriority: medium
ms.openlocfilehash: 0ae5b68d73a3776adf6250dbb96de827a106a6c5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610185"
---
# <a name="app-screenshots-images-and-trailers"></a>App-Screenshots, -Bilder und -Trailer

Durch aussagekräftige Bilder kann die Aufmerksamkeit potentieller Kunden im Store auf Ihre App gelenkt werden.

Sie können angeben, [Screenshots](#screenshots), [Logos](#store-logos), [Nachspänne](#trailers), und andere Grafikobjekte in Ihrer app-Store-Liste eingeschlossen werden sollen. Einige dieser Elemente sind erforderlich, während andere optional sind, (auch wenn einige der optionalen Bilder wichtig für die beste Anzeige im Store sind).

Während der [App-Übermittlung](app-submissions.md) geben Sie diese Grafikobjekte im Schritt [Store-Einträge](create-app-store-listings.md) an. Beachten Sie, dass es vom Betriebssystem des Kunden und weiteren Faktoren abhängt, wie Bilder im Store angezeigt werden.

Der Store können Sie auch das Symbol Ihrer app und andere Bilder, die Sie in Ihrer app-Paket einschließen. Führen Sie das [Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) aus, um zu ermitteln, ob erforderliche Bilder fehlen, bevor Sie Ihre App übermitteln. Anleitungen und Empfehlungen zu diesen Images finden Sie unter [-App-Symbole und Logos](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Screenshots

Screenshots sind die Bilder Ihrer App, die Ihren Kunden im Store-Eintrag der App angezeigt werden.

Sie haben die Möglichkeit zum Bereitstellen von Bildschirmfotos für andere Gerätefamilien, die Ihre App unterstützt, damit die entsprechenden Bildschirmfotos angezeigt werden, wenn ein Kunde den Store-Eintrag Ihrer App auf diesem Gerätetyp anzeigt. 

Für die Übermittlung ist nur ein Screenshot (für jede Gerätefamilie) erforderlich. Sie können jedoch bis zu neun Desktop-Screenshots und bis zu acht Screenshots für die anderen Gerätefamilien bereitstellen. Wir empfehlen, mindestens 4 Screenshots für jede Gerätefamilie hinzuzufügen, die von Ihrer App unterstützt werden, damit Benutzer sehen, wie die App auf ihrem Gerätetyp aussieht. (Fügen Sie keine Screenshots für gerätefamilien, die Ihre app nicht unterstützt.) Beachten Sie, dass **Desktop** Screenshots wird auch angezeigt werden, um Kunden auf Surface Hub-Geräte.

> [!NOTE]
> Microsoft Visual Studio enthält ein [Tool zum Erstellen von Screenshots](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Jeder Screenshot muss einer PNG-Datei im Quer- oder Hochformat entsprechen, und die Datei darf nicht größer als 50 MB sein.

Die Größenanforderungen hängen von der Gerätefamilie ab:
- Desktop: 1366 x 768 Pixel oder größer. Unterstützt 4K-Bilder (3840x2160). (Wird ebenfalls den Kunden auf Surface Hub-Geräten angezeigt.)
- Mobilgeräte: Images müssen eine der folgenden sein: 1080 x 1920, 1920 × 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 oder 480 x 800 Pixel.
- Xbox: 3480 x 2160 Pixel oder kleiner. Unterstützt 4K-Bilder (3840x2160).
- Holographic: 1268 x 720 Pixel oder größer. Unterstützt 4K-Bilder (3840x2160).

Beachten Sie für die optimale Anzeige die folgenden Richtlinien beim Erstellen Ihrer Bildschirmfotos:
- Platzieren Sie wichtige visuelle Elemente und Text in den oberen drei Vierteln des Bilds. Textüberlagerungen können im unteren Viertel angezeigt werden. 
- Fügen Sie Ihren Screenshots keine zusätzlichen Logos, Symbole oder Marketingnachrichten hinzu.
- Verwenden Sie nicht sehr helle oder dunkle Farben oder Streifen mit starkem Kontrast, die die Lesbarkeit von Textüberlagerungen beeinträchtigen können.

Sie können auch eine Kurzbeschreibung von maximal 200 Zeichen für die einzelnen Screenshots angeben.

> [!TIP]
> Bildschirmfotos werden in Ihrem Eintrag der Reihenfolge nach angezeigt. Nachdem Sie Ihre Bildschirmfotos hochgeladen haben, können Sie sie ziehen und ablegen, um sie neu anzuordnen. 

Hinweis: Wenn Sie Store-Einträge für [mehrere Sprachen](supported-languages.md) erstellen, erhalten Sie für jede Sprache eine Seite vom Typ **Store-Eintrag**. Sie müssen Bilder für jede Sprache separat hochladen (auch wenn Sie dieselben Bilder verwenden), einschließlich Beschriftungen, die für die einzelnen Sprachen verwendet werden. (Wenn Sie die Store-Angebote in vielen Sprachen verfügen, Sie finden es möglicherweise einfacher für das Aktualisieren von [Exportieren Ihrer Daten auflisten sowie zum Offlinearbeiten](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Store-Logos

Sie können Store-Logos hochladen, um eine angepasstere Darstellung im Store zu erstellen. Es wird empfohlen, dass Sie diese Bilder für eine optimale Anzeige im Store-Eintrag für alle Geräte und Betriebssystemversionen bereitstellen, die Ihre App unterstützt. Wenn Ihre App für Kunden auf Xbox verfügbar ist, sind einige dieser Bilder erforderlich.

Sie können diese Bilder als PNG-Dateien (maximal 50 MB) in drei Größen bereitstellen, die jeweils den folgenden Richtlinien entsprechen sollten.

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>9:16 – Plakate (720x1080 oder 1440x2160 Pixel)

Diese Größe wird als Logo in Store-Einträgen für Kunden unter Windows 10 und auf Xbox-Geräten verwendet, daher wird **dringend empfohlen**, dieses Bild für die ordnungsgemäße Anzeige bereitzustellen. Ihr Angebot sieht unter Umständen nicht gut, wenn Sie nicht, Sie es fügen, und nicht konsistent mit anderen Auflistungen, die Kunden beim Durchsuchen der Store angezeigt. Dieses Bild kann ebenfalls in den Suchergebnissen oder in speziell zusammengestellten Sammlungen verwendet werden.

Dieses Bild sollte den Namen Ihrer App enthalten, und Text auf dem Bild sollte die Lesbarkeitsanforderungen (Kontrastverhältnis von 4,5:1) erfüllen. Beachten Sie, dass auf dem unteren Viertel des Bilds Textüberlagerungen angezeigt werden. Stellen Sie sicher, dass Sie dort keinen Text oder das Hauptbild einfügen.

> [!NOTE]
> Wenn Ihre App für Kunden auf Xbox verfügbar ist, ist dieses Bild **erforderlich** und muss den Produkttitel enthalten. Der Titel muss im oberen Dreiviertel des Bilds angezeigt werden, da Textüberlagerungen auf den unteren Viertel des Bilds angezeigt werden.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 – Verpackungsillustration (1080 x 1080 or 2160 x 2160 Pixel)

Dieses Bild kann auf verschiedenen Store-Seiten für Windows 10 angezeigt werden (einschließlich Xbox), und wenn Sie keine **9:16 Illustrationen** angeben, wird dieses als zentrales Logo verwendet. Dieses Bild muss auch den Namen Ihrer App enthalten. Beachten Sie, dass auf dem unteren Viertel des Bilds Textüberlagerungen angezeigt werden und fügen Sie dort keinen Text oder das Hauptbild ein. Stellen Sie sicher, dass Sie den Namen Ihrer App im Bild einfügen. 

> [!NOTE]
> Wenn Ihre App für Kunden auf Xbox verfügbar ist, ist dieses Bild **erforderlich** und muss den Produkttitel enthalten. Der Titel muss im oberen Dreiviertel des Bilds angezeigt werden, da Textüberlagerungen auf den unteren Viertel des Bilds angezeigt werden.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 Symbol für App-Kachel (300 x 300 Pixel)

Dieses Bild ist für die ordnungsgemäße Anzeige auf Windows Phone 8.1 und früheren Versionen erforderlich. Wenn es sich bei Ihrer app zuvor veröffentlichte unterstützt Windows Phone 8.1 oder früher, und Sie können dieses Image nicht angeben, werden diese Kunden ein leeres Symbol mit Ihrer app-Liste angezeigt. (Dies gilt auch für Kunden mit Windows 10, wenn die app nur Pakete mit dem Ziel Windows Phone 8.1 oder früher aufweist.)

Wenn Ihrer Übermittlung *nur* UWP-Pakete enthält, müssen Sie nicht dieses Bild angeben (es sei denn, Sie das Kontrollkästchen für **für Kunden mit Windows 10 und Xbox, zeigen Sie hochgeladenen Logo-Bilder anstelle von Images von Meine Pakete** , wie im nächsten Abschnitt beschrieben).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Anzeige nur hochgeladen Logobilder in den Store.

Sie haben die Möglichkeit zu verhindern, dass der Store die Logobilder in Ihrer app-Paketen verwenden, wenn Ihr Angebot für Kunden, die unter Windows 10 (einschließlich Xbox) angezeigt, stattdessen den Store, die nur Bilder verwenden, die Sie hochladen. Dies bietet Ihnen mehr Kontrolle über die Darstellung Ihrer App in verschiedenen Anzeigen im Store für Kunden auf Windows 10 (einschließlich Xbox). (Wenn Ihre app zuvor veröffentlichte frühere Betriebssystemversionen unterstützt, können Kunden weiterhin Bilder aus Ihren Paketen angezeigt.)

Um den Store verwenden nur die Images, die Sie (für Kunden mit Windows 10 hochladen, einschließlich Xbox), und verwenden keine Bilder aus Ihren Paketen zu erhalten, überprüfen Sie das Kontrollkästchen **für Kunden mit Windows 10 und Xbox, zeigen Sie hochgeladenen Logo-Bilder anstelle von Images von Meine Pakete**.

Wenn Sie dieses Kontrollkästchen aktivieren, ein neuer Abschnitt namens **Store Anzeigen von Bildern** angezeigt wird. Hier können Sie 3-Images, einschließlich Hochladen der **1:1-app-Kachel-Symbol (300 x 300 Pixel)** Größe (Wenn Sie das Kontrollkästchen aktivieren, das Feld zu diesem Image wechselt in diesem Abschnitt). Es wird empfohlen, alle drei Bildgrößen bereitstellen, wenn Sie diese Option verwenden: 71 x 71, 300 x 300 und 150 x 150 Pixel. Es ist jedoch nur die Größe 300 x 300 erforderlich.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Nachspänne und zusätzliche Ressourcen

In diesem Abschnitt können Sie Grafiken bereitstellen, damit Sie Ihr Produkt im Store effektiver präsentieren können. Wir empfehlen die Bereitstellung dieser Bilder, um einen einladenden Store-Eintrag zu erstellen.

> [!TIP]
> Die [16:9 Superheld Art](#windows-10-and-xbox-image-169-super-hero-art) Image wird insbesondere empfohlen, wenn Sie einschließen möchten [video Nachspänne](#trailers) in Ihrer Store auflisten, wenn Sie sie nicht einschließen Ihre Nachspänne nicht am Anfang Ihres Angebots angezeigt.


### <a name="trailers"></a>Trailer

Trailer sind kurzen Videos, die Kunden die Anzeige Ihres Produkts in Aktion ermöglichen, damit sie seine Funktionen besser nachvollziehen können. Sie am oberen Rand der app-Store-Liste angezeigt werden (solange Sie enthalten eine [16:9 Superheld Art](#windows-10-and-xbox-image-169-super-hero-art) Image). 

Trailer sind mit [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming) codiert. Dabei wird die Qualität eines für Kunden angezeigten Videostreams in Echtzeit basierend auf der verfügbaren Bandbreite und CPU-Ressourcen angepasst.

> [!NOTE]
> Trailer werden nur für Kunden unter Windows 10, Version 1607 oder höher (einschließlich Xbox), angezeigt.

### <a name="upload-trailers"></a>Trailer hochladen

Sie können bis zu 15 Trailer zu Ihrem Store-Eintrag hinzufügen. Achten Sie darauf, dass sie alle unten aufgeführten [Anforderungen](#trailer-requirements) erfüllen.

Sie müssen eine Videodatei (MP4 oder MOV), eine Miniaturansicht und einen Titel für jeden bereitgestellten Trailer hochladen.

> [!IMPORTANT]
> Beim Nachspänne verwenden zu können, müssen Sie auch angeben einer [16:9 Superheld Art](#windows-10-and-xbox-image-169-super-hero-art) image Abschnitt in der Reihenfolge für Ihre Anhänger am Anfang Ihrer Store aufgelistet angezeigt. Dieses Bild wird angezeigt, wenn Ihre Trailer beendet sind.

Befolgen Sie folgende Empfehlungen, damit Ihre Trailer effektiv sind:
- Trailer sollten eine gute Qualität und eine minimale Länge aufweisen (60 Sekunden oder weniger als 2 GB wird empfohlen). 
- Verwenden Sie eine andere Miniaturansicht für jeden Trailer, sodass Kunden wissen, dass sie eindeutig sind.
- Da bei einigen Store-Layouts der obere und untere Rand der Trailer leicht abgeschnitten sein kann, stellen Sie sicher, dass wichtige Informationen in der Mitte des Bildschirms angezeigt werden.
- Bildfrequenz und Auflösung sollten dem Quellmaterial entsprechen. Beispielsweise sollten mit 720p60 aufgenommene Inhalte codiert und mit 720p60 hochgeladen werden. 

Sie müssen auch die unten genannten Anforderungen erfüllen.

**So fügen Sie zu Ihrem Angebot Nachspänne hinzu:**
1. Laden Sie die **Videodatei** Ihres Trailers im angegebenen Feld hoch. Ein Dropdownfeld wird auch angezeigt für den Fall, dass Sie einen Trailer wiederverwenden möchten, den Sie bereits hochgeladen haben (z. B. für einen Store-Eintrag in einer anderen Sprache).
2. Nachdem Sie den Trailer hochgeladen haben, müssen Sie ein **Miniaturbild** dafür hochladen. Dies muss eine PNG-Datei mit 1920 x 1080 Pixeln sein. In der Regel handelt es sich um ein Standbild aus dem Trailer.
3. Klicken Sie auf das Stiftsymbol, um einen **Titel** für Ihren Trailer hinzuzufügen (maximal 255 Zeichen).
4. Wenn Sie weitere Trailer zum Eintrag hinzufügen möchten, klicken Sie auf **Add trailer**, und wiederholen Sie die oben aufgeführten Schritte.

> [!TIP]
> Wenn Sie Store-Einträge in mehreren Sprachen erstellt haben, können Sie **Aus vorhandenem Trailer auswählen** auswählen, um den Trailer wiederzuverwenden, den Sie bereits hochgeladen haben. Sie müssen diese nicht einzeln für jede Sprache hochladen.

Um einen Trailer aus einem Eintrag zu entfernen, klicken Sie auf das **X** neben dem Dateinamen. Sie können auswählen, ob es in dem Sie arbeiten werden nur die aktuelle Liste Store aufheben, oder Entfernen von allen Ihres Produkts Store-Angebote (in jeder Sprache).


### <a name="trailer-requirements"></a>Traileranforderungen

Wenn Sie Ihre Trailer bereitstellen, müssen Sie die folgenden Anforderungen erfüllen:

- Das Videoformat muss MOV oder MP4 sein. 
- Die Videodauer sollte nicht 60 Sekunden überschreiten.
- Die Dateigröße des Trailers sollte 2 GB nicht überschreiten. 
- Die Videoauflösung muss 1920 x 1080 Pixel oder 3840 x 2160 Pixel sein.
- Die Miniaturansicht muss eine PNG-Datei mit einer Auflösung von entweder 1920 x 1080 Pixeln oder 3840 x 2160 Pixeln sein.
- Der Titel darf 255 Zeichen nicht überschreiten. 
- Schließen Sie keine Altersfreigaben in Ihrem Trailer ein.

Wie die anderen Felder auf der Seite des Store-Eintrags müssen Trailer die Zertifizierung bestehen, bevor Sie sie im Microsoft Store veröffentlichen können. Achten Sie darauf, dass Ihre Trailer die [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies) einhalten.

Es gibt weitere Anforderungen je nach Typ der Datei.

#### <a name="mov"></a>MOV

<table>
<tr>
<td>

**Video:**

<ul>
<li>1080p ProRes (bei Bedarf HQ)</li>
<li>Native Bildfrequenz; 29,97 BpS bevorzugt</li>
</ul>
</td>
<td>

**Audio:**

<ul>
<li>Stereo erforderlich</li>
<li>Empfohlener Audiopegel: -16 LKFS/LUFS</li>
</ul> 
</td>
</tr>
</table>

#### <a name="mp4"></a>MP4

<table>
<tr>
<td>

**Video:**

<ul>
<li>Codec: H.264</li>
<li>Progressiver Scan (kein Interlacing)</li>
<li>High Profile</li>
<li>2 aufeinanderfolgende B-Frames</li>
<li>Geschlossene GOP. GOP der Hälfte der Bildfrequenz</li>
<li>CABAC</li>
<li>50 MB/s </li>
<li>Farbraum: 4.2.0</li>
</ul>
</td>
<td>

**Audio:**

<ul>
<li>Codec: AAC-LC</li>
<li>Kanäle: Stereo oder Surround sound</li>
<li>Abtastrate: 48 kHz</li>
<li>Audio Bitrate: 384 KB/s für Stereosound, 512 KB/s für die Surroundsound</li>
</ul>
</td>
</tr>
</table>

Für H.264-Mezzanine-Dateien wird Folgendes empfohlen:
- Behälter: MP4
- Keine Bearbeitungslisten (oder AV-Synchronisierung geht womöglich verloren)
- moov atom am Anfang der Datei (schneller Start)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Windows 10 und Xbox-Bild (16:9 Besonderes Favoritenbild)

In der **Image für Windows 10 und Xbox** Abschnitt der **16:9 Superheld Art (Pixel 1920 × 1080 oder 3840 × 2160)** Image wird in verschiedenen Layouts in der Microsoft Store für alle Windows 10-Gerät-Typen (einschließlich Xbox) verwendet. Es wird empfohlen, dieses Image bereitzustellen, unabhängig davon, auf welche Betriebssystemversionen oder Gerättypen die App ausgerichtet ist.

Dieses Bild ist *erforderlich* für die ordnungsgemäße Anzeige, wenn Ihr Eintrag [Videotrailer](#trailers) enthält. Für Kunden mit Windows 10, Version 1607 oder höher (einschließlich Xbox), wird es als Hauptbild auf Ihrem Store-Eintrag verwendet (oder es wird angezeigt, nachdem alle Trailer beendet sind). Es kann auch verwendet werden, um Ihre App in Werbelayouts im Store zu präsentieren. Beachten Sie, dass dieses Bild weder den Produkttitel noch anderen Text enthalten darf.

Im Folgenden finden Sie einige Tipps, die Sie beim Entwerfen Ihres Bilds beachten sollten:

- Das Bild muss eine PNG-Datei mit 1920 x 1080 Pixel oder 3840 x 2160 Pixel sein.
- Wählen Sie ein dynamisches Bild aus, das mit der App zusammenhängt, um den Wiedererkennungswert sowie die Differenzierung zu fördern. Vermeiden Sie Stockfotografien oder generische visuelle Elemente.
- Fügen Sie keinen Text in das Bild ein.
- Platzieren Sie keine wichtigen visuellen Elemente in das untere Drittel des Bilds (da in einigen Layouts ein Farbverlauf über diesen Bereich angewendet wird).
- Platzieren Sie die wichtigsten Details in die Mitte des Bilds (da in einigen Layouts das Bild zugeschnitten wird).
- Minimieren Sie den leeren Bereich.
- Vermeiden Sie das Anzeigen der Benutzeroberfläche Ihrer App, und verwenden Sie keine Bilder für bestimmte Geräte.
- Vermeiden Sie politische und nationale Designs, Flaggen oder religiöse Symbole.
- Fügen Sie keine Bilder von taktlosen Gesten, Nacktheit, Glücksspiel, Währung, Drogen, Tabakprodukte oder Alkohol hinzu.
- Verwenden Sie keine Bilder mit Waffen, die auf den Benutzer gerichtet sind, oder übermäßiger Gewalt und Blut.

Durch das Bereitstellen des Bilds können wir es für Vermarktungsmöglichkeiten in Betracht ziehen. Dies garantiert nicht, dass die App präsentiert wird. Weitere Informationen finden Sie unter [Einfaches Bewerben Ihrer App](make-your-app-easier-to-promote.md).


### <a name="xbox-images"></a>Xbox-Bilder

Diese Bilder werden für die ordnungsgemäße Anzeige empfohlen, wenn Sie Ihre App auf Xbox veröffentlichen. 

Es gibt 3 verschiedene Größen, die Sie hochladen können:
- **Mit dem Branding des Schlüssels Kunst, 584 x 800 Pixel**: Muss den Titel des Produkts enthalten. Für dieses Image ist eine Brandlingleiste erforderlich. Der Titel und alle Hauptbilder müssen im oberen Dreiviertel des Bilds angezeigt werden, da die Textüberlagerung auf dem unteren Viertel angezeigt werden.
- **Mit dem Titel Hero Kunst, 1920 x 1080 Pixel**: Muss den Titel des Produkts enthalten. Der Titel und alle Hauptbilder müssen im oberen Dreiviertel des Bilds angezeigt werden, da die Textüberlagerung auf dem unteren Viertel angezeigt werden.
- **Ausgewählte Aktionspreis Quadrat Kunst, 1080 x 1080 Pixel**: Muss *nicht* des Produkts Titel enthalten.

> [!NOTE]
> Sie müssen ebenfalls für die besten Anzeige auf Xbox ein **9:16 (720 x 1080 oder 1440 x 2160 Pixel)** Bild im Abschnitt [Store-Logos](#store-logos) angeben.


### <a name="holographic-image"></a>Hologrammbild

Die Bildgröße **2:1 (2400 x 1200)** wird nur verwendet, wenn Ihre App die Hologramm-Gerätefamilie unterstützt. Wenn dies der Fall ist, wird empfohlen, dieses Bild zu verwenden.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Bilder für Windows 8.x bzw. Windows Phone 8.x 

Wenn Ihre app zuvor übermittelter frühere Betriebssystemversionen unterstützt (Windows 8.x bzw. Windows Phone 8.x), diese Images müssen angegeben werden, damit wir unsere zu berücksichtigen, mit der Ihre app in Werbe-Layouts (obwohl sie nicht garantieren, dass Ihre app präsentiert werden). Wenn Ihre app diese früheren Betriebssystemversionen nicht unterstützt, überspringen Sie diesen Abschnitt. (Dieser Abschnitt hieß früher **Optionale Werbebilder**.)

**Für Windows Phone 8.1 und früher**zwei Bildgrößen in Werbe-Layouts verwendet werden können: **1000 x 800 Pixel (5:4)** und **358 x 358 Pixel (1:1)**. Wenn Ihre app für Windows Phone 8.1 oder früher ausgeführt wird, wird empfohlen, Bilder in beiden dieser Größen bereitstellen.  

> [!TIP]
> Stellen Sie ein 300 x 300 großes App-Kachelsymbol im Abschnitt [Store-Logos](#store-logos) für alle Übermittlungen bereit, die Windows Phone 8.1 oder früher unterstützen. Dadurch wird sichergestellt, dass Ihre App im Store nicht mit einem leeren Symbol angezeigt wird.  

**Für Windows 8.1 und frühere Versionen** verwenden manche Werbelayouts unter Umständen ein Bild der Größe **414 x 180** Pixel. Wenn Ihre app für Windows 8.1 oder früher ausgeführt wird, wird empfohlen, ein Bild in diese Sizen bereitstellen.




