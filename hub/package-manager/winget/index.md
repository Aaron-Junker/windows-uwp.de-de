---
title: Installieren und Verwalten von Anwendungen mit dem Tool „winget“
description: Mit dem Befehlszeilentool „winget“ können Entwickler Anwendungen auf Windows 10-Computern suchen, installieren, aktualisieren, entfernen und konfigurieren.
ms.date: 04/28/2020
ms.topic: overview
ms.localizationpriority: medium
ms.openlocfilehash: 4c918dccb2873f47a16669c195c47180e2129476
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89168744"
---
# <a name="use-the-winget-tool-to-install-and-manage-applications"></a>Installieren und Verwalten von Anwendungen mit dem Tool „winget“

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Mit dem Befehlszeilentool **winget** können Entwickler Anwendungen auf Windows 10-Computern suchen, installieren, aktualisieren, entfernen und konfigurieren. Dieses Tool ist die Clientschnittstelle für den Windows-Paket-Manager-Dienst.

Das Tool **winget** ist zurzeit eine Vorschauversion, sodass momentan noch nicht alle geplanten Funktionen verfügbar sind.

## <a name="install-winget"></a>Installation von „winget“

Es gibt mehrere Möglichkeiten zum Installieren des Tools **winget**:

* Das Tool **winget** ist in der Flight- oder Vorschauversion von [Windows App Installer](https://www.microsoft.com/p/app-installer/9nblggh4nns1?ocid=9nblggh4nns1_ORSEARCH_Bing&rtc=1&activetab=pivot:overviewtab) enthalten. Sie müssen die Vorschauversion von **App-Installer** installieren, um **winget** zu verwenden. Um früher Zugriff zu erhalten, senden Sie eine Anfrage an das [Insider-Programm für Windows-Paket-Manager](https://aka.ms/AppInstaller_InsiderProgram). Wenn Sie am Flight-Ring teilnehmen, sehen Sie immer die aktuellen Vorschau-Updates.

* [Am Windows-Insider-Flight-Ring](https://insider.windows.com) teilnehmen.

* Installieren Sie das App-Installer-Paket für Windows-Desktop im Release-Ordner des [Repositorys für „winget“](https://github.com/microsoft/winget-cli).

> [!NOTE]
> Für das Tool **winget** wird Windows 10, Version 1709 (10.0.16299) oder eine höhere Version von Windows 10 benötigt.

## <a name="administrator-considerations"></a>Überlegungen für Administratoren

Das Verhalten des Installationsprogramms kann abhängig davon unterschiedlich sein, ob Sie **winget** mit Administratorrechten ausführen.

* Wenn Sie **winget** ohne Administratorberechtigungen ausführen, benötigen einige Anwendungen für die Installation möglichweise [mehr Berechtigungen](https://docs.microsoft.com/windows/security/identity-protection/user-account-control/). Wenn das Installationsprogramm ausgeführt wird, werden Sie von Windows [zum Gewähren der entsprechenden Berechtigungen](https://docs.microsoft.com/windows/security/identity-protection/user-account-control) aufgefordert. Wenn Sie dies nicht tun, kann die Anwendung nicht installiert werden.  

* Wenn Sie **winget** an einer Administrator-Eingabeaufforderung ausführen, werden keine [Aufforderungen zum Gewähren weiterer Berechtigungen](/windows/security/identity-protection/user-account-control/how-user-account-control-works) angezeigt, wenn dies für die Anwendung erforderlich ist. Gehen Sie immer vorsichtig vor, wenn Sie die Eingabeaufforderung als Administrator ausführen, und installieren Sie nur Anwendungen, denen Sie vertrauen.

## <a name="use-winget"></a>Verwenden von „winget“

Nachdem **App-Installer** installiert wurde, können Sie **winget** ausführen, indem Sie an einer Eingabeaufforderung „winget“ eingeben.

Einer der häufigsten Anwendungsfälle ist das Suchen und Installieren eines bestimmten Tools.

1. Um nach einem Tool zu [suchen](search.md), geben Sie `winget search \<appname>` ein.
2. Nachdem Sie sich vergewissert haben, dass das gewünschte Tool verfügbar ist, können Sie das Tool [installieren](install.md), indem Sie `winget install \<appname>` eingeben. Das Tool **winget** startet das Installationsprogramm und installiert die Anwendung auf Ihrem PC.
    ![Befehlszeile für „winget“](images\install.png)

3. Zusätzlich zum Installieren und Suchen bietet **winget** eine Reihe weiterer Befehle, mit denen Sie [Details für Anwendungen anzeigen](show.md), [Quellen ändern](source.md) und [Pakete überprüfen](validate.md) können. Zum Anzeigen einer vollständigen Liste der Befehle geben Sie `winget --help` ein.
    ![Hilfe zu „winget“](images\help.png)

### <a name="commands"></a>Befehle

Die aktuelle Vorschau des Tools **winget** unterstützt die folgenden Befehle.

| Befehl | Beschreibung |
|---------|-------------|
| [hash](hash.md) | Generiert den SHA256-Hash für das Installationsprogramm. |
| [help](help.md) | Zeigt die Hilfe für die Befehle des Tools **winget** an. |
| [install](install.md) | Installiert die angegebene Anwendung. |
| [search](search.md) | Sucht nach einer Anwendung. |
| [show](show.md) | Zeigt Details für die angegebene Anwendung an. |
| [source](source.md) | Hiermit werden die Windows-Paket-Manager-Repositorys hinzugefügt, entfernt und aktualisiert, auf die das Tool **winget** zugreift. |
| [validate](validate.md) | Überprüft eine Manifestdatei, die an das Windows-Paket-Manager-Repository übermittelt werden soll. |

### <a name="options"></a>Optionen

Die aktuelle Vorschau des Tools **winget** unterstützt die folgenden Optionen.

| Option | Beschreibung |
|--------------|-------------|
| **-v,--version** | Mit dieser Option wird die aktuelle Version von „winget“ zurückgegeben. |
| **--info** |  Mit „info“ finden Sie detaillierte Informationen zu „winget“, einschließlich der Links zu den Lizenzbedingungen und der Datenschutzerklärung. |
| **-?, --help** |  Hiermit wird zusätzliche Hilfe zu „winget“ abgerufen. |

## <a name="supported-installer-formats"></a>Unterstützte Formate von Installationsprogrammen

Die aktuelle Vorschau des Tools **winget** unterstützt die folgenden Typen von Installationsprogrammen.

* EXE
* MSIX
* MSI

## <a name="scripting-winget"></a>Skripterstellung für „winget“

Sie können Batch-Skripts und PowerShell-Skripts erstellen, um mehrere Anwendungen zu installieren.

``` CMD
@echo off  
Echo Install Powertoys and Terminal  
REM Powertoys  
winget install Microsoft.Powertoys  
if %ERRORLEVEL% EQU 0 Echo Powertoys installed successfully.  
REM Terminal  
winget install Microsoft.WindowsTerminal  
if %ERRORLEVEL% EQU 0 Echo Terminal installed successfully.   %ERRORLEVEL%
```

> [!NOTE]
> Bei der Verwendung mit einem Skript startet **winget** die Anwendungen in der angegebenen Reihenfolge. Wenn ein Installationsprogramm eine Erfolgs- oder Fehlermeldung zurückgibt, startet **winget** das nächste Installationsprogramm. Wenn ein Installationsprogramm einen anderen Prozess startet, ist es möglich, dass es vorzeitig zu **winget** zurückkehrt. Dies führt dazu, dass **winget** das nächste Installationsprogramm startet, bevor das vorherige abgeschlossen wurde.

## <a name="missing-tools"></a>Fehlende Tools

Wenn das [Community-Repository](../package/repository.md) Ihr Tool oder Ihre Anwendung nicht enthält, übermitteln Sie ein Paket an das [Repository](https://github.com/microsoft/winget-pkgs). Wenn Sie das gesuchte Tool hinzufügen, steht es in Zukunft Ihnen und allen anderen Benutzern zur Verfügung.

## <a name="open-source-details"></a>Informationen zu Open Source

Das Tool **winget** ist Open Source-Software und auf GitHub im Repository [https://github.com/microsoft/winget-cli/](https://github.com/microsoft/winget-cli/) verfügbar. Die Quelle zum Erstellen des Clients befindet sich im [Ordner „src“](https://github.com/microsoft/winget-cli/tree/master/src).

Die Quelle für **winget** ist in einer Visual Studio 2019-C++-Projektmappe enthalten. Um die Lösung ordnungsgemäß zu erstellen, installieren Sie die [aktuelle Version von Visual Studio mit der C++ Workload](https://visualstudio.microsoft.com/downloads/).

Wir empfehlen Ihnen, an der **winget**-Quelle auf GitHub mitzuwirken. Sie müssen zunächst dem Microsoft CLA zustimmen und ihn signieren.