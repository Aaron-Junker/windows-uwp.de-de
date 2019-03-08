---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Datenbindungen und MVVM
description: Die Datenbindung ist das Herzstück von der architektonisches Entwurfsmuster von Model-View-ViewModel (MVVM)-Benutzeroberfläche und losen Kopplung zwischen Benutzeroberfläche und nicht-UI-Code ermöglicht.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616735"
---
# <a name="data-binding-and-mvvm"></a>Datenbindungen und MVVM

Model-View-ViewModel (MVVM) ist ein architektonisches Entwurfsmuster Benutzeroberfläche für die Benutzeroberfläche und nicht-UI-Code entkoppeln. Mit MVVM, wenn Sie eine Benutzeroberfläche deklarativ in XAML definieren und verwenden Daten bindungsmarkup, um es auf anderen Ebenen, die mit Daten und Befehle zu verknüpfen. Die Infrastruktur für die Bindung enthält eine lose Kopplung, die die Benutzeroberfläche behält und die verknüpften Daten synchronisiert, und leitet Sie Benutzereingaben in die entsprechenden Befehle. 

Da es sich um lose Kopplung bereitstellt, wird die Verwendung der Datenbindung feste Abhängigkeiten zwischen verschiedenen Arten von Code reduziert. Dies erleichtert es, einzelne Codeeinheiten (Methoden, Klassen, Steuerelemente usw.) zu ändern, ohne dass unbeabsichtigte Nebeneffekte in andere Einheiten. Diese Entkopplung ist ein Beispiel der *Trennung der Belange*, dies ist ein wichtiges Konzept bei der viele Entwurfsmuster. 

## <a name="benefits-of-mvvm"></a>Vorteile von MVVM

Trennen Ihren Code hat viele Vorteile, darunter:

* Aktivieren ein iteratives, explorative Programmierstil. Änderung isoliert ist weniger riskant und einfacher zu experimentieren.
* Vereinfachen Komponententests. Codeeinheiten, die voneinander isoliert sind, können einzeln oder außerhalb von produktionsumgebungen getestet werden.
* Zusammenarbeit im Team die Unterstützung. Entkoppelte Code, der die durchdachte Schnittstellen entspricht kann später noch Mal von verschiedenen Personen oder Teams entwickelt, und integriert werden.
* Verbessern der Verwaltbarkeit. Beheben von Fehlern in entkoppelte Code ist weniger wahrscheinlich dazu führen, dass Regressionen in anderem Code.

Im Gegensatz zu MVVM eine app mit einer konventionellere "Code-Behind"-Struktur in der Regel verwendet die Datenbindung für nur-Anzeige-Daten und reagiert auf Benutzereingaben, indem Sie direkt behandeln von Ereignissen, die von Steuerelementen verfügbar gemacht werden. Die Ereignishandler im Code-Behind-Dateien (z. B. "MainPage.Xaml.cs") implementiert werden und sind oft eng gekoppelt, auf die Steuerelemente, die in der Regel enthalten Code, der die Benutzeroberfläche direkt bearbeitet. Dies macht es schwierig oder unmöglich, ein Steuerelement zu ersetzen, ohne den Ereignisbehandlungscode aktualisieren zu müssen. Mit dieser Architektur sammeln Code-Behind-Dateien häufig Code, der direkt an die Benutzeroberfläche, z. B. Datenbank-Access-Code ist nicht verknüpft, die mit letztendlich dupliziert und für die Verwendung mit anderen Seiten geändert wird.

## <a name="app-layers"></a>App-Ebenen

Wenn Sie das MVVM-Muster verwenden, ist eine app in den folgenden Ebenen unterteilt:

