---
title: Beispiele für Windows-Apps abrufen
description: Erfahren Sie, wie Sie GitHub-Codebeispiele, die die meisten Windows-Features und ihre API-Verwendungsmuster demonstrieren, durchsuchen, herunterladen und öffnen.
ms.date: 06/30/2020
ms.topic: article
keywords: Windows 10, Beispielcode, Codebeispiele
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: e08a81ac6f9bca2e4fe4b7451df830e20c714893
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89162944"
---
# <a name="get-windows-app-samples"></a>Beispiele für Windows-Apps abrufen

Viele offizielle Windows-Codebeispiele sind in verschiedenen GitHub-Repositorys verfügbar, z. B. [Beispiele für Universal Windows Platform (UWP)-App](https://github.com/microsoft/Windows-universal-samples), [klassische Windows-Beispiele](https://github.com/microsoft/Windows-classic-samples), zusammen mit einer Sammlung von [Beispielen für die Windows-Entwicklerdokumentation](#windows-developer-documentation-samples). Diese Beispiele veranschaulichen die meisten Windows-Features und ihre API-Verwendungsmuster.

:::image type="content" source="images/github-windows-samples-page.png" alt-text="GitHub-Repository für Windows Universal-Beispiele":::

Um das Auffinden bestimmter Beispiele etwas zu erleichtern, können Sie eine kategorisierte Sammlung von Codebeispielen für verschiedene Microsoft-Entwicklungstools und -technologien über den [Beispielbrowser](/samples/browse/) durchgehen und durchsuchen.

:::image type="content" source="images/samples-browser-windows.png" alt-text="Microsoft-Beispielbrowser":::

### <a name="windows-developer-documentation-samples"></a>Beispiele für die Windows-Entwicklerdokumentation

Hier finden Sie eine Liste von Beispielen für Mini-Apps, die speziell zur Unterstützung der Windows-Entwicklerdokumentation erstellt wurden. Sofern nicht anders angegeben, handelt es sich bei den folgenden Beispielen um Universal Windows Platform (UWP)-Apps, die für die Verwendung der neuesten [WinUI 2.4](/windows/apps/winui/winui2/release-notes/winui-2.4)-Steuerelemente aktualisiert wurden.

- [RSS-Reader](https://github.com/Microsoft/Windows-appsample-rssreader): Abrufen von RSS-Feeds und Anzeigen von Artikeln
- [Family Notes](https://github.com/Microsoft/Windows-appsample-familynotes): Entdecken verschiedener Eingabemodalitäten und -szenarien für Benutzersensibilisierung
- [Kundenbestellungen](https://github.com/Microsoft/Windows-appsample-customers-orders-database): Hilfreiche Features für Entwickler in Unternehmen, z. B. AAD-Authentifizierung (Azure Active Directory), Benutzeroberflächen-Steuerelemente (inklusive des Datenrasters), Sqlite- und SQL Azure-Datenbankintegration, Entity Framework und API-Clouddienste
- [Lunch Scheduler](https://github.com/Microsoft/Windows-appsample-lunch-scheduler): Planung von Mittagessen mit Freunden und Kollegen
- [Coloring Book](https://github.com/Microsoft/Windows-appsample-coloringbook): Windows Ink (einschließlich der Windows Ink-Symbolleiste) und Radial Controller (für Wheel-Geräte wie Surface Dial)-Features
- [Hilfsprogramm für das Netzwerk (Quizspiel)](https://github.com/Microsoft/Windows-appsample-networkhelper): Netzwerkermittlung und -kommunikation
- [HUE Lights-Controller](https://github.com/Microsoft/Windows-appsample-huelightcontroller): Intelligente Gebäudeautomatisierung mit Cortana und Bluetooth Low Energy (Bluetooth LE)
- [Marble Maze](https://github.com/Microsoft/Windows-appsample-marble-maze): Einfaches 3D-Spiel mit DirectX
- [PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab): Anzeigen und Bearbeiten von Bilddateien

## <a name="download-the-code"></a>Code herunterladen

Um die Beispiele herunterzuladen, navigieren Sie zu einem der Microsoft-Repositorys, z. B. [Apps für die universelle Windows-Plattform (UWP): Beispiele](https://github.com/microsoft/Windows-universal-samples). Wähle **Clone or download** (Klonen oder herunterladen) und dann **Download ZIP** (ZIP-Datei herunterladen) aus.

![Herunterladen von Beispielen](images/SamplesDownloadButton.png)

Die ZIP-Downloaddatei enthält immer die neuesten Beispiele. Du benötigst zum Herunterladen kein GitHub-Konto. Wenn ein SDK-Update veröffentlicht wird oder du alle aktuellen Änderungen und Ergänzungen übernehmen möchtest, lade einfach die neueste ZIP-Datei herunter.

> [!NOTE]
> Für das Öffnen, Erstellen und Ausführen der Windows-Beispiele sind Visual Studio und das Windows SDK erforderlich. Eine kostenlose Kopie von Visual Studio Community erhalten Sie [hier](https://www.microsoft.com/?ref=go).  
>
> Damit die Beispiele ordnungsgemäß funktionieren, solltest du sicherstellen, dass nicht einzelne Beispiele, sondern das gesamte Archiv entpackt wird. Viele der Beispiele hängen von gemeinsam verwendeten Dateien im Ordner *SharedContent* ab und verwenden verknüpfte Dateien, z. B. Beispieldateien für Vorlagen und Bildressourcen, um das Duplizieren gemeinsam verwendeter Dateien zu vermeiden.

## <a name="open-the-samples"></a>Öffnen der Beispiele

Öffne nach dem Herunterladen der ZIP-Datei die Beispiele in Visual Studio.

1. Klicken Sie vor dem Extrahieren des Archivs mit der rechten Maustaste auf die Datei, und wählen Sie **Eigenschaften** > **Blockierung aufheben** > **Übernehmen** aus. Entpacke anschließend das Archiv auf deinem Computer in einen lokalen Ordner.

    ![Entpacktes Archiv](images/SamplesUnzip1.png)

2. Jeder Ordner im Ordner „Samples“ enthält ein Beispiel für ein Windows-Feature.

    ![Beispielordner](images/SamplesUnzip2.png)

3. Wählen Sie ein Beispiel aus. Unterstützte Sprachen werden durch einen sprachspezifischen Unterordner angegeben.

    ![Sprachordner](images/SamplesUnzip3.png)

4. Wähle den Ordner der zu verwendenden Sprache aus. Im Ordnerinhalt siehst du eine Visual Studio-Projektmappendatei (.sln), die du in Visual Studio öffnen kannst.

    ![VS-Projektmappe](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Geben Sie Feedback, stellen Sie Fragen und melden Sie Probleme

Wenn du Fragen oder Probleme hast, klicke einfach auf die Registerkarte **Issues** (Probleme) im Repository, um ein neues Problem zu erstellen.

![Feedback-Bild](images/GitHubUWPSamplesFeedback.png)