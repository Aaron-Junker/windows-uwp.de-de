---
title: Rewards für Erfolge
description: Beschreibt, wie Sie eine Auszeichnung zum Übermitteln von Boni konfigurieren können.
ms.assetid: b6fc5bdb-ba7b-4687-985e-894182f066da
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, Xbox, Spiele, Uwp, Windows 10, Xbox eine, Leistung, Boni
ms.localizationpriority: medium
ms.openlocfilehash: 0fbbcb06a2aba927301cf982d09fdb55192ec84c
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617695"
---
# <a name="achievement-rewards"></a>Rewards für Erfolge

Das folgende Diagramm veranschaulicht, wie ein Entwickler den Lebenszyklus eines Titels verwalten kann. Das neue Auszeichnung-System wurde entworfen, unsere vertraut Mechanikers mit viel mehr Flexibilität zu erhöhen, in wie Erfolge Sperre aufgehoben werden, in wie und wann sie hinzugefügt werden und welche Vorteile sie stellen Benutzern –, die den Entwickler, die den Titel als einen Dienst ausführen können Wert hinzufügen und Verwalten von Engagement der Benutzer im Laufe der Zeit.

##### <a name="figure-1---how-a-title-might-drive-user-behavior"></a>Abbildung 1.   Wie ein Titel Benutzerverhalten steuern kann. #####
![rewarding_achievements](../images/omega/achievements_overview_01_drive_behavior.png)

### <a name="flexible-options-for-rewarding-achievement"></a>Flexible Optionen für die Auszeichnung belohnen ###
Mit der Xbox One haben wir die Auszeichnung-System zur Unterstützung von flexibler Reward Optionen erweitert. Gamerscore bleibt eine wertvolle Reward, die einen einzelnen, gemeinsamen Gaming-Faktor für den Benutzer über die Xbox Live-Ökosystems, aber jetzt nachverfolgt, den Entwickler oder den Herausgeber, Erfolge als ein Mechanismus für eine viel größere Anzahl von Boni, beide innerhalb Ihrer Titel verwenden können und außerhalb der Titel des.

Eine Auszeichnung kann mit mehreren Boni, bis zu einer Belohnung für jeden Typ Reward konfiguriert werden. Eine Auszeichnung kann auch mit keine explizite Reward konfiguriert werden; In diesem Fall fungiert erreichen der Zertifikation Symbol als visual Signal für den Player, der die Auszeichnung erworben.

Xbox Live, unterstützt die folgenden Arten von Boni:

* Gamerscore
* ClipArt
* Vorteile von in-app

#### <a name="gamerscore"></a>Gamerscore ####
Wir sind bestrebt, die Integrität des Werts Gamerscore, die sich unserer Xbox Live-Benutzer erstellt wurde. Es gibt nur eine Gamerscore pro Benutzer! Alle Gamerscore, die ein Benutzer auf vorhandenen Xbox Live-Plattformen wie z. B. Xbox 360, Xbox One oder Windows 10 verdient wird auf einem einzelnen Gamerscore für diesen Benutzer angerechnet.

Wenn ein Benutzer eine Auszeichnung Gamerscore aufhebt, wird durch die Xbox Live automatisch des Benutzers Gamerscore die konfigurierte Menge erhöht.

Es gibt Einschränkungen, die auf denen Titel Gamerscore als eine Belohnung in ihren Ergebnissen angeboten werden. Finden Sie die Richtliniendokumente https://developer.xboxlive.com/ die neuesten Informationen.

#### <a name="art"></a>ClipArt ####
Haben Sie einige interessante Konzept Kunst, die Ihren Entwicklern in einem frühen Zeitpunkt im Titel des Einführung Phase gezeichnet haben? Haben Sie ansprechende, hochauflösende Bilder, die die Hub-Anwendung ergänzen können, wenn Spieler finden Sie auf? Vielleicht unterstützt Ihre app mehrere Designs? Mit der Art nutzen können Sie ansprechende, reicher Erfahrungen in Ihre Titel und darüber hinaus Herunterfahren, die Ihre Spieler verdienen müssen.

Mit hoher Auflösung Konzept Kunst, frühen Entwurf-Zeichnungen, speziell erstellt Grafikobjekte und andere digitale Grafikobjekte können als eine Belohnung Benutzern angeboten, für das Entsperren eine Auszeichnung. Diese Ressourcen werden möglicherweise im Xbox-ein-Dashboard angezeigt und Sie im begleitenden Benutzeroberflächen angezeigt werden kann, um einfach durch Abfragen des Diensts Erfolge, um die relevanten Metadaten abzurufen.

