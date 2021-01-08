---
description: Erfahren Sie mehr über Project Reunion, welche Vorteile es für Entwickler bietet, was jetzt für Entwickler bereitsteht und wie Sie Feedback geben können.
title: Projektzusammenführung
ms.topic: article
ms.date: 01/07/2021
keywords: Windows Win32, Desktopentwicklung, Project Reunion
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: de1115281859ba322a03e3bab7a8d0e4ef71440d
ms.sourcegitcommit: 044c75ea0c6fb3463a0150acdae1ff867dc05f29
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 01/07/2021
ms.locfileid: "97972124"
---
# <a name="build-windows-apps-with-project-reunion-prerelease"></a>Erstellen von Windows-Apps mit Project Reunion (Vorabversion)

Project Reunion 0.1 Prerelease ist eine Vorschau des neuen Satzes von Entwicklerkomponenten und -tools, die die nächste Weiterentwicklung der Windows-App-Entwicklungsplattform darstellen. Project Reunion bietet einen einheitlichen Satz von APIs und Tools, die von jeder App auf einer breiten Palette von Zielversionen des Windows 10-Betriebssystems auf konsistente Weise verwendet werden können. Project Reunion ersetzt nicht die bestehenden Windows-App-Plattformen und -Frameworks wie UWP und natives Win32 sowie .NET, sondern ergänzt diese bestehenden Plattformen mit einem gemeinsamen Satz von APIs und Tools, auf die sich Entwickler plattformübergreifend verlassen können.

> [!NOTE]
> Project Reunion 0.1 Prerelease ist eine frühe Vorschau für Entwickler. Wir empfehlen Ihnen, diese Version in Ihrer Entwicklungsumgebung zu testen. Berücksichtigen Sie jedoch, dass sich Project Reunion bis zur endgültigen Veröffentlichung in vielerlei Hinsicht verändern wird. Project Reunion 0.1 Prerelease wird für Apps, die in Produktionsumgebungen verwendet werden, nicht unterstützt. **Project Reunion** ist ein Codename, der in einem zukünftigen Release geändert werden kann.

## <a name="goals-of-project-reunion"></a>Ziele von Project Reunion

Project Reunion stellt eine breite Palette von Windows-APIs mit Implementierungen bereit, die vom Betriebssystem entkoppelt sind und Entwicklern über NuGet-Pakete zur Verfügung gestellt werden. Project Reunion soll das Windows SDK nicht ersetzen. Das Windows SDK wird weiterhin unverändert funktionieren, und es gibt viele Kernkomponenten von Windows, die mit APIs, die über Betriebssystem- und Windows SDK-Versionen bereitgestellt werden, weiterentwickelt werden. Entwicklern wird empfohlen, Project Reunion in Ihrem eigenen Tempo zu übernehmen.

Project Reunion wurde zur Unterstützung der folgenden Ziele entwickelt.

#### <a name="unified-api-surface-across-different-types-of-windows-apps"></a>Unified API-Oberfläche für verschiedene Typen von Windows-Apps

Entwickler, die Windows-Apps erstellen möchten, müssen zwischen mehreren App-Plattformen und -Frameworks wählen. Jede Plattform stallt zwar viele Features und APIs bereit, die von mit anderen Plattformen erstellten Apps verwendet werden können, einige Features und APIs können jedoch nur von bestimmten Plattformen verwendet werden. Mit Project Reunion wird der Zugriff auf Windows-APIs für alle Windows 10-Apps vereinheitlicht. Unabhängig davon, für welches App-Modell Sie sich entscheiden, haben Sie Zugriff auf den gleichen Satz von Windows-APIs, die in Project Reunion verfügbar sind.

Wir planen im Laufe der Zeit weitere Investitionen in Project Reunion, um die Unterschiede zwischen den verschiedenen App-Modellen zu verringern. Project Reunion wird sowohl WinRT-APIs als auch native C-APIs enthalten.

