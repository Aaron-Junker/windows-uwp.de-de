---
title: Onlinespeicher – Technische Übersicht
description: Ausführlichen Informationen zur Funktionsweise von Speicher verbunden.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 6eddd11a370b8dcadc5108fe00539c2c6d1d9d1a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57624055"
---
# <a name="connected-storage"></a>Onlinespeicher

> [!NOTE]
> Dieses Dokument wurde ursprünglich für verwaltete Partner Xbox One-Entwickler.  Einige Xbox One bestimmte Inhalte wie bei lokalen und Titel Speicher kann für die UWP für Windows ignoriert werden.  Die konzeptionelle Inhalte und -API in diesem Dokument sind immer noch relevant.  Wenden Sie sich an Ihren Ansprechpartner bei Microsoft (falls zutreffend) mit Ihren Fragen.

Der Speicher und Speichern von Modellen auf der Xbox One unterscheiden sich deutlich von den auf der Xbox 360; Xbox One ist ein flexibler Anwendungsmodell, die schnelle Anwendung wechseln, mehrere Anwendungen gleichzeitige unterstützt und schnell anhalten und Fortsetzen von apps. Mit der verbundenen Speicherverwaltungs-API automatisch gespeicherte Daten für Benutzer über mehrere Xbox One Konsolen wechselt, und ist auch für die Offlineverwendung verfügbar.

In diesem Thema werden folgende Inhalte behandelt:

-   Die verbundene Speicher API verwenden gespeichert Spiele und app-Daten auf der Xbox One.
-   Bewährte Methoden für den Entwurf einer app speichern-Systemen, sodass sie gut mit User Experience Vorteile, die das Xbox One-Anwendungsmodell integriert.
-   Wie maximiert die Geschwindigkeit, mit der Ihre app Daten speichern kann, wird das System verbundenen Speicher
-   Wie das System Datenkonflikte Synchronisierung und die Daten aus mehreren Konsolen behandelt ohne app-Benutzeroberfläche.
-   Die verbundene resilienz Speichermodell, das so konzipiert ist, dass die app immer eine Self-konsistente Sicht der Daten in einzelne Container auch bei einem unterbrochenen Netzwerk Verbindungen oder die Stromversorgung ausfällt hat.

> [!NOTE]
> Der Begriff *app*, wie hier verweist auf alle Anwendungen auf die Konsole, einschließlich Spiele.

## <a name="overview"></a>Übersicht

Das Xbox One-Anwendungsmodell ermöglicht verwenden Sie mehrere apps gleichzeitig angezeigt werden, was bedeutet, dass Ihre app ein Benutzers auf Daten gespeichert werden, bevor Sie durch das Deaktivieren der Konsole oder mit einer anderen app fortfahren warten nicht anfordern kann. Xbox One Benutzer werden auch nutzen, müssen ihre Daten automatisch für Konsolen Roaming, damit jeder Xbox One-Konsole ihre eigenen Konsole fühlt. Die Xbox One-Plattform bietet die verbundenen-Speicher-API können Sie Ihre app, die diese Anforderungen zu erfüllen.

Das System verbundenen Speicher ermöglicht apps, die zum Speichern von Daten als ein oder mehrere *Blobs* in *Container*. Wenn eine app Daten speichert, wird er schnell aus der exklusiven Partition in der freigegebenen Partition kopiert, damit die Aufgaben bei der die Daten auf Datenträger speichern und Hochladen in Storage von Xbox Live-Titel außerhalb der Lebensdauer Ihrer app verarbeitet werden können.

Wenn Ihre app Daten eines bestimmten Benutzers aus dem System verbundenen Speicher anfordert, wird das System automatisch mit der Cloud für die aktualisierten Daten überprüft, und Benutzer werden benachrichtigt, wenn sie auf die Daten herunterladen warten müssen. Das System außerdem dazu aufgefordert, die Benutzer zwischen den in Konflikt stehenden Daten in einigen Fällen, z. B. wenn ein Benutzer offline auf mehr als eine Konsole gespielt haben, wählen, oder beim Hochladen von einer anderen Konsole ist die Daten dieses Benutzers gespeichert.

Ihre app verfügt auch eine dedizierte, jedoch begrenzte, Menge von Cloud-Speicher für jeden Benutzer, sodass Benutzer nicht schwer zu Daten aus einer app endgültig löschen, um Platz für eine andere app speichert schaffen Entscheidungen. Es gibt jedoch eine begrenzte Menge an Speicherplatz auf der Xbox One Festplatte für die Zwischenspeicherung lokal gespeichert, damit das System eine Benutzeroberfläche bereitstellt, für die Freigabe von lokalen Cachespeicher. Benutzer können steuern, was lokal zwischengespeichert wird; Sie verlieren nicht den Zugriff auf Daten, denen sie interessieren, wenn sie offline spielen. Das System verbundenen Speicher kann auch eine kleine Menge von Benutzer-unabhängige Daten für die app lokal gespeichert werden. Diese Daten pro Computer kein Roaming erfolgt und nicht in die Cloud hochgeladen.

Der Xbox One-verbundenen Speichersystem übernimmt die energieverwaltung des Systems, damit ausstehende Schreibvorgänge auf der Festplatte und Uploads in die Cloud automatisch behandelt werden, besteht keine Notwendigkeit für Titel für die Benutzeroberfläche anzuzeigen, z. B. "Speichern in Bearbeitung, bitte nicht ausschalten der Konsole."

### <a name="the-xbox-one-app-model-and-app-navigation"></a>Die Xbox One-app-Modell und die app-navigation

Xbox One können Benutzer schnell zwischen mehreren Anwendungen wechseln. Gut geschriebene Anwendungen können Benutzer, in denen sie während ihrer letzten Aktivität mit relevanten Kontext laden schnell aufgehört, auch wenn die app seit dem letzten heruntergefahren wurde der Benutzer darauf zugegriffen.

