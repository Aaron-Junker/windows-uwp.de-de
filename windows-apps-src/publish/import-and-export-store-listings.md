---
description: Sie können Speicher Listen für Ihre apps erstellen, ohne Partner Center zu verwenden, indem Sie Ihre Auflistungen in einer CSV-Datei exportieren, Ihre Informationen und Assets eingeben und dann die aktualisierte Datei importieren.
title: Importieren und Exportieren von Store-Einträgen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Import Store-Auflistungen, Exportieren von Store-Listen, Importieren von Export, Speichern von CSV
ms.localizationpriority: medium
ms.openlocfilehash: 426d5f84056347f5adb28cafa8aedfe5da255fdc
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93029683"
---
# <a name="import-and-export-store-listings"></a>Importieren und Exportieren von Store-Einträgen

Anstatt [Informationen für Ihre Store-Einträge direkt in Partner Center einzugeben](create-app-store-listings.md), können Sie Informationen hinzufügen oder aktualisieren, indem Sie Ihre Listen in einer CSV-Datei exportieren, Ihre Informationen und Assets eingeben und dann die aktualisierte Datei importieren. Sie können diese Methode verwenden, um Auflistungen von Grund auf neu zu erstellen oder die bereits erstellten Listen zu aktualisieren.

Diese Option ist besonders nützlich, wenn Sie Speicher Listen für Ihr Produkt in mehreren Sprachen erstellen oder aktualisieren möchten, da Sie die gleichen Informationen kopieren und in mehrere Felder einfügen und problemlos Änderungen vornehmen können, die auf bestimmte Sprachen angewendet werden sollen. Sie können diese Methode jedoch nicht verwenden, um [plattformspezifische Speicher Listen](create-platform-specific-store-listings.md) für zuvor veröffentlichte apps zu erstellen oder zu aktualisieren, die ältere Betriebssystemversionen unterstützen. 

