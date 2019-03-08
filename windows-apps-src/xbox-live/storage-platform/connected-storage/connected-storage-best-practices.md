---
title: Best Practices Storage verbunden
description: Empfehlungen zum Erzielen der besten Leistung und aus dem Speicher verbunden auftreten
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, eine Verbindung von Storage
ms.localizationpriority: medium
ms.openlocfilehash: dbdb8f95e9afc44a3280d897e74f74d327f94b97
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632925"
---
# <a name="connected-storage-best-practices"></a>Bewährte Methoden für Onlinespeicher

Entwickler sollten Daten speichern, in logischen Gruppen unterteilen, die statt monolithische speichert unabhängig voneinander aktualisiert werden. Dadurch wird ein Titel reduziert die Menge der Daten, die sie in verschiedenen Situationen, die sowohl lokale ressourcennutzung zu schreiben und Hochladen der Nutzung der Netzwerkbandbreite. Die API ermöglicht es auch Titeln zu mehr als ein Datenelement in einer atomaren Operation zu aktualisieren, die garantiert erfolgreich vollständig oder nicht wirksam zu (z. B. im Fall von schwerwiegenden Fehler Mid speichern).

Da Xbox One Benutzer schnell zwischen den Titeln wechseln können, sollten Entwickler entwerfen, deren Position zu deren aktuellem Status bereit, um auf kurzfristig in Erwartung des Empfangs einer Suspend-Ereignis, das zu einem beliebigen Zeitpunkt so gut wie möglich zu speichern. Der verbundene Speicher-API verwendet RAM außerhalb der Title-Reservierung als erster Punkt der Speicher, um den Titel schreibgeschwindigkeit während der kurze zu maximieren anhalten Zeitfenster. Klicken Sie dann bleiben die Daten in einem permanenten Speicher, gleicht sie mit jeder anderen datenschreibvorgänge seit dem letzten Upload, und plant die Datenuploads. Sobald gespeichert und in der Warteschlange für den Upload, ist das System stabil, um verschiedene Fehler wie z. B. Netzwerk Konnektivität verloren gehen oder Stromausfall.

## <a name="when-to-load-a-users-connected-storage-space-data"></a>Wann die Daten eines Benutzers verbundenen Speicher Speicherplatz geladen

Die Ausführungszeit für das Laden eines Benutzers verbundenen Datenspeicherplatz kann variieren. Apps sollten es sich um diese Aktion während der Hauptfunktion und nicht als Reaktion auf Abmelden eines Benutzers wird gestartet oder als Reaktion auf eine Suspend-Benachrichtigung erhalten, aus dem System durchführen.
Im Allgemeinen sollten apps Speicherplatz verbunden laden, wie ein Benutzer meldet sich an und alle gibt um zu spielen, es sei denn, in das Spiel ist, ist ein Modus, in denen es bestimmte keine Speicherfunktionalität ist, erforderlich. Berücksichtigen Sie auch das Laden eines Speicherplatzes mit verbunden mit langen Sequenzen von Laden von Daten, Ausrichtung, damit die Vorgänge parallel ausgeführt werden können.
Nachdem Ihre app die Daten eines Benutzers verbundenen Speicher Speicherplatz geladen wurde, sollten sie diesen Wert für die zukünftige speichert beibehalten. Beibehalten von einem verbundenen Speicherplatz im Laufe der Zeit muss sich nicht auf negative Auswirkungen auf Leistung oder Stabilität. Da eine Prüfung der Synchronisierung Laden einen Speicherplatz mit verbunden mit der Cloud verursacht werden, wenn das System online ist, freigeben und erneuten Laden des Benutzers verbunden Speicherplatz während der langsamen oder Umgebungen mit unzuverlässigem Netzwerk können dazu führen, dass des Benutzers eine Synchronisierung UI finden Sie unter bis die Systemzeiten heraus. Mit Storage Spaces nicht explizit freigegeben werden, um die dazu führen, dass bei der cloudsynchronisierung müssen verbunden. Einmal den Abschlusshandler in angegebenen ein `SubmitUpdatesAsync` Aufrufs wurde zurückgegeben mit `AsyncStatus::Completed`, das System übernimmt die Synchronisierung mit der Cloud, und zwar unabhängig davon, ob die app freigegeben der `ConnectedStorageSpace` Objekt.

## <a name="when-to-save"></a>Beim Speichern

Wenn eine Anwendung eine Suspend-Benachrichtigung empfängt, sollte die app mindestens relevante Daten, sodass das System in einem kontextbezogen entsprechenden Status für den Benutzer wiederherzustellen speichern.