#### <a name="in-app-rewards"></a>Vorteile von in-app ####
Wir werden die Boni für in-app eingeführt, um Entwicklern zu ermöglichen, mehr Flexibilität und Kontrolle über die Vorteile, die eine Auszeichnung bietet. Boni für in-app können Sie Erfolge zu verwenden, um benutzerdefinierte in-Game-Rewards direkt für Ihre Benutzer zu übermitteln, ohne unbedingt aktualisieren den Titel. Den Auszeichnung nutzen Sie ganz einfach mit einem Code, eine ID oder ein Ausdruck, die der Titel erkennt konfigurieren, und wenn der Benutzer das Erreichen der Zertifikation entsperrt, Xbox LIVE sendet diesen Code, Titel, und informiert den Titel des der Nutzen für den Benutzer übermitteln.

Der nutzen selbst liegt bei Ihnen als Entwickler. Reward Ideen gehören:

* Zusätzliche in-Game-Währung oder Punkte
* Zugriff auf ein Sonderzeichen, sorgfältig oder Zuordnung
* Eine temporäre Oberfläche Multiplikator.

### <a name="configuring-in-app-rewards"></a>Konfigurieren von in-app-Boni ###
Konfigurieren eine Belohnung in-app für einen Erfolg ist recht einfach. Der Besitzer der Auszeichnung muss einen Namen für die Belohnung, eine Beschreibung der Reward und ein Symbol Reward zusätzlich zu den Werten Reward angeben. Dieser Wert Reward richtet sich nach der Entwickler und vornehmen, die entweder das Spiel zu interpretieren und ordnungsgemäß verarbeiten kann oder, die der Benutzer kann als Rahmen der Title-spezifische Reward Einlösung eingeben, muss.

Ein Beispiel für ein Reward-Wert, den das Spiel interpretieren kann möglicherweise eine 5-Ziffer oder eine spezielle Zeichenfolge dem das Spiel oder des Spiels-Dienst kennt Maps auf einem bestimmten in-Game-Element. Entwickler sollten den Titel verwalteten Speicher (TMS)-Dienst, um erleichtern Ihnen neue Reward-Werte im Laufe der Zeit hinzuzufügen, die das Spiel lesen verstehen, nutzen.

Ein Beispiel für ein Reward-Wert, den der Benutzer übermitteln, muss möglicherweise einen speziellen Code oder eine Zeichenfolge, die der Benutzer in einer Einlösung funktioniert in den Titel in einer Begleit-app oder des Entwicklers-Website ein.

### <a name="redeeming-in-app-rewards"></a>In-app-Vorteile einlösen ###
Eine Belohnung in-app wird wirksam, wenn der Benutzer die Belohnung innerhalb des Spiels einlösen. Titel müssen bewusst sein, dass ein Benutzer einen Erfolg entsperrt hat, der mit einer Belohnung für in-app konfiguriert ist, damit der Titel der Reward ordnungsgemäß für den Benutzer bereitstellen kann. Zu diesem Zweck sollten Titel die folgenden Schritte ausführen:

1. Abfragen des Diensts Erfolge bei Titel starten oder Fortsetzen der Titel der Unterbrechung anzeigen, welche Erfolge entsperrt Boni für in-app haben und den Reward-Code für die einzelnen zu erhalten. Dies sollte immer um sicherzustellen, dass Sie alle Erfolge abfangen, die entsperrt wurden möglicherweise während der Ausführung des Titels wurde nicht oder auf einer anderen Konsole erfolgen.  

    Um abzufragen, können Sie die RESTful Erfolge-URIs oder die APIs in der Microsoft.Xbox.Services.Achievements-Namespace.

2. Registrieren Sie sich eine Benachrichtigung zu erhalten, wenn eine Ihren Ergebnissen entsperrt ist. Dies ist optional, jedoch wahrscheinlich die meisten Titeln wünschenswert. Beachten Sie, dass Titel nur diese Benachrichtigung erhalten sollen, wenn der Titel tatsächlich ausgeführt wird, wenn die Sperre eintritt. Dies ist ein weiterer Grund, warum der vorherige Schritt wichtig ist.

   Verwenden Sie zum Registrieren für eine Auszeichnung Benachrichtigung AchievementNotifier.GetTitleIdFilteredSource-Methode. Dieses Thema enthält Beispielcode, der veranschaulicht, wie Sie registrieren.
