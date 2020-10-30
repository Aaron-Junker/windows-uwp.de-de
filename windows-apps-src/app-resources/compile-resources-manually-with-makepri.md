---
description: „MakePri.exe“ ist ein Befehlszeilentool, mit dem Sie PRI-Dateien erstellen und sichern können. Es ist über MSBuild in Microsoft Visual Studio integriert, kann aber auch für Entwickler von Nutzen sein, die Pakete manuell oder mithilfe benutzerdefinierter Buildsysteme erstellen.
title: Manuelles Kompilieren von Ressourcen mit „MakePri.exe“
template: detail.hbs
ms.date: 10/23/2017
ms.topic: article
keywords: Windows 10, UWP, Ressourcen, Bild, Element, MRT, Qualifizierer
ms.localizationpriority: medium
ms.openlocfilehash: 579aaf5f833f2d3ddb8e4d8c080a6f94ac21f40f
ms.sourcegitcommit: a3bbd3dd13be5d2f8a2793717adf4276840ee17d
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/30/2020
ms.locfileid: "93031863"
---
# <a name="compile-resources-manually-with-makepriexe"></a>Manuelles Kompilieren von Ressourcen mit „MakePri.exe“

„MakePri.exe“ ist ein Befehlszeilentool, mit dem Sie PRI-Dateien erstellen und sichern können. Es ist über MSBuild in Microsoft Visual Studio integriert, kann aber auch für Entwickler von Nutzen sein, die Pakete manuell oder mithilfe benutzerdefinierter Buildsysteme erstellen.

> [!NOTE]
> MakePri.exe wird installiert, wenn Sie bei der Installation des Windows Software Development Kit die Option **Windows SDK für verwaltete UWP-apps** aktivieren. Es wird im Pfad installiert `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (sowie in Ordnern, die für die anderen Architekturen benannt sind). Beispiel: `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="in-this-section"></a>In diesem Abschnitt
|Thema|BESCHREIBUNG|
|-|-|
| [Befehlszeilenoptionen für „MakePri.exe“](makepri-exe-command-options.md) | MakePri.exe verfügt über die Befehle,,, `createconfig` `dump` `new` `resourcepack` und `versioned` . In diesem Thema werden die Befehlszeilenoptionen für ihre Verwendung ausführlich erläutert. |
| [Konfigurationsdatei für „MakePri.exe“](makepri-exe-configuration.md) | In diesem Thema wird das Schema der MakePri.exe XML-Konfigurationsdatei beschrieben. |
| [Formatspezifische Indexer für „MakePri.exe“](makepri-exe-format-specific-indexers.md) | In diesem Thema werden die Format spezifischen Indexer beschrieben, die vom MakePri.exe Tool verwendet werden, um den Index von Ressourcen zu generieren. |

## <a name="makepriexe-command-line-options"></a>Befehlszeilenoptionen für „MakePri.exe“

MakePri.exe verfügt über die Befehle,,, `createconfig` `dump` `new` `resourcepack` und `versioned` . Ausführliche Informationen zu deren Verwendung finden Sie unter [MakePri.exe Befehlszeilenoptionen](makepri-exe-command-options.md).

## <a name="makepriexe-configuration"></a>MakePri.exe Konfiguration

Die PRI-XML-Konfigurationsdatei bestimmt, wie und welche Ressourcen indiziert werden. Das Schema der XML-Konfigurationsdatei wird in [MakePri.exe Konfiguration](makepri-exe-configuration.md)beschrieben.

## <a name="format-specific-indexers"></a>Format spezifische Indexer

MakePri.exe wird in der Regel mit `new` den `versioned` Optionen, und verwendet `resourcepack` . In diesen Fällen werden Quelldateien indiziert, um einen Index von Ressourcen zu generieren. MakePri.exe verwendet verschiedene einzelne Indexer, um unterschiedliche Quell Ressourcen Dateien oder Container für Ressourcen zu lesen. Der einfachste Indexer ist der ordnerindexer, der den Inhalt eines Ordners für Ressourcen, z `.jpg` . b. oder Bilder, indiziert `.png` . Weitere Informationen finden Sie unter [MakePri.exe formatspezifischen Indexer](makepri-exe-format-specific-indexers.md).

## <a name="makepriexe-warnings-and-error-messages"></a>MakePri.exe von Warnungen und Fehlermeldungen

### <a name="resources-found-for-languages-languages-but-no-resources-found-for-default-languages-languages-change-the-default-language-or-qualify-resources-with-the-default-language"></a>Ressourcen für die Sprachen "<Sprache (en) >" gefunden, aber es wurden keine Ressourcen für die Standardsprache (n) gefunden: "<Sprache (n) >". Ändern Sie die Standardsprache, oder qualifizieren Sie Ressourcen mit der Standardsprache.

