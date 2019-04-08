---
title: Erweitern der App mit Diensten, Erweiterungen und Paketen
description: Beschreibt, wie eine Hintergrundtask zu erstellen, die ausgeführt wird, wenn es sich bei Ihrer Store-app (Universelle Windows Plattform) aktualisiert wird.
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, erweitern, aufschlüsseln, App-Dienst, Paket, Erweiterung
ms.localizationpriority: medium
ms.openlocfilehash: 47ab6491d09775bf86f0f484fc96d85bd07f53a4
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590675"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Erweitern der App mit Diensten, Erweiterungen und Paketen

Es gibt viele Technologien in Windows 10 zum Erweitern und componentizing Ihrer app ein. Diese Tabelle soll Ihnen ein welche Technologie zu ermitteln, sollten Sie je nach Anforderungen verwenden. Anschließend finden Sie eine kurze Beschreibung der jeweiligen Szenarien und Technologien.

| Szenario                           | Ressourcenpaket   | Bestandspaket      | Optionales Paket   | Flat-Bundle        | App-Erweiterung      | App-Dienst        | Streaming-Installation  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Drittanbieter-Code-Plug-ins            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| In-Process Code-Plugins              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX-Ressourcen (Zeichenfolgen/Images)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| On-Demand Inhalte <br/> (z. B. zusätzliche Spielstufen) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Separate Lizenzierung und Erwerb |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| In-App-Erwerb                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimierung der Installationszeit              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Verringern des Speicherbedarfs auf Datenträgern              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimieren der Verpackung                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reduzierung der Veröffentlichungszeit             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Beschreibungen der Szenarien (die Zeilen der obigen Tabelle)

**Drittanbieter-Plug-ins**  

Code, der im Store zum Herunterladen bereitsteht und den Sie über Ihre App ausführen können. Beispielsweise Erweiterungen für den Microsoft Edge-Browser.

**In-Proc-Code-Plug-ins**  

Code, der In-Process von Ihrer App ausgeführt wird. Nur C++ wird unterstützt. Kann auch Inhalte enthalten. Da der Code In-Process ausgeführt wird, wird von einem höheren Maß an Vertrauen ausgegangen. Sie können auswählen, nicht auf diese Art von Erweiterbarkeit für einen Drittanbieter verfügbar zu machen.

**UX-Objekte (Zeichenfolge/Images)**  

Ressourcen der Benutzeroberfläche wie lokalisierte Zeichenfolgen, Bilder sowie jegliche andere UI-Inhalte, die Sie aufgrund der Sprachumgebung oder anderen Gründen einbeziehen möchten.

**Für Demand-Inhalten**  

Inhalte, die Sie zu einem späteren Zeitpunkt herunterladen möchten. Beispielsweise In-App-Einkäufe, die Ihnen ermöglichen, neue Spielstufen, Designs oder Funktionen herunterzuladen.

**Separate Lizenzierung und Erwerb**  

Die Fähigkeit den Inhalt unabhängig von der App zu lizenzieren und zu erwerben.

**In-app-Erwerb**  

Gibt an, ob eine Unterstützung seitens des Programms vorhanden ist, die Inhalte App-intern zu erwerben.

**Optimieren der Installationszeit**

Stellt Funktionen zur Verkürzung der Zeit bereit, die benötigt wird, um die App aus dem Store herunterzuladen und zu starten.

**Reduzieren des Speicherbedarfs auf dem Datenträger** Reduziert die Größe einer App, indem nur notwendige Apps oder Ressourcen einbezogen werden.

**Optimieren der paketerstellung** optimiert die Verarbeitung des app-paketerstellung für umfangreiche oder komplexe apps.

**Reduzierung der Veröffentlichungszeit** Minimiert den Zeitaufwand für die Veröffentlichung Ihrer App im Store, auf einer lokalen Freigabe oder auf dem Webserver.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Beschreibung der Technologien (die Spalten der obigen Tabelle)

**Das ressourcenpaket**

Bei Ressourcenpaketen handelt es sich um an die Ressource gebundene Pakete, die Ihrer App eine Anpassung an zahlreiche Bildschirmgrößen und Systemsprachen ermöglichen. Das Ressourcenpaket zielt auf die Benutzersprache, die Systemskalierung sowie die DirectX-Funktionen ab und erlaubt der App dadurch eine Anpassung an zahlreiche Nutzerszenarien. Obwohl ein App-Paket mehrere Ressourcen enthalten kann, wird das Betriebssystem nur die für das Gerät des Benutzers notwendigen Ressourcen herunterladen. Dies spart Bandbreite und Festplattenspeicher.

**Asset-Paket** ressourcenpakete sind eine allgemeine, zentrale ausführbare Datei oder nicht ausführbare Dateien für die Verwendung von Ihrer app. In der Regel sind dies nicht-Prozessor oder sprachspezifische Dateien. Es kann sich beispielsweise um eine Sammlung von Bildern in einem Bestandspaket und von Videos in einem anderen Bestandspaket handeln, die beide von der App verwendet werden. Wenn Ihre app mehrere Architekturen und mehrere Sprachen unterstützt, diese Objekte können in der Architektur oder das ressourcenpaket enthalten sein, aber das bedeutet auch, dass die Ressourcen mehrere Male über die verschiedenen Architektur-Pakete, dupliziert würden dauert Sie Speicherplatz frei. Wenn Bestandspakete verwendet werden, müssen sie nur einmal im gesamten App-Paket enthalten sein. Weitere Informationen finden Sie unter [Einführung in Bestandspakete](../packaging/asset-packages.md).

