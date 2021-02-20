---
title: Bereitstellen einer App über die Registrierung loser Dateien
description: In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Geräteportal, App-Manager, Bereitstellung, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 34302b421f51fcc9fdf408baabc178190c7ed335
ms.sourcegitcommit: 2b7f6fdb3c393f19a6ad448773126a053b860953
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 02/12/2021
ms.locfileid: "100334890"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Bereitstellen einer App über die Registrierung loser Dateien 

In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken. Das Registrieren von losen Dateilayouts ermöglicht Entwicklern, ihre Apps schnell zu überprüfen, ohne die Apps packen und installieren zu müssen. 

## <a name="what-is-a-loose-file-layout"></a>Was ist ein loses Dateilayout?

Ein loses Dateilayout ist ganz einfach das Platzieren von App-Inhalten in einem Ordner, anstatt den Packprozess durchlaufen zu müssen. Der Inhalt des Pakets wird „lose“ in einem Ordner verfügbar gemacht und nicht gepackt. 

> [!WARNING]
> Über die Registrierung eines losen Dateilayouts können Entwickler und Designer ihre Apps während der aktiven Entwicklung schnell validieren. Dieses Konzept sollte nicht dazu verwendet werden, die App als „Dogfood“ oder Flight bereitzustellen. Es wird empfohlen, die abschließende Überprüfung mit einer gepackten App durchzuführen, die mit einem vertrauenswürdigen Zertifikat signiert wurde. 

## <a name="advantages-of-loose-file-registration"></a>Vorteile der Registrierung loser Dateien

- **Schnelle Überprüfung:** Da die App-Dateien bereits entpackt sind, können Benutzer das lose Dateilayout schnell registrieren und die App starten. Genau wie bei einer normalen App kann der Benutzer die App gemäß ihrem Entwurf verwenden. 
- **Einfache Verteilung im Netzwerk:** Wenn sich die losen Dateien in einer Netzwerkfreigabe befinden und nicht auf einem lokalen Laufwerk, können Entwickler den Netzwerkfreigabe-Speicherort an andere Benutzer senden, die Zugriff auf das Netzwerk haben, und sie können das lose Dateilayout registrieren und die App ausführen. Dadurch können mehrere Benutzer die App gleichzeitig validieren. 
- **Zusammenarbeit:** Die Registrierung loser Dateien ermöglicht Entwicklern und Designern, weiterhin an visuellen Objekten zu arbeiten, nachdem die App registriert wurde. Benutzer sehen diese Änderungen, wenn sie die App starten. Beachte, dass du auf diese Weise nur statische Objekte ändern kannst. Wenn du Code oder dynamisch erstellte Inhalte änderst, musst du die App neu kompilieren.

## <a name="how-to-register-a-loose-file-layout"></a>Registrieren eines losen Dateilayouts

Windows bietet mehrere Entwicklertools zum Registrieren von losen Dateilayouts auf lokalen und Remotegeräten. Du kannst zwischen `WinAppDeployCmd` (Windows SDK-Tool), Windows-Geräteportal, PowerShell und [Visual Studio](./deploying-and-debugging-uwp-apps.md#register-layout-from-network) auswählen. Im Folgenden wird erläutert, wie du mit diesen Tools lose Dateien registrierst. Stelle zunächst sicher, dass du über das folgende Setup verfügst:

- Auf deinen Geräten muss Windows 10 Creators Update (Build 14965) oder höher ausgeführt werden.
- Du musst den [Entwicklermodus](/windows/apps/get-started/enable-your-device-for-development) und die [Geräteerkennung](/windows/apps/get-started/enable-your-device-for-development#device-discovery) auf allen Geräten aktivieren.

> [!IMPORTANT]
> Die Registrierung loser Dateien ist nur auf Geräten verfügbar, die das SMB-Protokoll für Netzwerkfreigaben unterstützen: Desktop und Xbox. 

### <a name="register-with-winappdeploycmd"></a>Registrieren bei WinAppDeployCmd

Wenn du die SDK-Tools für das Windows 10 Creators Update (Build 14965) oder höher verwendest, kannst du den Befehl `WinAppDeployCmd` in einer Eingabeaufforderung verwenden.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Netzwerkpfad:** Pfad zu den losen Dateien der App.

**IP-Adresse:** IP-Adresse des Zielcomputers.

**PIN des Zielcomputers:** PIN, sofern zum Herstellen einer Verbindung mit dem Zielgerät erforderlich. Wenn eine Authentifizierung erforderlich ist, wirst du aufgefordert, den Versuch mit der Option `-pin` zu wiederholen. Weitere Informationen zum Erhalten einer PIN findest du unter [Geräteerkennung](/windows/apps/get-started/enable-your-device-for-development#device-discovery).

### <a name="windows-device-portal"></a>Windows-Geräteportal

Das Windows-Geräteportal ist auf allen Windows 10-Geräten verfügbar und wird von Entwicklern verwendet, um ihre Arbeit zu testen und zu validieren. Mit der Browserbenutzeroberfläche und den REST-Endpunkten wendet es sich an alle Zielgruppen der Entwicklercommunity. Weitere Informationen zum Geräteportal findest du unter [Übersicht über das Windows-Geräteportal](device-portal.md).

Führe die folgenden Schritte aus, um das lose Dateilayout im Geräteportal zu registrieren.

1. Stelle über die Schritte im Abschnitt **Setup** der [Übersicht über das Windows-Geräteportal](device-portal.md) eine Verbindung mit dem Geräteportal her.
1. Wähle auf der Registerkarte „Apps Manager“ die Option **Aus Netzwerk registrieren** aus.
1. Gib den Netzwerkfreigabepfad zum losen Dateilayout ein. 
1. Wenn das Hostgerät keinen Zugriff auf die Netzwerkfreigabe hat, wird eine Aufforderung zum Eingeben der erforderlichen Anmeldeinformationen angezeigt.
1. Nach Abschluss der Registrierung kannst du die App starten.

Auf der Seite „Apps Manager“ des Geräteportals kannst du auch optional lose Dateilayouts für deine Haupt-App registrieren. Aktiviere dazu das Kontrollkästchen **I want to specify optional packages** (Ich möchte optionale Pakete angeben), und gib dann die Netzwerkfreigabepfade der optionalen Apps an. 

### <a name="powershell"></a>PowerShell 

Du kannst auch mit Windows PowerShell lose Dateilayouts registrieren, jedoch nur auf dem lokalen Gerät. Wenn du ein Layout auf einem Remotegerät registrieren möchtest, musst du eine der anderen Methoden verwenden. 

Um das lose Dateilayout zu registrieren, starte PowerShell, und gib Folgendes ein.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="mapped-network-drives"></a>Zugeordnete Netzwerklaufwerke
Derzeit werden zugeordnete Netzwerklaufwerke für Registrierungen loser Dateien nicht unterstützt. Verweise mit dem vollständigen Netzwerkfreigabepfad auf das zugeordnete Laufwerk.

### <a name="registration-failure"></a>Registrierungsfehler
Das Gerät, auf dem die Registrierung stattfindet, muss auf das Dateilayout zugreifen können. Wenn das Dateilayout auf einer Netzwerkfreigabe gehostet wird, stelle sicher, dass das Gerät Zugriff hat. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Änderungen an visuellen Objekten werden nicht in die App geladen. 
Die App lädt ihre visuellen Objekte zum Startzeitpunkt. Wenn nach dem Starten der App Änderungen an den visuellen Objekten vorgenommen werden, musst du die App erneut starten, um die aktuellen Änderungen anzuzeigen.
