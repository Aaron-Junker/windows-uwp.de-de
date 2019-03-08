---
title: Übersicht über die verbundenen Speicher
description: Informationen Sie zur Verwendung von verbundenen Speicher zum Speichern und Laden Spieledaten auf Geräten.
ms.assetid: a0bacf59-120a-4ffc-85e1-fbeec5db1308
ms.date: 02/27/2018
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox, verbundenen Speicher
ms.localizationpriority: medium
ms.openlocfilehash: 40ad13e46e074154d72d7aad236747c3374110ef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639055"
---
# <a name="connected-storage"></a>Onlinespeicher
Verbundenen Speicher wurde entwickelt, können Sie Ihre Titel zum Speichern von Gaming-Daten und andere relevante Daten, die zwischen Geräten wechseln soll. Die verbundene Speicherverwaltungs-API können Titel für Xbox One und universelle Windows-Platform(UWP) zu speichern, laden und Löschen von Daten zu Softwaretiteln, die lokal gespeichert und auch mit der Cloud synchronisiert werden, wenn der Xbox One oder UWP-Titel mit dem Internet verbunden ist. Gespeicherte Daten werden auf jedem anderen Gerät verfügbar sein, die den Titel ausgeführt wird, nach der Synchronisierung. Entwicklern wird empfohlen, die Title-Status speichern so genau wie möglich zu bieten, dass die beste nicht Zuhause Erfahrung spielen. Verbundenen Speicher kann Sie in einem Spiel zu Hause fortgesetzt, und wählen Sie Ihr Spiel Recht, wo Sie aufgehört auf anderen Geräten, die dasselbe Spiel unterstützt.

## <a name="connected-storage-features"></a>Verbundene Speicherfunktionen

Die verbundene Speicherverwaltungs-API bietet die folgenden Features:

- Apps können schnell bis zu 16 MB an Daten zu einem Zeitpunkt in einen Arbeitsspeicherpuffer speichern, die dann auf die vom System lokal zwischengespeichert und in die Cloud hochgeladen.
- Für verwaltete Partner und ID@Xbox Entwickler:
    - 256 MB pro Benutzer/die app von Cloud-Speicher.
- Für Entwickler für Xbox Live Creators-Programm:
    - 64 MB pro Benutzer/die app von Cloud-Speicher.
- Stabile Antwort, die auf Stromausfälle – apps nicht für den Umgang mit partiellen Daten gespeichert haben.
- Daten werden automatisch in die Cloud hochgeladen, selbst wenn die app nicht ausgeführt wird.
- Daten werden für Xbox One oder UWP-Geräte, die für Xbox Live verbunden sind.
- Xbox Live übernimmt geräteübergreifenden synchronisieren und Konflikte ohne Beteiligung der App.

Das System verbundenen Speicher wird sichergestellt, dass es sich bei alle Speichervorgänge vollständig oder überhaupt nicht vorgenommen werden. Dies bedeutet, dass als Entwickler Sie ihn nie benötigen kümmern teilweise gespeicherten Daten, die Auswirkungen auf Ihre Spielzustands bei einem Stromausfall oder der Benutzer, die die Anwendung plötzlich schließen entweder manuell oder durch eine andere app/Spiel in der Konsole zu öffnen. Außerdem wird dadurch ein glatter Spiel spielen Erfahrung für Benutzer von Ihrem Titel aus, wie Speicher verbunden ist ein wichtiger Bestandteil der ermöglicht Benutzern den Wechsel zwischen zwei und apps schnell und kostenlos, aber dennoch wieder auf das ursprüngliche Spiel in den Zustand, in dem sie beibehalten. Um diese Features in Ihren eigenen Titel zu implementieren, müssen Sie einen Überblick über die Speicher-APIs verbunden haben.

## <a name="connected-storage-structure"></a>Verbundene Speicherstruktur

Die verbundenen Speichersystem kann apps, die zum Speichern von Daten als ein oder mehrere Blobs im Container. Klicken Sie auf einer hohen Ebene alle Daten aus dem Speicher verbunden mit einem Benutzer verknüpft ist, oder einen Benutzer oder bei Xbox Development Kit-Computer entwickelt Titel. Alle Daten gespeichert werden, indem Sie eine app für einen bestimmten Benutzer oder Computer in einem verbundenen Speicherplatz gespeichert ist. Jeder Benutzer Ihrer App Ruft eine verbundene Speicherplatz mit einem Grenzwert von 256 oder 64 MB Gesamtspeicher ab. Es ist wichtig zu beachten, dass dieser Speicher wird dediziert für Ihre app, die allein, es ist nicht mit anderen apps freigegeben.

### <a name="containers-and-blobs"></a>Container und blobs

Die Verbindung von Storage-Container oder Container kurz, wird die grundlegende speichereinheit. Jeder verbundene Speicherplatz kann zahlreiche Container enthalten, wie im folgenden Diagramm dargestellt.

