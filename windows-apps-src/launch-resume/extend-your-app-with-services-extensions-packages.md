---
title: Erweitern der App mit Diensten, Erweiterungen und Paketen
description: Beschreibt, wie eine Hintergrundaufgabe erstellt wird, die ausgeführt wird, wenn die universelle Windows-Plattform (UWP) Store-APP aktualisiert wird.
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, erweitern, aufschlüsseln, App-Dienst, Paket, Erweiterung
ms.localizationpriority: medium
ms.openlocfilehash: d9a98ef8e0ec53668277face05d83c08f6421cb7
ms.sourcegitcommit: c7e10793cbef55ace959ac8fc6ddd08e683602bd
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329513"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Erweitern der App mit Diensten, Erweiterungen und Paketen

Es gibt viele Technologien in Windows 10, um Ihre APP zu erweitern und zu integrieren. Anhand dieser Tabelle können Sie bestimmen, welche Technologie Sie abhängig von den Anforderungen verwenden sollten. Anschließend finden Sie eine kurze Beschreibung der jeweiligen Szenarien und Technologien.

| Szenario                           | Ressourcenpaket   | Bestandspaket      | Optionales Paket   | Flat-Bundle        | App-Erweiterung      | App-Dienst        | Streaming-Installation  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Drittanbieter-Code-Plug-ins            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| In-Process Code-Plugins              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX-Ressourcen (Zeichenfolgen/Images)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| On-Demand Inhalte <br/> (z. b. zusätzliche Ebenen) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Separate Lizenzierung und Erwerb |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| In-App-Erwerb                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimierung der Installationszeit              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Verringern des Speicherbedarfs auf Datenträgern              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimieren der Verpackung                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reduzierung der Veröffentlichungszeit             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Beschreibungen der Szenarien (die Zeilen der obigen Tabelle)

**Plug-ins von Drittanbietern**  

Code, der im Store zum Herunterladen bereitsteht und den Sie über Ihre App ausführen können. Beispielsweise Erweiterungen für den Microsoft Edge-Browser.

**In-proc-Code-Plug-ins**  

Code, der In-Process von Ihrer App ausgeführt wird. Kann auch Inhalte enthalten. Da der Code In-Process ausgeführt wird, wird von einem höheren Maß an Vertrauen ausgegangen. Sie können diese Art der Erweiterbarkeit für einen Drittanbieter nicht verfügbar machen.

**UX-Assets (Zeichenfolge/Bilder)**  

Ressourcen der Benutzeroberfläche wie lokalisierte Zeichenfolgen, Bilder sowie jegliche andere UI-Inhalte, die Sie aufgrund der Sprachumgebung oder anderen Gründen einbeziehen möchten.

**On-Demand-Inhalt**  

Inhalte, die Sie zu einem späteren Zeitpunkt herunterladen möchten. Beispielsweise In-App-Einkäufe, die Ihnen ermöglichen, neue Spielstufen, Designs oder Funktionen herunterzuladen.

**Getrennte Lizenzierung und Erwerb**  

Die Fähigkeit den Inhalt unabhängig von der App zu lizenzieren und zu erwerben.

**In-App-Erwerb**  

Gibt an, ob eine Unterstützung seitens des Programms vorhanden ist, die Inhalte App-intern zu erwerben.

**Optimieren der Installationszeit**

Stellt Funktionen zur Verkürzung der Zeit bereit, die benötigt wird, um die App aus dem Store herunterzuladen und zu starten.

**Reduzieren des Speicherbedarfs auf dem Datenträger** Reduziert die Größe einer App, indem nur notwendige Apps oder Ressourcen einbezogen werden.

**Optimieren der Paket** Erstellung Optimiert den App-Verpackungsprozess für umfangreiche oder komplexe apps.

**Reduzierung der Veröffentlichungszeit** Minimiert den Zeitaufwand für die Veröffentlichung Ihrer App im Store, auf einer lokalen Freigabe oder auf dem Webserver.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Beschreibung der Technologien (die Spalten der obigen Tabelle)

**Ressourcenpaket**

Bei Ressourcenpaketen handelt es sich um an die Ressource gebundene Pakete, die Ihrer App eine Anpassung an zahlreiche Bildschirmgrößen und Systemsprachen ermöglichen. Das Ressourcenpaket zielt auf die Benutzersprache, die Systemskalierung sowie die DirectX-Funktionen ab und erlaubt der App dadurch eine Anpassung an zahlreiche Nutzerszenarien. Obwohl ein App-Paket mehrere Ressourcen enthalten kann, wird das Betriebssystem nur die für das Gerät des Benutzers notwendigen Ressourcen herunterladen. Dies spart Bandbreite und Festplattenspeicher.

**Assetpaket** Assetpakete sind eine gängige, zentralisierte Quelle von ausführbaren Dateien oder nicht ausführbaren Dateien, die von Ihrer APP verwendet werden können. Dabei handelt es sich in der Regel um nicht Prozessor-oder sprachspezifische Dateien. Es kann sich beispielsweise um eine Sammlung von Bildern in einem Bestandspaket und von Videos in einem anderen Bestandspaket handeln, die beide von der App verwendet werden. Wenn Ihre APP mehrere Architekturen und mehrere Sprachen unterstützt, können diese Assets in das Architektur Paket oder das Ressourcenpaket aufgenommen werden. Dies bedeutet aber auch, dass die Assets mehrmals über die verschiedenen Architektur Pakete dupliziert werden. Speicherplatz auf dem Datenträger. Wenn Bestandspakete verwendet werden, müssen sie nur einmal im gesamten App-Paket enthalten sein. Weitere Informationen finden Sie unter [Einführung in Bestandspakete](/windows/msix/package/asset-packages).