#### <a name="consistent-support-across-windows-10-versions"></a>Konsistente Unterstützung über Windows 10-Versionen hinweg

Da sich die Windows-APIs mit neuen Betriebssystemversionen weiterentwickeln, müssen Entwickler Techniken wie [versionsadaptiven Code](/windows/uwp/debug-test-perf/version-adaptive-code) verwenden, um alle Versionsunterschiede zu berücksichtigen und so die Zielgruppe ihrer Anwendung zu erreichen. Dadurch kann der Code und die Entwicklungsumgebung komplexer werden. Project Reunion wird es uns ermöglichen, den Zugriff auf Project Reunion-APIs über verschiedene Betriebssystemversionen und alle Windows 10-Geräte hinweg zu vereinheitlichen, sodass wir Updates angepasst für mehr Entwickler und nicht nur für die neueste Version von Windows zur Verfügung stellen können. Der aktuelle Plan sieht vor, dass Project Reunion Windows 10, Version 1809, und alle späteren Versionen von Windows 10 unterstützt.

#### <a name="faster-release-cadence"></a>Schnellerer Versionsrhythmus

Neue Windows-APIs und -Funktionen sind in der Regel an Betriebssystemreleases gebunden, die ein- oder zweimal im Jahr erscheinen. Mit Project Reunion können wir häufiger und flexibler neue APIs und Features in Produktionsqualität veröffentlichen.

## <a name="get-started"></a>Erste Schritte

Project Reunion 0.1 Prerelease enthält neue APIs für die folgenden Featurebereiche.

| Funktion | Beschreibung |
|---------|-------------|
| [MRT Core (modernes Ressourcenverwaltungssystem)](mrtcore/mrtcore-overview.md) | MRT Core ist eine optimierte Version des modernen [Windows-Ressourcenverwaltungssystems](/windows/uwp/app-resources/resource-management-system), das als Teil von Project Reunion verteilt wird. |
| [DWriteCore](dwritecore.md) | DWriteCore ist eine Form von DirectWrite, die auf Windows-Versionen bis hinunter zu Windows 8 läuft und Ihnen die Möglichkeit eröffnet, es plattformübergreifend zu nutzen. |

