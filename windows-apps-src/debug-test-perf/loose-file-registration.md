---
title: Bereitstellen einer App über Registrieren loser Dateien
description: In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken.
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, Uwp, geräteportal, apps Manager, Bereitstellung, -sdk
ms.localizationpriority: medium
ms.openlocfilehash: 928c07bd23228f0fefd78be6019a0d116b2e6e4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635425"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Bereitstellen einer App über Registrieren loser Dateien 

In dieser Anleitung wird veranschaulicht, wie Sie das Layout für lose Dateien verwenden, um Windows 10-Apps zu überprüfen und freizugeben, ohne diese zu verpacken. Lose Dateilayouts registrieren kann Entwickler ihre apps ohne die Notwendigkeit zum Packen und installieren die apps schnell zu überprüfen. 

## <a name="what-is-a-loose-file-layout"></a>Was ist eine lose Dateilayout?

Lose Dateilayout wird einfach das Platzieren von app-Inhalte in einem Ordner, anstatt über den Prozess der paketerstellung. Der Inhalt des Pakets "lose" verfügbar sind in einem Ordner und nicht App-Pakete. 

> [!WARNING]
> Lose Datei-Layout-Registrierung ist für Entwickler und Designer, um ihre apps während der aktiven Entwicklung schnell zu überprüfen. Dieser Ansatz sollte nicht verwendet werden, um "Dogfood" oder die app flight. Es wird empfohlen, dass die letzte Überprüfung für ein app-Paket ausgeführt werden, die mit einem vertrauenswürdigen Zertifikat signiert wird. 

## <a name="advantages-of-loose-file-registration"></a>Vorteile der Registrierung von lose Dateien

- **Schnelle Überprüfung** – da die app-Dateien bereits entpackt sind, Benutzer können schnell registrieren Sie das Layout lose Datei und starten Sie die app. Genau wie eine normale app werden der Benutzer die app zu verwenden, während es entworfen wurde. 
- **Einfache im Netzwerk Verteilung** – Wenn losen Dateien sich in einer Netzwerkfreigabe statt einem lokalen Laufwerk befinden, Entwickler können die Netzwerkfreigabe-Speicherort für andere Benutzer mit Zugriff auf das Netzwerk senden und sie können das Layout lose Datei registrieren und Ausführen der -App. Dadurch können mehrere Benutzer gleichzeitig die app zu überprüfen. 
- **Zusammenarbeit** -lose Datei-Registrierung ermöglicht es, Entwicklern und Designern für visuelle Objekte Arbeit fortsetzen, während die app registriert ist. Benutzer sehen diese Änderungen auf, wenn sie die app starten. Beachten Sie, dass Sie nur statische Ressourcen, die auf diese Weise ändern können. Wenn Sie Code oder dynamisch erstellten Inhalt ändern möchten, müssen Sie die app neu kompilieren.

## <a name="how-to-register-a-loose-file-layout"></a>Vorgehensweise: Registrieren Sie ein Layout lose Datei

Windows bietet mehrere Entwicklertools, um lose Dateilayouts auf lokale und remote-Geräten zu registrieren. Sie wählen können `WinDeployAppCmd` (Windows SDK-Tool), Windows Device Portal, PowerShell, und [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Im folgenden werden wir zum Registrieren von loser Dateien, die mit diesen Tools besprochen. Aber stellen Sie zunächst sicher, dass Sie nach dem Setup verfügen:

- Ihre Geräte müssen auf dem Windows 10 Creators Update (Build 14965) oder höher sein.
- Sie müssen aktivieren [Entwicklermodus](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) und [Geräteermittlung](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) auf allen Geräten.

> [!IMPORTANT]
> Lose dateiregistrierung ist nur verfügbar, auf Geräten, die das Netzwerk-Dateifreigabe (SMB)-Protokoll unterstützt: Desktop und Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrieren bei WinDeployAppCmd

Wenn Sie die SDK-Tools, die entsprechenden, auf dem Windows 10 Creators Update (Build 14965) oder höher verwenden, können Sie mithilfe der `WinDeployAppCmd` in einer Eingabeaufforderung den Befehl.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Netzwerkpfad** – der Pfad zu der app lose Dateien.

**IP-Adresse** – die IP-Adresse des Zielcomputers.

**Zielcomputer PIN** – eine PIN, bei Bedarf zum Herstellen einer Verbindung mit dem Zielgerät. Werden Sie aufgefordert, den Verbindungsversuche mit Wiederholen der `-pin` option, wenn eine Authentifizierung erforderlich ist. Finden Sie unter [Geräteermittlung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) zu erfahren, wie Sie eine PIN zu erhalten.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal ist auf allen Windows 10-Geräten verfügbar und wird zum Testen und überprüfen ihre Arbeit von Entwicklern verwendet. Es eignet sich für alle Zielgruppen, die der Entwickler-Community mit der Browser-UX und REST-Endpunkte. Weitere Informationen zu Device Portal, finden Sie unter den [Übersicht über die Windows Device Portal](device-portal.md).

Um das Layout lose Datei in Device Portal registrieren zu können, gehen Sie wie folgt vor.

1. Verbinden mit Device Portal, indem Sie die Schritte in der **Setup** Teil der [Übersicht über die Windows Device Portal](device-portal.md).
1. Wählen Sie in der Registerkarte "Apps Manager" **registrieren, die von der Netzwerkfreigabe**.
1. Geben Sie den Netzwerkpfad für die Freigabe auf das Layout lose Datei ein. 
1. Wenn das Host-Gerät keinen Zugriff auf die Netzwerkfreigabe hat, wird eine Aufforderung zur Eingabe der erforderlichen Anmeldeinformationen vorhanden sein.
1. Sobald die Registrierung abgeschlossen ist, können Sie die app starten.

Für das Device Portal-Apps-Manager-Seite können Sie auch optionale lose Dateilayouts für Ihre Haupt-app registrieren, durch Auswählen der **optionale Pakete angeben möchte** Kontrollkästchen und dann das Netzwerk freigeben Pfade zu den optionalen apps . 

### <a name="powershell"></a>PowerShell 

Windows PowerShell können auch mit lose Dateilayouts, jedoch nur auf dem lokalen Gerät zu registrieren. Wenn Sie ein Layout für ein entferntes Gerät registrieren möchten, müssen Sie eine der anderen Methoden verwenden. 

Starten Sie PowerShell, und geben Sie Folgendes ein, um das Layout lose Datei zu registrieren.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Problembehandlung

### <a name="mapped-network-drives"></a>Zugeordnete Netzwerklaufwerke
Zugeordnete Netzwerklaufwerke werden derzeit für lose Datei Registrierungen nicht unterstützt. Finden Sie in das zugeordnete Laufwerk mit vollständigen den Netzwerkpfad für die Freigabe aus.

### <a name="registration-failure"></a>Registrierungsfehler
Das Gerät, auf dem die Registrierung durchgeführt wird, müssen auf das Dateilayout zugreifen. Wenn das Dateilayout auf einer Netzwerkfreigabe gehostet wird, stellen Sie sicher, dass das Gerät zugreifen kann. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Änderungen an der visuellen Anlagen werden nicht in der app geladen wird 
Die app lädt die visuelle Objekte zur Startzeit. Wenn Änderungen an der visuelle Objekte nach dem Starten der app vorgenommen wurden, müssen Sie die app aus, um die neuesten Änderungen anzuzeigen erneut starten.