Xbox One Konsolen können nur eine app in der exklusive Partition zu einem Zeitpunkt ausgeführt werden. Präsentieren eine Fast-switching-Benutzeroberfläche ist erforderlich, der aktuell ausgeführten exklusive app schnell Wenn der Benutzer versucht wird, führen Sie einen anderen heruntergefahren. Wenn ein Benutzer versucht, die in einer anderen app zu wechseln, sendet das System eine Suspend-Benachrichtigung an die aktiven app. Während dieser Zeit sollten die app den entsprechenden Status speichern und zurückgeben, die von der Funktion anhalten. Das System erzwingt eine maximale Zeit 1 Sekunde für diesen Vorgang. Wenn die app nicht innerhalb einer Sekunde zurückgegeben wurde, beendet das System erzwingt die Anwendung. Apps können nicht verhindern, dass Benutzer über diese Seite verlassen, wie das Navigationsmodell für moderne Smartphones und Tablets.

Es gibt auch andere Fälle, in denen eine exklusive app angehalten wird, z. B. der Systemvariable einen im Leerlauf befindet oder niedriger Energiestatus eingeben, wenn der Benutzer den Netzschalter auf der Konsole drückt. Sobald die app angehalten wird, können sie ohne entladen vom System fortgesetzt werden. Dies ermöglicht Funktionen schnell fortsetzen. Wichtig zu wissen ist, dass angehaltene apps auch beendet oder fortgesetzt werden können. Die app muss immer Zustand speichern, für den Fall, dass er beendet wird.

Um auch mit der Xbox One-Anwendungsmodell arbeiten, apps sollte darauf vorbereitet sein Status in einen Arbeitsspeicherpuffer schnell serialisieren, sodass relevanten Zustände kann, innerhalb gespeichert werden 1 Sekunde anhalten Zeitrahmen.

Beachten Sie, dass es ein Unterschied zwischen der gespeicherten Daten über Benutzer-Spiel, und Daten zum Status der app, wie z. B. die Position innerhalb eines Menüs. Zusätzlich zum sparen Spiel, wenn die app angehalten wird, sollten Sie eine Beibehaltung des Menüstatus im, wenn der Benutzer ist in der Mitte eine Einstellung konfigurieren oder Anpassen eines Zeichens außerhalb der wichtigsten game-Engine.

Benutzer können ein Spiel für einen langen Zeitraum angehalten lassen. Erwägen Sie eine andere Umgebung geboten wird, wenn das Spiel nach einer langen Unterbrechung fortgesetzt wird. Einen Benutzer wieder an eine problemlösung aus ihrer Kampagne ablegen möglicherweise ein irritierend und unerwartetes Verhalten, wenn der Benutzer zwei Wochen wiedergegeben noch nicht, während eine Pause von einer Stunde werden häufiger und eine schnelle Rückgabe aus, um Spiele zu rechtfertigen.

Weitere Informationen über die Xbox One-app-Modell finden Sie unter den folgenden Ressourcen:

-   [Moderne Anwendung Wechsel für Wohnzimmer](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest%202012/Xfest%202012%20-%20Modern%20Application%20Switching%20for%20the%20Living%20Room.pptx), eine Präsentation auf Xfest 2012
-   [Die Shell-Benutzeroberfläche](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=PROD-D_Experience.pptx&folder=platform/xfest2013), eine Präsentation auf Xfest 2013
-   [Verarbeiten der Prozesslebensdauer-Verwaltung für Xbox One](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), ein Whitepaper GDN

> [!NOTE]
> Einige Präsentationen bei [Xfest 2012](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2012.aspx) und [Xfest 2013](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2013.aspx) enthalten Informationen, die Ankündigung nicht mehr aktuell ist, dass die Xbox One offline Play unterstützt.


### <a name="storage-options-on-xbox-one"></a>Storage-Optionen auf der Xbox One

Xbox One bietet mehrere Speicheroptionen, die jeweils über seine eigenen Vorteile und Einschränkungen. Apps müssen möglicherweise eine Kombination von Optionen je nach den Anforderungen der apps zu verwenden.


<a name="connected-storage"></a>Verbundenen Speicher
-----------------
Verbundenen Speicher wurde entwickelt, können Sie apps speichern, Xbox One Gaming-Daten und andere relevante app Zustände-Daten, die zwischen Konsolen wechseln soll. Der verbundene Speicher-API für Xbox One unterstützt Sie bei speichern und Hochladen von Daten. Die API funktioniert in Kombination mit der Xbox One-Anwendungsmodell.

Die verbundene Speicherverwaltungs-API bietet die folgenden Features:

-   Apps können schnell bis zu 16 MB an Daten zu einem Zeitpunkt in einen Arbeitsspeicherpuffer in der Systempartition speichern, die dann auf die vom System lokal zwischengespeichert und in die Cloud hochgeladen.
- Für verwaltete Partner und ID@Xbox Entwickler:
  - 256 MB pro Benutzer/die app von Cloud-Speicher.
- Für Entwickler für Xbox Live Creators-Programm:
  - 64 MB pro Benutzer/die app von Cloud-Speicher.
-   Stabile Antwort, die auf Stromausfälle – apps nicht für den Umgang mit partiellen Daten gespeichert haben.
-   Daten werden automatisch in die Cloud hochgeladen, selbst wenn die app nicht ausgeführt wird.
-   Daten werden für Xbox One-Konsolen, die für Xbox Live verbunden sind.
-   Xbox Live übernimmt geräteübergreifenden synchronisieren und Konflikte ohne Beteiligung der App.

Weitere Informationen zu der verbundenen Speicherverwaltungs-API finden Sie unter im entsprechenden Abschnitt im xblesdk.chm (das die Xbox Live-Erweiterungs-SDK-APIs dokumentiert) in das Xbox Live-SDK.


<a name="xbox-live-title-storage"></a>Xbox Live-Titel-Speicher
-----------------------
Der Title-Storage-Dienst bietet eine plattformübergreifende-REST-API für die datenspeicherung mit den folgenden Funktionen:

-   Stellt Daten für Benutzer, apps und verschiedene Plattformen freigegeben
-   Unterstützt die Binär, JSON und Konfigurationsdateien
-   Für verwaltete Partner und ID@Xbox Entwickler:
    - 256 MB pro Benutzer/die app von Cloud-Speicher
    - 256 MB pro Titel globalen Speicher
