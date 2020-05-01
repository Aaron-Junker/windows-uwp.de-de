---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Datenbindungen und MVVM
description: Die Grundlage des Architekturentwurfsmusters MVVM (Model-View-ViewModel) ist die Datenbindung, die eine lose Kopplung zwischen UI-Code und Nicht-UI-Code ermöglicht.
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "63798107"
---
# <a name="data-binding-and-mvvm"></a>Datenbindungen und MVVM

„Model-View-ViewModel“ (MVVM) ist ein Entwurfsmuster für Benutzeroberflächenarchitekturen zum Entkoppeln von UI-Code und Nicht-UI-Code. Mit MVVM definieren Sie Ihre Benutzeroberfläche deklarativ in XAML und verwenden Datenbindungsmarkup, um es mit anderen Ebenen zu verknüpfen, die Daten und Befehle enthalten. Die Datenbindungsinfrastruktur bietet eine lose Kopplung, bei der die Benutzeroberfläche und die verknüpften Daten synchronisiert bleiben und Benutzereingaben an die entsprechenden Befehle weitergeleitet werden. 

Da es eine lose Kopplung bietet, reduziert die Verwendung der Datenbindung harte Abhängigkeiten zwischen verschiedenen Arten von Code. Dies vereinfacht das Ändern einzelner Codekomponenten (Methoden, Klassen, Steuerelemente usw.), ohne dass dies zu unbeabsichtigten Nebeneffekten in anderen Einheiten führt. Diese Entkopplung ist ein Beispiel für die *Trennung von Belangen*, bei der es sich um ein wichtiges Konzept in vielen Entwurfsmustern handelt. 

## <a name="benefits-of-mvvm"></a>Vorteile von MVVM

Die Entkopplung Ihres Codes bietet zahlreiche Vorteile, einschließlich:

* Ermöglichen eines iterativen, explorativen Programmierstils. Isolierte Änderungen sind weniger riskant, und man kann leichter damit experimentieren.
* Vereinfachung von Komponententests. Codekomponenten, die voneinander isoliert sind, können einzeln und außerhalb von Produktionsumgebungen getestet werden.
* Unterstützung der Teamzusammenarbeit. Entkoppelter Code, der gut entworfene Schnittstellen einhält, kann von separaten Personen oder Teams entwickelt und später integriert werden.
* Verbesserung der Verwaltbarkeit. Das Korrigieren von Fehlern in entkoppeltem Code führt mit geringerer Wahrscheinlichkeit zu Regressionen in anderem Code.

Anders als beim MVVM verwendet eine App mit einer stärker konventionellen „CodeBehind“-Struktur in der Regel Datenbindungen für reine Anzeigedaten und reagiert auf Benutzereingaben durch direkte Verarbeitung von Ereignissen, die von Steuerelementen verfügbar gemacht werden. Die Ereignishandler werden in CodeBehind-Dateien (z. B. MainPage.xaml.cs) implementiert und sind häufig eng an die Steuerelemente gekoppelt, die üblicherweise Code enthalten, der die Benutzeroberfläche direkt manipuliert. Dadurch wird es schwierig oder sogar unmöglich, ein Steuerelement zu ersetzen, ohne den Code für die Ereignisbehandlung aktualisieren zu müssen. Bei dieser Architektur häufen CodeBehind-Dateien häufig Code an, der sich nicht direkt auf die Benutzeroberfläche bezieht, wie z. B. Datenbankzugriffscode, der schließlich dupliziert und für die Verwendung mit anderen Seiten geändert wird.

## <a name="app-layers"></a>App-Ebenen

Bei Verwendung des MVVM-Musters wird eine App in die folgenden Ebenen unterteilt:

