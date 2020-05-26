---
title: Erstellen des Paketmanifests
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 054c8cf7fc104b78f0397f4d1536e1130668f4f8
ms.sourcegitcommit: 645cb099128072cfc905ec80b38bd280b98c9037
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 05/22/2020
ms.locfileid: "83825141"
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

Eine vollständige Liste und Beschreibungen der Elemente in einem Manifest finden Sie in der Manifestspezifikation im [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli)-Repository.

### <a name="minimal-required-schema"></a>Mindestens erforderliches Schema

#### <a name="minimal-required-schema"></a>[Mindestens erforderliches Schema](#tab/minschema/)

```yaml
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
Version: string # version numbering format
License: string # the open source license or copyright
InstallerType: string # enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx)
Installers:
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
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
Id: string # publisher.package format
Publisher: string # the name of the publisher
Name: string # the name of the application
AppMoniker: string # the common name someone may use to search for the package
Version: string # version numbering format for package version
Channel: string # a string representing the flight ring
License: string # the open source license or copyright
LicenseUrl: string # valid secure URL to license
MinOSVersion: string # version numbering format for minimum version of Windows supported
Description: string # description of the package
Homepage: string # valid secure URL for the package
Tags: list # additional strings a user would use to search for the package
FileExtensions: list # list of file extensions the package could support
Protocols: list # list of protocols the package provides a handler for
Commands: list # list of commands or aliases the user would use to run the package
InstallerType: string # enumeration of supported installer types (exe, msi, msix)
Custom: string # custom switches passed to the installer
Silent: string # switches passed to the installer for silent installation
SilentWithProgress: string # switches passed to the installer for non-interactive install
Interactive: string # experimental
Language: string # experimental
Log: string # specifies log redirection switches and path
InstallLocation: string # specifies alternate location to install package
Installers: # nested map of keys for specific installer
  - Arch: string # enumeration of supported architectures
  - URL: string # path to download installation file
  - Sha256: string # SHA256 calculated from installer
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file
  - Switches: # collection of entries to override root keys
  - Scope: string # experimental
  - SystemAppId: string # experimental
Localization: # nested map of keys for localization
  - Language: string # locale for display fields and localized URLs
ManifestVersion: string # version number format for manifest version
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

## <a name="tips-and-best-practices"></a>Tipps und bewährte Methoden

* Um eine optimale Benutzerfreundlichkeit der Such- und Installationsfunktionen für Ihre Software zu erzielen, wird empfohlen, dass Sie über das erforderliche Schema hinaus so viele optionale Elemente wie möglich einschließen. Beispielsweise ist das Feld `AppMoniker` optional. Wenn Sie dieses Feld jedoch einschließen, werden Kunden Ergebnisse angezeigt, die dem `AppMoniker`-Wert zugeordnet sind, wenn Sie den Befehl [search](../winget/search.md) ausführen (z. B. **vscode** für **Visual Studio Code**). Wenn nur eine App mit dem angegebenen `AppMoniker`-Wert vorhanden ist, können Kunden die Anwendung installieren, indem Sie anstelle der vollqualifizierten ID den Moniker angeben.
* Die `Id` muss eindeutig sein. Mehrere Übermittlungen mit dem gleichen Paketbezeichner sind nicht zulässig. Vermeiden Sie Leerzeichen, da Benutzer die `Id` ansonsten in Anführungszeichen setzen müssen, wenn Sie den [winget](../index.md)-Client verwenden.
* Vermeiden Sie das Erstellen mehrerer Ordner für den Herausgeber. Erstellen Sie z. B. keinen Ordner „Contoso Ltd“, wenn der Ordner „Contoso“ bereits vorhanden ist. Vermeiden Sie Leerzeichen auch beim Erstellen von Ordnern.
* Wenn möglich, sollten alle Pakete mit einer unbeaufsichtigten Installation übermittelt werden. Wenn Sie über eine ausführbare Datei verfügen, die keine unbeaufsichtigte Installation unterstützt, beeinträchtigt dies die Benutzerfreundlichkeit.
* Beschränken Sie die Länge der Zeichenfolgen im Manifest auf 100 Zeichen vor einem Zeilenumbruch.
* Wenn für die angegebene Version des Pakets mehr als ein Typ von Installationsprogramm vorhanden ist, kann eine Instanz von `InstallerType` unter jedem der `Installers` platziert werden.
