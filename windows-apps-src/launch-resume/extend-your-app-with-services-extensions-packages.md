---
title: Erweitern der App mit Diensten, Erweiterungen und Paketen
description: Beschreibt, wie eine Hintergrundaufgabe erstellt wird, die ausgeführt wird, wenn die universelle Windows-Plattform (UWP) Store-APP aktualisiert wird.
ms.date: 05/07/2018
ms.topic: article
keywords: Windows 10, UWP, erweitert, componentize, App Service, Package, Extension
ms.localizationpriority: medium
ms.openlocfilehash: 9239178701082012f2878e996ede2f3f56a9a94b
ms.sourcegitcommit: 7b2febddb3e8a17c9ab158abcdd2a59ce126661c
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/31/2020
ms.locfileid: "89155874"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Erweitern der App mit Diensten, Erweiterungen und Paketen

Es gibt viele Technologien in Windows 10, um Ihre APP zu erweitern und zu integrieren. Anhand dieser Tabelle können Sie bestimmen, welche Technologie Sie abhängig von den Anforderungen verwenden sollten. Danach folgt eine kurze Beschreibung der Szenarien und Technologien.

| Szenario                           | Ressourcenpaket   | Assetpaket      | Optionale Pakete   | Flatbundle        | App-Erweiterung      | App Service        | Streaming-Installation  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Drittanbieter-Code-Plug-ins            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| In-proc-Code-Plug-ins              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| UX-Assets (Zeichen folgen/Bilder)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| On-Demand-Inhalt <br/> (z. b. zusätzliche Ebenen) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Getrennte Lizenzierung und Erwerb |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| In-App-Erwerb                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Optimieren der Installationszeit              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Verringerung des Datenträgers              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Optimieren der Paket Erstellung                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Verkürzen der Veröffentlichungszeit             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Szenariobeschreibungen (die Zeilen in der obigen Tabelle)

**Plug-ins von Drittanbietern**  

Code, den Sie aus dem Store herunterladen und von Ihrer APP aus ausführen können. Beispielsweise Erweiterungen für den Microsoft Edge-Browser.

**In-proc-Code-Plug-ins**  

Code, der in-Process mit ihrer app ausgeführt wird. Kann auch Inhalt enthalten. Da der Code Prozess seitig ausgeführt wird, wird ein höheres Maß an Vertrauenswürdigkeit angenommen. Sie können diese Art der Erweiterbarkeit für einen Drittanbieter nicht verfügbar machen.

**UX-Assets (Zeichenfolge/Bilder)**  

Benutzeroberflächen Objekte, wie z. b. lokalisierte Zeichen folgen, Bilder und andere UI-Inhalte, die Sie basierend auf dem Gebiets Schema oder einem anderen Grund berücksichtigen möchten.

**On-Demand-Inhalt**  

Inhalt, den Sie zu einem späteren Zeitpunkt herunterladen möchten. Beispielsweise in-App-Käufe, mit denen Sie neue Ebenen, Skins oder Funktionen herunterladen können.

**Getrennte Lizenzierung und Erwerb**  

Die Möglichkeit, den Inhalt unabhängig von der APP zu lizenzieren und zu erwerben.

**In-App-Erwerb**  

Gibt an, ob programmgesteuerte Unterstützung zum Abrufen des Inhalts innerhalb der APP vorliegt.

**Optimieren der Installationszeit**

Stellt Funktionen bereit, um die Zeit zu verkürzen, die benötigt wird, um die APP aus dem Store abzurufen und zu starten.

**Verringerung** des Datenträgers Verringert die Größe einer APP, indem nur erforderliche Apps oder Ressourcen eingeschlossen werden.

**Optimieren der Paket** Erstellung Optimiert den App-Verpackungsprozess für umfangreiche oder komplexe apps.

**Verkürzen der Veröffentlichungszeit** Minimieren Sie die Zeitspanne, die für die Veröffentlichung der APP im Store, in der lokalen Freigabe oder im Webserver benötigt wird.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Technologie Beschreibungen (die Spalten in der obigen Tabelle)

**Ressourcenpaket**

Ressourcen Pakete sind reine Medienobjekte, mit denen Ihre APP an verschiedene Anzeige Größen und Systemsprachen angepasst werden kann. Das Ressourcenpaket zielt auf Benutzersprache, System Skalierung und DirectX-Features ab, sodass die APP auf eine Vielzahl von Benutzer Szenarien zugeschnitten werden kann. Obwohl ein App-Paket mehrere Ressourcen enthalten kann, lädt das Betriebssystem nur die relevanten Ressourcen pro Benutzergerät herunter, sodass Bandbreite und Speicherplatz gespart werden.

**Assetpaket** Assetpakete sind eine gängige, zentralisierte Quelle von ausführbaren Dateien oder nicht ausführbaren Dateien, die von Ihrer APP verwendet werden können. Dabei handelt es sich in der Regel um nicht Prozessor-oder sprachspezifische Dateien. Dies kann z. b. eine Sammlung von Bildern in einem Medienobjekt Paket und Videos in einem anderen Ressourcenpaket enthalten, die beide von der APP verwendet werden. Wenn Ihre APP mehrere Architekturen und mehrere Sprachen unterstützt, können diese Assets in das Architektur Paket oder das Ressourcenpaket aufgenommen werden. Dies bedeutet aber auch, dass die Assets mehrmals in den verschiedenen Architektur Paketen dupliziert werden, wobei Speicherplatz in Anspruch genommen wird. Wenn Asset-Pakete verwendet werden, müssen Sie nur einmal in das gesamte App-Paket eingeschlossen werden. Weitere Informationen finden Sie unter [Einführung in Asset-Pakete](/windows/msix/package/asset-packages) .