* Die **Modell** Ebene definiert die Typen, die Ihre Daten darstellen. Dies enthält alles, was erforderlich, um die Core-app-Domäne zu modellieren und beinhaltet häufig die Core-app-Logik. Diese Ebene ist unabhängig von der Ansicht und Ansichtsmodell Ebenen, und häufig teilweise in der Cloud befinden. Eine vollständig implementierte Modellebene können Sie mehrere verschiedene Client apps erstellen, wenn Sie aber, wie z. B. UWP und Web-apps, die mit den gleichen zugrunde liegenden Daten arbeiten.
* Die **Ansicht** Ebene definiert die Benutzeroberfläche mit XAML-Markup. Das Markup enthält Datenbindungsausdrücke (z. B. [X: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)), die die Verbindung zwischen bestimmten UI-Komponenten und verschiedenen Anzeigemodell und Model-Member definieren. Code-Behind-Dateien werden manchmal als Teil der ansichtsschicht verwendet, enthalten zusätzlichen Code erforderlich, anpassen oder die Benutzeroberfläche verändern oder Extrahieren von Daten aus Event Handler Argumente vor dem Aufruf einer Ansichtsmodell-Methode, die Arbeit ausführt. 
* Die **Anzeigemodell** Ebene Bindungsziele Daten für die Ansicht bereitstellt. In vielen Fällen das Anzeigemodell macht das Modell direkt verfügbar und enthält Member, die bestimmte Modellelemente zu umschließen. Das Ansichtsmodell kann auch die Elemente definieren, zum Nachverfolgen der Daten, die relevant ist an der Benutzeroberfläche, aber nicht auf das Modell, z. B. die Anzeige einer Liste von Elementen. Das Ansichtsmodell dient auch als Integrationspunkt mit anderen Diensten wie Datenbank-Zugriffscode. Bei einfachen Projekten können es nicht erforderlich, ein separates Modell-Ebene, jedoch nur einem Ansichtsmodell, das alle Daten kapselt, die Sie benötigen. 

## <a name="basic-and-advanced-mvvm"></a>Grundlegende und erweiterte MVVM

Wie bei jedem Entwurfsmuster, es gibt mehr als eine Möglichkeit zum Implementieren von MVVM, und viele verschiedene Techniken gelten als Teil von MVVM. Aus diesem Grund stehen Ihnen mehrere verschiedene Drittanbieter-MVVM-Frameworks die verschiedenen, einschließlich UWP XAML-basierten Plattformen unterstützt. Diese Frameworks können jedoch im Allgemeinen mehrere Dienste für die Implementierung von entkoppelten Architektur, sodass die genaue Definition von MVVM nicht eindeutig. 

Zwar anspruchsvolle MVVM-Frameworks sehr nützlich sein, insbesondere für Unternehmen-Projekte ist in der Regel Kosten bei der Einführung von bestimmten Muster oder Verfahren und die Vorteile sind nicht immer klar ist, abhängig von der Skalierung und die Größe des Ihr Projekt. Glücklicherweise können Sie nur diese Techniken, die eine klare und greifbare Leistung bereitstellen zu übernehmen und andere ignoriert, bis Sie sie benötigen. 

Insbesondere erhalten Sie viele Vorteile, einfach durch Verständnis und das volle Potenzial von Datenbindung und das Trennen Ihrer app-Logik in der zuvor beschriebenen Schichten anwenden. Dies kann erreicht werden, verwenden nur die Funktionen, die durch das Windows SDK und ohne alle externen Frameworks zur Verfügung gestellt. Insbesondere die [{X: Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) können Sie die Datenbindung, einfacher und höhere Leistung als im vorherigen XAML-Plattformen einen Großteil des Standardcodes zuvor überflüssig.

Weitere Anleitungen zur Verwendung von basic "," Out-of-the-Box-MVVM finden Sie in der [Kunden Bestellungen-Datenbankbeispiel](https://github.com/Microsoft/Windows-appsample-customers-orders-database) auf GitHub. Viele der anderen [UWP-app-Beispiele](https://github.com/Microsoft?q=windows-appsample
) auch verwenden, eine grundlegende MVVM-Architektur und die [Datenverkehr-App-Beispiels](https://github.com/Microsoft/Windows-appsample-trafficapp) umfasst sowohl Code-Behind und MVVM-Versionen, mit Anmerkungen zu dieser Version, die beschreibt die [MVVM-Konvertierung ](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Siehe auch

### <a name="topics"></a>Themen

[Die Datenbindung im Detail](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[{X: Bind}-Markuperweiterung](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Beispiele

[Kunden, Bestellungen-Beispieldatenbank](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[VanArsdel-Inventar-Beispiel](https://github.com/Microsoft/InventorySample)  
[Beispiel-App Datenverkehr](https://github.com/Microsoft/Windows-appsample-trafficapp)  