**Optionales Paket**

Optionale Pakete dienen entweder der Ergänzung oder Erweiterung der Originalfunktionsweise eines App-Pakets. Es ist möglich, eine App zu veröffentlichen und die optionalen Pakete zu einem späteren Zeitpunkt zu veröffentlichen oder sowohl die App als auch die optionalen Pakete gleichzeitig zu veröffentlichen. Die Erweiterung Ihrer App um optionale Pakete hat zum Vorteil, dass Sie Inhalte als separates App-Paket verbreiten und vermarkten können. Optionale Pakete werden in der Regel vom ursprünglichen App-Entwickler entwickelt, da diese über die Identität der Haupt-App (im Gegensatz zu App-Erweiterungen) ausgeführt werden. Je nach Definition Ihres optionalen Pakets können Sie Code, Ressourcen oder Code und Ressourcen Ihres optionalen Pakets in Ihre Haupt-App laden. Wenn Sie Ihre APP mit Inhalten verbessern müssen, die separat monetarisiert, lizenziert und verteilt werden können, sind optionale Pakete möglicherweise die richtige Wahl. Einzelheiten zur Implementierung, finden Sie unter [Optionale Pakete und die Erstellung zugehöriger Sets](/windows/msix/package/optional-packages).

**Flat-Bundle**
[Flat-Bundle-App-Pakete](/windows/msix/package/flat-bundles.md) ähneln regulären App Bundles, jedoch enthält ein Flat-Bundle nicht die App-Pakete aus dem Ordner, sondern nur *Referenzen* auf diese App-Pakete. Da nur Verweise auf App-Pakete anstelle der Dateien selbst enthalten sind, wird durch ein Flat-Bundle die Zeit verkürzt, die zum Packen und Herunterladen einer App nötig ist.

**App-Erweiterung**

Durch [App-Erweiterungen](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) kann Ihre UWP-App Inhalte hosten, die von anderen UWP-Apps bereitgestellt werden. Sie können auf schreibgeschützte Inhalte dieser Apps zugreifen sowie diese ermitteln und enumerieren.

Wenn eine App Erweiterungen unterstützt, kann jeder beliebige Entwickler Erweiterungen für diese App übermitteln. Daher bedarf es einer soliden Host-App, um Erweiterungen zu laden, die zuvor nicht mit dieser getestet wurde. Erweiterungen sollten als nicht vertrauenswürdig eingestuft werden.

Anwendungen können Code aus Erweiterungen nicht laden. Wenn Sie die Ausführung von Code benötigen, sollten Sie App-Dienste in Erwägung ziehen.

**App Service**

Windows-App-Dienste ermöglichen die Kommunikation zwischen apps, indem Sie der UWP-App erlauben, Dienste für eine andere universelle Windows-App bereitzustellen. App-Dienste ermöglichen Ihnen, Dienste ohne UI zu erstellen, die von Apps auf demselben Gerät aufgerufen werden können. Ab Windows 10, Version 1607 ist dies auch für Remote-Geräte möglich. Weitere Einzelheiten finden Sie unter [App-Service erstellen und verwenden](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service).

App-Dienste sind UWP-Apps, die Dienste für andere UWP-Apps bereitstellen. Sie entsprechen den Webdiensten auf einem Gerät. Ein App-Dienst wird als Hintergrundaufgabe in der Host-App ausgeführt und kann seine Dienste auch anderen Apps bereitstellen. Beispielsweise kann der Barcode-Scanner eines App-Dienstes auch anderen Apps nützlich sein. Oder möglicherweise verfügt eine Enterprise-Suite von Apps über einen gemeinschaftlichen App-Dienst für die Rechtschreibprüfung, der auch den anderen Apps in der Suite zur Verfügung steht.

**UWP-App-Streaming-Installation**

Die Installierung von Streaming ermöglicht Ihnen die Optimierung der Art und Weise, wie Ihre App Benutzern bereitgestellt wird. Anstatt darauf warten zu müssen, dass die gesamte App heruntergeladen ist, bevor sie verwenden werden kann, können Benutzer bereits mit der App interagieren, sobald ein notwendiger Teil heruntergeladen wurde. Es obliegt Ihnen, als Entwickler, Ihre App entsprechend in einen für die primäre Aktivierung und Initialisierung erforderlichen Bereich sowie einen Bereich für die restlichen Inhalte der App zu gliedern. Weitere Informationen sowie Einzelheiten zur Implementierung finden Sie unter [UWP-App-Streaming installieren](/windows/msix/package/streaming-install).

## <a name="see-also"></a>Weitere Informationen

[Erstellen und Verwenden eines App-Diensts](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Einführung in Asset-Pakete](/windows/msix/package/asset-packages)  
[Paket Erstellung mit dem Verpackungs Layout](/windows/msix/package/packaging-layout)  
[Optionale Pakete und die Erstellung zugehöriger Sets](/windows/msix/package/optional-packages)  
[Entwickeln mit Asset-Paketen und Paket Faltung](/windows/msix/package/package-folding)  
[Installieren von UWP-App-Streaming](/windows/msix/package/streaming-install)  
[Flatbundle-App-Pakete](/windows/msix/package/flat-bundles)  
[Windows. applicationmodel. Appservice-Namespace](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Windows. applicationmodel. Extensions-Namespace](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
