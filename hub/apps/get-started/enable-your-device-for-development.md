---
ms.assetid: 54973C62-9669-4988-934E-9273FB0425FD
title: Aktivieren Ihres Geräts für die Entwicklung
description: Erfahren Sie, wie Sie Ihr Windows 10-Gerät für Entwicklung und Debuggen aktivieren, indem Sie den Entwicklermodus in Visual Studio aktivieren.
keywords: Erste Schritte Entwicklerlizenz Visual Studio, Entwicklerlizenz Gerät aktivieren
ms.date: 10/13/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 642dd2ac6db5509e9bc4ea6ff8cb756312c3e90b
ms.sourcegitcommit: 56e9cab45d1c6e54841d61fdf23044fa01f50c43
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/13/2020
ms.locfileid: "92011905"
---
# <a name="enable-your-device-for-development"></a>Aktivieren Ihres Geräts für die Entwicklung

## <a name="activate-developer-mode-sideload-apps-and-access-other-developer-features"></a>Aktivieren des Entwicklermodus, Querladen von Apps und Zugreifen auf andere Entwicklerfeatures

![Aktivieren deiner Geräte für die Entwicklung](images/developer-poster.png)

> [!IMPORTANT]
> Wenn Sie keine eigenen Anwendungen auf Ihrem PC erstellen, brauchen Sie den Entwicklermodus nicht zu aktivieren. Wenn Sie versuchen, ein Problem mit Ihrem Computer zu beheben, ziehen Sie die [Windows-Hilfe](https://support.microsoft.com/hub/4338813/windows-help?os=windows-10) zurate. Wenn Sie zum ersten Mal entwickeln, sollten Sie die Umgebung [ einrichten](get-set-up.md), indem Sie die benötigten Tools herunterladen.

Wenn du deinen Computer für alltägliche Aktivitäten wie Spiele, Surfen im Web, E-Mails oder Office-Apps verwendest, musst du den Entwicklermodus *nicht* aktivieren. (In diesem Fall raten wir sogar davon ab.) Die restlichen Informationen auf dieser Seite sind dann für dich nicht relevant, und du kannst einfach mit dem fortfahren, womit du dich gerade beschäftigt hast. Danke für deinen Besuch!

Wenn du allerdings auf einem Computer zum ersten Mal Software mit Visual Studio schreibst, *musst* du den Entwicklermodus sowohl auf dem Entwicklungs-PC als auch auf allen Geräten aktivieren, die du zum Testen des Codes verwenden möchtest. Ist der Entwicklermodus beim Öffnen eines UWP-Projekts nicht aktiviert, wird entweder die Einstellungsseite **Für Entwickler** geöffnet oder das folgende Dialogfeld in Visual Studio angezeigt:

![Dialogfeld zum Aktivieren des Entwicklermodus in Visual Studio](images/latestenabledialog.png)

Sollte dieses Dialogfeld angezeigt werden, klicke auf **Für Entwickler-Einstellungen**, um die Einstellungsseite **Für Entwickler** zu öffnen.

> [!NOTE]
> Du kannst jederzeit zur Seite **Für Entwickler** navigieren, um den Entwicklermodus zu aktivieren oder zu deaktivieren. Gibt dazu einfach auf der Taskleiste im Cortana-Suchfeld „für Entwickler“ ein.

## <a name="accessing-settings-for-developers"></a>Zugreifen auf die Einstellungen für Entwickler

Gehe wie folgt vor, um den Entwicklermodus zu aktivieren oder auf andere Einstellungen zuzugreifen:

1.  Wähle im Dialogfeld **Für Entwickler** die gewünschte Zugriffsebene aus.
2.  Lesen Sie den Haftungsausschluss für die ausgewählte Einstellung, und klicken Sie dann auf **Ja** um die Änderung zu übernehmen.

> [!NOTE]
> Zum Aktivieren des Entwicklermodus benötigst du Administratorzugriff. Falls dein Gerät einer Organisation gehört, ist diese Option möglicherweise deaktiviert.

### <a name="developer-mode-features"></a>Features des Entwicklermodus

Der Entwicklermodus ersetzt die Windows 8.1-Anforderungen durch eine Entwicklerlizenz.  Neben dem Querladen bietet der Entwicklermodus Debug- und zusätzliche Bereitstellungsoptionen. Hierzu gehört das Starten eines SSH-Diensts, der Bereitstellungen auf diesem Gerät ermöglicht. Um den Dienst zu beenden, müssen Sie den Entwicklermodus deaktivieren.

Wenn du den Entwicklermodus auf dem Desktop aktivierst, wird ein Featurepaket installiert, das Folgendes umfasst:
- Windows-Geräteportal: Nur wenn die Option **Geräteportal aktivieren** aktiviert ist, ist das Geräteportal aktiviert und Firewallregeln werden für es konfiguriert.
- Installiert und konfiguriert Firewallregeln für SSH-Dienste, die eine Remoteinstallation von Apps ermöglichen. Durch Aktivieren der **Gerätesuche** wird der SSH-Server aktiviert.

Weitere Informationen zu diesen Features, oder wenn Sie Schwierigkeiten beim Installationsvorgang haben, finden Sie im [Entwicklermodus Features und Debuggen](developer-mode-features-and-debugging.md).

## <a name="see-also"></a>Weitere Informationen

* [Registrieren für ein Windows-Konto](sign-up.md)
* [Entwicklermodus-Features und-Debuggen](developer-mode-features-and-debugging.md).