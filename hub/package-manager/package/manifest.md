---
title: Erstellen des Paketmanifests
description: Wenn Sie ein Softwarepaket an das Windows-Paket-Manager-Repository übermitteln möchten, erstellen Sie zunächst ein Paketmanifest.
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 4b2c42fb8a9f8eb741ce253c3ea110fe4eeb10a3
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89166494"
---
# <a name="create-your-package-manifest"></a>Erstellen des Paketmanifests

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Wenn Sie ein Softwarepaket an das [Windows-Paket-Manager-Repository](repository.md) übermitteln möchten, erstellen Sie zunächst ein Paketmanifest. Das Manifest ist eine YAML-Datei mit einer Beschreibung der zu installierenden Anwendung.

In diesem Artikel wird der Inhalt eines Paketmanifests für den Windows-Paket-Manager beschrieben.

## <a name="yaml-basics"></a>YAML-Grundlagen

Das YAML-Format wurde aus Gründen der relativ guten Lesbarkeit und der Konsistenz mit anderen Microsoft-Entwicklungstools für Paketmanifeste ausgewählt. Wenn Sie mit der YAML-Syntax nicht vertraut sind, können Sie sich die Grundlagen unter [Learn YAML in Y Minutes](https://learnxinyminutes.com/docs/yaml/) (Erlernen von YAML in Y Minuten) aneignen.

> [!NOTE]
> Manifeste für den Windows-Paket-Manager unterstützen zurzeit nicht alle YAML-Features. Zu den nicht unterstützten YAML-Features zählen Anker, komplexe Schlüssel und Sätze.

## <a name="conventions"></a>Konventionen

In diesem Artikel werden die folgenden Konventionen verwendet:

* Links von `:` steht ein Literalschlüsselwort, das in Manifestdefinitionen verwendet wird.
* Rechts von `:` steht ein Datentyp. Der Datentyp kann ein primitiver Typ wie **string** oder ein Verweis auf eine umfangreiche Struktur sein, die an anderer Stelle in diesem Artikel definiert ist.
* Die Schreibweise `[` *Datentyp* `]` gibt ein Array des erwähnten Datentyps an. Beispielsweise steht `[ string ]` für ein Zeichenfolgenarray.
* Die Schreibweise `{` *Datentyp* `:` *Datentyp* `}` gibt die Zuordnung des einen Datentyps zum anderen an. Beispielsweise ist `{ string: string }` eine Zuordnung von Zeichenfolgen zu Zeichenfolgen.

## <a name="manifest-contents"></a>Inhalt des Manifests

Ein Paketmanifest muss eine Reihe erforderlicher Elemente enthalten und kann darüber hinaus weitere optionale Elemente enthalten, die zu einer höheren Benutzerfreundlichkeit der Softwareinstallation beitragen können. Dieser Abschnitt enthält kurze Zusammenfassungen des erforderlichen Manifestschemas und vollständige Manifestschemas sowie Beispiele für beides.

Jedes Feld in der Manifestdatei muss die Pascal-Schreibweise verwenden und darf nicht doppelt vorkommen.

Eine vollständige Liste und Beschreibungen der Elemente in einem Manifest finden Sie in der [Manifestspezifikation](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md) im [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli)-Repository.

### <a name="minimal-required-schema"></a>Mindestens erforderliches Schema

#### <a name="minimal-required-schema"></a>[Mindestens erforderliches Schema](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
  - Url: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[Beispiel](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>Vollständiges Schema

#### <a name="complete-schema"></a>[Vollständiges Schema](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Custom: string # Custom switches passed to the installer.
Silent: string # Switches passed to the installer for silent installation.
SilentWithProgress: string # Switches passed to the installer for non-interactive install.
Interactive: string # Experimental.
Language: string # Experimental.
Log: string # Specifies log redirection switches and path.
InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
  - Url: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
  - Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
  - Scope: string # Experimental.
  - SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[Gutes Beispiel](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[Besseres Beispiel](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> Wenn Ihr Installationsprogramm eine EXE-Datei ist und mit Nullsoft oder Inno erstellt wurde, können Sie stattdessen diese Werte angeben. Wird Nullsoft oder Inno angegeben, legt der Client automatisch die Werte „Silent“ (Unbeaufsichtigt) und „Silent with Progress“ (Unbeaufsichtigt mit Statusanzeige) für das Installationsverhalten des Installationsprogramms fest.

## <a name="installer-switches"></a>Installer-Switches

Sie können oft herausfinden, welche automatischen `Switches` für ein Installationsprogramm verfügbar sind, indem Sie ein `-?` über die Befehlszeile an das Installationsprogramm übergeben. Hier sehen Sie einige gebräuchliche automatische `Swtiches`, die für verschiedene Installertypen verwendet werden können.

| Installer | Befehl  | Dokumentation |  
| :--- | :-- | :--- |  
| MSI | `/q` | [Befehlszeilenoptionen für MSI](/windows/win32/msi/command-line-options) |
| InstallShield | `/s`  | [InstallShield-Befehlszeilenparameter](https://docs.flexera.com/installshield19helplib/helplibrary/IHelpSetup_EXECmdLine.htm) |
| Inno Setup | `/SILENT or /VERYSILENT` | [Inno Setup-Dokumentation](https://jrsoftware.org/ishelp/) |
| Nullsoft | `/S` | [Nullsoft-Installer für die Installation/Deinstallation im Hintergrund](https://nsis.sourceforge.io/Docs/Chapter4.html#silent) |

## <a name="tips-and-best-practices"></a>Tipps und bewährte Methoden

* Um eine optimale Benutzerfreundlichkeit der Such- und Installationsfunktionen für Ihre Software zu erzielen, wird empfohlen, dass Sie über das erforderliche Schema hinaus so viele optionale Elemente wie möglich einschließen. Beispielsweise ist das Feld `AppMoniker` optional. Wenn Sie dieses Feld jedoch einschließen, werden Kunden Ergebnisse angezeigt, die dem `AppMoniker`-Wert zugeordnet sind, wenn Sie den Befehl [search](../winget/search.md) ausführen (z. B. **vscode** für **Visual Studio Code**). Wenn nur eine App mit dem angegebenen `AppMoniker`-Wert vorhanden ist, können Kunden die Anwendung installieren, indem Sie anstelle der vollqualifizierten ID den Moniker angeben.
* Die `Id` muss eindeutig sein. Mehrere Übermittlungen mit dem gleichen Paketbezeichner sind nicht zulässig. Vermeiden Sie Leerzeichen, da Benutzer die `Id` ansonsten in Anführungszeichen setzen müssen, wenn Sie den [winget](../index.md)-Client verwenden.
* Vermeiden Sie das Erstellen mehrerer Ordner für den Herausgeber. Erstellen Sie z. B. keinen Ordner „Contoso Ltd“, wenn der Ordner „Contoso“ bereits vorhanden ist. Vermeiden Sie Leerzeichen auch beim Erstellen von Ordnern.
* Wenn möglich, sollten alle Pakete mit einer unbeaufsichtigten Installation übermittelt werden. Wenn Sie über eine ausführbare Datei verfügen, die keine unbeaufsichtigte Installation unterstützt, beeinträchtigt dies die Benutzerfreundlichkeit.
* Beschränken Sie die Länge der Zeichenfolgen im Manifest auf 100 Zeichen vor einem Zeilenumbruch.
* Wenn für die angegebene Version des Pakets mehr als ein Typ von Installationsprogramm vorhanden ist, kann eine Instanz von `InstallerType` unter jedem der `Installers` platziert werden.