- Für Entwickler für Xbox Live Creators-Programm:
  -   64 MB pro Benutzer/die app von Cloud-Speicher
  -   256 MB pro Titel globalen Speicher

Anforderungen für die Verwendung des Diensts:

-   Xbox One-Konsole muss online ist, um Zugriff auf den Dienst sein.
-   Alle Interaktionen zwischen Dienst müssen ausgeführt werden, während die app ausgeführt wird; die Datenübertragung wird nicht automatisch im Hintergrund abgeschlossen.

Weitere Informationen finden Sie unter *Xbox Live Titel Storage*, in der Dokumentation zu xdk-Version.


<a name="local-temporary-storage"></a>Temporären lokalen Speicher
-----------------------
In der Verwaltungskonsole besitzt eine app den Zugriff auf den temporären lokalen Speicher mit den folgenden Merkmalen:

-   2 GB Speicher für dedizierte Festplatte, zugegriffen werden kann, durch den Pfad T:\\.
-   Inhalt dieses Speichers können entfernt werden, wenn die app nicht ausgeführt wird.

Weitere Informationen zu den lokalen Speicher finden Sie in der lokalen Speicher, in der Dokumentation zu xdk-Version.


<a name="configuring-your-app-for-connected-storage"></a>Konfigurieren Ihrer app für die verbundenen Speicher
------------------------------------------
Wenn Sie die verbundenen Speicherverwaltungs-API verwenden, werden alle Lese- und schreiben, dass die Vorgänge mit einer Xbox Live primären Service Configuration ID (SCID), definiert in Ihrer app Manifestdatei "appxmanifest.xml" zugeordnet sind:

```xml
      <Extensions>
        <mx:Extension Category="xbox.live">
        <mx:XboxLive TitleId="<your title ID>" PrimaryServiceConfigId="<your SCID>"
        RequireXboxLive="<boolean indicating Live requirement>" />
        </mx:Extension>
      </Extensions>
```

Weitere Informationen zu den Titel für Ihre app-ID und SCID abrufen, finden Sie unter *Einstellung von Sandkästen für die Xbox Live-Entwicklungsseite*, in der Dokumentation zu xdk-Version.



## <a name="connected-storage-system-concepts"></a>Verbundene: System-Konzepte

Dieser Abschnitt beschreibt die Komponenten von System verbundenen Speicher, ihre Beziehungen und deren ordnungsgemäße Verwendung.

### <a name="connected-storage-space"></a>Verbundene Speicherplatz

Im Allgemeinen alle Daten in das System verbundenen Speicher eines Benutzers oder eines Computers (z. B. eine einzelne Xbox One-Konsole) zugeordnet ist. Alle Daten, die von einer app für einen bestimmten Benutzer oder Computer gespeichert werden in einem verbundenen Speicherplatz gespeichert.

Jeder Benutzer Ihrer App Ruft die einem verbundenen Speicherplatz mit einem Grenzwert von 256 MB-Gesamtspeicher ab. Es ist wichtig zu beachten, dass es sich bei diesen Speicher zu Ihrer app, die allein vorgesehen ist – es ist nicht mit anderen apps freigegeben.

Ihre app verfügt außerdem 64 MB Speicherplatz in einen lokalen verbundenen Speicherplatz für den Computer an. Dieser Speicherplatz ist unabhängig von Benutzern und zugegriffen werden kann, auch wenn kein Benutzer angemeldet sind.

Zum Abrufen von eines verbundenen Speicherplatzes, die app Ruft die *ConnectedStorageSpace.GetForUserAsync-Methode* oder *ConnectedStorageSpace.GetForMachineAsync Methode*. Diese einen potenziell langwierigen Vorgang, insbesondere dann, wenn der Benutzer verfügt über Daten auf einem Gerät gespeichert und ist Spiel zum ersten Mal auf einem anderen Gerät fortsetzen. Ausführliche Informationen zu diesem Prozess und die möglichen Fehlerzustände, die auftreten können, während die app darauf wartet, einen verbundene Speicherplatz zu erwerben, finden Sie unter *Synchronisieren eines verbundenen Speicherplatzes*weiter unten in diesem Artikel.

Nachdem die app erhält ein **ConnectedStorageSpace** Objekt aufrufen, die allen Methoden in der *Windows.Storage-Namespace* welche verwenden, das Objekt oder anderen Objekten abgeleitet ist, nicht auf eine Antwort abhängen von Web Services abgeschlossen. Da auf der Xbox eine HDD nicht exklusiv für die app aktiv ist, kann nicht jedoch strenge Obergrenzen für die Leistung dieser Methoden garantiert.


### <a name="connected-storage-containers-and-blobs"></a>Verbundene: Container und Blobs

Die *verbundenen Speichercontainer*, oder *Container* kurz, wird die grundlegende speichereinheit. Jeder verbundene Speicherplatz kann zahlreiche Container enthalten, wie im folgenden Diagramm dargestellt.

**Abbildung 1.  Verbundene Speicherplatz (pro Titel/Computer oder pro-Titel/Benutzer)**

![](../../images/connected_storage/connected_storage_space_containers.png) Daten werden gespeichert, in Containern als eine oder mehr Puffer aufgerufen *Blobs*. Das folgende Diagramm veranschaulicht die systeminterne Darstellung von Containern auf dem Datenträger. Für jeden Container ist es eine Containerdatei, die Verweise auf die Datendatei für jeden Blob im Container enthält.

**Abbildung 2.  Diagramm eines Containers**

![](../../images/connected_storage/container_storage_blobs.png)

Rufen Sie zum Speichern von Daten in einem Container die *ConnectedStorageContainer.SubmitUpdatesAsync-Methode*, eine Zuordnung von Namen und die Blobs (Objekte Puffer) bereitstellen. Alle Änderungen, die in beschriebenen eine **SubmitUpdatesAsync** Aufruf atomar angewendet werden, d. h. entweder angefordert wird, werden alle Blobs aktualisiert oder für den gesamte Vorgang wird beendet und bleibt der Container im Zustand vor dem Aufruf.

