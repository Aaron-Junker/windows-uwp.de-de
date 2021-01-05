---
title: Installieren von PowerToys
description: Installieren Sie PowerToys, eine Reihe von Dienstprogrammen für die Anpassung von Windows 10, mithilfe einer ausführbaren Datei oder eines Paket-Managers (winget, Chocolatey, Scoop).
ms.date: 12/02/2020
ms.topic: quickstart
ms.localizationpriority: medium
ms.openlocfilehash: c695be3784ff2145788e4887b5f9022fcc7a6999
ms.sourcegitcommit: ea1115b921d18c7bbddc95dba9275568ff57af02
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 12/28/2020
ms.locfileid: "97794226"
---
# <a name="install-powertoys"></a>Installieren von PowerToys

Es gibt mehrere Möglichkeiten zum Installieren von PowerToys:

- **[Ausführbare EXE-Datei für Windows](#install-with-windows-executable-file)** *(empfohlen)*
- [Windows-Paket-Manager](#install-with-windows-package-manager-preview) *(Vorschau)*
- Von der [Community gesteuerte Installations Tools](#community-driven-install-tools) *(nicht offiziell unterstützt)*

## <a name="requirements"></a>Anforderungen

- Windows 10 1803 (Build 17134) oder höher.
- [.Net Core 3,1-Desktop Laufzeit](https://dotnet.microsoft.com/download/dotnet-core/thank-you/runtime-desktop-3.1.4-windows-x64-installer). Diese Anforderung wird vom PowerToys-Installer verarbeitet.

Um sicherzustellen, dass Ihr Computer diese Anforderungen erfüllt, überprüfen Sie Ihre Windows 10-Version und die Buildnummer, indem Sie den **⊞ Win** *(Windows-Taste)*  +  **R** auswählen, geben Sie dann **winver** und **OK** ein. (Oder geben Sie den Befehl `ver` an der Windows-Eingabeaufforderung ein.) Sie können im Menü " **Einstellungen** " [auf die neueste Windows-Version aktualisieren](ms-settings:windowsupdate) .

## <a name="install-with-windows-executable-file"></a>Mithilfe der ausführbaren Windows-Datei installieren

So installieren Sie PowerToys mithilfe einer ausführbaren Windows-Datei:

1. Besuchen Sie die Seite mit den [Microsoft PowerToys-GitHub-Releases](https://github.com/microsoft/PowerToys/releases/).
2. Durchsuchen Sie die Liste der verfügbaren stabilen und experimentellen Versionen von PowerToys.
3. Wählen Sie das Dropdown Menü **Ressourcen** aus, um die Dateien für das Release anzuzeigen.
4. Wählen Sie die `PowerToysSetup-0.##.#-x64.exe` Datei aus, um den ausführbaren Installer für PowerToys herunterzuladen
5. Öffnen Sie nach dem herunterladen die ausführbare Datei, und befolgen Sie die Installationsanweisungen.

**Derzeit ist dies die empfohlene Installationsmethode.**

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
scoop install powertoys
```

Um PowerToys zu aktualisieren, führen Sie den folgenden Befehl über die Befehlszeile/PowerShell aus:

```powershell
scoop update powertoys
```

Wenn beim installieren/aktualisieren Probleme auftreten, melden Sie ein Problem im Scoop-Repository [auf GitHub](https://github.com/lukesampson/scoop/issues).
