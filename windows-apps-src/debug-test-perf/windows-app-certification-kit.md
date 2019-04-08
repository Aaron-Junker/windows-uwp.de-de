---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Zertifizierungskit für Windows-Apps
description: Damit Ihre App möglichst gute Chancen auf eine Veröffentlichung im Microsoft Store oder auf eine Windows-Zertifizierung hat, sollten Sie sie auf Ihrem Computer überprüfen und testen, bevor Sie sie zur Zertifizierung übermitteln. In diesem Thema wird erläutert, wie Sie das Zertifizierungskit für Windows-Apps installieren und ausführen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, Uwp, app-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: b480e96621e143e283a2556bdbef394aaf7dbc07
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597435"
---
# <a name="windows-app-certification-kit"></a>Zertifizierungskit für Windows-Apps



Um Ihre app [zertifizierten Windows](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) oder bereiten Sie sie für [zu den Microsoft Store-Veröffentlichung](https://msdn.microsoft.com/library/windows/apps/Hh694062), sollten Sie überprüfen und Testen Sie es zuerst lokal. In diesem Thema erfahren Sie, wie zum Installieren und Ausführen der [Windows App Certification Kit](https://go.microsoft.com/fwlink/p/?LinkID=309666) um sicherzustellen, dass Ihre app sicher und effizient.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für das Testen einer universellen Windows-App:

-   Sie müssen installieren und Ausführen von Windows 10.
-   Sie installieren müssen [Windows App Certification Kit, Version 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666), die im Windows Software Development Kit (SDK) für Windows 10 enthalten ist.
-   Sie müssen [Ihr Gerät für die Entwicklung aktivieren](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Sie müssen die zu testende Windows-App auf Ihrem Computer bereitstellen.

**Ein Hinweis zum direkten upgrades**

Beim Installieren einer neueren Version des [Zertifizierungskits für Windows-Apps]( https://go.microsoft.com/fwlink/p/?LinkID=309666) werden alle vorherigen Versionen des Kits ersetzt, die auf dem Computer installiert sind.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Interaktive Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps

1.  Suchen Sie im **Startmenü** nach den Einträgen **Apps** und **Windows Kits**, und klicken Sie auf die Option für das Zertifizierungskit für **Windows-Apps**.

2.  Wählen Sie im Zertifizierungskit für Windows-Apps die Kategorie der auszuführenden Überprüfung aus. Zum Beispiel: Wenn Sie eine Windows-app überprüfen, wählen Sie **überprüfen Sie eine Windows-app**.

    Sie können direkt zur jeweiligen App navigieren oder die App in einer Liste auf der Benutzeroberfläche auswählen. Bei der erstmaligen Ausführung des Zertifizierungskits für Windows-Apps werden auf der Benutzeroberfläche alle Windows-Apps aufgelistet, die auf dem Computer installiert sind. Bei nachfolgenden Ausführungen werden die Windows-Apps angezeigt, die Sie bereits überprüft haben. Falls die zu testende App nicht in der Liste enthalten ist, können Sie auf **Meine App ist nicht aufgeführt** klicken, um eine Liste aller installierten Apps im System anzuzeigen.

3.  Nachdem Sie die zu testende App eingegeben oder ausgewählt haben, klicken Sie auf **Weiter**.

4.  Auf dem nächsten Bildschirm sehen Sie den Testworkflow für die App, die Sie testen möchten. Wenn ein Test in der Liste deaktiviert ist, kann er nicht für Ihre Umgebung angewendet werden. Wenn Sie eine z. B. einer Windows 10-App unter Windows 7 testen, werden nur statische Tests für den Workflow angewendet. Beachten Sie, dass die Microsoft Store alle Tests aus diesem Workflow anfallen können. Wählen Sie aus, welche Tests Sie ausführen möchten, und klicken Sie dann auf **Weiter**.

    Das Zertifizierungskit für Windows-Apps beginnt mit dem Überprüfen der App.

5.  Geben Sie den Pfad zu dem Ordner an, in dem der Testbericht gespeichert werden soll, wenn Sie nach dem Testen dazu aufgefordert werden.

    Vom Zertifizierungskit für Windows-Apps werden ein XML-Bericht und ein HTML-Bericht erstellt und in diesem Ordner gespeichert.

6.  Öffnen Sie die Berichtsdatei, und überprüfen Sie die Ergebnisse des Tests.

**Beachten Sie**  , wenn Sie Visual Studio verwenden, können Sie die Windows-Zertifizierungskit für Apps ausführen, bei der Erstellung des app-Pakets. Informationen zur Vorgehensweise finden Sie unter [Verpacken von UWP-Apps](https://msdn.microsoft.com/library/windows/apps/Mt627715).

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps über eine Befehlszeile

**Wichtige**  muss die Windows-Zertifizierungskit für Apps im Kontext einer aktiven benutzersitzung ausgeführt werden.

1.  Navigieren Sie im Befehlsfenster zum Verzeichnis mit dem Zertifizierungskit für Windows-Apps.

    **Beachten Sie**    der Standardpfad ist "c:"\\Programmdateien\\Windows-Kits\\10\\Zertifizierungskit für Apps\\.

2.  Geben Sie die folgenden Befehle in dieser Reihenfolge ein, um eine App zu testen, die bereits auf dem Testcomputer installiert ist:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Wenn Sie App nicht installiert ist, können Sie die folgenden Befehle verwenden. Das Zertifizierungskit für Windows-Apps öffnet das Paket und wendet den entsprechenden Testworkflow an:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Öffnen Sie nach dem Test die Berichtsdatei `[report file name]`, und überprüfen Sie die Testergebnisse.

**Beachten Sie**  der Windows-Zertifizierungskit für Apps können von einem Dienst ausgeführt werden, aber der Dienst muss Initiieren der Kit innerhalb einer aktiven benutzersitzung und nicht in Session0 ausgeführt werden.

**Beachten Sie**    für Weitere Informationen über die Befehlszeile der Windows-Zertifizierungskit für Apps, geben Sie den Befehl `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Testen mit einem Computer mit geringem Energieverbrauch

Die Leistungstestgrenzen des Zertifizierungskits für Windows-Apps basieren auf der Leistung eines Computers mit geringem Energieverbrauch.

Die Merkmale des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Um festzustellen, ob die Leistung Ihrer app erfüllt die [Microsoft Store Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944), es wird empfohlen, dass Sie Ihre app auf einem Computer mit niedrigem Energieverbrauch, z. B. einem Intel Atom-prozessorbasierten Computer mit einer Bildschirmauflösung von 1366 x 768 (oder höher) testen und einer rotierenden Festplatte (im Gegensatz zu einer SSD-Festplatte).

Da Computer mit geringem Energieverbrauch weiterentwickelt werden, können sich die Leistungsmerkmale im Laufe der Zeit ändern. Finden Sie in der aktuellen [Microsoft Store Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944) und Testen Ihrer app mit der aktuellen Version der das Windows App Certification Kit, um sicherzustellen, dass die app mit den aktuellen leistungsanforderungen erfüllt.

## <a name="related-topics"></a>Verwandte Themen

* [Tests im Zertifizierungskit für Windows-App](windows-app-certification-kit-tests.md)
* [Microsoft Store-Richtlinien](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 

 