Wenn Ihr Spiel Design, regelmäßig und automatisch verwendet oder vom Benutzer initiierte gespeichert und verbundene Speicher häufiger als beim Empfangen einer benachrichtigungs Suspend aufgerufen werden können. Dies ist also eine gute Möglichkeit, das Risiko eines Datenverlusts aufgrund eines Stromausfalls oder eines Absturzes zu reduzieren.
Wenn Sie Ihre Arbeitsweise mit der xdk-Version, entwickeln Wenn ein Benutzer abmeldet, und gültig, das User-Objekt für diesen Benutzer bleibt zu diesem Zeitpunkt kann die app endgültige ausführen Speichervorgänge mit Speicher verbunden.

## <a name="robustness"></a>Robustheit

Da gespeicherte Daten immer mit der Cloud synchronisiert wurde, konnte ein Fehler in der gespeicherten Daten und app-Code, der die app zum Absturz führt dazu, dass in der Cloud gesichert und auf Geräten verteilt werden. Um zu verhindern, dass Benutzer mit einer app, die bei jedem Start stürzt ab, Entwerfen Sie die app aus, um sicherzustellen, dass ein:

-   Benutzer erreichen einen Punkt in der app, an der sie einen gespeicherten Zustand versetzen, verwalten können, auch wenn einige gespeicherte Daten falsch formatiert ist.
-   Die app kann beschädigte Daten beeinflusst werden automatisch zu verarbeiten, so viele Daten wie möglich wiederherstellen und alles wieder einen sicheren Zustand zu initialisieren.

## <a name="use-cases-for-save-game-designs"></a>Anwendungsfälle für die speichern-Game-Entwürfe

Der Entwurf für ein Spiel speichern System, das die optimale Verwendung von Containern in Speicherplatz verbunden ist, hängt von der Art der app:

### <a name="single-save"></a>Einzelne speichern

Für apps, die eine einzige, speichern Kampagne-Stil-Systems, wie eine erste Person Shooter:

-   Fügen Sie alle Daten in einem einzelnen Container, und Schreiben Sie immer an den gleichen Container anhand des Namens identifiziert.
-   Ist es eine Option zum Zurücksetzen der alle Daten, die alle gespeicherten Daten des Benutzers, deaktivieren, werden für den Fall, dass er wünscht Wiedergabe von der app von Anfang an und mit keine vorherigen Status beibehalten wird.

### <a name="multiple-saves"></a>Mehrere speichert.

Für apps, die mit eine feste Anzahl von Slots, z. B. fünf speichern zu können, gibt es zwei Möglichkeiten, den Container verwenden, um das Spiele Daten zu speichern:

-   Store alle 5 Slots in einem festen Namen Container mithilfe von 1-BLOB-Elements für die einzelnen speichern Slot. Verwenden diese Methode alle 5 Slots werden vollständig synchronisiert und verfügbar ist, oder, im Fall, den Synchronisierung zu einem beliebigen Zeitpunkt ein Fehler auftritt, keiner der die Steckplätze synchronisiert werden und bleibt in ihren vorherigen Zustand an. Wenn ein Benutzer die app offline auf zwei verschiedene Konsolen speichern Status in Einschubfach 1 auf der ersten Konsole und in Einschubfach 2 in der zweiten Konsole spielt, muss der Benutzer die Daten beibehalten, die zum Herstellen einer Verbindung beide Konsolen zu Xbox Live auswählen; die Zusammenführungslogik für Container erzeugt einen Konflikt auf.
-   Store jedem Steckplatz in einem Container mit dem eigenen Namen an. Dadurch wird ein unabhängiger Status in jedem Steckplatz, sogar auf mehreren Computern, die offline sind. Wenn ein Benutzer eines durch eine Synchronisierung abgebrochen wird, ist es jedoch möglich, dass nur einige der die Steckplätze während dieser Sitzung zur Verfügung stehen; Einige der Container kann nicht heruntergeladen haben. In diesem Fall wird der Benutzer benachrichtigt, dass die Synchronisierung nicht vollständig abgeschlossen wurde und der Cloud-Daten nicht auf die lokale Konsole.

Welcher Ansatz verwendet wird, sollte den Benutzer mit der Benutzeroberfläche, um einzelne speichert aus Slots löschen die app bereitstellen.

### <a name="warning"></a>Warnung

**Sollten keine abhängigen Daten in Containern gespeichert werden.** Speichern Sie Daten mit Abhängigkeiten nicht über mehr als ein Container. Container können aufgrund von unvollständigen Synchronisierung, Stromausfall oder andere fehlerbedingungen getrennt werden. In einem einzelnen Container gespeicherte Daten müssen es sich um eigenständige und konsistent sein.

### <a name="tips"></a>Tipps

