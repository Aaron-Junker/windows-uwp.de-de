---
Description: Sie können die Store-Angebote für Ihre apps erstellen, ohne den Partner Center Ihre Angebote in eine CSV-Datei exportieren, geben Sie Ihre Informationen und Ressourcen und importieren Sie dann die aktualisierte Datei.
title: Importieren und Exportieren von Store-Einträgen
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, Store-Einträge importieren, Store-Einträge exportieren, Export importieren, Store-Einträge CSV
ms.localizationpriority: medium
ms.openlocfilehash: 3d98795d6ab43f92ad2927553b037524e2fc1ec7
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/24/2019
ms.locfileid: "63784768"
---
# <a name="import-and-export-store-listings"></a>Importieren und Exportieren von Store-Einträgen

Anstelle von [eingeben von Informationen für Ihre Angebote Store direkt im Partner Center](create-app-store-listings.md), Sie haben die Möglichkeit zum Hinzufügen oder Aktualisieren von Informationen durch Ihre Angebote in eine CSV-Datei exportieren, geben Sie Ihre Informationen und Ressourcen und importieren Sie dann die aktualisierte Datei. Sie können diese Methode verwenden, um Einträge von Grund auf neu zu erstellen oder Angebote zu aktualisieren, die Sie bereits erstellt haben.

Diese Option ist besonders nützlich, wenn Sie Store-Einträge für Ihr Produkt in mehreren Sprachen erstellen oder aktualisieren möchten, da Sie die Informationen in mehrere Felder kopieren/einfügen und problemlos alle Änderungen vornehmen können, die für bestimmte Sprachen gelten sollen. Allerdings können keine diese Methode zum Erstellen oder aktualisieren [plattformspezifischen Store-Angebote](create-platform-specific-store-listings.md) für zuvor veröffentlichte apps, die ältere Betriebssystemversionen unterstützen. 