**Optionales Paket**

Optionale Pakete dienen entweder der Ergänzung oder Erweiterung der Originalfunktionsweise eines App-Pakets. Es ist möglich, eine App zu veröffentlichen und die optionalen Pakete zu einem späteren Zeitpunkt zu veröffentlichen oder sowohl die App als auch die optionalen Pakete gleichzeitig zu veröffentlichen. Die Erweiterung Ihrer App um optionale Pakete hat zum Vorteil, dass Sie Inhalte als separates App-Paket verbreiten und vermarkten können. Optionale Pakete werden in der Regel vom ursprünglichen App-Entwickler entwickelt, da diese über die Identität der Haupt-App (im Gegensatz zu App-Erweiterungen) ausgeführt werden. Je nach Definition Ihres optionalen Pakets können Sie Code, Ressourcen oder Code und Ressourcen Ihres optionalen Pakets in Ihre Haupt-App laden. Wenn müssen Sie Ihre app mit Inhalt zu erweitern, die monetarisierung lizenziert, und verteilte separat, klicken Sie dann optional Pakete möglicherweise die richtige Wahl für Sie. Einzelheiten zur Implementierung, finden Sie unter [Optionale Pakete und die Erstellung zugehöriger Sets](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Flat-Bundle**
[Flat-Bundle-App-Pakete](../packaging/flat-bundles.md) ähneln regulären App Bundles, jedoch enthält ein Flat-Bundle nicht die App-Pakete aus dem Ordner, sondern nur *Referenzen* auf diese App-Pakete. Da nur Verweise auf App-Pakete anstelle der Dateien selbst enthalten sind, wird durch ein Flat-Bundle die Zeit verkürzt, die zum Packen und Herunterladen einer App nötig ist.

**App-Erweiterung**

Durch [App-Erweiterungen](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) kann Ihre UWP-App Inhalte hosten, die von anderen UWP-Apps bereitgestellt werden. Sie können schreibgeschützte Inhalte dieser Apps ermitteln und enumerieren und darauf zugreifen.

Wenn eine App Erweiterungen unterstützt, kann jeder beliebige Entwickler Erweiterungen für diese App übermitteln. Daher bedarf es einer soliden Host-App, um Erweiterungen zu laden, die zuvor nicht mit dieser getestet wurde. Erweiterungen sollten als nicht vertrauenswürdig eingestuft werden.

Anwendungen können Code aus Erweiterungen nicht laden. Wenn Sie die Ausführung von Code benötigen, sollten Sie App-Dienste in Erwägung ziehen.

**App Service**

Windows-app-Dienste ermöglichen die Kommunikation der app zur app-Konfigurationsrichtlinien Ihrer UWP-app, um eine andere universelle Windows-app Dienste bereitzustellen. App-Dienste ermöglichen Ihnen, Dienste ohne UI zu erstellen, die von Apps auf demselben Gerät aufgerufen werden können. Ab Windows 10, Version 1607 ist dies auch für Remote-Geräte möglich. Weitere Einzelheiten finden Sie unter [App-Service erstellen und verwenden](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

App-Dienste sind UWP-Apps, die Dienste für andere UWP-Apps bereitstellen. Sie sind analog zu Webdiensten auf einem Gerät. Ein App-Dienst wird als Hintergrundaufgabe in der Host-App ausgeführt und kann seine Dienste auch anderen Apps bereitstellen. Beispielsweise kann der Barcode-Scanner eines App-Dienstes auch anderen Apps nützlich sein. Oder möglicherweise verfügt eine Enterprise-Suite von Apps über einen gemeinschaftlichen App-Dienst für die Rechtschreibprüfung, der auch den anderen Apps in der Suite zur Verfügung steht.

**Streaming von UWP-App installieren**

Die Installierung von Streaming ermöglicht Ihnen die Optimierung der Art und Weise, wie Ihre App Benutzern bereitgestellt wird. Anstatt darauf warten zu müssen, dass die gesamte App heruntergeladen ist, bevor sie verwenden werden kann, können Benutzer bereits mit der App interagieren, sobald ein notwendiger Teil heruntergeladen wurde. Es obliegt Ihnen, als Entwickler, Ihre App entsprechend in einen für die primäre Aktivierung und Initialisierung erforderlichen Bereich sowie einen Bereich für die restlichen Inhalte der App zu gliedern. Weitere Informationen sowie Einzelheiten zur Implementierung finden Sie unter [UWP-App-Streaming installieren](https://docs.microsoft.com/windows/uwp/packaging/streaming-install).

## <a name="see-also"></a>Weitere Informationen

[Erstellen und Verwenden eines App-Diensts](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Einführung in Asset-Pakete](../packaging/asset-packages.md)  
[Erstellen von Paketen mit dem Layout für die paketerstellung](../packaging/packaging-layout.md)  
[Optionale Pakete und die Erstellung zugehöriger Sets](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Entwickeln mit ressourcenpakete und das Paket zu falten](../packaging/package-folding.md)  
[Installieren von UWP-App-Streaming](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Flatfile-Paket-app-Pakete](../packaging/flat-bundles.md)  
[Windows.ApplicationModel.AppService-namespace](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows.ApplicationModel.Extensions-namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
