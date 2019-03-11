---
title: Einführung in Xbox One-Tools
description: Xbox One-spezifisches Tool Dev Home (unter Verwendung des Windows Device Portal).
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox One, Tools
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57609325"
---
# <a name="introduction-to-xbox-one-tools"></a>Einführung in Xbox One-Tools

In diesem Abschnitt wird erläutert, wie Sie auf das Xbox-Geräteportal über die Dev Home-App zugreifen.

## <a name="dev-home"></a>Dev Home

Dev Home ist ein Tool im Xbox One Development Kit, das die Produktivität von Entwicklern unterstützen soll. Dev Home bietet Funktionen zum Verwalten und Konfigurieren Ihres Dev Kit.

Dev Home ist die Standard-App, die geöffnet wird, wenn Ihre Konsole im Entwicklermodus gestartet wird. Sie können Dev Home auch über die Kachel **Dev Home** auf dem Startbildschirm öffnen. Ist keine Kachel vorhanden, befindet sich die Konsole nicht im Entwicklermodus.

Weitere Informationen zu Dev Home finden Sie unter [Entwickler-Startbildschirm auf der Konsole (Dev Home)](dev-home.md).

## <a name="xbox-device-portal"></a>Geräteportal für Xbox
Das Geräteportal für Xbox ist ein browserbasiertes Geräteverwaltungstool, mit dem Sie Spiele und Apps hinzufügen, Xbox Live-Test-Konten hinzufügen, Sandkastenumgebungen ändern und viele andere Aktionen durchführen können.

So aktivieren Sie das Xbox-Geräteportal auf Ihrer Xbox One Konsole

1. Wählen Sie die Kachel **Dev Home** auf dem Startbildschirm aus.

  ![Auswählen der Kachel „Dev Home“](images/introduction-to-xbox-one-tools-1.png)

2. Navigieren Sie in Dev Home zur Registerkarte **Home**, und wählen Sie im Abschnitt **Remotezugriff** die Option **Remotezugriffseinstellungen** aus.

  ![Tool „Remoteverwaltung"](images/introduction-to-xbox-one-tools-2.png)

3. Aktivieren Sie das Kontrollkästchen **Xbox-Geräteportal aktivieren**.

4. Aktivieren Sie unter **Authentifizierung** das Kontrollkästchen **Authentifizierung für Remotezugriff auf diese Konsole über das Web oder PC-Tools anfordern**.

5. Geben Sie **Benutzername** und __Kennwort__ ein, und wählen Sie **Speichern** aus. Diese Benutzeranmeldeinformationen werden zum Authentifizieren des Zugriffs auf Ihr Dev Kit über einen Browser verwendet.

6. Wählen Sie **Schließen** aus, und notieren Sie sich auf der Registerkarte **Home** die im Tool **Remoteverwaltung** aufgelistete URL.

7. Geben Sie die URL in Ihrem Browser ein. Sie erhalten eine Warnung zum Zertifikat, das bereitgestellt wurde, ähnlich wie folgender Screenshot, dass das von Ihrer Xbox One Konsole signierte Sicherheitszertifikat nicht als bekannter vertrauenswürdiger Herausgeber betrachtet wird. Klicken Sie in Edge auf **Details** und auf **Weiter zur Webseite**, um auf das Xbox-Geräteportal zuzugreifen.

    ![Sicherheitszertifikatwarnung](images/introduction-to-xbox-one-tools-3.png)

8. Melden Sie sich mit den Anmeldeinformationen an, die Sie konfiguriert haben.

## <a name="xbox-dev-mode-companion"></a>Begleitung für Xbox-Entwicklermodus
Die Begleitung für den Xbox-Entwicklermodus ist ein Tool, mit dessen Hilfe Sie auf der Konsole arbeiten können, ohne den PC verlassen zu müssen. Die App ermöglicht Ihnen die Anzeige des Konsolenbildschirms und das Senden von Eingaben an diesen. Weitere Informationen finden Sie unter [Begleitung für den Xbox-Entwicklermodus](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Siehe auch
- [Wie Sie Fiddler mit Xbox One verwenden, bei der Entwicklung für UWP](uwp-fiddler.md)
- [Übersicht über die Windows Device Portal](../debug-test-perf/device-portal.md)
- [UWP auf Xbox One](index.md)