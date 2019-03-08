---
title: Xbox Live-Seite "Zusammenfassung"
description: Beschreibt, wie Sie die Zusammenfassung von Xbox Live nutzen können
ms.assetid: ''
ms.date: 10/19/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox-Spiele, Uwp, Windows 10, Xbox one Xbox live summary, Zusammenfassung, veröffentlichen, Xbox live-Verlauf "," Befehlsleiste "," Registerkarte "Verlauf" "," Zusammenfassungstabelle
ms.openlocfilehash: 289b472939c721e5bfb373d4de62ed800840bf57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605985"
---
# <a name="the-xbox-live-configuration-summary-page"></a>Die Xbox Live-Konfiguration Seite "Zusammenfassung"

Sie können [Partner Center](https://developer.microsoft.com/dashboard) so konfigurieren Sie das Xbox Live für den Titel. Mit Partner Center werden Sie die Dienstkonfiguration für die einzelnen Titel des Sandboxes verwalten können.
Um die Xbox Live-Aspekte der Titel des konfigurieren, navigieren zu den Xbox Live-Abschnitt für Titel, befindet sich im **Services** > **Xbox Live**. Auf dieser Seite finden Sie eine Momentaufnahme der aktuellen Konfiguration auf der ausgewählten Sandbox. Außerdem finden Sie eine detaillierte Aufschlüsselung der was konfiguriert und mit der Sandbox sowie veröffentlicht wurde.

## <a name="sandbox-selector"></a>Sandbox-Auswahl

 [Sandboxes](../../xbox-live-sandboxes.md) sind nun ein Element Navigation auf oberster Ebene, wechseln Sie zwischen, oder durch Auswahl erweitern. Die Benutzeroberfläche des Titels Sandboxes oben als Registerkarten angezeigt. Die auf den einzelnen Registerkarten angezeigten Informationen wird im Rahmen der zugeordneten Sandbox.  

![Abbildung der Sandbox-Registerkarten wechseln](../../images/summary/sandbox-tabs1.gif)

 Sie können zusätzliche Sandboxes hinzufügen, indem Sie die Auswahl der "+" die bieten Ihnen ein Dialogfeld, Sie geben die Sandbox, kopieren Sie die Konfiguration von möchten, und welche Sandbox möchten die Konfiguration zu kopieren.  

 ![Abbildung der erweiterbaren Datenträger in einer neuen Registerkarte "Sandbox"](../../images/summary/sandbox-tabs2.gif)

## <a name="command-bar"></a>Befehlsleiste

Wie bereits erwähnt ist die angezeigte Seite immer innerhalb des Kontexts einer Sandbox, daher die Befehlsleiste, die direkt darunter verfügbar gemacht werden alle die Aktionen angezeigt, dass Sie Ihre bestimmten Sandbox ausführen können. Die Befehle, die für Sie verfügbar sind:  

* **Exportieren Sie** -steht Ihnen eine Zip-Datei, die alle konfigurierten Dokumente in der Sandbox enthält.
* **Import** -ermöglicht es Ihnen, geben Sie eine Zip-Datei mit gültigen XBL, die nach dem Hochladen dokumentiert werden in der Sandbox verfügbar sein.
* **Zertifizieren** – die aktuelle Konfiguration der Zertifizierungsstellen-Sandbox veröffentlicht.  *Sie können auch die Schaltfläche "Veröffentlichen" verwenden und ändern Sie das Ziel zum Zertifikat, um dies zu erreichen.*
* **Verlauf** -öffnet eine Registerkarte, die zeigt Informationen dazu, wer was und wann erstellt. Können Sie diese Registerkarte auf einer beliebigen Seite öffnen, und es wird gefiltert, um die Objekte, die auf dieser Seite erstellt.
* **Veröffentlichen von** -können Sie Ihre Sandbox für Quelle und Ziel auswählen. Nach dem auswählen eine Überprüfung wird ausgeführt, Sie darüber informiert, wenn Sie die Konfiguration veröffentlichen können. Wenn zugelassen, Auswahl wird veröffentlichen legen Sie die Konfiguration für den Sandkasten, damit Sie diese Konfiguration bei der Verwendung des entsprechenden Sandkasten testen können.  
  
  
![Bild der Befehlsleiste](../../images/summary/command-bar.png)  

## <a name="summary-table"></a>Zusammenfassungstabelle

Die Benutzeroberfläche bietet jetzt eine sinnvolle Roll Einrichten Ihrer verschiedener Konfigurationen ermöglicht ein auf einen Blick, was konfiguriert wurde, was optional ist und was noch vor der Veröffentlichung auf "Retail" erforderlich ist.  

* **Detail** – beschreibt, was für eine bestimmte Funktion in der aktuellen Sandbox konfiguriert wurde (Dies schließt die Objekte, die Sie erstellt haben, jedoch noch nicht veröffentlicht)
* **Seit der letzten Veröffentlichung** – Dadurch können Sie wissen, welche neuen Konfigurationen, die Sie erstellt haben, die nicht in Ihrer Sandbox zu Testzwecken veröffentlicht wurden
* **Status** – Sie werden informiert, ob dieses Feature auf "Retail" veröffentlicht werden kann. Nichts mit der Bezeichnung "Nicht bereit für den Einzelhandel" die angesprochen werden muss, "Optional" markierte Zeilen werden nach dem Ermessen des Entwicklers.

*Nachdem Sie die Schaltfläche"testen" ausgewählt haben, zuvor Validierung wird ausgeführt, und erst dann woher wissen Sie, wenn Sie ein Problem haben, die Sie korrigieren musste; Dies optimiert, und erstellt eine bessere Benutzererfahrung*  
  
![Bild der Befehlsleiste](../../images/summary/summary-table.png)  

## <a name="history-pane"></a>Verlaufsbereich

Im Verlaufsbereich zeigt an, welche Objekte in der Sandbox erstellt wurden, und gibt an, von wem und wann. Auf der Seite "Zusammenfassung" zeigt der Verlaufsbereich, alle Objekte erstellt, und Veröffentlichen von Aktionen, die auf die Sandbox vorgenommen. Wenn Sie diesen Bereich zu einer bestimmten Seite z. B. Erfolge öffnen werden Sie jedoch nur Auszeichnung Verlauf, sodass Sie problemlos Verlauf filter zum Suchen von angezeigt.  

![Bild von den Verlaufsbereich](../../images/summary/history.png)  

## <a name="best-practices"></a>Empfohlene Methoden

* Veröffentlichen Sie Ihre Änderungen in Ihrer Sandbox, nachdem Sie ein paar Änderungen, um sicherzustellen, dass sie auf Ihrer Testkonten und die Geräte aktiv sind vorgenommen haben.
* Verwenden Sie die neue Registerkartenansicht, die Zusammenfassungstabelle und Verlaufsbereich können Sie schnell zu identifizieren, was veröffentlicht, auf.
* In extremen Fällen, in denen müssen Sie XML-Vergleiche zwischen Sandboxes, mit denen Sie, das Exportfeature von beiden Sandkästen abzurufen dokumentiert, und öffnen Sie sie sich mit einem Tool wie über verglichen werden soll.
* Export kann verwendet werden, um eine lokale Version der Dateien abzurufen, die auf Ihrer eigenen quellcodeverwaltung ein Commit ausgeführt werden kann. Auf diese Weise, wenn Sie alle konfigurationsänderungen, die nicht tatsächlich verloren sie gehen verloren. Sie können die lokale Konfiguration von Ihrer quellcodeverwaltung und importieren sie wieder in der Sandbox.