Verbundene Speicherplatz (pro Titel/Computer oder pro-Titel/Benutzer)

![connected_storage_space_containers.png](../../images/connected_storage/connected_storage_space_containers.png)

 Daten werden als eine oder mehrere Puffer wird aufgerufen, Blobs im Container gespeichert. Das folgende Diagramm veranschaulicht die systeminterne Darstellung von Containern auf dem Datenträger. Für jeden Container ist es eine Containerdatei, die Verweise auf die Datendatei für jeden Blob im Container enthält.

Diagramm eines Containers

![container_storage_blobs.png](../../images/connected_storage/container_storage_blobs.png)

Rufen Sie die entsprechende APIs-Container-Methode SubmitUpdatesAsync, eine Zuordnung von Namen und die Blobs (Objekte Puffer) bereitstellen, um Daten in einem Container zu speichern. Alle Änderungen, die in einem Aufruf SubmitUpdatesAsync beschrieben werden automatisch angewendet, also entweder angefordert wird, werden alle Blobs aktualisiert oder der gesamte Vorgang wird beendet und bleibt der Container im Zustand vor dem Aufruf.

Einzelne Speichervorgänge, mit denen SubmitUpdatesAsync sind jeweils auf 16 MB Daten beschränkt.

## <a name="connected-storage-api"></a>Verbundene Speicher-API

Verbundenen Speicher verfügt über separate APIs für die xdk-Version und UWP-apps. Glücklicherweise entsprechen diese APIs miteinander eng. Die folgenden beiden APIs unterscheiden sich hauptsächlich in deren Namen Namespace- und Klassennamen. Die Funktionen erforderlich, um [speichern](connected-storage-saving.md), [laden](connected-storage-loading.md), und [löschen](connected-storage-deleting.md) Daten mit der API sind identisch mit dem Namen.

Weitere Unterschiede zwischen den beiden verbundenen Speicher-APIs finden Sie im Abschnitt verbundenen Speicher [Portieren von Xbox Live-Code aus xdk-Version für die UWP](../../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md).

