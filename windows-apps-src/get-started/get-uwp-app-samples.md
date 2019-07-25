---
title: Beispiele für UWP-Apps abrufen
description: Erfahren Sie, wie Sie die UWP-Codebeispiele von GitHub herunterladen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Beispielcode, Codebeispiele
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 7f56b0f9e4cb7f89b8bc929015ecdf6d5c64d42e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321076"
---
# <a name="get-uwp-app-samples"></a>Beispiele für UWP-Apps abrufen

Die Beispiele für Universelle Windows Plattform-Apps (UWP-Apps) sind über Repositorys auf GitHub verfügbar. Unter [SamplesDev](https://developer.microsoft.com/windows/samples%20%22Dev%20Center%20samples%22) finden Sie eine durchsuchbare, kategorisierte Liste. Alternativ können Sie das Repository [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit App-Beispielen für die Universelle Windows-Plattform") durchsuchen, das Beispiele für alle UWP-Features und ihre API-Verwendungsmuster enthält.  
![GitHub-Repository mit UWP-Beispielen](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Code herunterladen

Um die Beispiele herunterzuladen, wechseln Sie zum [Repository](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit App-Beispielen für die Universelle Windows-Plattform") und wählen **Clone or download** (Klonen oder herunterladen) und dann **Download ZIP** (ZIP herunterladen) aus. Oder klicken Sie einfach [hier](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "ZIP-Datei mit App-Beispielen für die Universelle Windows-Plattform herunterladen").

Die ZIP-Datei enthält stets die neuesten Beispiele. Sie benötigen zum Herunterladen kein GitHub-Konto. Wenn ein SDK-Update veröffentlicht wird oder wenn Sie alle aktuellen Änderungen/Ergänzungen übernehmen möchten, suchen Sie einfach nach der neuesten ZIP-Datei.

![Beispieldownload](images/SamplesDownloadButton.png)


> [!NOTE]
> Für das Öffnen, Erstellen und Ausführen der UWP-Beispiele sind Visual Studio 2015 oder höher und das Windows SDK erforderlich. Eine kostenlose Kopie von Visual Studio Community mit Unterstützung für die Erstellung von UWP-Apps erhalten Sie [hier](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Downloads für Windows-Entwicklungstools").  
>
> Darüber hinaus sollten Sie sicherstellen, dass nicht nur einzelne Beispiele, sondern das gesamte Archiv entpackt wurde. Alle Beispiele erfordern den Archivordner „SharedContent“. Die Beispiele für UWP-Features verfügen über verknüpfte Dateien in Visual Studio, um das Duplizieren gemeinsam verwendeter Dateien zu vermeiden, z. B. von Beispieldateien für Vorlagen und Bildressourcen. Diese gemeinsamen Dateien werden im Ordner „SharedContent“ im Stammverzeichnis des Repositorys gespeichert. Der Verweis in den Projektdateien erfolgt über Links.

Öffnen Sie nach dem Herunterladen der ZIP-Datei die Beispiele in Visual Studio:

1.  Klicken Sie vor dem Extrahieren des Archivs mit der rechten Maustaste darauf, und wählen Sie **Eigenschaften** > **Blockierung aufheben** > **Übernehmen** aus. Entpacken Sie anschließend das Archiv in einen lokalen Ordner auf Ihrem Computer.

    ![Entpacktes Archiv](images/SamplesUnzip1.png)
2.  Im Beispielordner wird eine Reihe von Ordnern angezeigt, die jeweils ein Beispiel für UWP-Features enthalten.

    ![Beispielordner](images/SamplesUnzip2.png)

3.  Wählen Sie ein Beispiel wie z. B. Höhenmesser aus, um mehrere Ordner mit den unterstützten Sprachen anzuzeigen.

    ![Sprachordner](images/SamplesUnzip3.png)

4.  Wählen Sie die gewünschte Sprache (z. B. CS für C\#) aus. Es wird eine Visual Studio-Projektmappendatei angezeigt, die Sie in Visual Studio öffnen können.

    ![VS-Projektmappe](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Geben Sie Feedback, stellen Sie Fragen und melden Sie Probleme

Wenn Sie Fragen oder Probleme haben, klicken Sie einfach auf die Registerkarte „Probleme“ im Repository, um ein neues Problem zu erstellen. Wir werden versuchen, zu helfen.

![Feedback-Bild](images/GitHubUWPSamplesFeedback.png)