Speichervorgänge, mit denen einzelne **SubmitUpdatesAsync** sind jeweils auf 16 MB Daten beschränkt.


### <a name="submitupdatesasync-behavior"></a>SubmitUpdatesAsync-Verhalten

Wenn **SubmitUpdatesAsync** wird aufgerufen, die für den Aufruf der bereitgestellten Puffer werden schnell kopiert aus der app-Partition in einer dedizierten Speicherbereich in der Systempartition. Sobald der Speicher in der Systempartition erfolgreich kopiert wurde, der Abschlusshandler bereitgestellt, der **SubmitUpdatesAsync** Aufruf erfolgt innerhalb der app, der angibt, an die app an, dass es sicher belegten Speicher freigeben lokal für die Daten.

Das System dann Blobs auf der Konsole zur Festplatte speichert und schließt den Vorgang mit einem endgültigen Container-Update, das den gesamten Vorgang für diesen Container ein Commit ausgeführt.

Besteht eine 16-MB-Obergrenze für den Speicher in freigegebenen Partition für den Empfang **SubmitUpdatesAsync** Daten. Wenn ein Aufruf von **SubmitUpdatesAsync** kann nicht direkt bearbeitet werden durch das System wahrscheinlich nicht genügend Speicher frei sein, in der dedizierten 16-MB-Puffer steht, der Aufruf wird in die Warteschlange für die Wartung. Das System kontinuierlich überträgt die Daten aus dem 16-MB-Puffer auf die Festplatte, und in der Warteschlange Updates werden verarbeitet, in der Reihenfolge, die sie angefordert wurden, sobald Platz in der 16-MB-Puffer verfügbar ist.

**Abbildung 3.  SubmitUpdatesAsync behavior**

![](../../images/connected_storage/submitupdatesasync_behavior.png) Hochladen in der Cloud erfolgt auf ähnliche Weise: Einzelne Blobs werden an den Dienst hochgeladen und der Update-Vorgang wird ein Commit ausgeführt von einem letzten Update in einer Containerdatei, die auf alle anderen hochgeladenen Blobs verweist. Hochladen in die Cloud, Dank dieser Konsolidierung in einer einzelnen und letzten Update sichergestellt, dass alle Daten in verwiesen eine **SubmitUpdatesAsync** Aufruf wird entweder in ihrer Gesamtheit ein Commit oder ein Container bleibt unverändert. Auf diese Weise auch, wenn ein System offline geschaltet wird oder während eines Vorgangs hochladen die Energieversorgung verliert, konnte ein Benutzer wechseln Sie an einem anderen Xbox One-Konsole, Herunterladen von Daten aus der Cloud, und weiterhin mit der eine konsistente Sicht der alle Container.

> [!IMPORTANT]
> Datenabhängigkeiten im Container sind nicht sicher.  Die Ergebnisse der einzelnen *SubmitUpdatesAsync* ist garantiert, dass Aufrufe vollständig oder überhaupt nicht angewendet werden.

**SubmitUpdatesAsync** Aufrufe müssen Sie nicht davon aus, die ein Future **SubmitUpdatesAsync** Aufruf wird erfolgreich abgeschlossen werden, um den Container in einem gültigen Zustand belassen. Das heißt, apps können nicht auf mehr als einem basieren **SubmitUpdatesAsync** Aufruf, um alle erforderlichen Daten in einen Container zu speichern. Jede **SubmitUpdatesAsync** Aufruf muss den Inhalt des angegebenen Containers in einem gültigen Zustand für die app später lesen lassen.

Um dieses Problem zu veranschaulichen, betrachten Sie ein Szenario, in dem ein Container für die Menge an "Gold" und Food frei, die ein Zeichen, die mit dem Namen Bob verfolgt, aus. Der Titel kann zwei Blobs, die mit dem Namen speichern *Food* und *"Gold"*. Bob beginnt mit 100 gold und keine Nahrungsmittel in seinem Bestand.

**Abbildung 4.  Beispielszenario: Bob beginnt mit 100 "Gold".**

![](../../images/connected_storage/submitupdatesasync_example_scenario1.png)

Bob verbringt jetzt 50 "Gold". Bereitet der Titel einer **SubmitUpdatesAsync** aufrufen, welche updates den Wert für den gold-Blob auf 50.

Das System zeichnet die aktualisierte Blob und die Informationen über das Container-Update auf den Puffer Updates. Klicken Sie dann kopiert das System den Wert des neuen Blob, auf die Festplatte geschrieben.

**Abbildung 5.  Das System die aktualisierte Informationen erfasst und kopiert die Werte auf der Festplatte.**

![](../../images/connected_storage/submitupdatesasync_example_scenario2.png)

Abschließend aktualisiert das System Containerdatei erstellt auf der auf das neue Blob verweisen. Schließlich entfernt das System das nicht referenzierte Blob in eine Garbage Collection-Vorgang.

**Abbildung 6.  Das System aktualisiert die Containerdatei in die HDD und entfernt nicht referenzierte Blob.**

![](../../images/connected_storage/submitupdatesasync_example_scenario3.png)

Beachten Sie, dass Sie weitere Blobs pro verwenden **SubmitUpdatesAsync** Aufruf, je mehr Zeit zum Abschließen der atomaren Operations der System-Dateivorgänge zum Speichern von Daten robust erforderlich ist. Die Granularität für die datenspeicherung im vorherigen Beispiel ist viel zu klein, aber es soll das Verhalten des atomaren Updates von mehreren Blobs in einem Container klar veranschaulichen.


### <a name="updating-multiple-blobs--the-wrong-way"></a>Aktualisieren von mehreren Blobs, die falsche Methode

