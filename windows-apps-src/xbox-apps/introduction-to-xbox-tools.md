---
title: Einführung in Xbox One-Tools
description: Erfahren Sie, wie Sie auf das Xbox one-Geräte Portal zugreifen, indem Sie die Developer Home-App im Xbox One Development Kit verwenden.
ms.date: 10/04/2017
ms.topic: article
keywords: Windows 10, UWP, Xbox One, Tools
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed9df02ba929d170eca5b37e4376220e93e4902f
ms.sourcegitcommit: e273e5901bfa6596dfef4cc741bb1c42614c25ab
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 09/01/2020
ms.locfileid: "89238315"
---
# <a name="introduction-to-xbox-one-tools"></a>Einführung in Xbox One-Tools

In diesem Abschnitt wird erläutert, wie Sie über die dev-Start-App auf das Xbox-Geräte Portal zugreifen.

## <a name="dev-home"></a>Dev Home

Dev Home ist ein Tool im Xbox One Development Kit, das die Produktivität von Entwicklern unterstützen soll. Dev Home bietet Funktionen zum Verwalten und Konfigurieren Ihres Dev Kit.

Dev Home ist die Standard-APP, die geöffnet wird, wenn die Konsole im Entwicklermodus startet. Sie können auch dev Home öffnen, indem Sie auf dem Startbildschirm die Kachel " **dev Home** " auswählen. Ist keine Kachel vorhanden, befindet sich die Konsole nicht im Entwicklermodus.

Weitere Informationen zu dev Home finden Sie unter [Developer Home in der Konsole (dev Home)](dev-home.md).

## <a name="xbox-device-portal"></a>Xbox-Geräte Portal
Das Xbox-Geräte Portal ist ein browserbasiertes Geräte Verwaltungs Tool, das Ihnen das Hinzufügen von spielen und apps, das Hinzufügen von Xbox Live-Testkonten, das Ändern von Sand Fächern und vieles mehr ermöglicht.

So aktivieren Sie das Xbox-Geräte Portal in der Xbox One-Konsole:

1. Wählen Sie auf dem Startbildschirm die Kachel **dev-Home** aus.

  ![Auswählen der Kachel „Dev Home“](images/introduction-to-xbox-one-tools-1.png)

2. Navigieren Sie in dev Home zur Registerkarte **Home** , und wählen Sie im Abschnitt **Remote Zugriff** die Option **Remote Zugriffs Einstellungen**aus.

  ![Tool „Remoteverwaltung"](images/introduction-to-xbox-one-tools-2.png)

3. Aktivieren Sie das Kontrollkästchen **Xbox-Geräte Portal aktivieren** .

4. Aktivieren Sie unter **Authentifizierung**das Kontrollkästchen **Authentifizierung für Remote Zugriff auf diese Konsole über das Web-oder PC-Tool** anfordern.

5. Geben Sie einen **Benutzernamen** und ein __Kennwort__ein, und wählen Sie **Speichern**. Diese Anmelde Informationen werden verwendet, um den Zugriff auf Ihr dev-Kit über einen Browser zu authentifizieren.

6. Wählen Sie **Schließen**aus, und notieren Sie sich auf der Registerkarte **Startseite** die URL, die im **Remote Zugriffs** Tool aufgeführt ist.

7. Geben Sie die URL in Ihren Browser ein. Sie erhalten eine Warnung zu dem angegebenen Zertifikat, ähnlich wie im folgenden Screenshot, weil das von Ihrer Xbox One-Konsole signierte Sicherheitszertifikat nicht als bekannter, vertrauenswürdiger Verleger angesehen wird. Klicken Sie in Edge auf **Details** , und navigieren Sie dann **zu der Webseite** , um auf das Xbox-Geräte Portal zuzugreifen.

    ![Sicherheitszertifikatwarnung](images/introduction-to-xbox-one-tools-3.png)

8. Melden Sie sich mit den konfigurierten Anmelde Informationen an.

## <a name="xbox-dev-mode-companion"></a>Begleitung für Xbox-Entwicklermodus
Die Begleitung für den Xbox-Entwicklermodus ist ein Tool, mit dessen Hilfe Sie auf der Konsole arbeiten können, ohne den PC verlassen zu müssen. Die App ermöglicht Ihnen die Anzeige des Konsolenbildschirms und das Senden von Eingaben an diesen. Weitere Informationen finden Sie unter [Begleitung für den Xbox-Entwicklermodus](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Weitere Informationen
- [Verwenden von Fiddler mit Xbox One bei der Entwicklung für UWP](uwp-fiddler.md)
- [Übersicht über das Windows-Geräteportal](../debug-test-perf/device-portal.md)
- [UWP auf Xbox One](index.md)