---
title: Installieren von PowerToys
description: Installieren Sie PowerToys, eine Reihe von Dienstprogrammen für die Anpassung von Windows 10, mithilfe einer ausführbaren Datei oder eines Paket-Managers (winget, Chocolatey, Scoop).
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: 7b6cf15e7d21eca9e24fcc2d81f9409b2cd94b6f
ms.sourcegitcommit: 884318ec5118cade85a31f4d5644436614e9f272
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 02/15/2021
ms.locfileid: "100524986"
---
# <a name="install-powertoys"></a>Installieren von PowerToys

Wir empfehlen die Installation von PowerToys mithilfe der weiter unten verknüpften Windows-Schaltfläche, aber alternative Installationsmethoden werden auch aufgeführt, wenn Sie die Verwendung eines Paket-Managers bevorzugen.

## <a name="install-with-windows-executable-file"></a>Mithilfe der ausführbaren Windows-Datei installieren

> [!div class="nextstepaction"]
> [Installieren von PowerToys](https://aka.ms/installpowertoys)

So installieren Sie PowerToys mithilfe einer ausführbaren Windows-Datei:

1. Besuchen Sie die Seite mit den [Microsoft PowerToys-GitHub-Releases](https://github.com/microsoft/PowerToys/releases/).
2. Durchsuchen Sie die Liste der verfügbaren stabilen und experimentellen Versionen von PowerToys.
3. Wählen Sie das Dropdown Menü **Ressourcen** aus, um die Dateien für das Release anzuzeigen.
4. Wählen Sie die `PowerToysSetup-0.##.#-x64.exe` Datei aus, um den ausführbaren Installer für PowerToys herunterzuladen
5. Öffnen Sie nach dem herunterladen die ausführbare Datei, und befolgen Sie die Installationsanweisungen.

## <a name="requirements"></a>Anforderungen

- Windows 10 1803 (Build 17134) oder höher.
- [.Net Core 3,1-Desktop Laufzeit](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer). Diese Anforderung wird vom PowerToys-Installer verarbeitet.
- x64-Architektur wird derzeit unterstützt. Arm-und x86-Unterstützung, um zu einem späteren Zeitpunkt verfügbar zu werden.

Um sicherzustellen, dass Ihr Computer diese Anforderungen erfüllt, überprüfen Sie Ihre Windows 10-Version und die Buildnummer, indem Sie den **⊞ Win** *(Windows-Taste)*  +  **R** auswählen, geben Sie dann **winver** und **OK** ein. (Oder geben Sie den Befehl `ver` an der Windows-Eingabeaufforderung ein.) Sie können im Menü " **Einstellungen** " [auf die neueste Windows-Version aktualisieren](ms-settings:windowsupdate) .

## <a name="alternative-install-methods"></a>Alternative Installationsmethoden

<!--  - **[Windows executable .exe file](#install-with-windows-executable-file)** *(Recommended)* -->
- [Windows-Paket-Manager](#install-with-windows-package-manager-preview) *(Vorschau)*
- Von der [Community gesteuerte Installations Tools](#community-driven-install-tools) *(nicht offiziell unterstützt)*

## <a name="install-with-windows-package-manager-preview"></a>Installieren mit dem Windows-Paket-Manager (Vorschau)

So installieren Sie PowerToys mithilfe der Windows-Paket-Manager-Vorschau (winget):

1. Laden Sie PowerToys vom [Windows-Paket-Manager](https://github.com/microsoft/winget-cli/releases)herunter.
2. Führen Sie den folgenden Befehl über die Befehlszeile/PowerShell aus:

```powershell
WinGet install powertoys
```

## <a name="community-driven-install-tools"></a>Von der Community gesteuerte Installations Tools

Diese von der Community gesteuerten Alternativen Installationsmethoden werden nicht offiziell unterstützt, und das PowerToys-Team aktualisiert oder verwaltet diese Pakete nicht.

### <a name="install-with-chocolatey"></a>Installieren mit Chocolatey

Führen Sie zum Installieren von PowerToys mithilfe von [Chocolatey](https://chocolatey.org/)den folgenden Befehl in der Befehlszeile/PowerShell aus:

```powershell
choco install powertoys
```

Führen Sie zum Aktualisieren von PowerToys Folgendes aus:

```powershell
choco upgrade powertoys
```

Wenn bei der Installation/Aktualisierung Probleme auftreten, besuchen Sie das [PowerToys-Paket auf chocolatey.org](https://chocolatey.org/packages/powertoys) , und befolgen Sie den [Prozess Chocolatey-selektivierungsprozess](https://chocolatey.org/docs/package-triage-process).

### <a name="install-with-scoop"></a>Mit der Schaufel installieren

Führen Sie den folgenden Befehl über die Befehlszeile/PowerShell aus, um PowerToys mithilfe von " [Scoop](https://scoop.sh/)" zu installieren:

```powershell
scoop bucket add extras
scoop install powertoys
```

Um PowerToys zu aktualisieren, führen Sie den folgenden Befehl über die Befehlszeile/PowerShell aus:

```powershell
scoop update powertoys
```

Wenn beim installieren/aktualisieren Probleme auftreten, melden Sie ein Problem im Scoop-Repository [auf GitHub](https://github.com/lukesampson/scoop/issues).