**Optionale Pakete**

Optionale Pakete werden verwendet, um die ursprüngliche Funktionalität eines App-Pakets zu ergänzen oder zu erweitern. Es ist möglich, eine APP zu veröffentlichen, gefolgt von optionalen Paketen zu einem späteren Zeitpunkt zu veröffentlichen oder die APP und optionale Pakete gleichzeitig zu veröffentlichen. Wenn Sie Ihre APP über ein optionales Paket erweitern, haben Sie die Vorteile, Inhalte als separates App-Paket zu verteilen und zu monetarialisieren. Optionale Pakete sind in der Regel vom ursprünglichen App-Entwickler entwickelt worden, da Sie mit der Identität der Haupt-app (im Gegensatz zu app-Erweiterungen) ausgeführt werden. Abhängig von der Definition des optionalen Pakets können Sie Code, Ressourcen oder Code und Assets aus dem optionalen Paket in Ihre Haupt-App laden. Wenn Sie Ihre APP mit Inhalten verbessern müssen, die separat monetarisiert, lizenziert und verteilt werden können, sind optionale Pakete möglicherweise die richtige Wahl. Weitere Informationen zur Implementierung finden Sie unter [Optionale Pakete und zugehörige Gruppen Erstellung](/windows/msix/package/optional-packages).

**Flatbundle** 
 [Flatbundle-App-Pakete](/windows/msix/package/flat-bundles) ähneln regulären App-Paketen, mit dem Unterschied, dass das flatbundle nur *Verweise* auf diese APP-Pakete enthält, anstatt alle App-Pakete in den Ordner einzubeziehen. Durch das enthalten von Verweisen auf App-Pakete anstelle der eigentlichen Dateien reduziert ein flatbundle die Zeitspanne, die zum Packen und Herunterladen einer APP benötigt wird.

**App-Erweiterung**

[App-Erweiterungen](/uwp/api/windows.applicationmodel.appextensions) ermöglichen der UWP-APP das Hosten von Inhalten, die von anderen UWP-apps bereitgestellt Sie können schreibgeschützte Inhalte dieser Apps ermitteln und enumerieren und darauf zugreifen.

Wenn eine APP Erweiterungen unterstützt, kann jeder Entwickler eine Erweiterung für die APP übermitteln. Daher muss die Host-App robust sein, wenn Sie eine Erweiterung lädt, mit der Sie nicht bereits getestet wurde. Erweiterungen sollten als nicht vertrauenswürdig eingestuft werden.

Anwendungen können keinen Code aus Erweiterungen laden. Wenn Sie Codeausführung benötigen, sollten Sie App Services in Erwägung gezogen.

**App Service**

Windows-App-Dienste ermöglichen die Kommunikation zwischen apps, indem Sie der UWP-App erlauben, Dienste für eine andere universelle Windows-App bereitzustellen. Mit App Services können Sie Dienste ohne Benutzeroberfläche erstellen, die apps auf demselben Gerät und ab Windows 10, Version 1607, auf Remote Geräten abrufen können. Weitere Informationen finden Sie unter [Erstellen und Verwenden eines App Service](./how-to-create-and-consume-an-app-service.md) .

App Services sind UWP-apps, die Dienste für andere UWP-apps bereitstellen. Sie entsprechen den Webdiensten auf einem Gerät. Ein App Service wird als Hintergrundaufgabe in der Host-app ausgeführt und kann den Dienst für andere apps bereitstellen. Beispielsweise kann ein App Service einen Barcode-Überprüfungs Dienst bereitstellen, der von anderen Anwendungen verwendet werden kann. Oder eine Unternehmens Suite von apps hat eine gängige Rechtschreibprüfung App Service, die für die anderen apps in der Suite verfügbar ist.

**UWP-App-Streaming-Installation**

Mit der Streaming-Installation können Sie optimieren, wie Ihre APP an Benutzer übermittelt wird. Anstatt zu warten, bis die gesamte App heruntergeladen wurde, bevor Sie Sie verwenden können, können Benutzer mit der app in Kontakt treten, sobald ein erforderlicher Teil heruntergeladen wurde. Sie müssen als Entwickler ihre app in einen erforderlichen Bereich aufteilen, um die grundlegende Aktivierung und den Start sowie zusätzliche Inhalte für den Rest der APP zu überprüfen. Weitere Informationen und Implementierungsdetails finden Sie unter [UWP-App-streaminginstallation](/windows/msix/package/streaming-install) .

## <a name="see-also"></a>Weitere Informationen

[Erstellen und Verwenden eines App-Diensts](./how-to-create-and-consume-an-app-service.md)  
[Einführung in Bestandspakete](/windows/msix/package/asset-packages)  
[Paketerstellung mit dem Verpackungslayout](/windows/msix/package/packaging-layout)  
[Optionale Pakete und die Erstellung zugehöriger Sets](/windows/msix/package/optional-packages)  
[Entwickeln mit Bestandspaketen und Paketfaltung](/windows/msix/package/package-folding)  
[Installieren von UWP-App-Streaming](/windows/msix/package/streaming-install)  
[Flat-Bundle-App-Pakete](/windows/msix/package/flat-bundles)  
[Windows. applicationmodel. Appservice-Namespace](/uwp/api/Windows.ApplicationModel.AppService)  
[Windows. applicationmodel. Extensions-Namespace](/uwp/api/windows.applicationmodel.appextensions)