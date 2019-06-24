---
title: Nutzen von Clouddiensten für UWP-Spiele
description: Erfahren Sie mehr über die Implementierung der Cloud als Back-End für Ihre UWP-Spiele.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, Spiele, Cloud-Dienste
ms.localizationpriority: medium
ms.openlocfilehash: b1b63c6eb3c803b26ead85332affe1fc4f20e314
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321334"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Nutzen von Clouddiensten für UWP-Spiele

Die Universelle Windows-Plattform (UWP) in Windows 10 bietet eine Reihe von APIs, die zur Entwicklung von Spielen für Microsoft-Geräte verwendet werden können. Bei der Entwicklung von Spielen für alle Plattformen und Geräte können Sie ein Cloud-Back-End nutzen, um das Spiel je nach Bedarf zu skalieren.

Wenn Sie eine vollständige Cloud-Back-End-Lösung für Ihr Spiel suchen, finden Sie weitere Informationen unter [Software-as-a Service für Game-Back-Ends](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>Was ist Cloud Computing?

Beim Cloud Computing werden über das Internet IT-Ressourcen und Anwendungen zum Speichern und Verarbeiten von Daten eingesetzt. Der Begriff _Cloud_ beschreibt die große Menge an nicht-lokalen Ressourcen, auf die Sie von überall aus zugreifen können.
Das Cloud Computing ermöglicht eine neue Art der Ressourcen- und Softwarenutzung. Sie müssen nicht mehr vorab für das komplette Produkt oder die Ressourcen zahlen, sondern können Plattformen, Software und Ressourcen stattdessen als Dienst nutzen. Cloudanbieter rechnen häufig nutzungsbasiert oder über Abonnements für Dienste ab.

##  <a name="why-use-cloud-services"></a>Vorteile von Clouddiensten

Ein Vorteil beim Einsatz von Clouddiensten für Spiele ist, dass Sie nicht vorab in eigene physische Server investieren müssen. Sie zahlen stattdessen erst später für die Nutzung oder die abonnierten Dienste. So senken Sie Ihre finanziellen Risiken bei der Entwicklung eines neuen Spieles. 

Ein weiterer Vorteil ist, dass Ihr Spiel auf umfangreiche Cloudressourcen bauen kann und so skalierbar ist (Sie können unerwartete Spitzen bei der Spielermenge, bei rechenintensiven Echtzeit-Berechnungen und bei der Datenverarbeitung abfangen). So sorgen Sie dafür, dass die Leistung Ihres Spiels rund um die Uhr stabil bleibt. Darüber hinaus können Cloudressourcen von jedem Gerät auf jeder Plattform weltweit genutzt werden. Sie können Ihr Spiel also global vermarkten.

Für Ihre Spieler ist ein überzeugendes Spielerlebnis das Wichtigste. Da die Game-Server in der Cloud von clientseitigen Updates unabhängig sind, erhalten Sie mehr Kontrolle und sorgen für eine sicherere Umgebung.   Indem Sie Ihre Game-Logik nicht auf dem Client, sondern serverseitig in der Cloud ausführen, können Sie für ein konsistentes Gameplay sorgen. Sie können Verbindungen zwischen Diensten konfigurieren und so für ein besser integriertes Spielerlebnis sorgen – beispielsweise, indem Sie In-Game-Käufe mit verschiedenen Zahlungsanbietern verknüpfen, mehrere Gaming-Netzwerke verbinden und In-Game-Updates über soziale Netzwerke wie Facebook und Twitter bereitstellen. 

Sie können dedizierte Cloudserver nutzen und so große und persistente Spielwelten erstellen, eine Gamer-Community schaffen, Daten von den Spielern sammeln und analysieren und so im Laufe der Zeit das Gameplay verbessern und das Monetisierungsmodell des Spiels optimieren.

Auch Spiele, die umfangreiche Funktionalitäten zur Datenverwaltung benötigen (beispielsweise mit Funktionen für Spiele in sozialen Netzwerken, die mit asynchronen Multiplayer-Mechanismen arbeiten), können über Clouddienste implementiert werden.

##  <a name="how-game-companies-use-the-cloud-technology"></a>Nutzung der Cloudtechnologie durch Entwickler

Hier erfahren Sie, wie andere Entwickler Cloudlösungen in ihre Spiele implementiert haben.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>Entwickler</th>
        <th>Beschreibung</th>
        <th>Kerndaten des Spiels</th>
        <th>Mehr erfahren</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent Games</a></td>
        <td><b>Tencent-Spiele</b> hat eine innovative Lösung mit Azure Service Fabric entwickelt, das es herkömmlichen PC-Spielen ermöglicht, als Dienst bereitgestellt zu werden. Deren Cloud-Spielelösung verwendet ein "Thin Client + Rich Cloud"-Modell, das Arbeitslasten im Back-End als Microservices verwendet.</td>
        <td>
            <ul>
                <li>Herkömmlichen PC-Spiele werden als Cloud-Spiele für Benutzer auf der ganzen Welt bereitgestellt. <li>Optimierte Spiel-Übermittlungsprozesse <li>Spielfunktionen, die als Microservices isoliert sind reduzieren die Komplexität, die wiederholten Arbeitslasten aufgrund von Abhängigkeiten und ermöglichen das unabhängige Aktualisieren neuer Feature. <li>Kleine Installationspaket-Downloads für die Wiedergabe der neuesten Spielinhalte (verminderte Größe des Pakets von GB auf MB) <li>Geringere Wartungskosten </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games und Microsoft erstellt, die cloudbasierte spielelösung</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Erstellen von Spielen mit Service Fabric: Informationen zu Tencents-Implementierung (Video)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 industries</a></td>
        <td><b>Halo 5: Überwachungen</b> implementiert <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan als Unternehmen</a> als seine social Gaming-Plattform mithilfe von Azure Cosmos DB (über die DocumentDB-API), die für die Geschwindigkeit und Flexibilität, aufgrund dessen automatische Indizierung Funktionen ausgewählt wurde.</td>
        <td>
            <ul>
                <li>Skalierbare Datenebene zur Verarbeitung der Erstellung und Verwaltung von Gruppen beim Multiplayer-Gameplay <li>Integration des Spiels mit sozialen Medien <li>Echtzeit-Datenabfragen über mehrere Attribute <li>Synchronisierung von Erfolgen und Statistiken im Spiel </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Social Gaming implementiert, mithilfe von Azure Cosmos DB (über die DocumentDB-API)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games entwickelte mit <b>Age of Ascent</b> ein herausragendes 3D-MMO (Massively Multiplayer Online), das auf Geräten mit modernen Browsern gespielt werden kann. Das Spiel kann also auf PCs, Laptops, Smartphones und anderen mobilen Geräten ohne Plug-Ins genutzt werden. Das Spiel verwendet ASP.NET Core, HTML5, WebGL und Azure.</td>
        <td>
            <ul>
                <li>Plattformübergreifendes, browserbasiertes Spiel <li>Eine einzige große und persistente offenen Welt <li>Durchführung von intensiven Gameplay-Berechnungen in Echtzeit <li>Skalierung mit der Anzahl der Spieler </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Erstellen von Spielen mit Service Fabric: Alter der Versalhöhe MMO-Spiel (Video)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Verwalten von spielkomponenten als Microservices mithilfe von Azure Service Fabric (Video)</a> 
                <li><a href="/Blogs/Azure/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Interview mit Age of Ascent-Entwickler (Video)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.nextgames.com/">Next Games</a></td>
        <td>Next Games, ist der Ersteller des <b>The Walking Dead: No Man's Land</b> Videospiel basierend auf Versions ursprünglichen Reihe. Das Spiel nutzt Azure als Back-End. Sie 1.000.000 Downloads in der ersten Woche und innerhalb der ersten Woche aufgetreten ist, wurde das Spiel, #1-iPhone und iPad-Free-App in den USA App Store, #1 kostenlose App im 12-Ländern und #1 kostenlosen Spiel in 13 Ländern.
        </td>
        <td>
            <ul>
                <li>Plattformübergreifend <li>Rundenbasiertes Multiplayer-Spiel <li>Flexible Leistungsskalierung <li>Spielerschutz vor Betrug <li>Dynamische Inhaltsübermittlung </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Wie es erstellt wurde: Nächste Spiele globale Onlinespiele-Plattform in Azure (mit Video-Blog)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Azure Cosmos DB (über die DocumentDB-API) Walking Dead verwendet werden, für die schnellere Entwicklungszyklus und attraktivere Gaming</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>Pixel Squad entwickelte mit der Unity-Engine und Azure das Spiel <b>Crime Coast</b>. <b>Crime Coast</b> ist ein Social-Strategiespiel für Android, iOS und die Windows-Plattformen. Für das Spiel wurden Azure Blob Speicher, Managed Azure Redis Cache und mehrere IIS-VMs mit Lastenausgleich sowie der Microsoft Notification-Hub eingesetzt. Erfahren Sie, wie die Skalierung und Handhabung der Spieler bei 5.000 gleichzeitigen Spielern möglich ist.
        </td>
        <td>
            <ul>
                <li>Plattformübergreifend <li>Multiplayer-Onlinespiel <li>Skalierung mit der Anzahl der Spieler </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Verwendung von Azure Cloud Services in Crime Coast MMO-Spiel</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Weitere Links

* [Hitman und Azure: Erstellen Sie Spiele Features wie schwer zu findenden Ziel, die nur mit der Cloud möglich sind.](https://channel9.msdn.com/Series/Hitman)
* [Azure als die geheimzutat für Hitcents, Spiel Troopers und InnoSpark](https://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Spiele-Start-UPS für Bizspark-Programm mithilfe von Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Design des Cloud-Back-Ends

Schon während die Spieldesigner und Produzenten über die erforderlichen Features und Funktionalitäten eines neuen Spieles sprechen, sollten Sie das Design der Spieleinfrastruktur berücksichtigen. Wenn Sie Spiele für unterschiedliche Geräte und die wichtigsten Plattformen entwickeln möchten, können Sie Azure als Back-End für das Spiel nutzen.

### <a name="understanding-iaas-paas-or-saas"></a>Grundlegendes zu IaaS, PaaS und SaaS

Zunächst müssen Sie entscheiden, welche Dienstform für Spiel am besten geeignet ist. Um einen Ansatz zur Erstellung Ihres Back-Ends auswählen zu können, sollten Sie die Unterschiede der drei Dienstformen kennen.

* [Infrastructure-as-a-Service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    IaaS stellt eine direkt verfügbare Computing-Infrastruktur dar, die über das Internet bereitgestellt und verwaltet wird. Stellen Sie sich vor, welche Möglichkeiten sich Ihnen eröffnen, wenn Sie mithilfe vieler Computer je nach Bedarf nach oben und unten skalieren können. Mit IaaS vermeiden Sie die Kosten und die Komplexität der Anschaffung und Verwaltung Ihrer eigenen Server und anderer Elemente einer Datacenter-Infrastruktur.

* [Platform-as-a-Service (PaaS)](https://azure.microsoft.com/overview/what-is-paas/)

    Ein PaaS-Dienst ist ähnlich wie ein IaaS-Dienst. Er umfasst jedoch auch die Verwaltung der Infrastruktur (z. B. Server, Storage und Netzwerk). Sie müssen also nicht nur keine physischen Server und die Datacenter-Infrastruktur anschaffen, sondern auch keine Softwarelizenzen kaufen und verwalten. Gleiches gilt für die zugrunde liegende Anwendungsstruktur, die Middleware, die Entwicklungstools und andere Ressourcen.

* [Software-as-a-Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    Software-as-a-Service (SaaS) ermöglicht Benutzern, sich mit cloudbasierten Apps zu verbinden und diese über das Internet zu verwenden. Es bietet eine vollständige Softwarelösung an, die Sie auf Basis eines nutzungsbasierten Zahlungsmodells von einem Cloud-Dienstanbieter erwerben.  Allgemeine Beispiele sind E-Mail, Kalender und Office-Tools (z. B. Microsoft Office 365). Sie vermieten die Verwendung einer App für Ihre Organisation, und Ihre Benutzer können eine Verbindung über das Internet, in der Regel über einen Webbrowser, herstellen. Die gesamte zugrunde liegende Infrastruktur, Middleware, App-Software und App-Daten befinden sich im Dienstanbieter-Rechenzentrum. Der Dienstanbieter verwaltet die Hardware und Software, und garantieren mit dem entsprechenden Servicevertrag die Verfügbarkeit und die Sicherheit des Spiels und Ihrer Daten. SaaS ermöglicht Ihrer Organisation den schnellen Einstieg für eine App mit minimale vorherigen Kosten.


### <a name="design-your-game-infrastructure-using-azure"></a>Entwerfen der Spieleinfrastruktur mit Azure

In diesem Abschnitt lernen Sie Möglichkeiten zur Nutzung der Azure Cloudangebote für ein Spiel kennen. Azure arbeitet mit Windows, Linux und bekannten Open-Source-Technologien wie Ruby, Python, Java und PHP. Weitere Informationen finden Sie unter [Azure für Gaming](https://azure.microsoft.com/solutions/gaming/).

| Anforderungen                 | Aktivitätsszenarien                            | Produktangebot                      | Produktfunktionen                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hosten Ihrer Domäne in der Cloud     | Effizientes Beantworten von DNS-Abfragen            | [Azure DNS](https://azure.microsoft.com/services/dns/) | Hosten Ihrer Domäne mit hoher Leistung und Verfügbarkeit  |
| Anmeldung, Identitätsüberprüfung      | Spieler meldet sich an und seine Identität wird authentifiziert  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Einmaliges Anmelden mit mehrstufiger Authentifizierung für alle Cloud-Web-Apps und lokalen Web-Apps            | 
| Spiel nutzt IaaS-Modell      | Spiel wird auf virtuellen Computern in der Cloud gehostet       | [Azure-VMs](https://azure.microsoft.com/services/virtual-machines/) | Skalierung der Game-Server von einer VM-Instanz bis zu mehreren tausend inkl. Virtual-Networking und Lastenausgleich; hybride Konsistenz mit lokalen Systemen           |
| Web-Spiele oder mobile Spiele nutzen PaaS-Modell            | Spiel wird auf einer verwalteten Plattform gehostet                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | PaaS für Websites oder mobile Spiele (über Azure-VMs mit Middleware, Entwicklungstools, BI, DB-Management)   |
| Skalierbares mehrstufiges Cloud-Spiel mit hoher Verfügbarkeit und mit mehr Kontrolle über das Betriebssystem (PaaS)        | Spiel wird auf einer verwalteten Plattform gehostet                | [Azure-Clouddienst](https://azure.microsoft.com/services/app-service/) | PaaS wurde entwickelt, um Anwendungen zu unterstützen, die skalierbar, zuverlässig und günstig ausgeführt werden   |
| Regionaler Lastenausgleich für eine verbesserte Leistung und Verfügbarkeit | Leitet eingehende Spielanfragen weiter. Kann als ersten Ebene des Lastenausgleichs fungieren.       | [Azure Traffic Manager](https://azure.microsoft.com/en-us/services/traffic-manager/) | Bietet mehrere automatische Failover-Optionen und Möglichkeit, Ihren Datenverkehr gleichmäßig oder mit gewichteten Werten zu verteilen. Kann nahtlos lokale und Cloud-Systeme kombinieren. |
| Cloud-Speicher für Spieldaten       | Aktuelle Spieledaten werden in der Cloud gespeichert und an die Client-Geräte gesendet | [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)| Keine Einschränkung hinsichtlich der Art der zu speichernden Datei, Objektspeicher für große Mengen an unstrukturierten Daten wie Bilder, Audioinhalte, Video und mehr.  |
| Temporäre Tabellen zur Datenspeicherung| Spieletransaktionen (Änderungen von Spielzuständen) werden vorübergehend in Tabellen gespeichert | [Azure-Tabellenspeicher](https://azure.microsoft.com/services/storage/tables/)| Spieledaten können in einem flexiblen Schema gemäß den Anforderungen des Spiels gespeichert werden |
| Warteschlange für Spieltransaktionen/-anfragen| Spieletransaktionen werden über eine Warteschlange verarbeitet | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Warteschlangen fangen unerwartete Datenverkehrsspitzen auf und könnten verhindern, dass Server durch eine plötzliche Flut von Anfragen überlastet werden   |
| Skalierbare relationale Spieldatenbank| Strukturierte Speicherung von relationalen Daten wie In-Game-Transaktionen zur Datenbank | [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)| SQL-Datenbank als Dienst ([Vergleich mit SQL auf einem virtuellen Computer](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Skalierbare verteilte Spieldatenbank mit geringer Latenz| Schnelles Lesen, Schreiben und Abfragen von Spiel- und Spielerdaten mit Schemaflexibilität | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| NoSQL-Dokumentdatenbank als Dienst mit geringer Latenz   |
| Nutzung eines eigenen Datencenters mit Azure-Diensten | Spiel aus dem eigenen Datacenter abgerufen und an die Client-Geräte gesendet | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Ermöglicht Ihrer Organisation die Bereitstellung von Azure-Diensten über Ihr eigenes Datacenter  |
| Übertragung großer Datenmengen| Große Dateien wie Bilder, Audio und Videos können mit Azure CDN über den nächstgelegenen CDN-Pop-Standort (Content Delivery Network) an die Benutzer gesendet werden.    | [Azure Content Delivery Network](https://azure.microsoft.com/services/cdn/) | Azure CDN basiert auf einer modernen Netzwerktopologie mit großen, zentralen Knoten und verarbeitet auch plötzliche Verkehrsspitzen und hohe Lasten. So steigen die Geschwindigkeit und Verfügbarkeit deutlich, und das Benutzererlebnis wird verbessert  |
| Niedrige Latenz               | Nutzen Sie die Zwischenspeicherung, um schnelle und skalierbare Spiele mit mehr Kontrolle und einer garantierten Isolation der Daten zu erstellen. Verbessern Sie außerdem die Mach-Making-Funktion des Spiels. | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Datenzugriff mit hohem Durchsatz und einer konsistent geringen Latenz für schnelle und skalierbare Azure-Anwendungen  |
| Hohe Skalierbarkeit, geringe Latenz | Verarbeitung von Fluktuationen bei der Anzahl der Benutzer im Spiel mit geringer Latenz für Lese- und Schreibvorgänge | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Für die komplexesten und datenintensiven Szenarien mit geringer Latenz und zuverlässiger Skalierung für viele gleichzeitige Benutzer geeignet. Service Fabric ermöglicht Ihnen die Entwicklung von Spielen, ohne einen für statuslose Apps erforderlichen separaten Speicher oder Zwischenspeicher erstellen zu müssen |
| Sammeln von mehreren Millionen Ereignissen pro Sekunde von Geräten                         | Protokollieren von mehreren Millionen Ereignissen pro Sekunde von Geräten | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | Telemetrieaufnahme von Spielen, Websites, Apps und Geräten auf Cloud-Niveau  |
| Verarbeiten von Spieldaten in Echtzeit  | Durchführen von Echtzeitanalyse von Spielerdaten zur Verbesserung des Gameplays| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Echtzeit-Datenstromverarbeitung in der Cloud  |
| Entwickeln eines vorausschauenden Gameplays         | Erstellen eines angepassten, dynamischen Gameplays auf Basis der Spielerdaten  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Ein vollständig verwalteter Cloud-Dienst, mit dem Sie ganz einfach Predictive-Analytics-Lösungen erstellen, bereitstellen und freigeben können  |
| Sammeln und Analysieren von Spieldaten| Massive Parallelverarbeitung von Daten aus relationalen und nicht-relationalen Datenbanken | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Flexibles Data-Warehouse als Dienst mit Enterprise-Funktionen   |
| Wecken des Benutzerinteresses für eine erhöhte Verwendung und Aufbewahrung| Senden von benutzerorientierten Pushbenachrichtigung an Plattformen von jedem Back-End zur Generierung von Interesse und Förderung bestimmter Spielaktionen | [Azure Notification Hubs](https://azure.microsoft.com/services/notification-hubs/)| Mit dem schnellen Übertragungs-Push erreichen Sie Millionen Mobilgeräte auf allen wichtigen Plattformen &mdash;iOS, Android, Windows, Kindle, Baidu. Ihr Spiel kann auf jedem Back-End gehostet werden &mdash;Cloud oder lokal.|
| Medieninhalte für Ihr Publikum lokal und weltweit bei gleichzeitigem Schutz Ihrer Inhalte wiedergeben| Qualitativ hochwertige Trailer für Spiele und Kino-Clips können von allen Geräten wiedergegeben werden| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| Bedarfsgesteuertes und Live-Videostreaming mit integrierten Netzwerkfunktionen für die Inhaltsübermittlung. Verwendung eines Spielers für alle Wiedergabeanforderungen, einschließlich Inhaltsschutz und Verschlüsselung.| 
| Entwicklung, Verteilen und Beta-Tests für Ihre mobilen Apps | Testen und Verteilen Ihrer mobilen App. Verwalten von App-Leistung und Benutzererfahrung. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Integriert Absturzberichte und Benutzermetriken mit einer Plattform für die App-Verteilung und Benutzer-Feedback. Unterstützt Android, Cordova, iOS, OS X, Unity, Windows und Xamarin-Apps. Berücksichtigen Sie auch [Visual Studio Mobile Center](https://visualstudio.microsoft.com/app-center/) &mdash; einsatzleitung für apps, die kombiniert umfassende Analysen, Absturzberichte, push-Benachrichtigungen, app-Verteilung und vieles mehr. |
| Erstellen von Marketingkampagnen zur Steigerung und Erhaltung der Nutzungszahlen  | Senden von Push-Benachrichtigungen an Zielgruppen zur Generierung von Interesse und Förderung bestimmter Spielaktionen gemäß Datenanalysen | [Mobile-Engagement](https://azure.microsoft.com/services/mobile-engagement/) – wird im März 2018 eingestellt und steht derzeit nur für vorhandene Kunden zur Verfügung. |  Steigerung der Spielzeit und der Benutzerzahlen auf allen wichtigen Plattformen (iOS, Android, Windows, Windows Phone) |


##  <a name="startup-and-developer-resources"></a>Ressourcen für Startups und Entwickler

* [Microsoft für Startups](https://startups.microsoft.com)

    Microsoft für Startups bietet produktspezifische, technische und Markteinführungsvorteile, um das Wachstum der Startups zu beschleunigen. Ein Vorteil ist ein kostenlose Azure-Konto. Sie haben eine Gutschrift von 200 USD auf Dienste für 30 Tage, 12 Monate beliebte kostenlose Dienste und mehr als 25 kostenlose Dienste. Weitere Informationen finden Sie unter [Bring your startup’s ideas to life with an Azure free account](https://azure.microsoft.com/free/startups/).
    
* [Developer-Programmen](e2e.md#developer-programs)

    Microsoft bietet mehrere Entwicklerprogramme wie like [ID@Xbox](https://www.xbox.com/Developers/id) und [Xbox Live Creators-Programm](https://developer.microsoft.com/games/xbox/xboxlive/creator) an, die Sie bei der Entwicklung und Veröffentlichung von Windows-Spielen unterstützen.

## <a name="learning-resources"></a>Lernressourcen

* //build 2016: [CodeLabs &mdash; verwenden Microsoft Azure App Service und Microsoft SQL Azure-Back-End zum Speichern des Spielstands in Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* //build 2017: [Bereitstellung von erstklassigen Spiel Erfahrungen mit Microsoft Azure: Erfahrungen aus Titel wie Halo Hitman und Walking Dead (Video)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Wiederverwendbare Reihe von Bausteinen, Projekte, Diensten und bewährte Methoden entwickelt, um gängige Spiele-Workloads mithilfe von Azure auf GitHub unterstützt: [Bausteine für Gaming mit Azure](https://github.com/MicrosoftDX/nether)
* [Gamingdienste in Azure (Videos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Tools und andere nützliche Links

* [MSDN-Foren &mdash; Azure-Plattform](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Cloud-basierten Auslastungstests-tool](https://visualstudio.microsoft.com/team-services/cloud-load-testing/)
* [SDKs und Befehlszeilentools](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Software-as-a Service für Game-Back-Ends

[Playfab](https://playfab.com/) unterstützt derzeit 1.200 Live-Spiele mit 80 Millionen monatlich aktiven Spielern. Es handelt sich um eine vollständige Back-End-Plattform, die die gesamte Fertigungstiefe von LiveOps mit Echtzeit-Steuerelementen enthält. 

Sie können diese Lösung in Ihr Mobiltelefon, PC oder auf Konsolespielen mit SDKs integrieren. SDKs sind für alle beliebten Spielengines und Plattformen verfügbar, einschließlich Android, iOS, Unreal, Unity und Windows. Informationen zu ersten Schritten finden Sie in der [Dokumentation](https://api.playfab.com/).

Es bietet Spielediensten wie Authentifizierung, Spielerverwaltung, mehrere Spieler und in Echtzeit-Analyse, damit Ihr Spiel seine Benutzeranzahl vergrößert. Nutzen Sie die Leistung von Echtzeitdaten-Pipelines und LiveOps, um Ihre Benutzer mit benutzerdefinierten In-Game-Elementen, Ereignissen und Sonderangeboten zu fesseln. Sie haben auch die Möglichkeit, A/B-Tests durchzuführen, Berichte zu generieren, Pushbenachrichtigungen zu senden und vieles mehr. 

Wir bieten ständig Innovationen an und fügen neue Features hinzu. Weitere Informationen finden Sie unter [Features](https://playfab.com/features/) und Preise finden Sie unter [Simple pricing that scales with you](https://playfab.com/pricing/).

## <a name="related-links"></a>Verwandte Links

* [Handbuch zur Entwicklung von Spielen unter Windows 10](https://docs.microsoft.com/windows/uwp/gaming/e2e)
* [Azure für gaming](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft für Startups](https://startups.microsoft.com)
* [ID@Xbox](https://www.xbox.com/Developers/id)


 

 
