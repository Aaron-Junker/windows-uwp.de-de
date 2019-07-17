---
Description: Verwenden Sie UTF-8-Zeichen, die Codierung für eine optimale Kompatibilität zwischen Web-apps und andere * Nix-basierten Plattformen (Unix, Linux und Varianten), Lokalisierung, Fehler, und zu verringern Verwaltungsaufwand testen.
title: Verwenden Sie die Windows-UTF-8-Codepage
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisierbarkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141807"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8-Codepage verwenden

Verwendung [UTF-8](http://www.utf-8.com/) zeichencodierung für eine optimale Kompatibilität zwischen Web-apps und andere * Nix-basierten Plattformen (Unix, Linux und Varianten), Lokalisierung, Fehler, und zu verringern Verwaltungsaufwand testen.

UTF-8 ist die universelle Codepage für die Internationalisierung und alle Unicode-Codepunkte mit variabler Breite 1 bis 4-Byte-Codierung unterstützt. Sie wird im Web sehr häufig verwendet, und ist die Standardeinstellung für * Nix-basierte Plattformen.

## <a name="-a-vs--w-apis"></a>Ein - oder -W-APIs
  
Win32-APIs unterstützen häufig sowohl – eine und -W-Varianten.

Ein - Varianten erkennen die ANSI-Codepage, die auf das System und die Unterstützung konfiguriert `char*`, während -W-Varianten in UTF-16 und Support betreiben `WCHAR`.

Bis vor kurzem hat Windows -W "Unicode"-Varianten über – ein APIs hervorgehoben. Allerdings den letzten Veröffentlichungen haben die ANSI-Codepage verwendet, und - A-APIs als eine Möglichkeit zum Einführen von UTF-8-Unterstützung für apps. Wenn die ANSI-Codepage für UTF-8 konfiguriert ist, arbeiten – eine APIs in UTF-8. Dieses Modell hat den Vorteil für die Unterstützung von vorhandenen Codes Dank – ein APIs ohne codeänderungen.

## <a name="set-a-process-code-page-to-utf-8"></a>Legen Sie eine Prozess-Codepage in UTF-8

Windows-Version 1903 (Mai 2019-Update), können die ActiveCodePage-Eigenschaft in die appxmanifest-Datei für App-Pakete oder das Fusion-Manifest für entpackt-apps Sie erzwingen, dass einen Prozess, die UTF-8 als die Prozess-Codepage verwendet.

Sie können diese Eigenschaft deklarieren und Ziel/ausführen, auf früheren Windows-builds, aber Sie müssen Erkennung von Legacycode Seiten und die Konvertierung wie gewohnt verarbeiten. Mit einer Mindestversion der Zielplattform-Version von Windows-Version 1903 sein werden die Codepage für den Prozess immer UTF-8, damit die Erkennung von Legacycode Seiten und die Konvertierung können vermieden werden.

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

Wie Windows systemintern in UTF-16 (`WCHAR`), müssen Sie möglicherweise für die Zusammenarbeit mit Windows APIs UTF-8-Daten in UTF-16 (oder umgekehrt) zu konvertieren.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) und [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) können zwischen UTF-8 und UTF-16 konvertieren (`WCHAR`) (und anderen Codepages). Dies ist besonders nützlich, wenn eine ältere Win32-API nur verstehen möglicherweise `WCHAR`. Diese Funktionen können Sie zum Konvertieren von UTF-8-Eingabe für `WCHAR` zum Übergeben an eine -W-API- und konvertieren dann Ergebnisse zurück, falls erforderlich.
Bei Verwendung dieser Funktionen mit `CodePage` festgelegt `CP_UTF8`, verwenden `dwFlags` entweder `0` oder `MB_ERR_INVALID_CHARS`, andernfalls ein `ERROR_INVALID_FLAGS` auftritt.

Hinweis: `CP_ACP` entspricht `CP_UTF8` nur dann, wenn auf Windows-Version 1903 (Mai 2019-Update) oder höher und die ActiveCodePage-Eigenschaft, die oben beschriebenen in UTF-8 festgelegt ist. Andernfalls wird es die Codepage des älteren Systems berücksichtigt. Es wird empfohlen, `CP_UTF8` explizit.

## <a name="related-topics"></a>Verwandte Themen

- [Codepages](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Codepage-IDs](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
