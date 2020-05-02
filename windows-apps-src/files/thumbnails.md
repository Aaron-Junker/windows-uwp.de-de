---
Description: 'Gewusst wie: Verwenden von Miniaturbildern zur Unterstützung von Benutzern beim Anzeigen einer Vorschau für Dateien in UWP-Apps.'
title: Richtlinien für Miniaturbilder in UWP-Apps
label: Thumbnail images
template: detail.hbs
ms.date: 12/19/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 15984e00b036bf44d6e4a7f60cb6435ea1add291
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "63808714"
---
# <a name="thumbnail-images"></a>Miniaturbilder

Diese Richtlinien beschreiben, wie Miniaturbilder als Dateivorschau für Benutzer verwendet werden, während diese in deiner UWP-App navigieren. 

**Wichtige APIs**

-   [**ThumbnailMode**](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>Sollte meine App Miniaturbilder enthalten?

Wenn deine App das Durchsuchen von Dateien ermöglicht, kannst du Miniaturbilder anzeigen, damit Benutzer eine schnelle Vorschau für diese Dateien erhalten. 

Verwende Miniaturbilder in folgenden Fällen: 
- Anzeigen einer Vorschau für mehrere Elemente in einer Galeriesammlung (z. B. Dateien und Ordner) In einer Fotogalerie sollten beispielsweise Miniaturbilder verwendet werden, um den Benutzern eine kleine Vorschau der einzelnen Bilder anzuzeigen, während sie ihre Fotodateien durchsuchen.

    ![Videogalerie](images/thumbnail-gallery.png)

- Anzeigen einer Vorschau für ein einzelnes Element in einer Liste (z. B. eine bestimmte Datei) Es kann beispielsweise sein, dass der Benutzer weitere Informationen zu einer Datei anzeigen möchte, z. B. ein größeres Miniaturbild als bessere Vorschau, bevor er sich für das Öffnen der Datei entscheidet. 

    ![Videovorschau](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>Empfohlene und nicht empfohlene Vorgehensweisen
- Gib den [Miniaturbildmodus](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView oder SingleItem) an, wenn du Miniaturbilder abrufst. So stellst du sicher, dass Miniaturbilder für den jeweiligen Dateityp optimiert sind, den Benutzer anzeigen möchten. 
    - Verwende den SingleItem-Modus, um unabhängig vom Dateityp ein Miniaturbild für ein einzelnes Element abzurufen. Die anderen Miniaturbildmodi dienen zum Anzeigen einer Vorschau mit mehreren Dateien. 

- Zeige anstelle von Miniaturbildern generische Platzhalterbilder an, während die Miniaturbilder geladen werden. Bei Verwendung von Platzhaltern erscheint deine App reaktionsfähiger, weil Benutzer mit der Vorschau interagieren können, bevor das Miniaturbild geladen wurde. 

    Platzhalterbilder sollten folgende Merkmale aufweisen:
    * Sie sollten sich konkret auf die Art des Elements beziehen, das sie darstellen. Beispielsweise sollten Ordner, Bilder und Videos jeweils über eigene spezielle Platzhalter verfügen. 
    * Sie sollten die gleiche Größe und das gleiche Seitenverhältnis wie das Miniaturbild aufweisen, das sie darstellen. 
    * Sie sollten angezeigt werden, bis das Miniaturbild geladen wurde. 

- Verwende Platzhalterbilder mit Beschriftungen, um Ordner oder Dateigruppen so darzustellen, dass sie sich von Einzeldateien unterscheiden.

- Zeige ein Platzhalterbild an, wenn kein Miniaturbild abgerufen werden kann. 

- Zeige zusätzliche Dateiinformationen an, wenn du eine Vorschau für Dokumente und Musikdateien bereitstellst. Benutzer können so wichtige Informationen zu einer Datei erhalten, die nur durch ein Miniaturbild unter Umständen nicht vermittelt werden können. Beispielsweise kannst du für eine Musikdatei zusätzlich zum Miniaturbild des Albumbilds auch den Namen des Interpreten anzeigen. 

- Zeige für Bild- und Videodateien keine zusätzlichen Dateiinformationen an. In den meisten Fällen ist ein Miniaturbild für Benutzer ausreichend, die Bilder und Videos durchsuchen. 

## <a name="additional-usage-guidelines"></a>Weitere Richtlinien zur Verwendung
Empfohlene [Miniaturbildmodi](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) und deren Features:

<table>
<tr>
<th> Vorschauanzeige für</th>
<th> Miniaturbildmodi </th>
<th> Features der abgerufenen Miniaturbilder </th>
</tr>
<tr>
<td> Bilder<br /> Videos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Größe</b>: Mittel, vorzugsweise mindestens 190 (bei einer Bildgröße von 190 x 130) <br />
<b>Seitenverhältnis</b>: Einheitliches, breites Seitenverhältnis von ca. 0,7 (190 x 130 bei einer Größe von 190) <br />
Zugeschnitten für die Vorschau. <br /> 
Gut geeignet für die Ausrichtung von Bildern in einem Raster aufgrund des einheitlichen Seitenverhältnisses.  </td>
</tr>
<tr>
<td> Dokumente<br />Musik </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Größe</b>: Klein, vorzugsweise mindestens 40 x 40 Pixel <br />
<b>Seitenverhältnis</b>:  Einheitlich, quadratisches Seitenverhältnis  <br />
Gut geeignet für die Vorschau von Albumbildern aufgrund des einheitlichen Seitenverhältnisses. <br /> 
Dokumente sehen genauso wie in einem Dateiauswahlfenster aus (Verwendung der gleichen Symbole). </td>
</tr>
</tr>
<tr>
<td> Beliebiges einzelnes Element (unabhängig vom Dateityp) </td>
<td> SingleItem </td>
<td> <b>Größe</b>: Klein, vorzugsweise mindestens 40 x 40 Pixel <br />
<b>Seitenverhältnis</b>:  Einheitlich, quadratisches Seitenverhältnis  <br />
Gut geeignet für die Vorschau von Albumbildern aufgrund des einheitlichen Seitenverhältnisses. <br /> 
Dokumente sehen genauso wie in einem Dateiauswahlfenster aus (Verwendung der gleichen Symbole). </td>
</tr>
</table>

Hier sind Beispiele dafür angegeben, wie sich abgerufene Miniaturbilder je nach Dateityp und Miniaturbildmodus unterscheiden:
<div class="mx-responsive-img">
<table>
<tr>
<th>Elementtyp</th>
<th>Beim Abrufen mithilfe von: <ul><li>PicturesView <li>VideosView</ul></th>
<th>Beim Abrufen mithilfe von: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>Beim Abrufen mithilfe von: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Picture</td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>Das Miniaturbild wird auf das quadratische Seitenverhältnis zugeschnitten. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Video</td>
<td>Das Miniaturbild weist ein Symbol auf, wodurch es sich von Bildern unterscheidet. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>Das Miniaturbild wird auf das quadratische Seitenverhältnis zugeschnitten. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>Für das Miniaturbild wird das ursprüngliche Seitenverhältnis der Datei verwendet. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Musik</td>
<td>Beim Miniaturbild handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der App-Kachel bestimmt. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Wenn die Datei über ein Albumbild verfügt, entspricht das Miniaturbild dem Albumbild.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
Andernfalls handelt es sich beim Miniaturbild um ein Symbol vor einem Hintergrund entsprechender Größe.</td>
<td>Wenn die Datei über ein Albumbild verfügt, entspricht das Miniaturbild dem Albumbild mit dem ursprünglichen Seitenverhältnis der Datei.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
Andernfalls ist das Miniaturbild ein Symbol. </td>
</tr>
<tr>
<td>Dokument</td>
<td>Beim Miniaturbild handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der App-Kachel bestimmt. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>Beim Miniaturbild handelt es sich um ein Symbol vor einem Hintergrund entsprechender Größe. Die Hintergrundfarbe wird von der Hintergrundfarbe der App-Kachel bestimmt. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>Das Miniaturbild für das Dokument, sofern vorhanden. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
Andernfalls ist das Miniaturbild ein Symbol. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Ordner</td>
<td>Wenn der Ordner eine Bilddatei enthält, wird das Miniaturbild des Bilds verwendet.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
Andernfalls wird kein Miniaturbild abgerufen.</td>
<td>Es wird kein Miniaturbild abgerufen.</td>
<td>Das Miniaturbild ist das Ordnersymbol.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Dateigruppe</td>
<td>Wenn der Ordner eine Bilddatei enthält, wird das Miniaturbild des Bilds verwendet.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> Andernfalls wird kein Miniaturbild abgerufen. </td>
<td>Wenn die Dateigruppe eine Datei mit einem Albumbild enthält, ist das Miniaturbild das Albumbild. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />Andernfalls wird kein Miniaturbild abgerufen. </td>
<td>Wenn die Dateigruppe eine Datei mit einem Albumbild enthält, entspricht das Miniaturbild dem Albumbild, und das ursprüngliche Seitenverhältnis der Datei wird verwendet. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />Andernfalls handelt es sich beim Miniaturbild um ein Symbol, das für eine Gruppe von Dateien steht. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Zugehörige Themen
- [ThumbnailMode-Enumeration](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [StorageItemThumbnail-Klasse](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [StorageFile-Klasse](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Beispiel für Datei- und Ordnerminiaturbild (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Listen- und Rasteransicht](../design/controls-and-patterns/lists.md)