Erwägen Sie ein Szenario, in dem Bob einige Lebensmittel kaufen möchte. Der Einfachheit halber werden wir sagen, dass 1 Einheit "Gold" 1 Einheit von Nahrungsmitteln kauft und Bob 25 Einheiten Lebensmittel kaufen möchte. Die app könnte eine ausgeben **SubmitUpdatesAsync** Aufruf zum Hinzufügen von 25 Einheiten von Nahrungsmitteln und einen anderen um 25 Einheiten "Gold" von der Bob subtrahieren\_Inventory-Container. Aber auch wenn die abgeschlossene Handler für beide **SubmitUpdatesAsync** Aufrufe aufgerufen wurden, besteht die Möglichkeit für die falsche Ergebnisse aufgrund von Ereignissen wie Stromausfall, die die Daten aus, die auf die Festplatte geschrieben wird, beenden kann, oder Unvollständige Synchronisierung in der Cloud. In den folgenden Diagrammen wird erläutert, welche Schritte vom System und das Ergebnis von einem Stromausfall auf die einzelnen Schritte.

Vorausgesetzt, dass die Daten aus beiden **SubmitUpdatesAsync** Aufrufe ist bereits im Puffer, Aktualisieren des Systems und dem Titel gehörigen-Abschluss-Handler für beide Aufrufe aufgerufen wurden.

Zuerst schreibt das System die Daten für den neuen Wert des BLOBs Essen, auf dem Datenträger.

**Abbildung 7.  Das System schreibt den Wert des BLOBs Food auf dem Datenträger.**

![](../../images/connected_storage/update_method_wrong_way_1.png) Als Nächstes aktualisiert das System den Container, um die neu geschriebene Wert verweisen. Wie das folgende Diagramm veranschaulicht, wenn Power nach diesem Schritt und vor dem nächsten verloren gegangen sind, würde Bob viel, erhalten Sie 25 Food erhalten, ohne die entsprechende "Gold" aus seinem Bestand abgezogen.

**Abbildung 8.  Aktualisiert den Container, um die neu geschriebene Wert verweisen.**

![](../../images/connected_storage/update_method_wrong_way_2.png)

Als Nächstes schreibt das System die Daten für den neuen Wert für den gold-Blob, auf dem Datenträger. Der Wert für "Gold" verweist auf das Bob\_Inventory-Container können immer noch nicht aktualisiert wurde, und Bob verfügt, 25 weitere "Gold" als er sollte, aber wir haben uns einen Schritt näher an das gewünschte Ergebnis.

**Abbildung 9.  Das System schreibt die Daten für den neuen Wert für den gold-Blob, auf dem Datenträger.**

![](../../images/connected_storage/update_method_wrong_way_3.png)

Abschließend aktualisiert das System die Containerdatei, um die neu geschriebene BLOBs für "Gold" verweisen, das gewünschte Ergebnis.

**Abbildung 10.  Das System aktualisiert die Containerdatei, um das neu geschriebene gold-Blob zu verweisen.**

![](../../images/connected_storage/update_method_wrong_way_4.png)

### <a name="updating-multiple-blobs--the-right-way"></a>Aktualisieren von mehreren Blobs – am besten

Ist der richtige Weg, um sicherzustellen, dass die Menge an "Gold" und Nahrungsmittel in Bobs Inventar automatisch aktualisiert, ohne Möglichkeit von falschen Zwischenzustand aufgrund einer Unterbrechung der Stromversorgung, aktualisieren Sie die beiden Blobs in einem einzelnen **SubmitUpdatesAsync** aufrufen. Das System wird dann die folgenden Schritte ausführen.

Zuerst schreibt das System die Daten für den neuen Wert des BLOBs Essen, auf dem Datenträger.

**Abbildung 11.  Das System schreibt die Daten für den neuen Wert des BLOBs Nahrungsmittel.**

![](../../images/connected_storage/update_method_right_way_1.png) Das System schreibt dann die Daten für den neuen Wert für den gold-Blob, auf dem Datenträger.

**Abbildung 12.  Das System schreibt die Daten für den neuen Wert für den gold-Blob.**

![](../../images/connected_storage/update_method_right_way_2.png) Abschließend aktualisiert das System Containerdatei erstellt beide die neuen Blobs zu verweisen.

**Abbildung 13.  Das System aktualisiert die Containerdatei, um die beiden neuen Blobs zu verweisen.**

![](../../images/connected_storage/update_method_right_way_3.png) Obwohl in diesem Beispiel sehr einfach ist, veranschaulicht es die Bedeutung der alle Änderungen an den Daten in einem Container, die atomar angewendet werden muss, durch das Ausgeben einer einzelnes **SubmitUpdatesAsync** mit allen gewünschten Updates aufrufen. Auf diese Weise für den Fall Food mit "Gold" Kauf wird die app eine mögliche Racebedingung, die nicht ordnungsgemäß aktualisiert nur einer der Werte und lassen Sie das Zeichen zu viel "Gold" vermieden.

### <a name="performance-characteristics-and-considerations"></a>Die Leistungsmerkmale und Überlegungen

Die 16 MB-Update-Puffer in der freigegebenen Partition ermöglicht, dass eine begrenzte Anzahl von updatevorgängen sehr schnell ausgeführt werden. Die Geschwindigkeit an, an der das System die Daten auf dem Datenträger beibehalten werden kann, hängt davon ab sowohl die Menge der Daten im Puffer und die Anzahl von Blobs. Da jedes Blob mit resilienz, je größer die Anzahl von Blobs in den Puffer auf den Datenträger geschrieben wird, die länger dauert es dauerhaft auf dem Datenträger gespeichert werden.

Abbildung 13 zeigt ein Beispiel für die Verarbeitungszeit für eine **SubmitUpdatesAsync** Vorgang alle zwei Sekunden mit zwei 512 KB-Blob-Aktualisierungen und einer 1024 KB blob aktualisieren, wenn keine anderen Festplatte-Aktivität auf dem System vorhanden ist. Das System kann auf einem stabilen Zustand jedes Update innerhalb von 14 – 18ms Verarbeitung ausgeführt werden.

**Abbildung 14.  Die Verarbeitungszeit für einen Vorgang SubmitUpdatesAsync, alle zwei Sekunden mit zwei 512 KB-Blob-Aktualisierungen und einen blob 1024 KB-Update und keine anderen Festplatte-Aktivität.**

![](../../images/connected_storage/submitupdatesasync_proc_time_mixed_size_fixed_interval.png) Abbildung 14 zeigt die Verarbeitungszeit für drei 1024 KB-blobs in verschiedenen Zeitintervallen.

