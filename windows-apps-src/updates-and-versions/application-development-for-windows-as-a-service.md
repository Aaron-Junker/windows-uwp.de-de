---
title: App-Entwicklung für Windows as a Service (WaaS)
description: Entkoppeln Sie die App-Freigabe und -Unterstützung von bestimmten Windows-Builds.
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, UWP
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: 478c4a16902329e1dd9267e0339dca0e0dcf3539
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321970"
---
# <a name="application-development-for-windows-as-a-service"></a>Anwendungsentwicklung für Windows as a Service

**Zielgruppe**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Core 

Heute basieren die Erwartungen von Benutzern häufig auf geräteorientierten Umgebungen, und vollständige Produktzyklen dürfen daher höchstens in Monaten und nicht in Jahren gemessen werden. Darüber hinaus müssen neue Versionen auf kontinuierlicher Basis verfügbar gemacht und mit minimaler Beeinträchtigung der Funktionsweise bereitgestellt werden. Microsoft hat Windows 10 so entwickelt, dass diese Anforderungen erfüllt werden, und dazu ein neues Konzept für die Entwicklung und Bereitstellung von Innovationen implementiert, das als [Windows als Dienst (WaaS)](https://docs.microsoft.com/windows/deployment/update/waas-overview) bezeichnet wird. Der Schlüssel zu deutlich kürzeren Produktzyklen bei gleichzeitiger Beibehaltung eines hohen Qualitätsniveaus ist ein innovatives, community-orientiertes Testkonzept, das Microsoft für Windows 10 implementiert hat. Die Community, die „Windows-Insider“, besteht aus Millionen von Benutzern auf der ganzen Welt. Wenn sich Windows-Insider an der Community beteiligen, testen sie im Laufe eines Produktzyklus viele Builds und geben Microsoft ihr Feedback im Rahmen eines iterativen Prozesses, der als Test-Flighting bezeichnet wird.

Als so genannte Test-Flights verteilte Builds bieten dem Technikteam von Windows wichtige Daten dazu, wie gut die Builds in der Praxis funktionieren. Windows-Insider-Programme für Windows-Insider ermöglichen Microsoft auch das Testen von Builds auf mehr Hardwareprodukten, in mehr Anwendungen und in mehr Netzwerkumgebungen, als dies früher möglich war, um potenzielle Probleme schneller zu erkennen. Microsoft ist daher überzeugt, dass das community-orientierte Test-Flighting sowohl die schnellere Bereitstellung von Innovationen, als auch eine höhere Qualität bei öffentlichen Freigaben als je zuvor ermöglichen wird.

## <a name="windows-10-release-types-and-cadences"></a>Versionstypen und -häufigkeiten von Windows 10

Obwohl Microsoft Test-Flight-Builds für Windows-Insider veröffentlicht, werden kontinuierlich zwei Arten von Windows 10-Versionen für die allgemeine Öffentlichkeit eingeführt:

Mit **Funktionsupdates** werden die neuesten Features, Umgebungen und Funktionen auf Geräten installiert, auf denen bereits Windows 10 ausgeführt wird. Da Funktionsupdates eine vollständige Version von Windows enthalten, installieren Benutzer damit auch Windows 10 auf Geräten, auf denen Windows 7 oder Windows 8.1 ausgeführt wird, und auf neuen Geräten, auf denen noch kein Betriebssystem installiert ist. Microsoft plant eine halbjährliche Veröffentlichung von Updates. 

**Qualitätsupdates** bieten Lösungen für Sicherheitsprobleme und andere wichtige Programmfehlerbehebungen. Qualitätsupdates werden mindestens einmal monatlich zur Optimierung der derzeit unterstützten Features bereitgestellt. Microsoft wird weiterhin Qualitätsupdates am „Update-Dienstag“ (oder „Patch-Dienstag“) veröffentlichen. Darüber hinaus kann es sein, dass Microsoft zusätzliche Qualitätsupdates für Windows 10 außerhalb dieses Rhythmus veröffentlicht, um damit auf die Anforderungen von Kunden zu reagieren.

Während der Entwicklung von Windows 10 hat Microsoft den Windows-Produktengineering- und -freigabezyklus optimiert, sodass die von Kunden angeregten Features, Umgebungen und Funktionen schneller als je zuvor bereitgestellt werden können. Außerdem bieten wir neue Möglichkeiten für die Bereitstellung und Installation von Funktions- und Qualitätsupdates, die die Implementierung und laufende Verwaltung vereinfachen, die Zahl der Mitarbeiter erhöhen, die auf die neuesten Windows-Funktionen und -Umgebungen zugreifen können, und die Gesamtbetriebskosten verringern. Daher haben wir neue Wartungsoptionen (halbjährlicher Kanal und Long-Term Servicing Channel (LTSC)) implementiert, die praktische Lösungen bieten, um in Unternehmensumgebungen mehr Geräte stets aktuell zu halten, als dies bisher möglich war.

In der folgenden Tabelle sind die verschiedenen Wartungskanäle und ihre wichtigsten Attribute beschrieben.

| Wartungsoption | Verfügbarkeit von neuen Featureupgrades zur Installation | Wartungszeitraum | Wichtige Vorteile | Unterstützte Editionen |
| --- | --- | --- | --- | --- |
| Semi-Annual Channel (gezielt) | Unmittelbar nach der ersten Veröffentlichung durch Microsoft | 18 Monate | Stellt Benutzern neue Funktionen so schnell wie möglich zur Verfügung | Home, Pro, Education, Enterprise, Mobile, IoT Core, Windows 10 IoT Core Pro (IoT Core Pro) |
| Semi-Annual Channel | Ungefähr vier Monate nach der ersten Veröffentlichung durch Microsoft | 18 Monate ab der ersten Veröffentlichung | Ermöglicht zusätzliche Zeit zum Testen der neuen Featureupgrades vor der Bereitstellung | Pro, Education, Enterprise, Mobile Enterprise, IoT Core Pro |
| Long-Term Servicing Channel (LTSC) | Unmittelbar nach der Veröffentlichung durch Microsoft | 10 Jahre | Ermöglicht die langfristige Bereitstellung ausgewählter Windows 10-Versionen in Konfigurationen mit geringen Änderungen | Enterprise LTSB |

Weitere Informationen finden Sie unter [Windows 10-Wartungsoptionen für Updates und Upgrades](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Unterstützen von Apps in Windows als Dienst

Ursprünglich bestand die Unterstützung von Apps darin, mit jeder neuen Windows-Version eine neue App-Version zu veröffentlichen. Dabei wurde davon ausgegangen, dass am zugrunde liegenden Betriebssystem bedeutende Änderungen vorgenommen wurden, die in der Anwendung zu einem Rückschritt führen konnten. Dieses Modell umfasste einen dedizierten Entwicklungs- und Validierungszyklus, der von unseren ISV-Partnern eine Anpassung an den Freigaberhythmus der Windows-Versionen verlangte.

Im WaaS-Modell (Windows als Dienst) verpflichtet sich Microsoft, die Kompatibilität des zugrunde liegenden Betriebssystems zu gewährleisten. Das bedeutet, dass Microsoft alle Anstrengungen unternehmen wird, damit dass App-Ökosystem nicht durch bedeutende Änderungen negativ beeinflusst wird. In diesem Szenario bleiben die meisten Apps (die nicht vom Kernel abhängig sind) bei Freigabe eines Windows-Builds funktionsfähig.

Aufgrund dieser Änderung empfiehlt Microsoft ISV-Partnern, die App-Freigabe und -Unterstützung von bestimmten Windows-Builds zu entkoppeln. Unsere gemeinsamen Kunden profitieren mehr von einem Modell, das auf dem Anwendungslebenszyklus basiert. Dies bedeutet Folgendes: Wenn eine Anwendungsversion veröffentlicht wird, wird sie für einen bestimmten Zeitraum unterstützt, und zwar unabhängig davon, wie viele Windows-Builds in der Zwischenzeit veröffentlicht wurden. Der ISV verpflichtet sich, die spezifische App-Version so lange zu unterstützen, wie sie im Lebenszyklus unterstützt wird. Microsoft verfolgt bei Windows einen ähnlichen Lebenszyklus-basierten Ansatz, der [hier](https://go.microsoft.com/fwlink/?LinkID=780549) beschrieben wird.

Dadurch entfällt auch die Notwendigkeit, den App-Freigabezeitplan auf den der veröffentlichten Windows-Versionen abzustimmen. ISV-Partner sollten die Möglichkeit haben, Features oder Updates in ihrer eigenen Geschwindigkeit freizugeben. Wir sind überzeugt, dass unsere Partner ihre Kunden unabhängig von den Windows-Versionen mit neuesten App-Updates auf dem Laufenden halten können. Außerdem ist es nicht erforderlich, dass unsere Kunden bei jeder Freigabe eines Windows-Builds eine explizite Supportzusage einholen. Im folgenden Beispiel für eine Supportzusage wird beschrieben, wie eine App unter verschiedenen Betriebssystemversionen unterstützt werden kann:

| Beispiel für eine Supportzusage zum Anwendungslebenszyklus | |
| --- | --- |
| Das Unternehmen Contoso ist Softwareentwickler und Herausgeber der beliebten Mojave-App, die im Bereich Unternehmens-Apps einen beachtlichen Marktanteil hat. Contoso veröffentlicht mit Mojave 14.0 die nächste Hauptversion und erklärt, dass der grundlegende Produktsupport für einen Zeitraum von drei Jahren ab dem Tag der Markteinführung gilt. Während des grundlegenden Supports sind alle Updates und Supportleistungen für das lizenzierte Produkt kostenlos erhältlich. Contoso bietet außerdem einen zweijährigen erweiterten Support. Während dieser Nachfrist können Kunden Updates und Supportleistungen gegen eine Gebühr in Anspruch nehmen. Mit dem Enddatum des erweiterten Supports wird diese Produktversion nicht mehr unterstützt. Während des grundlegenden Supports unterstützt Contoso Mojave 14.0 in allen veröffentlichten Windows-Builds. Darüber hinaus veröffentlicht Contoso nach Bedarf und unabhängig von Windows-Produktversionen Updates für Mojave. | |

In den folgenden Abschnitten finden Sie weitere Informationen dazu, wie Microsoft die Kompatibilität des zugrunde liegenden Betriebssystems sicherstellt. Außerdem wird ausführlich erläutert, wie Sie die Kompatibilität des kombinierten Ökosystems aus Betriebssystem und App gewährleisten können. In einem Abschnitt wird beschrieben, wie Sie mit Test-Flighting-Builds von Windows beeinträchtigte App-Funktionalität erkennen, bevor ein Windows-Build freigegeben wird. Zuletzt erfahren Sie, welche Instrumente und telemetrischen Daten uns helfen, die Qualität von Windows-Builds zu verbessern. ISVs wird empfohlen, einen vergleichbaren Ansatz bei ihrem App-Portfolio zu verfolgen.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Wichtige Änderungen seit Windows 7 zur Gewährleistung der App-Kompatibilität

Kompatibilität hat für Entwickler einen hohen Stellenwert. ISVs und Entwickler möchten sicherstellen, dass ihre Apps unter allen unterstützten Versionen des Windows-Betriebssystems erwartungsgemäß ausgeführt werden. Verbraucher und Unternehmen haben ebenfalls ein Interesse daran, dass die von ihnen erworbenen Apps weiterhin funktionieren. Wir wissen, dass Kompatibilität das Hauptkriterium für die Kaufentscheidung ist. Zuverlässige Apps, die unter Berücksichtigung bewährter Methoden programmiert wurden, verursachen bei der Veröffentlichung einer neuen Windows-Version wesentlich weniger Codeänderungen und verringern die Fragmentierung. Solche Apps reduzieren nicht nur den technischen Wartungsaufwand, sondern sind auch schneller marktreif.

Zu Zeiten von Windows 7 ging es vielmehr darum, auf Kompatibilitätsprobleme zu reagieren. Bei Windows 8 haben wir umgedacht und direkt innerhalb des Windows-Betriebssystems und nicht erst im Nachhinein für Kompatibilität gesorgt. Windows 10 ist entwurfsbedingt die bisher kompatibelste Betriebssystemversion. Hier einige wichtige Aspekte, die uns bei der Umsetzung geholfen haben:
-   **App-Telemetrie:** Telemetriedaten informieren uns über die Bedeutung der App im Windows-Ökosystem und helfen bei Kompatibilitätstests.
-   **ISV-Partnerschaften:** Externe Partner erhalten durch die direkte Zusammenarbeit Informationen, die zur Behebung der von Endbenutzern gemeldeten Probleme beitragen.
-   **Entwurfsprüfung, vorgeschaltete Erkennungsmechanismen:** Die Zusammenarbeit mit Featureteams reduziert die Anzahl bedeutender Änderungen in Windows. Kompatibilitätsprüfungen sind für jedes Featureteam ein Muss.
-   **Kommunikation:** Engmaschigere Kontrolle von API-Änderungen und verbesserte Kommunikation
-   **Test-Flighting und Feedbackschleife:** Die von Windows-Insidern getesteten Flight-Builds verbessern unsere Chancen, Kompatibilitätsprobleme bereits vor Freigabe des endgültigen Builds für die Öffentlichkeit auszumachen. Unser Feedbackverfahren sorgt nicht nur für die Fehlererkennung, sondern auch dafür, dass Benutzer die Features erhalten, die sie brauchen.

## <a name="best-practices-for-app-compatibility"></a>Bewährte Methoden für die App-Kompatibilität

Microsoft verwendet Diagnose- und Nutzungsdaten, mit deren Hilfe Probleme identifiziert und behoben, Produkte und Dienste verbessert und Benutzern personalisierte Funktionen zur Verfügung gestellt werden können. Die gesammelten Nutzungsdaten erstrecken sich auch auf Apps, die auf den PCs im Windows-Ökosystem ausgeführt werden. Wir ermitteln die von unseren Kunden genutzten Features und testen neue Versionen des Windows-Betriebssystems unter Verwendung der entsprechenden Apps, Geräte und Treiber. Dank der über 90-prozentigen Kompatibilität mit Tausenden beliebter Apps ist Windows 10 die kompatibelste Windows-Version aller Zeiten. Falls Probleme gefunden werden, wendet sich das Windows-Kompatibilitätsteam in der Regel mit Feedback an unsere ISV-Partner, um gemeinsam Lösungen zu erarbeiten. Wir möchten unseren gemeinsamen Kunden eine reibungslose Updateerfahrung bieten und dafür sorgen, dass die gesamte Funktionalität sowohl des Windows-Betriebssystems als auch ihrer Produktivitäts- oder Unterhaltungs-Apps erhalten bleibt.

Die folgenden Abschnitte enthalten von Microsoft empfohlene bewährte Methoden, mit denen Sie sicherstellen können, dass Ihre Apps mit Windows 10 kompatibel sind.

### <a name="windows-version-check"></a>Prüfung der Windows-Version

Die Betriebssystemversion wurde mit Windows 10 erhöht. Das bedeutet, dass die interne Versionsnummer in 10.0 geändert wurde. Die Anwendungs- und Gerätekompatibilität nach einer Änderung der Betriebssystemversion aufrechtzuerhalten, war uns schon immer sehr wichtig. Für die meisten App-Kategorien (ohne Kernelabhängigkeiten) hat die Änderung keine negativen Auswirkungen auf die App-Funktionen. Bereits vorhandene Apps können unter Windows 10 weiterhin problemlos ausgeführt werden.

Inwieweit sich diese Änderung auswirkt, richtet sich nach der jeweiligen App. Dies bedeutet, dass alle Apps, die die Betriebssystemversion überprüfen, eine höhere Versionsnummer erhalten, was sich wie folgt äußern kann:
-   Apps können von App-Installern u. U. nicht installiert werden und nicht starten.
-   Apps können instabil werden oder abstürzen.
-   Apps können Fehlermeldungen ausgeben, aber weiterhin ordnungsgemäß funktionieren.

Einige Apps überprüfen die Version und geben einfach eine Warnung an den Benutzer aus. Es gibt jedoch auch Apps, für die eine Versionsprüfung unumgänglich ist (in den Treibern oder im Kernelmodus, um eine Erkennung zu vermeiden). In diesen Fällen verursacht die App beim Auffinden einer falschen Version einen Fehler. Wir empfehlen statt einer Versionsprüfung eines der folgenden Verfahren:
-   Wenn die App von bestimmten API-Funktionen abhängig ist, stellen Sie sicher, dass sie auf die richtige API-Version ausgerichtet ist.
-   Die Änderung muss über APISet oder eine andere öffentliche API erkannt werden. Die Version darf nicht stellvertretend für ein Feature oder einen Fix verwendet werden. Wenn für bedeutende Änderungen keine ordnungsgemäße Prüfung verfügbar gemacht wird, liegt ein Fehler vor.
-   Achten Sie darauf, dass die App KEINE ungewöhnliche Versionsprüfung vornimmt, z. B. über die Registrierung, Dateiversionen, Offsets, den Kernelmodus, Treiber oder auf andere Weise. Wenn eine Versionsprüfung für die App unverzichtbar ist, verwenden Sie die GetVersion-APIs, die die Hauptversion, Nebenversion und Buildnummer zurückgeben sollten.
-   Beachten Sie bei Verwendung der [GetVersion](https://go.microsoft.com/fwlink/?LinkID=780555)-API, dass sich ihr Verhalten seit Windows 8.1 geändert hat.

Als Entwickler von Antischadsoftware- oder Firewall-Apps sollten Sie sich über Ihre üblichen Feedbackkanäle und über das Windows-Insider-Programm informieren.

### <a name="undocumented-apis"></a>Nicht dokumentierte APIs

Ihre Apps sollten keine nicht dokumentierten Windows-APIs aufrufen oder von bestimmten Windows-Dateiexporten oder Registrierungsschlüsseln abhängig sein. Dies kann fehlerhafte Funktionen, Datenverlust und potenzielle Sicherheitsprobleme zur Folge haben. Wenn Sie für Ihre App Funktionen benötigen, die nicht verfügbar sind, nutzen Sie Ihre üblichen Feedbackkanäle und das Windows-Insider-Programm, um Feedback zu übermitteln.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Entwickeln von UWP (Universelle Windows-Plattform)- und Centennial-Apps

Wir empfehlen allen ISVs von Win32-Apps, zukünftig [UWP (Universelle Windows-Plattform)-Apps](https://go.microsoft.com/fwlink/?LinkID=780560) und insbesondere [Centennial](https://go.microsoft.com/fwlink/?LinkID=780562)-Apps zu entwickeln. Die Entwicklung dieser App-Pakete birgt deutliche Vorteile gegenüber der Verwendung herkömmlicher Win32-Installationsprogramme. Da UWP-Apps außerdem im [Microsoft Store](https://go.microsoft.com/fwlink/?LinkID=780563) unterstützt werden, ist es für dich einfacher, deinen Benutzern ein automatisches Update auf eine konsistente Version zu bieten und deine Supportkosten zu senken.

Wenn das Centennial-Modell von Ihren Win32-App-Typen nicht unterstützt wird, sollten Sie unbedingt das richtige Installationsprogramm verwenden und sicherstellen, dass es eingehend getestet wurde. Das Installationsprogramm ist der erste Berührungspunkt Ihrer Benutzer oder Kunden mit der App und sollte einwandfrei funktionieren. Dies ist häufig nicht der Fall, u. a. auch, weil das Programm nicht für alle Szenarien getestet wurde. Mit dem [Zertifizierungskit für Windows-Apps](https://go.microsoft.com/fwlink/?LinkID=780565) können Sie die Installation und Deinstallation Ihrer Win32-App testen, ermitteln, ob nicht dokumentierte APIs verwendet werden, und andere allgemeine Leistungsprobleme aufdecken, die nicht den bewährten Methoden entsprechen, bevor dies Ihre Benutzer tun.

**Bewährte Methoden:**
-   Verwenden Sie Installationsprogramme, die die 32-Bit- und 64-Bit-Version von Windows unterstützen.
-   Achten Sie beim Entwickeln Ihrer Installationsprogramme darauf, dass sie mehrere Szenarien (sowohl benutzer- als auch computerspezifisch) abdecken.
-   Alle weitervertreibbaren Windows-Komponenten sollten im Originalpaket verbleiben. Durch ein Umpacken kann das Installationsprogramm beschädigt werden.
-   Planen Sie Entwicklungszeit für Ihre Installationsprogramme ein. Häufig wird im Lebenszyklus der Softwareentwicklung übersehen, dass sie zum Lieferumfang dazugehören.

## <a name="optimized-test-strategies-and-flighting"></a>Optimierte Teststrategien und Test-Flighting

Test-Flights des Windows-Betriebssystems sind Zwischenbuilds, die Windows-Insidern vor der allgemeinen Veröffentlichung eines endgültigen Builds zur Verfügung gestellt werden. Je mehr Insider diese Zwischenbuilds testen, umso mehr Feedback erhalten wir zur Buildqualität, Kompatibilität usw. und umso besser fällt die Qualität der endgültigen Builds aus. Durch Ihre Teilnahme am Test-Flighting-Programm können Sie sicherstellen, dass Ihre Apps unter iterativen Betriebssystembuilds erwartungsgemäß funktionieren. Wir würden uns freuen, wenn Sie uns Ihre Meinung zur Funktionsweise der Test-Flight-Builds, zu Problemen und anderen Aspekten mitteilen würden.

Wenn Sie Ihre App im Store anbieten, können Sie sie über den Store für Test-Flights bereitstellen, wodurch sie von Windows-Insidern installiert werden kann. Benutzer können Ihre App installieren und vorläufiges Feedback dazu abgeben, bevor Sie sie für die Allgemeinheit freigeben. In den folgenden Abschnitten erfahren Sie, wie Sie ihre Apps unter Verwendung von Windows-Test-Flight-Builds testen.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Schritt 1: Windows-Insider werden und am Test-Flighting teilnehmen
Als [Windows-Insider](https://go.microsoft.com/fwlink/p/?LinkId=521639) können Sie die Zukunft von Windows mitgestalten – Ihr Feedback hilft uns, Plattformfeatures und -funktionen zu optimieren. Als Mitglied dieser lebendigen Community können Sie sich mit anderen Interessierten vernetzen, Foren beitreten, Tipps und Tricks austauschen und alles über anstehende Veranstaltungen nur für Insider erfahren.

Du hast Zugriff auf Vorabversionen von Windows 10, Windows 10 Mobile, das aktuelle Windows SDK und den Emulator und verfügst über alle Tools, die du zum Entwickeln großartiger Apps benötigst. Außerdem erhältst du Neuigkeiten über die universelle Windows-Plattform und den Microsoft Store.

Darüber hinaus können Sie mit den Vorabversionen der Hardwareentwicklungskits überragende Hardwarelösungen erstellen und universelle Treiber für Windows entwickeln. IoT Core Insider Preview ist auch auf unterstützten IoT-Entwicklungsboards verfügbar, sodass Sie faszinierende vernetzte Lösungen auf Basis der universellen Windows-Plattform erstellen können.

Beachten Sie vor Ihrer Teilnahme Folgendes: Das Windows-Insider-Programm richtet sich an alle, die
-   Software testen möchten, die sich noch in der Entwicklung befindet.
-   Feedback zur Software und Plattform geben möchten.
-   keine Probleme mit häufigen Updates oder einem UI-Design haben, das sich im Laufe der Zeit signifikant verändern kann.
-   sich auf ihrem PC auskennen und problemlos Fehler beheben, Daten sichern, Festplatten formatieren, ein Betriebssystem von Grund auf neu installieren oder ein altes Betriebssystem ggf. wiederherstellen können.
-   wissen, was eine ISO-Datei ist und wie sie verwendet wird.
-   die Datei nicht auf Ihrem Hauptcomputer oder -gerät installieren.

### <a name="step-2-test-your-scenarios"></a>Schritt 2: Szenarien testen

Nach dem Update auf einen Test-Flight-Build sollen Ihnen die folgenden Beispieltestfälle den Einstieg erleichtern und aufzeigen, wie Sie Feedback sammeln. Achten Sie bei den meisten Tests darauf, x86- und AMD64-Systeme abzudecken.
**Saubere Installation testen:** Stellen Sie bei einer sauberen Installation von Windows 10 sicher, dass Ihre App voll funktionsfähig ist. Wenn Ihre App diesen Test und den Upgradetest nicht besteht, wird das Problem wahrscheinlich durch Änderungen am zugrunde liegenden Betriebssystem oder Fehler in der App verursacht. Falls sich die oben genannten Ursachen nach einer Prüfung bestätigen, sollten Sie das Windows-Insider-Programm nutzen, um Feedback abzugeben und gemeinsam mit anderen Insidern eine Lösung zu suchen.

**Upgrade testen:** Überprüfen Sie nach dem Upgrade von einer kompatiblen Windows-Version (z. B. Windows 7 oder Windows 8.1) auf Windows 10, ob Ihre App funktioniert. Die App sollte während des Upgrades keine Rollbacks verursachen und danach weiter ordnungsgemäß funktionieren – eine wichtige Voraussetzung für eine reibungslose Upgradeerfahrung.

**Neuinstallation testen:** Stellen Sie sicher, dass die Funktionen der App wiederhergestellt werden können, indem Sie sie nach dem Upgrade des PCs von einem kompatiblen Betriebssystem auf Windows 10 neu installieren. Wenn die App den Upgradetest nicht bestanden hat und Sie die Problemursache nicht eingrenzen konnten, lassen sich die ausgefallenen Funktionen u. U. durch eine Neuinstallation wiederherstellen. Falls beim Neuinstallationstest keine Probleme auftreten, ist dies ein Hinweis darauf, dass Teile der App möglicherweise nicht zu Windows 10 migriert wurden.

**Betriebssystem-/Gerätefeatures testen:** Vergewissern Sie sich, dass Ihre App erwartungsgemäß funktioniert, wenn sie von bestimmten Betriebssystemfunktionen abhängig ist. Im Folgenden einige allgemeine Testbereiche. Die Verwendung verschiedener gängiger PC-Modelle gewährleistet eine breite Abdeckung:
-   Audio
-   USB-Gerätefunktionen (Tastatur, Maus, Speicherstick, externe Festplatte usw.)
-   Bluetooth
-   Grafik/Anzeige (mehrere Monitore, Projektion, automatische Ausrichtung usw.)
-   Touchscreen (Ausrichtung, Bildschirmtastatur, Stift, Gesten usw.)
-   Touchpad (linke/rechte Taste, Tippen, Scrollen usw.)
-   Stift (Einfach-/Doppeltippen, Drücken, Halten, Radierer usw.)
-   Drucken/Scannen
-   Sensoren (Beschleunigungsmesser, Fusion usw.)
-   Kamera

### <a name="step-3-provide-feedback"></a>Schritt 3: Feedback bereitstellen

Lassen Sie uns wissen, wie Ihre App mit Test-Flight-Builds funktioniert. Wenn die App während der Tests Fehler verursacht, sollten Sie diese über das Partnerportal melden (sofern Sie Zugriff haben) oder Kontakt zu Microsoft aufnehmen. Ihr Feedback ist uns wichtig, da es hilft, gemeinsam mit unseren Partnern eine optimale Benutzererfahrung zu schaffen.

### <a name="step-4-register-on-ready-for-windows"></a>Schritt 4: Bei Ready For Windows registrieren
Auf der Website [Ready for Windows](https://go.microsoft.com/fwlink/?LinkID=780580) finden Sie ein Verzeichnis der Softwarelösungen, die Windows 10 unterstützen. Sie richtet sich an IT-Administratoren in Unternehmen und Organisationen weltweit, die die Bereitstellung von Windows 10 auf ihren Systemen in Betracht ziehen. IT-Administratoren können anhand der Website feststellen, ob die in ihrem Unternehmen bereitgestellte Software in Windows 10 unterstützt wird.

## <a name="related-topics"></a>Verwandte Themen
[Windows 10-Wartungsoptionen für Updates und Upgrades](https://docs.microsoft.com/windows/manage/introduction-to-windows-10-servicing)
