---
title: Problembehandlung bei der Installation der App-Installer-Datei
description: Allgemeine Probleme beim Querladen von Anwendungen mit der App-Installer-Datei.
ms.date: 05/02/2018
ms.topic: article
keywords: windows 10, uwp, app-installer, AppInstaller, querladen
ms.localizationpriority: medium
ms.openlocfilehash: f74a7bbfdfd5abe6a32c0ca3d81000e6f2d4758d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57660145"
---
# <a name="troubleshoot-installation-issues-with-the-app-installer-file"></a>Problembehandlung bei der Installation der App-Installer-Datei

Wenn Sie beim Installieren von Anwendungen aus der App-Installer-Datei Probleme feststellen, finden Sie in diesem Thema einige Anleitungen zur Problembehandlung, die hilfreich sein können.

## <a name="prerequisites"></a>Voraussetzungen

Um in Windows 10 Apps querladen zu können, muss das Gerät des Benutzers die folgenden Anforderungen erfüllen:

- Das Gerät muss für den Entwicklermodus bzw. für das Querladen von Apps aktiviert sein. Weitere Informationen finden Sie unter [Aktivieren Ihres Geräts für die Entwicklung](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
- Das zum Signieren des Pakets verwendete Zertifikat muss vom Gerät als vertrauenswürdig eingestuft werden. Im nachstehenden Abschnitt **Vertrauenswürdige Zertifikate** finden Sie weitere Informationen.
- Die Windows 10-Version muss das `.appinstaller`-Dateischema und das Verteilungsprotokoll unterstützen.

## <a name="common-issues"></a>Allgemeine Probleme

Es gibt einige allgemeine Probleme beim erstmaligen Querladen einer Anwendung auf dem Benutzergerät. In den nächsten Abschnitten werden die häufigsten Probleme und ihre Lösungen beschrieben.

### <a name="windows-version"></a>Windows-Version

In jeder Windows 10-Version wird der Querladevorgang weiterentwickelt. In der folgenden Tabelle finden Sie die Features, die in jeder Hauptversion verfügbar sind. Wenn Sie versuchen, eine App mit einer Methode querzuladen, die von Ihrer Windows 10-Version nicht unterstützt wird, erhalten Sie einen Bereitstellungsfehler.

| Version | Anmerkungen zum Querladen |
|---------|----------------|
| Build 17134 (April 2018-Update, Version 1804)    | Auf die `.appinstaller`-Datei kann über UNC-/Freigabeordner zugegriffen werden. Konfigurierbare Update-Prüfungen sind ebenfalls verfügbar. |
| Build 16299 (Fall Creators Update, Version 1709) | Einführung der `.appinstaller`-Datei, um automatische Updates für Ihre App bereitzustellen. Diese Version unterstützt nur HTTP-Endpunkte. Update-Prüfungen sind nicht konfigurierbar und erfolgen nur alle 24 Stunden. |
| Build 15063 (Creators Update, Version 1703)      | Die App-Installer-App kann App-Abhängigkeiten (nur im Releasemodus) aus dem Store herunterladen. |
| Build 14393 (Anniversary Update, Version 1607)   | Einführung der App-Installer-App zum Installieren von .appx- und .appxbundle-Dateien. Die .appinstaller-Datei wird nicht unterstützt. |
| Build 10586 (November Update, Version 1511)      | Querladen steht nur über PowerShell mit dem [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)-Befehl zur Verfügung. |
| Build 10240 (Windows 10, Version 1507)           | Querladen steht nur über PowerShell mit dem [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps)-Befehl zur Verfügung. |

### <a name="trusted-certificates"></a>Vertrauenswürdige Zertifikate

Das App-Paket muss anhand eines Zertifikats signiert werden, das vom Gerät als vertrauenswürdig eingestuft wird. Im Windows-Betriebssystem werden durch allgemeine Zertifizierungsstellen bereitgestellte Zertifikate standardmäßig als vertrauenswürdig eingestuft, wenn jedoch das Zertifikat nicht vertrauenswürdig ist, muss es auf dem Gerät installiert werden, **bevor** die App installiert wird. Um dem Zertifikat zu vertrauen, muss das Zertifikat in einem der folgenden Zertifikatspeicher des lokalen Computers auf dem Gerät vorhanden sein:

- Vertrauenswürdige Herausgeber
- Vertrauenswürdige Personen
- Vertrauenswürdige Stammzertifizierungsstellen (nicht empfohlen)

 >[!IMPORTANT]
 > Für das Installieren eines Zertifikats im Speicher des lokalen Computers ist Administratorzugriff erforderlich.

### <a name="dependencies-not-installed"></a>Nicht installierte Abhängigkeiten 

UWP-Anwendungen können abhängig von der Anwendungsplattform, die zum Generieren der App verwendet wird, Frameworkabhängigkeiten aufweisen. Wenn Sie C# oder VB verwenden, erfordert die App die .NET-Laufzeit- und .NET Framework-Pakete. Für C++-Anwendungen wird VCLibs benötigt.

>[!IMPORTANT] 
> Wenn das App-Paket in der Releasemodus-Konfiguration erstellt wurde, werden die Frameworkabhängigkeiten aus dem Microsoft Store abgerufen. Wurde die App hingegen in der Debugging-Modus-Konfiguration erstellt, werden die Abhängigkeiten von dem in der `.appinstaller`-Datei angegebenen Speicherort abgerufen.

### <a name="files-not-accessible"></a>Auf die Dateien kann nicht zugegriffen werden

Bei der Installation von einem HTTP-Endpunkt aus muss überprüft werden, dass auf alle Dateien mit dem richtigen MIME-Typ zugegriffen werden kann. Die einfachste Methode zum Überprüfen dieser Dateien besteht darin, den Links auf der von Visual Studio generierten HTML-Seite zu folgen. Sie müssen die folgenden Dateien überprüfen:

- `.appinstaller` Datei als ein `application/xml`
- `.appx` und `.appxbundle` -Dateien, als verfügbar `application/vns.ms-appx`

## <a name="isolate-app-installer-app-issues"></a>Isolieren von App-Installer App-Problemen

Wenn die App-Installer-App die App nicht installieren kann, helfen diese Schritte, das Installationsproblem zu identifizieren.

### <a name="verify-app-package-file-installation"></a>Überprüfen Sie die app-Paket-Datei-installation

- Herunterladen der app-Paketdatei an einen lokalen Ordner aus, und versuchen Sie, installieren Sie es mithilfe der [Add-AppxPackage](https://docs.microsoft.com/powershell/module/appx/add-appxpackage?view=win10-ps) PowerShell-Befehl.

- Laden Sie die `.appinstaller`-Datei in einen lokalen Ordner herunter und versuchen Sie sie mithilfe des `Add-AppxPackage -Appinstaller`-PowerShell-Befehls zu installieren.

## <a name="related-logs"></a>Zugehörige Protokolle

Die Bereitstellungsinfrastruktur der App stellt Protokolle zum Debuggen in der Windows-Ereignisanzeige bereit. Diese Protokolle befinden sich hier: `Application and Services Logs->Microsoft->Windows->AppxDeployment-Server`