Das System kann diese Updates in Abständen von 3 Sekunden 87ms im stabilen Zustand verarbeiten. Durch das die Häufigkeit auf einmal alle zwei Sekunden erhöhen, kann das System weiterhin Updates 87ms stabilen Zustand verarbeitet werden.

Reduzieren das Intervall auf 1 Sekunde zwischen den Updates ändert das Verhalten gleich bleibenden Status. Das System kann 60 Updates auf 87ms pro Update verarbeiten, aber jedes Update darüber hinaus dauert wesentlich länger, erreichen einen stabilen Zustand Verarbeitungszeit von 500 Millisekunden pro Update mit erheblichen Fluktuation (s). Die 16-MB-Memory-Puffer gefüllt wird schneller als die Daten können auf den Datenträger geleert wird. Updates sind gezwungen, zu warten, bis frühere Updates von geschrieben werden.

Der Effekt wird erheblich erhöht, beim Aktualisieren des Intervalls auf eine Update pro Sekunde 0,5. Das System kann verarbeiten nur 7-Updates in diesem Intervall, erneut am 87ms pro Update, vor dem Erreichen eines stabilen Status, in dem jedes Update mehr als 1 Sekunde verarbeitet werden, mit sehr hohen Variationen in Anspruch.

**Abbildung 15.  Die Verarbeitungszeit von drei 1024 KB-blobs in verschiedenen Zeitintervallen.**

![](../../images/connected_storage/submitupdatesasync_proc_time_fixed_size_various_intervals.png) Dies sind nur Beispiele zur Veranschaulichung. Ihre app in der Regel sollte nicht werden speichern Daten häufig, aber es auch wird nicht in der Regel werden Betrieb in einer Umgebung, frei von Datenträger-e/a.

Es ist wichtig zu verstehen, die Merkmale des Systems basierend auf diesen Beispielen – zum Messen der app unter verschiedenen Bedingungen ausgeführt, um sicherzustellen, dass Ihre speichern, die Vorgänge können in weniger als 1 Sekunde, während Ihre app Ausführen der Ereignishandler angehalten.


## <a name="synchronizing-a-connected-storage-space"></a>Synchronisieren von einem verbundenen Speicherplatz

-   Überprüfung der Netzwerkkonnektivität
-   Sperrenübernahme
-   Container auflisten Ausdrücke, Vergleichsausdrücke und Fusion-Logik
-   Container-download

Wenn Ihre app den Zugriff auf ein Leerzeichen verbundenen Speicher anfordert, führt das System einen Synchronisierung Prozess um gespeicherten Daten des Benutzers auf eine Xbox-Konsolen in einem konsistenten Zustand zu halten und seine Daten für offline-Wiedergabe verfügbar zu machen. Da synchronisieren unterschiedlich lange dauern kann und möglicherweise der Benutzer, Entscheidungen zu treffen muss, kann das System in verschiedenen Phasen des Prozesses Benutzeroberfläche für den Benutzer anzuzeigen.

Der Benutzer kann von Ihrer app navigieren, durch Drücken der Schaltfläche "Xbox" zu jedem Zeitpunkt, auch wenn die Synchronisierung Benutzeroberfläche aktiv ist. Das System wird die Benutzeroberfläche ausgeblendet, und die datensynchronisierung ausgeführt wird, soweit möglich ohne Eingreifen des Benutzers. Wenn der Benutzer zurück an die app navigiert, wird die Benutzeroberfläche wieder angezeigt, es sei denn, die Synchronisierung abgeschlossen ist. Wenn die Benutzeroberfläche ausgeblendet wird, führt das System noch nie eine Annahme zur Auswahl des Benutzers.

Da das System zeigt keine Synchronisierung Benutzeroberfläche an, wenn der Benutzer auf der Startseite und Rendering der app weiterhin in der großen App-Kachel angezeigt wird, es ist wichtig, dass die app angemessen visuelle Elemente beim Rendern einer **GetForUserAsync** Aufruf abgeschlossen ist. Die kontinuierliche Wiedergabe gibt für den Benutzer, dass die app weiterhin interaktiv wird, und darauf, dass Daten wartet zu laden.

Das folgende Diagramm zeigt die allgemeine Sequenz, die das System folgt, wenn eine app über einen verbundenen Speicherplatz anfordert. Wenn die gesamte Sequenz mehr als ein paar Sekunden dauert, wird die Synchronisierung vom System gezeichnet Benutzeroberfläche angezeigt.

**Abbildung 16.  Die Sequenz gefolgt von dem System, wenn eine app verbundene Speicher anfordert.**

![](../../images/connected_storage/app_requests_connected_storage_space.png) Das System durchläuft die folgenden Phasen aus, wenn Dienste eine **GetForUserAsync** Anforderung:

-   Überprüfung der Netzwerkkonnektivität
-   Sperrenübernahme
-   Container auflisten Ausdrücke, Vergleichsausdrücke und Fusion-Logik
-   Container-download

### <a name="connectivity-check"></a>Überprüfung der Netzwerkkonnektivität

Starten der Wartung einer **GetForUserAsync** anfordern, überprüft das System für die Konnektivität. Wenn die Konsole offline ist, der gesamten Synchronisierungsprozess wird übersprungen, und der verbundenen Speicherplatz für den angegebenen Benutzer ist für die aktuelle Sitzung als offline markiert. Alle geänderten Daten werden mit dem cloudspeicher abgestimmt beim nächsten Zugriff von Ihrer app verbundene Speicherplatz des gleichen Benutzers und das System kann den Titel Storage-Dienst erreichen. Keine Benutzeroberfläche wird in diesem Fall immer angezeigt.

Weitere Informationen zur Behandlung von offline außerhalb des Kontexts der verbundenen Speicher, finden Sie unter *Service Unterbrechung Flexibilität in Bezug auf eine Xbox-Titel*.

### <a name="lock-acquisition"></a>Sperrenübernahme