> [!TIP]
> Sie können dieses Feature auch zum Importieren und Exportieren von Details für einen Store-Eintrag für ein Add-On anzeigen. Für Add-Ons ist der Prozess identisch, mit Ausnahme davon, dass [nur die Felder, die relevant für Add-Ons sind](#add-ons) enthalten sind.

Bedenken Sie, dass Sie immer erstellen oder Aktualisieren von Listen direkt im Partner Center (selbst wenn Sie die Import/Export-Methode zuvor verwendet haben) können. Aktualisieren direkt im Partner Center kann einfacher sein, Sie sind nur eine einfache Änderung vornehmen, aber Sie können beide Methoden verwenden, zu einem beliebigen Zeitpunkt.

## <a name="export-listings"></a>Exportieren von Einträgen

Klicken Sie auf der Übermittlungsübersicht für eine App auf **Eintrag exportieren** (im Abschnitt **Store-Einträge**), um eine im UTF-8 codierte CSV-Datei zu generieren. Speichern Sie diese Datei an einem Speicherort auf Ihrem Computer.

Sie können Microsoft Excel oder einem anderen Editor zum Bearbeiten dieser Datei verwenden. Beachten Sie, dass Sie mit der Office 365-Versionen von Excel die **.csv-Datei als UTF-8-CSV (Komma-getrennt) (CSV)** speichert, andere Versionen werden möglicherweise jedoch nicht unterstützt. Nähere Informationen dazu, welche Versionen von Excel dieses Feature unterstützen finden Sie im [Excel 2016 New Features Bulletin](https://support.office.com/en-us/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73) und Weitere Informationen zur Codierung als UTF-8 in verschiedenen Editoren finden Sie [hier](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Wenn Sie bisher noch keine Einträge für Ihr Produkt erstellt haben, enthält die exportierte CSV-Datei keine benutzerdefinierten Daten. Sie sehen Spalten für **Feld**, **ID**, **Typ** und **Standard**, sowie Zeilen, die jedem Element entsprechen, das in einem Store-Eintrag angezeigt werden kann.

Wenn Sie bereits Einträge erstellt haben (oder Pakete hochgeladen haben), sehen Sie auch Spalten mit Gebietsschemacodes, die der Sprache jedes Eintrags entsprechen, den Sie erstellt haben (oder den wir in Ihren Paketen entdeckt haben), sowie alle Eintragsinformationen, die Sie zuvor angegeben haben.
     
Nachfolgend finden Sie ein Überblick darüber, was in jeder der Spalten in der exportierten CSV-Datei enthalten ist:
- Die Spalte **Feld** enthält einen Namen, der mit jedem Teil eines Store-Eintrags verknüpft ist. Diese entsprechen die gleichen Elemente, die Sie beim Erstellen von Store-Angebote im Partner Center bereitstellen können, obwohl einige der Namen geringfügig unterscheiden. Für Elemente, für die Sie mehr als eine Antwort für den gleichen Typ eingeben können, sehen Sie mehrere Zeilen, bis zur maximalen Anzahl, die Sie angeben können. Für **App-Features** sehen Sie **Feature1**, **Feature2**usw., bis zu **Feature20** (da Sie nur bis zu 20 Features der App angeben können).
- Die **ID** Spalte enthält eine Anzahl, die Partner Center mit jedem Feld zuordnet. 
- Die **Typ** Spalte stellt die allgemeine Anleitung dazu, welche Art von Informationen zum Bereitstellen für dieses Feld, z. B. **Text** oder **relativen Pfad (oder die URL zur Datei im Partner Center)**. 
- Die Spalte **Standard** (und alle Spalten, die mit Gebietsschemacodes gekennzeichnet sind) stellen den Text oder Objekte dar, die mit jedem Teil der Store-Eintrag verknüpft sind. Sie können die Felder in diesen Spalten aktualisieren, um Ihre Store-Einträge zu bearbeiten.

>[!IMPORTANT]
> Ändern Sie die Informationen in den Spalten **Feld**, **ID** oder **Typ** nicht. Die Informationen in diesen Spalten muss in der Reihenfolge unverändert bleiben, damit die importierte Datei verarbeitet werden kann.

## <a name="update-listing-info"></a>Aktualisierte Eintragsinformationen

Nachdem Sie Ihre Einträge exportiert und die CSV-Datei gespeichert haben, können Sie die Eintragsinformationen direkt in der CSV-Datei bearbeiten. 

Zusammen mit der Spalte **Standard** enthält jede Sprache, für die Sie einen Eintrag erstellt haben, eine eigene Spalte. Die Änderungen, die Sie in einer Spalte vornehmen, werden für die Beschreibung in dieser Sprache angewendet. Sie können Einträge für neue Sprachen durch Hinzufügen des Gebietsschemacodes in der nächsten leere Spalte in der obersten Zeile hinzufügen. Eine Liste der gültigen Gebietsschemacodes finden Sie unter [Unterstützte Sprachen](supported-languages.md).

Sie können die Spalte **Standard** verwenden, um Informationen einzugeben, die Sie für alle App-Beschreibungen freigeben möchten. Wenn das Feld für eine bestimmte Sprache leer ist, werden die Informationen aus der Spalte „Standard” für diese Sprache verwendet. Sie können dieses Feld für eine bestimmte Sprache überschreiben, indem Sie verschiedene Informationen für die jeweilige Sprache eingeben.

Die meisten der Store-Eintragsfelder sind optional. Eine **Beschreibung** und ein Bildschirmfoto ist für jeden Eintrag erforderlich. Für Sprachen, die keine zugeordneten Pakete haben, müssen Sie ebenfalls einen **Titel** angeben, der vorschreibt, welcher Ihrer reservierten App-Name für diesen Eintrag verwendet werden soll. Für alle anderen Felder können Sie das Feld leer lassen, wenn es nicht im Eintrag eingeschlossen sein soll. Wenn Sie für eine bestimmte Sprache ein Feld leer lassen, wird überprüft, ob Informationen in diesem Feld in der Standardspalte vorhanden sind. Wenn dies der Fall ist, werden diese Informationen verwendet. 

Nehmen wir folgendes Beispiel: 

![Beispiel für einen exportierten Eintrag](images/listingimport.png)
     
- Der Text "Standardbeschreibung" wird für das Feld **Beschreibung** im englischen und französischen Eintrag verwendet. Das Feld **Beschreibung** im englischen Eintrag würden allerdings den Text "Spanische Beschreibung" verwenden. 
- Für das Feld **Versionshinweise** wird der Text "englische Versionshinweise" für englisch verwendet und der Text "Französische Versionshinweise" wird für französisch verwendet. Es werden jedoch keine Versionshinweise für Spanisch angezeigt.

Wenn Sie auf keinem bestimmten Feld Änderungen vornehmen möchten, können Sie die gesamte Zeile aus dem Arbeitsblatt löschen **mit Ausnahme der Zeilen für Trailer und ihren zugehörigen Miniaturansichten und Titeln**. Das Löschen einer Zeile hat außer für diese Elemente keine Auswirkung auf die zugehörigen Daten für das Feld des Eintrags. So können Sie jegliche Zeilen entfernen, die Sie nicht bearbeiten möchten, damit Sie sich auf die Felder konzentrieren können, in denen Sie Änderungen vornehmen möchten.

Das Löschen der Informationen aus einem Feld für eine Sprache, ohne dabei die ganze Zeile zu entfernen, funktioniert unterschiedlich, je nach dem Feld. Für die Felder, deren **Typ****Text** ist, löscht die Informationen aus einem Feld den gesamten Eintrag aus der Liste in dieser Sprache.  Löschen die Informationen in einem Feld um ein Bild, z. B. einen Screenshot oder ein Logo, müssen jedoch keine Auswirkungen; der vorherigen Abbildung wird weiterhin verwendet werden, es sei denn, Sie entfernen, indem Sie direkt im Partner Center bearbeiten. Löschen die Informationen für ein Feld Nachspann wird dieses Nachspanns tatsächlich aus Partner Center entfernen, seien Sie sicher, dass Sie eine Kopie aller erforderlichen Dateien verfügen, bevor Sie diesen Vorgang ausführen.

Viele der Felder in den exportierten Einträgen erfordern eine Texteingabe, wie im obigen Beispiel **Beschreibung** und **Versionshinweise** gezeigt. Geben Sie für diese Arten Felder einfach den entsprechenden Text in das Feld für jede Sprache ein. Folgen Sie unbedingt der Länge und anderen Anforderungen für die einzelnen Felder. Weitere Informationen zu diesen Anforderungen finden Sie unter [App Store-Einträge erstellen](create-app-store-listings.md).

Das Bereitstellen von Informationen für Felder, die Ressourcen entsprechen, wie z. B. Bilder und Trailer, sind etwas komplizierter. Statt **Text**, **Typ** für diese Objekte ist **relativen Pfad (oder die URL zur Datei im Partner Center)**. 
     
Wenn Sie bereits Ressourcen für Ihre Store-Einträge hochgeladen haben, werden diese Ressourcen von der URL dargestellt. Diese URLs können in mehreren Beschreibungen für ein Produkt oder sogar über verschiedene Produkte im gleichen Entwicklerkonto wiederverwendet werden, damit Sie diese URLs kopieren und sie in einem anderen Feld wiederverwenden, wenn Sie dies wünschen.

> [!TIP]
> Um zu bestätigen, welche Ressource einer bestimmten URL entspricht, können Sie die URL in einen Browser eingeben, um das Bild anzuzeigen (oder den Videotrailer herunterladen).  Sie müssen mit Ihrem Partner Center-Konto in dieser URL funktioniert angemeldet sein.

Wenn Sie ein neues Medienobjekt zu verwenden, das Sie zum Partner Center zuvor hinzugefügt haben möchten, können Sie dafür durch Ihre Angebote als einen Ordner und nicht als eine CSV-Datei importieren. Sie müssen einen Ordner erstellen, der die CSV-Datei enthält. Fügen Sie dann Ihre Bilder diesem Ordner hinzu, entweder im Stammverzeichnis oder in einem Unterordner. Sie müssen den vollständigen Pfad in das Feld eingeben, z. B. den Namen des Stammordners.

> [!TIP]
> Achten Sie für optimale Ergebnisse darauf, wenn Sie Ihren Eintrag als Ordner importieren, dass Sie die neueste Version von Microsoft Edge, Chrome oder Firefox verwenden.

Wenn der Stammordner beispielsweise **My_folder** heißt und Sie ein Bild verwenden möchten, dass **screenshot1.png** für **DesktopScreenshot1** heißt, können Sie screenshot1.png dem Stammverzeichnis dieses Ordners hinzufügen, und dann **my_folder/screenshot1.png** in das Feld **DesktopScreenshot1** eingeben. Wenn Sie einen Ordner für Images im Stammordner erstellt haben und dann screenshot1.jpg dort abgelegt wurde, geben Sie **my_folder/images/screenshot1.png** an. Beachten Sie, dass nach dem Importieren Ihre Angebote über einen Ordner, Pfade zu Bildern in URLs auf die Dateien im Partner Center das nächste Mal konvertiert werden, das Sie Ihre Angebote exportieren. Sie können diese URLs kopieren und sie erneut einfügen (z. B. um die gleichen Ressourcen in mehreren Sprachen des Eintrags zu verwenden). 

> [!IMPORTANT]
> Wenn Ihre exportierten Auflistung Nachspann enthält, denken Sie daran, dass die URL der Nachspann oder die Miniaturansicht aus der CSV-Datei vollständig, die gelöschte Datei aus Partner Center löschen wird, und Sie nicht mehr in der können sie es darauf zugreifen werden (es sei denn, es wird auch in Ano verwendet RE auflisten, in denen es nicht gelöscht wurde). 

## <a name="import-listings"></a>Importieren von Einträgen

Nachdem Sie alle Ihre Änderungen in der CSV-Datei eingegeben haben (und alle Objekte, die Sie hochladen möchten), müssen Sie die Datei speichern, bevor Sie sie hochladen. Wenn Sie eine Version von Microsoft Excel verwenden, die UTF-8 Codierung unterstützt, stellen Sie sicher, dass Sie **Speichern als** verwenden und das **CSV UTF-8 (Komma-getrennt) (.csv)**-Format verwenden. Wenn Sie einen anderen Editor verwenden, um die CSV-Datei anzuzeigen und zu bearbeiten, stellen Sie sicher, dass die CSV-Datei vor dem Hochladen in UTF-8 codiert ist.

Wenn Sie die aktualisierte CSV-Datei hochladen möchten und die Daten des Eintrags importieren, wählen Sie in der Übermittlungsübersicht **Einträge importieren** aus. Wenn Sie nur eine CSV-Datei importieren, wählen Sie **CSV importieren**, navigieren Sie zur Datei, und klicken Sie auf **Öffnen**. Wenn Sie einen Ordner mit Bilddateien importieren, wählen Sie „Ordner importieren”, navigieren Sie zu dem Ordner, und klicken Sie auf **Ordner auswählen**. Stellen Sie sicher, dass sich nur eine CSV-Datei im Ordner befindet, sowie alle Objekte, die Sie hochladen. 

Beim Verarbeiten Ihrer importierten CSV-Datei wird eine Statusanzeige eingeblendet, die den Status des Imports und der Überprüfung angezeigt. Dies kann einige Zeit dauern, insbesondere dann, wenn viele Einträge und/oder Bilddateien vorhanden sind. 

Wenn Probleme erkannt werden, sehen Sie einen Hinweis, der angibt, dass Sie erforderlichen Updates vornehmen müssen. Versuchen Sie es dann erneut. Wählen Sie das Link **Fehler anzeigen**, um anzuzeigen, welche Felder ungültig werden und warum. Sie müssen diese Probleme in der CSV-Datei beheben (oder ungültige Ressourcen ersetzen) und Ihre Einträge erneut importieren.

> [!TIP]
> Sie können diese Informationen später erneut über das Link **Anzeigen von Fehlern für den letzten Import** abrufen.

Keines der Informationen aus der CSV-Datei wird im Partner Center gespeichert werden, bis alle Fehler in der Datei, sogar für Felder ohne Fehler behoben wurden. Nachdem Sie eine CSV-Datei, die keine Fehler aufweist importiert haben, der Auflistung-Informationen, die, den von Ihnen bereitgestellte, wird im Partner Center gespeichert und für dieser Übermittlung verwendet werden.

Sie können weiterhin Updates zu Ihren Angeboten stellen, entweder durch eine andere aktualisierte CSV-Datei importieren oder durch die Änderungen direkt in Partner Center.

## <a name="add-ons"></a>Add-Ons

Für Add-ons, importieren und Exportieren von Store-Angebote verwendet die oben beschriebenen Prozess mit dem Unterschied, dass Sie nur, dass die drei Felder, die relevant für sehen [-Add-On-Store-Angebote](create-add-on-store-listings.md): **Beschreibung**, **Titel**, und **StoreLogo300x300** (bezeichnet als **Symbol** in der Store-Angebotsseite im Partner Center). Das Feld **Titel** ist erforderlich, und die beiden anderen Felder sind optional.

Beachten Sie, dass Sie Store-Einträge separat für jedes Add-On in Ihrer App importieren und exportieren müssen, indem Sie zu der Übermittlungsübersicht für das Add-On navigieren.


