---
Description: Verwenden Sie UTF-8-Zeichen, die Codierung für eine optimale Kompatibilität zwischen Web-apps und andere * Nix-basierten Plattformen (Unix, Linux und Varianten), Lokalisierung, Fehler, und zu verringern Verwaltungsaufwand testen.
title: Verwenden Sie die Windows-UTF-8-Codepage
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 453d58b0d52aaa24461784b6f393b26b93e572a1
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/11/2019
ms.locfileid: "67827376"
---
# <a name="use-the-utf-8-code-page"></a>Verwenden Sie die UTF-8-Codepage

Verwendung [UTF-8](http://www.utf-8.com/) zeichencodierung für eine optimale Kompatibilität zwischen Web-apps und andere * Nix-basierten Plattformen (Unix, Linux und Varianten), Lokalisierung, Fehler, und zu verringern Verwaltungsaufwand testen.

UTF-8 ist die universelle Codepage für die Internationalisierung und alle Unicode-Codepunkte mit variabler Breite 1 bis 4-Byte-Codierung unterstützt. Sie wird im Web sehr häufig verwendet, und ist die Standardeinstellung für * Nix-basierte Plattformen.

## <a name="-a-vs--w-apis"></a>Ein - oder -W-APIs
  
Win32-APIs unterstützen häufig sowohl – eine und -W-Varianten.

Ein - Varianten erkennen die ANSI-Codepage, die auf dem System und den Support Char *, konfiguriert werden, während -W-Varianten in UTF-16 und Support arbeiten `WCHAR`.

Bis vor kurzem hat Windows -W "Unicode"-Varianten über – ein APIs hervorgehoben. Allerdings den letzten Veröffentlichungen haben die ANSI-Codepage verwendet, und - A-APIs als eine Möglichkeit zum Einführen von UTF-8-Unterstützung für apps. Wenn die ANSI-Codepage für UTF-8 konfiguriert ist, arbeiten – eine APIs in UTF-8. Dieses Modell hat den Vorteil für die Unterstützung von vorhandenen Codes Dank – ein APIs ohne codeänderungen.

## <a name="set-a-process-code-page-to-utf-8"></a>Legen Sie eine Prozess-Codepage in UTF-8

Sie können erzwingen, dass einen Prozess mit UTF-8 als Codepage Prozess über die appxmanifest-Datei für App-Pakete oder das Fusion-Manifest für entpackt apps, die mithilfe der ActiveCodePage-Eigenschaft.

Sie können diese Eigenschaft deklarieren und Ziel/ausführen, auf früheren Windows-builds, jedoch müssen Sie behandeln Erkennung von Legacycode Seiten und die Konvertierung wie gewohnt (mit eine minimale Zielversion von 19-H-1, die Codepage für den Prozess immer werden UTF-8).

## <a name="examples"></a>Beispiele

**AppX-Manifests für ein app-Paket:**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Fusion-Manifest für eine entpackt Win32-app:**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> Fügen Sie ein Manifest über die Befehlszeile mit einer vorhandenen ausführbaren Datei `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Codepagekonvertierung

Wie Windows systemintern in UTF-16 (WCHAR) ausgeführt wird, müssen Sie für die Zusammenarbeit mit Windows APIs UTF-8-Daten in UTF-16 (oder umgekehrt) zu konvertieren.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) und [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) können Sie zwischen UTF-8 und UTF-16 (WCHAR) (und anderen Codepages) konvertieren. Dies ist besonders nützlich, wenn eine ältere Win32-API nur WCHAR verstehen können. Diese Funktionen können Sie zum Konvertieren der UTF-8-Eingabe in WCHAR zum Übergeben an eine -W-API- und konvertieren dann Ergebnisse zurück, falls erforderlich.
Bei Verwendung dieser Funktionen in Windows mit der CodePage CP_UTF8 verwenden Sie DwFlags, der entweder 0 oder MB_ERR_INVALID_CHARS, andernfalls tritt ein ERROR_INVALID_FLAGS auf.

Hinweis: CP_ACP ist gleichbedeutend mit der CP_UTF8 nur dann, wenn auf Windows-Version 1903 ausgeführt (Mai 2019-Update), und die oben beschriebene ActiveCodePage-Eigenschaft wird in UTF-8 festgelegt. Andernfalls wird es die Codepage des älteren Systems berücksichtigt. Es wird empfohlen, explizite Verwendung der CP_UTF8.

## <a name="related-topics"></a>Verwandte Themen

- [Codepages](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Codepage-IDs](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
