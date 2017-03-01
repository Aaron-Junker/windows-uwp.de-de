---
author: mijacobs
Description: "Gut gestaltete Animationen machen Apps lebhaft und lassen sie realistisch und perfekt erscheinen. Helfen Sie Benutzern, Kontextänderungen zu verstehen, und verbinden Sie Interaktionen mit visuellen Übergängen."
title: Bewegung und Animation in UWP-Apps
ms.assetid: 21AA1335-765E-433A-85D8-560B340AE966
label: Motion
template: detail.hbs
ms.author: mijacobs
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "Windows 10, UWP"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fc0c4f50cc7970fe6ff4cfa5c631a03a9f216470
ms.lasthandoff: 02/07/2017

---

# <a name="motion-for-uwp-apps"></a>Bewegung für UWP-Apps

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

Zweckmäßige, gut gestaltete Animationen machen Apps lebendig und lassen sie realistisch und perfekt erscheinen. Helfen Sie Benutzern dabei, Kontextänderungen zu verstehen, und verbinden Sie Interaktionen mit visuellen Übergängen.

## <a name="benefits-of-animation"></a>Vorteile der Animation


Animation bedeutet nicht nur, Dinge beweglich zu machen. Durch Animation lässt sich ein physisches Ökosystem für den Benutzer erstellen, in dem er leben und das er durch Berührungen verändern kann. Die Qualität der Erfahrung hängt davon ab, wie gut die App auf den Benutzer reagiert und welche Art von Persönlichkeit die Benutzeroberfläche vermittelt.

Achten Sie darauf, dass die Animation einem Zweck in Ihrer App dient. Die besten UWP-Apps (Universelle Windows-Plattform) verwenden Animation, um die Benutzeroberfläche zum Leben zu erwecken. Die Animation sollte folgenden Zwecken dienen:

-   Feedback basierend auf dem Verhalten des Benutzers geben.
-   Dem Benutzer beibringen, wie er mit der Benutzeroberfläche interagieren kann.
-   Angeben, wie zu vorherigen oder folgenden Ansichten navigiert werden kann.

Wenn ein Benutzer mehr Zeit in Ihrer App verbringt oder Aufgaben in Ihrer App komplexer werden, wird hochwertige Animation immer wichtiger: Durch Animation lässt sich beeinflussen, wie der Benutzer die kognitive Belastung und die Benutzerfreundlichkeit Ihrer App wahrnimmt. Animation bringt viele weitere direkte Vorteile mit sich:

-   **Animation bietet Hinweise hinsichtlich der Interaktion.**

    Animation ist direktional: Sie bewegt sich vor und zurück, in den Inhalt hinein und aus ihm hinaus und hinterlässt gleichzeitig kleine „Brotkrümel“-Hinweise, wie der Benutzer zur aktuellen Ansicht gelangt ist.

-   **Animation kann den Eindruck verbesserter Leistung vermitteln.**

    Wenn die Netzwerkgeschwindigkeit langsamer wird oder das System nicht reagiert, können Animationen dem Benutzer die Wartezeit kürzer erscheinen lassen.

-   **Animation schafft Persönlichkeit.**

    Die wohlüberlegte Windows Phone-Benutzeroberfläche nutzt Bewegung, um den Eindruck zu vermitteln, dass eine App mit dem Hier und Jetzt beschäftigt ist, und wirkt dem Gefühl entgegen, dass der Benutzer sich durch geschachtelte Hierarchien gräbt.

-   **Animation schafft Konsistenz.**

    Übergänge können Benutzern beim Erlernen der Bedienung neuer Apps helfen, indem Analogien mit Aufgaben hergestellt werden, mit denen der Benutzer bereits vertraut ist.

-   **Animation schafft Eleganz.**

    Durch Animationen kann dem Benutzer mitgeteilt werden, dass das Telefon noch mit der Verarbeitung beschäftigt und nicht eingefroren ist, und es können passiv neue Informationen angezeigt werden, die den Benutzer möglicherweise interessieren.

<h2>Inhalt dieses Abschnitts</h2>

<table>
<tr>
<th align="left">Animationstyp</th>
<th align="left">Beschreibung</th>
</tr>
    <tr>
        <td>[Hinzufügen und Löschen](motion-list.md)
        </td>
        <td>Mit Listenanimationen können Sie einzelne oder mehrere Elemente in einer Sammlung wie z. B. einem Fotoalbum oder einer Liste mit Suchergebnissen einfügen oder entfernen.
        </td>
    </tr>
    <tr>
        <td>[Drag & Drop](motion-dragdrop.md)
        </td>
        <td>Verwenden Sie Drag & Drop-Animationen, wenn Benutzer Objekte verschieben, z. B. wenn sie ein Element innerhalb einer Liste verschieben oder ein Element auf einem anderen ablegen.
        </td>
    </tr>
    <tr>
        <td>[Rand](motion-edgebased.md)
        </td>
        <td>Randbasierte Animationen blenden UI-Elemente ein oder aus, die vom Bildschirmrand ausgehen. Die Aktionen zum Anzeigen und Ausblenden können von Benutzern oder von der App initiiert werden. Die UI-Elemente können die Anwendung überlagern oder Teil der Hauptoberfläche der App sein. Wenn das UI-Element Teil der App-Oberfläche ist, müssen Sie möglicherweise die Größe der restlichen App entsprechend anpassen.
        </td>
    </tr>   
    <tr>
        <td>[Einblenden](motion-fade.md)
        </td>
        <td>Verwenden Sie Ein- und Ausblendungsanimationen, um Elemente anzuzeigen oder nicht anzuzeigen. Die beiden üblichen Animationen dieser Art sind das Einblenden und das Ausblenden.
        </td>
    </tr>   
    <tr>
        <td>[Zeiger](motion-pointer.md)
        </td>
        <td>Zeigeranimationen stellen visuelles Feedback für Benutzer bereit, wenn diese auf ein Element tippen. Bei der Animation für „Zeiger nach unten“ wird das gedrückte Element leicht verkleinert und geneigt. Sie wird wiedergegeben, wenn erstmalig auf ein Element getippt wird. Die Animation für „Zeiger nach oben“, mit der der ursprüngliche Zustand des Elements wiederhergestellt wird, wird beim Loslassen des Zeigers wiedergegeben.
        </td>
    </tr>   
    <tr>
        <td>[Popupanimationen](motion-popup-animations.md)
        </td>
        <td>Verwenden Sie Popupanimationen, um Popup-UI-Elemente für Flyouts oder benutzerdefinierte Popup-UI-Elemente anzuzeigen und auszublenden. Popupelemente sind Container, die über dem Inhalt der App angezeigt werden und ausgeblendet werden, wenn Benutzer außerhalb des Popupelements tippen oder klicken.
        </td>
    </tr>     
    <tr>
        <td>[Ändern der Position](motion-reposition.md)
        </td>
        <td>Verschiebt Elemente an eine neue Position.
        </td>
    </tr>
</table>

 

 

 

