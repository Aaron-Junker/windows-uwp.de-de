---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Leistung
description: Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: c13fb0f1b3d982d372896547bf2e47d978532ea4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 04/29/2020
ms.locfileid: "70393449"
---
# <a name="performance"></a>Leistung


Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. In diesem Abschnitt erfahren Sie, wie Sie Ihren Leistungsworkflow strukturieren, Animationsfehler und Probleme mit der Bildfrequenz beheben und Startzeit, Seitennavigationszeit und Speicherverwendung optimieren.

Sofern dies noch nicht geschehen ist, kannst du durch einfaches Portieren deiner App auf Windows 10 als Ziel erhebliche Leistungssteigerungen erzielen. Verschiedene XAML-Optimierungen (z. B. [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) sind nur in Windows 10-Apps verfügbar. Weitere Informationen findest du unter [Portieren von Apps zu Windows 10](https://docs.microsoft.com/windows/uwp/porting/index) und in der //build/-Sitzung [Wechseln zur universellen Windows-Plattform](https://channel9.msdn.com/Events/Build/2015/3-741).

| Thema | Beschreibung |
|-------|-------------|
| [Planen der Leistung](planning-and-measuring-performance.md) | Benutzer erwarten, dass ihre Apps zuverlässig und reibungslos funktionieren und den Akku nicht übermäßig beanspruchen. Technisch gesehen ist die Leistung keine funktionale Anforderung. Wenn Sie die Leistung aber als Feature behandeln, hilft es Ihnen dabei, die Erwartungen der Benutzer zu erfüllen. Das Festlegen von Zielen und deren Messung sind wichtige Faktoren. Ermitteln Sie die für Sie leistungskritischen Szenarien, und legen Sie fest, was unter guter Leistung zu verstehen ist. Messen Sie die Ziele dann während des gesamten Lebenszyklus Ihres Projekts frühzeitig und häufig, um sicherzustellen, dass Sie Ihre Ziele erreichen. |
| [Optimieren von Hintergrundaktivitäten](optimize-background-activity.md) | Erstellen Sie UWP-Apps, die mit dem System interagieren, um Hintergrundaufgaben auf eine den Akku effizient nutzende Weise auszuführen. |
| [Optimieren der ListView- und GridView-Benutzeroberfläche](optimize-gridview-and-listview.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) durch UI-Virtualisierung, Elementreduzierung und progressive Aktualisierung von Elementen. |
| [Virtualisierung von ListView- und GridView-Daten](listview-and-gridview-data-optimization.md) | Verbessern Sie die Leistung und Startzeit von [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) durch Datenvirtualisierung. |
| [Verbessern der Leistung bei der Garbage Collection](improve-garbage-collection-performance.md) | In C# und Visual Basic geschriebene UWP-Apps profitieren von der automatischen Arbeitsspeicherverwaltung des .NET Garbage Collectors. Dieser Abschnitt bietet einen Überblick über das Verhalten des .NET Garbage Collectors sowie über die bewährten Methoden zur Leistungssteigerung für den Garbage Collector in UWP-Apps. |
| [Aufrechterhalten der Reaktionsfähigkeit des UI-Threads](keep-the-ui-thread-responsive.md) | Benutzer erwarten, dass eine App beim Durchführen einer Berechnung reaktionsfähig bleibt, unabhängig vom jeweiligen Computertyp. Das bedeutet für jede App etwas anderes. Für einige Apps bedeutet dies z. B. Folgendes: realistischere physische Effekte bereitstellen, Daten vom Datenträger oder aus dem Internet schneller laden, komplexe Szenen schnell darstellen, schnell zwischen Seiten navigieren, Anweisungen im Nu finden oder schnelles Verarbeiten von Daten. Unabhängig von der Art der Berechnung möchten Benutzer, dass die App auf ihre Eingabe reagiert. Es stört sie, wenn die App scheinbar nicht reagiert, während sie &quot;denkt&quot;. |
| [Optimieren Ihres XAML-Markups](optimize-xaml-loading.md) | Die Analyse von XAML-Markup zum Erstellen von Objekten im Arbeitsspeicher kann für eine komplexe Benutzeroberfläche viel Zeit in Anspruch nehmen. Hier finden Sie einige Punkte, die Sie zur Optimierung der XAML-Markupanalyse, Ladezeit und Effizienz des Arbeitsspeichers für Ihre App vornehmen können. | 
| [Optimieren des XAML-Layouts](optimize-your-xaml-layout.md) | Das Layout kann sowohl bezüglich der CPU-Auslastung als auch des Aufwands ein ressourcenintensiver Teil einer XAML-App sein. Hier sind einige einfache Schritte, mit denen Sie die Layoutleistung Ihrer XAML-App verbessern können. | 
| [Tipps zu MVVM und Sprachleistung](mvvm-performance-tips.md) | In diesem Thema werden einige Leistungsaspekte in Bezug auf die Wahl von Softwaredesignmustern und Programmiersprachen erläutert. |
| [Bewährte Methoden für die Leistung deiner App beim Starten](best-practices-for-your-app-s-startup-performance.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit optimalen Startzeiten, indem Sie die Vorgehensweise beim Starten und Aktivieren optimieren. |
| [Optimieren von Animationen, Medien und Bildern](optimize-animations-and-media.md) | Erstellen Sie UWP-Apps (Universelle Windows-Plattform) mit flüssigen Animationen, hoher Bildfrequenz und leistungsstarker Medienaufzeichnung und -wiedergabe. |
| [Optimieren von Anhalten/Fortsetzen](optimize-suspend-resume.md) | Erstellen Sie UWP-Apps Universelle Windows-Plattform), die die Verwendung des Prozesslebensdauer-Systems optimieren und nach dem Anhalten oder Beenden effizient fortgesetzt werden. |
| [Optimieren des Dateizugriffs](optimize-file-access.md) | Erstellen Sie UWP-Apps, die effizient auf das Dateisystem zugreifen und dadurch Leistungsprobleme aufgrund von Datenträgerlatenz und Arbeitsspeicher-/CPU-Zyklen vermeiden. |
| [Windows-Runtimekomponenten und Optimieren der Interoperabilität](windows-runtime-components-and-optimizing-interop.md) | Erstellen Sie UWP-Apps, die UWP-Komponenten verwenden, mit systemeigenen und verwalteten Typen zusammenarbeiten und gleichzeitig Probleme mit der Interoperabilitätsleistung vermeiden. |
| [Tools für Profilerstellung und Leistung](tools-for-profiling-and-performance.md) | Microsoft bietet verschiedene Tools zur Verbesserung der Leistung Ihrer UWP-App.|