* Die **Modell**ebene definiert die Typen, die Ihre Geschäftsdaten darstellen. Dies umfasst alles, was erforderlich ist, um die Kerndomäne der App zu modellieren, und umfasst häufig Kernlogik der App. Diese Ebene ist vollständig unabhängig von den Ansichts- und Ansichtsmodellebenen und befindet sich häufig teilweise in der Cloud. Bei einer vollständig implementierten Modellebene können Sie bei Bedarf mehrere verschiedene Client-Apps erstellen, z. B. UWP- und Web-Apps, die mit denselben zugrunde liegenden Daten funktionieren.
* Die **Ansichts**ebene definiert die Benutzeroberfläche mithilfe von XAML-Markup. Das Markup enthält Datenbindungsausdrücke (z. B. [x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)), die die Verbindung zwischen bestimmten Benutzeroberflächenkomponenten und verschiedenen Ansichtsmodell- und Modellmembern definieren. CodeBehind-Dateien werden manchmal als Teil der Ansichtsebene verwendet, um zusätzlichen Code aufzunehmen, der zum Anpassen oder Manipulieren der Benutzeroberfläche erforderlich ist, oder um Daten aus Ereignishandlerargumenten zu extrahieren, bevor eine Ansichtsmodellmethode aufgerufen wird, die die Arbeit ausführt. 
* Die **Ansichtsmodell**ebene stellt Datenbindungsziele für die Ansicht bereit. In vielen Fällen macht das Ansichtsmodell das Modell direkt verfügbar oder stellt Member bereit, die bestimmte Modellmember umschließen. Das Ansichtsmodell kann auch Member zum Nachverfolgen von Daten definieren, die für die Benutzeroberfläche relevant sind, aber nicht für das Modell, wie z. B. die Anzeigereihenfolge einer Liste von Elementen. Das Ansichtsmodell fungiert außerdem als Ort für die Integration anderer Dienste, z. B. dem Datenbankzugriffscode. Bei einfachen Projekten benötigen Sie möglicherweise keine separate Modellebene, sondern nur ein Ansichtsmodell, das alle benötigten Daten kapselt. 

## <a name="basic-and-advanced-mvvm"></a>Einfaches und erweitertes MVVM

Wie bei jedem beliebigen Entwurfsmuster gibt es mehr als eine Möglichkeit zum Implementieren von MVVM, und viele verschiedene Verfahren werden als Teil von MVVM angesehen. Aus diesem Grund gibt es mehrere verschiedene MVVM-Frameworks von Drittanbietern, die die verschiedenen XAML-basierten Plattformen, einschließlich UWP, unterstützen. Diese Frameworks enthalten jedoch im Allgemeinen mehrere Dienste für die Implementierung entkoppelter Architektur, wodurch die exakte Definition von MVVM etwas mehrdeutig wird. 

Obwohl anspruchsvolle MVVM-Frameworks sehr nützlich sein können, insbesondere bei Projekten auf Unternehmensebene, gehen in der Regel mit der Übernahme eines bestimmten Musters oder Verfahrens Kosten einher, und die Vorteile sind nicht immer einsichtig, abhängig von der Skalierung und Größe Ihres Projekts. Glücklicherweise können Sie nur die Verfahren übernehmen, die einen deutlichen und konkreten Vorteil bieten, und andere ignorieren, bis Sie sie benötigen. 

Insbesondere können Sie einen großen Vorteil erzielen, indem Sie einfach die gesamte Leistungsfähigkeit der Datenbindung verstehen und anwenden und Ihre App-Logik trennen und in die zuvor beschriebenen Ebenen aufteilen. Dies können Sie erreichen, indem Sie nur die vom Windows SDK bereitgestellten Funktionen, ohne Einbeziehung externere Frameworks, verwenden. Besonders die [Markuperweiterung {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) vereinfacht die Datenbindung und erhöht deren Leistungsfähigkeit gegenüber früheren XAML-Plattformen, indem viele der früher erforderlichen Codebausteine überflüssig werden.

Weitere Anleitungen zur Verwendung des einfachen, sofort einsatzbereiten MVVM finden Sie im [Kundenauftragsdatenbank-Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) auf GitHub. Viele der anderen [UWP-App-Beispiele](https://github.com/Microsoft?q=windows-appsample
) verwenden ebenfalls eine einfache MVVM-Architektur, und das [Beispiel für eine App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp) enthält sowohl eine CodeBehind- als auch eine MVVM-Version, zusammen mit Anmerkungen, die die [MVVM-Konvertierung](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md) beschreiben. 

## <a name="see-also"></a>Siehe auch

### <a name="topics"></a>Themen

[Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Markuperweiterung {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Beispiele

[Kundenauftragsdatenbank-Beispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel-Bestandsbeispiel](https://github.com/Microsoft/InventorySample)  
[Beispiel für eine App mit Verkehrsinformationen](https://github.com/Microsoft/Windows-appsample-trafficapp)  