Das System versucht, nach dem Überprüfen der Konnektivität, exklusiven Zugriff auf die Cloud-Speicher Ihrer app und den aktuellen Benutzer zugeordnet. Dies erfolgt durch Ablegen der Sperrdatei im Bereich verbundenen Speicher Ihres Speichers Titel. Wenn die Konsole online ist, des Diensts erreichen und kann in kurzer Zeit die Sperre abzurufen, wird keine Benutzeroberfläche angezeigt, und der Synchronisierungsvorgang wird fortgesetzt.

Sobald das System hat eine Sperre für einen bestimmten verbundenen Speicherplatz abgerufen und an die app eine Instanz von einem verbundenen Speicherplatz zurückgegeben wird, ruft keines Ihrer app-API arbeiten mit Daten in, verbundene Speicherplatz bei erfolgreicher Anforderungen blockiert werden. Die Sperre enthält ausreichenden Schutz, selbst wenn ein Benutzer, trennen das Netzwerkkabel aus dem System, nachdem Ihre app einen verbundenen Speicherplatz erworben hat, die API-Aufrufe basierend auf der lokal verfügbaren Daten eingesetzt werden.

Es gibt einige mögliche Szenarien bei der Übernahme gleichtakt:

 **Synchronisieren die Benutzeroberfläche** "Synchronisieren" Benutzeroberfläche wird angezeigt, wenn die Konsole online ist, aber es hat nicht die Sperre aus dem Dienst innerhalb eines kurzen Zeitraums abgerufen,.

 **Unterbrechen die Sperre** , wenn der Benutzer auf einer anderen Konsole die app gespielt haben, seit er zuletzt auf dem aktuellen Objekt wiedergegeben, ist es möglich, dass die andere Konsole exklusiven Zugriff auf den Speicherplatz hat und in der Mitte Hochladen von Daten. Es ist auch möglich, dass eine andere Konsole Hochladen von Daten gestartet wurde, jedoch hat vor dem Beenden der Verbindung oder zur energiesteuerung verloren.

Beiden Fällen werden als bezeichnet *Sperrenkonflikte*, und in jedem Fall stellt das System-Benutzeroberfläche, um zu erklären, dass eine andere Konsole Daten hochlädt. Der Benutzer kann warten, bis dieser Prozess abgeschlossen oder mit den Daten, die derzeit verfügbaren, in der Cloud arbeiten. Wenn der Benutzer zum Arbeiten mit clouddaten, nimmt das System die Sperre für sich selbst (unterbricht die Sperre) exklusiven Zugriff auf den Cloud-Speicher für den Benutzer und die app zu erwerben. Das Hochladen über die andere Konsole wird abgebrochen, und der Synchronisierungsvorgang wird fortgesetzt.

### <a name="container-listing-comparison-and-merger-logic"></a>Container auflisten Ausdrücke, Vergleichsausdrücke und Fusion-Logik

Nach dem Erwerb einer Sperre von, fordert das System eine Liste aller Container in der Cloud für den bestimmten app und Benutzer. Klicken Sie dann den Inhalt der lokalen Festplatte mit den Daten in der Cloud verglichen und entsprechend den Ergebnissen des Vergleichs wird fortgesetzt:

 **Lokale Daten entspricht die Cloud** Wenn keine Änderungen gegenüber anderen Konsolen stattgefunden haben, und die Daten in der Cloud und lokalen festen Laufwerk entspricht, und klicken Sie dann die Synchronisierung ist abgeschlossen haben, den Abschlusshandler der der **GetForUserAsync**Aufruf zu diesem Zeitpunkt aufgerufen werden soll, und Ihre app mit dem lädt und speichert fortfahren kann.

 **Keine lokalen Daten** Wenn die Cloud Daten enthält, aber die lokale Konsole verfügt nicht über alle, werden die Daten aus der Cloud lokal heruntergeladen. Dies kann beispielsweise auftreten, wenn der Benutzer zum ersten Mal bei der Freunden wiedergegeben wird.

 **Dieselben Container, die geändert wird, lokal und in der Cloud** , wenn der Benutzer Container in der Cloud geändert wurde, indem Sie auf einer anderen Konsole und die gleichen Container geändert werden, wurde bei Verwendung die aktuelle Konsole offline, die Daten können nicht automatisch zusammengeführt werden. Der Benutzer wird aufgefordert werden, auf welche Daten beibehalten werden sollen. Im Fall von Konflikten kann der Benutzer eine Richtlinie für den Austausch auswählen: Der lokale oder clouddaten werden immer beibehalten oder der Benutzer auswählen kann **Abbrechen** und die Wahl. Wenn der Benutzer der Cloud oder lokalen Daten als eine Richtlinie für den Austausch, Container mit dem gleichen Namen, aber mit unterschiedlichem Inhalt – werden entsprechend aufgelöst werden.

Wenn der Benutzer auswählt **Abbrechen**, der Titel haben Zugriff auf das Speichern System in einen nicht aufgelösten versetzt, als ob der Benutzer offline Wiedergabe wurden. In diesem Fall wird die Auflösung des Konflikts zwischen Benutzeroberfläche erneut das nächste Mal die app den Zugriff auf den verbundenen Speicherplatz fordert angezeigt, wenn die Konsole online ist.

### <a name="container-download"></a>Container-download

Sobald alle Konflikte gelöst wurden, verfügt das System alle Informationen, die erforderlich sind, um zu identifizieren, welche Container aus der Cloud heruntergeladen werden müssen. Alle erforderlichen Container heruntergeladen werden den Abschlusshandler der der *ConnectedStorageSpace.GetForUserAsync Methode* zu diesem Zeitpunkt aufgerufen werden, und Ihre app mit dem lädt und speichert fortfahren kann.

Einige mögliche Fehler während dieses Schritts:

**Nicht genügend lokaler Speicherplatz**  
Im Fall von nicht genügend lokale Festplatten-Speicherplatz für die erforderlichen Container werden Benutzer mit der Benutzeroberfläche angezeigt auffordert, um Speicherplatz freizugeben, durch das Entfernen der lokal gespeicherte Daten. Damit können sie endgültig löschen wichtige Daten, die in der Cloud gesichert ist nicht zu vermeiden, gibt die Benutzeroberfläche deutlich an Daten, die einfach lokale Cache ist und Daten, die für die aktuelle Konsole eindeutig ist.

