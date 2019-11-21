---
description: If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10.
title: Move from Windows Phone Silverlight to UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d37e40d58d8825d4361efbd10108c1169b8df87f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260090"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Move from Windows Phone Silverlight to UWP


If you’re a developer with a Windows Phone Silverlight app, then you can make great use of your skill set and your source code in the move to Windows 10. With Windows 10, you can create a Universal Windows Platform (UWP) app, which is a single app package that your customers can install onto every kind of device. For more background on Windows 10, UWP apps, and the concepts of adaptive code and adaptive UI that we'll mention in this porting guide, see the [Guide to Universal Windows Platform (UWP) apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

When you port your Windows Phone Silverlight app to a Windows 10 app, you'll be able to catch up on the mobile features that were [introduced in Windows Phone 8.1](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105)), and go far beyond them to use the Universal Windows Platform (UWP) whose app model and UI framework are universal across all Windows 10 devices. Dies ermöglicht die Unterstützung von PCs, Tablets, Telefonen und einer großen Zahl von anderen Gerätearten – mit einer Codebasis und einem App-Paket. Zudem vervielfacht sich die potenzielle Zielgruppe Ihrer App, und Sie erhalten neue Möglichkeiten durch freigegebene Daten, gekaufte Consumables usw. For more info on new features, see [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest).

If you choose to, the Windows Phone Silverlight version of your app and the Windows 10 version of it can both be available to customers at the same time.

**Note**  This guide is designed to help you port your Windows Phone Silverlight app to Windows 10 manually. Zum Portieren Ihrer App können Sie neben den Informationen in diesem Leitfaden auch die Developer Preview der **Silverlight-Brücke von Mobilize.Net** ausprobieren, um den Portierungsprozess zu automatisieren. This tool analyzes your app's source code and converts references to Windows Phone Silverlight controls and APIs to their UWP counterparts. Da dieses Tool noch die Developer Preview-Version aufweist, unterstützt es noch nicht alle Konvertierungsszenarien. Die meisten Entwickler sollten mit dem Einstieg in dieses Tool jedoch Zeit und Mühe sparen. Besuchen Sie die [Website von Mobilize.NET](https://www.mobilize.net/uwp-bridge), um die Developer Preview zu testen.

## <a name="xaml-and-net-or-html"></a>XAML und .NET oder HTML?

Windows Phone Silverlight has a XAML UI framework based on Silverlight 4.0, and you program against a version of the .NET Framework and a small subset of UWP APIs. Since you used Extensible Application Markup Language (XAML) in your Windows Phone Silverlight app, it's likely that XAML will be your choice for your Windows 10 version because most of your knowledge and experience will transfer, as will much of your source code and the software patterns you use. Auch Ihr UI-Markup und -Design lassen sich mühelos portieren. Ihnen werden die verwalteten APIs, das XAML-Markup, das Benutzeroberflächenframework und die Tools angenehm vertraut vorkommen, und Sie können C++, C# oder Visual Basic zusammen mit XAML in einer UWP-App verwenden. Obwohl es die eine oder andere Herausforderung zu meistern gilt, werden Sie vielleicht überrascht sein, wie einfach der Prozess letztlich ist.

