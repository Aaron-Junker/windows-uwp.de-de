---
title: Speicherkonfiguration im Partner Center-Titel
description: Erfahren Sie, wie Sie Titel Speicher im Partner Center zu konfigurieren
ms.date: 04/24/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, die Title-Speicher, die Partner Center
ms.openlocfilehash: d32f9f2f4e003db50ad560acc513511a850d43b5
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612095"
---
# <a name="configure-storage-for-you-title-in-partner-center"></a>Konfigurieren des Speichers für Sie Titel im Partner Center

Xbox Live, können Sie Ihr Spiel in der Cloud über den Titel Storage-Dienst zugeordnete Daten zu speichern. Seite für die Konfiguration der Title-Speicher können Sie bestimmen, welche Arten von cloudspeicherdiensten Ihr Spiel jedoch zulassen, sowie Hochladen von Dateien, die für den globalen Speicher verwendet werden.

Finden Sie auf die Konfigurationsseite für die Xbox Live Titel Storage [Partner](https://partner.microsoft.com/dashboard), wählen Ihre app aus **Übersicht über die** oder **Produkte**, öffnen die  **Dienste** Drop nach unten, und wählen **Xbox Live**. Entwickler, in dem Creators-Programm benötigen, klicken Sie auf **Optionen anzeigen** in die **Cloud speichert und-Speicher** Abschnitt ihrer Configuration-Seite, um die Title-Speicher-Konfigurationsoptionen finden Sie unter. Mit dem vollständigen Satz der Xbox Live-Funktionen müssen finden Sie die **Titel Storage** Link, auf der Konfigurationsseite für die Title-Speicher zu navigieren.

Titel-Speicherkonfiguration verfügt über zwei Hauptabschnitte. Die Title-Speicher-Einstellungsabschnitt und globalen Speicher-Abschnitt für die Verwaltung der Datei.

## <a name="section-1-title-storage-settings"></a>Abschnitt 1: Titel-speichereinstellungen

![Screenshot der Storage-Einstellungen des Titels](../../images/dev-center/title-storage/title-storage-settings.JPG)

In diesem Abschnitt können Sie alle vier Speichertypen durch Aktivieren des Kontrollkästchens in Aktivieren der **Active** Spalte. Nach dem Auswählen des Titels Sie Speichertypen ist die Wahl zwischen vom Lesen der Daten an den Spieler, der ihn besitzt eingeschränkt wird, indem Sie auf die Storage-Zeilentyp **Besitzer** nur Optionsfeld aus, oder durch Klicken auf die öffentlichfreigegebenen**Jeder** Optionsfeld. Bei Auswahl von **Besitzer nur** für einen bestimmten **Speichertyp** dann die Title-Storage-Daten dieses Typs nur von der Spieler gelesen werden, die Daten generiert hat. Bei Auswahl von **jeder** für einen bestimmten **Speichertyp** dann die Title-Storage-Daten dieses Typs ist für alle Spieler lesbar. Schreiben oder Ändern von gespeicherten Daten ist nur verfügbar, den Benutzer, die sie in allen Fällen generiert.

## <a name="storage-types"></a>Speichertypen

Es gibt vier Speichertypen, die auf der Konfigurationsseite für die Title-Speicher aktiviert werden kann. Sie finden eine Beschreibung der einzelnen Speichertypen mit dem Mauszeiger auf das Symbol "Info" neben dem Namen der einzelnen Speichertypen oder Lesen in der folgenden Tabelle.

|Speichertyp |Beschreibung |Beispiel für die Verwendung  |
|---------|---------|---------|
|Global             |Upload der Daten zum Partner Center, die von einem beliebigen Gerät gelesen werden können, und für jeden Benutzer zugänglich ist. Kann nur auf von der Developer-Uploads an Partner Center geschrieben werden. | Ankündigen von Updates für alle Benutzer über die im Spiel-Newsfeed.     |
|Onlinespeicher  |Ermöglicht das Hintergrund die Synchronisierung der Spieledaten auf XboxOne und Windows 10-Spiele. Ein stabile Fault tolerant Spiel Service zu speichern. Von jedem Gerät gelesen Sie werden können, können von Xbox One und Windows 10-Geräten geschrieben Sie werden    | Speichern Sie Dateien für einen einzelnen Benutzer zum Play auf einer separaten-Konsole zu ermöglichen.         |
|Universelle          |Netzwerk zugegriffen werden kann Blob-Speicher, ermöglicht den Zugriff auf beliebigen Geräten Schreib-/, die keine Windows Phone und Xbox 360 ist. Kann von Android und IOS-Geräten gelesen werden.      | Speichern Sie bei oder andere Statistiken von mehreren Windows-Geräten zugegriffen werden.        |
|Vertrauenswürdige            |Netzwerk zugegriffen werden kann Blob-Speicher, die nur von Xbox One, Xbox 360 und Windows Phone geschrieben werden kann. Kann von jedem Gerät gelesen werden. Können von Android und IOS gelesen werden.     | Speichern des Spielers Rangfolge in Multiplayer-Spiele.        |

## <a name="section-2-global-storage-file-management"></a>Abschnitt 2: Globaler Speicher dateiverwaltung

![Screenshot der globalen Speicher-management](../../images/dev-center/title-storage/global-storage-file-management.JPG)

Um zu sehen, die vollständige Verwaltung der globalen Speicher-Datei "Optionen" Sie müssen klicken Sie auf die **Optionen anzeigen** Dropdown-Liste. In diesem Abschnitt können Sie die Dateien und Ordnern, die aufgerufen werden können hinzufügen Wenn die **Global** Speichertyp in der Title-Einstellungen auf aktiv festgelegt ist. Ihr Spiel müssen veröffentlicht werden, für das Testen, um Dateien in diesem Abschnitt hinzufügen. Eine Warnung am oberen Rand der Konfigurationsseite für die Title-Speicher möglicherweise angezeigt werden, wenn Ihr Spiel nicht angemessen zu Testzwecken veröffentlicht wird.

## <a name="manage-global-storage-files"></a>Verwalten von globalen Speicher-Dateien

Verwaltung von globalen Speicher können Sie zum Hochladen und Herunterladen von Dateien, die für den globalen Speicher verwendet werden. Diese Dateien können von jedem Benutzer, der den Titel besitzt möglicherweise zugegriffen werden und sind für alle Spieler, die das Spiel wieder freigegeben werden soll. Um finden Sie unter "Optionen" die globalen Speicherdatei. Sie müssen klicken Sie auf die **Optionen anzeigen** Dropdown-Liste neben dem Titel der Abschnitte. Hinzufügen Ihrer erste Datei, klicken Sie auf die **+ neu...**  Link. Sie werden dann die Option zum Hinzufügen von einer neuen Datei oder eines Ordners an den Dateien des globalen Speicher zugewiesen.

### <a name="new-folders"></a>Neue Ordner

Beim Hinzufügen von Dateien ein neuer Ordner in Ihren globalen Speicher lediglich nennen Sie den Ordner, und klicken Sie auf die **erstellen** Schaltfläche. Der neue Ordner wird in der Datei-Explorer-Tabelle angezeigt.

![Ordner-Dialogfeld hinzufügen](../../images/dev-center/title-storage/add-folder-global-storage-filled.JPG)

Zum Hinzufügen von Dateien in den Ordner Sie müssen diese in den Ordner hochladen direkt von den Ordnern übertragen **Aktionen** Schaltfläche: "**...** ", und wählen **Hochladen von Dateien**. Sie können keine ziehen und Ablegen von Dateien in Ordnern in der Explorer-Dateitabelle. Mithilfe der **erstellen Sie Ordner** Aktion in der **Aktionen** Menü eines Ordners wird einen untergeordneter Ordner erstellt. Auswählen der **löschen** Aktion in der **Aktionen** Menü eines Ordners wird der Ordner und dessen Inhalt gelöscht.

### <a name="new-files"></a>Neue Dateien

Beim Hinzufügen einer neuen Datei in den globalen Speicher Sie dazu aufgefordert werden, zum Hochladen einer Datei im Datei-Explorer des Computers und klicken Sie dann aufgefordert, eines der drei Dateitypen, binär, Konfiguration und JSON. Zusätzlich zum Hochladen einer Datei mit den **+ neu...**  Schaltfläche, Sie können auch ziehen und Ablegen von Dateien von Ihrem Computer, auf die Datei-Explorer-Tabelle.

> [!WARNING]
> Sie können nicht Ordner in der Tabelle der Datei-Explorer ziehen, versucht, diese Aktion führt zu dem Ordner, wie eine Datei behandelt werden und funktionieren nicht wie erwartet.

Datei-Verwaltungsaktionen: ![Management Gif-Datei](../../images/dev-center/title-storage/global-storage-management.gif)

#### <a name="file-types"></a>Dateitypen

* Binary - binary-Typs für Bilder, Sounds und benutzerdefinierte Daten verwendet werden soll. Diese Daten müssen HTTP-Anzeigename sein.
* Config - Konfigurationsdateien enthalten Informationen über Ihr Spiel und können dynamische Abfrage, die Grundlage von eingegebenen Werte zurückgeben.
* JSON - JSON-Dateien. Diese Dateien enthalten einige Informationen zu einem Aspekt Ihres Spiels, ähnlich wie eine Config-Datei.

## <a name="further-reading"></a>Weitere Informationen

Mehr über die Xbox Live-Storage-Plattform finden unter den [Titel speicherdokumentation](../../storage-platform/xbox-live-title-storage/xbox-live-title-storage.md) die erhalten weitere Informationen zu Universal, vertrauenswürdige, globalen Speicher und Dateitypen für Speicher, und die [Speicher verbunden Dokumentation](../../storage-platform/connected-storage/connected-storage-overview.md), die erfahren Sie mehr über das Speichern von Spielen Fortschritt in der Cloud, damit Sie Ihr Spiel zwischen Geräten fortsetzen können.