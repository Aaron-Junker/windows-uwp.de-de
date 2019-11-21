---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Zertifizierungskit für Windows-Apps
description: Damit Ihre App möglichst gute Chancen auf eine Veröffentlichung im Microsoft Store oder auf eine Windows-Zertifizierung hat, sollten Sie sie auf Ihrem Computer überprüfen und testen, bevor Sie sie zur Zertifizierung übermitteln. In diesem Thema wird erläutert, wie Sie das Zertifizierungskit für Windows-Apps installieren und ausführen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, App-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 4772edb9c99426396b7fa3a8734e2f45391c3a0f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74257837"
---
# <a name="windows-app-certification-kit"></a>Zertifizierungskit für Windows-Apps



Wenn Sie Ihre APP in [Windows zertifizieren](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) oder für [die Veröffentlichung auf die Microsoft Store](https://docs.microsoft.com/windows/uwp/publish/app-submissions)vorbereiten möchten, sollten Sie Sie zuerst lokal überprüfen und testen. In diesem Thema wird gezeigt, wie Sie das [zertifizierungskit für Windows-apps](https://msdn.microsoft.com/en-US/windows/apps/bg127575) installieren und ausführen, um sicherzustellen, dass Ihre APP sicher und effizient

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für das Testen einer universellen Windows-App:

-   Sie müssen Windows 10 installieren und ausführen.
-   Sie müssen das [Windows-zertifizierungskit für Apps (Version 10]( https://go.microsoft.com/fwlink/p/?LinkID=309666)) installieren, das im Windows Software Development Kit (SDK) für Windows 10 enthalten ist.
-   Sie müssen [Ihr Gerät für die Entwicklung aktivieren](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Sie müssen die zu testende Windows-App auf Ihrem Computer bereitstellen.

**Hinweis zu direkten Upgrades**

Beim Installieren einer neueren Version des [Zertifizierungskits für Windows-Apps]( https://go.microsoft.com/fwlink/p/?LinkID=309666) werden alle vorherigen Versionen des Kits ersetzt, die auf dem Computer installiert sind.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Interaktive Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps

1.  Suchen Sie im Startmenü nach den Einträgen **Apps** und **Windows Kits**, und klicken Sie auf die Option für das Zertifizierungskit für Windows-Apps.

2.  Wählen Sie im Zertifizierungskit für Windows-Apps die Kategorie der auszuführenden Überprüfung aus. Beispiel: Wenn Sie eine Windows-App überprüfen, wählen Sie die Option zum **Überprüfen einer Windows-App** aus.

    Sie können direkt zur jeweiligen App navigieren oder die App in einer Liste auf der Benutzeroberfläche auswählen. Bei der erstmaligen Ausführung des Zertifizierungskits für Windows-Apps werden auf der Benutzeroberfläche alle Windows-Apps aufgelistet, die auf dem Computer installiert sind. Bei nachfolgenden Ausführungen werden die Windows-Apps angezeigt, die Sie bereits überprüft haben. Falls die zu testende App nicht in der Liste enthalten ist, können Sie auf **Meine App ist nicht aufgeführt** klicken, um eine Liste aller installierten Apps im System anzuzeigen.

3.  Nachdem Sie die zu testende App eingegeben oder ausgewählt haben, klicken Sie auf **Weiter**.

4.  Auf dem nächsten Bildschirm sehen Sie den Testworkflow für die App, die Sie testen möchten. Wenn ein Test in der Liste deaktiviert ist, kann er nicht für Ihre Umgebung angewendet werden. Wenn Sie eine z. B. einer Windows 10-App unter Windows 7 testen, werden nur statische Tests für den Workflow angewendet. Beachten Sie, dass der Microsoft Store möglicherweise alle Tests von diesem Workflow anwendet. Wählen Sie aus, welche Tests Sie ausführen möchten, und klicken Sie dann auf **Weiter**.

    Das Zertifizierungskit für Windows-Apps beginnt mit dem Überprüfen der App.

5.  Geben Sie den Pfad zu dem Ordner an, in dem der Testbericht gespeichert werden soll, wenn Sie nach dem Testen dazu aufgefordert werden.

    Vom Zertifizierungskit für Windows-Apps werden ein XML-Bericht und ein HTML-Bericht erstellt und in diesem Ordner gespeichert.

6.  Öffnen Sie die Berichtsdatei, und überprüfen Sie die Ergebnisse des Tests.

> [!NOTE]
> Wenn Sie Visual Studio verwenden, können Sie das zertifizierungskit für Windows-apps ausführen, wenn Sie das App-Paket erstellen. Informationen zur Vorgehensweise finden Sie unter [Verpacken von UWP-Apps](/windows/msix/package/packaging-uwp-apps).

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps über eine Befehlszeile

> [!IMPORTANT]
> Das zertifizierungskit für Windows-apps muss im Kontext einer aktiven Benutzersitzung ausgeführt werden.

1.  Navigieren Sie im Befehlsfenster zum Verzeichnis mit dem Zertifizierungskit für Windows-Apps.

    **Beachten Sie**   der Standardpfad ist C:\\Programme\\Windows Kits\\10\\zertifizierungskit für apps\\.

2.  Geben Sie die folgenden Befehle in dieser Reihenfolge ein, um eine App zu testen, die bereits auf dem Testcomputer installiert ist:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Wenn Sie App nicht installiert ist, können Sie die folgenden Befehle verwenden. Das Zertifizierungskit für Windows-Apps öffnet das Paket und wendet den entsprechenden Testworkflow an:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Öffnen Sie nach dem Test die Berichtsdatei `[report file name]`, und überprüfen Sie die Testergebnisse.

**Beachten Sie**  das zertifizierungskit für Windows-apps von einem Dienst aus ausgeführt werden kann. der Dienst muss den Kit-Prozess jedoch innerhalb einer aktiven Benutzersitzung initiieren und kann nicht in Session0 ausgeführt werden.

**Hinweis**   Weitere Informationen zur Befehlszeile des zertifizierungskit für Windows-apps erhalten Sie, indem Sie den Befehl `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Testen mit einem Computer mit geringem Energieverbrauch

Die Leistungstestgrenzen des Zertifizierungskits für Windows-Apps basieren auf der Leistung eines Computers mit geringem Energieverbrauch.

Die Merkmale des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Um zu ermitteln, ob die Leistung Ihrer APP den [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)entspricht, empfiehlt es sich, die APP auf einem Computer mit geringer Leistung zu testen, z. b. auf einem Computer mit Intel Atom-Prozessor, auf dem eine Bildschirmauflösung von 1366x768 (oder höher) und einer rotierenden Festplatte (im Gegensatz zu einer Solid-State-Festplatte).

Da Computer mit geringem Energieverbrauch weiterentwickelt werden, können sich die Leistungsmerkmale im Laufe der Zeit ändern. Informieren Sie sich in den aktuellsten [Microsoft Store Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies) , und testen Sie Ihre APP mit der aktuellsten Version des zertifizierungskit für Windows-apps, um sicherzustellen, dass Ihre APP die neuesten Leistungsanforderungen erfüllt.

## <a name="related-topics"></a>Verwandte Themen

* [Windows App Certification Kit-Tests](windows-app-certification-kit-tests.md)
* [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)
 

 