Siehe [Roadmap für Universal Windows Platform (UWP)-Apps mit C# oder Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)).

**Note**  Windows 10 supports much more of the .NET Framework than a Windows Phone Store app does. For example, Windows 10 has several System.ServiceModel.\* namespaces as well as System.Net, System.Net.NetworkInformation, and System.Net.Sockets. So, now is a great time to port your Windows Phone Silverlight and have your .NET code just compile and work on the new platform. Weitere Informationen finden Sie unter [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md).
Another great reason to recompile your existing .NET source code into a Windows 10 app is that you will benefit from .NET Native, which an ahead-of-time compilation technology that converts MSIL into natively-runnable machine code. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

In diesem Portierungshandbuch geht es vorrangig um XAML. Alternativ können Sie mit JavaScript, Cascading Stylesheets (CSS) und HTML5 sowie der Windows-Bibliothek für JavaScript eine funktional vergleichbare App erstellen – viele der verwendeten UWP-APIs sind in diesem Fall identisch. Obwohl sich die Windows-Runtime-Benutzeroberflächenframeworks von XAML und HTML unterscheiden, funktionieren beide für sämtliche Windows-Geräte.

## <a name="targeting-the-universal-or-the-mobile-device-family"></a>Universelle Geräte oder Mobilgeräte

Eine Option ist das Portieren Ihrer App zu einer App für universelle Geräte. In diesem Fall besteht für die Installation der App der größte Spielraum. Wenn Ihre App die APIs aufruft, die nur in Mobilgeräten implementiert sind, können Sie diese Aufrufe mit adaptivem Code schützen. Alternativ dazu können Sie Ihre App auch zu einer App für Mobilgeräte portieren. In diesem Fall müssen Sie keinen adaptiven Code schreiben.

## <a name="adapting-your-app-to-multiple-form-factors"></a>Anpassen der App an mehrere Formfaktoren

Die im vorherigen Abschnitt gewählte Option bestimmt die Palette an Geräten, auf denen Ihre App oder Apps ausgeführt werden, und dabei kann es sich um eine sehr breite Palette von Geräten handeln. Selbst wenn Sie Ihre App auf Mobilgeräte beschränken, müssen Sie dennoch ein breites Spektrum an Bildschirmgrößen unterstützen. Wenn Ihre App also auf verschiedenen Formfaktoren ausgeführt wird, die vorher nicht unterstützt wurden, sollten Sie Ihre Benutzeroberfläche auf diesen Formfaktoren testen und ggf. Änderungen vornehmen, damit sich die UI auf jedem Gerät entsprechend anpasst. Stellen Sie sich dies als eine Aufgabe nach der Portierung oder eine erweiterte Zielvorgabe für das Portieren vor. Die Fallstudie [Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) enthält hierzu ein Praxisbeispiel.

## <a name="approaching-porting-layer-by-layer"></a>Portieren der einzelnen Ebenen

-   **Ansicht**. Die Ansicht und das Ansichtsmodell bilden die UI Ihrer App. Im Idealfall besteht die Ansicht aus Markup, das an feststellbare Eigenschaften eines Ansichtsmodells gebunden ist. Ein weiteres gängiges, aber nur auf kurze Sicht zweckmäßiges Muster ist das direkte Ändern von UI-Elementen mit imperativem Code in einer CodeBehind-Datei. In beiden Fällen kann ein Großteil des UI-Markups und -Designs – und sogar imperativer Code zum Ändern von UI-Elementen – einfach portiert werden.
-   **Ansichtsmodelle und Datenmodelle**. Auch wenn Sie Muster zur Trennung der Zuständigkeiten (z. B. MVVM) nicht ausdrücklich anwenden, gibt es in Ihrer App zwangsläufig Code, der die Funktion des Ansichts- und Datenmodells übernimmt. Code für das Ansichtsmodell nutzt Typen in den Namespaces des Benutzeroberflächenframeworks. Sowohl der Code für das Ansichtsmodell als auch der Code für das Datenmodell nutzen zudem nicht visuelle Betriebssystem- und .NET-APIs (darunter APIs für den Datenzugriff). Und die meisten dieser APIs sind [für UWP-Apps verfügbar](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)). Sie können daher davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt. Bedenken Sie aber, dass ein Ansichtsmodell ein Modell oder eine *Abstraktion* einer Ansicht ist. Ein Ansichtsmodell stellt den Zustand und das Verhalten der UI bereit, während die Ansicht selbst die visuellen Elemente bereitstellt. Aus diesem Grund sind wahrscheinlich für jede UI, die Sie an die verschiedenen, von der UWP unterstützten Formfaktoren anpassen, entsprechende Änderungen des Ansichtsmodells erforderlich. Für Netzwerke und das Aufrufen von Clouddiensten können Sie normalerweise zwischen .NET und UWP-APIs wählen. Informationen zu den Faktoren, die diese Entscheidung beeinflussen, finden Sie unter [Clouddienste, Netzwerk und Datenbanken](wpsl-to-uwp-business-and-data.md).
-   **Clouddienste**. Höchstwahrscheinlich werden einige (oder sogar die meisten) Teile Ihrer App in Form von Diensten in der Cloud ausgeführt. Der auf dem Clientgerät ausgeführte Teil der App stellt eine Verbindung mit diesen Diensten her. Dies ist der Teil einer verteilten App, bei dem es am wahrscheinlichsten ist, dass er beim Portieren des Clientteils unverändert bleibt. Falls Sie noch keine Clouddienste nutzen, sind [Microsoft Azure Mobile Services](https://azure.microsoft.com/services/mobile-services/) eine gute Wahl für Ihre UWP-App. Sie bieten leistungsstarke Back-End-Komponenten, die universelle Windows-Apps für verschiedenste Dienste aufrufen können – angefangen bei einfachen Benachrichtigungen für Live-Kachelaktualisierungen bis hin zur komplexen Skalierbarkeit, die eine Serverfarm bereitstellen kann.

Überlegen Sie vor oder während der Portierung, ob Ihre App dadurch verbessert werden könnte, wenn Sie sie umgestalten und Code mit einem ähnlichen Zweck in Ebenen zusammenfassen, anstatt ihn willkürlich zu verteilen. Die Aufteilung Ihrer UWP-App in die oben beschriebenen Ebenen erleichtert Ihnen das Ausschließen von Fehlern, das Testen sowie das spätere Lesen und Warten Ihrer App. Sie können die Wiederverwendbarkeit von Funktionen verbessern – und Probleme mit Unterschieden in den UI-APIs vermeiden –, indem Sie das Model-View-ViewModel-Muster ([MVVM](https://msdn.microsoft.com/magazine/dd419663.aspx)) anwenden. Dieses Muster trennt die Daten-, Geschäfts- und UI-Teile der App voneinander. Auch innerhalb der UI kann das Muster Zustand und Verhalten von den visuellen Elementen getrennt halten, sodass diese separat getestet werden können. Das MVVM-Muster bietet Ihnen die Möglichkeit, Ihre Daten und Geschäftslogik einmal zu schreiben und auf allen Geräten, unabhängig von der UI, zu verwenden. Es ist wahrscheinlich, dass Sie auch einen Großteil des Ansichtsmodells und der Ansichtselemente auf verschiedenen Geräten wiederverwenden können.

## <a name="one-or-two-exceptions-to-the-rule"></a>Ein oder zwei Ausnahmen von der Regel

Beim Lesen dieses Portierungshandbuchs können Sie auch die Informationen unter [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md) nutzen. Im Allgemeinen wird eine einfache Zuordnung verwendet, und in der Tabelle mit den Namespace- und Klassenzuordnungen werden etwaige Ausnahmen beschrieben.

Die gute Nachricht ist, dass auf der Featureebene nur sehr wenig nicht in der UWP unterstützt wird. Wenn Sie den Rest dieses Handbuchs für das Portieren lesen, werden Sie feststellen, dass sich die meisten Ihrer Kenntnisse und der Großteil Ihres Quellcodes sehr gut auf UWP-Apps übertragen lassen. But, here are the few Windows Phone Silverlight features that you may have used for which there is no UWP equivalent.

| Feature, für das es kein UWP-Äquivalent gibt | Windows Phone Silverlight documentation for the feature |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Im Allgemeinen wird als Ersatz [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) mit C++ verwendet. Siehe [Entwickeln von Spielen](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) und [Interoperabilität von DirectX und XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). | [XNA Framework Class Library](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|Foto-Apps | [Lenses for Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| Thema| Beschreibung|
|------|------------| 
| [Namespace and class mappings](wpsl-to-uwp-namespace-and-class-mappings.md) | This topic provides a comprehensive mapping of Windows Phone Silverlight APIs to their UWP equivalents. |
| [Porting the project](wpsl-to-uwp-porting-to-a-uwp-project.md) | You begin the porting process by creating a new Windows 10 project in Visual Studio and copying your files into it. |
| [Problembehandlung](wpsl-to-uwp-troubleshooting.md) | Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen. |
| [Porting XAML and UI](wpsl-to-uwp-porting-xaml-and-ui.md) | The practice of defining UI in the form of declarative XAML markup translates extremely well from Windows Phone Silverlight to UWP apps. Sie werden feststellen, dass große Abschnitte Ihres Markups kompatibel sind, sobald Sie Verweise auf Systemressourcenschlüssel aktualisiert, einige Namen von Elementtypen geändert und „clr-namespace“ in „using“ geändert haben. |
| [Porting for I/O, device, and app model](wpsl-to-uwp-input-and-sensors.md) | Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (überschneiden sich mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Touch, Maus, Tastatur und Stift. |
| [Porting business and data layers](wpsl-to-uwp-business-and-data.md) | Im Hintergrund Ihrer UI befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für UWP-Apps verfügbar](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)). Sie können daher davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt. |
| [Porting for form factor and UX](wpsl-to-uwp-form-factors-and-ux.md) | Windows-Apps weisen auf PCs, Mobilgeräten und vielen anderen Arten von Geräten ein einheitliches Erscheinungsbild auf. Die Benutzeroberfläche, Eingabe und Interaktionsmuster sind fast identisch, und Benutzer profitieren von einer einheitlichen Umgebung auf allen Geräten.|
|[Case study: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | This topic presents a case study of porting a very simple Windows Phone Silverlight app to a Windows 10 UWP app. With Windows 10, you can create a single app package that your customers can install onto a wide range of devices, and that's what we'll do in this case study. |
| [Case study: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | This case study—which builds on the info given in [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)—begins with a Windows Phone Silverlight app that displays grouped data in a **LongListSelector**. Im Ansichtsmodell stellt jede Instanz der **Author**-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In **LongList Selector** können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. |

## <a name="related-topics"></a>Verwandte Themen

**Dokumentation**
* [What's new for developers in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [Anleitung für UWP (Universelle Windows-Plattform)-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Roadmap for Universal Windows Platform (UWP) apps using C# or Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [Nächste Schritte für Windows Phone 8-Entwickler](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**Magazine-Artikel**
* [Visual Studio Magazine: Windows Phone 8.1: Ein Riesenschritt für die Konvergenz](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**Präsentationen**
* [The Story of Bringing Nokia Music from Windows Phone to Windows 8](https://channel9.msdn.com/Events/Build/2013/2-219)
 

