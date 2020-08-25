---
title: Nutzen von Clouddiensten für UWP-Spiele
description: Erfahren Sie mehr über die Implementierung der Cloud als Back-End für Ihre UWP-Spiele.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.date: 03/27/2018
ms.topic: article
keywords: Windows 10, UWP, Games, Cloud Services
ms.localizationpriority: medium
ms.openlocfilehash: 3d959c490474a6280d878be9679abafc31b565d8
ms.sourcegitcommit: 720413d2053c8d5c5b34d6873740be6e913a4857
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/25/2020
ms.locfileid: "88846860"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Nutzen von Clouddiensten für UWP-Spiele

Die Universelle Windows-Plattform (UWP) in Windows 10 bietet eine Reihe von APIs, die zur Entwicklung von Spielen für Microsoft-Geräte verwendet werden können. Bei der Entwicklung von Spielen für alle Plattformen und Geräte können Sie ein Cloud-Back-End nutzen, um das Spiel je nach Bedarf zu skalieren.

Wenn Sie nach einer umfassenden Cloud-Back-End-Lösung für Ihr Spiel suchen, lesen Sie [Software-as-a-Service für Game Backend](#software-as-a-service-for-game-backend).

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
        <th>Developer</th>
        <th>Beschreibung</th>
        <th>Kerndaten des Spiels</th>
        <th>Weitere Informationen</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent-Spiele</a></td>
        <td><b>Tencent Games</b> hat eine innovative Lösung mithilfe von Azure Service Fabric ermöglicht, herkömmliche PC-Spiele als Dienst bereitzustellen. Ihre cloudspiellösung verwendet ein "Thin Client + Rich Cloud"-Modell, das Arbeits Auslastungen als mikrodienste im Back-End durchführt.</td>
        <td>
            <ul>
                <li>Herkömmliche PCs werden als cloudspiele an Benutzer auf der ganzen Welt übermittelt. <li>Optimierter spielverarbeitungs Prozess <li>Spiele Funktionen, die als "mikroservices" isoliert sind, um die Komplexität zu reduzieren, Workloads aufgrund von Abhängigkeiten zu reduzieren und neue Features unabhängig zu aktualisieren <li>Download von kleinen Installationspaketen zum Abspielen neuester Spielinhalte (reduzierte Paketgröße von GB auf MB) <li>Geringere Wartungskosten </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">Tencent Games und Microsoft haben die cloudspiellösung erstellt</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Entwickeln von spielen mit Service Fabric: Details zur Implementierung von tencent (Video)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td><b>Halo 5: Wächter</b> haben <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan-Unternehmen</a> als soziale Spielplattform mithilfe von Azure Cosmos DB (über die documentdb-API) implementiert, die aufgrund ihrer automatischen Indizierungs Funktionen für die Geschwindigkeit und Flexibilität ausgewählt wurde.</td>
        <td>
            <ul>
                <li>Skalierbare Datenebene zur Verarbeitung der Erstellung und Verwaltung von Gruppen beim Multiplayer-Gameplay <li>Integration des Spiels mit sozialen Medien <li>Echtzeit-Datenabfragen über mehrere Attribute <li>Synchronisierung von Erfolgen und Statistiken im Spiel </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Mit Azure Cosmos DB implementiertes soziales Spiel (über die documentdb-API)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://web.ageofascent.com/">Illyriad Games</a></td>
        <td>Illyriad Games entwickelte mit <b>Age of Ascent</b> ein herausragendes 3D-MMO (Massively Multiplayer Online), das auf Geräten mit modernen Browsern gespielt werden kann. Dieses Spiel kann also auf PCs, Laptops, Mobiltelefonen und anderen mobilen Geräten ohne Plug-ins wiedergegeben werden. Das Spiel verwendet ASP.net Core, HTML5, WebGL und Azure.</td>
        <td>
            <ul>
                <li>Plattformübergreifendes, browserbasiertes Spiel <li>Eine einzige große und persistente offenen Welt <li>Durchführung von intensiven Gameplay-Berechnungen in Echtzeit <li>Skalierung mit der Anzahl der Spieler </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Entwickeln von spielen mit Service Fabric: Age of Ascent MMO Game (Video)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Verwalten von Spielkomponenten als Mikro Dienste mithilfe von Azure Service Fabric (Video)</a> 
                <li><a href="/Blogs/Azure/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Interview mit Alter of Ascent Developers (Video)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.nextgames.com/">Nächste Spiele</a></td>
        <td>Next Games ist der Entwickler von <b>The Walking Dead: No Man's Land</b>, das auf der Serie von AMC basiert. Das Spiel nutzt Azure als Back-End. Am Veröffentlichungswochenende gab es 1.000.000 Downloads, und in der ersten Woche schaffte es das Spiel in den USA auf Platz 1 der kostenlosen iPhone- und iPad-Apps, in 12 Ländern auf Platz 1 der kostenlosen Apps und in 13 Ländern auf Platz 1 der kostenlosen Spiele.
        </td>
        <td>
            <ul>
                <li>Plattformübergreifend <li>Rundenbasiertes Multiplayer-Spiel <li>Flexible Leistungsskalierung <li>Schutz vor gambetrugs <li>Dynamische Inhalts Übermittlung </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">So haben wir es erstellt: Next Games Global Online Gaming Platform in Azure (Blog mit Video)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">Beim Durchlaufen von Dead verwendet Azure Cosmos DB (über die documentdb-API), um den Entwicklungszeitraum zu beschleunigen</a>
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
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Verwendung von Azure Cloud Services durch das Crime Coast-Spiel</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Weitere Links

* [Hitman und Azure: Erstellen Sie Spiel Features wie ein schwer fassbares Ziel, die nur mit der Cloud möglich sind.](https://channel9.msdn.com/Series/Hitman)
* [Azure als Geheimwaffe für Hitcents, Game Troopers und InnoSpark](https://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Spiele-Startups im Bizspark-Programm nutzen Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Design des Cloud-Back-Ends

Schon während die Spieldesigner und Produzenten über die erforderlichen Features und Funktionalitäten eines neuen Spieles sprechen, sollten Sie das Design der Spieleinfrastruktur berücksichtigen. Wenn Sie Spiele für unterschiedliche Geräte und die wichtigsten Plattformen entwickeln möchten, können Sie Azure als Back-End für das Spiel nutzen.

### <a name="understanding-iaas-paas-or-saas"></a>Grundlegendes zu IaaS, PaaS und SaaS

Zunächst müssen Sie entscheiden, welche Dienstform für Spiel am besten geeignet ist. Um einen Ansatz zur Erstellung Ihres Back-Ends auswählen zu können, sollten Sie die Unterschiede der drei Dienstformen kennen.

* [Infrastructure-as-a-Service (IaaS)](https://azure.microsoft.com/overview/what-is-iaas/)

    IaaS stellt eine direkt verfügbare Computing-Infrastruktur dar, die über das Internet bereitgestellt und verwaltet wird. Stellen Sie sich vor, welche Möglichkeiten sich Ihnen eröffnen, wenn Sie mithilfe vieler Computer je nach Bedarf nach oben und unten skalieren können. Mit IaaS vermeiden Sie die Kosten und die Komplexität der Anschaffung und Verwaltung Ihrer eigenen Server und anderer Elemente einer Datacenter-Infrastruktur.

* [Platform-as-a-Service (PAS)](https://azure.microsoft.com/overview/what-is-paas/)

    Ein PaaS-Dienst ist ähnlich wie ein IaaS-Dienst. Er umfasst jedoch auch die Verwaltung der Infrastruktur (z. B. Server, Storage und Netzwerk). Sie müssen also nicht nur keine physischen Server und die Datacenter-Infrastruktur anschaffen, sondern auch keine Softwarelizenzen kaufen und verwalten. Gleiches gilt für die zugrunde liegende Anwendungsstruktur, die Middleware, die Entwicklungstools und andere Ressourcen.

* [Software-as-a-Service (SaaS)](https://azure.microsoft.com/overview/what-is-saas/)

    Mit Software-as-a-Service (SaaS) können Benutzer eine Verbindung mit cloudbasierten Apps über das Internet herstellen und diese Apps verwenden. Es bietet eine umfassende Softwarelösung, die Sie auf Basis eines clouddienstanbieters erwerben.  Gängige Beispiele hierfür sind e-Mail-, Kalender-und Office-Tools (z. b. Microsoft 365 Office-Apps). Sie vermieten die Verwendung einer APP für Ihre Organisation, und Ihre Benutzer stellen eine Verbindung über das Internet her, normalerweise mit einem Webbrowser. Alle zugrunde liegenden Komponenten wie Infrastruktur, Middleware, App-Software und App-Daten befinden sich im Rechenzentrum des Dienstanbieters. Der Dienstanbieter verwaltet die Hardware und Software und mit dem entsprechenden Dienstvertrag die Verfügbarkeit sowie die Sicherheit des Spiels und ihrer Daten. Mit SaaS kann Ihr Unternehmen eine App mit minimalen Investitionskosten schnell bereitstellen.


### <a name="design-your-game-infrastructure-using-azure"></a>Entwerfen der Spieleinfrastruktur mit Azure

In diesem Abschnitt lernen Sie Möglichkeiten zur Nutzung der Azure Cloudangebote für ein Spiel kennen. Azure arbeitet mit Windows, Linux und bekannten Open-Source-Technologien wie Ruby, Python, Java und PHP. Weitere Informationen finden Sie unter [Azure für Gaming](https://azure.microsoft.com/solutions/gaming/).

| Requirements (Anforderungen)                 | Aktivitätsszenarien                            | Produktangebot                      | Produktfunktionen                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hosten Ihrer Domäne in der Cloud     | Effizientes Beantworten von DNS-Abfragen            | [Azure DNS](https://azure.microsoft.com/services/dns/) | Hosten Ihrer Domäne mit hoher Leistung und Verfügbarkeit  |
| Anmeldung, Identitätsüberprüfung      | Spieler meldet sich an und seine Identität wird authentifiziert  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Einmaliges Anmelden mit mehrstufiger Authentifizierung für alle Cloud-Web-Apps und lokalen Web-Apps            | 
| Spiel nutzt IaaS-Modell      | Spiel wird auf virtuellen Computern in der Cloud gehostet       | [Virtuelle Azure-Computer](https://azure.microsoft.com/services/virtual-machines/) | Skalierung der Game-Server von einer VM-Instanz bis zu mehreren tausend inkl. Virtual-Networking und Lastenausgleich; hybride Konsistenz mit lokalen Systemen           |
| Web-Spiele oder mobile Spiele nutzen PaaS-Modell            | Spiel wird auf einer verwalteten Plattform gehostet                | [Azure App Service](https://azure.microsoft.com/services/app-service/) | PaaS für Websites oder mobile Spiele (über Azure-VMs mit Middleware, Entwicklungstools, BI, DB-Management)   |
| Hoch verfügbar, skalierbares, skalierbares n-Tier-cloudspiel mit mehr Kontrolle über das Betriebssystem (PAS)        | Spiel wird auf einer verwalteten Plattform gehostet                | [Azure-Clouddienst](https://azure.microsoft.com/services/app-service/) | Mit der Unterstützung von skalierbaren, zuverlässigen und kostengünstigen Anwendungen   |
| Lastenausgleich über Regionen hinweg zur Verbesserung der Leistung und Verfügbarkeit | Leitet eingehende Spiel Anforderungen weiter. Kann als erste Ebene des Lasten Ausgleichs fungieren.       | [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) | Bietet mehrere Optionen für automatisches Failover und die Möglichkeit, den Datenverkehr gleichmäßig oder mit gewichteten Werten zu verteilen. Lokale und cloudbasierte Systeme können nahtlos kombiniert werden. |
| Cloud-Speicher für Spieldaten       | Aktuelle Spieledaten werden in der Cloud gespeichert und an die Client-Geräte gesendet | [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)| Keine Einschränkung hinsichtlich der Art der zu speichernden Datei, Objektspeicher für große Mengen an unstrukturierten Daten wie Bilder, Audioinhalte, Video und mehr.  |
| Temporäre Tabellen zur Datenspeicherung| Spieletransaktionen (Änderungen von Spielzuständen) werden vorübergehend in Tabellen gespeichert | [Azure Table Storage](https://azure.microsoft.com/services/storage/tables/)| Spieledaten können in einem flexiblen Schema gemäß den Anforderungen des Spiels gespeichert werden |
| Warteschlange für Spieltransaktionen/-anfragen| Spieletransaktionen werden über eine Warteschlange verarbeitet | [Azure Queue Storage](https://azure.microsoft.com/services/storage/queues/)| Warteschlangen fangen unerwartete Datenverkehrsspitzen auf und könnten verhindern, dass Server durch eine plötzliche Flut von Anfragen überlastet werden   |
| Skalierbare relationale Spieldatenbank| Strukturierte Speicherung von relationalen Daten wie In-Game-Transaktionen zur Datenbank | [Azure SQL-Datenbank](https://azure.microsoft.com/services/sql-database/)| SQL-Datenbank als Dienst ([Vergleich mit SQL auf einem virtuellen Computer](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Skalierbare verteilte Spieldatenbank mit geringer Latenz| Schnelles Lesen, Schreiben und Abfragen von Spiel- und Spielerdaten mit Schemaflexibilität | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| NoSQL-Dokumentdatenbank als Dienst mit geringer Latenz   |
| Nutzung eines eigenen Datencenters mit Azure-Diensten | Spiel aus dem eigenen Datacenter abgerufen und an die Client-Geräte gesendet | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Ermöglicht Ihrer Organisation die Bereitstellung von Azure-Diensten über Ihr eigenes Datacenter  |
| Übertragung großer Datenmengen| Große Dateien wie Bilder, Audio und Videos können mit Azure CDN über den nächstgelegenen CDN-Pop-Standort (Content Delivery Network) an die Benutzer gesendet werden.    | [Übersicht über das Azure Content Delivery Network (CDN)](https://azure.microsoft.com/services/cdn/) | Azure CDN basiert auf einer modernen Netzwerktopologie mit großen, zentralen Knoten und verarbeitet auch plötzliche Verkehrsspitzen und hohe Lasten. So steigen die Geschwindigkeit und Verfügbarkeit deutlich, und das Benutzererlebnis wird verbessert  |
| Geringe Wartezeit               | Nutzen Sie die Zwischenspeicherung, um schnelle und skalierbare Spiele mit mehr Kontrolle und einer garantierten Isolation der Daten zu erstellen. Verbessern Sie außerdem die Mach-Making-Funktion des Spiels. | [Azure Redis Cache](https://azure.microsoft.com/services/cache/) | Datenzugriff mit hohem Durchsatz und einer konsistent geringen Latenz für schnelle und skalierbare Azure-Anwendungen  |
| Hohe Skalierbarkeit, geringe Latenz | Verarbeitung von Fluktuationen bei der Anzahl der Benutzer im Spiel mit geringer Latenz für Lese- und Schreibvorgänge | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Für die komplexesten und datenintensiven Szenarien mit geringer Latenz und zuverlässiger Skalierung für viele gleichzeitige Benutzer geeignet. Service Fabric ermöglicht Ihnen die Entwicklung von Spielen, ohne einen für statuslose Apps erforderlichen separaten Speicher oder Zwischenspeicher erstellen zu müssen |
| Sammeln von mehreren Millionen Ereignissen pro Sekunde von Geräten                         | Protokollieren von mehreren Millionen Ereignissen pro Sekunde von Geräten | [Azure Event Hubs](https://azure.microsoft.com/services/event-hubs/) | Telemetrieaufnahme von Spielen, Websites, Apps und Geräten auf Cloud-Niveau  |
| Verarbeiten von Spieldaten in Echtzeit  | Durchführen von Echtzeitanalyse von Spielerdaten zur Verbesserung des Gameplays| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Echtzeit-Datenstromverarbeitung in der Cloud  |
| Entwickeln eines vorausschauenden Gameplays         | Erstellen eines angepassten, dynamischen Gameplays auf Basis der Spielerdaten  | [Azure Machine Learning](https://azure.microsoft.com/services/machine-learning/) | Ein vollständig verwalteter Cloud-Dienst, mit dem Sie ganz einfach Predictive-Analytics-Lösungen erstellen, bereitstellen und freigeben können  |
| Sammeln und Analysieren von Spieldaten| Massive Parallelverarbeitung von Daten aus relationalen und nicht-relationalen Datenbanken | [Azure Data Warehouse](https://azure.microsoft.com/services/sql-data-warehouse/)| Flexibles Data-Warehouse als Dienst mit Enterprise-Funktionen   |
| Einbinden von Benutzern zur Steigerung der Nutzung und Aufbewahrung| Senden Sie gezielte Pushbenachrichtigungen an beliebige Plattformen von jedem Back-End aus, um Interessen zu generieren und bestimmte Spiele Aktionen zu fördern. | [Azure Notification Hubs](https://azure.microsoft.com/services/notification-hubs/)| Schnelles Broadcast Push zum Erreichen von Millionen mobiler Geräte auf allen wichtigen Plattformen unter &mdash; IOS, Android, Windows, Kindle, Baidu. Ihr Spiel kann in jeder Back &mdash; -End-Cloud oder lokal gehostet werden.|
| Streamen von Medieninhalten an lokale und weltweite Zielgruppen, während Sie Ihre Inhalte schützen| Spiele-und kinoclips für Broadcast Qualität können von allen Geräten aus überwacht werden.| [Azure Media Services](https://azure.microsoft.com/services/media-services/)| On-Demand-und Live-Video Streaming mit integrierten Content Delivery Network Funktionen. Verwenden Sie einen Player für alle Wiedergabe Anforderungen, einschließlich Inhalts Schutz und Verschlüsselung.| 
| Entwickeln, verteilen und Beta-Testen Ihrer mobilen apps | Testen und verteilen Sie Ihre Mobile App. App-Leistung und Verwaltung von Benutzer Erlebnisses. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Integration von Absturz Berichterstattung und benutzermetriken in eine APP-Verteilung und eine Benutzer Feedback Plattform. Unterstützt Android-, Cordova-, Ios-, OS X-, Unity-, Windows-und xamarin-apps. Beachten Sie auch [Visual Studio Mobile Center](https://visualstudio.microsoft.com/app-center/) &mdash; Mission Control für apps, die umfangreiche Analysen, Absturzberichte, Pushbenachrichtigungen, App-Verteilung und vieles mehr kombinieren. |
| Erstellen von Marketingkampagnen zur Steigerung und Erhaltung der Nutzungszahlen  | Senden von Push-Benachrichtigungen an Zielgruppen zur Generierung von Interesse und Förderung bestimmter Spielaktionen gemäß Datenanalysen | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) : wird vom März 2018 eingestellt und ist zurzeit nur für Bestandskunden verfügbar. |  Steigerung der Spielzeit und der Benutzerzahlen auf allen wichtigen Plattformen (iOS, Android, Windows, Windows Phone) |


##  <a name="startup-and-developer-resources"></a>Ressourcen für Startups und Entwickler

* [Microsoft für Startups](https://startups.microsoft.com)

    Microsoft für Startups bietet Produkt-, technische und Markt Einführungs Vorteile, um das Wachstum von Startups zu beschleunigen. Ein Vorteil besteht darin, ein kostenloses Azure-Konto zu erhalten. Sie haben $200 Gutschrift für die Untersuchung von Diensten für 30 Tage, 12 Monate beliebte kostenlose Dienste und sind immer 25 Dienste kostenlos. Weitere Informationen finden Sie unter [einbringen Ihrer Ideen zum Leben mit einem kostenlosen Azure-Konto](https://azure.microsoft.com/free/startups/).
    
* [Entwicklerprogramme](e2e.md#developer-programs)

    Microsoft bietet mehrere Entwickler Programme wie [ID@Xbox](https://www.xbox.com/Developers/id) und das [Xbox Live Creators-Programm](https://developer.microsoft.com/games/xbox/xboxlive/creator) an, um Sie beim entwickeln und Veröffentlichen von spielen zu unterstützen.

## <a name="learning-resources"></a>Lernressourcen

* Build 2016: [codelta ABS &mdash; verwendet Microsoft Azure App Service und Microsoft SQL Azure Backend zum Speichern der Spielergebnisse in Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* Build 2017: [Bereitstellung von erstklassigen Spiel Erlebnissen mithilfe von Microsoft Azure: Lektionen, die von Titeln wie Halo, Hitman und Walking Dead (Video) gelernt wurden](https://channel9.msdn.com/Events/Build/2017/P4062)
* Wiederverwendbare Bausteine, Projekte, Dienste und bewährte Methoden für die Unterstützung gängiger Gaming-Workloads mit Azure auf GitHub: [Bausteine für Spiele in Azure](https://github.com/MicrosoftDX/nether)
* [Gaming Services in Azure (Videos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Tools und andere hilfreiche Links

* [MSDN-Foren &mdash; Azure Platform](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Cloudbasiertes Auslastungs Test Tool](https://visualstudio.microsoft.com/team-services/cloud-load-testing/)
* [Sdert und Befehlszeilen Tools](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Software-as-a Service für Game-Back-Ends

[Playfab](https://playfab.com/) unterstützt derzeit mehr als 1.200 Live Spiele mit 80 Millionen monatlich aktiven Playern. Es handelt sich um eine vollständige Back-End-Plattform, die vollständige Stapel LiveOps mit Echtzeitsteuerung umfasst. 

Sie können diese Lösung mithilfe von SDK in Ihre mobilen, PC-oder Konsolenspiele integrieren. Es stehen SDKs für alle gängigen Spiel-Engines und-Plattformen zur Verfügung, einschließlich Android, Ios, Unreal, Unity und Windows. Informationen zu den ersten Schritten finden Sie in der [Dokumentation](https://api.playfab.com/).

Es bietet Spieldienste wie Authentifizierung, Player-Datenverwaltung, multiplayerfunktionen und Echtzeitanalysen, damit Ihr Spiel seine Benutzerbasis vergrößern kann. Nutzen Sie die Leistungsfähigkeit von Echtzeitdaten Pipeline und Live OPS, um Ihre Benutzer mit angepassten in-Game-Elementen,-Ereignissen und-Aktionen zu versorgen. Außerdem haben Sie die Möglichkeit, A/B-Tests durchzuführen, Berichte zu generieren, Pushbenachrichtigungen zu senden und vieles mehr. 

Wir sind ständig auf dem neuesten stellen und fügen neue Features hinzu. Weitere Informationen finden Sie unter [Features](https://playfab.com/features/) und für Preise. Weitere Informationen finden [Sie unter einfache Preise für die Skalierung mit Ihnen](https://playfab.com/pricing/).

## <a name="related-links"></a>Verwandte Links

* [Handbuch zur Entwicklung von Spielen unter Windows 10](https://docs.microsoft.com/windows/uwp/gaming/e2e)
* [Azure für Gaming](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft für Startups](https://startups.microsoft.com)
* [ID@Xbox](https://www.xbox.com/Developers/id)


 

 
