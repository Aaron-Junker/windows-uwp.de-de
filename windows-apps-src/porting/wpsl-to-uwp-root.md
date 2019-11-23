---
description: Wenn Sie ein Entwickler mit einer Windows Phone Silverlight-APP sind, können Sie Ihre Fähigkeiten und Ihren Quellcode bei der Umstellung auf Windows 10 optimal nutzen.
title: Wechseln von Windows Phone Silverlight zu UWP
ms.assetid: 9E0C0315-6097-488B-A3AF-7120CCED651A
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, UWP
ms.localizationpriority: medium
ms.openlocfilehash: d37e40d58d8825d4361efbd10108c1169b8df87f
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260090"
---
#  <a name="move-from-windowsphone-silverlight-to-uwp"></a>Wechseln von Windows Phone Silverlight zu UWP


Wenn Sie ein Entwickler mit einer Windows Phone Silverlight-APP sind, können Sie Ihre Fähigkeiten und Ihren Quellcode bei der Umstellung auf Windows 10 optimal nutzen. Mit Windows 10 können Sie eine APP für die universelle Windows-Plattform (UWP) erstellen, bei der es sich um ein einzelnes App-Paket handelt, das Ihre Kunden auf jeder Art von Gerät installieren können. Weitere Hintergrundinformationen zu Windows 10, UWP-apps und den Konzepten von adaptiver Code und adaptiver Benutzeroberfläche, die wir in diesem Leitfaden erwähnen werden, finden Sie im [Leitfaden zu universelle Windows-Plattform-Apps (UWP)](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide).

