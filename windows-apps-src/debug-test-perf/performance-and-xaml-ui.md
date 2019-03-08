---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Leistung
description: Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c105425be5b8eb56f32956f126a8f6c2c4f30f2e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57644275"
---
# <a name="performance"></a>Leistung


Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. In diesem Abschnitt erfahren Sie, wie Sie Ihren Leistungsworkflow strukturieren, Animationsfehler und Probleme mit der Bildfrequenz beheben und Startzeit, Seitennavigationszeit und Speicherverwendung optimieren.

Wenn Sie dies noch einem Schritt, der nicht getan haben wir gesehen, zu erheblichen leistungsverbesserungen nur Ihre app für Windows 10 Portieren ist. Einige XAML-Optimierungen (z. B. [{X: Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) sind nur in Windows 10-apps verfügbar. Finden Sie unter [apps auf Windows 10 Portieren](https://msdn.microsoft.com/library/windows/apps/Mt238321) und die Sitzung //build/ [verschieben in die universelle Windows-Plattform](https://channel9.msdn.com/Events/Build/2015/3-741).

| Thema | Beschreibung |
|-------|-------------|
| [Planen für Leistung](planning-and-measuring-performance.md) | Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. |
| [Optimieren der Hintergrundaktivität](optimize-background-activity.md) | Erstellen Sie UWP-Apps, die mit dem System interagieren, um Hintergrundaufgaben auf eine den Akku effizient nutzende Weise auszuführen. |
| [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) durch UI-Virtualisierung, Elementreduzierung und progressive Aktualisierung von Elementen. |
| [Virtualisierung von ListView- und GridView-Daten](listview-and-gridview-data-optimization.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) durch Datenvirtualisierung. |
| [Verbesserung der Garbage Collection-Leistung](improve-garbage-collection-performance.md) | In C# und Visual Basic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NET Garbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NET Garbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps. |
| [Aufrechterhalten der Reaktionsfähigkeit des UI-Threads](keep-the-ui-thread-responsive.md) | Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp. Das bedeutet für jede App etwas anderes. Für einige Apps bedeutet dies z. B. Folgendes: realistischere physische Effekte bereitstellen, Daten vom Datenträger oder aus dem Internet schneller laden, komplexe Szenen schnell darstellen, schnell zwischen Seiten navigieren, Anweisungen im Nu finden oder schnelles Verarbeiten von Daten. Unabhängig von der Art der Berechnung möchten Benutzer, dass die App auf ihre Eingabe reagiert. Es stört sie, wenn die App scheinbar nicht reagiert, während sie &quot;denkt&quot;. |
| [Optimieren Ihres XAML-Markups](optimize-xaml-loading.md) | Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können. | 
| [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md) | Das Layout kann sowohl bezüglich der CPU-Auslastung als auch des Aufwands ein ressourcenintensiver Teil einer XAML-App sein. Hier sind einige einfache Schritte, mit denen Sie die Layoutleistung Ihrer XAML-App verbessern können. | 
| [Leistungstipps für MVVM und Sprache](mvvm-performance-tips.md) | In diesem Thema werden einige Leistungsaspekte in Bezug auf die Wahl von Softwaredesignmustern und Programmiersprachen erläutert. |
| [Bewährte Methoden für die Leistung Ihrer app starten](best-practices-for-your-app-s-startup-performance.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit optimalen Startzeiten, indem Sie die Vorgehensweise beim Starten und Aktivieren optimieren. |
| [Optimieren von Bildern, Medien und Animationen](optimize-animations-and-media.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe. |
| [Suspend/Resume-optimieren](optimize-suspend-resume.md) | Erstellen Sie UWP-Apps Universelle Windows-Plattform), die die Verwendung des Prozesslebensdauer-Systems optimieren und nach dem Anhalten oder Beenden effizient fortgesetzt werden. |
| [Optimieren Sie den Zugriff auf Dateien](optimize-file-access.md) | Erstellen Sie UWP-Apps, die effizient auf das Dateisystem zugreifen und dadurch Leistungsprobleme aufgrund von Datenträgerlatenz und Arbeitsspeicher-/CPU-Zyklen vermeiden. |
| [Windows-Runtime-Komponenten und Optimieren von interop](windows-runtime-components-and-optimizing-interop.md) | Erstellen Sie UWP-Apps, die UWP-Komponenten verwenden, mit systemeigenen und verwalteten Typen zusammenarbeiten und gleichzeitig Probleme mit der Interoperabilitätsleistung vermeiden. |
| [Tools zur profilerstellung und Leistung](tools-for-profiling-and-performance.md) | Microsoft bietet verschiedene Tools zur Verbesserung der Leistung Ihrer UWP-App.|