> [!TIP]
> Sie können diese Funktion auch zum Importieren und Exportieren von Speicher Listen Details für ein Add-on verwenden. Bei Add-ons funktioniert der Prozess mit der Ausnahme, dass [nur die für Add-ons relevanten Felder](#add-ons) eingeschlossen werden.

Beachten Sie, dass Sie Listen immer direkt im Partner Center erstellen oder aktualisieren können (auch wenn Sie zuvor die Import-/Exportmethode verwendet haben). Das direkte aktualisieren in Partner Center ist möglicherweise einfacher, wenn Sie nur eine einfache Änderung vornehmen, aber Sie können beide Methoden jederzeit verwenden.

## <a name="export-listings"></a>Listen exportieren

Klicken Sie auf der Seite Übermittlungs Übersicht für eine APP auf die **Liste exportieren** (im Abschnitt **Store-Listen** ), um eine CSV-Datei zu generieren, die in UTF-8 codiert ist. Speichern Sie diese Datei an einem Speicherort auf Ihrem Computer.

Sie können Microsoft Excel oder einen anderen Editor verwenden, um diese Datei zu bearbeiten. Beachten Sie, dass Sie mit Microsoft 365 Versionen von Excel eine CSV-Datei als **CSV-UTF-8-Datei (durch Trennzeichen getrennt) (*. CSV)** speichern können, die von anderen Versionen jedoch möglicherweise nicht unterstützt wird. Informationen zu den Versionen von Excel, die dieses Feature unterstützen, finden Sie im [Bulletin zu neuen Features in Excel 2016](https://support.office.com/article/what-s-new-in-excel-for-office-365-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73?ui=en-US&rs=en-001&ad=US)und weitere Informationen zur Codierung als UTF-8 in [verschiedenen Editoren](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Wenn Sie noch keine Auflistungen für Ihr Produkt erstellt haben, enthält die CSV-Datei, die Sie exportiert haben, keine benutzerdefinierten Daten. Sie sehen Spalten für **Feld** , **ID** , **Typ** und **Standard** und Zeilen, die jedem Element entsprechen, das in einer Speicher Auflistung angezeigt werden kann.

Wenn Sie bereits Auflistungen erstellt (oder Pakete hochgeladen haben), sehen Sie auch Spalten mit sprach Gebiets Schema-Codes, die der Sprache für jede von Ihnen erstellte Auflistung entsprechen (oder in ihren Paketen erkannt wurden), sowie alle aufgelisteten Informationen, die Sie zuvor bereitgestellt haben.
     
Im folgenden finden Sie eine Übersicht darüber, was in den einzelnen Spalten der exportierten CSV-Datei enthalten ist:
- Die Spalte **Feld** enthält einen Namen, der jedem Teil einer Speicher Auflistung zugeordnet ist. Diese entsprechen denselben Elementen, die Sie beim Erstellen von Store-Auflistungen im Partner Center bereitstellen können. einige der Namen unterscheiden sich jedoch geringfügig. Für Elemente, bei denen Sie mehr als einen der gleichen Elementtypen eingeben können, werden mehrere Zeilen angezeigt, bis zur maximalen Anzahl, die Sie angeben können. Für **App-Features** werden z. b. **Feature1** , **Feature2** usw. angezeigt, die bis zu **Feature20** werden (da Sie bis zu 20 App-Features bereitstellen können).
- Die **ID** -Spalte enthält eine Zahl, die Partner Center den einzelnen Feldern zuordnet. 
- Die **Typspalte** enthält allgemeine Anleitungen dazu, welche Art von Informationen für dieses Feld bereitgestellt werden soll, z. b. **Text** oder **relativer Pfad (oder URL zu Datei im Partner Center)** . 
- Die **Standard** Spalte (und alle Spalten, die mit Language-Locale-Codes gekennzeichnet sind) stellen den Text oder die Assets dar, die jedem Teil der Store-Auflistung zugeordnet sind. Sie können die Felder in diesen Spalten bearbeiten, um Aktualisierungen an den Store-Listen vorzunehmen.

>[!IMPORTANT]
> Ändern Sie keine der Informationen in den Spalten **Feld** , **ID** oder **Typ** . Die Informationen in diesen Spalten müssen unverändert bleiben, damit die importierte Datei verarbeitet wird.

## <a name="update-listing-info"></a>Aktualisieren von Listen Informationen

Nachdem Sie Ihre Auflistungen exportiert und die CSV-Datei gespeichert haben, können Sie Ihre Auflistungs Informationen direkt in der CSV-Datei bearbeiten. 

Zusammen mit der **Standard** Spalte verfügt jede Sprache, für die Sie eine Auflistung erstellt haben, über eine eigene Spalte. Die Änderungen, die Sie in einer Spalte vornehmen, werden auf die Beschreibung in dieser Sprache angewendet. Sie können Auflistungen für neue Sprachen erstellen, indem Sie den sprach Gebiets Schema-Code der nächsten leeren Spalte in der obersten Zeile hinzufügen. Eine Liste der gültigen sprach Gebiets Schema Codes finden Sie [unter Unterstützte Sprachen](supported-languages.md).

Sie können die Spalte **Standard** verwenden, um Informationen einzugeben, die Sie in allen Beschreibungen Ihrer APP freigeben möchten. Wenn das Feld für eine bestimmte Sprache leer bleibt, werden die Informationen aus der Standard Spalte für diese Sprache verwendet. Sie können dieses Feld für eine bestimmte Sprache überschreiben, indem Sie unterschiedliche Informationen für diese Sprache eingeben.

Die meisten Speicher Listenfelder sind optional. Die **Beschreibung** und ein Screenshot sind für jede Auflistung erforderlich. für Sprachen, denen keine Pakete zugeordnet sind, müssen Sie auch einen **Titel** angeben, um anzugeben, welche ihrer reservierten APP-Namen für diese Auflistung verwendet werden sollen. Für alle anderen Felder können Sie das Feld leer lassen, wenn Sie es nicht in die Liste aufnehmen möchten. Wenn Sie ein Feld für eine bestimmte Sprache leer lassen, überprüfen wir, ob in der Standard Spalte Informationen in diesem Feld vorhanden sind. Wenn dies der Fall ist, werden diese Informationen verwendet. 

Sehen Sie sich z. b. das folgende Beispiel an: 

![Beispiel für exportierte Auflistung](images/listingimport.png)
     
- Der Text "Default Description" wird für das **Beschreibungs** Feld in den Listen "en-US" und "fr-FR" verwendet. Allerdings würde das **Beschreibungs** Feld in der Liste "es-es" den Text "Spanish Description" verwenden. 
- Für das Feld **ReleaseNotes** wird der Text "German Release Notes" für en-US verwendet, und der Text "französische Versions Hinweise" wird für "fr-FR" verwendet. Für es-es werden jedoch keine Versions Anmerkungen angezeigt.

Wenn Sie keine Änderungen an einem bestimmten Feld vornehmen möchten, können Sie die gesamte Zeile aus der Tabelle löschen, **mit Ausnahme der Zeilen für die Nachspann und der zugehörigen Miniaturansichten und Titel** . Abgesehen von diesen Elementen wirkt sich das Löschen einer Zeile nicht auf die Daten aus, die mit diesem Feld in den Listen verknüpft sind. Auf diese Weise können Sie alle Zeilen entfernen, die Sie nicht bearbeiten möchten, sodass Sie sich auf die Felder konzentrieren können, in denen Sie Änderungen vornehmen.

Das Löschen der Informationen in einem Feld für eine Sprache, ohne die gesamte Zeile zu entfernen, funktioniert je nach Feld anders. Bei Feldern, **Type** deren Typ **Text** ist, wird dieser Eintrag durch das Löschen der Informationen in einem Feld einfach aus der Auflistung in dieser Sprache entfernt.  Das Löschen der Informationen in einem Feld für ein Bild, z. b. Screenshot oder Logo, hat jedoch keine Auswirkungen. das vorherige Bild wird weiterhin verwendet, es sei denn, Sie entfernen es durch direktes Bearbeiten im Partner Center. Wenn Sie die Informationen für ein Nachspann Feld löschen, wird dieser nach Spann aus Partner Center entfernt. Stellen Sie also sicher, dass Sie über eine Kopie aller benötigten Dateien verfügen, bevor Sie dies tun.

Viele der Felder in den exportierten Auflistungen erfordern einen Text Eintrag, wie z. b. die im obigen Beispiel, **Description** und **ReleaseNotes** . Geben Sie für diese Typen von Feldern einfach den entsprechenden Text in das Feld für jede Sprache ein. Achten Sie darauf, dass Sie die Längen-und anderen Anforderungen für jedes Feld einhalten. Weitere Informationen zu diesen Anforderungen finden Sie unter [Erstellen von App Store-Listen](create-app-store-listings.md).

Das Bereitstellen von Informationen für Felder, die Assets entsprechen (z. b. Bilder und Nachspann), ist etwas komplizierter. Anstelle von **Text** handelt es sich bei dem **Typ** für diese Assets um einen **relativen Pfad (oder eine URL zu einer Datei im Partner Center)** . 
     
Wenn Sie bereits Assets für Ihre Store-Einträge hochgeladen haben, werden diese Objekte dann durch eine URL dargestellt. Diese URLs können in mehreren Beschreibungen für ein Produkt oder sogar über verschiedene Produkte innerhalb desselben Entwickler Kontos wieder verwendet werden, sodass Sie diese URLs kopieren können, um Sie in einem anderen Feld wiederzuverwenden, wenn Sie möchten.

> [!TIP]
> Um zu bestätigen, welches Asset einer URL entspricht, können Sie die URL in einen Browser eingeben, um das Bild anzuzeigen (oder das Nachspann Video herunterzuladen).  Sie müssen bei Ihrem Partner Center-Konto angemeldet sein, damit diese URL funktioniert.

Wenn Sie ein neues Asset verwenden möchten, das Sie dem Partner Center noch nicht hinzugefügt haben, können Sie dies tun, indem Sie die Auflistungen als Ordner anstatt als einzelne CSV-Datei importieren. Sie müssen einen Ordner erstellen, der die CSV-Datei enthält. Fügen Sie dann die Images desselben Ordners hinzu, entweder im Stamm Ordner oder in einem Unterordner. Sie müssen den vollständigen Pfad, einschließlich des Stamm Ordner namens, in das Feld eingeben.

> [!TIP]
> Um optimale Ergebnisse zu erzielen, wenn Sie Ihre Auflistungen als Ordner importieren, stellen Sie sicher, dass Sie die neueste Version von Microsoft Edge, Chrome oder Firefox verwenden.

Wenn der Stamm Ordner beispielsweise **my_folder** lautet und Sie ein Bild mit dem Namen **screenshot1.png** für **DesktopScreenshot1** verwenden möchten, können Sie screenshot1.png dem Stamm dieses Ordners hinzufügen und dann **my_folder/screenshot1.png** im Feld **DesktopScreenshot1** eingeben. Wenn Sie einen Ordner Images in ihrem Stamm Ordner erstellt und dann screenshot1.jpg dort abgelegt haben, geben Sie **my_folder/Images/-screenshot1.png** ein. Beachten Sie Folgendes: Nachdem Sie Ihre Auflistungen mithilfe eines Ordners importiert haben, werden die Pfade zu Ihren Bildern beim nächsten Exportieren der Listen in URLs in die Dateien in Partner Center konvertiert. Sie können diese URLs kopieren und einfügen, um Sie erneut zu verwenden (z. b. um dieselben Ressourcen in mehreren Auflistungs Sprachen zu verwenden). 

> [!IMPORTANT]
> Wenn die exportierte Auflistung nach spannenden enthält, sollten Sie beachten, dass durch das Löschen der URL zum Nachspann oder des Miniatur Bilds aus der CSV-Datei die gelöschte Datei vollständig aus dem Partner Center entfernt wird und Sie dort nicht mehr darauf zugreifen können (es sei denn, Sie wird auch in einer anderen Liste verwendet, in der Sie nicht gelöscht wurde). 

## <a name="import-listings"></a>Listen importieren

Nachdem Sie alle Änderungen in die CSV-Datei eingegeben haben (und alle Assets eingefügt haben, die Sie hochladen möchten), müssen Sie die Datei vor dem Hochladen speichern. Wenn Sie eine Version von Microsoft Excel verwenden, die UTF-8-Codierung unterstützt, wählen Sie **Speichern** unter aus, und verwenden Sie das **CSV-UTF-8-Format (durch Kommas getrennt) (*. CSV)** . Wenn Sie einen anderen Editor zum Anzeigen und Bearbeiten der CSV-Datei verwenden, stellen Sie sicher, dass die CSV-Datei vor dem Hochladen in UTF-8 codiert ist.

Wenn Sie bereit sind, die aktualisierte CSV-Datei hochzuladen und die Auflistungs Daten zu importieren, wählen Sie auf der Seite "Übermittlungs Übersicht" **Import Listen** Wenn Sie nur eine CSV-Datei importieren, wählen Sie **Import. CSV** aus, navigieren Sie zu Ihrer Datei, und klicken Sie auf **Öffnen** . Wenn Sie einen Ordner mit Bilddateien importieren, wählen Sie Ordner importieren aus, navigieren Sie zu Ihrem Ordner, und klicken Sie auf **Ordner auswählen** . Stellen Sie sicher, dass in Ihrem Ordner nur eine CSV-Datei zusammen mit allen Assets vorhanden ist, die Sie hochladen. 

Beim Verarbeiten der importierten CSV-Datei wird eine Statusanzeige angezeigt, die den Import-und Validierungs Status widerspiegelt. Dies kann einige Zeit in Anspruch nehmen, insbesondere dann, wenn viele Auflistungen und/oder Bilddateien vorhanden sind. 

Wenn Probleme erkannt werden, wird ein Hinweis angezeigt, dass Sie erforderliche Updates vornehmen müssen, und versuchen Sie es noch mal. Wählen Sie den Link **Fehler anzeigen** aus, um anzuzeigen, welche Felder ungültig sind und warum. Sie müssen diese Probleme in Ihrer CSV-Datei beheben (oder alle ungültigen Assets ersetzen) und ihre Auflistungen dann erneut importieren.

> [!TIP]
> Sie können diese Informationen später erneut über den Link **Fehler für den letzten Import anzeigen** aufrufen.

Keine der Informationen aus Ihrer CSV-Datei wird im Partner Center gespeichert, bis alle Fehler in der Datei behoben wurden, auch wenn die Felder fehlerfrei sind. Nachdem Sie eine CSV-Datei mit Fehlern importiert haben, werden die von Ihnen bereitgestellten Informationen im Partner Center gespeichert und für diese Übermittlung verwendet.

Sie können Ihre Auflistungen weiterhin aktualisieren, indem Sie eine andere aktualisierte CSV-Datei importieren oder Änderungen direkt im Partner Center vornehmen.

## <a name="add-ons"></a>Add-Ons

Für Add-ons verwendet das Importieren und Exportieren von Speicher Listen den oben beschriebenen Prozess, mit dem Unterschied, dass Ihnen nur die drei Felder angezeigt werden, die für [Add-on-Speicher Listen](create-add-on-store-listings.md)relevant sind: **Description** , **Title** und **StoreLogo300x300** (als **Symbol** auf der Seite "Store-Auflistung" im Partner Center bezeichnet). Das Feld " **Title** " ist erforderlich, und die beiden anderen Felder sind optional.

Beachten Sie, dass Sie Speicher Listen für jedes Add-on in Ihrer APP separat importieren und exportieren müssen, indem Sie zur Seite "Übermittlungs Übersicht" für das Add-on navigieren.