Wenn Sie Ihre Windows Phone Silverlight-App auf eine Windows 10-App portieren, können Sie die mobilen Features, die [in Windows Phone 8,1 eingeführt](https://docs.microsoft.com/previous-versions/windows/apps/ff402535(v=vs.105))wurden, auffangen und weit über Sie hinausgehen, um die universelle Windows-Plattform (UWP) zu verwenden, deren App-Modell und Benutzeroberflächen-Framework auf allen Windows 10-Geräten universell sind. Dies ermöglicht die Unterstützung von PCs, Tablets, Telefonen und einer großen Zahl von anderen Gerätearten – mit einer Codebasis und einem App-Paket. Zudem vervielfacht sich die potenzielle Zielgruppe Ihrer App, und Sie erhalten neue Möglichkeiten durch freigegebene Daten, gekaufte Consumables usw. Weitere Informationen zu neuen Features finden Sie unter [Neues für Entwickler in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest).

Wenn Sie sich für entscheiden, können die Windows Phone Silverlight-Version Ihrer APP und die Windows 10-Version von Kunden gleichzeitig zur Verfügung stehen.

**Hinweis**  dieses Handbuch soll Ihnen helfen, Ihre Windows Phone Silverlight-app manuell auf Windows 10 zu portieren. Zum Portieren Ihrer App können Sie neben den Informationen in diesem Leitfaden auch die Developer Preview der **Silverlight-Brücke von Mobilize.Net** ausprobieren, um den Portierungsprozess zu automatisieren. Dieses Tool analysiert den Quellcode Ihrer APP und konvertiert Verweise auf Windows Phone Silverlight-Steuerelemente und-APIs in Ihre UWP-Entsprechungen. Da dieses Tool noch die Developer Preview-Version aufweist, unterstützt es noch nicht alle Konvertierungsszenarien. Die meisten Entwickler sollten mit dem Einstieg in dieses Tool jedoch Zeit und Mühe sparen. Besuchen Sie die [Website von Mobilize.NET](https://www.mobilize.net/uwp-bridge), um die Developer Preview zu testen.

## <a name="xaml-and-net-or-html"></a>XAML und .NET oder HTML?

Windows Phone Silverlight verfügt über ein XAML-UI-Framework, das auf Silverlight 4,0 basiert, und Sie programmieren mit einer Version des .NET Framework und einer kleinen Teilmenge von UWP-APIs. Da Sie in Ihrer Windows Phone Silverlight-App Extensible Application Markup Language (XAML) verwendet haben, ist es wahrscheinlich, dass XAML Ihre Wahl für Ihre Windows 10-Version ist, da der Großteil ihres Wissens und ihrer Erfahrungen übertragen wird. die verwendeten Software Muster. Auch Ihr UI-Markup und -Design lassen sich mühelos portieren. Ihnen werden die verwalteten APIs, das XAML-Markup, das Benutzeroberflächenframework und die Tools angenehm vertraut vorkommen, und Sie können C++, C# oder Visual Basic zusammen mit XAML in einer UWP-App verwenden. Obwohl es die eine oder andere Herausforderung zu meistern gilt, werden Sie vielleicht überrascht sein, wie einfach der Prozess letztlich ist.

Siehe [Roadmap für Universal Windows Platform (UWP)-Apps mit C# oder Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10)).

**Beachten  Sie** , dass Windows 10 viel mehr .NET Framework als eine Windows Phone Store-App unterstützt. Windows 10 hat z. b. mehrere System. Service Model.\* Namespaces sowie System.net, System .net. NetworkInformation und System .net. Sockets. Nun ist es ein guter Zeitpunkt, Ihre Windows Phone Silverlight zu portieren und ihren .NET-Code einfach zu kompilieren und auf der neuen Plattform zu arbeiten. Weitere Informationen finden Sie unter [Namespace- und Klassenzuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md).
Ein weiterer guter Grund für die Neukompilierung Ihres vorhandenen .NET-Quellcodes in eine Windows 10-App besteht darin, dass Sie von .net Native profitieren, bei der es sich um eine vorzeitliche Kompilierungs Technologie handelt, die MSIL in nativ ausführbaren Computercode konvertiert. .NET Native-Apps starten schneller, verbrauchen weniger Arbeitsspeicher und benötigen weniger Akkuenergie als ihre MSIL-Gegenstücke.

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

Die gute Nachricht ist, dass auf der Featureebene nur sehr wenig nicht in der UWP unterstützt wird. Wenn Sie den Rest dieses Handbuchs für das Portieren lesen, werden Sie feststellen, dass sich die meisten Ihrer Kenntnisse und der Großteil Ihres Quellcodes sehr gut auf UWP-Apps übertragen lassen. Im folgenden finden Sie jedoch einige Windows Phone Silverlight-Features, die Sie möglicherweise verwendet haben, für die keine UWP-Entsprechung vorhanden ist.

| Feature, für das es kein UWP-Äquivalent gibt | Windows Phone Silverlight-Dokumentation für das Feature |
|----------------------------------------------|---------------------------------------------------------|
| Microsoft XNA. Im Allgemeinen wird als Ersatz [Microsoft DirectX](https://docs.microsoft.com/windows/desktop/directx) mit C++ verwendet. Siehe [Entwickeln von Spielen](https://docs.microsoft.com/previous-versions/windows/apps/hh452744(v=win.10)) und [Interoperabilität von DirectX und XAML](https://docs.microsoft.com/previous-versions/windows/apps/hh825871(v=win.10)). | [XNA-Framework-Klassenbibliothek](https://docs.microsoft.com/previous-versions/windows/xna/bb200104(v=xnagamestudio.41)) | 
|Foto-Apps | [Linsen für Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/jj206990(v=vs.105)) |

&nbsp;

| Thema| Beschreibung|
|------|------------| 
| [Namespace-und Klassen Zuordnungen](wpsl-to-uwp-namespace-and-class-mappings.md) | Dieses Thema bietet eine umfassende Zuordnung von Windows Phone Silverlight-APIs zu ihren UWP-Entsprechungen. |
| [Portieren des Projekts](wpsl-to-uwp-porting-to-a-uwp-project.md) | Sie beginnen den Portierungs Prozess, indem Sie ein neues Windows 10-Projekt in Visual Studio erstellen und die Dateien in das Projekt kopieren. |
| [Problembehandlung](wpsl-to-uwp-troubleshooting.md) | Wir empfehlen dringend, dieses Handbuch für das Portieren vollständig zu lesen. Wir wissen aber auch, dass Sie möglichst schnell die Phase erreichen möchten, in der Ihr Projekt erstellt und ausgeführt wird. Zu diesem Zweck können Sie einen temporären Fortschritt erzielen, indem Sie allen nicht unbedingt erforderlichen Code auskommentieren und anschließend zurückkehren, um die Schulden später zu bezahlen. Die Tabelle mit Symptomen und Möglichkeiten zur Problembehandlung in diesem Thema kann in dieser Phase hilfreich für Sie sein, ersetzt jedoch nicht das Lesen der nächsten Themen. Sie können die Tabelle jederzeit zu Rate ziehen, während Sie die weiteren Themen lesen. |
| [Portieren von XAML und UI](wpsl-to-uwp-porting-xaml-and-ui.md) | Die Vorgehensweise zum Definieren der Benutzeroberfläche in Form von deklarativem XAML-Markup übersetzt hervorragend von Windows Phone Silverlight zu UWP-apps. Sie werden feststellen, dass große Abschnitte Ihres Markups kompatibel sind, sobald Sie Verweise auf Systemressourcenschlüssel aktualisiert, einige Namen von Elementtypen geändert und „clr-namespace“ in „using“ geändert haben. |
| [Portieren für e/a, Gerät und App-Modell](wpsl-to-uwp-input-and-sensors.md) | Code, der in das Gerät selbst integriert und auf dessen Sensoren abgestimmt ist, umfasst auch Eingaben vom und Ausgaben an den Benutzer. Auch die Datenverarbeitung kann einbezogen werden. Aber dieser Code wird in der Regel nicht als UI-Ebene oder Datenebene betrachtet. Dieser Code enthält die Integration in Vibrationscontroller, Beschleunigungsmesser, Gyroskop, Mikrofon und Lautsprecher (überschneiden sich mit Spracherkennung und Sprachsynthese), (geografischen) Standort und Eingabemodalitäten, z. B. Touch, Maus, Tastatur und Stift. |
| [Portieren von Geschäfts-und Datenschichten](wpsl-to-uwp-business-and-data.md) | Im Hintergrund Ihrer Benutzeroberfläche befinden sich Ihre Geschäfts- und Datenebenen. Der Code auf diesen Ebenen ruft Betriebssystem- und .NET Framework-APIs auf (z. B. Hintergrundverarbeitung, Position, Kamera, Dateisystem, Netzwerk und andere Datenzugriffsfunktionen). Die meisten dieser APIs sind [für UWP-Apps verfügbar](https://docs.microsoft.com/previous-versions/windows/br211369(v=win.10)). Sie können daher davon ausgehen, dass sich ein Großteil dieses Codes ohne Änderungen portieren lässt. |
| [Portieren für den Formular Faktor und die UX](wpsl-to-uwp-form-factors-and-ux.md) | Windows-Apps weisen auf PCs, Mobilgeräten und vielen anderen Arten von Geräten ein einheitliches Erscheinungsbild auf. Die Benutzeroberfläche, Eingabe und Interaktionsmuster sind fast identisch, und Benutzer profitieren von einer einheitlichen Umgebung auf allen Geräten.|
|[Fallstudie: Bookstore1](wpsl-to-uwp-case-study-bookstore1.md) | In diesem Thema wird beschrieben, wie Sie eine sehr einfache Windows Phone Silverlight-app in eine Windows 10-UWP-App portieren. Mit Windows 10 können Sie ein einzelnes App-Paket erstellen, das Ihre Kunden auf einer großen Bandbreite von Geräten installieren können, und das ist das, was wir in dieser Fallstudie tun werden. |
| [Fallstudie: Bookstore2](wpsl-to-uwp-case-study-bookstore2.md) | Diese Fallstudie –, die auf den in [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md)angegebenen Informationen aufbaut, beginnt mit einer Windows Phone Silverlight-APP, die gruppierte Daten in einem **longlistselector**anzeigt. Im Ansichtsmodell stellt jede Instanz der **Author**-Klasse die Gruppe der vom betreffenden Autor verfassten Titel dar. In **LongList Selector** können wir dann entweder die Bücherliste nach Autoren gruppiert anzeigen oder die Liste verkleinern, um eine Sprungliste der Autoren zu erhalten. |

## <a name="related-topics"></a>Verwandte Themen

**Dokumentation**
* [Neues für Entwickler in Windows 10](https://docs.microsoft.com/windows/uwp/whats-new/windows-10-version-latest)
* [Anleitung für UWP (Universelle Windows-Plattform)-Apps](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
* [Roadmap für universelle Windows-Plattform-Apps (UWP) C# mit oder Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br229583(v=win.10))
* [Nächste Schritte für Windows Phone 8-Entwickler](https://docs.microsoft.com/previous-versions/windows/apps/dn655121(v=vs.105))

**Magazine-Artikel**
* [Visual Studio Magazine: Windows Phone 8.1: Ein Riesenschritt für die Konvergenz](https://visualstudiomagazine.com/articles/2014/05/01/whats-new-for-developers-with-windows-phone-8_1.aspx)

**Präsentationen**
* [Der Artikel zum Einbinden von Nokia Music von Windows Phone zu Windows 8](https://channel9.msdn.com/Events/Build/2013/2-219)
 