Wenn die Benutzeroberfläche für den Benutzer angezeigt wird:

-   Wenn der Benutzer über genügend Speicherplatz freigegeben, die Synchronisierung fortgesetzt, und Sie werden abgeschlossen ist.
-   Wenn der Benutzer aus der UI abbricht, ohne über genügend Speicherplatz, den Abschlusshandler der Freigabe der **GetForUserAsync** aufrufen, gibt **OutOfLocalStorage**– finden Sie unter *ConnectedStorageErrorStatus Enumeration*. Die app sollte bestätigen Sie, dass der Benutzer die sitzungsverbindung wiedergegeben werden, ohne die Möglichkeit zum Speichern von Daten. Wenn der Benutzer zustimmt, sollten die app ohne Speichern von Daten für diesen Benutzer fortfahren. Wenn der Benutzer darauf hinweist, möchte, dass er Daten während der Wiedergabe speichern, die app wiederholt werden soll die **GetForUserAsync** aufrufen, die zeigt dann der Benutzeroberflächenautomatisierungs, um Speicherplatz freizugeben.

**Synchronisierung abgebrochen wird**  
Wenn der Benutzer möchte nicht warten, bis die Synchronisierung abgeschlossen, und wählt Abbrechen, wird der Benutzer darüber informiert, dass nicht alle von der gespeicherten Daten zur Verfügung stehen. Den Abschlusshandler der der **GetForUserAsync** Aufruf zu diesem Zeitpunkt aufgerufen werden, und die app mit dem lädt und speichert fortfahren kann.

**Netzwerktimeout**  
Wenn die Daten für den Timeout aufgrund eines Problems mit der Netzwerkverbindung oder der Verfügbarkeit des Diensts herunterladen, erhält der Benutzer die Möglichkeit, die Synchronisierung zu wiederholen. Wenn er nicht auswählt, wird der Benutzer darüber informiert, dass nicht alle von der gespeicherten Daten zur Verfügung stehen. Den Abschlusshandler der der **GetForUserAsync** Aufruf zu diesem Zeitpunkt aufgerufen werden, und die app mit dem lädt und speichert fortfahren kann.

## <a name="development-tools"></a>Entwicklungstools

Zwei Tools helfen Ihnen bei der Entwicklung von Ihrer app mit verbundenen Speicher: XbStorage und Fiddler.

### <a name="managing-connected-storage-with-xbstorage"></a>Verwalten von verbundenen Speicher mit XbStorage

XbStorage ist ein Entwicklungstool, die es ermöglicht, lokale verbundenen Speicherdaten auf eine Xbox One Development-Kits von einem PC verwalten.

Das Tool ermöglicht das Löschen von lokalen verbundenen Speicherplätze von der Festplatte, als auch importieren und Exportieren von Speicherplätzen einzelne Benutzer oder Computer-verbundene mithilfe von XML-Dateien.

Wenn ein Vorgang auf einem lokalen verbundenen Speicherplatz ausgeführt wird, verhält sich das System so, als ob dieser Vorgang von der app selbst über ausgeführt worden wären. Kopieren der Daten aus einer verbundenen Speicher in eine lokale Datei führt die Synchronisierung mit der Cloud vor dem Kopieren.

Kopieren von Daten aus einer XML-Datei auf dem Entwicklungs-PC in einem verbundenen Speicher-Container unter dem Xbox One Dev-Kit wird, die Konsole zu starten, die Daten in die Cloud hochladen. Es gibt eine Ausnahme: Wenn das Dev-Kit die Sperre abrufen kann oder ein Konflikt zwischen den Containern in der Konsole und den in der Cloud vorliegt. In diesem Fall die-Konsole verhält sich wie der Benutzer nicht zum Auflösen des Konflikts beschlossen hatten-z. B. durch Auswählen einer Version des Containers zu – und die-Konsole verhält sich wie offline bis zum nächsten Abspielen wurden der Titel wird gestartet.

Der Befehl zum Zurücksetzen des XbStorage löscht den lokalen Speicher an alle SCIDs und Benutzer Daten er ändert jedoch nicht in der Cloud gespeicherten Daten. Dies ist nützlich für das Festlegen einer Konsole mit dem Zustand, dem es in wäre, wenn ein Benutzer auf eine Konsole roaming und Herunterladen von Daten aus der Cloud bei der Wiedergabe eines Titels wurden.

Weitere Informationen zu XbStorage, finden Sie unter *verbundenen Speicher verwalten (xbstorage.exe)*, in der Dokumentation zu xdk-Version.

### <a name="monitoring-connected-storage-network-activity-using-fiddler"></a>Überwachen der verbundenen Speicher Netzwerkaktivität mit Fiddler

Es kann hilfreich sein, zu bestimmen, ob die Konsole mit dem Dienst interagiert, wenn Cloud-Speicher-Vorgänge ausgeführt werden. Mit Fiddler können Sie herausfinden, ob die Konsole Aufrufe an den Dienst erfolgreich durchführt oder es-Autorisierungsfehler aufgetreten ist. Weitere Informationen zum Einrichten von Fiddler auf eine Xbox One, finden Sie unter *Verwendung von Fiddler mit Xbox One*, in dieser Dokumentation xdk-Version.

## <a name="resources"></a>Ressourcen

Zusätzlich zu den Ressourcen, die über vorgeschlagen werden können die folgenden hilfreich bei der Entwicklung Ihrer app oder Titel sein:

-   Verbundene Speicherübersicht, in der Dokumentation zu xdk-Version
-   [Verarbeiten Sie die Verwaltung der Lebensdauer](https://developer.xboxlive.com/_layouts/xna/download.ashx?file=ProcessLifetimeManagement_08_2013_qfe5.zip&folder=platform/aug2013xdk_qfe5/samples), ein Beispiel zur Verfügung, in Beispielen auf der Game Developer Network (GDN)
-   ["Prozess (Lifetime Management, PLM) für Xbox One"](https://developer.xboxlive.com/en-us/platform/development/education/Documents/PLM%20for%20Xbox%20One.aspx), ein Whitepaper von Whitepapers zu GDN verfügbar
