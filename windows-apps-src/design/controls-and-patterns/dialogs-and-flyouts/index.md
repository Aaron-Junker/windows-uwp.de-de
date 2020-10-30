---
description: Dialogfelder und Flyouts zeigen vorübergehende UI-Elemente an, die angezeigt werden, wenn der Benutzer sie anfordert oder eine Aktion erfolgt, die eine Benachrichtigung oder Genehmigung erfordert.
title: Dialogfelder und Flyouts
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: bf4746a6d3a024fec31045bbf9c63d3b11315b8c
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93033843"
---
# <a name="dialogs-and-flyouts"></a>Dialogfelder und Flyouts

Dialogfelder und Flyouts sind vorübergehende UI-Elemente, die angezeigt werden, wenn Ereignisse Benachrichtigungen, Genehmigungen oder zusätzliche Informationen vom Benutzer erfordern.

> **Plattform-APIs:** [ContentDialog-Klasse](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [Flyout-Klasse](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**Dialoge**

![Beispiel für ein Dialogfeld](../images/dialogs/dialog_RS2_delete_file.png)

Dialogfelder sind modale Benutzeroberflächenüberlagerungen, die kontextbezogene App-Informationen enthalten. Dialogfelder blockieren Interaktionen mit dem App-Fenster, bis sie explizit geschlossen werden. Sie verlangen häufig eine Aktion vom Benutzer.

**Flyouts**

![Flyout-Beispiel](../images/flyout-example2.png)

Ein Flyout ist ein einfaches kontextbezogenes Popupmenü zum Anzeigen der Benutzeroberfläche, die im Zusammenhang mit den Aktionen des Benutzers steht. Es umfasst die Platzierungs- und Größenlogik und kann dazu verwendet werden, ein sekundäres Steuerelement oder weitere Einzelheiten zu einem Element anzuzeigen.

Im Gegensatz zu einem Dialogfeld kann ein Flyout durch Tippen oder Klicken auf eine Stelle außerhalb des Flyouts schnell geschlossen werden. Das Drücken der ESC- oder Zurück-Taste sowie das Ändern der Größe des App-Fensters oder der Ausrichtung des Geräts haben denselben Effekt.

## <a name="is-this-the-right-control"></a>Ist dies das richtige Steuerelement?

Dialogfelder und Flyouts stellen sicher, dass den Benutzern wichtige Informationen bekannt sind, sie stellen jedoch auch eine Unterbrechung dar. Da Dialogfelder modal (gesperrt) sind, unterbrechen sie die Benutzer und verhindern, dass diese bis zur Interaktion mit dem Dialogfeld andere Schritte durchführen können. Flyouts sind weniger störend, das Anzeigen zu vieler Flyouts kann jedoch eine ablenkende Wirkung haben.

Wenn ein Dialogfeld oder Flyout eingesetzt werden soll, müssen Sie sich für eine der beiden Optionen entscheiden.

Angesichts der Tatsache, dass Dialogfelder im Gegensatz zu Flyouts Interaktionen blockieren, sollten Dialogfelder auf Situationen beschränkt bleiben, in denen der Benutzer unterbrochen werden soll, um sich auf eine bestimmte Information oder die Beantwortung einer Frage zu konzentrieren. Flyouts hingegen können verwendet werden, wenn Sie auf etwas aufmerksam machen möchten, das der Benutzer jedoch ignorieren kann.

   <p><b>Fälle, in denen ein Dialogfeld verwendet werden sollte:</b> <br/>
<ul>
<li>Für wichtige Informationen, die der Benutzer vor dem Fortsetzen lesen und bestätigen <b>muss</b>. Beispiele:
<ul>
  <li>Die Sicherheit des Benutzers ist möglicherweise gefährdet.</li>
  <li>Der Benutzer möchte eine wertvolle Ressource endgültig ändern.</li>
  <li>Der Benutzer möchte eine wertvolle Ressource löschen.</li>
  <li>Bestätigen eines In-App-Einkaufs</li>
</ul>

</li>
<li>Fehlermeldungen, die sich auf den Gesamtkontext der App beziehen, z. B. Verbindungsfehler.</li>
<li>Fragen, wenn dem Benutzer eine blockierende Frage gestellt werden muss, z. B. wenn die App nicht im Auftrag des Benutzers eine Auswahl treffen kann. Eine blockierende Frage kann nicht ignoriert oder verschoben werden und sollte dem Benutzer klar definierte Auswahlmöglichkeiten bieten.</li>
</ul>
</p>


   <p><b>Fälle, in denen ein Flyout verwendet werden sollte:</b> <br/>
<ul>
<li>Erfassen zusätzlicher Informationen, die erforderlich sind, bevor eine Aktion abgeschlossen werden kann.</li>
<li>Anzeigen von Informationen, die nur vorübergehend relevant sind. So können Sie z. B. in einer Fotogalerie-App ein Flyout einsetzen, damit eine große Version des Bilds angezeigt wird, wenn der Benutzer auf eine Miniaturansicht klickt.</li>
<li>Anzeigen weiterer Informationen, z. B. von Details oder ausführlicheren Beschreibungen eines Elements auf der Seite.</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Möglichkeiten, die Verwendung von Dialogfeldern und Flyouts zu vermeiden

Berücksichtigen Sie die Bedeutung der zu vermittelnden Informationen: Sind sie wichtig genug, um den Benutzer zu unterbrechen? Berücksichtigen Sie zudem, wie häufig die Informationen angezeigt werden müssen. Wenn ein Dialogfeld oder eine Benachrichtigung alle paar Minuten angezeigt wird, sollten Sie diese Informationen stattdessen in die primäre UI einbinden. So können Sie z. B. in einem Chat-Client anstatt eines Flyouts, der jedes Mal angezeigt wird, wenn sich ein Freund anmeldet, eine Liste der Freunde anzeigen, die derzeit online sind, und diejenigen Freunde hervorheben, die sich gerade anmelden.

Dialogfelder werden häufig zum Bestätigen einer Aktion vor deren Ausführung verwendet (z. B. vor dem Löschen einer Datei). Wenn Sie davon ausgehen, dass die Benutzer häufig eine bestimmte Aktion ausführen, sollten Sie eine Möglichkeit bereitstellen, versehentliche Aktionen rückgängig zu machen, anstatt jedes Mal die Bestätigung der Aktion zu erzwingen.

## <a name="how-to-create-a-dialog"></a>Erstellen eines Dialogfelds

Informationen finden Sie im [Artikel zu Dialogfeldern](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Erstellen eines Flyouts

Informationen finden Sie im [Artikel zu Flyouts](flyouts.md). 

## <a name="examples"></a>Beispiele

<table>
<th align="left">XAML-Steuerelementekatalog<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Wenn Sie die App <strong style="font-weight: semi-bold">XAML-Steuerelementekatalog</strong> installiert haben, klicken Sie hier, um die App zu öffnen und <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> oder <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> in Aktion zu sehen.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Beziehen der XAML-Steuerelementekatalog-App (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Abrufen des Quellcodes (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

