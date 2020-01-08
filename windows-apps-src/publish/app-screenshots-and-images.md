---
Description: Sie können die Bildschirmfotos, Logos und andere Grafikobjekte (z. B. Trailer und Werbebilder) auswählen, die im Store-Eintrag Ihrer App enthalten sein sollen.
title: App-Screenshots, -Bilder und -Trailer
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 03/07/2019
ms.topic: article
keywords: Windows 10, UWP, Trailer, Video, Screenshot, Bild, Symbol, Store-Eintrag, Store-Eintragsbilder
ms.localizationpriority: medium
ms.openlocfilehash: 48a8566c80516588939dc0ef071c3da4b9232d64
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75684611"
---
# <a name="app-screenshots-images-and-trailers"></a>App-Screenshots, -Bilder und -Trailer

Durch aussagekräftige Bilder kann die Aufmerksamkeit potentieller Kunden im Store auf Ihre App gelenkt werden.

Sie können [Screenshots](#screenshots), [Logos](#store-logos), Nachspann und andere Kunst Ressourcen bereitstellen, die in die Store-Auflistung Ihrer APP [eingeschlossen werden sollen](#trailers). Einige dieser Elemente sind erforderlich, während andere optional sind, (auch wenn einige der optionalen Bilder wichtig für die beste Anzeige im Store sind).

Während der [App-Übermittlung](app-submissions.md) geben Sie diese Grafikobjekte im Schritt [Store-Einträge](create-app-store-listings.md) an. Beachten Sie, dass es vom Betriebssystem des Kunden und weiteren Faktoren abhängt, wie Bilder im Store angezeigt werden.

Der Speicher kann auch das Symbol Ihrer APP und andere Bilder verwenden, die Sie im Paket Ihrer APP enthalten. Führen Sie das [Zertifizierungskit für Windows-Apps](../debug-test-perf/windows-app-certification-kit.md) aus, um zu ermitteln, ob erforderliche Bilder fehlen, bevor Sie Ihre App übermitteln. Anleitungen und Empfehlungen zu diesen Images finden Sie unter [App-Symbole und-Logos](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Screenshots

Screenshots sind die Bilder Ihrer App, die Ihren Kunden im Store-Eintrag der App angezeigt werden.

Sie haben die Möglichkeit zum Bereitstellen von Bildschirmfotos für andere Gerätefamilien, die Ihre App unterstützt, damit die entsprechenden Bildschirmfotos angezeigt werden, wenn ein Kunde den Store-Eintrag Ihrer App auf diesem Gerätetyp anzeigt. 

Für die Übermittlung ist nur ein Screenshot (für jede Gerätefamilie) erforderlich. Sie können jedoch bis zu neun Desktop-Screenshots und bis zu acht Screenshots für die anderen Gerätefamilien bereitstellen. Wir empfehlen, mindestens 4 Screenshots für jede Gerätefamilie hinzuzufügen, die von Ihrer App unterstützt werden, damit Benutzer sehen, wie die App auf ihrem Gerätetyp aussieht. (Verwenden Sie keine Screenshots für Gerätefamilien, die Ihre App nicht unterstützt.) Beachten Sie, dass **Desktop**-Screenshots auch für Kunden auf Surface Hub-Geräten angezeigt werden.

> [!NOTE]
> Microsoft Visual Studio enthält ein [Tool zum Erstellen von Screenshots](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Jeder Screenshot muss einer PNG-Datei im Quer- oder Hochformat entsprechen, und die Datei darf nicht größer als 50 MB sein.

Die Größenanforderungen hängen von der Gerätefamilie ab:
- Desktopgerät: 1366 x 768 Pixel oder größer. Unterstützt 4K-Bilder (3840x2160). (Wird ebenfalls den Kunden auf Surface Hub-Geräten angezeigt.)
- Mobilgeräte: Bilder müssen folgende Größe haben: 1080x1920, 1920x1080, 768x1280, 1280x768, 720x1280 und 1280x720, 800x480 oder 480x800 Pixel.
- Xbox: 3480 x 2160 Pixel oder kleiner. Unterstützt 4K-Bilder (3840x2160).
- Geräte für Hologramme: 1268 x 720 Pixel oder größer. Unterstützt 4K-Bilder (3840x2160).

Beachten Sie für die optimale Anzeige die folgenden Richtlinien beim Erstellen Ihrer Bildschirmfotos:
- Platzieren Sie wichtige visuelle Elemente und Text in den oberen drei Vierteln des Bilds. Textüberlagerungen können im unteren Viertel angezeigt werden. 
- Fügen Sie Ihren Screenshots keine zusätzlichen Logos, Symbole oder Marketingnachrichten hinzu.
- Verwenden Sie nicht sehr helle oder dunkle Farben oder Streifen mit starkem Kontrast, die die Lesbarkeit von Textüberlagerungen beeinträchtigen können.

Sie können auch eine Kurzbeschreibung von maximal 200 Zeichen für die einzelnen Screenshots angeben.

> [!TIP]
> Bildschirmfotos werden in Ihrem Eintrag der Reihenfolge nach angezeigt. Nachdem Sie Ihre Bildschirmfotos hochgeladen haben, können Sie sie ziehen und ablegen, um sie neu anzuordnen. 

Hinweis: Wenn Sie Store-Einträge für [mehrere Sprachen](supported-languages.md) erstellen, erhalten Sie für jede Sprache eine Seite vom Typ **Store-Eintrag**. Sie müssen Bilder für jede Sprache separat hochladen (auch wenn Sie dieselben Bilder verwenden), einschließlich Beschriftungen, die für die einzelnen Sprachen verwendet werden. (Wenn Sie in einer Vielzahl von Sprachen Speicher Listen haben, ist es möglicherweise einfacher, Sie zu aktualisieren, indem Sie [Ihre Auflistungs Daten exportieren und offline arbeiten](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Store-Logos

Sie können Store-Logos hochladen, um eine angepasstere Darstellung im Store zu erstellen. Es wird empfohlen, dass Sie diese Bilder für eine optimale Anzeige im Store-Eintrag für alle Geräte und Betriebssystemversionen bereitstellen, die Ihre App unterstützt. Wenn Ihre App für Kunden auf Xbox verfügbar ist, sind einige dieser Bilder erforderlich.

Sie können diese Bilder als PNG-Dateien (maximal 50 MB) in drei Größen bereitstellen, die jeweils den folgenden Richtlinien entsprechen sollten.

### <a name="916-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>9:16 – Plakate (720x1080 oder 1440x2160 Pixel)

Diese Größe wird als Logo in Store-Einträgen für Kunden unter Windows 10 und auf Xbox-Geräten verwendet, daher wird **dringend empfohlen**, dieses Bild für die ordnungsgemäße Anzeige bereitzustellen. Ihre Auflistung wird möglicherweise nicht angezeigt, wenn Sie Sie nicht einschließen, und Sie ist nicht mit anderen Listen konsistent, die Kunden beim Durchsuchen des Stores sehen. Dieses Bild kann ebenfalls in den Suchergebnissen oder in speziell zusammengestellten Sammlungen verwendet werden.

Dieses Bild sollte den Namen Ihrer App enthalten, und Text auf dem Bild sollte die Lesbarkeitsanforderungen (Kontrastverhältnis von 4,5:1) erfüllen. Beachten Sie, dass auf dem unteren Viertel des Bilds Textüberlagerungen angezeigt werden. Stellen Sie sicher, dass Sie dort keinen Text oder das Hauptbild einfügen.

> [!NOTE]
> Wenn Ihre App für Kunden auf Xbox verfügbar ist, ist dieses Bild **erforderlich** und muss den Produkttitel enthalten. Der Titel muss im oberen Dreiviertel des Bilds angezeigt werden, da Textüberlagerungen auf den unteren Viertel des Bilds angezeigt werden.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 – Verpackungsillustration (1080 x 1080 or 2160 x 2160 Pixel)

Dieses Bild kann auf verschiedenen Store-Seiten für Windows 10 angezeigt werden (einschließlich Xbox), und wenn Sie keine **9:16 Illustrationen** angeben, wird dieses als zentrales Logo verwendet. Dieses Bild muss auch den Namen Ihrer App enthalten. Beachten Sie, dass auf dem unteren Viertel des Bilds Textüberlagerungen angezeigt werden und fügen Sie dort keinen Text oder das Hauptbild ein. Stellen Sie sicher, dass Sie den Namen Ihrer App im Bild einfügen. 

> [!NOTE]
> Wenn Ihre App für Kunden auf Xbox verfügbar ist, ist dieses Bild **erforderlich** und muss den Produkttitel enthalten. Der Titel muss im oberen Dreiviertel des Bilds angezeigt werden, da Textüberlagerungen auf den unteren Viertel des Bilds angezeigt werden.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 Symbol für App-Kachel (300 x 300 Pixel)

Dieses Bild ist für die ordnungsgemäße Anzeige auf Windows Phone 8.1 und früheren Versionen erforderlich. Wenn Ihre zuvor veröffentlichte APP Windows Phone 8,1 oder früher unterstützt, und Sie dieses Image nicht bereitstellen, wird für diese Kunden ein leeres Symbol mit der Liste Ihrer Apps angezeigt. (Dies gilt auch für Kunden unter Windows 10, wenn Ihre APP nur Pakete für Windows Phone 8,1 oder früher verwendet.)

Wenn Ihre Übermittlung *nur* UWP-Pakete umfasst, müssen Sie dieses Image nicht bereitstellen (es sei denn, Sie aktivieren das Kontrollkästchen **für Kunden unter Windows 10 und Xbox, zeigen Sie hochgeladene Logo Bilder anstelle der Bilder aus den Paketen**an, wie im nächsten Abschnitt beschrieben).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Nur hochgeladene Logo Bilder im Store anzeigen

Sie können verhindern, dass der Speicher die Logo Bilder in den Paketen Ihrer APP verwendet, wenn Sie Ihre Liste für Kunden unter Windows 10 (einschließlich Xbox) anzeigen, und Sie müssen stattdessen nur Images verwenden, die Sie hochladen. Dies bietet Ihnen mehr Kontrolle über die Darstellung Ihrer App in verschiedenen Anzeigen im Store für Kunden auf Windows 10 (einschließlich Xbox). (Wenn Ihre zuvor veröffentlichte App frühere Betriebssystemversionen unterstützt, sehen diese Kunden möglicherweise weiterhin Bilder aus ihren Paketen.)

Damit der Speicher nur die von Ihnen hochgeladenen Images nutzt (für Kunden unter Windows 10, einschließlich Xbox) und keine Images aus ihren Paketen verwenden, aktivieren Sie das Kontrollkästchen **für Kunden unter Windows 10 und Xbox, und zeigen Sie hochgeladene Logo Bilder anstelle der Bilder aus meinen Paketen**an.

Durch Aktivieren dieses Kontrollkästchens wird ein neuer Abschnitt namens **Bilder für die Store-Anzeige** angezeigt. Hier können Sie drei Bilder hochladen, einschließlich der **1:1-App-Kachel Symbol (300 x 300 Pixel)** . (wenn Sie das Kontrollkästchen aktivieren, wird das Feld, das das Bild enthält, in diesen Abschnitt verschoben). Es wird empfohlen, alle drei Bildgrößen bereitzustellen, wenn Sie diese Option verwenden: 300 x 300, 150 x 150 und 71 x 71 Pixel. Es ist jedoch nur die Größe 300 x 300 erforderlich.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Nachspann und zusätzliche Assets

In diesem Abschnitt können Sie Grafiken bereitstellen, damit Sie Ihr Produkt im Store effektiver präsentieren können. Wir empfehlen die Bereitstellung dieser Bilder, um einen einladenden Store-Eintrag zu erstellen.

> [!TIP]
> Das [16:9 Superhero Art](#windows-10-and-xbox-image-169-super-hero-art) -Image wird besonders empfohlen, wenn Sie [Video](#trailers) Nachspann in Ihre Store-Liste einschließen möchten. Wenn Sie ihn nicht einschließen, werden Ihre nach spannenden nicht oben in der Liste angezeigt.


### <a name="trailers"></a>Trailer

Trailer sind kurzen Videos, die Kunden die Anzeige Ihres Produkts in Aktion ermöglichen, damit sie seine Funktionen besser nachvollziehen können. Sie werden am Anfang der Store-Auflistung Ihrer App angezeigt (sofern Sie ein [16:9 Superhero-Grafik](#windows-10-and-xbox-image-169-super-hero-art) enthalten). 

Trailer sind mit [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming) codiert. Dabei wird die Qualität eines für Kunden angezeigten Videostreams in Echtzeit basierend auf der verfügbaren Bandbreite und CPU-Ressourcen angepasst.

> [!NOTE]
> Trailer werden nur für Kunden unter Windows 10, Version 1607 oder höher (einschließlich Xbox), angezeigt.

### <a name="upload-trailers"></a>Trailer hochladen

Sie können bis zu 15 Trailer zu Ihrem Store-Eintrag hinzufügen. Achten Sie darauf, dass sie alle unten aufgeführten [Anforderungen](#trailer-requirements) erfüllen.

Sie müssen eine Videodatei (MP4 oder MOV), eine Miniaturansicht und einen Titel für jeden bereitgestellten Trailer hochladen.

> [!IMPORTANT]
> Bei der Verwendung von Nachspann müssen Sie auch einen [16:9 Superhero](#windows-10-and-xbox-image-169-super-hero-art) -Image Abschnitt bereitstellen, damit Ihre Nachspann oben in der Store-Liste angezeigt werden. Dieses Bild wird angezeigt, wenn Ihre Trailer beendet sind.

Befolgen Sie folgende Empfehlungen, damit Ihre Trailer effektiv sind:
- Trailer sollten eine gute Qualität und eine minimale Länge aufweisen (60 Sekunden oder weniger als 2 GB wird empfohlen). 
- Verwenden Sie eine andere Miniaturansicht für jeden Trailer, sodass Kunden wissen, dass sie eindeutig sind.
- Da bei einigen Store-Layouts der obere und untere Rand der Trailer leicht abgeschnitten sein kann, stellen Sie sicher, dass wichtige Informationen in der Mitte des Bildschirms angezeigt werden.
- Bildfrequenz und Auflösung sollten dem Quellmaterial entsprechen. Beispielsweise sollten mit 720p60 aufgenommene Inhalte codiert und mit 720p60 hochgeladen werden. 

Sie müssen auch die unten genannten Anforderungen erfüllen.

**So fügen Sie Ihrer Auflistung einen Spann hinzu:**
1. Laden Sie die **Videodatei** Ihres Trailers im angegebenen Feld hoch. Ein Dropdownfeld wird auch angezeigt für den Fall, dass Sie einen Trailer wiederverwenden möchten, den Sie bereits hochgeladen haben (z. B. für einen Store-Eintrag in einer anderen Sprache).
2. Nachdem Sie den Trailer hochgeladen haben, müssen Sie ein **Miniaturbild** dafür hochladen. Dies muss eine PNG-Datei mit 1920 x 1080 Pixeln sein. In der Regel handelt es sich um ein Standbild aus dem Trailer.
3. Klicken Sie auf das Stiftsymbol, um einen **Titel** für Ihren Trailer hinzuzufügen (maximal 255 Zeichen).
4. Wenn Sie weitere Trailer zum Eintrag hinzufügen möchten, klicken Sie auf **Add trailer**, und wiederholen Sie die oben aufgeführten Schritte.

> [!TIP]
> Wenn Sie Store-Einträge in mehreren Sprachen erstellt haben, können Sie **Aus vorhandenem Trailer auswählen** auswählen, um den Trailer wiederzuverwenden, den Sie bereits hochgeladen haben. Sie müssen diese nicht einzeln für jede Sprache hochladen.

Um einen Trailer aus einem Eintrag zu entfernen, klicken Sie auf das **X** neben dem Dateinamen. Sie können auswählen, ob Sie es nur aus der aktuellen Speicher Liste, in der Sie arbeiten, entfernen möchten, oder ob Sie Sie aus allen Speicher Listen Ihres Produkts (in jeder Sprache) entfernen möchten.


### <a name="trailer-requirements"></a>Traileranforderungen

Wenn Sie Ihre Trailer bereitstellen, müssen Sie die folgenden Anforderungen erfüllen:

- Das Videoformat muss MOV oder MP4 sein. Wenn Sie 4K-Videos hochladen, wird nur MP4 unterstützt.
- Die Videodauer sollte nicht 60 Sekunden überschreiten.
- Die Dateigröße des Trailers sollte 2 GB nicht überschreiten.
- Die Videoauflösung muss 1920 x 1080 Pixel oder 3840 x 2160 Pixel sein.
- Die Miniaturansicht muss eine PNG-Datei mit einer Auflösung von entweder 1920 x 1080 Pixeln oder 3840 x 2160 Pixeln sein.
- Der Titel darf 255 Zeichen nicht überschreiten.
- Schließen Sie keine Altersfreigaben in Ihrem Trailer ein.

> [!WARNING]
> Eine Ausnahme von der Anforderung, Alters Bewertungen in ihren Nachspann einzuschließen, gilt **nur** für die **auf der Produktseite**angezeigten **Microsoft Store** . Alle Nachrichten außerhalb von Partner Center, die nicht ausschließlich für die Anzeige auf der Produktseite des Microsoft Store vorgesehen sind, **müssen** bei Bedarf eingebettete Bewertungsinformationen entsprechend den entsprechenden Richtlinien der Bewertungsbehörde anzeigen.  

Wie die anderen Felder auf der Seite des Store-Eintrags müssen Trailer die Zertifizierung bestehen, bevor Sie sie im Microsoft Store veröffentlichen können. Achten Sie darauf, dass Ihre Trailer die [Microsoft Store-Richtlinien](store-policies.md) einhalten.

Es gibt weitere Anforderungen je nach Typ der Datei.

#### <a name="mov"></a>MOV

| Video | Audio | 
| --- | --- | 
| <ul><li>1080p ProRes (bei Bedarf HQ)</li><li>Native Bildfrequenz; 29,97 BpS bevorzugt</li></ul> | <ul><li>Stereo erforderlich</li><li>Empfohlener Audiopegel: -16 LKFS/LUFS</li></ul> |


#### <a name="mp4"></a>MP4

| Video | Audio |
| --- | --- |
| <ul><li>Codec: [H. 264](https://docs.microsoft.com/windows/desktop/DirectShow/h-264-video-types) (AVC1)  </li><li>Progressiver Scan (kein Interlacing)</li><li>High Profile</li><li>2 aufeinanderfolgende B-Frames</li><li>Geschlossene GOP. GOP der Hälfte der Bildfrequenz</li><li>CABAC</li><li>50 MB/s </li><li>Farbraum: 4.2.0</li></ul> | <ul><li>Codec: AAC-LC</li><li>Kanäle: Stereo oder Surround-Sound</li><li>Samplingrate: 48 KHz</li><li>Audiobitrate: 384 KB/s für Stereo, 512 KB/s für Surround-Sound</li></ul> |

> [!WARNING]
> Kunden hören möglicherweise keine Audiodaten für MP4-Dateien, die mit anderen Codecs als AVC1 codiert sind.

Für H.264-Mezzanine-Dateien wird Folgendes empfohlen:
- Container: MP4
- Keine Bearbeitungslisten (oder AV-Synchronisierung geht womöglich verloren)
- moov atom am Anfang der Datei (schneller Start)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Windows 10 und Xbox-Bild (16:9 Besonderes Favoritenbild)

Im Abschnitt **Windows 10 und Xbox Image** wird das Image **16:9 Superhero Art (1920 x 1080 oder 3840 x 2160 Pixel)** in verschiedenen Layouts des Microsoft Store auf allen Windows 10-Gerätetypen (einschließlich Xbox) verwendet. Es wird empfohlen, dieses Image bereitzustellen, unabhängig davon, auf welche Betriebssystemversionen oder Gerättypen die App ausgerichtet ist.

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
- Verwenden Sie keine Bilder mit Waffen, die auf den Benutzer gerichtet sind, oder übermäßige Gewalt und Blut.

Durch das Bereitstellen des Bilds können wir es für Vermarktungsmöglichkeiten in Betracht ziehen. Dies garantiert nicht, dass die App präsentiert wird. Weitere Informationen finden Sie unter [Einfaches Bewerben Ihrer App](make-your-app-easier-to-promote.md).


### <a name="xbox-images"></a>Xbox-Bilder

Diese Bilder werden für die ordnungsgemäße Anzeige empfohlen, wenn Sie Ihre App auf Xbox veröffentlichen. 

Es gibt 3 verschiedene Größen, die Sie hochladen können:
- **Grafik mit Branding, 584 x 800 Pixel**: Muss den Titel des Produkts enthalten. Für dieses Image ist eine Brandlingleiste erforderlich. Der Titel und alle Hauptbilder müssen im oberen Dreiviertel des Bilds angezeigt werden, da die Textüberlagerung auf dem unteren Viertel angezeigt werden.
- **Favoritenbild mit Titel, 1920 x 1080 Pixel**: Muss den Titel des Produkts enthalten. Der Titel und alle Hauptbilder müssen im oberen Dreiviertel des Bilds angezeigt werden, da die Textüberlagerung auf dem unteren Viertel angezeigt werden.
- **Ausgewähltes quadratisches Werbebild, 1080 x 1080 Pixel**: Muss *nicht* den Produkttitel enthalten.

> [!NOTE]
> Sie müssen ebenfalls für die besten Anzeige auf Xbox ein **9:16 (720 x 1080 oder 1440 x 2160 Pixel)** Bild im Abschnitt [Store-Logos](#store-logos) angeben.


### <a name="holographic-image"></a>Hologrammbild

Die Bildgröße **2:1 (2400 x 1200)** wird nur verwendet, wenn Ihre App die Hologramm-Gerätefamilie unterstützt. Wenn dies der Fall ist, wird empfohlen, dieses Bild zu verwenden.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Bilder für Windows 8.x bzw. Windows Phone 8.x 

Wenn Ihre zuvor übermittelte App frühere Betriebssystemversionen unterstützt (Windows 8. x und/oder Windows Phone 8. x), müssen diese Images bereitgestellt werden, damit wir Ihre APP in Werbe Layouts in Erwägung gezogen haben (obwohl Sie nicht garantieren, dass Ihre APP angezeigt wird). Wenn Ihre APP diese früheren Betriebssystemversionen nicht unterstützt, überspringen Sie diesen Abschnitt. (Dieser Abschnitt hieß früher **Optionale Werbebilder**.)

**Für Windows Phone 8.1 und frühere Versionen** können zwei Bildgrößen in Werbelayouts verwendet werden: **1000 x 800 Pixel (5:4)** und **358 x 358 Pixel (1:1)** . Wenn Ihre APP unter Windows Phone 8,1 oder einer früheren Version ausgeführt wird, empfiehlt es sich, in beiden Größen Images bereitzustellen.  

> [!TIP]
> Stellen Sie ein 300 x 300 großes App-Kachelsymbol im Abschnitt [Store-Logos](#store-logos) für alle Übermittlungen bereit, die Windows Phone 8.1 oder früher unterstützen. Dadurch wird sichergestellt, dass Ihre App im Store nicht mit einem leeren Symbol angezeigt wird.  

**Für Windows 8.1 und frühere Versionen** verwenden manche Werbelayouts unter Umständen ein Bild der Größe **414 x 180** Pixel. Wenn Ihre APP unter Windows 8.1 oder früher ausgeführt wird, empfiehlt es sich, ein Image in dieser Größe bereitzustellen.




