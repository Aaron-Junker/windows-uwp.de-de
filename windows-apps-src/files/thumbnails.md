---
Description: So Miniaturbilder zu verwenden, um Benutzern, die Vorschau von Dateien in UWP-apps zu helfen.
title: Richtlinien für Miniaturbilder in UWP-Apps
label: Thumbnail images
template: detail.hbs
ms.date: 12/19/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 15984e00b036bf44d6e4a7f60cb6435ea1add291
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57642005"
---
# <a name="thumbnail-images"></a>Miniaturbilder

Diese Richtlinien beschreiben, wie Miniaturbilder verwendet werden, um den Benutzern eine Dateivorschau zu bieten, während sie in Ihrer UWP-App navigieren. 

**Wichtige APIs**

-   [**ThumbnailMode**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>Sollte meine App Miniaturansichten beinhalten?

Wenn Ihre App das Durchsuchen von Dateien ermöglicht, können Sie Miniaturbilder anzeigen, damit Benutzer eine schnelle Vorschau auf diese Dateien erhalten. 

Verwenden Sie Miniaturansichten für Folgendes: 
- Anzeigen einer Vorschau für mehrere Elemente in einer Galeriesammlung (beispielsweise Dateien und Ordner) In einer Fotogalerie sollten zum Beispiel Miniaturansichten verwendet werden, um den Benutzern eine kleine Vorschau der einzelnen Bilder anzuzeigen, während sie ihre Fotodateien durchsuchen.

    ![Videogalerie](images/thumbnail-gallery.png)

- Anzeigen einer Vorschau für ein einzelnes Element in einer Liste (beispielsweise eine bestimmte Datei) Möglicherweise möchte ein Benutzer mehr Informationen über eine Datei sehen, darunter eine größere Miniaturansicht für eine bessere Vorschau, bevor er entscheidet, ob er die Datei öffnet. 

    ![Videovorschau](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen
- Geben Sie den [Miniaturansichtsmodus](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView oder SingleItem) an, wenn Sie Miniaturansichten abrufen. So stellen Sie sicher, dass Miniaturbilder für die Anzeige der Dateitypen optimiert sind, die Benutzer anzeigen möchten. 
    - Verwenden Sie den SingleItem-Modus, um unabhängig vom Dateityp eine Miniaturansicht für ein einzelnes Element abzurufen. Die anderen Miniaturansichtsmodi dienen zum Anzeigen von Vorschauen auf mehrere Dateien. 

- Zeigen Sie generische Platzhalterbilder anstelle von Miniaturansichten an, während die Miniaturansichten geladen werden. Wenn Sie Platzhalter auf diese Weise verwenden, scheint die App besser zu reagieren, da Benutzer mit Vorschauen interagieren können, bevor die Miniaturansicht geladen ist. 

    Platzhalterbilder sollten folgende Merkmale aufweisen:
    * Sie sollten sich konkret auf die Art des Elements beziehen, das sie darstellen. Beispielsweise sollten Ordner, Bilder und Videos alle eigene spezielle Platzhalter haben. 
    * Sie sollten die gleiche Größe und das gleiche Seitenverhältnis aufweisen wie das Miniaturbild, das sie darstellen. 
    * Sie sollten angezeigt werden, bis das Miniaturbild geladen ist. 

- Verwenden Sie Platzhalterbilder mit Beschriftungen, um Ordner oder Dateigruppen darzustellen und von einzelnen Dateien zu unterscheiden.

- Wenn Sie eine Miniaturansicht nicht abrufen können, zeigen Sie stattdessen ein Platzhalterbild an. 

- Zeigen Sie zusätzliche Dateiinformationen an, wenn Sie Vorschauen für Dokumente und Musikdateien bereitstellen. Benutzer können dann wichtige Informationen über eine Datei identifizieren, die durch ein Miniaturbild allein möglicherweise nicht vermittelt werden. Beispielsweise können Sie für eine Musikdatei neben der Miniaturansicht des Albumcovers auch den Namen des Interpreten anzeigen. 

- Zeigen Sie für Bild- und Videodateien keine zusätzlichen Dateiinformationen an. In den meisten Fällen ist ein Miniaturbild ausreichend für Benutzer, die Bilder und Videos durchsuchen. 

## <a name="additional-usage-guidelines"></a>Weitere Richtlinien zur Verwendung
Empfohlene [Miniaturansichtsmodi](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) und deren Features:

<table>
<tr>
<th> Vorschauanzeige für</th>
<th> Miniaturansichtsmodi </th>
<th> Features der abgerufenen Miniaturansichten </th>
</tr>
<tr>
<td> Bilder<br /> Videos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Größe</b>: "Mittel", vorzugsweise mindestens 190 (wenn die Größe des Abbilds 190 x 130 ist) <br />
<b>Seitenverhältnis</b>: Einheitliche, breit Seitenverhältnis von ca..7 (190 x 130 ist die Größe 190) <br />
Zugeschnitten für die Vorschau <br /> 
Gute Ausrichtung von Bildern in einem Raster durch einheitliches Seitenverhältnis  </td>
</tr>
<tr>
<td> Dokumente<br />Musik </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Größe</b>: Kleine, vorzugsweise mindestens 40 x 40 Pixel <br />
<b>Seitenverhältnis</b>:  Einheitliche, quadratische Seitenverhältnis  <br />
Aufgrund des quadratischen Seitenverhältnisses gut geeignet für die Vorschau von Albumcovern <br /> 
Dokumente sehen ebenso aus wie in einem Dateiauswahlfenster (Verwendung derselben Symbole). </td>
</tr>
</tr>
<tr>
<td> Alle einzelnen Elemente (unabhängig vom Dateityp) </td>
<td> SingleItem </td>
<td> <b>Größe</b>: Kleine, vorzugsweise mindestens 40 x 40 Pixel <br />
<b>Seitenverhältnis</b>:  Einheitliche, quadratische Seitenverhältnis  <br />
Aufgrund des quadratischen Seitenverhältnisses gut geeignet für die Vorschau von Albumcovern <br /> 
Dokumente sehen ebenso aus wie in einem Dateiauswahlfenster (Verwendung derselben Symbole). </td>
</tr>
</table>

Hier sehen Sie Beispiele dafür, wie sich abgerufene Miniaturbilder je nach Dateityp und Miniaturansichtsmodus unterscheiden:
<div class="mx-responsive-img">
<table>
<tr>
<th>Elementtyp</th>
<th>Beim Abruf mithilfe von: <ul><li>PicturesView <li>VideosView</ul></th>
<th>Beim Abruf mithilfe von: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>Beim Abruf mithilfe von: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Picture</td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>Die Miniaturansicht wird auf das quadratische Seitenverhältnis zugeschnitten. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Video</td>
<td>Die Miniaturansicht weist ein Symbol auf, wodurch sie sich von Bildern unterscheidet. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>Die Miniaturansicht wird auf das quadratische Seitenverhältnis zugeschnitten. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Musik</td>
<td>Bei der Miniaturansicht handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der Kachel der App bestimmt. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Wenn die Datei über ein Albumcover verfügt, entspricht die Miniaturansicht dem Albumcover.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
Andernfalls handelt es sich bei der Miniaturansicht um ein Symbol vor einem Hintergrund entsprechender Größe.</td>
<td>Wenn die Datei über ein Albumcover verfügt, entspricht die Miniaturansicht dem Albumcover mit dem ursprünglichen Seitenverhältnis der Datei.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
Anderenfalls ist die Miniaturansicht ein Symbol. </td>
</tr>
<tr>
<td>Dokument</td>
<td>Bei der Miniaturansicht handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der Kachel der App bestimmt. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>Bei der Miniaturansicht handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der Kachel der App bestimmt. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>Die Miniaturansicht für das Dokument, sofern vorhanden. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
Anderenfalls ist die Miniaturansicht ein Symbol. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Ordner</td>
<td>Wenn der Ordner eine Bilddatei enthält, wird die Miniaturansicht für das Bild verwendet.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
Andernfalls wird keine Miniaturansicht abgerufen.</td>
<td>Es wird keine Miniaturansicht abgerufen.</td>
<td>Das Miniaturbild ist das Ordnersymbol.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Dateigruppe</td>
<td>Wenn der Ordner eine Bilddatei enthält, wird die Miniaturansicht für das Bild verwendet.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> Andernfalls wird keine Miniaturansicht abgerufen. </td>
<td>Wenn die Dateigruppe eine Datei mit einem Albumcover enthält, ist die Miniaturansicht das Albumcover. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />Andernfalls wird keine Miniaturansicht abgerufen. </td>
<td>Wenn die Dateigruppe eine Datei mit einem Albumcover enthält, ist die Miniaturansicht das Albumcover und verwendet das ursprüngliche Seitenverhältnis der Datei. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />Andernfalls handelt es sich bei der Miniaturansicht um ein Symbol, das eine Gruppe von Dateien darstellt. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Verwandte Themen
- [ThumbnailMode-Enumeration](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [StorageItemThumbnail-Klasse](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- ["Storagefile"-Klasse](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Datei- und Miniaturansicht-Beispiel (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Liste und die Rasteransicht](../design/controls-and-patterns/lists.md)