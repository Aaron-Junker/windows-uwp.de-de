---
title: Beispiele für UWP-Apps abrufen
description: Erfahre, wie du die UWP-Codebeispiele von GitHub herunterlädst.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, Beispielcode, Codebeispiele
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ac3c99bc364e81386a362f1d1b5530bee9d462c4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259515"
---
# <a name="get-uwp-app-samples"></a>Beispiele für UWP-Apps abrufen

Die Beispiele für Universelle Windows Plattform-Apps (UWP-Apps) sind in Repositorys auf GitHub verfügbar. Unter [Codebeispiele](https://developer.microsoft.com/windows/samples) finden Sie eine durchsuchbare, kategorisierte Liste. Alternativ können Sie das Repository [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit Beispielen für Universelle Windows-Plattform-Apps – GitHub-Repository") durchsuchen. Das Repository mit Beispielen für die universelle Windows-Plattform enthält Beispiele, in denen alle UWP-Features und deren API-Verwendungsmuster veranschaulicht werden.

![UWP-Beispielrepository von GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Code herunterladen

Um die Beispiele herunterzuladen, wechseln Sie zum [Repository](https://github.com/Microsoft/Windows-universal-samples "GitHub-Repository mit Beispielen für Universelle Windows-Plattform-Apps – GitHub-Repository"). Wähle **Clone or download** (Klonen oder herunterladen) und dann **Download ZIP** (ZIP-Datei herunterladen) aus. 

![Herunterladen von Beispielen](images/SamplesDownloadButton.png)

Sie können auch die [Beispiele](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "ZIP-Datei mit Beispielen für Universelle Windows-Plattform-Apps herunterladen") aus diesem Artikel herunterladen.

Die ZIP-Downloaddatei enthält immer die neuesten Beispiele. Du benötigst zum Herunterladen kein GitHub-Konto. Wenn ein SDK-Update veröffentlicht wird oder du alle aktuellen Änderungen und Ergänzungen übernehmen möchtest, lade einfach die neueste ZIP-Datei herunter.

> [!NOTE]
> Für das Öffnen, Erstellen und Ausführen der UWP-Beispiele sind Visual Studio 2015 oder höher und das Windows SDK erforderlich. Eine kostenlose Kopie von Visual Studio Community erhalten Sie [hier](https://www.microsoft.com/?ref=go). Visual Studio Community unterstützt das Entwickeln von UWP-Apps.  
>
> Damit die Beispiele ordnungsgemäß funktionieren, solltest du sicherstellen, dass nicht einzelne Beispiele, sondern das gesamte Archiv entpackt wird. Alle Beispiele erfordern den Archivordner „SharedContent“. Die Beispiele für UWP-Features verfügen über verknüpfte Dateien in Visual Studio, um das Duplizieren gemeinsam verwendeter Dateien zu vermeiden, z. B. von Beispieldateien für Vorlagen und Bildressourcen. Allgemeine Dateien werden im Stammverzeichnis des Repositorys im SharedContent-Ordner gespeichert. Links werden in den Projektdateien verwendet, um auf allgemeine Dateien zu verweisen.
> 

## <a name="open-the-samples"></a>Öffnen der Beispiele

Öffne nach dem Herunterladen der ZIP-Datei die Beispiele in Visual Studio.

1.  Klicke vor dem Extrahieren des Archivs mit der rechten Maustaste, und wähle **Eigenschaften** > **Blockierung aufheben** > **Übernehmen** aus. Entpacke anschließend das Archiv auf deinem Computer in einen lokalen Ordner.

    ![Entpacktes Archiv](images/SamplesUnzip1.png)
2.  Jeder Ordner im Ordner „Samples“ enthält ein Beispiel für eine UWP-Funktion.

    ![Beispielordner](images/SamplesUnzip2.png)
3.  Wähle ein Beispiel aus, z.B. „Altimeter“. Unterordner geben die unterstützten Sprachen an.

    ![Sprachordner](images/SamplesUnzip3.png)
4.  Wähle den Ordner der zu verwendenden Sprache aus. Im Ordnerinhalt siehst du eine Visual Studio-Projektmappendatei (.sln), die du in Visual Studio öffnen kannst. Beispiel: *Altimeter.sln*.

    ![VS-Projektmappe](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Geben Sie Feedback, stellen Sie Fragen und melden Sie Probleme

Wenn du Fragen oder Probleme hast, klicke einfach auf die Registerkarte **Issues** (Probleme) im Repository, um ein neues Problem zu erstellen. Wir werden versuchen, zu helfen.

![Feedback-Bild](images/GitHubUWPSamplesFeedback.png)
