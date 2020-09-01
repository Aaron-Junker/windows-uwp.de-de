---
Description: Verwenden Sie die UTF-8-Zeichencodierung für eine optimale Kompatibilität zwischen Web-Apps und anderen \* nix-basierten Plattformen (UNIX, Linux und Varianten), minimieren Sie Lokalisierungsfehler, und reduzieren Sie den Testaufwand.
title: Verwenden der Windows UTF-8-Codepage
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: Windows 10, UWP, Globalisierung, Lokalisier barkeit, Lokalisierung
ms.localizationpriority: medium
ms.openlocfilehash: 72e422ee3e1a911658b2fe4957967aeba116c353
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89173464"
---
# <a name="use-the-utf-8-code-page"></a>UTF-8-Codepage verwenden

Verwenden Sie die [UTF-8](http://www.utf-8.com/) -Zeichencodierung für eine optimale Kompatibilität zwischen Web-Apps und anderen \* nix-basierten Plattformen (UNIX, Linux und Varianten), minimieren Sie Lokalisierungsfehler, und reduzieren Sie den Testaufwand.

UTF-8 ist die universelle Codepage für die Internationalisierung und kann den gesamten Unicode-Zeichensatz codieren. Sie wird im Web als Standard verwendet und ist die Standardeinstellung für * nix-basierte Plattformen.

> [!NOTE]
> Ein codiertes Zeichen dauert zwischen 1 und 4 Bytes. UTF-8-Codierung unterstützt längere Byte Sequenzen (bis zu 6 Bytes), aber der größte Codepunkt von Unicode 6,0 (U + 10FFFF) benötigt nur 4 Bytes.

## <a name="-a-vs--w-apis"></a>-A-und-W-APIs
  
Win32-APIs unterstützen häufig sowohl-A-als auch-W-Varianten.

-Eine Variante erkennt die auf dem System konfigurierte ANSI-Codepage und unterstützt `char*` , während-W-Varianten in UTF-16 und unterstützt werden `WCHAR` .

Bis vor kurzem hat Windows "Unicode"-W-Varianten über-A-APIs hervorgehoben. In neueren Releases wurden jedoch die ANSI-Codepage und die-A-APIs verwendet, um die UTF-8-Unterstützung für apps einzuführen. Wenn die ANSI-Codepage für UTF-8 konfiguriert ist, funktionieren die APIs in UTF-8. Dieses Modell hat den Vorteil, dass vorhandener Code, der mit-A-APIs erstellt wurde, ohne Codeänderungen unterstützt wird.

## <a name="set-a-process-code-page-to-utf-8"></a>Festlegen einer Prozess Codepage auf UTF-8

Ab Windows, Version 1903 (Mai 2019 Update), können Sie die activecodepage-Eigenschaft im appxmanifest für App-Pakete oder das Fusion-Manifest für nicht gepackte Apps verwenden, um die Verwendung von UTF-8 als Prozess Codepage durch einen Prozess zu erzwingen.

Sie können diese Eigenschaft deklarieren und für frühere Windows-Builds ausführen/ausführen, aber Sie müssen die Legacy-Code Page Erkennung und-Konvertierung wie gewohnt behandeln. Bei einer Mindestversion von Windows Version 1903 ist die Prozess Codepage immer UTF-8, sodass die Erkennung und Konvertierung von Legacy-Codeseiten vermieden werden kann.

## <a name="examples"></a>Beispiele

**AppX-Manifest für eine APP mit App-Paket:**

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

**Fusion-Manifest für eine nicht gepackte Win32-App:**

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
> Hinzufügen eines Manifests zu einer vorhandenen ausführbaren Datei in der Befehlszeile mit `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Code Page Konvertierung

Da Windows System eigen in UTF-16 ( `WCHAR` ) betrieben wird, müssen Sie möglicherweise UTF-8-Daten in UTF-16 (oder umgekehrt) konvertieren, um mit Windows-APIs zusammenzuarbeiten.

Mit " [MultiByteToWideChar](/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) " und " [WideCharToMultiByte](/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) " können Sie zwischen UTF-8 und UTF-16 ( `WCHAR` ) (und anderen Codepages) konvertieren. Dies ist besonders nützlich, wenn eine Legacy-Win32-API nur verstanden werden kann `WCHAR` . Diese Funktionen ermöglichen es Ihnen, UTF-8-Eingaben in zu konvertieren `WCHAR` , um Sie an eine-W-API zu übergeben, und dann alle Ergebnisse bei Bedarf zurück zu konvertieren.
Wenn Sie diese Funktionen verwenden `CodePage` , wobei auf festgelegt `CP_UTF8` ist, verwenden Sie `dwFlags` entweder `0` oder `MB_ERR_INVALID_CHARS` , andernfalls tritt ein auf `ERROR_INVALID_FLAGS` .

> [!NOTE]
> `CP_ACP` entspricht `CP_UTF8` nur, wenn unter Windows, Version 1903 (Mai 2019 Update) oder höher ausgeführt wird und die oben beschriebene activecodepage-Eigenschaft auf UTF-8 festgelegt ist. Andernfalls wird die ältere System Codepage berücksichtigt. Es wird empfohlen, explizit zu verwenden `CP_UTF8` .

## <a name="related-topics"></a>Zugehörige Themen

- [Codepages](/windows/desktop/Intl/code-pages)
- [Code Page Bezeichner](/windows/desktop/Intl/code-page-identifiers)