---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Zertifizierungskit für Windows-Apps
description: Damit Ihre App möglichst gute Chancen auf eine Veröffentlichung im Microsoft Store oder auf eine Windows-Zertifizierung hat, sollten Sie sie auf Ihrem Computer überprüfen und testen, bevor Sie sie zur Zertifizierung übermitteln. In diesem Thema wird erläutert, wie Sie das Zertifizierungskit für Windows-Apps installieren und ausführen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP, App-Zertifizierung
ms.localizationpriority: medium
ms.openlocfilehash: 174ff4e588d75293ecb729312883f4792196c87a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "79110104"
---
# <a name="windows-app-certification-kit"></a>Zertifizierungskit für Windows-Apps

Um eine [Windows-Zertifizierung](/windows/win32/win_cert/windows-certification-portal) für deine App zu erhalten oder sie für die [Veröffentlichung im Microsoft Store](/windows/uwp/publish/app-submissions) vorzubereiten, solltest du sie zunächst lokal validieren und testen. In diesem Thema wird gezeigt, wie du das [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) installierst und ausführst.

## <a name="prerequisites"></a>Voraussetzungen

Voraussetzungen für das Testen einer universellen Windows-App:

- Installieren und Ausführen von Windows 10.
- Du musst das [Zertifizierungskit für Windows-Apps](https://developer.microsoft.com/windows/downloads/app-certification-kit/) installieren, das im Windows Software Development Kit (SDK) für Windows 10 enthalten ist.
- Sie müssen [Ihr Gerät für die Entwicklung aktivieren](/windows/uwp/get-started/enable-your-device-for-development).
- Sie müssen die zu testende Windows-App auf Ihrem Computer bereitstellen.

> [!NOTE]
> **Direktes Upgrade:** Durch die Installation einer neueren Version des [Zertifizierungskits für Windows-Apps](https://developer.microsoft.com/windows/develop/app-certification-kit) werden alle vorherigen Versionen des Kits ersetzt.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Interaktive Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps

1. Suche im **Startmenü** nach den Einträgen **Apps** und **Windows Kits**, und klicke auf die Option für das **Zertifizierungskit für Windows-Apps**.

2. Wählen Sie im Zertifizierungskit für Windows-Apps die Kategorie der auszuführenden Überprüfung aus. Beispiel: Wenn du eine Windows-App überprüfst, wähle die Option **Windows-App validieren** aus.

    Sie können direkt zur jeweiligen App navigieren oder die App in einer Liste auf der Benutzeroberfläche auswählen. Bei der erstmaligen Ausführung des Zertifizierungskits für Windows-Apps werden auf der Benutzeroberfläche alle Windows-Apps aufgelistet, die auf dem Computer installiert sind. Bei nachfolgenden Ausführungen werden die Windows-Apps angezeigt, die Sie bereits überprüft haben. Falls die zu testende App nicht in der Liste enthalten ist, können Sie auf **Meine App ist nicht aufgeführt** klicken, um eine Liste aller installierten Apps im System anzuzeigen.

3. Nachdem Sie die zu testende App eingegeben oder ausgewählt haben, klicken Sie auf **Weiter**.

4. Auf dem nächsten Bildschirm sehen Sie den Testworkflow für die App, die Sie testen möchten. Wenn ein Test in der Liste deaktiviert ist, kann er nicht für Ihre Umgebung angewendet werden. Wenn Sie eine z. B. einer Windows 10-App unter Windows 7 testen, werden nur statische Tests für den Workflow angewendet. Beachte, dass der Windows Store alle Tests aus diesem Workflow anwenden kann. Wählen Sie aus, welche Tests Sie ausführen möchten, und klicken Sie dann auf **Weiter**.

    Das Zertifizierungskit für Windows-Apps beginnt mit dem Überprüfen der App.

5. Geben Sie den Pfad zu dem Ordner an, in dem der Testbericht gespeichert werden soll, wenn Sie nach dem Testen dazu aufgefordert werden.

    Vom Zertifizierungskit für Windows-Apps werden ein XML-Bericht und ein HTML-Bericht erstellt und in diesem Ordner gespeichert.

6. Öffnen Sie die Berichtsdatei, und überprüfen Sie die Ergebnisse des Tests.

> [!NOTE]
> Bei Verwendung von Visual Studio kannst du das Zertifizierungskit für Windows-Apps ausführen, wenn du das App-Paket erstellst. Informationen zur Vorgehensweise finden Sie unter [Verpacken von UWP-Apps](/windows/msix/package/packaging-uwp-apps).

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Überprüfung der Windows-App mit dem Zertifizierungskit für Windows-Apps über eine Befehlszeile

> [!IMPORTANT]
> Das Zertifizierungskit für Windows-Apps muss im Kontext einer aktiven Benutzersitzung ausgeführt werden.

1. Navigieren Sie im Befehlsfenster zum Verzeichnis mit dem Zertifizierungskit für Windows-Apps.

    **Hinweis**  Der Standardpfad lautet „C:\\Programme\\Windows Kits\\10\\App Certification Kit\\“.

2. Geben Sie die folgenden Befehle in dieser Reihenfolge ein, um eine App zu testen, die bereits auf dem Testcomputer installiert ist:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Wenn Sie App nicht installiert ist, können Sie die folgenden Befehle verwenden. Das Zertifizierungskit für Windows-Apps öffnet das Paket und wendet den entsprechenden Testworkflow an:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. Öffnen Sie nach dem Test die Berichtsdatei `[report file name]`, und überprüfen Sie die Testergebnisse.

**Hinweis**  Das Zertifizierungskit für Windows-Apps kann über einen Dienst ausgeführt werden. Der Dienst muss den Prozess für das Kit jedoch innerhalb einer aktiven Benutzersitzung initiieren, eine Ausführung in „Session0“ ist nicht möglich.

**Hinweis**  Weitere Informationen zur Befehlszeile des Zertifizierungskits für Windows-Apps erhältst du durch Eingabe des Befehls `appcert.exe /?`.

## <a name="testing-with-a-low-power-computer"></a>Testen mit einem Computer mit geringem Energieverbrauch

Die Leistungstestgrenzen des Zertifizierungskits für Windows-Apps basieren auf der Leistung eines Computers mit geringem Energieverbrauch.

Die Merkmale des Computers, auf dem der Test ausgeführt wird, können die Testergebnisse beeinflussen. Um festzustellen, ob die Leistung deiner App den [Richtlinien für den Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) entspricht, wird empfohlen, die App auf einem Computer mit geringem Energieverbrauch zu testen – beispielsweise auf einem Computer mit Intel Atom-Prozessor, einer Auflösung von 1366 × 768 (oder höher) und einem herkömmlichen Festplattenlaufwerk (kein Solid-State-Laufwerk).

Da Computer mit geringem Energieverbrauch weiterentwickelt werden, können sich die Leistungsmerkmale im Laufe der Zeit ändern. Verwende die aktuellen [Richtlinien für den Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), und teste deine App mit der aktuellen Version des Zertifizierungskits für Windows-Apps, um sicherzustellen, dass deine App den aktuellen Leistungsanforderungen entspricht.

## <a name="related-topics"></a>Zugehörige Themen

- [Tests im Zertifizierungskit für Windows-Apps](windows-app-certification-kit-tests.md)
- [Microsoft Store-Richtlinien](https://docs.microsoft.com/legal/windows/agreements/store-policies)