Weitere Informationen zu den zukünftigen Plänen der Integration weiterer Komponenten in Project Reunion finden Sie [hier](https://github.com/microsoft/ProjectReunion/blob/master/docs/README.md).

> [!NOTE]
> Bestimmte andere vorhandene Komponenten, einschließlich der Windows-UI-Bibliothek (WinUI), MSIX und WebView2, sind bereits vom Betriebssystem entkoppelt und folgen den Project Reunion-Richtlinien (beispielsweise werden sie unter Windows 10, Version 1809, und späteren Betriebssystemversionen unterstützt). Diese anderen Komponenten sind jedoch zurzeit nicht Teil des Project Reunion-NuGet-Pakets.  

### <a name="set-up-your-development-environment"></a>Einrichten der Entwicklungsumgebung

Wenn Sie Project Reunion 0.1 Prerelease ausprobieren möchten, müssen Sie mit einem der mitgelieferten C++-Beispiele beginnen. Diese Beispiele sind für die Verwendung des Project Reunion-NuGet-Pakets vorkonfiguriert. Diese Vorschau unterstützt nicht die Installation des Project Reunion-NuGet-Pakets in Ihren eigenen Projekten. Gehen Sie wie folgt vor, um Ihre Entwicklungsumgebung für die Verwendung eines der Beispiele einzurichten.

1. Stelle sicher, dass auf deinem Entwicklungscomputer Windows 10 (Version 1809, Build 17763) oder eine höhere Betriebssystemversion installiert ist.

2. Installieren Sie [Visual Studio 2019, Version 16.9, Vorschau 2 (oder höher)](https://visualstudio.microsoft.com/vs/preview/). Stellen Sie sicher, dass die folgenden Elemente im Visual Studio-Installer ausgewählt sind:
    - Stellen Sie sicher, dass auf der Registerkarte **Workloads** die folgenden Workloads ausgewählt sind.
        - **.NET-Desktopentwicklung**
        - **Desktopentwicklung mit C++**
        - **Entwicklung für die universelle Windows-Plattform** (stellen Sie außerdem sicher, dass die optionale Komponente **UWP-Tools (Universelle Windows-Plattform) für C++ (v142)** für diesen Workload auf der Registerkarte **Installationsdetails** ausgewählt ist)
    - Stellen Sie sicher, dass auf der Registerkarte **Einzelne Komponenten** im Abschnitt **SDKs, Bibliotheken und Frameworks** die Option **Windows 10 SDK (10.0.19041.0)** ausgewählt ist.

3. Installieren Sie die neueste Version der [C++/WinRT Visual Studio-Erweiterung (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) aus dem Visual Studio Marketplace.

4. Stellen Sie sicher, dass auf Ihrem System eine NuGet-Paketquelle für **nuget.org** aktiviert ist. Weitere Informationen finden Sie unter [Allgemeine NuGet-Konfigurationen](/nuget/consume-packages/configuring-nuget-behavior).

5. Laden Sie das [WinUI 3 Vorschau 3-VSIX-Paket](https://aka.ms/winui3/preview3-download) herunter und installieren Sie es. Dieser Schritt ist nur für die Beispiele „Hallo Welt“ und „MRT Core“ erforderlich, die bereits für die Verwendung von WinUI 3 konfiguriert sind. Anweisungen zum Hinzufügen des VSIX-Pakets zu Visual Studio finden Sie unter [Suchen nach und Verwenden von Visual Studio-Erweiterungen](/visualstudio/ide/finding-and-using-visual-studio-extensions#install-without-using-the-manage-extensions-dialog-box).

6. Klonen und untersuchen Sie die folgenden Beispiele:
    - [Beispiel für den DWriteCore-Katalog](https://github.com/microsoft/Project-Reunion-Samples/tree/main/DWriteCore/DWriteCoreGallery): Diese Beispielanwendung veranschaulicht die [DWriteCore](dwritecore.md)-API.
    - [Beispiel für MRT Core](https://github.com/microsoft/Project-Reunion-Samples/tree/main/MrtCore): Diese Beispielanwendung veranschaulicht die [MRT Core](mrtcore/mrtcore-overview.md)-API.
    - [Hallo Welt-Beispiel](https://github.com/microsoft/Project-Reunion-Samples/tree/main/HelloWorld/reunioncppdesktopsampleapp): Dieses Beispiel veranschaulicht eine grundlegende Integration in das Project Reunion-NuGet-Paket.

## <a name="known-issues"></a>Bekannte Probleme

Project Reunion 0.1 Prerelease weist die folgenden Einschränkungen auf:

 - Dieses Release wird nur für mit MSIX gepackte C++/Win32-Apps unterstützt.
 - Dieses Release unterstützt C# nicht.
 - Dieses Release unterstützt .NET 5 nicht.

## <a name="developer-roadmap"></a>Roadmap für Entwickler

Die neuesten Project Reunion-Pläne finden Sie auf unserer [GitHub-Seite](https://github.com/microsoft/ProjectReunion).

## <a name="give-feedback-and-contribute"></a>Feedback und Mitwirkung

Wir entwickeln Project Reunion als Open-Source-Projekt. Auf unserer [Github-Seite](https://github.com/microsoft/ProjectReunion) finden Sie viele weitere Informationen darüber, wie wir Project Reunion in die Tat umsetzen wollen, und wir möchten Sie einladen, Teil des Entwicklungsprozesses zu werden. Schauen Sie sich unseren [Leitfaden für Mitwirkende](https://github.com/microsoft/ProjectReunion/blob/master/docs/contributor-guide.md) an, um Fragen zu stellen, Diskussionen zu starten oder Featurevorschläge zu machen. Wir möchten sicherstellen, dass Project Reunion Entwicklern wie Ihnen den größtmöglichen Nutzen bringt.