Finden Sie, dass der xdk-Version verbunden Storage-APIs in der xdk-Version CHM-Datei unter dem Pfad dokumentiert: **Xbox eine xdk-Version >>-API-Referenz >> Plattform-API-Referenz >> System-API-Referenz >> Windows.Xbox.Storage**.
Darüber hinaus sind die APIs xdk-Version auf dokumentiert die [developer.microsoft.com Site](https://developer.microsoft.com/en-us/games/xbox/docs/xdk/storage-xbox-microsoft-n).
Der Link zum xdk-Version-APIs benötigen Sie ein Microsoft-Account(MSA), die für Xbox-Entwickler Kit(XDK) Zugriff aktiviert wurde.
Windows.Xbox.Storage ist der Name des Namespaces verbundenen Speicher für Xbox One-Konsolen.

Sie finden die UWP verbundenen Speicher-APIs finden Sie unter die Xbox Live SDK-CHM-Datei unter dem Pfad: **Xbox Live-APIs >> Xbox Live-Plattform Erweiterungen SDK-API-Referenz >> Windows.Gaming.XboxLive.Storage**.
Die UWP verbundenen Speicher-APIs sind auch online unter dokumentiert die [Windows.Gaming.XboxLive.Storage Namespaceverweis](https://docs.microsoft.com/en-us/uwp/api/windows.gaming.xboxlive.storage).
Windows.Gaming.XboxLive.Storage ist der Name des verbundenen Speicher Namespaces für UWP-apps.

Erste Schritte mit verbundenen Speicher Sie einen verbundenen Speicher abrufen müssen *Speicherplatz*. Speicherplatz verbunden mit einem Benutzer oder Computer verknüpft ist, und enthält alle verbundenen Speicher Daten für diesen Benutzer oder Computer in der Form verknüpft *Container* und *Blobs*. Erwerben einen verbundenen Speicherplatz für einen Computer oder Benutzer erhalten Sie Zugriff auf Lese- und Schreibberechtigungen für diese, die gespeicherten Entitätsdaten. Zum Abrufen eines Speicherplatzes mit verbundenen Titel xdk-Version und die UWP Ruft die `GetForUserAsync` Methode xdk-Version Titel ruft möglicherweise auch die `GetForMachineAsync` -Methode, UWP Titel kann nicht aufgerufen `GetForMachineAsync`. `GetForUserAsync` und `GetForMachineAsync` befinden sich die `ConnectedStorageSpace` Klasse in der xdk-Version. `GetForUserAsync` ist Bestandteil der `GameSaveProvider` Klasse in der UWP-API. Diese Methoden sind potenziell langer Vorgänge, insbesondere dann, wenn der Benutzer verfügt über Daten auf einem Gerät gespeichert und ist Spiel zum ersten Mal auf einem anderen Gerät fortsetzen. `GetForUserAsync` Ruft ab einen verbundenen Speicherplatz für einen Benutzer, das Sie dann zum Erstellen, Zugriff auf und Löschen von Containern verwenden können.

Aufrufen, um einen Container erstellen oder Zugriff auf einen zuvor erstellten Container, der `CreateContainer` Funktion Ihrer `ConnectedStorageSpace` oder `GameSaveProvider` -Klasse, dadurch erhalten Sie Zugriff auf ein benannter Container für den Benutzer oder Computer für die `ConnectedStorageSpace` oder `GameSaveProvider`verwendet, um den Container zu erstellen. Die `ConnectedStorageSpace` und `GameSaveProvider` Klassen auch umfassen die `DeleteContainerAsync` Funktion, die Sie zum Löschen eines Containers kann bereitgestellt, Sie geben Sie den Namen des Containers gelöscht werden soll.

Zum Aktualisieren von Blobs im Container Aufruf `SubmitUpdatesAsync` in die `ConnectedStorageContainer` Klasse in der xdk-Version oder in der `GameSaveContainer` Klasse von der UWP-API. `SubmitUpdatesAsync` ermöglicht es Ihnen, geben Sie eine Liste der Namen und die Puffer als Daten in das Blob geschrieben werden speichern eine Liste der Namen der zu löschenden Blobs sowie einen Anzeigenamen für die aufrufende Container. Dies ist die Funktion, die Sie letztendlich aufrufen, zum Aktualisieren von Daten in Ihrem verbundenen Speicher müssen.

Um Beispiele für die verbundene Speicher-APIs verwendet, finden Sie unter den folgenden Artikeln Speicher verbunden: [Speichern von Daten](connected-storage-saving.md)
[Laden von Daten](connected-storage-loading.md)
[Löschen von Daten](connected-storage-deleting.md)

> [!NOTE]
> Hinweis zur Sicherheit:
>
> Universelle Windows-Plattform (UWP) apps auf PCs speichern lokale Daten an einem Ort, die für den lokalen Benutzer zugänglich ist, und ist nicht grundsätzlich Benutzer vor Manipulationen geschützt.
>Sollten Sie erwägen, Anwenden von Ihren eigenen verschlüsselungs- und Gültigkeit Spiel speichern Daten überprüft werden, um zu verhindern, dass Benutzer das Spiel speichert außerhalb des Spiels zu ändern.
>Dem gleichen Grund sollten Sie entscheiden, wenn Sie zulassen möchten, PC und Xbox-Versionen Ihres Spiels speichert oder voneinander getrennt halten.

## <a name="managing-local-storage"></a>Verwalten von lokalem Speicher

Wenn verbundene Speicher in Ihrer Xbox Development Kit oder UWP-app zu testen, müssen Sie auf Ihrem Gerät Entwicklung lokal gespeicherten Daten zu bearbeiten.

Das Tool Xbstorage wird mit der xdk-Version geliefert und ist ein Befehlszeilentool, das Sie lokalen Speicher auf Ihrem entwicklungskonsole bearbeiten kann.
Für UWP-Entwickler besteht ein identische Tool für die PC-namens gamesaveutil.exe der im Lieferumfang von Windows 10-SDKS für den Fall Creators Update(10.0.16299.91) und höher zur Verfügung.

Beide Tools können Sie lokalen Speicher auf dem Gerät mit den folgenden Befehlen ändern:

|Befehl  |Beschreibung  |
|---------|---------|
|Zurücksetzen    | Führt eine Zurücksetzung auf dem Speicher verbunden. |
|Importieren   | Importiert Daten aus der angegebenen XML-Datei in einem verbundenen Speicher. |
|Exportieren   | Exportiert Daten aus einer verbundenen Speicher, in der angegebenen XML-Datei. |
|löschen   | Löscht Daten aus einem verbundenen Speicherplatz. |
|Generieren | Dummydaten generiert und speichert in der angegebenen XML-Datei. |
|Simulieren | Storage Speicherplatzmangel simuliert. |

Weitere Informationen zu den Funktionen, die den Xbstorage-Tool und gamesaveutils.exe lesen [Verwalten von lokalem Speicher mit verbundenen](connected-storage-xb-storage.md).

## <a name="technical-overview"></a>Technische Übersicht

Zu einem mehr Tiefe Betrachtung der Funktionsweise von verbundenen Speicher in der xdk-Version an, lesen Sie die [verbunden technische Übersicht über Storage](connected-storage-technical-overview.md). Die technische Übersicht über wurde speziell für Entwickler von xdk-Version geschrieben, aber die Konzepte, die in enthaltenen gelten im Allgemeinen in den Speicher verbunden.