**Führen Sie keine Benutzer davon abhalten Sie, durch das Deaktivieren der Konsole oder diese Seite verlassen.** Titel des sollten nicht davon abhalten, Benutzer durch das Deaktivieren der Konsole oder Ihre app verlassen, beim Speichern. Auf der Xbox 360 werden das System ein Benutzers deaktiviert werden, während Ihre Titel speichert, die Daten des Benutzers nicht gespeichert. Für Xbox One der Titel ein Suspend-Ereignis empfängt und verfügt über eine Sekunde mit, dass die Speicher-API von verbundenen um Zustand zu speichern. Das System stellt sicher, dass Daten ordnungsgemäß auf die Festplatte geschrieben sind, bevor sie vollständig heruntergefahren oder der Energiesparmodus wechselt. Unterbrechung genauso tritt auf, wenn der Benutzer des Titels Datenträger ein, um eine andere spielen auswirft.

**Behalten Sie die Speicherplätze verbunden.** Sollen Sie ConnectedStorageSpace Objekte beibehalten werden, anstatt versuchen Sie, sie zu laden, jedes Mal, wenn ein Lese- oder Schreibvorgang-Ereignis auftritt. Es gibt keine negativen Auswirkungen auf die Leistungsfähigkeit oder Stabilität verursacht durch ein ConnectedStorageSpace-Objekt für einen längeren Zeitraum beibehalten.

**Halten Sie Datengrößen klein.** Halten Sie die gespeicherte Datenmenge klein. Alle Benutzerdaten im Speicher angeschlossen ist in die Cloud hochgeladen, wenn die Konsole online ist. Optimieren Sie Ihre Datenformate, um Verzögerungen bei der minimalen und die Auslastung der Netzwerkbandbreite zu gewährleisten.

**Stellen Sie sicher, dass Benutzer nicht zu speichern, dagegen nicht.** Überprüfen Sie OutOfLocalStorage-Fehler zurückgegeben, die von GetForUserAsync und SubmitUpdatesAsync und "Query Users", um festzustellen, ob sie wirklich testen, ohne zu speichern. Wenn ein Benutzer gibt an, um Spiele zu speichern möchten, wiederholen Sie den Vorgang.

**Überprüfen des Benutzers Kontingent und aus, um Speicherplatz zu deaktivieren.** Überprüfen Sie für ein zurückgegebenes SubmitUpdatesAsync QuotaExceeded-Fehler. Wenn Ihre app auf diese Nachricht empfängt, Benachrichtigen von Benutzern, dass sie keine weiteren Daten speichern, bis sie Sie Speicherplatz freigegeben haben und mit Benutzeroberfläche, die ihnen ermöglicht, die dazu präsentieren. Jeder Benutzer erhält 256 oder 64 MB Daten pro app, und jeder Titel xdk-Version erhält, 64 MB pro Computer Speicher, der an die Konsole lokale ist.

**Speichern Sie den Status des Menüs später für die Wiederherstellung an.** Speichern Sie im Menüstatus und andere app-Einstellungen, zusätzlich zum Sparen von Core Spieledaten. Wenn der Benutzer einer anderen app spielt und dann zurück, mit Ihrer kehrt, stellen sie eine kontextbezogen entsprechende Menü Zustand her.
Reagieren Sie auf Änderungen des angemeldeten Benutzers. Benutzer können sich anmelden, während Ihre app angehalten wird. Wenn die app fortgesetzt wird, sollte es ermitteln, ob der angemeldete Benutzer geändert wurde. Beachten Sie, navigieren zu einer geeigneten Stelle innerhalb der app, wie z. B. ein Menü, in diesem Fall.

**Eine Benutzeroberfläche zum Verwalten von gespeicherten Daten bereitstellt.** Ihre app sollte Benutzeroberfläche bereitstellen, die Benutzern ermöglicht, ihre gespeicherten Daten innerhalb der app zu verwalten. Für apps mit der ein automatisches Speichern System sollte die app eine Option zum Zurücksetzen der gespeicherter Daten, damit Benutzer auf eine Play Standardzustand zurücksetzen können bieten.

**Stellen Sie sicher, dass Benutzer immer Benutzeroberfläche zum Verwalten von gespeicherte Spiele erreichen können.** Stellen Sie sicher, dass Ihre app immer die Verwaltungsoberfläche für gespeicherte Spiele, auch bei Buggy gespeicherter Daten erreichen kann. Wenn die Daten eines Benutzers gespeicherte aufgrund einer app oder beschädigte Daten nicht gelesen werden, sollte die app Benutzern in einen Zustand wiederherstellen, mit denen nicht abstürzt bzw. zu verhindern, dass sie spielen.