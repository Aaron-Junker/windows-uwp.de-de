---
title: Bereitstellen einer App über die Registrierung loser Dateien
description: In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, Geräte Portal, App-Manager, Bereitstellung, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681931"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Bereitstellen einer App über die Registrierung loser Dateien 

In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken. Das Registrieren von locker-Datei Layouts ermöglicht Entwicklern, Ihre apps schnell zu überprüfen, ohne die apps Verpacken und installieren zu müssen. 

## <a name="what-is-a-loose-file-layout"></a>Was ist ein loses Datei Layout?

Das lose Datei Layout ist einfach das Platzieren von App-Inhalten in einem Ordner, anstatt den Verpackungsprozess durchlaufen zu müssen. Der Inhalt des Pakets ist "locker" in einem Ordner verfügbar und nicht gepackt. 

> [!WARNING]
> Eine lose dateilayoutregistrierung ist für Entwickler und Designer, die Ihre apps während der aktiven Entwicklung schnell validieren können. Diese Vorgehensweise sollte nicht verwendet werden, um die APP zu "Dogfood" oder zu fliegen. Es wird empfohlen, die abschließende Überprüfung für eine APP mit einem vertrauenswürdigen Zertifikat durchzuführen, die mit einem vertrauenswürdigen Zertifikat signiert ist. 

## <a name="advantages-of-loose-file-registration"></a>Vorteile der locker-Datei Registrierung

- **Schnelle Überprüfung** : da die APP-Dateien bereits entpackt sind, können Benutzer schnell das lose Datei Layout registrieren und die app starten. Ebenso wie eine reguläre App kann der Benutzer die APP so verwenden, wie er entworfen wurde. 
- **Einfache in-Network-Verteilung** : Wenn sich die lockeren Dateien auf einer Netzwerkfreigabe anstatt auf einem lokalen Laufwerk befinden, können Entwickler den Speicherort der Netzwerkfreigabe an andere Benutzer senden, die Zugriff auf das Netzwerk haben, und Sie können das lose Datei Layout registrieren und die app ausführen. Dadurch können mehrere Benutzer die APP gleichzeitig validieren. 
- **Zusammenarbeit** : die lose Datei Registrierung ermöglicht Entwicklern und Designern, weiterhin an visuellen Objekten zu arbeiten, während die APP registriert ist. Benutzern werden diese Änderungen angezeigt, wenn Sie die app starten. Beachten Sie, dass Sie statische Assets nur auf diese Weise ändern können. Wenn Sie Code oder dynamisch erstellte Inhalte ändern müssen, müssen Sie die APP erneut kompilieren.

## <a name="how-to-register-a-loose-file-layout"></a>Registrieren eines losen Datei Layouts

Windows bietet mehrere Entwicklertools zum Registrieren von lockeren Datei Layouts auf lokalen und Remote Geräten. Sie können aus `WinDeployAppCmd` (Windows SDK Tool), Windows-Geräte Portal, PowerShell und [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network)auswählen. Im folgenden wird erläutert, wie Sie mit diesen Tools lose Dateien registrieren. Stellen Sie zunächst sicher, dass Sie das Setup befolgen:

- Ihre Geräte müssen auf dem Windows 10 Creators Update (Build 14965) oder höher ausgeführt werden.
- Sie müssen den [Entwicklermodus](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) und die [Geräte](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) Ermittlung auf allen Geräten aktivieren.

> [!IMPORTANT]
> Die lose Datei Registrierung ist nur auf Geräten verfügbar, die das SMB-Protokoll (Network Share) unterstützen: Desktop und Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrieren bei windeployappcmd

Wenn Sie die SDK-Tools verwenden, die dem Windows 10 Creators Update (Build 14965) oder höher entsprechen, können Sie den `WinDeployAppCmd`-Befehl in einer Eingabeaufforderung verwenden.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Netzwerkpfad** – der Pfad zu den lockeren Dateien der app.

**IP-Adresse** – die IP-Adresse des Ziel Computers.

**PIN des Ziel** Computers – ggf. eine PIN zum Herstellen einer Verbindung mit dem Zielgerät. Wenn eine Authentifizierung erforderlich ist, werden Sie aufgefordert, die Option `-pin` erneut zu versuchen. Weitere Informationen zum erhalten einer PIN finden Sie unter [Device Discovery](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) .

### <a name="windows-device-portal"></a>Windows Geräteportal

Das Windows-Geräte Portal ist auf allen Windows 10-Geräten verfügbar und wird von Entwicklern verwendet, um Ihre Arbeit zu testen und zu überprüfen. Mit der Browser-UX und den Rest-Endpunkten werden alle Zielgruppen der Entwickler Community genutzt. Weitere Informationen zum Geräte Portal finden Sie in der [Übersicht über das Windows-Geräte Portal](device-portal.md).

Führen Sie die folgenden Schritte aus, um das Layout der losen Datei im Geräte Portal zu registrieren.

1. Stellen Sie eine Verbindung mit dem Geräte Portal her, indem Sie die Schritte im Abschnitt **Setup** der [Übersicht über das Windows-Geräte Portal](device-portal.md)ausführen.
1. Wählen Sie auf der Registerkarte Apps-Manager die Option **von Netzwerkfreigabe registrieren aus**.
1. Geben Sie den Netzwerkfreigabe Pfad zum Layout der losen Datei ein. 
1. Wenn das Hostgerät keinen Zugriff auf die Netzwerkfreigabe hat, wird eine Eingabeaufforderung zur Eingabe der erforderlichen Anmelde Informationen angezeigt.
1. Nachdem die Registrierung fertiggestellt wurde, können Sie die app starten.

Auf der Seite App-Manager des Geräte Portals können Sie auch optionale locker-Datei Layouts für Ihre Haupt-App registrieren, indem Sie das Kontrollkästchen **Ich möchte optionale Pakete angeben** aktivieren und dann die Netzwerkfreigabe Pfade der optionalen apps angeben. 

### <a name="powershell"></a>PowerShell 

Mit Windows PowerShell können Sie auch locker-Datei Layouts registrieren, jedoch nur auf dem lokalen Gerät. Wenn Sie ein Layout auf einem Remote Gerät registrieren müssen, müssen Sie eine der anderen Methoden verwenden. 

Um das Layout der losen Datei zu registrieren, starten Sie PowerShell, und geben Sie Folgendes ein.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Fehlerbehebung

### <a name="mapped-network-drives"></a>Zugeordnete Netzwerklaufwerke
Derzeit werden zugeordnete Netzwerklaufwerke für locker-Datei Registrierungen nicht unterstützt. Verweisen Sie auf das zugeordnete Laufwerk mit dem vollständigen Netzwerkfreigabe Pfad.

### <a name="registration-failure"></a>Registrierungsfehler
Das Gerät, auf dem die Registrierung stattfindet, muss auf das Datei Layout zugreifen können. Wenn das Datei Layout auf einer Netzwerkfreigabe gehostet wird, stellen Sie sicher, dass das Gerät Zugriff hat. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Änderungen an visuellen Objekten werden nicht in der App geladen. 
Die APP lädt Ihre visuellen Objekte zum Zeitpunkt der Startzeit. Wenn nach dem Starten der APP Änderungen an den visuellen Objekten vorgenommen wurden, müssen Sie die APP erneut starten, um die neuesten Änderungen anzuzeigen.