Diese Warnung wird angezeigt, wenn MakePri.exe oder MSBuild Dateien oder Zeichen folgen Ressourcen für eine bestimmte benannte Ressource ermittelt, die anscheinend mit sprach Qualifizierern gekennzeichnet sind, aber für eine Standardsprache keinen Kandidaten gefunden haben. Der Prozess für die Verwendung von Qualifizierern in Datei-und Ordnernamen wird unter [Anpassen Ihrer Ressourcen für Sprache, Skalierung und andere Qualifizierer](tailor-resources-lang-scale-contrast.md)beschrieben. In einer Datei oder einem Ordner ist möglicherweise ein sprach Name enthalten, es werden jedoch keine Ressourcen erkannt, die für die exakte Standardsprache qualifiziert sind. Wenn ein Projekt z. b. "en-US" als Standardsprache verwendet und eine Datei mit dem Namen "de/logo.png" enthält, aber keine Dateien enthält, die mit der Standardsprache "en-US" gekennzeichnet sind, wird diese Warnung angezeigt. Um diese Warnung zu entfernen, müssen entweder Dateien oder Zeichen folgen Ressourcen mit der Standardsprache qualifiziert werden, oder die Standardsprache muss geändert werden. Wenn Sie die Standardsprache ändern möchten, öffnen Sie die Projekt Mappe in Visual Studio, und öffnen Sie `Package.appxmanifest` . Vergewissern Sie sich auf der Registerkarte Anwendung, dass die Standardsprache entsprechend festgelegt ist (z. b. "en" oder "en-US").

### <a name="no-default-or-neutral-resource-given-for-resource-identifier-the-application-may-throw-an-exception-for-certain-user-configurations-when-retrieving-the-resources"></a>Für "" wurde keine standardmäßige oder neutrale Ressource angegeben <resource identifier> . Die Anwendung löst möglicherweise eine Ausnahme für bestimmte Benutzerkonfigurationen aus, wenn die Ressourcen abgerufen werden.

Diese Warnung wird angezeigt, wenn MakePri.exe oder MSBuild Dateien oder Ressourcen ermittelt, die mit sprach Qualifizierern gekennzeichnet sind, für die die Ressourcen unklar sind. Es gibt Qualifizierer, aber es gibt keine Garantie dafür, dass ein bestimmter Ressourcen Kandidat zur Laufzeit für diesen Ressourcen Bezeichner zurückgegeben werden kann. Wenn kein Ressourcen Kandidat für eine bestimmte Sprache, homeregion oder ein anderer Qualifizierer gefunden werden kann, der ein Standardwert ist oder immer dem Kontext eines Benutzers entspricht, wird diese Warnung angezeigt. Zur Laufzeit können für bestimmte Benutzerkonfigurationen, wie z. b. die Spracheinstellungen eines Benutzers oder den Speicherort ( **Einstellungen**  >  **Zeit & sprach**  >  **Region & Sprache** ), die APIs, die zum Abrufen der Ressource verwendet werden, eine unerwartete Ausnahme auslösen. Um diese Warnung zu entfernen, sollten Standard Ressourcen bereitgestellt werden, z. b. eine Ressource in der Standardsprache des Projekts oder in der Region "Global Home" (homeregion-001).

## <a name="using-makepriexe-in-a-build-system"></a>Verwenden von MakePri.exe in einem Buildsystem

Buildsysteme sollten den MakePri.exe- `new` ,-oder-Befehl verwenden `versioned` `resourcepack` , abhängig vom Projekttyp, der erstellt wird. Buildsysteme, die eine neue PRI-Datei erstellen, sollten den `new` Befehl verwenden. Buildsysteme, die die Kompatibilität interner Offsets durch Iterationen sicherstellen müssen, können den- `versioned` Befehl verwenden. Buildsysteme, die eine PRI-Datei erstellen müssen, die zusätzliche Varianten von Ressourcen enthält. Überprüfung, um sicherzustellen, dass für diese Variante keine neuen Ressourcen hinzugefügt werden, sollte der Befehl verwendet werden `resourcepack` .

Buildsysteme, die eine explizite Kontrolle über Quelldateien erfordern, die indiziert werden, können den resfiles-Indexer verwenden, anstatt einen Ordner zu indizieren. Buildsysteme können auch mehrere Index Übergänge mit unterschiedlichen [Format spezifischen indexatoren](makepri-exe-format-specific-indexers.md) verwenden, um eine einzelne PRI-Datei zu generieren.

Buildsysteme können auch den Format spezifischen PRI-Indexer zum Hinzufügen von vorgefertigten PRI-Dateien zum PRI für das Paket aus anderen Komponenten, z. b. Klassenbibliotheken, Assemblys, sdassemblys und DLLs, verwenden.

Wenn PRI-Dateien für andere Komponenten, Klassenbibliotheken, Assemblys, DLLs und sdken erstellt werden, sollte die **initialpath** -Konfiguration verwendet werden, um sicherzustellen, dass die Komponenten Ressourcen über eigene unter Ressourcen Zuordnungen verfügen, die nicht mit der app in Konflikt stehen, in der Sie enthalten sind.

## <a name="related-topics"></a>Verwandte Themen
* [Befehlszeilenoptionen für „MakePri.exe“](makepri-exe-command-options.md)
* [MakePri.exe Konfiguration](makepri-exe-configuration.md)
* [Formatspezifische Indexer für „MakePri.exe“](makepri-exe-format-specific-indexers.md)
* [Passen Sie Ihre Ressourcen für Sprache, Skalierung und andere Qualifizierer an.](tailor-resources-lang-scale-contrast.md)
