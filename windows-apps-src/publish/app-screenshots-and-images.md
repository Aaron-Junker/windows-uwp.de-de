---
description: Sie können die Screenshots, Logos und anderen Kunst Ressourcen (z. b. "Nachspann" und "Werbebilder") auswählen, die in die Store-Auflistung Ihrer APP eingeschlossen werden sollen.
title: App-Screenshots, -Bilder und -Trailer
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 09/24/2020
ms.topic: article
keywords: Windows 10, UWP, Nachspann, Video, Screenshot, Bild, Symbol, Store-Auflistung, Listen Bilder speichern
ms.localizationpriority: medium
ms.openlocfilehash: b9af3ab765b218d933a4382f7b241704c81af2f1
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031303"
---
# <a name="app-screenshots-images-and-trailers"></a>App-Screenshots, -Bilder und -Trailer

Gut entworfene Images sind eine der wichtigsten Möglichkeiten, Ihre APP für potenzielle Kunden im Geschäft darzustellen.

Sie können [Screenshots](#screenshots), [Logos](#store-logos), Nachspann und andere Kunst Ressourcen bereitstellen, die in die Store-Auflistung Ihrer APP [eingeschlossen werden sollen](#trailers). Einige davon sind erforderlich, und einige sind optional (obwohl einige der optionalen Images wichtig sind, um die beste Anzeige zu speichern).

Während der [App-Übermittlung](app-submissions.md)stellen Sie diese Kunst Ressourcen im Schritt [Store-Auflistungen](create-app-store-listings.md) bereit. Beachten Sie, dass die Images, die im Speicher verwendet werden, und die Art und Weise, wie Sie angezeigt werden, je nach Betriebssystem des Kunden und anderen Faktoren variieren können.

Der Speicher kann auch das Symbol Ihrer APP und andere Bilder verwenden, die Sie im Paket Ihrer APP enthalten. Führen Sie das [zertifizierungskit für Windows-apps](../debug-test-perf/windows-app-certification-kit.md) aus, um zu ermitteln, ob erforderliche Images fehlen, bevor Sie Ihre APP einreichen. Anleitungen und Empfehlungen zu diesen Images finden Sie unter [App-Symbole und-Logos](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Screenshots

Screenshots sind die Bilder Ihrer App, die Ihren Kunden im Store-Eintrag der App angezeigt werden.

Sie haben die Möglichkeit, Screenshots für die verschiedenen Gerätefamilien bereitzustellen, die von ihrer App unterstützt werden, damit die entsprechenden Screenshots angezeigt werden, wenn ein Kunde die Store-Auflistung Ihrer APP auf dem betreffenden Gerätetyp anzeigt. 

Für ihre Übermittlung ist nur ein Screenshot (für eine beliebige Gerätefamilie) erforderlich, aber Sie können auch mehrere bereitstellen. bis zu 9 Desktop Screenshots und bis zu 8 Screenshots für die anderen Gerätefamilien. Es wird empfohlen, für jede Gerätefamilie, die ihre App unterstützt, mindestens vier Screenshots bereitzustellen, damit Benutzer sehen können, wie die APP Ihren Gerätetyp untersucht. (Schließen Sie keine Screenshots für Gerätefamilien ein, die Ihre APP nicht unterstützt.) Beachten Sie, dass **Desktop-** Screenshots auch Kunden auf Surface Hub Geräten angezeigt werden.

> [!NOTE]
> Microsoft Visual Studio bietet ein [Tool, das Sie beim Erfassen von Screenshots unterstützt](/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Jeder Screenshot muss eine PNG-Datei in der Querformat-oder Hochformat-Ausrichtung sein, und die Dateigröße darf nicht größer als 50 MB sein.

Die Größenanforderungen hängen von der Gerätefamilie ab:
- Desktop: 1366 x 768 Pixel oder größer. Unterstützt 4K-Bilder (3840 x 2160). (Wird auch Kunden auf Surface Hub Geräten angezeigt.)
- Mobile: Bilder müssen eine der folgenden sein: 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 oder 480 x 800 Pixel.
- Xbox: 3480 x 2160 Pixel oder kleiner. Unterstützt 4K-Bilder (3840 x 2160).
- Holographic: 1268 x 720 Pixel oder größer. Unterstützt 4K-Bilder (3840 x 2160).

Beachten Sie beim Erstellen Ihrer Screenshots die folgenden Richtlinien, um die beste Anzeige zu erhalten:
- Behalten Sie wichtige visuelle Elemente und Text im oberen 3/4 des Bilds bei. Text Überlagerungen werden möglicherweise unten 1/4 angezeigt. 
- Fügen Sie Ihren Screenshots keine weiteren Logos, Symbole oder Marketing Nachrichten hinzu.
- Verwenden Sie keine äußerst hellen oder dunklen Farben oder stark kontrastreiche Streifen, die die Lesbarkeit von Text Überlagerungen beeinträchtigen können.

Sie können auch eine kurze Beschriftung bereitstellen, die jeden Screenshot in 200 Zeichen oder weniger beschreibt.

> [!TIP]
> Screenshots werden in der Reihenfolge in der Liste angezeigt. Nachdem Sie Ihre Screenshots hochgeladen haben, können Sie Sie per Drag & amp; Drop neu anordnen. 

Beachten Sie, dass Sie, wenn Sie Store-Listen für [mehrere Sprachen](supported-languages.md)erstellen, jeweils eine **Store-Listen** Seite haben. Sie müssen Bilder für jede Sprache separat hochladen (auch wenn Sie dieselben Bilder verwenden), einschließlich Beschriftungen, die für die einzelnen Sprachen verwendet werden. (Wenn Sie in einer Vielzahl von Sprachen Speicher Listen haben, ist es möglicherweise einfacher, Sie zu aktualisieren, indem Sie [Ihre Auflistungs Daten exportieren und offline arbeiten](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Store-Logos

Sie können Store-Logos hochladen, um eine besser angepasste Anzeige im Speicher zu erstellen. Es wird empfohlen, diese Images bereitzustellen, damit Ihre Store-Auflistung optimal auf allen Geräten und Betriebssystemversionen angezeigt wird, die von ihrer App unterstützt werden. Beachten Sie, dass einige dieser Images erforderlich sind, wenn Ihre APP für Kunden auf der Xbox verfügbar ist.

Sie können diese Images als PNG-Dateien (nicht größer als 50 MB) bereitstellen, von denen jeder die folgenden Richtlinien befolgt.

### <a name="23-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>2:3 Poster-Grafik (720 x 1080 oder 1440 x 2160 Pixel)

Dies wird als hauptlogobild für Kunden auf Windows 10-und Xbox-Geräten verwendet. Daher wird **dringend empfohlen** , dieses Image bereitzustellen, um eine ordnungsgemäße Anzeige sicherzustellen. Ihre Auflistung wird möglicherweise nicht angezeigt, wenn Sie Sie nicht einschließen, und Sie ist nicht mit anderen Listen konsistent, die Kunden beim Durchsuchen des Stores sehen. Dieses Image kann auch in Suchergebnissen oder in editorimage-Auflistungen verwendet werden.

Dieses Bild sollte den Namen Ihrer APP enthalten, und jeder Text im Image sollte den zugänglichen Anforderungen der besseren Lesbarkeit entsprechen (4,51-Kontrast Quote). Beachten Sie, dass Text Überlagerungen möglicherweise im unteren Quartal dieses Bilds angezeigt werden. Stellen Sie daher sicher, dass Sie Text oder Schlüsselbilder dort nicht einschließen.

> [!NOTE]
> Wenn Ihre APP für Kunden auf der Xbox verfügbar ist, ist dieses Image **erforderlich** und muss den Titel des Produkts enthalten. Der Titel muss in den ersten drei Quartalen des Bilds angezeigt werden, da Text Überlagerungen möglicherweise im unteren Quartal des Bilds angezeigt werden.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 Box-Grafik (1080 x 1080 oder 2160 x 2160 Pixel)

Dieses Bild wird möglicherweise auf verschiedenen Store-Seiten für Windows 10 (einschließlich Xbox) angezeigt, und wenn Sie das **2:3 Poster-Grafik** Bild nicht angeben, wird es als Hauptlogo verwendet. Dieses Bild sollte auch den Namen Ihrer APP enthalten. Text Überlagerungen können im unteren Quartal dieses Bilds angezeigt werden. Fügen Sie also keine Text-oder Schlüsselbilder ein. Stellen Sie sicher, dass Sie den Namen Ihrer APP in dieses Image einschließen. 

> [!NOTE]
> Wenn Ihre APP für Kunden auf der Xbox verfügbar ist, ist dieses Image **erforderlich** und muss den Titel des Produkts enthalten. Der Titel muss in den ersten drei Quartalen des Bilds angezeigt werden, da Text Überlagerungen möglicherweise im unteren Quartal des Bilds angezeigt werden.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 App-Kachel Symbol (300 x 300 Pixel)

Dieses Bild ist für die ordnungsgemäße Anzeige auf Windows Phone 8,1 und früher erforderlich. Wenn Ihre zuvor veröffentlichte APP Windows Phone 8,1 oder früher unterstützt, und Sie dieses Image nicht bereitstellen, wird für diese Kunden ein leeres Symbol mit der Liste Ihrer Apps angezeigt. (Dies gilt auch für Kunden unter Windows 10, wenn Ihre APP nur Pakete für Windows Phone 8,1 oder früher verwendet.)

Wenn Ihre Übermittlung *nur* UWP-Pakete umfasst, müssen Sie dieses Image nicht bereitstellen (es sei denn, Sie aktivieren das Kontrollkästchen  **für Kunden unter Windows 10 und Xbox, zeigen Sie hochgeladene Logo Bilder anstelle der Bilder aus den Paketen** an, wie im nächsten Abschnitt beschrieben).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Nur hochgeladene Logo Bilder im Store anzeigen

Sie können verhindern, dass der Speicher die Logo Bilder in den Paketen Ihrer APP verwendet, wenn Sie Ihre Liste für Kunden unter Windows 10 (einschließlich Xbox) anzeigen, und Sie müssen stattdessen nur Images verwenden, die Sie hochladen. Dadurch erhalten Sie mehr Kontrolle über das Erscheinungsbild Ihrer APP in verschiedenen anzeigen im gesamten Store für Kunden unter Windows 10 (einschließlich Xbox). (Wenn Ihre zuvor veröffentlichte App frühere Betriebssystemversionen unterstützt, sehen diese Kunden möglicherweise weiterhin Bilder aus ihren Paketen.)

Damit der Speicher nur die von Ihnen hochgeladenen Images nutzt (für Kunden unter Windows 10, einschließlich Xbox) und keine Images aus ihren Paketen verwenden, aktivieren Sie das Kontrollkästchen **für Kunden unter Windows 10 und Xbox, und zeigen Sie hochgeladene Logo Bilder anstelle der Bilder aus meinen Paketen** an.

Durch Aktivieren dieses Kontrollkästchens wird ein neuer Abschnitt namens **Bilder für die Store-Anzeige** angezeigt. Hier können Sie drei Bilder hochladen, einschließlich der **1:1-App-Kachel Symbol (300 x 300 Pixel)** . (wenn Sie das Kontrollkästchen aktivieren, wird das Feld, das das Bild enthält, in diesen Abschnitt verschoben). Es wird empfohlen, alle drei Bildgrößen bereitzustellen, wenn Sie diese Option verwenden: 300 x 300, 150 x 150 und 71 x 71 Pixel. Es ist jedoch nur die 300 x 300-Größe erforderlich.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Nachspann und zusätzliche Assets

In diesem Abschnitt können Sie Grafiken bereitstellen, die Ihnen helfen, Ihr Produkt effektiver im Store anzuzeigen. Es wird empfohlen, diese Images bereitzustellen, um eine einladendere Store-Auflistung zu erstellen.

> [!TIP]
> Das [16:9 Superhero Art](#windows-10-and-xbox-image-169-super-hero-art) -Image wird besonders empfohlen, wenn Sie [Video](#trailers) Nachspann in Ihre Store-Liste einschließen möchten. Wenn Sie ihn nicht einschließen, werden Ihre nach spannenden nicht oben in der Liste angezeigt.


### <a name="trailers"></a>Trailer

Nachspann sind kurze Videos, die Kunden eine Möglichkeit bieten, Ihr Produkt in Aktion zu sehen, damit Sie besser verstehen können, was es ist. Sie werden am Anfang der Store-Auflistung Ihrer App angezeigt (sofern Sie ein [16:9 Superhero-Grafik](#windows-10-and-xbox-image-169-super-hero-art) enthalten). 

Nachspann werden mit [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming)codiert, wodurch die Qualität eines Videodaten Stroms, der den Clients in Echtzeit bereitgestellt wird, basierend auf der verfügbaren Bandbreite und den CPU-Ressourcen angepasst wird

> [!NOTE]
> Nach Spann Informationen werden nur Kunden unter Windows 10, Version 1607 oder höher (einschließlich Xbox), angezeigt.

### <a name="upload-trailers"></a>Nachspann hochladen

Sie können Ihrer Store-Auflistung bis zu 15 Nachspann hinzufügen. Stellen Sie sicher, dass Sie alle unten aufgeführten [Anforderungen](#trailer-requirements) erfüllen.

Für jeden von Ihnen bereitgestellten Nachspann müssen Sie eine Videodatei (. MP4 oder. mov), ein Miniaturbild und einen Titel hochladen.

> [!IMPORTANT]
> Bei der Verwendung von Nachspann müssen Sie auch einen [16:9 Superhero](#windows-10-and-xbox-image-169-super-hero-art) -Image Abschnitt bereitstellen, damit Ihre Nachspann oben in der Store-Liste angezeigt werden. Dieses Bild wird angezeigt, nachdem Sie die Wiedergabe ihrer Nachspann abgeschlossen haben.

Befolgen Sie diese Empfehlungen, um die Nachrichten zu optimieren:
- Nachspann sollten eine gute Qualität und eine minimale Länge aufweisen (60 Sekunden oder weniger und weniger als 2 GB empfohlen). 
- Verwenden Sie für jeden Nachspann eine andere Miniaturansicht, damit Kunden wissen, dass Sie eindeutig sind.
- Da einige Speicher Layouts den oberen und unteren Rand Ihres Nachspann leicht anfertigen können, stellen Sie sicher, dass die Schlüsselinformationen in der Mitte des Bildschirms angezeigt werden.
- Die Framerate und die Auflösung müssen mit dem Quellmaterial identisch sein. So sollten beispielsweise Inhalts aufschüsse bei 720p60 codiert und bei 720p60 hochgeladen werden. 

Außerdem müssen Sie die unten aufgeführten Anforderungen befolgen.

**So fügen Sie Ihrer Auflistung einen Spann hinzu:**
1. Laden Sie die **Videodatei** für den Nachspann in das Feld angezeigt. Eine Dropdown Liste wird auch angezeigt, wenn Sie einen Nachspann wieder verwenden möchten, den Sie hochgeladen haben (vielleicht für eine Store-Auflistung in einer anderen Sprache).
2. Nachdem Sie den Nachspann hochgeladen haben, müssen Sie ein **Miniaturbild** hochladen, um darauf zu klicken. Dabei muss es sich um eine PNG-Datei handeln, die 1920 x 1080 Pixel beträgt, und ist in der Regel ein Bild, das aus dem Nachspann entnommen wird.
3. Klicken Sie auf das Stift Symbol, um einen **Titel** für den Nachspann hinzuzufügen (255 Zeichen oder weniger).
4. Wenn Sie der Auflistung weitere Nachspann hinzufügen möchten, klicken Sie auf Nachspann **Hinzufügen** , und wiederholen Sie die oben aufgeführten Schritte.

> [!TIP]
> Wenn Sie Filialen in mehreren Sprachen erstellt haben, können Sie die Option **aus vorhandenen nach** Spann auswählen auswählen, um die bereits hochgeladenen Auflistungen wiederzuverwenden. Sie müssen Sie nicht einzeln für jede Sprache hochladen.

Um einen Nachspann aus einer Liste zu entfernen, klicken Sie auf das **X** neben dem Dateinamen. Sie können auswählen, ob Sie es nur aus der aktuellen Speicher Liste, in der Sie arbeiten, entfernen möchten, oder ob Sie Sie aus allen Speicher Listen Ihres Produkts (in jeder Sprache) entfernen möchten.


### <a name="trailer-requirements"></a>Nach Spann Anforderungen

Beachten Sie bei der Bereitstellung Ihrer Nachspann die folgenden Anforderungen:

- Das Videoformat muss "MOV" oder "MP4" lauten.
- Die Dateigröße des Nachspann sollte 2 GB nicht überschreiten.
- Die Videoauflösung muss 1920 x 1080 Pixel betragen.
- Die Miniaturansicht muss eine PNG-Datei mit einer Auflösung von 1920 x 1080 Pixel sein.
- Der Titel darf nicht länger als 255 Zeichen sein.
- Nehmen Sie keine Alters Bewertungen in ihren Nachspann vor.

> [!WARNING]
> Eine Ausnahme von der Anforderung, Alters Bewertungen in ihren Nachspann einzuschließen, gilt **nur** für die **auf der Produktseite** angezeigten **Microsoft Store** . Alle Nachrichten außerhalb von Partner Center, die nicht ausschließlich für die Anzeige auf der Produktseite des Microsoft Store vorgesehen sind, **müssen** bei Bedarf eingebettete Bewertungsinformationen entsprechend den entsprechenden Richtlinien der Bewertungsbehörde anzeigen.  

Wie die anderen Felder auf der Seite "Store-Auflistung" müssen die Zertifikate bestanden werden, bevor Sie Sie in der Microsoft Store veröffentlichen können. Stellen Sie sicher, dass ihre Anhänger den [Microsoft Store Richtlinien](store-policies.md)entsprechen.

Je nach Dateityp gibt es weitere Anforderungen.

#### <a name="mov"></a>MOV

| Video | Audio | 
| --- | --- | 
| <ul><li>1080p ProRes (falls zutreffend)</li><li>Native Framerate; 29,97-fps bevorzugt</li></ul> | <ul><li>Stereo erforderlich</li><li>Empfohlene Audioebene:-16 lkfs/lufs</li></ul> |


#### <a name="mp4"></a>MP4

| Video | Audio |
| --- | --- |
| <ul><li>Codec: [H. 264](/windows/desktop/DirectShow/h-264-video-types) (AVC1)  </li><li>Progressiver Scan (keine Überlappung)</li><li>Hohes Profil</li><li>2 aufeinander folgende B-Frames</li><li>Geschlossener GOP. GOP der Hälfte der Framerate</li><li>CABAC</li><li>50 MB/s </li><li>Farbraum: 4.2.0</li></ul> | <ul><li>Codec: AAC-LC</li><li>Kanäle: Stereo-oder Umschließungs Sound</li><li>Abtastrate: 48 kHz</li><li>Audiobitrate: 384 KB/s für Stereo, 512 KB/s für den umgebenden Sound</li></ul> |

> [!WARNING]
> Kunden hören möglicherweise keine Audiodaten für MP4-Dateien, die mit anderen Codecs als AVC1 codiert sind.

Für H. 264-zwischen Dateien wird Folgendes empfohlen:
- Container: MP4
- Keine Bearbeitungs Listen (oder Sie können die AV-Synchronisierung verlieren)
- am Anfang der Datei am Anfang der Datei (schnell Start)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Windows 10-und Xbox-Image (16:9 Superhero-Kunst)

Im Abschnitt **Windows 10 und Xbox Image** wird das Image **16:9 Superhero Art (1920 x 1080 oder 3840 x 2160 Pixel)** in verschiedenen Layouts des Microsoft Store auf allen Windows 10-Gerätetypen (einschließlich Xbox) verwendet. Es wird empfohlen, dieses Image bereitzustellen, unabhängig davon, welche Betriebssystemversionen oder Gerätetypen Ihre APP als Ziel haben.

Dieses Bild ist für die ordnungsgemäße Anzeige *erforderlich* , wenn die Auflistung [Video](#trailers)-Nachspann enthält. Für Kunden unter Windows 10, Version 1607 oder höher (einschließlich Xbox), wird Sie als Haupt Image am Anfang ihrer Store-Auflistung verwendet (oder wird angezeigt, nachdem alle nach spannenden nach spannenden). Sie kann auch verwendet werden, um Ihre APP in Werbe Layouts im gesamten Store bereitzustellen. Beachten Sie, dass dieses Bild nicht den Titel oder anderen Text des Produkts enthalten darf.

Im folgenden finden Sie einige Tipps, die Sie beim Entwerfen des Images beachten sollten:

- Das Bild muss eine PNG-Datei sein, die entweder 1920 x 1080 Pixel oder 3840 x 2160 Pixel ist.
- Wählen Sie ein dynamisches Bild aus, das sich auf die APP bezieht, um die Erkennung und Differenzierung zu fördern. Vermeiden Sie Stockfotografien oder generische visuelle Elemente.
- Fügen Sie keinen Text in das Bild ein.
- Vermeiden Sie das Platzieren wichtiger visueller Elemente im unteren dritten Teil des Bilds (da wir in manchen Layouts einen Farbverlauf über diesen Teil anwenden können).
- Platzieren Sie die wichtigsten Details in der Mitte des Bilds (da wir das Image in einigen Layouts zuschneiden können).
- Minimieren Sie Leerraum.
- Vermeiden Sie das Anzeigen der Benutzeroberfläche Ihrer App, und verwenden Sie keine Bilder für bestimmte Geräte.
- Vermeiden Sie politische und nationale Designs, Flaggen oder religiöse Symbole.
- Fügen Sie keine Bilder von taktlosen Gesten, Nacktheit, Glücksspiel, Währung, Drogen, Tabakprodukte oder Alkohol hinzu.
- Verwenden Sie keine Bilder mit Waffen, die auf den Benutzer gerichtet sind, oder übermäßiger Gewalt und Blut.

Wenn Sie dieses Image bereitstellen, können Sie Ihre APP für die bereitgestellten Aktions Chancen in Erwägung gezogen, aber es ist nicht garantiert, dass Ihre APP vorgestellt wird. Weitere Informationen finden [Sie unter machen Sie Ihre APP leicht zu bewerben](make-your-app-easier-to-promote.md) .


### <a name="xbox-images"></a>Xbox-Bilder

Diese Images sind für die ordnungsgemäße Anzeige erforderlich, wenn Sie die APP auf der Xbox veröffentlichen. 

Es gibt drei verschiedene Größen, die Sie hochladen können:
- **Marken Schlüsselart, 584 x 800 Pixel** : muss den Titel des Produkts enthalten. Für dieses Bild ist eine Branding-Leiste erforderlich. Behalten Sie den Titel und alle Schlüsselbilder in den ersten drei Quartalen des Bilds, da im unteren Quartal ein Overlay angezeigt werden kann.
- **Mit dem Titel Hero Art, 1920 x 1080 Pixel** : muss den Titel des Produkts enthalten. Behalten Sie den Titel und alle Schlüsselbilder in den ersten drei Quartalen des Bilds, da im unteren Quartal ein Overlay angezeigt werden kann.
- **Vorgestellte Werbe Quadrat Kunst, 1080 x 1080 Pixel** : der Titel des Produkts darf *nicht* enthalten sein.

> [!NOTE]
> Zur optimalen Anzeige auf Xbox müssen Sie im Abschnitt [Store-Logos](#store-logos) auch ein 9:16-Image **(720 x 1080 oder 1440 x 2160 Pixel)** bereitstellen.


### <a name="holographic-image"></a>Holographic-Bild

Das **2:1-Image (2400 x 1200)** wird nur verwendet, wenn Ihre APP die Holographic-Gerätefamilie unterstützt. Wenn dies der Fall ist, empfiehlt es sich, dieses Image bereitzustellen.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Bilder nur für Windows 8. x und/oder Windows Phone 8. x 

Wenn Ihre zuvor übermittelte App frühere Betriebssystemversionen unterstützt (Windows 8. x und/oder Windows Phone 8. x), müssen diese Images bereitgestellt werden, damit wir Ihre APP in Werbe Layouts in Erwägung gezogen haben (obwohl Sie nicht garantieren, dass Ihre APP angezeigt wird). Wenn Ihre APP diese früheren Betriebssystemversionen nicht unterstützt, überspringen Sie diesen Abschnitt. (Dieser Abschnitt wurde früher als **optionale Werbebilder** bezeichnet.)

**Für Windows Phone 8,1 und frühere** Versionen können zwei Bildgrößen in Aktions Layouts verwendet werden: **1000 x 800 Pixel (5:4)** und **358 x 358 Pixel (1:1)** . Wenn Ihre APP unter Windows Phone 8,1 oder einer früheren Version ausgeführt wird, empfiehlt es sich, in beiden Größen Images bereitzustellen.  

> [!TIP]
> Stellen Sie sicher, dass Sie ein Symbol für die APP-Kachel "300 x 300 app" im Abschnitt " [Store-Logos](#store-logos) " bereitstellen, um die Übermittlung Windows Phone 8,1 oder früher Dadurch wird sichergestellt, dass Ihre APP nicht im Store mit einem leeren Symbol angezeigt wird.  

**Bei Windows 8.1 und früheren** Versionen können einige Werbe Layouts ein Image in der **414 x 180** Pixelgröße verwenden. Wenn Ihre APP unter Windows 8.1 oder früher ausgeführt wird, empfiehlt es sich, ein Image in dieser Größe bereitzustellen.
