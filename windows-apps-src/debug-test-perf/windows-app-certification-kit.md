---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Zertifizierungskit für Windows-Apps
description: Damit Ihre app die beste Chance auf der Microsoft Store oder Chancen Windows-Zertifizierung veröffentlicht wird, überprüfen Sie und Testen sie lokal, bevor Sie sie zur Zertifizierung übermitteln. In diesem Thema wird erläutert, wie Sie das Zertifizierungskit für Windows-Apps installieren und ausführen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, app-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 614f59fe06528d7b5bac36290eae14f0d7d49653
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/03/2018
ms.locfileid: "8473700"
---
# <a name="windows-app-certification-kit"></a>Zertifizierungskit für Windows-Apps



Zum Abrufen von Ihrer app, die [Windows-Zertifizierung](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) , oder für die [Veröffentlichung im Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Hh694062)vorbereiten, sollten Sie überprüfen und testen es zunächst lokal. In diesem Thema wird das Installieren und führen Sie das [Zertifizierungskit für Windows-Apps](http://go.microsoft.com/fwlink/p/?LinkID=309666) , um sicherzustellen, dass Ihre app sicheren und effizienten veranschaulicht.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für das Testen einer universellen Windows-App:

-   Sie müssen installieren und Ausführen von Windows 10.
-   Sie müssen [Version der App Zertifizierungskit für Windows 10]( http://go.microsoft.com/fwlink/p/?LinkID=309666), installieren die im Windows Software Development Kit (SDK) für Windows 10 enthalten ist.
-   Sie müssen [Ihr Gerät für die Entwicklung aktivieren](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Sie müssen die zu testende Windows-App auf Ihrem Computer bereitstellen.

**Hinweis zu direkten Upgrades**

Beim Installieren einer neueren Version des [Zertifizierungskits für Windows-Apps]( http://go.microsoft.com/fwlink/p/?LinkID=309666) werden alle vorherigen Versionen des Kits ersetzt, die auf dem Computer installiert sind.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Interaktive Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps

1.  Suchen Sie im Startmenü**** nach den Einträgen **Apps** und **Windows Kits**, und klicken Sie auf die Option für das Zertifizierungskit für Windows-Apps****.

2.  Wählen Sie im Zertifizierungskit für Windows-Apps die Kategorie der auszuführenden Überprüfung aus. Beispiel: Wenn Sie eine Windows-App überprüfen, wählen Sie die Option zum **Überprüfen einer Windows-App** aus.

    Sie können direkt zur jeweiligen App navigieren oder die App in einer Liste auf der Benutzeroberfläche auswählen. Bei der erstmaligen Ausführung des Zertifizierungskits für Windows-Apps werden auf der Benutzeroberfläche alle Windows-Apps aufgelistet, die auf dem Computer installiert sind. Bei nachfolgenden Ausführungen werden die Windows-Apps angezeigt, die Sie bereits überprüft haben. Falls die zu testende App nicht in der Liste enthalten ist, können Sie auf **Meine App ist nicht aufgeführt** klicken, um eine Liste aller installierten Apps im System anzuzeigen.

3.  Nachdem Sie die zu testende App eingegeben oder ausgewählt haben, klicken Sie auf **Weiter**.

4.  Auf dem nächsten Bildschirm sehen Sie den Testworkflow für die App, die Sie testen möchten. Wenn ein Test in der Liste deaktiviert ist, kann er nicht für Ihre Umgebung angewendet werden. Wenn Sie eine z.B. einer Windows10-App unter Windows7 testen, werden nur statische Tests für den Workflow angewendet. Beachten Sie, dass der Microsoft Store alle Tests aus diesem Workflow anwenden kann. Wählen Sie aus, welche Tests Sie ausführen möchten, und klicken Sie dann auf **Weiter**.

    Das Zertifizierungskit für Windows-Apps beginnt mit dem Überprüfen der App.

5.  Geben Sie den Pfad zu dem Ordner an, in dem der Testbericht gespeichert werden soll, wenn Sie nach dem Testen dazu aufgefordert werden.

    Vom Zertifizierungskit für Windows-Apps werden ein XML-Bericht und ein HTML-Bericht erstellt und in diesem Ordner gespeichert.

6.  Öffnen Sie die Berichtsdatei, und überprüfen Sie die Ergebnisse des Tests.

**Hinweis:** Wenn Sie Visual Studio verwenden, können Sie das Zertifizierungskit für Windows-Apps ausführen, wenn Sie Ihr app-Paket erstellen. Informationen zur Vorgehensweise finden Sie unter [Verpacken von UWP-Apps](https://msdn.microsoft.com/library/windows/apps/Mt627715).

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps über eine Befehlszeile

**Wichtige**das Zertifizierungskit für für Windows-App muss im Kontext einer aktiven benutzersitzung ausgeführt werden.

1.  Navigieren Sie im Befehlsfenster zum Verzeichnis mit dem Zertifizierungskit für Windows-Apps.

    **Hinweis:**  der Standardpfad lautet C:\\Program c:\\programme\\windows Kits\\10\\App Certification kit\\ ".

2.  Geben Sie die folgenden Befehle in dieser Reihenfolge ein, um eine App zu testen, die bereits auf dem Testcomputer installiert ist:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Wenn Sie App nicht installiert ist, können Sie die folgenden Befehle verwenden. Das Zertifizierungskit für Windows-Apps öffnet das Paket und wendet den entsprechenden Testworkflow an:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Öffnen Sie nach dem Test die Berichtsdatei `[report file name]`, und überprüfen Sie die Testergebnisse.

**Hinweis:** das Zertifizierungskit für für Windows-App kann über einen Dienst ausgeführt werden, aber der Dienst muss den kitvorgang innerhalb einer aktiven benutzersitzung initiieren und kann nicht in Session0 ausgeführt werden.

**Hinweis:**  Weitere Informationen zur Befehlszeile Zertifizierungskits für Windows-App, geben Sie den Befehl `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Testen mit einem Computer mit geringem Energieverbrauch

Die Leistungstestgrenzen des Zertifizierungskits für Windows-Apps basieren auf der Leistung eines Computers mit geringem Energieverbrauch.

Die Eigenschaften des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Um festzustellen, ob die Leistung Ihrer app die [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944)erfüllt, wird empfohlen, dass Sie Ihre app auf einem Computer mit geringem Energieverbrauch, z. B. eine Intel Atom-Prozessor-basierten Computer mit einer Auflösung von 1366 x 768 (oder höher) und einer rotierenden Festplatte testen Laufwerk (im Gegensatz zu einem Festkörperlaufwerk).

Da Computer mit geringem Energieverbrauch weiterentwickelt werden, können sich die Leistungsmerkmale im Laufe der Zeit ändern. Verweisen auf die aktuelle [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944) , und Testen Sie Ihre app mit der aktuellen Version des Zertifizierungskits für Windows-App sicherstellen, dass Ihre app die aktuellen leistungsanforderungen entspricht.

## <a name="related-topics"></a>Verwandte Themen

* [Tests im Zertifizierungskit für Windows-Apps](windows-app-certification-kit-tests.md)
* [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 

 




