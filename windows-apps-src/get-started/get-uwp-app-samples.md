---
title: Beispiele für UWP-Apps abrufen
description: Erfahren Sie, wie Sie die Beispiele für UWP-Codes von GitHub herunterladen können.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Beispielcode, Codebeispiele
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 4cdf38a4bd77c4f6affb813c9e1de68463c43100
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57598535"
---
# <a name="get-uwp-app-samples"></a>Beispiele für UWP-Apps abrufen

Die Beispiele für Universelle Windows Plattform-Apps (UWP-Apps) sind über Repositorys auf GitHub verfügbar. Unter [Samples](https://developer.microsoft.com/windows/samples "Dev Center Beispiele") finden Sie eine durchsuchbare, kategorisierte Liste, oder durchsuchen Sie die Repository [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit App-Beispielen für die Universelle Windows-Plattform"), die Beispiele für alle UWP-Features und ihre API-Verwendungsmuster enthält.  
![Beispielrepository von GitHub UWP](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Code herunterladen

Informationen zum Herunterladen der Beispiele finden Sie unter [Repository](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit App-Beispielen für die Universelle Windows-Plattform"). Wählen Sie **Clone or download** und dann **Herunterladen der ZIP-Datei**. Oder klicken Sie einfach [hier](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "ZIP-Datei mit App-Beispielen für die Universelle Windows-Plattform herunterladen").

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

4.  Wählen Sie die Sprache, Sie wie z. B. CS für C# verwenden, möchten\#, sehen Sie eine Visual Studio-Projektmappendatei, die Sie in Visual Studio öffnen können.

    ![VS-Projektmappe](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Geben Sie Feedback, stellen Sie Fragen und melden Sie Probleme

Wenn Sie Fragen oder Probleme haben, klicken Sie einfach auf die Registerkarte „Probleme“ im Repository, um ein neues Problem zu erstellen. Wir werden versuchen, zu helfen.

![Feedback-Bild](images/GitHubUWPSamplesFeedback.png)
